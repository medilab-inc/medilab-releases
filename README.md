# MediLab Release Notes

MediLab製品群のリリースノートを一元管理するリポジトリです。

## 📦 対象製品

- **Kakeru（カケル君）** - 診療記録・SOAP管理システム
- **Mamoru（まもる君）** - [製品説明を追加]

## 📁 ディレクトリ構造

```
medilab-releases/
├── products/
│   ├── kakeru/              # Kakeru製品のリリースノート
│   │   └── releases/
│   │       └── 2025/
│   │           ├── Q1/
│   │           ├── Q2/
│   │           ├── Q3/
│   │           └── Q4/
│   └── mamoru/              # Mamoru製品のリリースノート
│       └── releases/
│           └── 2025/
└── templates/               # リリースノートテンプレート
```

## 📝 リリースノートの作成

### 1. ファイル作成

該当製品・四半期のディレクトリに新しいリリースノートを作成します。

```bash
# 例：Kakeruの2025年Q3リリースノート作成
cp templates/release-note-template.md products/kakeru/releases/2025/Q3/v2.5.0_2025-08-27.md
```

### 2. 内容記載

テンプレートに従って以下の内容を記載します：
- バージョン情報
- 主要な変更点（ユーザー視点）
- 技術的変更点（必要に応じて）
- 確認方法
- 既知の問題

## 📋 命名規則

### ファイル名
```
v{major}.{minor}.{patch}_{YYYY-MM-DD}.md
```

例：
- `v2.5.0_2025-08-27.md` - メジャーリリース
- `v2.4.1_2025-08-15.md` - パッチリリース
- `v2.4.0-hotfix1_2025-08-16.md` - ホットフィックス

## ✏️ 記述ルール

### 必須項目

1. **バージョン情報**
   - バージョン番号（セマンティックバージョニング）
   - リリース日
   - リリースタイプ（Major/Minor/Patch/Hotfix）

2. **変更内容**
   - ユーザー向け新機能・改善
   - バグ修正
   - 既知の問題

3. **確認方法**
   - 各機能の動作確認手順
   - 期待される結果

### 推奨カテゴリ

- 🚀 **新機能** - 新しく追加された機能
- ⚡ **機能改善** - 既存機能の改善
- 🐛 **バグ修正** - 不具合の修正
- 🏎️ **パフォーマンス** - 速度・効率の改善
- 🔒 **セキュリティ** - セキュリティ関連の修正
- 🏗️ **インフラ** - 基盤・インフラの変更
- 📚 **ドキュメント** - ドキュメントの更新

### PR参照

GitHub PRへの参照は以下の形式で記載：
```
- 機能の説明 (#PR番号)
- [BE] バックエンドの変更 (yakureki-back#123)
- [FE] フロントエンドの変更 (yakureki-front#456)
```

## 🔗 関連リポジトリ

### Kakeru
- Backend: https://github.com/medilab-inc/yakureki-back
- Frontend: https://github.com/medilab-inc/yakureki-front

### Mamoru
- [リポジトリリンクを追加]

## 📧 お問い合わせ

- Slack: #release-notes
- Email: release@medilab.jp