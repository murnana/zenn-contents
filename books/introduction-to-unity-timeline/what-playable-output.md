---
title: Playable Outputとは
---
### Playable Output とは
PlayableだけでPlayableGraphを生成しても、そのままでは動作しません。Playable達が動くには、出力先が必ず必要です。

その出力先が PlayableOutput になります。

Playable Outputは、1つのPlayableGraph に対して複数個持つことができます。一方で、Playable Outputと接続するもの…よくあるのは、コンポーネント…とは、1対1の関係です。