---
title: "Visual Studio Codeのターミナルにのみパスを通す方法"
emoji: "⚙️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vscode"]
published: false
---

普段のターミナルまたはコンソールには不要でも、Visual Studio Codeに統合されたターミナルでパスを通しておきたい場合があります。
以下は、そんなときの設定方法です。

:::details Visual Studio Codeに統合されたターミナルとは
CLIツールの実行などを行う場所である、ターミナルには二種類があります。

- _Integrated terminal_ ... Visual Studio Codeに統合されたターミナル
- _External terminal_ ... Visual Studio Codeの外側、コンピューターの環境設定で設定されているターミナル
  - Windowsの場合、cmd.exeやPowerShell、Windowsターミナルなど
  - macOSの場合、ターミナルアプリなど

この記事では、 _Integrated terminal_ でパスを通す方法について記述しています。
:::

## コンピューター毎、OS毎にパスを通す
Visual Studio Codeの設定項目に、 `terminal.integrated.env.<platform>` という項目があります。ここに設定することで、Visual Studio Codeの統合ターミナルにパスを通すことができます。

例えばWindowsで、ローカルインストールされた Claude Codeに対してパスを通す場合は以下になります。
```json
"terminal.integrated.env.windows": {
    "PATH": "${env:USERPROFILE}\\.claude\\local\\node_modules\\.bin;${env:PATH}"
}
```

設定の適用範囲は、ユーザー毎、プロジェクト毎等いくつかあります。
これについては [User and workspace settings](https://code.visualstudio.com/docs/configure/settings) を読むとよいでしょう。


## ターミナルのプロファイル毎にパスを通す
上記設定はどのプロファイルでも通しますが、ターミナルのプロファイル毎に設定できます。

その設定は `terminal.integrated.profiles.<platform>.<profile_name>.env` の中になります。

```json
"terminal.integrated.profiles.windows": {
    // .. 前略 ..
    "PowerShell (Claude Code)": {
        "source": "PowerShell",
        "icon": "terminal-powershell",
        "env": {
            "PATH": "${env:USERPROFILE}\\.claude\\local\\node_modules\\.bin;${env:PATH}"
        }
    }
}
```

## 参考
* [Terminal Profiles](https://code.visualstudio.com/docs/terminal/profiles#_configuring-profiles)
* [Variables reference](https://code.visualstudio.com/docs/reference/variables-reference)
* [How can I globally set the PATH environment variable in VS Code? - Stack Overflow](https://stackoverflow.com/a/45637716/9357460)
* [【VSCode】ターミナル起動時のパス（PATH）を変更する方法](https://note.affi-sapo-sv.com/vscode-terminal-path.php)
