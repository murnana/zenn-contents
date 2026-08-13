---
title: Playable とは
---
### Playable とは
Playable は、APIが駆動する単位のようなものです。正確には、木構造を形成する「節」の部分です。この節は末端になることもあれば、根(ルート)になることもあります。

例えば、1つのAnimationClipからは1つのPlayableが生成されます。Timelineであれば、1つのTimelineClipから1つのPlayableが生成されます。

このPlayableは親子関係を持つことができます。

例えば2つのAnimationClipをブレンドしたい場合、AnimationClipから1つずつと、親となるPlayableを用意します。その後、親のPlayableに子となるAnimationClipを繋げます。

Timelineの場合、TimelineClipがTrackを親として繋がり、さらにTrackがTimeline本体を親として繋がります。
※ TimelineとTrack、TimelineClipについての具体的な話は別のページになります。ここでは「Playableは親子関係を持つ」ことを覚えてください。