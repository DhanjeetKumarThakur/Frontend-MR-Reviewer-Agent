---
name: Frontend MR Review
description: Reviews the current frontend Merge Request against its story requirements, existing codebase patterns, and frontend engineering rules.
---

# Frontend MR Review

Review the current Merge Request using:

1. The MR source branch.
2. The MR target branch.
3. The provided story / requirement.
4. The Git diff between the source and target branches.
5. The existing codebase.
6. `.github/frontend-rules.md`.

## Review priorities

Evaluate the implementation in this order:

### Requirement correctness

Does the implementation actually satisfy the story or requirement?

### Functional correctness

Does the changed code behave correctly and avoid obvious regressions?

### Existing project patterns

Does the implementation use appropriate patterns already established in the codebase?

### Frontend engineering standards

Does the implementation follow the rules in:

`.github/frontend-rules.md`

### Maintainability

Does the change introduce unnecessary duplication, complexity, or abstractions?

### Backward compatibility

Could the change unintentionally break existing consumers, components, services, APIs, or behavior?

## Review scope

Start with the MR diff.

Inspect surrounding code only when required to understand the changed code or validate a finding.

Do not review unrelated files or unrelated existing code.

## Finding quality

Only report findings that are actionable and supported by evidence.

Prefer a small number of high-confidence findings over a large number of speculative or subjective comments.

Do not report a stylistic preference as a defect when the existing implementation is clear and consistent with the codebase.