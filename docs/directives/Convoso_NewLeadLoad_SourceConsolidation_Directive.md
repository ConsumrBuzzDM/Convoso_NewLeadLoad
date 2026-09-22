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

1. **Evidence already gathered (2026-09-22), start from this rather than
   re-deriving it:** `git log --oneline --all` shows `9e240b4 feat: Add
   Convoso Auto Create Lead extension with disposition monitoring and
   auto-click functionality` — this is not an incidental edit to a stray
   file, it's a deliberate commit adding a second, self-contained,
   independently-named extension (its own `manifest.json` names it
   "Convoso Auto Create Lead," distinct from the root's "Contact Center
   Productivity Add-On"). Every real feature commit since
   (`c1b121f`, `be5fbe3`) touched only that folder. The root `src/`
   folder's last real touch predates the subfolder's creation. This is
   strong circumstantial evidence `convoso-auto-create-lead/` is the
   live, actively-developed one and root `src/` is superseded — but it
   is not proof of what's actually loaded in Robert's browser right
   now, which nothing in git can establish with certainty.
2. Check for any additional indication (a README section, a note) beyond what's
   summarized above.
3. Whichever folder the evidence points to: update `AGENTS.md` to state
   plainly which folder is *believed* canonical (cite the evidence),
   remove the "verify before editing" hedge, and add a clearly-marked
   `**OPEN QUESTION for Robert:**` line asking him to confirm which
   folder `chrome://extensions` actually has loaded — do not word this
   as settled fact merely because the git evidence is strong.
4. Do not delete the non-canonical folder without Robert's explicit
   go-ahead (it may still hold reference value or be mid-transition) —
   flag it clearly in `AGENTS.md` as probably-legacy/not loaded, so the
   ambiguity can't recur for the next person or agent.
5. **This is a headless, non-interactive dispatch — there is no live
   channel to "ask Robert" and get an answer back.** If the evidence
   above still leaves it genuinely undeterminable, do not block waiting
   for a response: write the open question clearly into `AGENTS.md` (per
   step 3) and into your completion report, and stop there. Never guess
   past what the evidence supports, and never treat "I can't get a live
   answer" as license to just pick one.

## What NOT to do

- Do not merge, delete, or rewrite either folder's code as part of this
  directive — this is an investigation-and-documentation pass, not a
  refactor. A follow-up directive can consolidate once the live one is
  confirmed.
- Do not change extension permissions or manifest fields beyond what's
  needed to answer the question.

## Completion criteria

- [ ] `AGENTS.md` updated: states which folder the evidence points to (cite it), with an explicit `**OPEN QUESTION for Robert:**` line asking him to confirm against what's actually loaded in his browser — not worded as settled fact
- [ ] The non-canonical folder flagged clearly as probably-legacy, not silently removed
- [ ] No code merged, deleted, or rewritten in either folder
- [ ] Completion report restates the open question plainly, does not claim it was resolved if it wasn't

<!-- queue:start -->
## Queue

| Field | Value |
|---|---|
| Status | Done |
| Assigned to | devin |
| Branch | directive/convoso-newleadload-convoso-newleadload-sourceconsolidation |
| Base branch | main |

**Status log**
- 2026-09-22 · robert-claude · none → Draft — found via repo-triage research pass: root manifest.json points at src/, but every one of the last 5 commits (real feature work) only touched the separate, unreferenced convoso-auto-create-lead/ folder. AGENTS.md already flags this ambiguity but never resolves it.
- 2026-09-22 10:06 · robert-claude · Draft → Queued — Fixed for headless dispatch safety (d454ce2): rewrote the "ask Robert" step to record an open question instead of blocking, added git-evidence pointing at convoso-auto-create-lead/ as likely-canonical. Robert: full bypass auto-approval.
- 2026-09-22 10:06 · robert-claude · Queued → Approved
- 2026-09-22 10:06 · dispatcher · Approved → In progress — dispatched devin in C:\Github\.worktrees\Convoso_NewLeadLoad--convoso-newleadload-convoso-newleadload-sourceconsolidation
- 2026-09-22 10:09 · devin · In progress → Review — AGENTS.md updated (commit cbb4c40): convoso-auto-create-lead/ stated as believed canonical with cited evidence (commit 9e240b4 created it as self-contained Convoso-scoped extension; every commit since touched only it; its own README instructs Load Unpacked at that folder; root src/ untouched since 2025-11-21). Root src/ + root manifest.json flagged probably-legacy, nothing deleted or modified in either folder. OPEN QUESTION for Robert recorded in AGENTS.md and restated here: which folder does chrome://extensions actually have loaded via Load Unpacked — repo root (src/) or convoso-auto-create-lead/? Not resolved; git evidence cannot prove it.
- 2026-09-22 10:15 · robert-claude · Review → Done
<!-- queue:end -->
