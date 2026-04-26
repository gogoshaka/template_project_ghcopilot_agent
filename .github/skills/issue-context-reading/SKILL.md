---
name: issue-context-reading
description: "How to navigate the plan/spec/issue hierarchy when picking up work from a GitHub issue or PR. Use when starting work on a sub-issue or reviewing a PR."
---

# Issue Context Reading

## Context Sources

When picking up a sub-issue (user story) or reviewing a PR, read these sources in order:

1. **Sub-issue description** — Contains the user story and acceptance criteria. This is your primary contract.
2. **Parent feature issue** — Provides overall feature context and how this user story fits the bigger picture.
3. **Spec file** (`.github/specs/<slug>.md`) — Full product spec with all user stories, problem description, proposed solution, and technical notes.
4. **Plan section** (`.github/plans/<slug>.md` → `### US<N>`) — Implementation steps, file-level changes, test plan, and definition of done.

## Interpreting Dispatch Context

When dispatched by the Tech Lead via the `task` tool, your prompt will reference:

- **Plan:** path to the plan file and section (e.g., `.github/plans/<slug>.md` → `### US1`)
- **Spec:** path to the spec file (e.g., `.github/specs/<slug>.md`)
- **Agent guide:** path to your agent file (Feature Builder or QA Reviewer)

Read all referenced files before starting work.

## Issue Hierarchy

- **Feature issue** (type: `Feature`) — Parent issue containing the full spec. One per feature.
- **Task sub-issue** (type: `Task`) — One per user story. Contains acceptance criteria. Linked to the parent.
- You may work on **individual sub-issues** rather than an entire feature at once.

## Discovering Issues from the Spec

The PM agent writes all issue numbers into the **`## Issue Tracking`** table at the bottom of the spec file. This table maps user story titles to issue numbers.
