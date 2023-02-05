---
title: "[Unity] [Adjust] iOS14以上と未満での広告ID取得時の動作の違い"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "adjust"]
published: false
---

# 広告ID (IDFA)の取得方法 (Unity)

ここでは、Unityが持つ `` と Unity SDK of Adjust™ を組み合わせた方法を紹介します。

Adjustを使わない場合は、「[iOS 14 Advertisiong Support](https://docs.unity3d.com/Packages/com.unity.ads.ios-support@1.2/manual/index.html)」パッケージを使うのが一般的です。



```cs
/// <summary>
/// IDFAの取得リクエスト (Private)
/// </summary>
public void RequestAdvertisingIdentifierAsync() {
  if (com.adjust.sdk.Adjust.getAppTrackingAuthorizationStatus() == 0)
  {
    // iOS14以降、ユーザーに問い合わせる必要がある
    // AppTrackingAuthorizationStatus == 0 は
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
