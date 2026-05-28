# modernize-repo

A Claude Code skill that modernizes a repository with Docker, CI/CD, and README updates for JavaScript, Python, and PHP projects.

## Usage

```
/modernize-repo [js|python|php]
```

## What It Does

### 1. Dockerize
- `Dockerfile` — multi-stage build
- `docker-compose.yml` — for local development
- `.dockerignore`

### 2. CI/CD
- GitHub Actions workflow — lint, audit, and Docker build

### 3. E2E Testing (Playwright)
- Adds `@playwright/test` to `package.json` devDependencies
- Adds scripts to `package.json`:
  - `"test:e2e": "playwright test"`
  - `"screenshot": "playwright test tests/e2e/screenshot.spec.ts"` — full-page screenshots only
- Creates `playwright.config.ts`
- Creates `tests/e2e/screenshot.spec.ts`

### 4. README Updates
- Adds CI badges
- Adds a Quick Start section with Docker and Podman startup instructions

## Options

| Flag | Effect |
|------|--------|
| `--skip-docker` | Skip all Docker-related files |
| `--skip-ci` | Skip all CI/CD-related files |
| `--readme-only` | Update README only |

## Allowed Tools

| Tool | Scope |
|------|-------|
| `Bash` | `git *`, `docker`, `podman` |
| `PowerShell` | `git *`, `docker`, `podman` |
| `Edit` | `*.json`, `*.toml`, `*.yaml`, `*.yml`, `requests.txt`, `*.md`, `*.lock`, `composer.*`, `package.*`, `pyproject.*`, `Dockerfile*`, `docker-compose*`, `.dockerignore`, `.github/**/*` |
| `Glob` | File pattern matching |
| `Grep` | Content search |
| `WebFetch` | External URLs |
| `Agent` | Subagent delegation |
