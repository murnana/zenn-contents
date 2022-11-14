---
title: "[Unity] GameObject.tags に \"EditorOnly\" を設定するとビルドに入らない"
emoji: "🍣"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: true
published_at: 2022-11-26 19:00
---

# 使い方

## A. Inspectorウィンドウから特定のGameObjectに設定する

1. Inspectorウィンドウに GameObjectを表示する
2. 「Tag」を「EditorOnly」に設定する


## B. C#スクリプトから特定のGameObjectに設定する

```csharp
UnityEngine.GameObject gameObject ;
gameObject.tag = "EditorOnly";
```



# 説明
Unityが編集モードの時のみに使用するGameObjectがある場合、GameObjectに「EditorOnly」タグをつけることにより、
「EditorOnly」タグのついたGameObjectとその子になっているGameObjectは、ビルド時に削除されます。



:::details 参考
<!-- textlint-disable -->
- [Unity - Scripting API: GameObject.tag](https://docs.unity3d.com/2020.3/Documentation/ScriptReference/GameObject-tag.html)
- [【Unity】ゲームオブジェクトに「EditorOnly」タグを設定してビルドに含まれないようにする - コガネブログ](https://baba-s.hatenablog.com/entry/2014/08/07/093316)
<!-- textlint-enable -->
:::
