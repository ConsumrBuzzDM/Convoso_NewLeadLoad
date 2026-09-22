# Convoso_NewLeadLoad — Roadmap

*Last updated: 2026-09-22 by robert-claude (Tower)*

## Why these milestones

The source-consolidation investigation already ran (Done) and left one Robert question - which folder chrome://extensions actually loads. The roadmap sequences exactly that: answer, retire the dead tree, then optionally a smoke-test harness.
## Purpose

A Chrome MV3 extension that closes a real operational gap: after an agent
dispositions a call on the Convoso CRM, the UI doesn't always auto-load the
next lead. This extension watches for that stall and auto-clicks "Create
Lead" so agents keep dialing without a manual click every time. It exists
purely to remove that one friction point — nothing more.

## Current maturity

Small, stable, single-purpose. No build step, no automated test suite —
verification has always been manual (Load Unpacked + a real disposition
flow against the live CRM). Two source trees exist in this repo (see
`AGENTS.md` § "Which source folder is canonical"); `convoso-auto-create-lead/`
is believed to be the one actually running, with real feature work landing
there through late 2025/early 2026.

## Phase history

| When | What |
|---|---|
| Early | Root `src/` built — a generic, retargetable extension (`<all_urls>`, configurable selectors) |
| 2025-12-05 | `convoso-auto-create-lead/` added — a second, Convoso-scoped, independently-named rewrite |
| Late 2025–early 2026 | Real feature work (tab-transition detection, button-visibility monitoring, disposition-to-lead flow detection) — all landed in the subfolder only |
| 2026-09-22 | Source-folder ambiguity investigated and documented (`Convoso_NewLeadLoad_SourceConsolidation_Directive.md`, Done) |

## Active / Queued

Nothing currently queued. Check `docs/directives/` / `DirectiveQueueMCP`
for anything newer than this note.

## Candidate next phases

1. **Retire the non-canonical folder.** Blocked on Robert confirming which
   folder `chrome://extensions` actually has loaded (open question in
   `AGENTS.md`). Once answered: if root `src/` is confirmed dead, a small
   follow-up directive can delete it (or move it to an `archive/` folder)
   and simplify `AGENTS.md` back down to a single source tree. Low risk,
   well-scoped, ready to write the moment the question is answered.
2. **A minimal smoke-test harness** — not proposed as urgent, just
   flagging: this repo has zero automated verification, which is exactly
   why the source-folder ambiguity went unnoticed for months. A small
   Puppeteer/Playwright smoke test (load the extension, confirm the
   content script injects and the CONFIG selectors resolve against a
   fixture page) would catch a regression class no manual testing
   reliably does. Not scoped further here — genuinely might not be worth
   the investment for a repo this small; a judgment call for Robert, not
   an agent to decide unilaterally.

## Constraints & gotchas for future dispatches

- **No CI, no automated tests.** A dispatched agent cannot verify runtime
  behavior — "done" here means "matches what's manually checked against
  a real CRM disposition flow," never a green test run. Don't claim more
  certainty than that.
- **No build step.** Editing source files directly changes what loads —
  there's no compile/bundle step to catch syntax errors before runtime.
- This repo has no live-service or sibling-repo dependencies that have
  caused dispatch problems elsewhere in this workspace — nothing special
  to warn about beyond the two points above.

## Open questions

- Which folder does `chrome://extensions` actually have loaded via Load
  Unpacked — root (`src/`) or `convoso-auto-create-lead/`? See `AGENTS.md`
  for the evidence gathered so far. Only Robert can answer this.

```yaml roadmap
status: approved
approved: 2026-09-22 Robert via devin
reviewed: '2026-09-22'
replan_after_days: 14
stop_if: Convoso auto-loads the next lead natively.
milestones:
- id: M1
  title: One canonical source tree
  status: active
  exit:
  - grep:
      path: AGENTS.md
      pattern: canonical
  steps:
  - id: M1.1
    title: Get Robert's answer on which folder is loaded, then retire the dead one
    kind: design
    size: S
    value: 4
    status: pending
    directive: ''
    detail: convoso-auto-create-lead/ is believed canonical; if root src/ is confirmed dead, delete or archive it and simplify AGENTS.md. Low risk, well-scoped - it only waits on the chrome://extensions answer.
    accept:
    - grep:
        path: AGENTS.md
        pattern: canonical
- id: M2
  title: Optional smoke-test harness
  status: pending
  exit:
  - file: test/
  steps:
  - id: M2.1
    title: Add a minimal extension smoke test if Robert wants one
    kind: tests
    size: M
    value: 2
    needs:
    - M1.1
    status: pending
    directive: ''
    detail: Zero automated verification is why the folder ambiguity went unnoticed for months. A Puppeteer/Playwright smoke test would catch injection/selector regressions - genuinely a judgment call whether a repo this small is worth it; gated on Robert.
    accept:
    - file: test/
```
```
