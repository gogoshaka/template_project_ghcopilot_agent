```markdown
---
name: github-shipping
description: "Workflow for creating GitHub Feature issues and Task sub-issues from approved specs. Use when shipping a spec to GitHub after user approval."
---

# GitHub Shipping

## Overview

Once the user explicitly approves a spec, **create the GitHub issues yourself** using MCP GitHub tools. Do not ask the user to run CLI commands — you call MCP tools directly.

<!-- CUSTOMIZE: Replace with your org/repo -->
All issues are created in the `<your-org>/<your-repo>` repository.

## GitHub Issue Types

This project uses **GitHub Issue Types** to visually distinguish work:

| Issue Type | Used For |
|------------|----------|
| `Feature` | A feature spec (parent issue). Contains summary, problem, solution, and links to child user stories. |
| `Task` | A user story or technical task (sub-issue under a feature). Contains one user story with its acceptance criteria. |
| `Bug` | A defect report. Not part of the spec workflow — created ad hoc. |

Before creating issues, call `mcp_io_github_git_list_issue_types` with `owner: "<your-org>"`, `repo: "<your-repo>"` to discover available issue type names.

## Issue Hierarchy

Each feature becomes a **parent issue**. Each user story becomes a **sub-issue** linked to the parent via `mcp_io_github_git_sub_issue_write`. This enables:

- Independent assignment and tracking per user story
- Coding agents picking up individual sub-issues
- Progress tracking at the feature level via sub-issue completion

## Shipping Workflow

**Step 1 — Discover issue types:**
Call `mcp_io_github_git_list_issue_types` with `owner: "<your-org>"`, `repo: "<your-repo>"`.

**Step 2 — Create the parent feature issue:**
Call `mcp_io_github_git_issue_write` with:
- `method: "create"`
- `owner: "<your-org>"`, `repo: "<your-repo>"`
- `title: "Feat/ <Feature Title>"`
- `body:` contents of the spec file
- `labels: ["<area>"]`
- `type: "Feature"` (if available)

**Step 3 — Create sub-issues for each user story:**
One per user story with acceptance criteria and spec reference.

**Step 4 — Link sub-issues to the parent:**
Call `mcp_io_github_git_sub_issue_write` for each.

**Step 5 — Write issue numbers back to the spec file:**
Update the `## Issue Tracking` table in the spec so downstream agents can discover issues.

**Step 6 — Report back to the user.**

**Step 7 — Hand off to Tech Lead:**
Inform the user that the spec is shipped and ready for the Tech Lead agent to pick up for planning and implementation.

## Sub-Issue Body Format

Each sub-issue body must include:
- The user story ("As a … I want … so that …")
- All acceptance criteria as checkboxes
- A reference to the spec file: `**Spec:** .github/specs/<slug>.md`
- Any dependency notes

## Area Labels

<!-- CUSTOMIZE: Define your project's area labels -->

- `backend` — server / API changes
- `frontend` — UI / client changes
- `database` — schema / migration changes
- `infra` — deployment, CI/CD, DevOps
- `docs` — documentation changes

If a label doesn't exist yet, ask the user to create it.
```
