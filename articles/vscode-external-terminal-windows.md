---
title: "[Visual Studio Code] 外部ターミナルを Windows Terminal に設定する"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vscode","windowsterminal"]
published: false
---

## 設定方法

設定の項目 `terminal.external.windowsExec` に、以下を入れます。

```json
{
  "terminal.external.windowsExec": "wt"
}
```

または「設定」の検索欄に `terminal.external.windowsExec` を入れて探し、`wt` と入れます。

![](/images/articles/vscode-external-terminal-windows/terminal.external.windowsExec.webp)


## 説明

:::message
この「説明」セクションは、AI（Claude）が執筆しました。既定値と起動時の挙動は、VS Code のソースコードで確認しています。
:::

### `terminal.external.windowsExec` とは

Visual Studio Code が「外部ターミナル」として起動する実行ファイルを指定する設定です。既定値は `C:\Windows\System32\cmd.exe` です。

この設定が使われるのは、次の操作で外部ターミナルを開くときです。

- コマンドパレットの `Open New External Terminal`（コマンド ID: `workbench.action.terminal.openNativeConsole`）
- エクスプローラーの右クリックメニューにある「外部ターミナルで開く」

エディター内に表示される統合ターミナルには影響しません。統合ターミナルのシェルは `terminal.integrated.defaultProfile.windows` で変更します。

### `wt` とは

`wt` は Windows Terminal の実行エイリアス（アプリ実行エイリアス）です。実体は `%LOCALAPPDATA%\Microsoft\WindowsApps\wt.exe` にあり、このフォルダーが `PATH` に含まれているため、フルパスを書かなくても起動できます。

`terminal.external.windowsExec` に `wt` を指定すると、外部ターミナルを開く操作で、対象フォルダーを作業ディレクトリにした Windows Terminal が起動します。

## 参考
* [How to set Windows Terminal (UWP) as an external terminal for Visual Studio Code? - Stack Overflow](https://stackoverflow.com/a/58812185/9357460)
* [vscode/externalTerminalService.ts at 8494e45 · microsoft/vscode](https://github.com/microsoft/vscode/blob/8494e45e1f3e8c51c21aeb0ee7c3ebada6eb821c/src/vs/platform/externalTerminal/node/externalTerminalService.ts)
