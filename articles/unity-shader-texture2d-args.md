---
title: "[Unity] TEXTURE2D_ARGS と TEXTURE2D_PARAM"
emoji: "🖼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity"]
published: false
---

# 環境
- Unity 2020.3.29f1
- Universal RP 10.8.1

# 使い方

関数の引数宣言では `TEXTURE2D_PARAM`
その関数に引数として渡す際は `TEXTURE2D_ARGS`



# 説明

名前が似ていますが、使い分けないとGLES20向けビルドでエラーになります。
[GLES2向け](https://github.com/Unity-Technologies/Graphics/blob/v10.8.1/com.unity.render-pipelines.core/ShaderLibrary/API/GLES2.hlsl#L95) と [GLES3向け](https://github.com/Unity-Technologies/Graphics/blob/v10.8.1/com.unity.render-pipelines.core/ShaderLibrary/API/GLES3.hlsl#L85) で、展開されるマクロが変わるためです。

@[gist](https://gist.github.com/murnana/ff1d00cbb21b74d2925463d71d87e412?file=SampleTexture2DArgs.shader)
