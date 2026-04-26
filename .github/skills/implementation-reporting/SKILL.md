---
name: implementation-reporting
description: "Required output format for implementation summaries when completing feature work or opening PRs."
---

# Implementation Reporting

## Required Output

Every implementation summary (in PR descriptions and handoff reports) must include:

### What Changed

Group changes by area:

- **Backend** — server / API changes
- **Frontend** — UI / client changes
- **Database** — migrations and type updates
- **Config** — environment, CI/CD, or infrastructure changes

### Why Each Change Was Needed

Brief justification linking each change back to acceptance criteria or plan steps.

### Verification Performed

- Commands run (e.g., `npm test`, `pytest`)
- Pass/fail counts
- Any manual checks performed
- Test failures with context (even if unrelated to your changes)

### Known Limitations or Follow-ups

- Edge cases not covered
- Deferred work
- Dependencies on other user stories
