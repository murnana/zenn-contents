---
title: "[Unity] ドロップダウンリストを追加する UIElements編"
emoji: "💧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: true
published_at: 2022-09-03 19:00
---

# 作り方

![Dropdown GUI Sample](/images/articles/unity-editor-uielements-popup/drop-down.jpg)*Dropdown GUI Sample*


```csharp
private System.Collections.Generic.Dictionary<int, string> m_DisplayValue;
private System.Collections.Generic.List<int>               m_PopupValues;

//----

m_PopupValues = new System.Collections.Generic.List<int>
{
    1,
    2,
    3,
};

m_DisplayValue = new System.Collections.Generic.Dictionary<int, string>
{
    { 1, "Hoge 1" },
    { 2, "Hoge 2" },
    { 3, "Hoge 3" },
};

//----

new UnityEditor.UIElements.PopupField<int>(
    label: "Popup",
    choices: m_PopupValues,
    defaultIndex: 0,
    formatSelectedValueCallback: value => m_DisplayValue[value],
    formatListItemCallback: value => m_DisplayValue[value]
);
```

# 説明

ドロップダウンリストを、`UIElements`で作ることができます。

実際に使用する値と、表示するための文字列とで分けて作ることができます。

階段状に、段々にもできます。

![Step GUI](/images/articles/unity-editor-uielements-popup/drop-down-steps.jpg)*Step GUI*


```csharp
private System.Collections.Generic.Dictionary<int, string> m_DisplayValue;
private System.Collections.Generic.List<int>               m_PopupValues;

//----

m_PopupValues = new System.Collections.Generic.List<int>
{
    1,
    2,
    3,

    4,
    5,
    6
};

m_DisplayValue = new System.Collections.Generic.Dictionary<int, string>
{
    { 1, "Hoge 1" },
    { 2, "Hoge 2" },
    { 3, "Hoge 3" },

    { 4, "Piya/Hoge 1" },
    { 5, "Piya/Hoge 2" },
    { 6, "Piya/Hoge 3" },
};

//----

new UnityEditor.UIElements.PopupField<int>(
    label: "Popup",
    choices: m_PopupValues,
    defaultIndex: 0,
    formatSelectedValueCallback: value => m_DisplayValue[value],
    formatListItemCallback: value => m_DisplayValue[value]
);
```

その他、従来のIMGUIでも描画可能です
@[card](https://zenn.dev/murnana/articles/unity-editor-imgui-popup)
