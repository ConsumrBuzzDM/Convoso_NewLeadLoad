# AGENTS.md — Guide for AI Agents Working on Contact Center Productivity Add-On

This file gives AI agents (Claude, Gemini CLI, Devin, Cursor, etc.) the
essential context for working on this repo: project layout, commands,
conventions, and behavioral guidelines. Read this before starting work.
See also `C:\Github\Agents\CONVENTIONS.md` for conventions that apply across
all of Robert's repos.

---

## Project

Chrome Extension (MV3) that automates the transition to a new lead after a
disposition screen closes in a CRM. The repo contains two extension source
trees (see "Which source folder is canonical" below):

- `convoso-auto-create-lead/` — "Convoso Auto Create Lead," scoped to
  `*.convoso.com`; auto-clicks "Create Lead" after a disposition. Believed
  canonical.
- root `manifest.json` + `src/` — "Contact Center Productivity Add-On," a
  generic version on `<all_urls>` with configurable selectors. Probably
  legacy.

- **Language:** JavaScript (Chrome MV3, no build step)
- **Permissions:** canonical: `storage`, `activeTab`,
  `host_permissions: *://*.convoso.com/*`; legacy root: `storage`,
  `scripting`, `host_permissions: <all_urls>`

## Repo layout

```
convoso-auto-create-lead/        believed-canonical extension source (see below)
  manifest.json                  own MV3 manifest — "Convoso Auto Create Lead",
                                 host_permissions scoped to *://*.convoso.com/*
  content.js                     content script — core detection/click logic
  background.js                  service worker (state management)
  popup.html / popup.js          extension popup UI
  styles.css                     popup styling
  README.md                      subfolder docs — instructs Load Unpacked at
                                 this folder
manifest.json                    root MV3 manifest ("Contact Center
                                 Productivity Add-On") — probably legacy
src/                             root extension source — probably legacy
  background/service-worker.js   background service worker
  content/content.js             content script
  options/                       popup, settings page, help docs
docs/                            SDD + agent directives
```

### Which source folder is canonical

`convoso-auto-create-lead/` is **believed canonical** — the extension Robert
actually develops and most likely has loaded via Load Unpacked. Evidence:

- Commit `9e240b4` (2025-12-05) deliberately added it as a self-contained,
  independently-named extension ("Convoso Auto Create Lead") scoped to
  `*://*.convoso.com/*` — the CRM this repo actually targets.
- Every commit since (`c1b121f`, `be5fbe3`, `31d5940`, `502f537`) touched
  only that folder, including real feature work.
- Its own `README.md` instructs "Load unpacked → select the
  `convoso-auto-create-lead` folder."
- Root `src/` was last touched 2025-11-21 — before the subfolder existed.

The root `manifest.json` + `src/` ("Contact Center Productivity Add-On") are
**probably legacy**: not referenced by the actively-developed extension and
not updated since the subfolder was created. Kept in place pending Robert's
confirmation — do not delete, but do not land new work there unless Robert
says the root extension is what's actually loaded.

**OPEN QUESTION for Robert:** which folder does `chrome://extensions`
actually have loaded via Load Unpacked — the repo root (which loads `src/`)
or `convoso-auto-create-lead/`? Git history can't prove this; please confirm
so the probably-legacy copy can be retired.

## Commands

No build step or automated test suite was found. Load unpacked via
`chrome://extensions/` → Developer mode → Load unpacked, pointed at
`convoso-auto-create-lead/` (believed canonical — see above), to test
changes.

## Conventions

- **`convoso-auto-create-lead/` (believed canonical):** Convoso-specific by
  design — selectors and timing live in the `CONFIG` block at the top of
  `content.js`; host permissions are scoped to `*://*.convoso.com/*`.
  Keep new detection logic in that `CONFIG`-driven pattern.
- **Root `src/` (probably legacy):** built as a generic, retargetable
  extension — CSS selectors are configuration, and `<all_urls>` host
  permission is broad by design. Don't narrow it without confirming intent;
  it would break the "any CRM" use case if this copy is ever revived.

---

## Commit conventions

- **Commit every change** with a descriptive message focused on why, not
  just what.
- **Push to GitHub** after committing — Robert reviews after the fact, not
  gating each push.
- **Do not amend prior commits. Do not force-push.** The git history is an
  audit trail, not a draft — once pushed, it's immutable. Local-only commits
  that haven't been pushed yet may be amended freely.
- **Robert also commits directly himself**, interleaved with agent commits.
  This is expected behavior, not an anomaly.

## Never trust agent completion summaries

Agent-reported completion is a hypothesis until independently verified
against actual pasted output. Concretely:

- Check `git status` and `git diff` before committing to verify what
  actually changed.
- Check `git reflog` when verifying a prior session's claimed work, not
  just `git log`.
- Where a finding corrects an earlier assumption, the correction is made
  **in place, marked explicitly** ("SUPERSEDED," "CORRECTED"), not silently
  edited.

## Behavioral guidelines

### Think before coding

- State assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.

### Simplicity first

- Minimum code that solves the problem. Nothing speculative.

### Surgical changes

- Touch only what you must. Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.

### Goal-driven execution

- "Fix the bug" → reproduce via Load Unpacked against a real disposition
  flow, then verify the fix the same way — there's no automated suite.

## Documentation files

- `README.md` — overview of the root extension (probably legacy)
- `convoso-auto-create-lead/README.md` — docs for the believed-canonical
  extension
- `docs/Software Design Document (SDD)_ Contact Center Productivity
  Add-On.md` — SDD for the root extension
- `AGENTS.md` — this file

## Common pitfalls

1. **Two source folders, one believed canonical.** Edit
   `convoso-auto-create-lead/` for real work; root `src/` + root
   `manifest.json` are probably legacy (see "Which source folder is
   canonical" above). Don't delete either until Robert confirms what's
   loaded in his browser.
2. **No automated tests.** Manually verify against a real CRM disposition
   flow before calling a change done.
