---
name: spec-writing
description: "Template and workflow for writing product specs and decomposing features into user stories. Use when creating or updating a spec file in .github/specs/."
---

# Spec Writing

## Decomposition

Break a requirement into the right number of features based on:

- **Independence** — Can sub-parts be built, tested, and shipped separately?
- **Scope boundaries** — Does a single spec cover too many moving parts?
- **Pipeline isolation** — Do different parts touch different systems?
- **Risk isolation** — Should risky/uncertain parts be separated from straightforward changes?

### Decomposition Workflow

1. **Analyze** — Read the user's request and identify the distinct capabilities or changes needed.
2. **Propose a breakdown** — Present the user with a numbered list of features you plan to spec out, with a one-line summary each. Explain *why* you're splitting (or not splitting) this way.
3. **Get alignment** — The user confirms the breakdown before you write any specs.
4. **Draft specs** — Create one markdown file per feature at `.github/specs/<slug>.md`.
5. **Review** — Present the spec to the user. Explicitly ask: "Does this spec look good, or do you want changes?" Do NOT proceed until the user explicitly approves.
6. **Iterate** — If the user requests changes, apply them and re-present.
7. **Ship** — Once approved, create GitHub issues (Feature parent + Task sub-issues). Only after issues are created, offer the handoff to the Tech Lead.

## Spec File Format

Every spec file MUST follow this template:

```markdown
# <Title>

## Summary
One paragraph: what are we building and why.

## User Stories

### US1: <Short title>
**As a** [user type], **I want** [goal] **so that** [benefit].

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

### US2: <Short title>
**As a** [user type], **I want** [goal] **so that** [benefit].

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

## Problem
What's broken or missing today.

## Proposed Solution
How we solve it. Keep it high-level — describe behavior, not implementation.

## Out of Scope
What this does NOT include.

## Success Metrics
How we know this worked. What number moves.

## Technical Notes
Any constraints, dependencies, or architecture hints the engineer should know.

## Effort Estimate
T-shirt size: XS / S / M / L / XL — with brief justification.

## Issue Tracking
<!-- PM agent fills this section after creating GitHub issues -->
| User Story | Issue | Status |
|------------|-------|--------|
| (parent)   | —     | —      |
```
