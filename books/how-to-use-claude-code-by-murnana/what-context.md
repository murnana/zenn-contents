---
title: "コンテキストとは、なぜAIは忘れるのか"
---

## コンテキストとは

正確には「Context window」(コンテキストウィンドウ) と言います。
会話履歴、ファイル内容、検索やツールの実行結果、プロンプトなど、Claudeが全て認識できる情報の最大総量を指します。

最大数量はモデルによって異なります。


## チャットまたはセッションが長いと、なぜ「バカ」になっていくのか
これは人間と同じで、最初のほうで行っていた会話をわすれてたり、逆に情報量が多すぎて混乱するといった状況に似ています。

一般的に、LLMには1回の会話で覚えていられる量に限界があると分かっています。

これはAIの仕組み上、現在の技術では避けることができません。

この覚えていられる量の限界に近づくと、理解していたはずのもの精度が落ちてきます。

## 参考
- [Explore the context window - Claude Code Docs](https://code.claude.com/docs/en/context-window)
- [Give Claude context: CLAUDE.md and better prompts | Claude Help Center](https://support.claude.com/en/articles/14553240-give-claude-context-claude-md-and-better-prompts)
- [Context windows - Claude Platform Docs](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- [Effective context engineering for AI agents \ Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
