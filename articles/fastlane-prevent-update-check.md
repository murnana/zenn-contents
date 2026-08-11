---
title: "[fastlane] fastlaneが立ち上がる時に、fastlane自身の更新チェックを入れない方法"
emoji: "🔇"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["fastlane"]
published: false
---

# 方法
環境変数 `FASTLANE_SKIP_UPDATE_CHECK` を設定します。

```txt:.env
FASTLANE_SKIP_UPDATE_CHECK="1"
```


# 説明
fastlaneはAndroidまたはiOSアプリのデプロイをシンプルにするためのツールです。

fastlaneは通常、コマンドを使用する度にfastlane自身やfastlaneに入っているアクションの更新状態をチェックし、
更新があればそれをCUI上で伝えます。

一方で、更新の確認が必要ないこともあります。
例えばアプリ提出用としてビルドするときや、緊急でHotfixのアプリをリリースする時などです。

fastlaneでは、環境変数 `FASTLANE_SKIP_UPDATE_CHECK` で更新チェックをスキップできます。



# 参考
- [Configuring fastlane](https://docs.fastlane.tools/advanced/fastlane/#configuring-fastlane) - _fastlane - fastlane docs_
- [setting FASTLANE_SKIP_UPDATE_CHECK env variable does not prevent update check · Issue #10163 · fastlane/fastlane](https://github.com/fastlane/fastlane/issues/10163)
