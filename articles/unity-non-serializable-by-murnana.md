---
title: "[Unity] NonSerializable属性について"
emoji: "🎃"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: false
---

## 使い方
Unityエディターがスクリプトの再読み込みを行う際、値を復元して欲しくない場合に使用します。

//TODO: NonSerializable がついたフィールドを持った ScriptableObject vs
// ただの private フィールドを持った ScriptableObject が AssetBundleになった時のサイズ


## 説明
Unityは、メンバー変数をシリアル化できます。この機能はスクリプトにあるフィールドの値や、マテリアルにあるプロパティ、アニメーションクリップのカーブ等、様々なものがあります。

Unityは通常、プロパティをシリアル化しませんが、Unityエディターがホットリロードを行った際のみシリアル化します。


### Unityにおけるシリアル化
ホットリロードでの挙動の前に、通常のUnityでのシリアル化について説明します。

Unityのシリアライザーは、フィールドに対して動作します。これにより、例えばシーンを読み込んだ際、`Image`コンポーネントが持つフィールドを復元し、画像と色を設定します。

シリアル化の条件は以下の通りです。
- `public` フィールドであること、または `global::UnityEngine.SerializeField` 属性のついたフィールドであること
- フィールドタイプが以下のものであること
  - プリミティブデータ型 (`int`, `float`, `bool`, `string` など)
  - Enum型 (32バイト以下)
  - 固定長サイズのバッファ
  - `global::System.SerializableAttribute` 属性を持つ構造体またはクラス
  - `global::UnityEngine.Object` から派生したクラス
  - 上記のフィード型の1次元配列 (`T[]`)
  - 上記のフィード型の1次元リスト (`System.Collections.Generic.List<T>`)

以下はシリアル化できません。
- 定数 (`const`)
- 読み取り専用がついたもの (`readonly`)
- 静的変数 (`static`)
- null 許容値型 (`global::System.Nullable<T>`)
- 多次元配列 (`T[,]`)
- ジャグ配列 (`T[][]`)
- 辞書型 (`System.Collections.Generic.Dictionary<TKey,TValue>`)


### Unityのホットリロード
Unityエディターが、スクリプトの作成、編集等による変更を、Unityの再起動なしで反映することを指します。

ホットリロードでは、現在読み込まれているすべてのスクリプトの変数をシリアル化して保存した後、スクリプトを再度読み込み、読み込みが終わると、前回シリアル化したものを元に戻します。

このホットリロードでは、通常のUnityのシリアル化とは別の条件が使われます。今までのシリアル化可能な変数のほかに、プロパティ、`SerializeField`が **ついていない** プライベート変数も含まれます。


## 参考
- [Unity - Manual: Script serialization](https://docs.unity3d.com/2021.2/Documentation/Manual/script-Serialization.html)
