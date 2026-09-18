# Scoring Playbook

Evidence standard for marking a material movement "explained", and how to handle the edge cases.

## Evidence standard per source type

| Source | Counts as explained when | Does NOT count |
| --- | --- | --- |
| Insight | An insight on file names a cause for this slice, covering the period the movement occurred in | An insight about the same account/department but a different, unrelated period or a different slice entirely |
| Answered question | A tracked question about this slice was actually answered (not just asked — an open or unanswered question is hanging) | A question that was sent but has no reply yet, or was answered with something that doesn't actually name a driver ("checking on it") |
| Task driver | A driver hypothesis on a task was confirmed, not merely proposed | An unconfirmed/unjudged driver guess — treat it the same as no record |

If more than one record exists for the same slice, use the most specific and most recent one; do not average or combine conflicting explanations — flag the conflict instead and report both, noting the movement's status as explained-but-disputed.

## Matching a record to a movement

A record must cover the SAME dimensions (or a parent/ancestor of them) and a period that overlaps the two forecast versions being compared. A record for a different cost center, or for a period entirely outside the window, does not explain this movement even if the topic sounds similar — do not stretch a loosely related record to cover a gap.

## Partial coverage

When a record explains only part of a line's movement (e.g. a driver is recorded for a subset of the months in scope, or for one component of a blended line), the movement is still "hanging" for its residual — report the explained portion and the remaining unexplained amount separately, the same way variance-investigation always states what remains unexplained rather than rounding up to "resolved."

## What NOT to do

- Do not infer a driver from the shape of the numbers alone (a big vendor invoice pattern, a headcount ramp implied by the trend) — that is a hypothesis, not a record, and belongs in "hanging" with a note suggesting where to look, never in "explained."
- Do not let a single well-explained large line make an otherwise-hanging slice look scored — report at the line level the report's scope specifies, not a blended parent-level score that hides children with no record.
- Do not silently drop a movement that has no matching record — every material movement must appear in either the explained or hanging list; there is no third, ignored bucket.
