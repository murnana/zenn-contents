# CLAUDE.md

Claude Code guidance for this repository.

## Project Overview

Zenn content repository with Japanese technical articles and books. Contains:

- **articles/**: Technical articles in Markdown
- **books/**: Technical books in Markdown (empty, can add content)
- **images/**: Static assets
- **assets/**: Additional resources

## Commands

### Content Management
```bash
# Preview content locally
npx zenn preview

# Create new article
npx zenn new:article

# Create new book
npx zenn new:book

# List articles
npx zenn list:articles

# List books
npx zenn list:books
```

### Quality Assurance
```bash
# Run textlint (main test command)
npm test

# Manual textlint with options
npx textlint -f checkstyle './{articles,books}/*.md'
```

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

### Guidelines
- Articles in Japanese
- Polite form (です/ます)
- Japanese technical writing conventions via textlint
- Uses preset-japanese, preset-jtf-style, preset-ja-technical-writing

## Development Environment

Dev Containers with:
- Node.js 20 (Bullseye)
- VS Code extensions:
  - `zenn.zenn-preview`
  - `yzhang.markdown-all-in-one`
  - `Anthropic.claude-code`

## CI/CD

`proofreading.yml` workflow on PRs to `main`:
- Triggers on `.md`, package, workflow changes
- Runs textlint in Dev Container
- Uses reviewdog for PR comments

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