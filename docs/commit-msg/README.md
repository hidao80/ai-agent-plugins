# commit-msg

A Claude Code skill that proposes concise, well-formatted commit messages.

## Overview

`commit-msg` inspects the current diff and git history, then suggests a commit message that is short, clear, and prefixed with an appropriate [gitmoji](https://gitmoji.dev/) tag. Emojis are never used — only the text tag (e.g. `:bug:`, `:sparkles:`).

## Usage

```
/commit-msg
```

Run after staging your changes. The skill reads the staged diff and recent log to propose a message that fits the project's commit history.

## Output Format

```
:gitmoji: Short imperative summary
```

**Example:**

```
:bug: Fix null pointer in user authentication flow
```

## Rules

- **No emojis** — gitmoji tags are written in text form only (`:tag:`)
- **Concise** — the subject line is kept short and focused
- **Imperative mood** — written as a command ("Fix", "Add", "Remove")
