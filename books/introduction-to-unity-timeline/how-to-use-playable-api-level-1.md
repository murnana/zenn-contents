---
title: シンプルな PlayableGraph
---

シンプルな実装の流れを理解するために、「何もしない」PlayableGraphを作ってみましょう。

「何もしない」と書きましたが、実際にはPlayableGraph内にある、Playableの時間を経過させます。この動きはアニメーションを再生するのと同じです。

## PlayableGraphの生成

C#の実装では、最初に`UnityEngine.Playables.PlayableGraph` インスタンスを生成します。生成には静的関数 `UnityEngine.Playables.PlayableGraph.Create()` を呼び出します。

```csharp
var graph = PlayableGraph.Create();
```

次に、Playableを生成します。Playableの型は `UnityEngine.Playables.Playable` にします。別の型を使う時もありますが、今回は「何もしない」のでこれにします。

```csharp
var playable = Playable.Create(graph: graph, input: 0);
```

そして、PlayableOutputを作ります。
PlayableOutputを作る型にも色々ありますが、今回は `UnityEngine.Playables.ScriptPlayableOutput` を使います。

```csharp
var output = ScriptPlayableOutput.Create(graph: graph, name: null);
```

最後に、`playable` と `output` を接続します。

```csharp
output.SetSourcePlayable(value: playable, port: 0);
```

これで準備完了です。

## PlayableGraphの破棄
PlayableGraphはC#のGC[^1]と違い、自動的にメモリの解放をしません。メモリリークによるクラッシュを避けるためにも、実際に動かす前にメモリの解放を意識しましょう。

生成したPlayable、PlayableOutputは、大元であるPlayableGraphを破棄すれば同時に解放されます。

```csharp
if(graph.IsValid()){ // 変数 graph がメモリに存在しているのか
  graph.Destroy();   // graph を破棄
}
```

あえて一つずつ破棄する場合は、以下のように書きます。

```csharp
// PlayableOutputの破棄
if(output.IsOutputValid()){
  graph.DestroyOutput(output: output);
}

// Playable の破棄
if(playable.CanDestroy()){ // 破棄可能か
  playable.Destroy();
}
```


## PlayableGraphの再生・停止

メモリ解放を仕込み終えたら、動かすための仕込みをします。

<!— TODO デフォルトのPlayableGraphって再生状態だったっけ？ —>

PlayableGraph全体の再生・(一時)停止であれば、 `UnityEngine.Playables.PlayableGraph` の関数を使います。

```csharp
// 再生
graph.Play();

// 停止 (一時停止)
graph.Stop();
```

注意すべきなのは、この `Stop()` 関数は、Playable全体の動作的に「一時停止」と同じです。再び `Play()` をすれば、停止した時間から再開します。詳細は別の章に書きます。

PlayableGraphの再生・停止の状態も取得できます。

```csharp
// 再生中か
bool isPlaying = graph.IsPlaying() ;
```

PlayableGraph全体だけではなく、Playableそのものにも再生・一時停止があります。

```csharp
// 再生
playable.Play();

// 一時停止
playable.Pause();
```

Playableも再生・一時停止状態が取れます。

ただしPlayable自身の状態なので、親が停止しても子は再生状態だったり、逆の場合もあります。またPlayableGraphの状態とも関係がありません。これについては後の章で紹介します。

```csharp
UnityEngine.Playables.PlayState playState = playable.GetPlayState();

switch(playState){
  case PlayState.Play: break; // 再生中
  case PlayState.Pause: break; // 一時停止中
}
```

## Playable1個だけのPlayableGraph図

PlayableGraphは木構造を持つため、これを図で表すことができます。

この図は公式パッケージ「PlayableGraph Visualizer」 を使用すると、実際に動かしながら学ぶことができます。

残念ながらこのツール自体はデバックに向いていません。複雑なグラフになると全体図は描画されるものの、その中の一部を選択して中身を見ることが困難になります。

[^1]: Garbage Collection (ガベージコレクション) の略称。コンピューター上でメモリ解放を自動的に行う為の戦略の一つ。
