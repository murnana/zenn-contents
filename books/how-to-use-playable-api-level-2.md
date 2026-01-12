---
title: 複数の子を持つPlayable
---

Playableは、Playable同士で親子関係を持つことができます。この機能はアニメーションを切り替える際のモーションブレンドや、3Dモデルで表情と全身のモーション処理を分けるといった事をする時に役立ちます。

## 親一つ、子が2つのPlayableGraph

Playable同士のブレンド処理などについてはひとまず置いておき、ここではただPlayableの親子関係を作る実装をします。

親のPlayableを `parentPlayable` 、子のPlayableを `playableA`、`playableB` の2つとします。

### inputに子の数を指定して、Playableの生成

最初に `UnityEngine.Playables.PlayableGraph` インスタンスを生成します。

```csharp
var graph = PlayableGraph.Create();
```

そして親となるPlayableを作ります。引数 `input` には後で生成、接続するこの数を渡します。今回は2つ作って繋げるため、 `2` を渡します。

```csharp
var parentPlayable = Playable.Create(graph: graph, input: 2);
```

では、子のPlayableを2つ作ります。こちらは更に子に繋げるものがないため、 `input` に渡す引数は `0` です。

```csharp
var playableA = Playable.Create(graph: graph, input: 0);
var playableB = Playable.Create(graph: graph, input: 0);
```

### Playableを親子関係にする

親子関係を形成するには、 `UnityEngine.Playables.PlayableGraph` の `Connect` を使用します。

最初に `playableA` を繋げます。引数 `destinationInputPort` には、`parentPlayable` のinputポート番号を入れます。 `playableA` は1番目なので `0` を渡します。

```csharp
// playableAを子として、parentPlayableに繋げます
graph.Connect(
  source: playableA,
  sourceOutputPort: 0,
  destination: parentPlayable,
  destinationInputPort: 0
);
```

もう片方のPlayable、 `playableB`も繋げます。引数 `destinationInputPort` には、 `parentPlayable` にとって2番目の子なので、番号は `1` になります。

```csharp
// playableBを子として、parentPlayableに繋げます
graph.Connect(
  source: playableB,
  sourceOutputPort: 0,
  destination: parentPlayable,
  destinationInputPort: 1
);
```

最後に、親のPlayableを出力に繋げます。

```csharp
// PlayableOutputの生成
var output = ScriptPlayableOutput.Create(graph: graph, name: null);

// 生成したPlayableOutputに、親のPlayableを繋げる
output.SetSourcePlayable(value: parentPlayable, port: 0);
```

これでPlayableGraphが完成しました。


