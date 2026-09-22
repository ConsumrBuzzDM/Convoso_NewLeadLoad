# Convoso_NewLeadLoad — Resolve the Dual Source-Folder Ambiguity

## Context

`AGENTS.md` flags this as a known pitfall ("Two possible source locations
... verify before editing") but never resolves which one is actually
canonical — and the ambiguity is real, not just theoretical. The root
`manifest.json` (what `chrome://extensions` → Load Unpacked loads by
default at the repo root) points at `src/background/service-worker.js`,
`src/content/content.js`, `src/options/*` — but every one of the five
most recent commits (`502f537`, `31d5940`, `be5fbe3`, `c1b121f`,
`9e240b4`), including real feature work ("Improve tab transition
detection and button visibility monitoring," "Improve disposition-to-lead
flow detection"), touched only `convoso-auto-create-lead/` — a separate
folder with its own independent `manifest.json` that the root manifest
never references.

Two real possibilities, and it matters which:
1. `convoso-auto-create-lead/` has become the actual extension Robert
   loads (by pointing "Load Unpacked" at that subfolder directly), and
   `src/` at the root is now dead code nobody runs. If so, the root
   `manifest.json` and `src/` are misleading clutter.
2. Recent development has been landing in the wrong folder and never
   actually reaches what the root manifest loads — i.e., real bug fixes
   from the last several commits may not be live anywhere Robert is
   actually testing from.

## Scope

1. Determine which folder is actually in use. Check for any indication
   (a README section, a note, or ask Robert directly if it can't be
   determined from the repo alone) of which one gets loaded via
   "Load Unpacked" in practice.
2. Whichever is confirmed live: update `AGENTS.md` to state plainly
   which folder is canonical and remove the "verify before editing"
   hedge — replace it with a direct statement.
3. Whichever is confirmed dead: do not delete it without Robert's
   explicit go-ahead (it may still hold reference value or be
   mid-transition) — but flag it clearly in `AGENTS.md` as legacy/not
   loaded, so the ambiguity can't recur for the next person or agent.
4. If genuinely undeterminable from the repo alone, stop and ask Robert
   directly rather than guessing — this is exactly the kind of thing
   AGENTS.md's own "Think before coding" section calls for.

## What NOT to do

- Do not merge, delete, or rewrite either folder's code as part of this
  directive — this is an investigation-and-documentation pass, not a
  refactor. A follow-up directive can consolidate once the live one is
  confirmed.
- Do not change extension permissions or manifest fields beyond what's
  needed to answer the question.

## Completion criteria

- [ ] Confirmed which folder (`src/` or `convoso-auto-create-lead/`) is actually loaded/live
- [ ] `AGENTS.md` updated to state this plainly, replacing the current hedge
- [ ] The non-canonical folder flagged clearly as legacy, not silently removed
- [ ] If undeterminable, the question is posed to Robert explicitly rather than guessed

<!-- queue:start -->
## Queue

| Field | Value |
|---|---|
| Status | Draft |
| Assigned to | devin |
| Branch | - |
| Base branch | main |

**Status log**
- 2026-09-22 · robert-claude · none → Draft — found via repo-triage research pass: root manifest.json points at src/, but every one of the last 5 commits (real feature work) only touched the separate, unreferenced convoso-auto-create-lead/ folder. AGENTS.md already flags this ambiguity but never resolves it.
<!-- queue:end -->
