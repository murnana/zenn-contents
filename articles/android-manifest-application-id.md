---
title: "[Android] Gradleで設定するapplicationIdを、マニフェストファイルでも使うには"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["android"]
published: false
---





# 参考

## マニフェストファイル _AndroidManifest.xml_ について
- [App Manifest Overview | Android Developers](https://developer.android.com/guide/topics/manifest/manifest-intro)
- [アプリ マニフェストの概要 | Android デベロッパー | Android Developers](https://developer.android.com/guide/topics/manifest/manifest-intro?hl=ja)

## application ID - Gradle _build.gradle_ での設定
- [Configure the app module | Android Developer](https://developer.android.com/studio/build/configure-app-module#set_the_application_id)
- [アプリ モジュールを設定する | Android デベロッパー | Android Developers](https://developer.android.com/studio/build/configure-app-module?hl=ja#set_the_application_id)


## マニフェストファイルでの `${applicationId}` の記述について
- [Manage manifest files | Android Developers](https://developer.android.com/studio/build/manage-manifests#inject_build_variables_into_the_manifest) - Inject build variables into the manifest
- [マニフェスト ファイルを管理する | Android デベロッパー | Android Developers](https://developer.android.com/studio/build/manage-manifests?hl=ja#inject_build_variables_into_the_manifest) - マニフェストにビルド変数を追加する
- [Build Variantsで開発版Androidアプリを分ける - ninjinkun's diary](https://ninjinkun.hatenablog.com/entry/2014/08/18/102849)
- [android - Using ${applicationId} in library manifest - Stack Overflow](https://stackoverflow.com/questions/30790768/using-applicationid-in-library-manifest)
