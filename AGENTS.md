# AGENTS.md — Guide for AI Agents Working on Contact Center Productivity Add-On

This file gives AI agents (Claude, Gemini CLI, Devin, Cursor, etc.) the
essential context for working on this repo: project layout, commands,
conventions, and behavioral guidelines. Read this before starting work.
See also `C:\Github\Agents\CONVENTIONS.md` for conventions that apply across
all of Robert's repos.

---

## Project

Chrome Extension (MV3) that automates the transition to a new lead after a
disposition screen closes in a CRM. Monitors the CRM interface and
auto-clicks "New Lead" if one doesn't load automatically after a call
disposition. Runs on `<all_urls>` via a content script — CSS selectors and
timing are configurable to match different CRM layouts.

- **Language:** JavaScript (Chrome MV3, no build step)
- **Permissions:** `storage`, `scripting`; `host_permissions: <all_urls>`

## Repo layout

```
manifest.json                    MV3 manifest
src/
  background/service-worker.js   background service worker
  content/content.js             content script — core detection/click logic
  options/
    popup.html / popup.js        extension popup UI
    options.html / options.js    settings page
    help.html                   help documentation
  assets/                        icons (16/48/128)
docs/SDD.md                      Software Design Document
convoso-auto-create-lead/        possible alternate/legacy source copy — verify before editing
```

## Commands

No build step or automated test suite was found. Load unpacked via
`chrome://extensions/` → Developer mode → Load unpacked to test changes.

## Conventions

- **CSS selectors are configuration, not hardcoded logic.** The extension
  is designed to be retargeted at different CRMs via configurable
  selectors and timing delays — keep new detection logic consistent with
  that pattern rather than hardcoding a single CRM's DOM.
- `<all_urls>` host permission is broad by design (it needs to work on
  whatever CRM the operator points it at) — don't narrow it without
  confirming that's intended, since it would break the "any CRM" use case.

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

- `README.md` — overview, features, project structure, installation
- `docs/SDD.md` — Software Design Document
- `AGENTS.md` — this file

## Common pitfalls

1. **Two possible source locations** (`src/` at root vs.
   `convoso-auto-create-lead/`) — confirm which is the active extension
   source before editing.
2. **No automated tests.** Manually verify against a real CRM disposition
   flow before calling a change done.
