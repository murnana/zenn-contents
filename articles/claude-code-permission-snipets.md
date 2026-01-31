---
title: "[Claude Code] permissions内の書き方たち"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claude code"]
published: false
---

# Claude Codeの permissionsとは

.claude/settings.json ファイル内に `permissions` という項目があります。
これは、Claude Codeにどのようなことを自動的に許可し、どのようなことを自動的に拒否するのか?という設定があります。

例えばこのリポジトリーの場合、 `npx textlint` コマンドの実行は自動で許可しますが、Claude Codeの認証ファイルは読みに行かせない、みたいなことをしています。

https://github.com/murnana/zenn-contents/blob/4db94b74c529eba60cffe8628107d47927b23c03/.claude/settings.json#L5-L13

settings.json については、詳しくは [Claude Code settings - Claude Code Docs](https://code.claude.com/docs/en/settings#permission-settings) にあります。しかし、ツール毎の細かい表記レシピについては中々見つからないため、自分用にここに書こうと思います。

# ツール別の書き方

## Read & Edit
Claude Codeが読み取りを行うときのものです。
Editは編集系に対するものになります。

### 認証情報読み取らせたくない
アスタリスク「`*`」がワイルドカードになっています。
```json:settings.json
{
  "permissions": {
    "deny": [
      "Read(**/*credentials.json)",
      "Read(**/appsettings.*.json)"
    ]
  }
}
```
絶対パスの場合、先頭に `//`を付けます
```json:settings.json
{
  "permissions": {
    "deny": [
      "Read(//home/node/.claude.json)",
      "Read(//home/node/.claude.json*)"
    ]
  }
}
```


# 参考
- [Identity and Access Management - Claude Code Docs](https://code.claude.com/docs/en/iam)
- [Claude Code settings - Claude Code Docs](https://code.claude.com/docs/en/settings)
