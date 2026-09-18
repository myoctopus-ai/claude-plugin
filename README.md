# claude-plugin

Claude Code plugin marketplace source for the Octopus AI plugin. This repo is what customers add as a marketplace; see [`claude/README.md`](claude/README.md) for what the plugin itself does.

## Install (customer-facing)

```bash
claude plugin marketplace add https://github.com/myoctopus-ai/claude-plugin.git
claude plugin install octopus
```

## Update

```bash
claude plugin update octopus
```

## Layout

```
.claude-plugin/marketplace.json   — marketplace manifest, lists the plugin(s) in this repo
claude/                           — the plugin itself (.claude-plugin/plugin.json, .mcp.json, skills/)
```

## Releasing a change

Edit `claude/`, bump `claude/.claude-plugin/plugin.json`'s `version`, commit, and push to `main`. Customers pick it up on their next `claude plugin update octopus`.
