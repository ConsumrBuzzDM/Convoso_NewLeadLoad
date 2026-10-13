# Convoso_NewLeadLoad: reconcile DIRECTION.md with what this repo actually is

**Assignee:** devin. **Depends on:** nothing (docs-only). Verified against
`origin/main` f97014b on 2026-10-12.

## 1. Why this exists

`docs/DIRECTION.md` contradicts itself. Its prose (drafted 2026-09-22 from the
repo's own files) says this repo is a Chrome MV3 extension that auto-clicks
"Create Lead" when Convoso fails to auto-load the next lead after a
disposition. The fenced ```` ```yaml direction ```` block appended on
2026-09-25 (`answered: ... robert-claude-tower`, commit f97014b) says the repo
"Automates importing prepared lead lists into the Convoso dialer", with
`done_when: Imports land with correct fields and lists, with counts verified`
and `do_not: Never mutate the live dialer without an explicit order`.

The code says the prose is right: `convoso-auto-create-lead/manifest.json` and
root `manifest.json` are the only manifests, there is no upload, CSV or
network-call code anywhere (a case-insensitive grep for csv, upload, fetch( and
XMLHttp over `src/` and `convoso-auto-create-lead/` finds nothing), and the
workspace repo index describes it as "Chrome extension: auto-advances to the
next lead after a disposition closes." The yaml block looks
like it was answered from the repo name ("NewLeadLoad"), not the repo.

Because RepoMan and the refill pass read the yaml block as Robert's intent,
leaving it wrong would steer future directives toward building a live-dialer
lead importer here. That is a high-stakes mistake in the employer's Convoso, so
the contradiction is fixed before anything else is queued.

## 2. Scope

Edit **only** `docs/DIRECTION.md`:

1. Rewrite the fenced ```` ```yaml direction ```` block so it describes the
   extension, keeping the same keys and the same fence/tag:
   - `purpose`: one line, taken from the prose Purpose section (auto-click
     "Create Lead" when Convoso does not auto-load the next lead after a
     disposition).
   - `done_when`: one line matching the prose "Definition of done"
     (one canonical source tree; agents never guess which folder is live).
   - `do_not`: carry over the prose "Do not" bullets (no CI/tests claims, no
     build step, do not delete a source tree before the `chrome://extensions`
     answer lands) and keep "never run against or mutate live Convoso without an
     explicit order".
   - Leave `audience`, `hours_per_week`, `stakes` exactly as they are: those are
     answers, not facts this run can check. Change `answered:` to
     `2026-10-12 repoman (reconciled; needs Robert's confirmation)`.
2. Add one line directly under the italic "Drafted ..." header line:
   `*Reconciled 2026-10-12: the yaml block previously described a lead-import tool, which this repo is not. Needs Robert's review.*`
3. Do not touch the prose sections beyond that one line, and do not touch any
   other file.

## 3. What NOT to do

- Do not edit `docs/ROADMAP.md`, `AGENTS.md`, any extension source, or either
  `manifest.json`.
- Do not delete or move either source tree (that is roadmap step M1.1 and waits
  on Robert's `chrome://extensions` answer).
- Do not run, load or exercise the extension against Convoso; no network.
- Do not invent audience/hours/stakes answers.

## 4. Verification

No test suite exists in this repo (measured baseline: 0 tests, no runner;
`node --check` syntax pass on all three `convoso-auto-create-lead/*.js` files at
f97014b on 2026-10-12). The gate is read-only text checks:

```
git diff --stat origin/main -- .
git diff --name-only origin/main -- .
grep -c "importing prepared lead lists" docs/DIRECTION.md
grep -c "auto-click" docs/DIRECTION.md
node --check convoso-auto-create-lead/content.js
```

Expected: `--name-only` lists exactly `docs/DIRECTION.md` (plus this directive's
own queue block if the dispatcher edits it); the first `grep -c` prints `0`;
the second prints a number of at least 1; `node --check` exits 0.

## 5. Rules for this run

- Work on the dispatched branch only; never push to main.
- Surgical change: keep every other byte of `docs/DIRECTION.md` unchanged.

## 6. Completion criteria

- [ ] The yaml direction block describes the extension; the phrase "importing prepared lead lists" no longer appears in `docs/DIRECTION.md`
- [ ] `audience`, `hours_per_week`, `stakes` unchanged
- [ ] Only `docs/DIRECTION.md` changed
- [ ] Report restates that Robert must confirm the reconciled block

## Forbidden Actions

- Never push to main/master, deploy, or mark anything Done.
- Never run the extension against live Convoso or make any network call.
- Never edit the queue, `agentflow.toml`, or any other repo.

## Required from User

- Robert confirms the reconciled direction block, and answers the open
  `chrome://extensions` question (root `src/` or `convoso-auto-create-lead/`),
  which gates roadmap step M1.1.

## Sandbox needs

- Exec(git diff)
- Exec(git status)
- Exec(grep)
- Exec(node --check)

<!-- queue:start -->
## Queue

| Field | Value |
|---|---|
| Status | Draft |
| Assigned to | devin |
| Depends on | - |
| Branch | - |
| Base branch | main |

**Status log**
- 2026-10-12 21:41 · claude-tower-repoman · none → Draft — authored in RepoMan visit; DIRECTION.md yaml block contradicts the prose and the code
<!-- queue:end -->
