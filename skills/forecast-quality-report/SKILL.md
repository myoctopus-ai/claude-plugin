---
name: forecast-quality-report
description: >
  This skill should be used when the user asks "how good was this
  forecast", "score the forecast", "how well supported is this
  forecast", "how many of the changes since last roll were explained",
  "forecast quality report", "forecast accountability", or wants to know
  how much of a forecast's movement from the prior version is backed by
  a recorded reason versus left hanging. Produces a scored report, not
  just a chat answer.
metadata:
  version: "0.1.0"
---

# Forecast Quality Report

Score how well a forecast is supported by measuring how many of its material movements from the prior forecast have a recorded driver, versus how many are unexplained. An unexplained movement degrades the score — the report exists to surface those, not just to produce a number.

## Scope check

Before pulling data, confirm what the user has not already said:

1. **Which forecast is under review**, and **which prior forecast** it is scored against — default to the immediately preceding version unless told otherwise (e.g. the roll it replaced, not the original budget).
2. **Period covered** — remaining months of the year, a quarter, full year.
3. **Level of detail** — entity, cost center, account, or a specific hierarchy branch.
4. **Materiality threshold** — the amount above which a movement counts toward the score at all. Immaterial movements are excluded entirely, not counted as either explained or hanging. If the user hasn't set one, propose one based on the size of the total and confirm it.

Do not guess the version pair. Scoring against the wrong baseline produces a meaningless number.

## Find the material movements

Use the Octopus AI connector for all figures. List the connector's available tools first and pick the ones that expose plan figures by version and hierarchy metadata — do not assume tool names.

Compute, at the agreed level: prior value, current value, delta, delta % per line, exactly as in a version comparison. Apply the materiality threshold to get the scored set — every line at or above it. This set is the denominator; nothing below the threshold enters the score in either direction.

Separate timing from level (see the phasing-vs-level test — same rule as any version comparison): a pure phasing move is not what this report is about, but it still needs a driver on file to count as explained, same as a level change.

## Score each material movement: explained or hanging

A movement counts as **explained** only when a recorded item exists that names its driver — search org memory (insights, answered questions, and task driver fields) scoped to that line's dimensions and the period between the two forecast versions. Any of the following counts:

- An insight on file for that slice and period stating a cause.
- A tracked question about that slice that was actually answered — the answer is the driver.
- A task's recorded driver hypothesis, when it was confirmed rather than left as a guess.

A movement counts as **hanging** when no such record exists — regardless of how obvious the story looks from the numbers alone. Never mark something explained on a plausible narrative you constructed yourself; that is exactly the failure mode this report exists to catch. See `references/scoring-playbook.md` for the full evidence standard per source type and how to handle a partial or stale match.

## Compute the score

Report both:

- **Count score** — explained material movements ÷ total material movements.
- **Magnitude score** — dollar sum of explained movements ÷ dollar sum of all material movements (unsigned, i.e. by absolute delta).

State both, not just one. A count score can look good while one huge unexplained line dominates the actual dollar exposure — always give the reader the size-weighted picture too, the same principle variance-investigation uses for its residual.

## Report the finding

Structure:

1. **Headline** — forecast and prior-forecast names, period, count score, magnitude score.
2. **Hanging movements** — ranked by absolute delta, largest first. This is the priority list: each row is a movement with no driver on file, one line stating what's missing (nothing to cite yet, not a guess at what it might be).
3. **Explained movements** — each with its cited driver source (insight / answered question / confirmed task) and a one-line summary of the recorded reason.
4. **Excluded (immaterial)** — a count only, not a line-by-line list, so the reader knows what was deliberately left out of the score.

Never round a hanging item into "probably fine" — a movement with no record is hanging even if it's small enough to be unsurprising; report it, just don't let it dominate the narrative if it's not material-plus.

## Offer to close the gap

For material hanging movements, offer to send a tracked question to the line's responsible channel or owner asking for the driver — using the same auto-routing a normal ask would use. Only send with the user's go-ahead; do not send questions unprompted just because a report was requested.

## Export

For a one-off quick check, answer inline with the headline score and the hanging-movements table. For a recurring or shareable report, build the file with the matching document skill (pptx or xlsx) using the structure above, and offer to share it to ~~chat rather than doing so unprompted.

## Recurring runs

When the user runs this again for a later roll, match the prior run's level of detail and materiality threshold unless told otherwise, so the score is comparable roll over roll.
