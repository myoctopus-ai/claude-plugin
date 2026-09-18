# Report Structure

Slide-by-slide layout for the forecast quality deck, and the rules for deriving each movement's action item.

## Slide 1 — Headline

- Forecast under review and the prior forecast it's scored against, both by their org display name (from the version legend, never a guessed label).
- Period covered, level of detail, materiality threshold.
- Count score and magnitude score, both stated plainly (e.g. "14 of 78 material movements explained (18%) — $2.1M of $68.5M by dollar exposure (3%)").
- One line naming the biggest single driver of the magnitude gap, if one line dominates (e.g. "one $31M unexplained line accounts for 45% of the unexplained exposure").

## Slide(s) 2 — Unexplained gaps

Table, ranked by absolute delta, largest first: line, prior value, current value, delta, suggested action ("Send tracked question to `<owner/channel>`" if resolvable via the line's dimensions, else "Send tracked question — no clear owner, ask an admin to assign one"). This is the slide a reviewer acts on first — lead the deck with it, not bury it after the explained movements.

If the list is long, split across slides by rank (e.g. top 15 per slide) rather than shrinking the table to fit — a report a reviewer can't read is worse than one more slide.

## Slide(s) 3 — Explained movements

Table: line, delta, driver source (insight / answered question / confirmed task), one-line recorded reason, derived action item. See "Deriving an action item from an explanation" below for exactly how the last column is filled — most rows will legitimately read "No action — explanation is definitive," and that's the expected, correct outcome for a well-explained movement, not a gap in the report.

## Slide 4 — Action items summary

One consolidated table, every row from slides 2 and 3 that is NOT "no action," ordered by the underlying line's absolute delta (largest first) regardless of which source slide it came from. Columns: line, delta, action item, type (Unexplained gap / Follow-up on explained). This is the slide a reader forwards to someone else to actually go do the work — it should stand alone without needing the rest of the deck for context.

## Slide 5 — Excluded (immaterial)

A single stated count ("187 lines below the $30K threshold were excluded"), not a table. Optionally the combined dollar magnitude of what was excluded, so the reader can sanity-check that the threshold isn't hiding something large in aggregate even though no single line qualifies.

## Deriving an action item from an explanation

The rule: the action item is what the RECORD says should happen next, never what you'd infer is sensible given the topic. Read the actual recorded text (the insight's content, the answered question's reply, the task's confirmed driver) for an explicit forward-looking statement.

| Recorded explanation says... | Action item |
| --- | --- |
| "One-time — a single reversed order, no further action" | "No action — explanation is definitive" |
| "Rent savings already built into the plan going forward" | "No action — explanation is definitive" |
| "Pending the Q2 vendor renegotiation" | "Confirm the Q2 vendor renegotiation lands before the next roll" (only if the record actually says this — quote or closely paraphrase it, don't add detail it doesn't contain) |
| "Managed as one basket with accepted net offsets, no material changes planned" | "No action — explanation is definitive" |
| Names a cause but doesn't say whether it recurs or is resolved | State that ambiguity explicitly as the action item ("Confirm with `<owner>` whether this recurs next roll — not stated in the record"), rather than picking optimistic or pessimistic by default |

Do not:
- Invent a follow-up for a closed, one-time explanation just to give every row an action — "no action" is a correct, common, and useful answer.
- Credit a note that's topically adjacent but doesn't actually cover this movement's dimensions or period (see `references/scoring-playbook.md`) — that line stays hanging, not explained-with-an-action-item.
- Turn a hedge inside the record itself (e.g. "likely offset elsewhere, unconfirmed") into a confident action item — carry the hedge through into the action item's wording rather than smoothing it away.
