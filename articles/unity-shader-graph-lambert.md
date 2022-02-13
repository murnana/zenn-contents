---
title: "Unity ShaderGraphでランバート反射を描く"
emoji: "🔖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ "unity", "ShaderGraph" ]
published: false
---

# 概要

Unityでは、シェーダーをノードベースで作るためのツール「Shader Graph」があります。
https://unity.com/ja/features/shader-graph


## ランバート反射 / Lambertian reflectance とは

![](/images/articles/unity-shader-graph-lambert/Lambertian.jpg)
*Shader Graph上のランバート反射*
光が物体に当たった時、カメラや目には光が当たって物体の色が見える部分と、陰となって暗くなっている部分とに分かれます。
これは **拡散反射**、または **乱反射** (**Diffuse reflection**) と呼ばれます。
カタカナだと、ディフューズと書きますね。モデルをいじっている人はみたことあるのではないでしょうか？

拡散反射はその名の通り、光さえ当たっていればどのような角度から見ても同じように見えます。

ランバート反射はこの拡散反射を表現するためのアルゴリズムの一つになります。


## シェーダーグラフ / Shader Graph とは

[Unity 2018.1 から導入された機能](https://blog.unity.com/ja/technology/introduction-to-shader-graph-build-your-shaders-with-a-visual-editor)です。
それまで、シェーダーといえばCg/HLSL言語で書くものでしたが、この機能はCg/HLSL言語ではなく、ノードベースでGUI上に表現され、線を繋げる形にすることで、シェーダー言語を書かずともシェーダーをつくることができます。

プログラマーからすれば、まだかゆいところに手が届いていない[^1]代物なので実務で使うには難しいのですが、ノードの途中でどういう結果になっているのかがわかるので、シェーダーを学んだり、アーティストが試しに絵をつくってみたいときには手が届きやすいです。

…とはいえ今回、ライトを使用する必要があるのでHLSLを書くことになります。
(ライトのノードがないためです)


## 環境

この記事を書く時点での、私の環境は以下になっています。
最近になってビルドインシェーダーに対応[^2]しましたが、この記事ではUniversal Render Pipeline環境下で作ります。

| 名称         | バージョン                                                                                              |
| :----------- | :------------------------------------------------------------------------------------------------------ |
| OS           | Windows 10 Home 19043.1526                                                                              |
| プロセッサ   | Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz   3.60 GHz                                                      |
| RAM          | 16.0 GB                                                                                                 |
| GPU          | NVIDIA GeForce GTX 1060 3GB                                                                             |
| Unity        | [2020.3.25f1](https://unity3d.com/jp/unity/whats-new/2020.3.25)                                         |
| Universal RP | [10.8.1](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@10.8/manual/index.html) |
| Shader Graph | [10.8.1](https://docs.unity3d.com/Packages/com.unity.shadergraph@10.8/manual/First-Shader-Graph.html)   |




# Shader Graphを書いてみよう

メニューからAssets > Create > Shader > Universal Render Pipeline > Unlit Shader Graph の順で、空っぽのグラフを作成します。
![](/images/articles/unity-shader-graph-lambert/create-unlit-shader-graph.png)
*Assets > Create > Shader > Universal Render Pipeline > Unlit Shader Graph*


灰色しか出ないシェーダーができました。
![](/images/articles/unity-shader-graph-lambert/empty-unlit-graph.jpg)
*灰色が出るShader*


## ランバート反射に必要な要素をShader Graphに配置する

ここに、ランバート反射で必要なものを先に置きます。
ランバート反射の計算自体には、以下の要素が必要です:

- 法線ベクトル
- ライトの向き

さらに、色を追加する場合は以下の要素が加わります

- ベースカラー
- ライトカラー

残念ながら、ライト情報ををShader Graph上に持ってくる方法が、Custom Functionを作ることしかありません。
また追加ライトに対応する場合、`for`文による繰り返市処理が必要なため、大半はHLSLで書かなくてはなりません。

では、ノードを用意しましょう。

### プロパティにベースカラーを追加
Propertiesに、Colorの型で「BaseColor」という名前のプロパティを作ります。
これはベースカラーになります。

1. Shader Graphが写っているウィンドウの、「Blackboard」を表示(表示していなければ)
2. 「＋」ボタンから「Color」を選択
3. 名前を「BaseColor」と入力する

![](/images/articles/unity-shader-graph-lambert/create-base-color.gif)
*BaseColorプロパティの生成*

その後、ドラッグ & ドロップでGraph状に載せます。



### 法線ベクトルの追加
今回はメッシュに含まれている法線を使います。

1. グラフを描画するところで右クリック
2. 「Create Node」をクリック
3. 検索窓に「Normal」と打つ
4. 検索で出てきた「Normal Vector」(Input > Geometryの中にあります) をクリック

![](/images/articles/unity-shader-graph-lambert/add-normal-vector.gif)
*Normal Vectorの追加 カラフルじゃないのはgifサイズを削ったためです*



### Custom Functionの追加
HLSLが書かれたテキストファイルをShader Graphで読み込むため、
Custom Functionノードを追加します。追加方法は法線ベクトルと同じです。

1. グラフを描画するところで右クリック
2. 「Create Node」をクリック
3. 検索窓に「Custom」と打つ
4. 検索で出てきた「Custom Function」(Utilityの中にあります) をクリック


これで、一通り追加しました。

![](/images/articles/unity-shader-graph-lambert/end-create-node.gif)
*作成したノードとプロパティ*



## Custom Functionのカスタマイズ
HLSLを書いていくのですが、その前に「Custom Function」ノードのInput, Outputを設定していきます。

1. Graph Inspectorが表示されていない場合は、右上の「Graph Inspector」を選択します<br />![](/images/articles/unity-shader-graph-lambert/select-graph-inspector.jpg) *Graph Inspectorの表示/非表示ボタン*
2. ビュー上にある「Custom Function」を選択します<br />選択している状態だと、Graph InspectorにCustom Functionを設定する項目が出てきます。

### Inputs項目の追加
1. Inputsの「+」ボタンを1回押して、Inputsに1つ追加します
2. 追加した項目の「New」をクリックし、「New」を「BaseColor」に変更します
3. 「BaseColor」の横の「Float」と書かれたプルダウンを選択します
4. プルダウンから「Vector4」を選択します
5. もう一度、Inputsの「+」ボタンを1回押して、Inputsに1つ追加します
6. 追加した項目の名前を「Normal」に変更します
7. 「Normal」の横の「Float」と書かれたプルダウンを選択します
8. プルダウンから「Vector3」を選択します


### Outputs項目の追加
1. Outputsの「+」ボタンを1回押して、Outputsに1つ追加します
2. 追加した項目の「New」をクリックし、「New」を「Color」に変更します
3. 「Color」の横の「Float」と書かれたプルダウンを選択します
4. プルダウンから「Vector4」を選択します


![](/images/articles/unity-shader-graph-lambert/edit-custom-input-output.gif)
*Custom FunctionノードにInput, Outputを設定*


[^1]: マルチパスやZTestの指定、そして頂点シェーダーとフラグメントシェーダーの境界があいまいなので最適化にも不向きです
[^2]: Shader Graph 12.0.0、Unity 2021.2から
