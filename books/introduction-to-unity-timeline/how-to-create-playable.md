---
title: Playableの実装
---
Playable APIの概要を押さえたところで、実際にPlayable APIを

## 何もしないPlayableを組み立てる


## Playable APIで物体を移動させる
最初は単調に、ある地点まで移動するAPIを作ってみます。

### PlayableBehabiour を継承する

最初にやることは、PlayableBehabiourを継承したクラスを作ることです。

PlayableBehabiourには独自のライフサイクルがあります。Playable本体の再生、停止はもちろん、PlayableGraph本体での再生、停止など、パターンはいくつかありますが、最低限覚える関数は以下です。
- 