---
title: "[Android] Gradleで設定するapplicationIdを、マニフェストファイルでも使うには"
emoji: "🆔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["android"]
published: true
published_at: 2023-12-29 19:00
---

# 使い方
`AndroidManifest.xml` の中で、 `${applicationId}` と書かれたものがビルド後置換されます。
```xml:AndroidManifest.xml
<intent-filter ... >
    <action android:name="${applicationId}.TRANSMOGRIFY" />
    ...
</intent-filter>
```


# 説明
Androidは、アプリケーションの識別子として **application ID** (**アプリケーション ID**)を持ちます。
このIDは一意である必要があります。同じIDがあると、同じアプリとして認識されます。

この性質があるため、開発者の多くは別の環境のアプリを作る場合、複数のIDを複数作ります。
Android開発では、ビルド バリアントを使用してアプリケーション IDを変更することが多いです。

しかし、アプリケーション IDは様々な場所で使われることが多いです。
例えばプッシュ通知を使用する場合、権限名にアプリケーション IDが使われます。
```xml:AndroidManifest.xml
<permission android:name="${applicationId}.permission.C2D_MESSAGE" android:protectionLevel="signature" />
<uses-permission android:name="${applicationId}.permission.C2D_MESSAGE" />
```


# 参考

## マニフェストファイル _AndroidManifest.xml_ について
- [App Manifest Overview | Android Developers](https://developer.android.com/guide/topics/manifest/manifest-intro)
- [アプリ マニフェストの概要 | Android デベロッパー | Android Developers](https://developer.android.com/guide/topics/manifest/manifest-intro?hl=ja)

## application ID - Gradle _build.gradle_ での設定
- [Configure the app module | Android Developer](https://developer.android.com/studio/build/configure-app-module#set_the_application_id)
- [アプリ モジュールを設定する | Android デベロッパー | Android Developers](https://developer.android.com/studio/build/configure-app-module?hl=ja#set_the_application_id)

## build variants (ビルド バリアント)
- [Configure build variants | Android Studio | Android Developers](https://developer.android.com/build/build-variants)
- [ビルド バリアントを設定する | Android デベロッパー | Android Developers](https://developer.android.com/studio/build/build-variants?hl=ja)

## マニフェストファイルでの `${applicationId}` の記述について
- [Manage manifest files | Android Developers](https://developer.android.com/studio/build/manage-manifests#inject_build_variables_into_the_manifest) - Inject build variables into the manifest
- [マニフェスト ファイルを管理する | Android デベロッパー | Android Developers](https://developer.android.com/studio/build/manage-manifests?hl=ja#inject_build_variables_into_the_manifest) - マニフェストにビルド変数を追加する
- [Build Variantsで開発版Androidアプリを分ける - ninjinkun's diary](https://ninjinkun.hatenablog.com/entry/2014/08/18/102849)
- [android - Using ${applicationId} in library manifest - Stack Overflow](https://stackoverflow.com/questions/30790768/using-applicationid-in-library-manifest)
