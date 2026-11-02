---
name: requirements-reviewer
description: Compare a feature implementation with its specification before acceptance, reporting evidence-backed coverage, gaps, unsupported behavior, data or security concerns, and tests still needed.
---

# Requirements reviewer

Use this workflow when asked to assess whether a feature or application change meets a supplied specification. If the specification or implementation cannot be located, report the missing input rather than inventing requirements.

## Review boundary

Review only. Do not edit code, specifications, tests, or other files unless the user asks. Do not treat review findings as permission to implement fixes, commit, or merge. Run relevant existing checks when available; avoid commands that rewrite tracked files or affect real user data.

## Evidence to examine

- Read applicable repository guidance and the named specification, including acceptance criteria, constraints, and exclusions. Identify ambiguities or conflicts with earlier specifications rather than silently resolving them.
- Trace each criterion through the relevant implementation, including UI event handlers, business logic, rendering, and persistence where applicable. Inspect the change diff when available and distinguish new behavior from existing behavior.
- Examine relevant tests and verification records. Separate a planned test, a historical user-reported result, a result observed during this review, and static code evidence. Passing helper tests do not establish browser integration behavior.
- Examine data preservation, validation, unsafe rendering, and network or access behavior within the feature's scope. Do not invent a production security requirement for a practice application.

## Report

Identify the reviewed specification and implementation revision, scope, and checks run. Provide:

1. **Satisfied acceptance criteria:** Map every criterion to an outcome (satisfied, partial, unmet, or unverified) with specification and code/test evidence. State the basis and limits of verification.
2. **Gaps:** Prioritize concrete mismatches and missing evidence separately. Give the affected requirement, observed behavior or evidence limit, impact, and a focused recommendation. Unexecuted tests alone are not proof of a defect.
3. **Unsupported behavior:** Identify behavior beyond or contrary to the specification; distinguish inherited behavior and deliberate compatibility from new scope expansion. Say when none is found.
4. **Data or security concerns:** Report demonstrated risks with their paths through the code, or say no feature-specific concern was found within the reviewed scope.
5. **Tests still needed:** Identify uncovered scenarios, expected results, and whether they are required to establish a criterion or optional regression confidence.

Cite file paths and line numbers (or stable specification section/criterion and code symbols) for material claims. Each finding must connect specification evidence with implementation evidence; for missing verification, also cite the relevant tests or verification record. Label inference and uncertainty. Do not fabricate execution results or manufacture findings when the implementation matches.

Finish with an acceptance recommendation and identify the most important finding, if any. Leave accepting, modifying, or rejecting findings to the reviewer; do not automatically apply changes.
