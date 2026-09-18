# Connectors

## How tool references work

Plugin files use `~~category` as a placeholder for whatever tool you connect in that category. The plugin describes workflows in terms of categories rather than specific products, so the same skills work whichever tools your organization uses.

## Connectors for this plugin

| Category      | Placeholder | Included servers | Other options          |
| ------------- | ----------- | ---------------- | ---------------------- |
| FP&A platform | —           | Octopus AI       | —                      |
| Chat          | `~~chat`    | —                | Slack, Microsoft Teams |

The Octopus AI connector ships with the plugin and is required. `~~chat` is optional — it is only used to share a finished report or finding, and the skills will simply present the file in the conversation if no chat tool is connected.
