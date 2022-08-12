---
title: "bashで整数かどうかを判断する"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["bash"]
published: true
---

a
# 方法

```bash
if [[ "$1" =~ ^[0-9]+$ ]]; then
  echo 'is number'
else
  echo 'is not number'
fi
```


# 説明

`$1` はbashスクリプトを実行する際に渡してきた、1番目の引数を指します。

`=~` は右辺を正規表現として解釈するための書き方です。

`^[0-9]+$` は正規表現になります。`^`は文字列の一番最初を指します。`[0-9]`は文字列で0~9という数字の文字を指します。`+`はその直前にさした文字(つまり0~9のどれか)の1回以上の繰り返しです。`$` は文字列の末尾を指します。

`if [[ ]]; then` はbashの`if`文と`[[`構文を表しています。`[[`構文は条件判断式と呼ばれ、`]]`で閉じることができます。この内部は通常のテスト構文だけではなく、正規表現の文字も扱うことができます。


# 参考

<!-- textlint-disable -->
- [シェルスクリプトで数字かどうか判断する方法(exprだけじゃない](https://rcmdnk.com/blog/2018/09/05/computer-bash-zsh/)
- [とほほのBash入門 - とほほのWWW入門](https://www.tohoho-web.com/ex/shell.html)
<!-- textlint-enable -->
