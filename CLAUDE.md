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

This repository provides two purpose-specific Dev Container environments.

### Writing Environment (`.devcontainer/write/`)

Full-featured development environment for article writing and editing.

**Features:**
- Node.js 22 (Debian Bookworm)
- Claude Code CLI integrated (AI writing assistance)
- VS Code extensions:
  - `zenn.zenn-preview` (real-time preview)
  - `yzhang.markdown-all-in-one` (Markdown editing support)
  - `Anthropic.claude-code` (AI integration)
- Claude authentication credentials auto-mounted
- Security hardening (non-root execution, capability restrictions)

**Usage:**
```bash
# Open in VS Code
code .
# Command Palette (Ctrl+Shift+P)
# Select "Dev Containers: Reopen in Container"
# Choose ".devcontainer/write/devcontainer.json"
```

### CI Environment (`.devcontainer/ci/`)

Lightweight environment optimized for textlint execution in GitHub Actions.

**Features:**
- Node.js 22 (Debian Bookworm)
- Minimal dependencies (textlint only)
- No extensions (fast startup)
- No mount configurations (CI compatibility)
- Security hardening (non-root execution, capability restrictions)

**Purpose:**
- GitHub Actions PR checks (automated)
- Local CI environment reproduction for testing

## CI/CD

`proofreading.yml` workflow on PRs to `main`:
- Triggers on `.md`, package, workflow changes, and `.devcontainer/ci/` changes
- Runs textlint in CI Dev Container (`.devcontainer/ci/devcontainer.json`)
- Uses reviewdog for PR comments
- Actions pinned to commit SHA for supply chain security

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