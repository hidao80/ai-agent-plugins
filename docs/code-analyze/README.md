# code-analyze

A Claude Code skill that systematically analyzes a repository and produces structured documentation readable by both humans and AI agents.

## Overview

`code-analyze` walks through a codebase in 18 defined steps, producing a set of Markdown files under `.claude/analyzed/`. Each file is stamped with the commit hash at the time of analysis, making the output fully traceable and reproducible.

## Usage

```
/code-analyze
```

The skill runs each step in order, updating memory after every step completes. Do not skip steps or change their order.

## Output Structure

All generated files are written to:

```
.claude/analyzed/
├── dependencies.md
├── infra.md
├── databases.md
├── screens.md
├── configurations.md
├── components.md
├── utilities.md
├── performance.md
├── known_bugs.md
├── security.md
├── test.md
├── development-workflow.md
├── overview.md
├── notes.md
├── todo.md
├── naming_convention.md
├── use_cases.md
└── index.md
```

Every generated `.md` file includes this front matter:

```md
---
name: analyzed-{basename}
description: {One-sentence purpose statement.}
type: analysis
commit-hash: {target commit hash}
---
```

## Analysis Steps

| Step | Category | What is documented |
|------|----------|--------------------|
| 1 | `dependencies` | Library name, version, license, vulnerability, update status |
| 2 | `infra` | CI/CD pipelines and IaC configuration |
| 3 | `databases` | Connection info, architecture, migrations, table list by category, domain area summary |
| 4 | `screens` | Entry points, default routes, URL patterns, controller inheritance, view file conventions |
| 5 | `configurations` | Main config files, environment-specific settings |
| 6 | `components` | Application structure, custom vendor namespaces |
| 7 | `utilities` | Global helper classes, functions, and traits |
| 8 | `performance` | Processing time, parallelism, bottleneck identification |
| 9 | `known_bugs` | Security issues, architectural/design issues, compatibility issues, analytical limitations |
| 10 | `security` | Injection (SQL/XSS/Command/Path traversal), credential leakage, authentication/authorization gaps, CORS, data exposure, dependency CVEs, rate limiting, file upload validation, transport security |
| 11 | `test` | Test suite contents and coverage |
| 12 | `development-workflow` | Development workflow and processes |
| 13 | `overview` | Project overview, tech stack, repository structure, request flow, domain config, scale, main features |
| 14 | `notes` | Supplementary notes from the analysis |
| 15 | `todo` | Prioritized list: security, tests, database, code quality, infra, DX, performance |
| 16 | `naming_convention` | Variable, table, column, function, and class naming patterns |
| 17 | `use_cases` | Use case diagrams in Mermaid notation |
| 18 | `index.md` | Index linking all generated documents |

## Quality Rules

### Traceability
- Every file ends with the current commit hash.
- When implementation and tests disagree, tests are the source of truth.
- Discrepancies between layers are documented as Known Bugs or Inconsistencies.

### Documentation Hygiene
- Entries for deleted files, components, or features are removed entirely.
- Diagrams use Mermaid notation (ER, Flow, Use Case).
- Configuration file documentation contains summaries only — no boilerplate.

### Accuracy
- Unverified information is labeled **Unconfirmed**, **Speculative**, or **Unknown**.
- Speculative suggestions include a recommendation score (1–5).

## Allowed Tools

| Tool | Scope |
|------|-------|
| `Bash` | `git *`, `sed`, `cat`, `find`, `wc`, `cd`, `echo`, `ls` |
| `PowerShell` | `git *`, `sed`, `cat`, `where`, `cd`, `echo`, `ls` |
| `Read` | Any file |
| `Write` | `.claude/analyzed/*.md` |
| `WebFetch` | External URLs |
| `Glob` | File pattern matching |
| `Grep` | Content search |
| `Agent` | Subagent delegation |
