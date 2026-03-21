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


## Development Environment

Dev Container configuration is at `.devcontainer/devcontainer.json`.

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
```

## CI/CD

`proofreading.yml` workflow on PRs to `main`:
- Triggers on `.md`, package, workflow changes, and `.devcontainer/` changes
- Runs textlint in Dev Container (`.devcontainer/devcontainer.json`)
- Uses reviewdog for PR comments
- Actions pinned to commit SHA for supply chain security
