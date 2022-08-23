---
title: "[C#] 継承の書き方"
emoji: "🔰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["csharp"]
published: false
---

# 継承を表す書き方
```csharp
/// <summary>
/// 基底クラス
/// </summary>
class BaseClass {}

/// <summary>
/// 派生クラス
/// </summary>
class DerivedClass : BaseClass {}
```



# 説明

プログラミングにおいて「継承」は、クラス同士の関係性を表します。この関係には方向性があり、継承元と継承先、という方向をそれぞれ持ちます。例えば「元となるクラス`BaseClass`」と「`BaseClass`を継承したクラス`DerivedClass`」というのがあった場合、継承元が`BaseClass`、継承先が`DerivedClass`となります。

継承元となるクラスは「基底クラス(base class)」と呼ばれます。別名「親クラス(parent class)」、「スーパークラス(super class)」とも呼ばれるためしばしば混乱をもたらしますが、**今回は「基底クラス」という用語を用います。**

継承先となるクラスは「派生クラス(derived class)」と呼ばれます。別名「子クラス(child class)」、「サブクラス(sub class)」、「拡張クラス(extended class)」とも呼ばれるため、基底クラス同様しばしば混乱をもたらしますが、**今回は「派生クラス」という用語を用います。**

**C#では、継承を行えるのは`class`(クラス)のみです。** 基底クラスとして`struct`(構造体)は使用できません。基底クラスとして`class`を用いたとしても、派生クラスとして`struct`は使用できません。基底クラス、派生クラスともに`struct`を使用できません。

C#での継承は、基底クラスにあるメンバー変数、メンバー関数の実装を引き継ぐことができます。基底クラスのメンバー変数やメンバー関数は「アクセシビリティ」次第で派生クラスでも使用できます。

似たような書き方に`interface`があります。「継承」と同じような書き方行うことができますが、クラス同士の継承とは違い実装が伴いません。


# 参考サイト/文献

- Microsoft.“C# での継承 | Microsoft Docs”.Microsoft Docs.https://docs.microsoft.com/ja-jp/dotnet/csharp/fundamentals/tutorials/inheritance, (参照 2022-08-24)
- Nobuyuki Iwanaga."継承 - C# によるプログラミング入門 | ++C++; // 未確認飛行 C".++C++;.https://ufcpp.net/study/csharp/oo_inherit.html, (参照 2022-08-24)
