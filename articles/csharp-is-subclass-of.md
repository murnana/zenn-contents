---
title: "[C#] BがAを継承しているのか(型変換が可能か)を判定する"
emoji: "🤨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["csharp"]
published: true
---

# 型のテスト演算子`is`を用いる
```csharp
var b = new B();
var bIsA = b is A;
if (bIsA) {
  // 変数bはAから継承した
  // 変数bはインターフェースAを持つ
  // 変数bはAへの暗黙的な参照変換を持つ
  // 変数bはnullではない
}
```
[sharplab.io](https://sharplab.io/#v2:C4LgTgrgdgNAJiA1AHwAICYCMBYAUH1AZgAINiBBYgbwF88CSyAhYkC6u/Lo09UzAOzU8xUaUaYAbKQAsxALIBDAJZRiACgCUw3GL3EAborDEARsQC8xKAFMA7sSZaA3CP2ijJ0wEkAzpStzZV8KV113YmUAMw0ff20qNwjiAHoU4kBITUAHU1NAewZyQGkGQEiGQD5bQHxXQHUGQH0GJIi0zJzcwBKGQGeGQH6GQB+GQFWGQHKGDsBOhnJAJIZAQGNAEwZa93rsvPJADwZAOwZAdLNATbzAELdAKwZAIeVAc0cMwG3jUYnw5OnGqAgAG0vAcwZcjcARBkn9AGEAeyhfN8ubADoAdTAymANnUACIqAAGGiREJUTA0MEwayKAC2NjeUXUpk0yOAAE8AA4YrHkTSaMLJTh6Tg0IA==)


`is`演算子は左辺が右辺との間に互換性があるかを調べます。調べた結果は真偽値(`bool`)で返却されます。互換性がある場合は`true`です。

:::details `if(b is A a)` という書き方
C#7.0以降、`is`演算子は宣言パターンに対して照合できます。宣言パターンを使用すると、今まで変数を宣言してキャスト、または`as`演算子を用いてnullチェックを行う、といった書き方とは別の書き方ができます。
```csharp
var b = new B();
if (b is A a) {
  // if文内で変数aが、クラスまたはインターフェースAとして振舞う
  a.RunOnlyA();
} else {
  // if文内で変数aが、クラスまたはインターフェースAとして振舞えない
}
```
```csharp
var b = new B();
if (!(b is A a)) {
  // if文内で変数aが、クラスまたはインターフェースAとして振舞えない
} else {
  a.RunOnlyA();
}

```
:::

# `System.Type` クラスの関数を使う

:::details `System.Type.IsSubclassOf`を用いる
```csharp
System.Type aType = typeof(A);
System.Type bType = typeof(B);
if (bType.IsSubclassOf(aType)) {
  // タイプbはAから継承した
}
```
[sharplab.io](https://sharplab.io/#v2:C4LgTgrgdgNAJiA1AHwAICYCMBYAUH1AZgAINiBBYgbwF88CSyAhYkC6u/Lo09UzAOzU8xUaUaYAbKQAsxALIBDAJZRiACgCUw3GL38AdABUAngAcApsUWnLxALzFg5iwHsAZuvKaA3CP2iqJjGLsQARrZWjs6WHupMvv4Byu4aES4GAJIAzgDKEGEYAPKeNi6a2lRJAcQA9LXEgP0MgCUMgOsMYYD2DOSA0gyAkQyAfLaA+K6A6gyA+gzVNcQAwq5Q2a4ANhYGAOpgysAW6gBEVAAMNMTK2cTZBWQe1Jg02zDhkXdllom6NZx6nDRAA)


`System.Type.IsSubclassOf` は `System.Type`を引数に持つ関数です。
引数で渡されたタイプが継承元であるかを返却します。

インターフェースを実装しているのかは判断できません。
:::



:::details `System.Type.IsAssignableTo`を用いる
```csharp
System.Type aType = typeof(A);
System.Type bType = typeof(B);
if (bType.IsAssignableTo(aType)) {
  // タイプBはAから継承した (あるいは間接的に継承した)
  // タイプBはAというインターフェースを実装している
  System.Console.Write("{0} is assignable to {1}", bType, aType);
}
```
[sharplab.io](https://sharplab.io/#v2:C4LgTgrgdgNAJiA1AHwAIGYAEqBMmCCAwgDYCGAzuZgN4C+AsAFAbZ4BCJFVIBnlNDRkxYBLKMACmYAGakAxhIIBJcVNkKBwrLkwcylaWAD2AWxWSZ8xT3zm1VzcyzlgkOcF0BlVxHcAxYzNVSw0bOxDFOiZop1ZsAEYAdhomTDTsbXiANmwAFkwAWVIxTAAKAEoUxnSahIA6ABUATwAHRVJmtswAXkxgVokjaVKifXJygG5U2rTUeMaBzAAjTsVe/rah0r0uSemZkWkylYG6pXJ8ShEAcyhSJeIJBqNSjoHyyup9mfSAel/MIB+hkAJQyAdYY2IB7BnwgGkGQCRDIBM30A/kaAdQZAPoMZUAQgyAaIZACIMEMAyamAUuNACFugGsGJFo8rfH6Yf5AsGQ/CACwYcYAxBmBgGeGQGAH4ZAKsMgHKGbmAToZAEkMgHztQCjEcjAGYMOKx1J+czqcwAnKUAETUAAMtEwIioXBudweimARho8Vo6pgy1WNrebT21R+ghpCrS/wAtN6fb6/X73fVVRUps6ZoH3YcyhtBsMdgZAuF1BJymcLldbvdHs9SjGtrZgsmPp9AzUlcHNTq9QaM8bHn1zdRLda+gMtvHyIZTEmrOUbXnhgWLEWnS7I0dc224943MAAt3C7205dyEas08XgORj2FMWqjSy/MK9rdfrMIbMyaGxarf2p9sZ7454nF7u75tBzuU6OZq70oJaCAA)


`System.Type.IsAssignableTo` は、引数で渡された型 **に** 代入可能かを真偽値で返却します。
そのため、継承元や間接的に継承しているクラスや、インターフェースであっても代入可能であれば`true`を返却します。
:::



:::details `System.Type.IsAssignableFrom` を用いる
```csharp
System.Type aType = typeof(AClass);
System.Type bType = typeof(BClass);
if (aType.IsAssignableFrom(bType)) {
  // タイプBはAから継承した (あるいは間接的に継承した)
  // タイプBはAというインターフェースを実装している
}
```
[sharplab.io](https://sharplab.io/#v2:C4LgTgrgdgNAJiA1AHwAIGYAEqBMmCCAwgDYCGAzuZgN4C+AsAFAbZ4BCJFVIBnlNDRkxa5sARgDsNJplnYsqMQDZsAFkwBZUgEsomABQBKaYzlnxAOgAqATwAOAU0ylbjzAF5Mwew4D2AM30iMkpDAG4Zc1lFax9MACNXJ09vRwD9DhDycMio7X8DFx8LAElyfEptAHMoUnjiBwAxMF8AW31En0NjalyouQB6AcxAfoZAEoZAdYY2QHsGfEBpBkBIhkBM30B/I0B1BkB9BgNAIQZAaIZAEQZpwGTUwFLjQBC3QGsGVc3DPv7MIdHJmfxACwZ9wDEGMcBnhhHAH4ZAKsMgHKGP6AToZAEkMgHztQCjEWtAGYM+12d36MUUAE59AAiagABlomG0VC41Vq9Sc/harRoYlomJgziS9M6jhypn6gjMgloQA=)


`System.Type.IsAssignableFrom` は、丁度 `System.Type.IsAssignableTo`と逆の動きをします。つまり引数に渡された型 **が** 代入可能かを真偽値で返却します。
:::



:::details `System.Type.IsInstanceOfType` を用いる
```csharp
var b = new BClass();
System.Type aType = typeof(AClass);
if (aType.IsInstanceOfType(b)) {
  // 変数bはAから継承した (あるいは間接的に継承した)
  // 変数bはAというインターフェースを実装している
  // 変数bがnullである
}
```
[sharplab.io](https://sharplab.io/#v2:C4LgTgrgdgNAJiA1AHwAIGYAEqBMmCCAwgDYCGAzuZgN4C+AsAFAbZ4BCJFVIBnlNDRkxa5sARgDsNJplnYsqMQDZsAFkwBZUgEsomABQBKaYzlnMHMvwBGmALyYoAUwDuFvuSMBuGedmKAOgAVAE8ABydMUlCI+0xgcKcAewAzfSIrckMfUz9MbRSDaMSAgElyUqhyYFIoAGMnAHkUmKd9a0Njal88uQB6PsxASE1AB1NrQHsGfEBpBkBIhkBM30B/I0B1BkB9BgNAIQZAaIZAEQZxwGTUwFLjQBC3QGsGRdXDHt7MAeGxycALBm3AMQZAEoZAZ4ZAfoZAH4ZAVYZAcoZvoBOhkASQyAfO1AKMRS0AZgzbTZXXq3UbWQAyDFAIMRiIBzBi2CLygUUAE59AAiagABlo+SoumqtQamFSNDEtBJMEcpAAtsk0h12cUItk8bJBGZBLQgA==)

`System.Type.IsInstanceOfType` は引数で渡された変数 `b` が継承可能であったり、インターフェースが実装されている場合にtrueを返却します。
:::



# 説明
C#では、型変換したい変数が、指定された型に型変換できなかった場合の対策として `is` 演算子を用いることができます。
`if` 文と組み合わせることによって、変換できた場合と変換できなかった場合という書き方ができます。

一方で、`System.Type`には引数で渡された型(または変数)がどういう継承関係にあるのか、(あるいはインターフェースが実装されたのか)を判定する方法があります。
`System.Type`だけを扱いたい場合はこちらの方法を使います。

:::details 多くの場合 `is` 演算子の判定で十分です
`is` 演算子出の書き方と、`System.Type.IsInstanceOfType` での書き方を比較すると、
`is`演算子のほうが英語の文章に近い書き方なので、読みやすいです。
```csharp:is演算子
var b = new B();
if (b is A) {
  // ...
}
```
```csharp:System.Type.IsInstanceOfType
var b = new B();
if (typeof(A).IsInstanceOfType(b)) {
  // ...
}
```

@[card](https://sharplab.io/#v2:C4LglgNgNAJiDUAfAAgZgATIEzoILoG8BfAWACg1McAhdEPQ0s8y7TARgHZDz0/MMydgDZMAFnQBZAIZgAdugAUASh5l+G9ADdpAJ3QAjdAF50cgKYB3dNRUBucuQDEYAGbpgugK7nemvgD0AehgAM6AKPaA6d6ACtp+/m5KRmF4qgRx/vxB6AB0uekaTE7mEKG+6hlZQtkAKgCeAA7m2QCSoc1yocDScgDG5gDyrnWN+fwJisAN5gD2roq4yi1tHV29A0NTigbKqaP+WbnZe+iF5nIwbulMREA=)



:::


# 参考
- [型のテスト演算子とキャスト式 - C# リファレンス | Microsoft Docs](https://docs.microsoft.com/ja-jp/dotnet/csharp/language-reference/operators/type-testing-and-cast)
- [パターン - C# リファレンス | Microsoft Docs](https://docs.microsoft.com/ja-jp/dotnet/csharp/language-reference/operators/patterns#declaration-and-type-patterns)
- [T型が任意のクラスを継承しているか判定するには？](https://teratail.com/questions/52851) - teratail
- [Type.IsSubclassOf(Type) Method (System) | Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/api/System.Type.IsSubclassOf?view=net-6.0)
- [Type.IsAssignableTo(Type) メソッド (System) | Microsoft Docs](https://docs.microsoft.com/ja-jp/dotnet/api/system.type.isassignableto?view=net-6.0)
- [Type.IsAssignableFrom(Type) メソッド (System) | Microsoft Docs](https://docs.microsoft.com/ja-jp/dotnet/api/system.type.isassignablefrom?view=net-6.0)
- [Type.IsInstanceOfType(Object) メソッド (System) | Microsoft Docs](https://docs.microsoft.com/ja-jp/dotnet/api/system.type.isinstanceoftype?view=net-6.0)
