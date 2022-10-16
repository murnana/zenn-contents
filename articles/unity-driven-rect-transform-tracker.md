---
title: "[Unity] uGUIのRectTransformを操作すならDrivenRectTransformTrackerも忘れずに"
emoji: "😽"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: true
---

UnityEngine.`DrivenRectTransformTracker`

[DrivenRectTransformTracker](https://docs.unity3d.com/ScriptReference/DrivenRectTransformTracker.html)



# 使用方法

`UnityEngine.RectTransform`. anchoredPosition を操作するとします。

操作したら、`UnityEngine.DrivenRectTransformTracker.Add()` を呼び出します。

```csharp
UnityEngine.DrivenRectTransformTracker m_Tracker = new UnityEngine.DrivenRectTransformTracker();  // メンバー変数にすべし
UnityEngine.RectTransform m_RectTransform;

//...

// X軸をずらす
var anchoredPosition = m_RectTransform.anchoredPosition ;
anchoredPosition.x += width ;
m_RectTransform.anchoredPosition = anchoredPosition ;

// トラッカーに登録する
m_Tracker.Add(this, m_RectTransform, DrivenTransformProperties.AnchoredPositionX);
```

すると、操作されたRectTransformがInspector Windowで、編集ができない状態になります。

---

作法として、MonoBehaviour.OnDisableで `UnityEngine.DrivenRectTransformTracker.Clear()` を呼びます。(これにより、トラッカーの登録が解除されます)

