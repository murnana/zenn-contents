---
title: "[Android] <application android:isGame=true> は非推奨"
emoji: "🎮"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["android", "game"]
published: true
published_at: 2023-01-14 19:00
---

# 対処法
代わりに、`android:appCategory=game` を使います。



# 説明
Androidアプリ開発では、アプリケーション起動や使用要件を書き示した _AndroidManifest.xml_ ファイルを必ず用意します。

その中には、アプリケーションの情報を示す `<application>` タグが含まれます。

`<application>` タグにはいくつか属性を含めることができますが、その中にアプリがゲームアプリなのか、を示す属性があります。この属性を見て、Androidはアプリをカテゴリ分けするときの指標にしたり、時にはアプリの動作そのものを定義したりします。

ゲームの場合、かつては `android:isGame` という属性をつけていました。この属性がついたアプリはゲームに分類されていました。

(これがいつから変わったのか辿れていないのですが…) 今は、`android:appCategory` に変更されています。 `android:isGame` は非推奨になりました。

また、Android 12 (API 31) から「ゲームモード」という機能が追加されています。このゲームモードのAPIを使用するためにもこの属性が必要になります。



# 参考

- [Android デベロッパー  |  Android Developers. (2023). Retrieved 3 January 2023, from https://developer.android.com/guide/topics/manifest/application-element#isGame](https://developer.android.com/guide/topics/manifest/application-element#isGame)
- [Android デベロッパー  |  Android Developers. (2023). Retrieved 3 January 2023, from https://developer.android.com/guide/topics/manifest/application-element#appCategory](https://developer.android.com/guide/topics/manifest/application-element#appCategory)
- [機能と API の概要. (2023). Retrieved 3 January 2023, from https://developer.android.com/about/versions/12/features#gamemode](https://developer.android.com/about/versions/12/features#gamemode)
- [Game Mode  |  Android game development  |  Android Developers. (2023). Retrieved 3 January 2023, from https://developer.android.com/games/gamemode](https://developer.android.com/games/gamemode)
