---
title: "[Unity] ドロップダウンリストを追加する EditorGUILayout編"
emoji: "💧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: true
published_at: 2022-09-03 19:00
---


# 作り方

![Dropdown GUI Sample](/images/articles/unity-editor-imgui-popup/drop-down.gif)*Dropdown GUI Sample*


```csharp
private UnityEngine.GUIContent[] m_PopupDisplayOptions;
        private int                      m_PopupIndex;
//----

m_PopupDisplayOptions = new[]
{
    new UnityEngine.GUIContent("Hoge 1"),
    new UnityEngine.GUIContent("Hoge 2"),
    new UnityEngine.GUIContent("Hoge 3"),
};

//----

m_PopupIndex = UnityEditor.EditorGUILayout.Popup(
    label: new UnityEngine.GUIContent("Popup"),
    selectedIndex: m_PopupIndex,
    displayedOptions: m_PopupDisplayOptions
);
```



# 説明

`UnityEditor.EditorGUILayout.Popup`を使用して、InspectorウィンドウやEditorウィンドウで描画できます。

インデックスは `int` 型のインデックスで管理します。

表示項目は `string[]` でも、`GUIContent[]`でもできます。

また選択肢を階段状に表現できます。

![Step GUI](/images/articles/unity-editor-imgui-popup/drop-down-in-steps.jpg)*Step GUI*



```csharp
private UnityEngine.GUIContent[] m_PopupDisplayOptions;
private int                      m_PopupIndex;
//----

m_PopupDisplayOptions = new[]
{
    new UnityEngine.GUIContent("Hoge 1"),
    new UnityEngine.GUIContent("Hoge 2"),
    new UnityEngine.GUIContent("Hoge 3"),
    new UnityEngine.GUIContent("Piya/Hoge 1"),
    new UnityEngine.GUIContent("Piya/Hoge 2"),
    new UnityEngine.GUIContent("Piya/Hoge 3"),
    new UnityEngine.GUIContent("Piya/Hoge/Hoge 1"),
    new UnityEngine.GUIContent("Piya/Hoge/Hoge 2"),
    new UnityEngine.GUIContent("Piya/Hoge/Hoge 3"),
};

//----

m_PopupIndex = UnityEditor.EditorGUILayout.Popup(
    label: new UnityEngine.GUIContent("Popup"),
    selectedIndex: m_PopupIndex,
    displayedOptions: m_PopupDisplayOptions
);
```

その他、UIToolsでも描画可能です。
@[card](https://zenn.dev/murnana/articles/unity-editor-uielements-popup)
