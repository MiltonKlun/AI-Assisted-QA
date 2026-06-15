# Deferred ledger — decisions with triggers, not forgotten items

> IMPROVEMENT-PLAN Phase 8 (IP-8.1). Some scope was deliberately **not built**.
> This is the difference between "deferred" and "forgotten": each entry has an
> explicit **re-evaluation trigger**, an owner, and a link back to where it was
> specified. Review this list on the existing `/evolve` cadence (every 90 days
> or 10 runs, `docs/evolve-loop.md`); when a trigger fires, re-open the item.

| Item                                 | What it is                                                                                                                             | Deferred until (trigger)                                                                                                                                                    | Source                                |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| **PFI-3** Story-source + CI ports    | Generalize the front door (story source) and CI the way test-management was generalized (a port + adapters).                           | `docs/evidence.md` is published **and** a second story source or CI provider is actually needed by an adopter.                                                              | `plan-poor-fit-improvements.md` PFI-3 |
| **PFI-4** `TestRunnerAdapter`        | Make execution pluggable (Cypress / k6 / manual-only) so non-Playwright teams get the pipeline's value. The biggest architectural gap. | `docs/evidence.md` is published **and** a concrete non-Playwright adopter exists.                                                                                           | `plan-poor-fit-improvements.md` PFI-4 |
| **CE-2** CE vocabulary map           | Map Compound-Engineering terms to this project's primitives.                                                                           | The runner + lite track are adopted by **≥2 coworkers** (i.e. the team is large enough for shared CE vocabulary to matter).                                                 | `plan-compound-engineering.md` CE-2   |
| **CE-4** Ideate/brainstorm front-end | A pre-Analyst "is this worth testing / what should we test first" step (own skills, not the CE plugin).                                | Same: ≥2 coworkers using the flow, and a felt need for front-of-loop prioritization.                                                                                        | `plan-compound-engineering.md` CE-4   |
| **CE-5** Reconcile one compound loop | Ensure `/evolve` stays the single compound mechanism if CE primitives are adopted.                                                     | Triggered only if CE-2/CE-4 are re-opened.                                                                                                                                  | `plan-compound-engineering.md` CE-5   |
| **CE-6** Re-evaluate the CE plugin   | Decide whether to actually install `everyinc/compound-engineering-plugin`.                                                             | A coworker already uses it, **or** the front-of-loop skills (CE-4) prove insufficient. **Never** adopting its Stage-5 autonomy — that contradicts the permanent human gate. | `plan-compound-engineering.md` CE-6   |
| **Dual-judge evaluation** (TG13)     | A second-judge layer over agent evaluation.                                                                                            | Prompt-eval drift or a measured judging-reliability problem the single evaluator can't catch.                                                                               | `docs/dual-judge-evaluation.md`       |

## How to re-open an item

1. The trigger fired — note where (a run, a coworker request, an evidence result).
2. Re-read the source plan entry linked above.
3. If still worth doing, add it as a new phase to `IMPROVEMENT-PLAN.md` (one
   PR off `main`, the usual discipline) — do not just start coding it.
4. If the trigger fired but it's _still_ not worth it, record that decision
   here (update the row) so the next reviewer doesn't re-litigate it.

## Why these are deferrals, not rejections

None of these is forbidden (unlike n8n / dashboards / autonomous batch, which
are permanent no's — `README.md` §9, `STRATEGY.md`). They are **good ideas
without a current trigger**. Building them now would be speculative scope; the
trigger is what turns them from speculation into justified work.
