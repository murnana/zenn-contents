---
title: "[Unity] Property Drawerによるフィールドのカスタマイズ"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: true
---

# 作り方

1. [`UnityEngine.PropertyAttribute`][PropertyAttribute] を継承したC#クラスを作成します (これを仮に、`SampleAttribute` とします)
2. `SampleAttribute` を描画するためのクラスとして、[`UnityEditor.PropertyDrawer`][PropertyDrawer] を継承したクラスを作成します (仮に `SampleAttributeDrawer`とします)
3. `SampleAttributeDrawer` に属性として [`UnityEditor.CustomPropertyDrawer`][CustomPropertyDrawer] をつけます。
4. [`UnityEditor.CustomPropertyDrawer`][CustomPropertyDrawer] のコンストラクターにある引数として、描画する対象の属性 (`SampleAttribute`) の型を与えます

```cs: SampleAttribute.cs
// Inspectorウィンドウで表示している
// フィールドのラベルを任意に変更する
// という目的をもたせてみます
public class SampleAttribute : UnityEngine.PropertyAttribute {
  /// <summary>
  /// コンストラクタ
  /// </summary>
  /// <param name="label">ラベルとして表示するテキスト</param>
  public SampleAttribute(string label) {
      Label = label;
  }

  /// <summary>
  /// ラベルに表示するテキスト
  /// </summary>
  public string Label { get; }
}
```
```cs: SampleAttributeDrawer.cs
[UnityEditor.CustomPropertyDrawer(typeof(SampleAttribute))]
public class SampleAttributeDrawer : UnityEditor.PropertyDrawer {
  // 後述する描画を行うメンバー関数を追加します
}
```

## IMGUIを使用して描画を行う

1. `SampleAttributeDrawer` のメンバー関数として `SampleAttributeDrawer.GetPropertyHeight` と `SampleAttributeDrawer.OnGUI` を作成します
2. [`SampleAttributeDrawer.OnGUI`][PropertyDrawer.OnGUI] では `UnityEditor.EditorGUI` または `UnityEngine.GUI` を使用して描画をさせます
3. [`SampleAttributeDrawer.GetPropertyHeight`][PropertyDrawer.GetPropertyHeight] で、`SampleAttributeDrawer.OnGUI` 内で描画した結果の合計となる高さを返却します

```cs: SampleAttributeDrawer.cs
[UnityEditor.CustomPropertyDrawer(typeof(SampleAttribute))]
public class SampleAttributeDrawer : UnityEditor.PropertyDrawer
{
  /// <inheritdoc />
  public override float GetPropertyHeight(
    UnityEditor.SerializedProperty property,
    UnityEngine.GUIContent         label
  )
  {
    // カスタム対象のAttributeに変換します
    var labelAttribute = (SampleAttribute)attribute;

    // (今回は)カスタムラベルの情報を作成します
    var customLabel = new UnityEngine.GUIContent(labelAttribute.Label);

    // 元の UnityEditor.SerializedProperty とカスタムしたラベルの高さを合わせて返却します
    return UnityEditor.EditorGUI.GetPropertyHeight(
      property: property,
      label: customLabel,
      includeChildren: true
    );
  }

  /// <inheritdoc />
  public override void OnGUI(
    UnityEngine.Rect               position,
    UnityEditor.SerializedProperty property,
    UnityEngine.GUIContent         label
  )
  {
    // カスタム対象のAttributeに変換します
    var labelAttribute = (SampleAttribute)attribute;

    // (今回は)カスタムラベルの情報を作成します
    var customLabel    = new UnityEngine.GUIContent(labelAttribute.Label);

    // 元の UnityEditor.SerializedProperty とともにカスタムしたラベルで描画します
    UnityEditor.EditorGUI.PropertyField(
      position: position,
      property: property,
      label: customLabel,
      includeChildren: true
    );
  }
}
```


## UIToolKit (UIElements) で描画を行う
:::message
元となるウィンドウがUIelementで形成されない限り、この関数が使われることはありません。
そのため、必ずIMGUIで描画する方も一緒に実装してください
:::

1. [SampleAttributeDrawer.CreatePropertyGUI][PropertyDrawer.CreatePropertyGUI] を作成します
2. 内部でVisualElementsを作成し、返却します

```cs: SampleAttributeDrawer.cs
[UnityEditor.CustomPropertyDrawer(typeof(SampleAttribute))]
public class SampleAttributeDrawer : UnityEditor.PropertyDrawer
{
  /// <inheritdoc />
  public override UnityEngine.UIElements.VisualElement CreatePropertyGUI(
      UnityEditor.SerializedProperty property
  )
  {
    // カスタム対象のAttributeに変換します
    var labelAttribute = (SampleAttribute)attribute;

    // カスタムしたラベル情報とともに、元のプロパティのUIElementを返却します
    return new UnityEditor.UIElements.PropertyField(
      property: property,
      label: labelAttribute.Label
    );
  }
}
```


# 説明

「Property Drawer」(プロパティ ドロワー)はシリアライズ可能なフィールド(メンバ変数)を、UnityEditorのInspectorウィンドウ上など、GUIで「いい感じに」表示するために使われます。「いい感じ」というのは、プログラムで不都合な値が入らないよう制約をかけたり、フィールドの名称だけでは説明不足であるという理由で、マウスオーバーでそのフィールドを指した際に現れるツールチップスとしてテキストを加えるなどが可能です。

Inspector上にコンポーネントをカスタマイズして表示する`UnityEditor.Editor`と違い、`UnityEngine.PropertyAttribute`は特定のコンポーネントクラスに依存せず、属性を付与されたフィールドに対してカスタマイズを行うことができます。これによりカスタマイズした「Property Drawer」はさまざまなフィールドで使いまわすことができます。


### 環境
- Unity: 2021.3.4f1
- OS: Microsoft Windows 10 Home 10.0.19044
- CPU: Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
- GPU: NVIDIA GeForce GTX 1060 3GB
- RAM:  16.0 GB
- システムの種類: 64 ビット オペレーティング システム、x64 ベース プロセッサ


# 参考

- [Unity - Manual: Property Drawers](https://docs.unity3d.com/2021.3/Documentation/Manual/editor-PropertyDrawers.html)
- [【C#, Unity】自作Attributeの作り方とその応用例(PropertyAttribute) - はなちるのマイノート](https://www.hanachiru-blog.com/entry/2022/04/20/153950)
- [自分だけのPropertyDrawerを作ろう！ - Qiita](https://qiita.com/kyusyukeigo/items/8be4cdef97496a68a39d)

[PropertyAttribute]: "https://docs.unity3d.com/2021.3/Documentation/ScriptReference/PropertyAttribute.html"

[PropertyDrawer]: "https://docs.unity3d.com/2021.3/Documentation/ScriptReference/PropertyDrawer.html"
[PropertyDrawer.GetPropertyHeight]: "https://docs.unity3d.com/2021.3/Documentation/ScriptReference/PropertyDrawer.GetPropertyHeight.html"
[PropertyDrawer.OnGUI]: "https://docs.unity3d.com/2021.3/Documentation/ScriptReference/PropertyDrawer.OnGUI.html"
[PropertyDrawer.CreatePropertyGUI]: "https://docs.unity3d.com/2021.3/Documentation/ScriptReference/PropertyDrawer.CreatePropertyGUI.html"

[CustomPropertyDrawer]: "https://docs.unity3d.com/2021.3/Documentation/ScriptReference/CustomPropertyDrawer.html"
