# Investigation Playbook

Tier-by-tier checks and the evidence each conclusion requires.

## Tier 1 — Validity checks

| Check | How to test | If it fails |
|---|---|---|
| Period completeness | Compare the actuals as-of date to the period end | Report as partial-period; annualize or wait for close |
| Plan basis | Confirm which version the plan figures come from | Re-run against the intended basis |
| Exclusions | Apply the organization's standing exclusion list | State which lines were excluded and why |
| NULL handling | Check whether missing rows are being read as zero | Re-pull with explicit null treatment |
| Sign convention | Confirm cost polarity in the source | Re-compute; a sign flip inverts the narrative |
| Double count | Check parent rows are not summed with their children | Re-aggregate from leaf level |

A failure at this tier is the finding. Report it and stop.

## Tier 2 — Decomposition

At each level, compute each child's contribution to the parent variance in currency, not percent. Follow only children above the materiality threshold.

- **One child ≥ 70% of the variance** → concentrated. Continue drilling that branch.
- **No child above ~30%** → spread. Stop drilling and look for a common driver: an allocation rate, a headcount assumption, an FX rate, a shared vendor, a plan-loading error across the group.
- **Offsetting children** → something moved between lines. Look for a reclass before treating either side as real.

## Tier 3 — Price / volume / timing tests

**Timing test.** Compare year-to-date actual vs. year-to-date plan alongside the period figures. Period variance large, YTD variance small → timing. Also check the adjacent months for an equal and opposite movement.

**Volume test.** Where a quantity driver exists (headcount, seats, units, hours), compare actual quantity to planned quantity at the planned unit rate. If quantity explains most of the gap, it is volume.

**Rate test.** Hold quantity at actual and compare actual unit cost to planned unit cost. Rate variances tend to persist, so they matter more for the remaining-period run rate than one-off volume spikes.

**Scope test.** Look for lines, vendors, or cost elements present in actuals with no plan counterpart at all.

Record which tests were run and their results. A conclusion of "timing" with no YTD comparison behind it is an assertion, not a finding.

## Tier 4 — Transaction review

Retrieve transactions for the account and period. Work down from the largest absolute amounts.

Look for:

- **Accruals and reversals** — a large accrual in one period reversing in the next is timing, not overspend
- **Manual journals** — especially late-period ones; check the description and preparer
- **Reclasses** — an equal and opposite entry in another account; confirm both sides before concluding
- **Duplicate postings** — same vendor, amount, and date
- **Catch-up charges** — several periods of a recurring cost booked at once
- **New vendors** — no prior history in this account

Stop once the ranked entries account for the material portion of the gap. State the residual explicitly.

## Evidence standards

| Conclusion | Minimum evidence |
|---|---|
| Timing | YTD comparison plus the offsetting period identified |
| Volume | Actual vs. planned quantity at planned rate |
| Rate | Actual vs. planned unit cost at actual quantity |
| Scope | The unplanned line or vendor named |
| Data artifact | The specific failed validity check |
| One-off event | The transaction or entries, with amounts |

Where evidence is unavailable, report the cause as unestablished and name the owner who would know. Do not close an investigation on a plausible story the data does not support.
