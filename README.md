# Murnana's Zenn Articles & Books

* [📘 How to use Zenn CLI](https://zenn.dev/zenn/articles/zenn-cli-guide)

## 開発環境

このリポジトリでは、用途別に2つのDev Container環境を提供しています。

### 📝 執筆用環境（`.devcontainer/write/`）

記事の執筆・編集に使用するフル機能の開発環境です。

**特徴:**
- Node.js 22（Debian Bookworm）
- Claude Code CLI搭載（AI執筆支援）
- VS Code拡張機能:
  - Zenn Preview（リアルタイムプレビュー）
  - Markdown All in One（編集支援）
  - Claude Code（AI統合）
- Claude認証情報の自動マウント
- セキュリティ強化設定（非root実行、権限制限）

**使い方:**
```bash
# VS Codeで開く
code .
# コマンドパレット（Ctrl+Shift+P）から
# "Dev Containers: Reopen in Container" を選択
# ".devcontainer/write/devcontainer.json" を選択
```

### 🤖 CI環境（`.devcontainer/ci/`）

GitHub Actionsでのtextlint実行に特化した軽量環境です。

**特徴:**
- Node.js 22（Debian Bookworm）
- 最小限の依存関係（textlintのみ）
- 拡張機能なし（高速起動）
- マウント設定なし（CI環境での互換性）
- セキュリティ強化設定（非root実行、権限制限）

**用途:**
- GitHub ActionsのPRチェック（自動実行）
- ローカルでのCI環境再現テスト

## コマンド

```bash
# 記事のプレビュー
npx zenn preview

# 新規記事作成
npx zenn new:article

# textlint実行
npm test
```
