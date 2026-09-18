# Octopus

Claude Code plugin marketplace for Octopus AI. Connects Claude to Octopus AI's Leo — a conversational connection plus a set of direct, deterministic tools for data and messaging — and adds three FP&A workflows on top.

## Install

```bash
claude plugin marketplace add https://github.com/myoctopus-ai/octopus-ai-plugin.git
claude plugin install octopus-ai
```

## Update

```bash
claude plugin update octopus-ai
```

## What's included

| Component | Name | Purpose |
| --- | --- | --- |
| MCP server | Leo | Connection to Octopus AI at `https://app.myoctopus.ai/mcp` |
| Tool | Ask Octopus | Conversational access to Leo, with your existing permissions, actions and history |
| Tool | Explore dimension hierarchy | List a dimension's hierarchies, or walk one's node tree (a node's immediate children, or the full subtree) |
| Tool | Get user/org preferences | Read the stored exclusions, visibility rules, and display settings for you and your organization |
| Tool | Search insights | Search org memory — insights and discussion — by semantic query, dimension, person, and date range, all optional |
| Tool | Query data | Plan or transaction figures for a business slice, grouped or (transactions) row-level |
| Tool | List channels and users | Who and where a question or message could go |
| Tool | Send a question | A tracked ask to an explicit channel/person, or auto-routed by business slice |
| Tool | Question status | Which sent questions are answered versus still open |
| Skill | Version Comparison Report | Compares two or more versions — forecast, budget, working, actuals — with a red/amber/green status per line, and exports a deck or spreadsheet |
| Skill | Forecast Quality Report | Scores a forecast by how many of its material movements from the prior version have a recorded driver versus are left hanging |
| Skill | Variance Investigation | Works a budget-vs-actual gap from headline down to transaction-level cause |

Every tool above is read-only except "Send a question."

## Setup

Installing the plugin adds the Leo connector. Sign in to Octopus AI when prompted — Leo uses an OAuth sign-in flow, so there is no API key to configure and no environment variables to set.

Confirm the connection by asking Claude what Leo data is available before running a skill or tool.

## Usage

**Version comparison** — trigger with phrases like:

- "Compare the current forecast to last month's roll"
- "What changed since the prior forecast?"
- "Compare budget to working for Q4"
- "Build the forecast change deck"

Claude will confirm which versions, the period, the level of detail, the materiality threshold, the RAG method, and whether you want a deck or a spreadsheet, then produce the file.

**Forecast quality** — trigger with phrases like:

- "How good was this forecast?"
- "How many of the changes since last roll were actually explained?"
- "Score this roll's forecast quality"

Claude will confirm the forecast/prior-forecast pair, period, and materiality threshold, then report a count score and a dollar-weighted score, with every hanging (unexplained) movement listed and offered up as a question to send.

**Variance investigation** — trigger with phrases like:

- "Why is this account over budget?"
- "Investigate the variance in this cost center"
- "Drill into what's driving the overspend"

Claude will confirm the comparison basis and materiality, then work down through validity checks, hierarchy decomposition, price/volume/timing tests, and — only if needed — transaction review. It reports the cause, the evidence behind it, and any amount left unexplained.

**Direct tools** — Claude can also use the tools above on their own, without a skill, whenever a request is a plain lookup rather than a report: "what departments roll up under IT?", "what are my saved preferences?", "has anyone explained the marketing overspend?", "pull Q3 actuals by vendor", "who's in the #fpa-it-spend channel?", "ask finance ops about the late accrual", "did anyone answer that headcount question?".

## Customization

All three skills reference a chat tool generically as `~~chat` for sharing finished output. See `CONNECTORS.md` for the categories used and how placeholders resolve to whatever tools you have connected.

## Releasing a change

Edit the plugin's files, bump `.claude-plugin/plugin.json`'s `version`, commit, and push to `main`. Customers pick it up on their next `claude plugin update octopus-ai`.

## Version

0.2.0
