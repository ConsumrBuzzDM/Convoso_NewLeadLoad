# Convoso_NewLeadLoad - Direction

*Drafted 2026-09-22 by devin (Tower) from the repo's own docs - needs Robert's review before it is treated as intent.*

## Purpose

Chrome MV3 extension closing one operational gap: after an agent dispositions a
call, Convoso doesn't always auto-load the next lead - this watches for the
stall and auto-clicks "Create Lead".

## Current state

Small, stable, single-purpose. No build/tests - manual Load Unpacked + real
disposition flow is the bar. Two source trees coexist; `convoso-auto-create-
lead/` is believed canonical (real feature work landed there late 2025). The
source-consolidation investigation (2026-09-22) is Done and documented.

## Next steps

1. Robert confirms which folder `chrome://extensions` actually loads -> then
   retire the dead tree and simplify AGENTS.md.
2. Optional minimal smoke-test harness (Puppeteer/Playwright fixture) - a
   judgment call, gated on Robert.

## Definition of done

One canonical source tree; agents never again guess which folder is live.

## Do not

- No CI/tests - "done" means "matches manual verification on a real CRM flow";
  don't claim more.
- No build step - editing source changes what loads directly.
- Don't delete a source tree before the chrome://extensions answer lands.

## Sources of truth

- `AGENTS.md` (canonical-folder section), `docs/ROADMAP.md` (yaml roadmap
  block), the consolidation directive (Done).
