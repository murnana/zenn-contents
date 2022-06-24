---
title: "bashで数字かどうかを判断する"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---


bashなど、シェルスクリプトを普段書かないので…他の方が書いた記事を参考にしています…
つまりただの二番煎じです。
https://rcmdnk.com/blog/2018/09/05/computer-bash-zsh/


## 個人的に直感的と思った方法: `[[ “$x” =~ ^[0-9]+$ ]]`

```bash
if [[ "$1" =~ ^[0-9]+$ ]]; then
  echo 'is number'
else
  echo 'is not number'
fi
```

どこかで見たことある書き方(正規表現ですね)なので安心する…
小数点が欲しいという人には別の書き方もあります。

```bash
if [[ "$1" =~ ^-?[0-9]+\.?[0-9]*$ ]]; then
  echo 'is a number'
else
  echo 'is not a number'
fi
```

~~正規表現はこだわると魔術になりがちだったりしますけどね~~



## メジャーな方法: `expr $x + 1`

そもそも `expr` というコマンドについてから。

調べたところ、`expr` コマンドは式を評価するコマンドだそうです。
https://atmarkit.itmedia.co.jp/ait/articles/1712/28/news019.html


気を付けるべきなのは、計算するコマンドではないので
計算式で0が返ってくる場合、計算結果が偽となり0以外が返ってくるとのことです。
https://qiita.com/kugyu10/items/41164809d2580cfb2d92

つまり、`expr $x + 1` という書き方が許される範囲は _0 <= x <= ???、ただしxは整数_ ということのようです
(何桁まで許されるのかはしらない…)
