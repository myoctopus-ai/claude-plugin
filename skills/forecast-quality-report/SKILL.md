---
name: forecast-quality-report
description: >
  This skill should be used when the user asks "how good was this
  forecast", "score the forecast", "how well supported is this
  forecast", "how many of the changes since last roll were explained",
  "forecast quality report", "forecast accountability", or wants to know
  how much of a forecast's movement from the prior version is backed by
  a recorded reason versus left as an unexplained gap. Produces a scored
  deck — risks and saving opportunities, who explained what and how
  well, and one page of actions — not just a chat answer.
metadata:
  version: "0.3.0"
---

# Forecast Quality Report

Score how well a forecast is supported: of the material cost movements between the version under review and the version it replaced, how many have a human explanation on record, how good those explanations are, and how much exposure is still an unexplained gap. Every movement is a **risk** (cost went up) or a **saving opportunity** (cost went down); every gap is a place a human hasn't yet said why. The report's job is to get more of those reasons recorded — it scores, then it asks.

## Scope check

Confirm only what the user has not already said:

1. **Which forecast is under review.** The baseline is never asked for — it is always the version this one replaced (see "Pick the baseline").
2. **Period covered** — default the rest of the year from the version's first forecast month; accept a quarter or full year if asked.
3. **Level of detail** — entity, cost center, account, or a hierarchy branch. Default to the top level of the org's account hierarchy.
4. **Materiality threshold** — the movement size that counts. Propose one from the total's size and confirm it.
5. **Revenue is excluded by default.** This is a cost-quality report; revenue lines swamp the exposure numbers and are owned by a different process. Include revenue only if the user asks, and then score it on its own slide with the favorable/unfavorable direction inverted.

## Pick the baseline

Always the prior version — the roll this one replaced — never a user-chosen pair:

- Resolve the version under review to its number with the connector's forecast-legend tool (names vary by org: one org's "RF08FN" is another's "Roll08" — never guess).
- The baseline is the largest loaded forecast number below it for the same year, skipping Working. Confirm it is actually loaded (a plan query that comes back empty means it isn't — fall back to the next one down). If the version under review is the first roll of the year, the baseline is Budget.
- State the pair on the cover. Do not ask "which prior?" — the answer is defined, and asking invites a wrong baseline that makes the score meaningless.

## Pull the figures

Use the Octopus AI connector; list its tools first and use the ones that expose plan figures by version, actuals, the account hierarchy, and the forecast legend — do not assume tool names.

- Pull both versions' plan figures for the year at the agreed level, **excluding income-type accounts** (the plan query takes an account-type exclusion; if the org's type vocabulary differs, the tool's error lists it).
- Pull actuals by month for the year to find the **closed month** — the latest month with actuals. Months up to and including it are **YTD**; later months are **YTG**; all twelve are **FY**.
- Roll leaves up through the hierarchy to the agreed level; never sum a parent with its own children.

## Find the material movements

Per line: baseline, current, delta, delta %. Apply the threshold to get the scored set — the denominator. Immaterial lines are outside the score in both directions.

Classify every material movement:

- **Risk** — cost went **up** (unfavorable). Red.
- **Saving opportunity** — cost went **down** (favorable). Green.
- Separate **timing** (nets to ~zero across the period; months moved) from **level** (the total moved). A timing move is still scored and still needs a reason, but say it is timing so nobody reads it as a real cut or overrun.

Never present a movement as just a signed number. The sign alone reads as "good"/"bad" the wrong way round for costs; the reader should see Risk or Opportunity every time.

## Gather the human explanations

For each material movement, look for a recorded human reason — search per line (or per group sharing dimensions), not once broadly:

- **Org memory** (insights and discussion) scoped to the line's dimensions and the window between the two versions, noting **who** recorded it and **when**.
- **Questions asked** — the task search filtered to the same dimensions and window: who asked, who answered, the reply, and any insight the answer was captured into. An answered or resolved question **is** a human explanation; an open one is a gap that already has a question in flight (say so — don't ask it twice).

A movement is **explained** only when a record names its driver and covers its dimensions and period. Adjacent, partial or stale records do not count — list them separately as "looks like an explanation but isn't", and say specifically why. Never explain a movement from the shape of the numbers. See `references/scoring-playbook.md` for the evidence standard and for grading the explanations you do find.

## Grade each explanation

Every explanation gets a quality grade against four marks — cause named, quantified, one-time vs recurring stated, owner or timing given — and **one** coaching line on what would make it stronger next time (`references/scoring-playbook.md`, "Explanation quality"). This is the report's teaching moment: name the person, credit what they did well, and say the one thing to add. One line. Not a paragraph.

## Compute the score

- **Count score** — explained material movements ÷ total material movements.
- **Magnitude score** — |delta| of explained movements ÷ |delta| of all material movements.
- **Explained risk vs explained opportunity** — the two magnitudes split by direction, so the reader sees whether the *risks* specifically have reasons.

Report all of them. A good count score with the largest risk unexplained is not a good forecast.

## Derive the actions

One action per material movement, all landing on a single closing slide:

- **Unexplained gap** → ask the line's owner for the reason — a tracked question, auto-routed by the line's dimensions. Phrase it as what the reason would let the org do: *"to size this risk"*, *"to lock in this saving"*. Largest exposure first.
- **Explained, forward step in the record** → that step, as the record states it (closely paraphrased, never embellished).
- **Explained, thin explanation** → ask the same person for the missing mark (the amount, the recurrence, the owner) — a short follow-up, credited to them, not a new question to a channel.
- **Explained and definitive** → "No action — explanation is definitive." Expected and correct; do not manufacture a follow-up.

Every ask says what a fuller reason buys. People give more explanations when they see what the explanation is for.

## Build the deck

Always a deck (pptx skill), laid out per `references/report-structure.md`: cover with the FY / YTD / YTG version summary color-coded by risk and opportunity, then the score, then risks and opportunities with their explanations and grades, then the records that don't count, then exactly one page of recommended actions at the end. Answer inline only when the user asks for a bare score.

Offer to send the tracked questions from the actions page — only with the user's go-ahead — and offer to share the deck to ~~chat rather than doing so unprompted.

## Recurring runs

Next roll, keep the same level, threshold and revenue treatment so scores compare roll over roll, and note the score's movement on the cover.
