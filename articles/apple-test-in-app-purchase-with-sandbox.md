---
title: "[Apple] 課金テストで使用する、Sandbox用Apple IDについて"
emoji: "🏜"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ios", "macOS"]
published: true
published_at: 2023-10-21 19:00
---

# 作り方
作り方に詳細については、 [Sandbox 用 Apple ID の作成 - App 内課金のテスト - App Store Connect - Help - Apple Developer](https://developer.apple.com/jp/help/app-store-connect/test-in-app-purchases/create-sandbox-apple-ids/) をご覧ください。
ここでは、注意点だけ挙げておきます。


## 既存のApple IDは Sandbox用にすることはできない
一般的に使用される Apple IDは、Sandbox用に切り替えられません。

また、一般的に使用されているApple IDのメールアドレスで、Sandbox用 Apple IDを作成はできません。

必ず Sandbox用 Apple IDとなるメールアドレスを新規に用意してください。
用意するメールアドレスは、例えば[Gmailのエイリアス](https://support.google.com/mail/answer/22370?hl=ja#zippy=%2Cgmail-%E3%82%A8%E3%82%A4%E3%83%AA%E3%82%A2%E3%82%B9%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6%E3%83%A1%E3%83%BC%E3%83%AB%E3%81%AE%E8%87%AA%E5%8B%95%E6%8C%AF%E3%82%8A%E5%88%86%E3%81%91%E3%82%92%E8%A1%8C%E3%81%86) や [iCloudメールのメールエイリアス](https://support.apple.com/ja-jp/guide/icloud/mm6b1a490a/icloud) でもかまいません。



# 説明
例えばソーシャルゲームにおける有償ジェムの購入、という機能を実装した場合、
課金テストを行う必要があります。

App Storeで販売する場合の課金テストは、現金やクレジットカードなどの支払い以外に、
Sandbox用Apple IDを通した課金テストが可能です。



# 参考

## Sandbox Apple ID の作成
- ~~[Sandbox 用 Apple ID の作成 - App Store Connect ヘルプ](https://help.apple.com/app-store-connect/#/dev8b997bee1)~~ 旧ドキュメント
- [Sandbox 用 Apple ID の作成 - App 内課金のテスト - App Store Connect - Help - Apple Developer](https://developer.apple.com/jp/help/app-store-connect/test-in-app-purchases/create-sandbox-apple-ids/)
- [Create sandbox Apple IDs - Test in-app purchases - App Store Connect - Help - Apple Developer](https://developer.apple.com/help/app-store-connect/test-in-app-purchases/create-sandbox-apple-ids/)


## SandboxでのApp内課金のテスト
- [Testing in-app purchases with sandbox - Apple Developer Documentation](https://developer.apple.com/documentation/storekit/in-app_purchase/testing_in-app_purchases_with_sandbox)
- [SandboxでのApp内課金のテスト - 日本語ドキュメント - Apple Developer](https://developer.apple.com/jp/documentation/storekit/in-app_purchase/testing_in-app_purchases_with_sandbox/)


## 開発の全段階におけるXcodeとSandboxによるテスト
- [Testing at all stages of development with Xcode and sandbox - Apple Developer Documentation](https://developer.apple.com/documentation/storekit/in-app_purchase/testing_at_all_stages_of_development_with_xcode_and_sandbox)
- [開発の全段階におけるXcodeとSandboxによるテスト - 日本語ドキュメント - Apple Developer](https://developer.apple.com/jp/documentation/storekit/in-app_purchase/testing_at_all_stages_of_development_with_xcode_and_sandbox/)
