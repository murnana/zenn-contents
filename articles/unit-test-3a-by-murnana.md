---
title: "3A – Arrange, Act, Assert とは"
emoji: "ℹ️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["test"]
published: false
---

Bill Wakeさんが提唱する、単体テストに適した3つのパターンです。

- Arrange _- 準備_
- Act _- 実行_
- Assert _- アサート_


## 説明
オブジェクトの動作をテストする良い方法の1つは、オブジェクトをそれぞれ「興味深い」構成にし、その状態で様々なアクションを試すことです。

### Arrange - 準備
テストするオブジェクトのセットアップをします。

他にも必要なオブジェクトがある場合もあります(コラボレーターといいます)。
これらは、テストオブジェクト (モック、フェイクなど)の場合もあれば、本物の場合もあります。


### Act - 実行
何らかのミューテーターを通じて、オブジェクトに作用します。
パラメーター(おそらく、テストオブジェクト)を指定する必要がある場合もあります。


### Assert - アサート
オブジェクト、コラボレーター、パラメーター、(稀に)グローバルな状態についての正当性を実証します。



## 参考
<!-- textlint-disable -->
- Bill Wake."3A - Arrange, Act, Assert - XP123".XP123.2011-04-26,[https://xp123.com/articles/3a-arrange-act-assert/](https://xp123.com/articles/3a-arrange-act-assert/).
- Kent Beck.テスト駆動開発.和田卓人訳.株式会社オーム社,2017,320p
<!-- textlint-enable -->
