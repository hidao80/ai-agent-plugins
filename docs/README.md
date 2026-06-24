![](./images/hero_image.webp)

# ai-agent-plugins — AI agent Plugins by hidao80

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-blueviolet?logo=anthropic&logoColor=white)](https://claude.ai/code)

A plugin marketplace registry for [Claude Code](https://claude.ai/code) developed by [hidao80](https://github.com/hidao80/hidao80).

## Available Plugins

| Name | Version | Description | Category | Tags |
|------|---------|-------------|----------|------|
| [claude-code-notify](https://github.com/hidao80/claude-code-notify-plugin) | 2026.03.23 | Plays a completion sound when Claude Code finishes on Windows, macOS, and Linux. | Utilities | `notification` `hook` `sound` `stop-hook` |
| [post-chatwork](https://github.com/hidao80/post-chatwork-skill) | 2026.05.16 | A Claude Code skill plugin for posting messages and files to Chatwork. | Utilities | `notification` `chatwork` `auto-post` |
| [code-analyze](code-analyze/README.md) | 2026.05.08 | Record and update project documentation in a readable state for both humans and AI. | developed | `command` `documentation` `analyze` |
| [commit-msg](commit-msg/README.md) | 2026.03.18 | Propose concise commit messages. | developed | `command` `documentation` `git` |
| [modernize-repo](modernize-repo/README.md) | 2026.03.03 | Modernize your repository with Docker, CI/CD, and README updates for JavaScript, Python and PHP. | developed | `command` `modernize` `javascript` `python` `php` |
| [sbi-feedback](sbi-feedback/README.md) | 2025.11.24 | Provide structured positive feedback for work reports using the SBI framework. | human-resources | `command` `report` `feedback` `hr` |
| [security-audit](security-audit/README.md) | 2025.11.24 | Checks the security of a repository. Use for security audits, vulnerability checks, and privacy verification. | human-resources | `command` `security` `audit` `vulnerability` |

## How to Use (Claude Code)

### Step 1 — Add this marketplace to Claude Code

```bash
claude plugin marketplace add https://github.com/hidao80/claude-plugins.git
```

or from the Claude Code prompt:

```bash
/plugin marketplace add https://github.com/hidao80/claude-plugins.git
```


This registers the marketplace so Claude Code can discover the plugins listed in it.

### Step 2 — Install a plugin

```bash
claude plugin install <plugin-name>@hidao80-plugins
```

or from the Claude Code prompt:

```bash
/plugin install <plugin-name>@hidao80-plugins
```

Replace the plugin name with any name from the **Available Plugins** table above.

## How to Use (Codex CLI)

### Step 1 — Add this marketplace to Codex CLI

```bash
codex plugin marketplace add hidao80/claude-plugins
```

### Step 2 — Install a plugin

Open `/plugins` in Codex CLI, select the **hidao80-plugins** marketplace, and install the plugin you want — or install it directly by name:

```bash
/plugin install <plugin-name>@hidao80-plugins
```

Replace the plugin name with any name from the **Available Plugins** table above. Start a new thread (or restart the Codex desktop app) to activate it.

## Contributing

To add a new plugin to this marketplace:

1. Fork this repository.
2. Add a new entry to the `plugins` array in `.claude-plugin/marketplace.json`.
3. Fill in all required fields: `name`, `version`, `source`, `description`, `homepage`, `category`, and `tags`.
4. Open a pull request with a brief description of the plugin.

Please ensure the plugin repository is publicly accessible before submitting.

# License

MIT
