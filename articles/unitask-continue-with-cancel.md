---
title: "[UniTask] キャンセルされても処理を続ける"
emoji: "🦁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "unitask"]
published: true
published_at: 2022-10-03 19:00
---

# 実装

`UniTask.SuppressCancellationThrow` をキャンセルする可能性のあるUniTaskの後につけます。

```csharp
public UniTask SomeMethods(){...};

SomeMethods().SuppressCancellationThrow()
  .ContinueWith(isCanceled => {
    if (isCanceled) {
        // キャンセルされたとき
    } else {
        // キャンセルされていないとき
    }
  })
  .Forget();
```



# 説明

UniTaskは、Unity上でasync/awaitをゼロアロケーションで書くことが可能になるオープンソースです。

async/awaitで書かれた関数は、 `System.Threading.CancelletonToken` など、 `System.OperationCanceledException` という例外で処理を中断できます。

中断すると、通常は中断した処理より後にある関数は実行されません。それはasync/awaitで書かれた関数内の処理だけではなく、 `UniTask.ContinueWith` のように後続するaync/awaitの処理についても同様です。

中断がかかっても処理を続けたい場合、try/catch/finally文を使用することで処理を続けることができます。ただし一般的に、try/catch文の処理は重いです。

UniTaskではtry/catch以外の方法として `UniTask.SuppressCancellationThrow` を使用できます。これは通常の例外のない処理の他に、 `System.OperationCanceledException` が発生した時も処理を実行できます。

中断したのかは、第一引数の真偽値で返却されます。



# 参考
- "Haruma:K (id:halya_11). 【Unity】UniTaskのキャンセル処理まとめ・Taskとの比較 - LIGHT11". Light11.Hatenadiary.Com, 2022, https://light11.hatenadiary.com/entry/2021/04/27/200610#SuppressCancellationThrow%E3%81%A7%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AB%E7%8A%B6%E6%85%8B%E3%82%92%E6%88%BB%E3%82%8A%E5%80%A4%E3%81%A8%E3%81%97%E3%81%A6%E5%BE%97%E3%82%8B. Accessed 2 Oct 2022.
- https://github.com/Cysharp/UniTask