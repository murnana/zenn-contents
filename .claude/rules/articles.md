---
description: Zenn article writing rules for Markdown files
paths: 
  - "articles/*.md"
  - "books/**/*.md"
---

## Content Structure

### Article Format
Uses Zenn frontmatter:
```yaml
---
title: "Article Title"
emoji: "🎮"
type: "tech"  # tech or idea
topics: ["android", "game"]
published: true
published_at: 2023-01-14 19:00  # optional
---
```

## Guidelines
- Articles in Japanese
- Polite form (です/ます)
- Japanese technical writing conventions via textlint
- Uses preset-japanese, preset-jtf-style, preset-ja-technical-writing

## textlint Configuration

`.textlintrc` includes:
- Japanese presets (preset-japanese, preset-jtf-style, preset-ja-technical-writing)
- Custom rules for Zenn syntax (`:::`, `:::details`)
- Disabled sentence length limits
- Enforces polite form

## Article Review Workflow

### Review Checklist
When reviewing articles, check the following:

1. **Grammar & Style (textlint)**
   ```bash
   npx textlint articles/<article-name>.md
   npx textlint --fix articles/<article-name>.md  # Auto-fix
   ```
   - Common issues: redundant expressions, punctuation
   - Application names with dots (e.g., `.app`) may need alternative phrasing

2. **Zenn Format**
   - Frontmatter: title, emoji, type, topics, published
   - Zenn syntax: `:::details`, code blocks with language tags
   - JSON syntax: no trailing commas

3. **Technical Accuracy**
   - Verify paths and settings with official docs
   - Check code examples for syntax errors
   - Validate environment variable references

4. **Content Structure**
   - Logical flow: introduction → explanation → solution → references
   - Heading hierarchy (H2 for main sections)
   - Code examples with proper context

5. **Consistency**
   - Terminology (e.g., "Visual Studio Code" not abbreviated in body)
   - Katakana long vowels (コンピューター with ー)
   - Number formatting

### Commit Convention
- Initial commit: "Add article about [topic]"
- Review fixes: "Fix article review issues" with detailed list
- Include bilingual descriptions when appropriate
