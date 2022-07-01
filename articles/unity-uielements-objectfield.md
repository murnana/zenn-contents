---
title: "[Unity] UXMLテンプレートでObjectFieldを使うときのタイプ指定方法"
emoji: "📄"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: true
published_at: 2022-07-01 20:00
---

# 使用方法
```xml:*.uxml
<uie:ObjectField type="UnityEngine.Texture2D, UnityEngine.CoreModule" />
```
タイプ指定には [`System.Type.AssemblyQualifiedName`][System.Type.AssemblyQualifiedName] の一部を使用してください。[`System.Type.AssemblyQualifiedName`][System.Type.AssemblyQualifiedName]の結果は`UnityEngine.Texture2D, UnityEngine.CoreModule, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` のように、アセンブリのバージョン番号、カルチャ、トークンが出てきますが、これらは不要です。


# 説明

[`ObjectField`][UIElements.ObjectField] はUIElementsのコントロールフィールドの一種です。
テクスチャ、マテリアル、シェーダー、コンポーネントなど、[UnityEngine.Object][UnityEngine.Object] のものを入れるためのフィールドになります。


[`ObjectField`][UIElements.ObjectField] に入るものは [UnityEngine.Object][UnityEngine.Object] の派生であれば何でもよいため、UXMLテンプレートで何も宣言していない場合は _<no type>_ と表示されます。
![no type と表示されているObjectField](/images/unity-uielements-objectfield/failed-no-type.jpg)*<uie:ObjectField label="Texture" />で、no type と表示されているObjectField*


型を指定する場合は `type` 属性に値を入れる必要があります。しかし `type` 属性に対して`<uie:ObjectField label="Texture" type="UnityEngine.Texture2D" />`と入力しても変化はありません。
Consoleウィンドウには、_TypeLoadException: Could not load type 'UnityEngine.Texture2D' from assembly 'UnityEngine.UIElementsModule, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null'._ というエラー分が表示されます。


UI ToolKit上では _Type_ 項目に `UnityEngine.Texture2D` と入れると、アセンブリ名は勝手に入ってきます。
![UI ToolKit上のObjectField](/images/unity-uielements-objectfield/add-from-ui-toolkit.jpg)*UI ToolKit上のObjectField*
.uxml ファイル上では以下の形になります
```xml:*.uxml
<uie:ObjectField label="Texture" type="UnityEngine.Texture2D, UnityEngine.CoreModule" />
```


### 環境
- Unity: 2021.3.4f1
- OS: Microsoft Windows 10 Home 10.0.19044
- CPU: Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
- GPU: NVIDIA GeForce GTX 1060 3GB
- RAM:  16.0 GB
- システムの種類: 64 ビット オペレーティング システム、x64 ベース プロセッサ




# 参考
- [No type in ObjectField created in uxml (UIElements) - Unity Answers](https://answers.unity.com/questions/1625662/in-objectfield-created-in-uxml-uielements.html)


[System.Type.AssemblyQualifiedName]: https://docs.microsoft.com/en-us/dotnet/api/system.type.assemblyqualifiedname?view=net-6.0
[UIElements.ObjectField]: https://docs.unity3d.com/2021.3/Documentation/ScriptReference/UIElements.ObjectField.html
[UnityEngine.Object]: https://docs.unity3d.com/2021.3/Documentation/ScriptReference/Object.html
