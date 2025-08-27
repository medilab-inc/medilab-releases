# GitHub → Notion 自動同期 セットアップガイド

このガイドでは、GitHubリポジトリのMarkdownファイルを自動的にNotionに同期し、社内メンバーがGitHubアカウントなしでドキュメントを閲覧できる環境を構築します。

## 📋 前提条件

- Notionのワークスペース管理者権限
- GitHubリポジトリの設定権限（Secrets設定のため）
- Slackワークスペースの管理者権限またはアプリ追加権限（オプション）

## 🚀 セットアップ手順

### ステップ1: Notion Integration の作成

1. [Notion Developers](https://www.notion.so/my-integrations) にアクセス
2. 「New Integration」をクリック
3. 以下の設定で作成：
   - **Name**: `MediLab Release Notes Sync`
   - **Associated workspace**: 対象のワークスペースを選択
   - **Capabilities**: 
     - ✅ Read content
     - ✅ Update content
     - ✅ Insert content

4. 作成後、**Internal Integration Token**をコピー（後で使用）

### ステップ2: Notionページの準備

1. Notionで同期先となる親ページを作成
   - 例: `製品リリースノート` というページを作成

2. 作成したページで右上の「...」メニューから「Connections」を選択

3. 先ほど作成した Integration を追加
   - `MediLab Release Notes Sync` を検索して追加

4. ページのURLから**Page ID**を取得：
   ```
   https://www.notion.so/workspace/ページ名-{PAGE_ID}
   ```
   ※ハイフン以降の32文字の英数字がPage ID

### ステップ3: Slack通知の設定（オプション）

同期の成功/失敗をSlackに通知する場合は、以下の設定を行います。

#### Slack Appの作成

1. [Slack API](https://api.slack.com/apps) にアクセス
2. 「Create New App」→「From scratch」を選択
3. アプリ情報を入力：
   - **App Name**: `MediLab GitHub Notifier`
   - **Pick a workspace**: 対象のワークスペースを選択

#### Incoming Webhooksの有効化

1. 左メニューの「Incoming Webhooks」を選択
2. トグルスイッチを**ON**に設定
3. 「Add New Webhook to Workspace」をクリック
4. 通知先チャンネルを選択して「Allow」
   - 例: `#release-notes`

#### Webhook URLの取得

作成されたWebhook URLをコピー：
```
https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXX
```

### ステップ4: GitHub Secrets の設定

GitHubリポジトリで以下のSecretsを設定：

1. リポジトリの「Settings」→「Secrets and variables」→「Actions」へ移動

2. 「New repository secret」で以下を追加：

   | Secret Name | Value | 説明 |
   |-------------|-------|------|
   | `NOTION_TOKEN` | `secret_xxx...` | Notion Integration Token |
   | `NOTION_ROOT_PAGE_ID` | `32文字の英数字` | 同期先のNotion Page ID |
   | `SLACK_WEBHOOK` | `https://hooks.slack.com/...` | (オプション) Slack通知用 |

### ステップ5: 動作確認

1. `products/`配下のMarkdownファイルを編集してコミット
2. GitHub Actionsが自動実行されることを確認
3. Notionページに内容が同期されることを確認
4. （Slack設定済みの場合）Slackに通知が届くことを確認

## 📁 同期される内容

以下のディレクトリ内のMarkdownファイルが自動同期されます：

```
medilab-releases/
├── products/     # → Notion「製品リリースノート」フォルダ
│   ├── kakeru/   # → Kakeru製品のページ階層
│   └── mamoru/   # → Mamoru製品のページ階層
├── docs/         # → Notion「ドキュメント」フォルダ
└── templates/    # → テンプレートも同期
```

## 🔄 同期のタイミング

- `main`ブランチへのpush時に自動同期
- 手動実行も可能（Actions → sync-to-notion → Run workflow）

## ⚙️ カスタマイズ

### 同期対象を変更する場合

`.github/workflows/sync-to-notion.yml` の `paths` セクションを編集：

```yaml
paths:
  - 'products/**/*.md'
  - 'docs/**/*.md'
  - 'templates/**/*.md'
  # 追加したいパスを記載
```

### 特定のファイルを除外する場合

notion-syncの設定で `--exclude` オプションを使用：

```yaml
notion-sync sync \
  --token "$NOTION_TOKEN" \
  --root-page "$NOTION_ROOT_PAGE_ID" \
  --source-dir ./products \
  --recursive \
  --exclude "**/draft-*.md" \
  --create-root-folder "製品リリースノート"
```

### Slack通知メッセージのカスタマイズ

`.github/workflows/sync-to-notion.yml`でメッセージをカスタマイズ：

```yaml
- name: Notify Slack on Success
  if: success()
  uses: rtCamp/action-slack-notify@v2
  env:
    SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
    SLACK_CHANNEL: '#release-notes'
    SLACK_USERNAME: 'GitHub Actions'
    SLACK_ICON: ':github:'
    SLACK_COLOR: 'good'
    SLACK_TITLE: 'Notion同期完了'
    SLACK_MESSAGE: |
      ✅ リリースノートがNotionに同期されました
      
      📝 更新内容: ${{ github.event.head_commit.message }}
      👤 実行者: ${{ github.actor }}
      🔗 コミット: ${{ github.server_url }}/${{ github.repository }}/commit/${{ github.sha }}
```

## 🆘 トラブルシューティング

### Notion同期が失敗する場合

1. **Notion Token の確認**
   - Tokenが正しくコピーされているか
   - Integrationがページに追加されているか

2. **Page ID の確認**
   - 32文字の英数字であることを確認
   - URLから正しく抽出されているか

3. **権限の確認**
   - IntegrationにRead/Write権限があるか
   - 対象ページでIntegrationが有効になっているか

### Slack通知が届かない場合

1. **Webhook URLの確認**
   - URLが正しくコピーされているか
   - 余分な空白や改行が含まれていないか

2. **チャンネル設定の確認**
   - Webhookが正しいチャンネルに設定されているか
   - チャンネル名の先頭に`#`が付いているか

3. **Secretsの確認**
   ```bash
   # GitHub CLIでSecretsの存在を確認
   gh secret list
   ```

### エラーメッセージ別対処法

| エラー | 原因 | 対処法 |
|--------|------|--------|
| `invalid_payload` | JSON形式が不正 | メッセージの構文を確認 |
| `action_prohibited` | Webhookが無効化 | Slack管理画面で再有効化 |
| `channel_not_found` | チャンネルが存在しない | チャンネル名を確認 |
| `no_service` | Webhook URLが無効 | 新しいWebhookを作成 |

## 🧪 テスト方法

### Notion同期のテスト

1. テスト用のMarkdownファイルを作成してpush
2. GitHub ActionsのログでNotion APIの応答を確認

### Slack通知のテスト

コマンドラインから直接テスト：

```bash
# Webhook URLをテスト（実際のURLに置き換えてください）
curl -X POST https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXX \
  -H 'Content-Type: application/json' \
  -d '{"text": "テストメッセージ: GitHub ActionsからSlackへの通知テスト"}'
```

## 📝 注意事項

- 同期は**一方向**（GitHub → Notion）
- Notion側での編集は次回同期時に上書きされる
- 大量のファイルを同期する場合はAPI制限に注意
- Webhook URLは秘密情報として扱う（GitHub Secretsを必ず使用）
- `<details>`タグ: Notionでは折りたたみブロックとして変換される

## 🔒 セキュリティのベストプラクティス

1. **秘密情報の管理**
   - Notion TokenやWebhook URLはコードに直接記載しない
   - 必ずGitHub Secretsを使用

2. **アクセス範囲の制限**
   - Notion Integrationは必要最小限のページのみに追加
   - Slack Webhookは必要なチャンネルのみに限定

3. **定期的な監査**
   - 不要になったIntegrationやWebhookは削除
   - アクセスログを定期的に確認

## 🔗 参考リンク

- [Notion API Documentation](https://developers.notion.com/)
- [notion-sync GitHub](https://github.com/startnext/notion-sync)
- [Slack Incoming Webhooks](https://api.slack.com/messaging/webhooks)
- [GitHub Actions Documentation](https://docs.github.com/actions)
- [GitHub Encrypted Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)