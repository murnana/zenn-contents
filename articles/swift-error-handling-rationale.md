---
title: "[Swift] エラーの分類について"
emoji: "👮"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["error", "swift"]
published: true
---

:::message
プログラミング言語の設計上、Swiftではエラー処理のためにどのような考えで設計している (あるいはした) のかが書かれている「[Error Handling Rationale and Proposal](https://github.com/apple/swift/blob/main/docs/ErrorHandlingRationale.rst)」から、自分なりに解釈したレポートです。
:::


# 説明
プログラムによって発生するエラーは様々にありますが、プログラマーがエラーに対しどのように反応させるのかによっていくつかに分類できます。

Swiftではエラーを明確にするため、4種類のエラーを提案しています。

- Simple domain errors (単純なドメインエラー)
- Recoverable errors (回復可能なエラー)
- Universal errors (普遍的なエラー)
- Logic failures (論理的な失敗)


## Simple domain errors
`String.ToInt()`のように、「間違った状態」だけを返却します。

Swiftでは「Optional」として戻り値を定義します。つまり正しい値か、`nil`が返ってくる状態にします。



## Recoverable errors
ファイルが見つからない、ネットワークタイムアウトなど、ユーザーの操作によって復旧可能なエラーを指します。

例えばネットワークエラーの場合、以下の原因が考えられます。

- オフライン状態である
- 寸断が発生した
- クライアントで投げたAPIの値が不正
- サーバーがビジー状態である
- サーバーがハードウェアの故障を起こしている

などがありますが、そのうちのいくつかはクライアント側で解決できるものもあれば、サーバーの回復を待つ必要があるときもあります。

多くは現在の処理を中止、クリーンアップを行い、ユーザーに報告する動作で回復をはかります。エラー処理で考える場合、エラーが発生した地点から元の呼び出し場所まで、エラーを伝搬させます。



## Universal errors
実行時に引き起こされる、強制終了をさせるエラーです。

例えば以下のようなエラーが該当します。

- プロセスが「SIGINT」を受信して強制終了
- スレッドの強制終了
- メモリ不足によるエラー
- スタックオーバーフロー


プログラミングでは通常、オブジェクトへのプロパティをアクセスしたときにエラーが発生することは考えられません。しかし例えばそのプロパティはメモリへのアクセスではなく、データベースへのアクセスの場合は、クエリが失敗してエラーが発生する、という可能性が出てきます。

こういった分かりにくいエラーもUniversal errorsに該当します。これは避けるべきですが、言語の仕様によってエラーを見つけ、解消することは難しいです。



## Logic failures
プログラミングのミスによって発生するエラーです。

- 配列へのアクセス時に範囲外のインデクスを指定した
- Optional型で実際には`nil`が入っているのに、強制的に正しい型でアクセスしようとした

こういったものは通常、コードを修正することによって解消します。



# 参考
- "Swiftのエラー4分類が素晴らしすぎるのでみんなに知ってほしい - Qiita". Qiita.Com, 2022, https://qiita.com/koher/items/a7a12e7e18d2bb7d8c77. Accessed 21 Sep 2022.
- "apple. swift". Github.Com, 2022, https://github.com/apple/swift/blob/main/docs/ErrorHandlingRationale.rst#kinds-of-error. Accessed 21 Sep 2022.
