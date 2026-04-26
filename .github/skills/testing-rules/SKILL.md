---
name: testing-rules
description: "Standards for running tests, reporting results, and handling test failures during implementation and review."
---

# Testing Rules

## Running Tests

1. **Always run existing tests** after implementation to verify no regressions — even if the plan says "no new tests needed."
2. **Run tests before marking work as complete.** No implementation summary or review verdict can be finalized without test evidence.

## Modifying Tests

1. **Never modify existing tests to make them pass.** If a pre-existing test fails after your changes, the test is correct and your implementation is wrong — fix the implementation code, not the test.
2. The only exception is when the plan explicitly calls for changing test expectations (e.g., a renamed API or changed return type).

## Writing New Tests

1. Write new tests when the plan's Test Plan section specifies them.
2. Tests go in the same package as the changed code, following existing patterns.

## Reporting Test Results

1. **Report test results** in every implementation summary or review — include the command run, pass/fail counts, and any failures with context.
2. If tests fail and the failure is unrelated to your changes, note it explicitly but do not suppress it.
3. A review verdict **cannot be PASS** without successful test evidence.
