---
title: "[Homebrew] brew bundleで環境構築を統一する"
emoji: "📦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["homebrew", "mac"]
published: true
published_at: 2022-11-19 19:00
---

# 使い方 (プロジェクト単位の場合)

## 1. Homebrewをインストール
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```


## 2. _Brewfile_ を作成したいディレクトリに移動

コマンドを実行する前に `cd` で移動します。

例えばリポジトリ単位の場合は、リポジトリのルートディレクトリに移動します。
仮に、移動先を `<プロジェクトのルートディレクトリのパス>` とすると、コマンドは以下になります。
```bash
cd <プロジェクトのルートディレクトリのパス>
```


## 3. _Brewfile_ を作成

_Brewfile_ を作成するコマンドを打ちます。
```bash
brew dump
```


## 4. 作成された _Brewfile_ を編集

`brew dump` は、現在の環境にあるものをリストアップします
そのため内容を編集し、必要な分だけ記載するようにします。

ファイルは _<プロジェクトのルートディレクトリのパス>/Brewfile_ にあります。


## 5. _Brewfile_ 内に記載されたパッケージをロック

_Brewfile_ だけだと、別の環境でバージョンを合わせるのが難しくなります。
そのため、_Brewfile.lock.json_ を生成します。

生成するためのコマンドは以下になります。
```bash
brew install --no-upgrade
```



# 参考

- [macOS（またはLinux）用パッケージマネージャー — Homebrew](https://brew.sh/index_ja)
- [brew(1) – The Missing Package Manager for macOS (or Linux) — Homebrew Documentation](https://docs.brew.sh/Manpage#bundle-subcommand) _- bundle -_
- [Homebrew/homebrew-bundle: 📦 Bundler for non-Ruby dependencies from Homebrew, Homebrew Cask and the Mac App Store.](https://github.com/Homebrew/homebrew-bundle#usage)
- [Macの開発環境構築を自動化する 2019年夏版 - karakaram-blog](https://www.karakaram.com/how-to-automate-your-mac-set-up/)
