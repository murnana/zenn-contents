---
title: "[fastlane] fastlaneの更新"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["fastlane"]
published: false
---

# 方法

## Bundlerでfastlaneをセットアップした場合
`bundle update fastlane` を実行します。
(Gitの場合)その後、 `Gemfile.lock` ファイルをコミットします。



# 説明
fastlaneはAndroidまたはiOSアプリのデプロイをシンプルにするためのツールです。

fastlane自身の更新は、セットアップ時の環境によって方法が異なります。


## Bundlerでfastlaneをセットアップした場合
Bundlerのシステムで更新します。

Bundlerは、Rubyプロジェクト向けに、Rubyのバージョンと依存関係にあるgemのインストールを解決するためのツールです。
gem は、Rubyで書かれたライブラリの形式です。

Bundlerで管理されているgemは、Bundlerのコマンド `bundle update` で更新できます。
引数なしに、そのまま `bundle update` を実行すると、`Gemfile` に書かれたgemすべてを更新しようとします。

fastlaneと、fastlaneが依存するgemを更新する場合は `bundle update fastlane` を実行します。

ただし `bundle update fastlane` は、依存関係で、他のgemとも依存関係があるものも更新されます。そのため fastlane 以外のツールで `bundle` コマンドなどで使用している場合は、fastlane と共通して使用しているgemについて注意する必要があります。



# 参考
- [Getting started with fastlane for iOS](https://docs.fastlane.tools/getting-started/ios/setup/#managed-ruby-environment-bundler-macoslinuxwindows) - _fastlane - fastlane docs_
- [Bundler: bundle update](https://bundler.io/v1.13/man/bundle-update.1.html)
- [bundle updateで特定のgemのみ更新する時に気をつけるべきポイント](https://scrapbox.io/10nin/bundle_update%E3%81%A7%E7%89%B9%E5%AE%9A%E3%81%AEgem%E3%81%AE%E3%81%BF%E6%9B%B4%E6%96%B0%E3%81%99%E3%82%8B%E6%99%82%E3%81%AB%E6%B0%97%E3%82%92%E3%81%A4%E3%81%91%E3%82%8B%E3%81%B9%E3%81%8D%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88)
