# hidao80-plugins — Claude Code Plugin Marketplace

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-blueviolet?logo=anthropic&logoColor=white)](https://claude.ai/code)

A plugin marketplace registry for [Claude Code](https://claude.ai/code) developed by [hidao80](https://codeberg.org/hidao80/hidao80).

> [!Note]
> This repository contains **no plugin code**. It is a registry that points to external plugin repositories. The actual plugin implementations live in their respective GitHub repositories.

## Available Plugins

| Name | Version | Description | Category | Tags |
|------|---------|-------------|----------|------|
| [claude-code-notify](https://github.com/hidao80/claude-code-notify-plugin) | 2026.03.23 | Plays a completion sound when Claude Code finishes on Windows, macOS, and Linux. | Utilities | `notification` `hook` `sound` `stop-hook` |
| [post-chatwork](https://github.com/hidao80/post-chatwork-skill) | 2026.05.16 | A Claude Code skill plugin for posting messages and files to Chatwork. | Utilities | `notification` `chatwork` `auto-post` |

## How to Use

### Step 1 — Add this marketplace to Claude Code

```bash
claude marketplace add https://github.com/hidao80/claude-marketplace
```

This registers the marketplace so Claude Code can discover the plugins listed in it.

### Step 2 — Install a plugin

```bash
claude plugin install claude-code-notify
```

```bash
claude plugin install post-chatwork
```

Replace the plugin name with any name from the **Available Plugins** table above.

## marketplace.json Schema

The registry is defined in `.claude-plugin/marketplace.json`. Each entry in the `plugins` array follows this structure:

```jsonc
{
  "name": "hidao80-plugins",        // Marketplace identifier
  "owner": {
    "name": "hidao80",              // Owner handle
    "url": "https://..."            // Owner profile URL
  },
  "description": "...",             // Short description of the collection
  "plugins": [
    {
      "name": "plugin-name",        // Unique plugin identifier (used in CLI install command)
      "version": "YYYY.MM.DD",      // Release version
      "source": {
        "source": "github",         // Hosting platform ("github", "gitlab", etc.)
        "repo": "owner/repo"        // Repository path
      },
      "description": "...",         // What the plugin does
      "homepage": "https://...",    // Link to plugin documentation (optional)
      "category": "Utilities",      // Plugin category
      "tags": ["tag1", "tag2"]      // Searchable tags
    }
  ]
}
```

---

## Contributing

To add a new plugin to this marketplace:

1. Fork this repository.
2. Add a new entry to the `plugins` array in `.claude-plugin/marketplace.json`.
3. Fill in all required fields: `name`, `version`, `source`, `description`, `category`, and `tags`.
4. Open a pull request with a brief description of the plugin.

Please ensure the plugin repository is publicly accessible before submitting.

# License

MIT
