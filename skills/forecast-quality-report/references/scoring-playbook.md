# Scoring Playbook

Evidence standard for marking a material movement "explained", how to handle edge cases, and how to grade the explanations you find.

## Evidence standard per source type

| Source | Counts as explained when | Does NOT count |
| --- | --- | --- |
| Insight | An insight on file names a cause for this slice, covering the window between the two versions | An insight about the same account/department but a different period, or a different slice |
| Answered question | A tracked question about this slice has a human reply that names a driver (state `answered` or `resolved`) | A question that is `asked` with no reply yet — that is a gap with an ask already in flight; a reply that names no driver ("checking on it") |
| Task driver | A driver hypothesis on a task was confirmed by a human, not merely proposed | An unconfirmed/unjudged driver guess — treat as no record |

Take the most specific and most recent record when several exist. Conflicting records: report both, mark the line explained-but-disputed, and make the action "reconcile".

## Matching a record to a movement

A record must cover the SAME dimensions (or an ancestor of them) and a period overlapping the two versions. A record for a different cost center, or a period outside the window, does not explain this movement however similar the topic. Say specifically why it falls short ("covers Herzliya; the movement is Property Rent Taxes") and keep the line in the gaps.

## Partial coverage

When a record explains part of a movement, the remainder is still a gap: report the explained portion, the residual amount, and score the line by the majority. State the residual — never round a partial to "resolved".

## Direction

Costs: up = **Risk** (unfavorable), down = **Opportunity** (favorable). Revenue (only when the user includes it): inverted. Timing moves keep their direction label but carry "timing" so they aren't read as level changes.

## Explanation quality — the four marks

Grade every explanation on record, one star per mark met:

| Mark | Met when the record… | Typical "next time" |
| --- | --- | --- |
| **Cause** | names the specific event or decision (a vendor, a contract, a hire, a policy), not a category | "name the event, not the account" |
| **Amount** | states how much of the movement it covers (a figure or "all of it") | "add the amount it explains" |
| **Recurrence** | says whether it is one-time, timing, or a new run-rate | "say if this repeats next roll" |
| **Owner or timing** | names who owns it or when it lands/reverses | "add who owns it / which month" |

- ★★★★ — all four. Write "Model explanation." in Next time.
- ★★★ — three; the missing one is the Next time.
- ★★ — two; Next time names the more important of the two missing (Amount or Recurrence first).
- ★ — cause only, or a hedge ("likely", "probably") with nothing firm. Next time: the firmest single addition.

Grading rules:
- Grade what is written, not what you infer. A confident tone earns nothing; a stated figure does.
- Carry a hedge through: a "likely offset elsewhere" is graded as unconfirmed on that mark and the action asks to confirm it.
- The coaching line is one clause, constructive, and names the mark — never the person. It is the same line whether the explainer is the CFO or an analyst.
- Credit first: when the explanation is ★★★ or better, the By cell or the coaching line acknowledges it plainly ("clear and quantified").

## What NOT to do

- Do not infer a driver from the numbers (a headcount ramp implied by a trend is a hypothesis, not a record) — it belongs in the gaps with the ask, never in explained.
- Do not let one well-explained large line make a slice look scored — report at the line level the scope names.
- Do not drop a movement that has no matching record — every material movement appears as explained or as a gap; there is no third bucket.
- Do not soften a gap. "No explanation on record" is the correct, useful cell — it is what gets a reason written.
