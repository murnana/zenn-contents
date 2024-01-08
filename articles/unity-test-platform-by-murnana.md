---
title: "[Unity] プラットフォーム別テスト"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: true
published_at: 2024-01-13 19:00
---

## 概要
特定のプラットーフォームのみ動作するテストでは
`UnityEngine.TestTools.UnityPlatformAttribute` 属性を使用できます。

また`UnityEditor`名前空間が有効な時、
`UnityEditor.TestTools.RequirePlatformSupportAttribute` 属性を付与すると、
そのプラットフォームのためのモジュールがインストールされているときのみのテストができます。


## 説明
Unityで単体テストを書く時、一般的にUnity Test Frameworkが使用されます。

その中で、プラットフォームによってはテスト対象ではないものがあります。例えばiOS/Androidの通知をテストするときは、Windows/macOSのようなスタンドアロンでは不要です。

よくある手段としては、プラットフォーム固有の `#define` ディレクティブを使用し、囲うことで
ビルド時には出力しない方法です。
```cs
#if UNITY_ANDROID || UNITY_IOS
// Android, iOS固有のテストを書く
//...
#endif
```

`#define` ディレクティブで書いたほうが、
他のプラットフォームへ切り替えた際に発生するコンパイルエラーを防ぐことができます。


### `UnityEngine.TestTools.UnityPlatformAttribute`
主に PlayMode テストで使用します。

属性 `UnityEngine.TestTools.UnityPlatformAttribute` は`#define` ディレクティブと違い、アセンブリには含まれますがテスト実行時はその関数の実行をしません。テストのステータスが Skip になります。
```cs
[Test]
[UnityPlatform(RuntimePlatform.Android)]
public void SampleTest()
{
    // UnityEditor上で選択しているプラットフォームがAndroid
    // または、実行時のプラットフォームがAndroidの場合のみ実行されます
}
```


### `UnityEditor.TestTools.RequirePlatformSupportAttribute`
主に EditMode テストで使用します。

Unityの環境の作り方次第では、特定のプラットフォーム向けのモジュールを入れないことがあります。
この属性は、モジュールがインストールされていない場合にテストを実行させないためのものです。


## 参考
- [Class UnityPlatformAttribute | Test Framework | 1.3.8](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/api/UnityEngine.TestTools.UnityPlatformAttribute.html)
- [Class RequirePlatformSupportAttribute | Test Framework | 1.3.8](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/api/UnityEditor.TestTools.RequirePlatformSupportAttribute.html)
