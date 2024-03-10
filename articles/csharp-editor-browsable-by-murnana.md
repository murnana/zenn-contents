---
title: "[C#] 特定のプロパティ、メソッド等を、入力候補に上げない方法"
emoji: "📇"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["csharp","visualstudio","rider"]
published: true
published_at: 2024-03-10 19:00
---

## 使い方
属性 `System.ComponentModel.EditorBrowsableAttribute` を付けます。
```cs
/// <summary>
/// インテリセンスで表示されるとき、後ろのほうになるクラス
/// </summary>
[global::System.ComponentModel.EditorBrowsableAttribute(state: global::System.ComponentModel.EditorBrowsableState.Advanced)]
public class AdvancedClass {}

/// <summary>
/// インテリセンスで表示されなくなるクラス
/// </summary>
/// <remarks>
/// コンパイル時はアクセス修飾子に従うので、本当の意味で見えなくなるわけではありません
/// </remarks>
[global::System.ComponentModel.EditorBrowsableAttribute(state: global::System.ComponentModel.EditorBrowsableState.Never)]
public class NeverClass {}
```

クラスだけではなく、構造体(`struct`)、インターフェース(`interface`)、列挙体(`enum`)、
コンストラクター、プロパティ(`get`,`set`)、メンバー変数、メソッド、
デリゲート(`delegete`)、イベント(`event`)にもつけられます。



## 説明
### インテリセンス (IntelliSense) とは
Visual Studio等のコードエディターで、入力中に候補として関連する文字列を挙げてくれる機能のことを指します。

類語として「コード補完 (Code completion)」、「コンテンツ アシスト (Content assist)」、「コードヒント (Code hinting)」があります。


### `System.ComponentModel.EditorBrowsableAttribute` とは
この属性がついたクラス等について、インテリセンスでどのように表示するのか(あるいは、非表示にするのか)を指定します。

指定方法は `System.ComponentModel.EditorBrowsableState` で定義されており、3つあります。

- `Always`: 常に参照されます。
- `Advanced`: 高度なユーザー向けに参照されます。IDEの設定次第では非表示になります。
- `Never`: 入力候補として上がりません。


### 以下筆者の気持ち
この属性は自分自身が書くためというより、他の人が書くための入力補助を行う意味が強いです。
特に、ライブラリ、フレームワークの開発者や、Unity上でAssembly Definition Files を使ってアセンブリを分けている場合に役立ちます。

使ってもいいけど、ちゃんと理解してから使って欲しいものには `EditorBrowsableAttribute(EditorBrowsableState.Advanced)`を付けます。

廃止予定のものや、構造上仕方なく `public` にしているものは `EditorBrowsableAttribute(EditorBrowsableState.Never)` を付けます。
※ 廃止予定だったら大抵 `System.ObsoleteAttribute` もつけるので、あんまり意味ないかも知れないです。
※ 構造上仕方なくつけるのは、本当は設計が良くないのです。かなしい。



## 参考
- [特定のプロパティやメソッドをインテリセンス上で非表示にする. (EditorBrowsable, EditorBrowsableState.Never, Intellisense) - いろいろ備忘録日記](https://devlights.hatenablog.com/entry/20080418/p1)
- [IntelliSense を使用してクイック ヒントと入力候補を取得する - Visual Studio (Windows) | Microsoft Learn](https://learn.microsoft.com/ja-jp/visualstudio/ide/using-intellisense?view=vs-2022)
- [Code completion (IntelliSense) | JetBrains Rider Documentation](https://www.jetbrains.com/help/rider/Auto-Completing_Code.html)
- [IntelliSense in Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense)
- [Java コンテンツ・アシスト設定 - IBM Documentation](https://www.ibm.com/docs/ja/radfws/9.7?topic=editor-content-assist)
- [コードヒントとコード補完機能](https://helpx.adobe.com/jp/dreamweaver/using/code-hints.html)
- [EditorBrowsableAttribute Class (System.ComponentModel) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.editorbrowsableattribute?view=net-8.0)
- [EditorBrowsableState Enum (System.ComponentModel) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.editorbrowsablestate?view=net-8.0)