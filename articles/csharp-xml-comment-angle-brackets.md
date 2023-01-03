---
title: "[C#] XMLコメントで、「<」「>」を使うには"
emoji: "⛰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["csharp"]
published: true
published_at: 2023-01-07 19:00
---

# 対処法

MicrosoftのC#ドキュメントに、このように記述されています。

> ドキュメント コメントのテキストに山かっこを表示する場合は、`<` と `>` の HTML エンコードを使用します。これらはそれぞれ、`&lt;` と `&gt;` になります。 このエンコードは次の例に示されています。
> ```csharp
> /// <summary>
> /// This property always returns a value &lt; 1.
> /// </summary>
> ```



# 説明
C#には、クラスや関数などの説明のために XML要素のコメントを書くことがあります。
このコメントは、APIドキュメント生成に役立つほか、IDEでTips表示されたり、もちろん他のプログラマーに対して役立つ情報を乗せることができます。

このコメントは `///` で始まります。これらが書かれた行はすべてコメントになります。

![RiderでTipsが表示される様子](/images/articles/csharp-xml-comment-angle-brackets/tips-from-rider.jpg)*RiderでTipsが表示される様子*



# 参考

- [C# ドキュメント コメントとして推奨される XML タグ. (2023). Retrieved 3 January 2023, from https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/xmldoc/recommended-tags](https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/xmldoc/recommended-tags)
- [C# のXMLコメントで特殊文字を表示させる | vicのブログ. (2023). Retrieved 3 January 2023, from https://ameblo.jp/voldo55/entry-11568759236.html](https://ameblo.jp/voldo55/entry-11568759236.html)
