---
title: "[Rider] ソースコードの最後に改行を入れる設定"
emoji: "🎬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["rider"]
published: true
published_at: 2024-05-06 20:00
---

## 設定方法
言語設定がデフォルト(英語)の場合、Settingsより「`Line feed at end of file`」と検索すると、言語毎に設定できます。
![Settings > Editor > Code Style > C#](/images/articles/rider-insert-final-newline-by-murnana/settings-en.png)*Line feed at end of file がハイライトされている様子*


言語設定が日本語の場合、設定より「`EOF の改行`」と検索すると、言語毎に設定できます。
![Settings > Editor > Code Style > C#](/images/articles/rider-insert-final-newline-by-murnana/settings-ja.png)*EOF の改行 がハイライトされている様子*


## 参考
- [EditorConfig properties for C#: Line Breaks | JetBrains Rider Documentation](https://www.jetbrains.com/help/rider/EditorConfig_CSHARP_LineBreaksPageSchema.html#resharper_csharp_insert_final_newline)
- [EditorConfig Properties · editorconfig/editorconfig Wiki](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties#insert_final_newline)
