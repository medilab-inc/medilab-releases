# {Product} v{version} リリースノート

**リリース日:** {date}  
**製品:** {product_name}  
**バージョン:** v{version}  
**タイプ:** {release_type} (Major/Minor/Patch)

## 📊 リリースサマリー

| 項目 | 詳細 |
|------|------|
| 影響範囲 | {impact_level} |
| ダウンタイム | {downtime} |
| データ移行 | {migration_required} |
| 対象環境 | {environments} |

## 🎯 主要な変更点

### ユーザー向け新機能・改善
<!-- ユーザーが直接体感できる機能を記載 -->

#### 1. {feature_name}
- **概要:** {description}
- **確認方法:**
  1. {step1}
  2. {step2}
  3. {step3}
- **関連PR:** #{pr_numbers}

### 管理者向け新機能・改善
<!-- 管理者・運用者向けの機能を記載 -->

#### 1. {admin_feature_name}
- **概要:** {description}
- **確認方法:**
  1. {step1}
  2. {step2}
- **関連PR:** #{pr_numbers}

## 📝 詳細な変更内容

### 🚀 新機能 (New Features)
<!-- feature ラベルのPR -->
- [ ] {feature_description} (#{pr_number})

### ⚡ 機能改善 (Enhancements)
<!-- enhancement ラベルのPR -->
- [ ] {enhancement_description} (#{pr_number})

### 🐛 バグ修正 (Bug Fixes)
<!-- bug ラベルのPR -->
- [ ] {bug_description} (#{pr_number})

### 🏎️ パフォーマンス改善 (Performance)
<!-- performance ラベルのPR -->
- [ ] {performance_description} (#{pr_number})

### 🔒 セキュリティ (Security)
<!-- security ラベルのPR -->
- [ ] {security_description} (#{pr_number})

### 🏗️ インフラ・基盤 (Infrastructure)
<!-- infrastructure ラベルのPR -->
- [ ] {infra_description} (#{pr_number})

## 🔧 技術的変更

### 依存関係の更新
- {dependency} を v{old_version} から v{new_version} へ更新

### 破壊的変更
<!-- Breaking Changes がある場合のみ記載 -->
⚠️ **注意:** 以下の変更は既存の実装に影響を与える可能性があります。

- {breaking_change_description}

### 廃止予定機能
<!-- Deprecated features がある場合のみ記載 -->
- {deprecated_feature} は v{future_version} で削除予定です

## 📋 動作確認チェックリスト

### 基本動作確認
- [ ] ログイン・ログアウト
- [ ] 主要画面の表示
- [ ] データの作成・更新・削除

### 新機能確認
- [ ] {new_feature_check1}
- [ ] {new_feature_check2}

### 修正確認
- [ ] {bug_fix_check1}
- [ ] {bug_fix_check2}

## ⚠️ 既知の問題

| 問題 | 影響 | 回避策 | 修正予定 |
|------|------|--------|----------|
| {issue} | {impact} | {workaround} | v{fix_version} |

## 🔄 アップグレード手順

### 前提条件
- {prerequisite1}
- {prerequisite2}

### 手順
1. **バックアップ**
   ```bash
   # バックアップコマンド例
   ```

2. **サービス停止**
   ```bash
   # 停止コマンド例
   ```

3. **アップグレード実行**
   ```bash
   # アップグレードコマンド例
   ```

4. **動作確認**
   - 上記チェックリストを実施

### ロールバック手順
問題が発生した場合：
```bash
# ロールバックコマンド例
```

## 📊 統計情報

### PR統計
- **Backend PRs:** {be_pr_count}
- **Frontend PRs:** {fe_pr_count}
- **その他:** {other_pr_count}
- **合計:** {total_pr_count}

### 変更の内訳
```
新機能:           ████████░░ 40%
バグ修正:         ██████░░░░ 30%
パフォーマンス:   ████░░░░░░ 20%
その他:           ██░░░░░░░░ 10%
```

## 🙏 Contributors

このリリースに貢献してくださった皆様：
- @{contributor1}
- @{contributor2}

## 📚 関連ドキュメント

- [ユーザーマニュアル]({user_manual_link})
- [API ドキュメント]({api_doc_link})
- [移行ガイド]({migration_guide_link})

## 📞 サポート

問題が発生した場合：
- **Slack:** #{support_channel}
- **Email:** support@medilab.jp
- **緊急連絡先:** {emergency_contact}

---

*このリリースノートは {generated_date} に生成されました。*