# Phase 2 — Retrospective

> **Status:** WRITTEN (2026-06-04), pending team review. This file's
> existence + review is the final Phase 2 completion criterion
> (`phase2-integrations.md` §6). Phase 3 does not start until this is
> reviewed.

---

## 1. Stories processed

Three Jira-sourced (`SK`) stories ran end-to-end through the full pipeline
(Analyst Mode B → Gates 1–4 → execution → classification → report → PR →
CI → merge).

| Story | Title                          | Branches    | TCs | Result | PR  |
| ----- | ------------------------------ | ----------- | --- | ------ | --- |
| SK-10 | valid user logs in             | E2E         | 3   | pass   | #2  |
| SK-13 | sort products by name & price  | E2E         | 5   | pass   | #3  |
| SK-16 | complete checkout              | E2E         | 3   | pass   | #4  |

All 11 generated tests passed live; 0 failures, 0 bug drafts across all
three. Every spec/test was authored against the live app via the
playwright-test MCP (§3.8 — nothing generated from text alone).

**On the API branch:** all `SK` stories target Saucedemo, which is
front-end-only (no real backend API). Forcing an API branch onto them would
mean inventing endpoints (a §3.8 violation), so all three ran E2E-only by
deliberate decision. The dual E2E+API capability was already proven
end-to-end earlier by **STORY-002** against reqres.in (Newman 5/5, in CI) —
so Phase 2's API path is demonstrated, just not by these three SK stories.

---

## 2. CI friction points

- **First-run setup** was the main one-time cost: no git history/remote
  existed, the old repo had to be force-overwritten, and `quality-checks`
  only became selectable as a required check **after** its first run (a
  bootstrap PR, #1, solved this).
- **Line endings (CRLF):** Windows checkout produced CRLF locally while git
  stored LF, so `prettier --check` warned locally even though CI (Linux/LF)
  passed. Fixed permanently by adding `.gitattributes` (`eol=lf`) +
  `core.autocrlf false` during the SK-13 slice. No recurrence after.
- **`newman-api`** failed on the very first PR until the `REQRES_API_KEY`
  GitHub **secret** was added (the local `.env` key isn't visible to CI).
  It's informational, so it never blocked — correct by design.
- Otherwise CI was stable: `quality-checks` ~25–30s, full Playwright
  ~1m20s, every required check green on every story PR.

---

## 3. Time per story (qualitative)

Wall-clock wasn't instrumented precisely, but the shape was consistent:
the **gates + agent steps were fast**; the cost concentrated in (a) the
one-time setup and (b) the live MCP exploration during planning/generation
(worth it — it's what kept the tests real and green first-try). SK-13/SK-16
(5 and 3 TCs with multi-step flows) were not meaningfully slower than SK-10
once the loop was warm.

---

## 4. Are the gates still useful?

- **Gate 2** earned its keep: it's where the E2E-vs-other-level judgment
  happens. We confirmed 100% E2E was *correct* for these stories (all are
  genuine UI behavior with no lower seam) rather than a lazy default —
  exactly the discrimination the gate exists for.
- **Gate 1** caught the right things: thin Jira ACs were split verbatim and
  the implicit preconditions (SK-16 needs login + a cart item) were surfaced
  as ambiguities instead of invented requirements.
- **Gate 4** stayed valuable: the Playwright lint rule
  (`prefer-web-first-assertions`) forced a more robust assertion style on
  SK-13 before the human even reviewed — a good example of Gate-4-adjacent
  tooling raising quality.
- **G1 and G2 were always approved together** across all three stories with
  no divergence. That's 3 data points toward the TG7 `qa_scope_approved`
  consolidation — not yet the 10+ the doc suggests, but trending that way.

---

## 5. TestLink sync

Not exercised during these three SK slices (the optional `--apply-testlink`
steps were skipped to keep the slices focused on the gate flow + CI). The
sync path itself was **live-verified during TG4/TG10** against the local
TestLink container (4 STORY-002 cases created, `testlink_id` written back,
execution-status mapping confirmed). Recommendation: run one SK story through
`--apply-testlink` + `--apply-testlink-execution` as a confidence check
early in Phase 3.

---

## 6. Jira bug creation

No Red failures occurred (all 11 tests passed), so no bugs were promoted
during TG13. The promotion path was **live-verified during TG5** (SK-42
created via `create-jira-bugs.js --apply`, then deleted), including the
de-dup-on-filled-key behavior. The optional Mode-B "pipeline started"
comment was available but not used.

---

## 7. Recommendations for Phase 3 (controlled healing)

- **Single-occupancy artifacts hurt across stories.** `context.json`,
  `analysis/`, `release/` each hold one run, so each story overwrites the
  last (and a stray `git checkout -- .` during the SK-13 slice briefly
  clobbered the shared files — recovered, but it proved the fragility).
  This is the concrete motivation for the Phase 3 `runs/[story-id]/[run-id]/`
  history layout — prioritize it.
- **Healer would have had nothing to do here** (0 failures), but the
  Green-only guardrail is the right starting scope; the locators we used
  (`[data-test=...]`) are exactly the stable kind a Green locator-fix
  Healer should target.
- **Evaluation harness** (`npm run evaluate`) could start scoring the SK
  outputs, not just the example dataset.
- Consider the **TG7 gate consolidation** after a few more runs confirm G1/G2
  never diverge.

---

## 8. Phase 2 completion checklist

- [x] Jira story ingestion (Mode B) worked — SK-10/13/16.
- [x] Manual story input (Mode A) still works — unchanged from Phase 1/1.5.
- [x] TestLink test-case sync works with `--apply` — verified TG4.
- [x] TestLink execution-result sync works with `--apply` — verified TG10.
- [x] GitHub Actions ran `quality-checks` blocking on PRs.
- [x] Playwright + Newman reports uploaded as CI artifacts.
- [x] Jira bugs creatable from drafts, duplicate-safe — verified TG5.
- [x] Review audit fields available (TG6) — used on every gate this slice.
- [x] Gate consolidation option available (TG7) — backward-compatible.
- [x] Evaluation dataset runs (`npm run evaluate`).
- [x] 3+ stories processed end-to-end with Jira as source.
- [ ] This retrospective reviewed by the team.

---

## 9. Parked for after Phase 2 (proposed TG14)

Jira write-back for test cases / stories (Option A copy-paste/CSV export;
Option B automatic `--apply` creation) — to be evaluated now that TG13 is
done. See `phase2-integrations.md` §8.
