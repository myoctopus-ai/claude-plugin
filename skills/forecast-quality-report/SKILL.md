---
name: forecast-quality-report
description: >
  This skill should be used when the user asks "how good was this
  forecast", "score the forecast", "how well supported is this
  forecast", "how many of the changes since last roll were explained",
  "forecast quality report", "forecast accountability", or wants to know
  how much of a forecast's movement from the prior version is backed by
  a recorded reason versus left hanging. Produces a scored deck with
  action items, not just a chat answer.
metadata:
  version: "0.2.0"
---

# Forecast Quality Report

Score how well a forecast is supported by measuring how many of its material movements from the prior forecast have a recorded driver, versus how many are unexplained gaps. An unexplained movement degrades the score — the report exists to surface those and turn both the gaps and the explanations you DO have into a concrete action list, not just to produce a number.

## Scope check

Before pulling data, confirm what the user has not already said:

1. **Which forecast is under review**, and **which prior forecast** it is scored against — default to the immediately preceding version unless told otherwise (e.g. the roll it replaced, not the original budget).
2. **Period covered** — remaining months of the year, a quarter, full year.
3. **Level of detail** — entity, cost center, account, or a specific hierarchy branch.
4. **Materiality threshold** — the amount above which a movement counts toward the score at all. Immaterial movements are excluded entirely, not counted as either explained or hanging. If the user hasn't set one, propose one based on the size of the total and confirm it.

Do not guess the version pair. Scoring against the wrong baseline produces a meaningless number.

## Find the material movements

Use the Octopus AI connector for all figures. List the connector's available tools first and pick the ones that expose plan figures by version and hierarchy metadata — do not assume tool names. Version display names vary per organization; resolve a version name to its number rather than guessing (there's a connector tool for exactly this — look for one that returns the org's forecast-version legend).

Compute, at the agreed level: prior value, current value, delta, delta % per line, exactly as in a version comparison. Apply the materiality threshold to get the scored set — every line at or above it. This set is the denominator; nothing below the threshold enters the score in either direction.

Separate timing from level (see the phasing-vs-level test — same rule as any version comparison): a pure phasing move is not what this report is about, but it still needs a driver on file to count as explained, same as a level change.

## Score each material movement: explained or hanging

A movement counts as **explained** only when a recorded item exists that names its driver — search org memory (insights, answered questions, and task driver fields) scoped to that line's dimensions and the period between the two forecast versions. Search per material line (or per material group sharing the same dimensions), not once broadly for the whole report — a handful of broad queries will miss real matches and understate the score. Any of the following counts:

- An insight on file for that slice and period stating a cause.
- A tracked question about that slice that was actually answered — the answer is the driver.
- A task's recorded driver hypothesis, when it was confirmed rather than left as a guess.

A movement counts as **hanging** — an unexplained gap — when no such record exists, regardless of how obvious the story looks from the numbers alone. Never mark something explained on a plausible narrative you constructed yourself; that is exactly the failure mode this report exists to catch. A record on an adjacent or topically-similar slice does not count either — state specifically why it falls short rather than silently skipping it. See `references/scoring-playbook.md` for the full evidence standard per source type and how to handle a partial or stale match.

## Compute the score

Report both:

- **Count score** — explained material movements ÷ total material movements.
- **Magnitude score** — dollar sum of explained movements ÷ dollar sum of all material movements (unsigned, i.e. by absolute delta).

State both, not just one. A count score can look good while one huge unexplained line dominates the actual dollar exposure — always give the reader the size-weighted picture too, the same principle variance-investigation uses for its residual.

## Derive action items

Every material movement gets exactly one action item — this is what turns the report into something a reader can act on, not just a scored table.

**Hanging (unexplained gap)** — the action item is always some form of "get a driver on record": offer to send a tracked question to the line's responsible owner or channel (the connector can auto-route by the line's dimensions). Rank these by absolute delta — the largest hanging gaps are the priority asks.

**Explained** — derive the action item FROM what the recorded explanation itself says, never from what you'd guess given the topic. Read the recorded text for a forward-looking implication:

- Names a fix, a decision pending, or an owner who needs to follow up → that's the action item, stated as the record states it (e.g. "confirm the Q2 vendor renegotiation lands before the next roll," not invented phrasing).
- States something is one-time / already resolved / recurring-and-accepted, with no further step implied → the action item is **"No action — explanation is definitive,"** not a manufactured task.
- Is ambiguous about whether anything further is needed → say so explicitly rather than picking a side.

Never add an action item beyond what the record supports. An explained line with a definitive, closed explanation should end with "no action," not a follow-up you invented to make the list feel more thorough. See `references/report-structure.md` for the action-item derivation rules and worked examples.

## Report the finding

Default to a deck (see Export) built from these sections; if answering inline for a quick check, cover the same ground more briefly:

1. **Headline** — forecast and prior-forecast names, period, materiality threshold, count score, magnitude score.
2. **Unexplained gaps** — the hanging list, ranked by absolute delta, largest first. Each row: line, delta, and the action item (send a tracked question) — this is the priority list.
3. **Explained movements** — each with its cited driver source (insight / answered question / confirmed task), a one-line summary of the recorded reason, and its derived action item (including "no action" where that's what the record supports).
4. **Action items summary** — every non-"no action" item from both sections above, in one consolidated list ordered by the underlying line's absolute delta, so the reader has a single prioritized to-do list rather than having to assemble one from two tables.
5. **Excluded (immaterial)** — a count only, not a line-by-line list, so the reader knows what was deliberately left out of the score.

Never round a hanging item into "probably fine" — a movement with no record is hanging even if it's small enough to be unsurprising; report it, just don't let it dominate the narrative if it's not material-plus.

## Offer to close the gap

For unexplained gaps, offer to send the suggested tracked questions from the action items summary — using the same auto-routing a normal ask would use. Only send with the user's go-ahead; do not send questions unprompted just because a report was requested.

## Export

Default to building a deck (pptx) — this report is meant to be reviewed and acted on by people who weren't in the room, not just read once in chat. Build it with the pptx skill, using the slide-by-slide structure in `references/report-structure.md`. Only skip the file and answer inline when the user explicitly asks for a quick number rather than a report (e.g. "what's the score" with nothing more).

An xlsx export is the right call instead when the user specifically wants the full line-level data to filter/sort themselves rather than a reviewable narrative deck.

Present the finished file and offer to share it to ~~chat rather than doing so unprompted.

## Recurring runs

When the user runs this again for a later roll, match the prior run's level of detail, materiality threshold, and export format unless told otherwise, so the score is comparable roll over roll.
