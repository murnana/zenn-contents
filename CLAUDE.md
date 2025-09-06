# CLAUDE.md

このファイルは、Claude Code（claude.ai/code）がこのリポジトリで作業する際のガイダンスを提供します。

## プロジェクト概要

これは日本語の技術記事と本を含むZennコンテンツリポジトリです。Zennは日本の技術文書プラットフォームです。リポジトリには以下が含まれます：

- **articles/**: Markdown形式の技術記事（28記事）
- **books/**: Markdown形式の技術書（現在は空ですが、コンテンツ追加可能）
- **images/**: 記事や本の静的アセット
- **assets/**: 追加の静的リソース

## 開発コマンド

### コンテンツ管理
```bash
# ブラウザでコンテンツをローカルプレビュー
npx zenn preview

# 新しい記事を作成
npx zenn new:article

# 新しい本を作成
npx zenn new:book

# 全記事をリスト表示
npx zenn list:articles

# 全ての本をリスト表示
npx zenn list:books
```

### 品質保証
```bash
# 全マークダウンコンテンツでtextlintを実行（メインテストコマンド）
npm test
# 以下と同等: textlint "./{articles,books}/*.md"

# 特定のオプションでtextlintを手動実行
npx textlint -f checkstyle './{articles,books}/*.md'
```

## コンテンツ構造

### 記事形式
記事はZennのfrontmatter形式を使用します：
```yaml
---
title: "記事タイトル"
emoji: "🎮"  # 表示絵文字
type: "tech"  # tech: 技術記事 / idea: アイデア記事
topics: ["android", "game"]  # タグ
published: true
published_at: 2023-01-14 19:00  # 公開日時（オプション）
---
```

### コンテンツガイドライン
- 記事は日本語で書かれています
- 技術文書スタイル（です/ます調）を使用します
- textlintで強制される日本語技術文書の慣例に従います
- textlintルールには日本語技術文書、JTFスタイル、一般的な日本語のプリセットが含まれます

## 開発環境

プロジェクトはDev Containersを使用します：
- Node.js 20（Bullseye）
- 必要なVS Code拡張機能：
  - `zenn.zenn-preview` - Zennコンテンツプレビュー
  - `yzhang.markdown-all-in-one` - Markdown編集サポート
  - `taichi.vscode-textlint` - textlint統合

## CI/CD

GitHub Actionsワークフロー（`proofreading.yml`）が`main`ブランチへのプルリクエストで実行されます：
- `.md`ファイル、packageファイル、ワークフローファイルの変更でトリガー
- Dev Containerを使用してtextlintを実行
- textlint違反の自動PRレビューにreviewdogを使用
- textlintの問題をPRコメントで直接報告

## textlint設定

`.textlintrc`に配置され、以下を含みます：
- 日本語言語プリセット（preset-japanese、preset-jtf-style、preset-ja-technical-writing）
- Zennマークダウン構文用のカスタムルール（`:::`と`:::details`ブロックを許可）
- 文の長さ制限を無効化
- コンテンツ全体でです/ます調を強制