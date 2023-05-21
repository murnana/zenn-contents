---
title: "[Unity] [Adjust] iOS14以上と未満での広告ID取得時の動作の違い"
emoji: "🛂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "adjust"]
published: true
published_at: 2023-05-22 19:00
---

# IDFA の取得方法 (Unity)

ここでは、Unityが持つ `UnityEngine.Application.RequestAdvertisingIdentifierAsync` と Unity SDK of Adjust™ を組み合わせた方法を紹介します。

Adjustを使わない場合は、「[iOS 14 Advertisiong Support](https://docs.unity3d.com/Packages/com.unity.ads.ios-support@1.2/manual/index.html)」パッケージを使うのが一般的です。



```cs
/// <summary>
/// IDFAの取得リクエスト (Private)
/// </summary>
public void RequestAdvertisingIdentifierAsync() {
  // ユーザーに問い合わせる必要があるのか
  // 0: ユーザーに質問する必要がある
  // https://github.com/adjust/unity_sdk#get-current-authorisation-status
  if (com.adjust.sdk.Adjust.getAppTrackingAuthorizationStatus() == 0)
  {
    // iOS14以降、ユーザーに問い合わせる必要がある
    com.adjust.sdk.Adjust.requestTrackingAuthorizationWithCompletionHandler(
      (int status) => {
          RequestAdvertisingIdentifierImpl();
      }
    );
  } else {
    RequestAdvertisingIdentifierImpl();
  }
}


/// <summary>
/// IDFAの取得リクエスト (Private)
/// </summary>
private void RequestAdvertisingIdentifierImpl() {
  UnityEngine.Application.RequestAdvertisingIdentifierAsync(
    delegateMethod: () =>{ (
        string  advertisingId,
        bool    trackingEnabled,
        string  error
      ) =>  {
        if (string.IsNullOrEmpty(error)) {
          // エラーなし
          Debug.LogFormat(
            "advertisingId {0}, trackingEnabled: {1}",
            advertisingId,
            trackingEnabled
          );
        } else {
          // エラーあり
          Debug.LogWarningFormat(
            "advertisingId {0}, trackingEnabled: {1}, error: {2}",
            advertisingId,
            trackingEnabled,
            error
          );
        }
      }
    }
  );
} 
```


# 説明

[Adjust](https://www.adjust.com/) とは、スマートフォンアプリを最大限に売るための情報収集、解析する手助けをするサービスです。

その中の1つに、広告の効果を測定するというものがあります。
例えば、どこの広告からの流入ユーザーが、どのくらいゲームを遊んでくれたのか、といった計測をします。

その情報を取得する手段として、Appleソフトウェアの開発では「IDFA (Identifier for Advertisers)」を使用します。このIDは端末固有のものです。
また、ユーザーはこのIDの提供を拒否できます。

iOS14以降、IDFAを取得する際は必ずユーザーに許可を聞く動作が必須となりました。その特定のAPIを呼ばないままIDFAを取得しようとした場合、IDFAはユーザーが拒否したときと同じものを取得するようになっています。

Unity SDK of Adjust™ の場合、`ATTrackingManager.trackingAuthorizationStatus` をラップした `com.adjust.sdk.Adjust.getAppTrackingAuthorizationStatus()` を使用し、ステータスを取得できます。

また、`ATTrackingManager.requestTrackingAuthorizationWithCompletionHandler:` をラップした `com.adjust.sdk.Adjust.requestTrackingAuthorizationWithCompletionHandler` を使用して、ユーザーに問い合わせることもできます。



- [IDFAとは？| Adjustモバイルマーケティング用語集 | Adjust](https://www.adjust.com/ja/glossary/idfa/)
- [adjust/unity_sdk: This is the Unity SDK of](https://github.com/adjust/unity_sdk)
- [ユーザーのプライバシーとデータの使用 - App Store - Apple Developer](https://developer.apple.com/jp/app-store/user-privacy-and-data-use/)
- [App Tracking Transparency | Apple Developer Documentation](https://developer.apple.com/documentation/apptrackingtransparency)
- [AdSupport | Apple Developer Documentation](https://developer.apple.com/documentation/adsupport)
