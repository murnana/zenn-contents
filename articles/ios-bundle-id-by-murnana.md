---
title: "Bundle ID または App ID"
emoji: "🍎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ios","unity"]
published: true
published_at: 2024-10-26 19:00
---

iOSがアプリケーションを識別する主な情報はバンドルID (Bundle ID) です。

App Store上では、アプリID (App ID) と呼ばれる場合もあります。厳密にはバンドルIDとは別物ですが、アプリIDがバンドルIDを内包していること、アプリID内にあるもう1つのIDはAppleが発行するため、この違いは普段あまり意識をしません。

:::details なぜこのような記事を書いたか
ゲームエンジンでスマートフォンゲームを作るとき、普段意識してないからです。

Unity 2022から、iOSも含めた `Application.applicationIdentifier` のようにアプリの識別子を持つプラットフォームは、新規プロジェクト生成時、デフォルトでProduct NameとCompany Nameから引っ張って勝手に作ってくれるようになっています。

これを放っておくと、アプリ名を変更する度に別のアプリとしてインストールしたり、変な名前のIDのまま運用することになります。

:::

## バンドルIDとアプリID

「バンドルID」と「アプリID」は厳密には違うIDです。
普段この2つの違いを意識することはありませんが、注意が必要です。

### バンドルID

「バンドルID」はアプリ実行に必要なプログラム、アセット等を固めてできた成果物に振られるIDです。

iOSはこのIDの違いで同じアプリかを判断しています。

例えば開発用と本番用に分けたい場合、本番用にはApp Storeで配布する時に使うIDと同じものを使用します。そうではない場合、別のIDを使うという方法を取れます。

- [CFBundleIdentifier | Apple Developer Documentation](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleidentifier)

### アプリID

「アプリID」はプロビジョニングプロファイル内で識別されるIDです。

公式ドキュメントにある「2つのパーツからなる文字列」は、チームID (Team ID) とバンドルID (Bundle ID) のことを指します。

![app-id.svg](/images/articles/ios-bundle-id-by-murnana/app-id.webp)

- [アプリIDを登録する - IDを管理する - アカウント - ヘルプ - Apple Developer](https://developer.apple.c/jp/help/account/manage-identifiers/register-an-app-id/)
- [App ID](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/AppID.html)

#### チームID

チームIDはAppleが自動生成するIDです。全10文字で構成されています。

チームIDは Apple Developer アカウントの「メンバーシップの詳細」で確認できます。

- [チームIDを確認する - チームを管理する - アカウント - ヘルプ - Apple Developer](https://developer.apple.com/jp/help/account/manage-your-team/locate-your-team-id/)


### バンドルIDはアプリIDと紐づく

ビルド後の成果物をApp Store Connectへアップロードすると、成果物のバンドルIDを見て同一のアプリページを用意してくれます。App Store側からすれば、このIDは「アプリID」(英語では App ID) として扱われます。

このアプリIDは全世界レベルで唯一である必要があります。そのため **App Storeへのアプリアップロード後、バンドルIDやアプリIDを変更できません** 。

## バンドルIDの命名規則

英数字 (`A–Z`, `a–z`, `0–9`)、ハイフン (`-`)、ピリオド (`.`) を組み合わせた文字列です。

通常、DNS (Domain Name System)で使用している、組織のドメイン名を反転した名前を持ってきます。これは「Organization identifier」と呼ばれます。

例えば「www.example.com」というドメイン名を使っていて、アプリのIDを「hoge」であれば、「`com.example.hoge`」という接頭文字をつけます。

- [Creating an Xcode project for an app | Apple Developer Documentation](https://developer.apple.com/documentation/xcode/creating-an-xcode-project-for-an-app)
- [Preparing your app for distribution | Apple Developer Documentation](https://developer.apple.com/documentation/xcode/preparing-your-app-for-distribution)

## 明示的なアプリIDと、ワイルドカードアプリID

アプリIDは、アプリを配布する前にApple Developerで登録する必要があります。

このIDには2種類あります。1つが単一のアプリで使用される「明示的なアプリID(Explict App ID)」、もう1つは複数のアプリで使用可能な「ワイルドカードアプリID(Whildcard App ID)」です。

明示的なアプリIDは、唯一のアプリに対するプロビジョニングプロファイルを生成できます。
アプリ内課金や、Apple Push Notification Service (APNs)を使用できます。

一方、ワイルドカードアプリIDはワイルドカード「*」を用いることで、ワイルドカード部分に任意の文字列を含めたアプリを生成できます。
例えば「`com.example.*`」というアプリIDを登録しておけば、`com.example.hoge`や``com.example.page`といった複数のアプリを同じプロビジョニングプロファイルで作ることができます。
一方で、アプリ内課金や、Apple Push Notification Service (APNs)といった一部のAppleのサービスを使うことはできません。

- [アプリIDを登録する - IDを管理する - アカウント - ヘルプ - Apple Developer](https://developer.apple.c/jp/help/account/manage-identifiers/register-an-app-id/)

## Unityでの設定方法

Unityで設定する際は以下の手順があります。
※前提として、UnityにiOSモジュールが必要です。

1. Unity プロジェクトを開く
2. メニューの「Edit」から「Project Settings…」を開く
3. 開かれたウィンドウの左メニューから「Player」を選択する
4. プラットフォームの「iOS」を選択する
5. 「Other Setttings」を開く
6. 「Override Default Bundle Identifier」にチェックを入れる
7. 「Bundle Identifier」を入れる

C#スクリプトで書く場合は、 `PlayerSettings.SetApplicationIdentifier` を使います。
```cs
UnityEditor.PlayerSettings.SetApplicationIdentifier(buildTarget: UnityEditor.Build.NamedBuildTarget.iOS, identifier: "com.example.hoge")
```

Unity 6以降、ビルドプロファイルを使用することで開発と本番などを分けることもできます。詳しくはマニュアルをご覧ください。

- [Unity - Manual: iOS Player settings](https://docs.unity3d.com/Manual/class-PlayerSettingsiOS.html#Identification)

---

:::details 画像に使用しているフォント
- [あんずもじ2020](http://www8.plala.or.jp/p_dolce/)
- [JetBrains Mono](https://www.jetbrains.com/lp/mono/)
:::
