# Exercise 7 requirements review

## Skill inspection and invocation

Created `.agents/skills/requirements-reviewer/SKILL.md`. Inspection confirms
that it defines when to apply the workflow, specification/code/test evidence,
all five required reporting categories, citations, and a review-only boundary.
It explicitly prohibits editing files unless asked. No supporting scripts or UI
metadata are necessary for this focused workflow.

Invocation applied by Codex after reading the new Skill:

> $requirements-reviewer Review the priority-filter feature against
> docs/priority-filter-spec.md. Do not change files.

Reviewed implementation: `4a15ab762a1ef4bf81e6e3ad258d15e40a365ddf` (Exercise 6,
also the starting point of `exercise-7-skill`). The review phase changed no files.
The report and documentation corrections below were saved afterward as the
separate response step authorized by the Exercise 7 assignment.

## Satisfied acceptance criteria and evidence limits

Requirement references below are to the numbered Acceptance Criteria in
`docs/priority-filter-spec.md`. Historical browser reports are evidence recorded
in that document, not new observations by this reviewer. `npm test` was run
during this review: 10 passed, 0 failed. No browser checks were performed in
this review; no application behavior changed in Exercise 7.

| Criterion | Review outcome | Implementation and verification evidence |
| --- | --- | --- |
| 1. Initial view | Partial verification; implementation supported by inspection | `src/app.js:20-24` loads records, starts with All, and renders; `index.html:108` initially presses All. `src/request-store.js:59-63` preserves array order. Store tests at `tests/request-store.test.js:127` cover All, but not page startup. The historical criterion-1 verification row explicitly leaves initial active selection and full row order for confirmation. |
| 2. Matching results | Satisfied on code, tests, and historical manual report | `src/app.js:26-34` handles clicks and renders immediately; `src/request-store.js:59-63` matches exact priorities. Tests at `tests/request-store.test.js:127` cover High/Low, and the verification record reports Low/Normal/High browser checks. |
| 3. Switching priorities | Satisfied on code, tests, and historical manual report | The same click handler replaces activePriority; Array.filter preserves relative order. Repeated filtering and two High records are checked in the test at line 127. Historical switching checks are reported. |
| 4. Empty results | Satisfied on code, tests, and historical manual report | `src/app.js:64-77` sets the required message and hides only the table. Controls remain outside the hidden wrapper (`index.html:106-123`). The test at line 127 covers an empty match; historical empty-state checks are reported. |
| 5. Reset | Satisfied on code, tests, and historical manual report | All is exposed in `index.html:108`; the helper's All/default/empty cases restore a copy in original order. The test at line 127 resets after empty results. Historical All checks are reported. A separate Clear button is not required by the specification's alternative wording. |
| 6. Missing priority | Satisfied on code and automated store tests | `src/app.js:20` preserves missing priority for filtering; strict comparison in the helper excludes it from specific matches. `tests/request-store.test.js:148` checks loaded legacy data against Normal and All. A missing-priority browser check remains additional integration confidence. |
| 7. Data integrity | Satisfied on code and automated store tests | The filter handler only changes activePriority, pressed attributes, and rendered rows; it never saves. `src/request-store.js:59-63` copies/filters without modifying records. Tests at lines 127 and 148 verify frozen fixtures and unchanged stored contents. |
| 8. Client-side operation | Satisfied on code and historical network report | The filter handler calls local rendering and the array helper; no network, database, or login path is present. The specification's Verification Results reports an empty Chrome Network panel during all filter choices. This run did not repeat that check. |
| 9. Keyboard access | Satisfied on static support and historical manual report | Native type=button controls and the labeled group appear in `index.html:106-111`; `styles.css:152` supplies button focus styling. Keyboard accessibility is historically reported, not exercised by the store tests. |
| 10. Visual indicator | Satisfied on static support and historical manual report | `src/app.js:30-32` updates aria-pressed on every choice; `styles.css:232` styles the pressed button distinctly. Highlighting is historically reported, not freshly observed or covered by automated browser tests. |

## Gaps and most important finding

**F1 — Verification overclaim (documentation, medium priority):** Criterion 1
requires the initial All selection and every original record in original order
when the page opens. Before this response, both verification documents stated
that all 10 criteria were Pass, while their criterion-1 row admitted that the
initial active button and full row order were not separately checked. The code
supports the expected behavior (`src/app.js:20-24`, `index.html:108`, and
`filterRequestsByPriority`), but store tests at
`tests/request-store.test.js:127` cannot establish page initialization. Calling
the criterion fully verified could lead a reviewer to skip that remaining
browser check. Qualify the status or perform and record the missing check.
This is a missing-evidence finding, not a demonstrated implementation defect.

No confirmed functional mismatch was found in the scoped implementation.

## Unsupported behavior

No new unsupported feature was identified. Legacy rows display Normal through
`src/app.js:createRequestRow` while remaining absent from Normal-filter results.
This is intentional compatibility: `docs/priority-spec.md` requires the Normal
display for legacy records, while filter criterion 6 excludes missing priorities
from specific matches. The explicit preserveMissingPriority load option and
legacy filtering test implement both requirements. Do not treat the display
fallback as proof that a Normal filter must include those records.

## Data or security concerns

No feature-specific concern was found within the review scope. Filtering has
no storage write path; the existing storage key remains unchanged
(`src/request-store.js:STORAGE_KEY`, `src/app.js` filter handler).
User-provided cell values use textContent (`src/app.js:93`), consistent with
AGENTS.md's safe-rendering rule. This is a scoped review, not a production
security audit; the README already limits this localStorage practice app's use.

## Tests still needed

- To close F1: in a test browser profile with multiple known records, reload
  and verify All is initially active and every row follows the stored order.
  Record the actual result before changing criterion 1 to Pass.
- Optional integration regression confidence: exercise a loaded missing-priority
  record in the browser; reset from an empty result; compare storage before and
  after repeated clicks; add matching and nonmatching requests while filtered.
  The existing guide describes these scenarios; this review does not claim they
  were all executed.
- Browser automation for pressed-state changes, keyboard activation, and the
  displayed empty message would add regression coverage. No new dependencies
  or framework are required by this assignment, and none were added.

## Decision and response

**Accept F1 as a documentation finding.** Static evidence supports the
implementation, so changing working application code would not address the
actual issue. Changed criterion 1 to Partial in both
`docs/priority-filter-spec.md` and `docs/priority-filter-testing.md`, and replaced
the blanket all-10-Pass statement with nine Pass / one Partial and its evidence
limit. The acceptance criteria themselves are unchanged.

Acceptance recommendation: accept this Skill and evidence correction for
Exercise 7. Retain the initial-view browser confirmation as an explicit
Exercise 6 verification follow-up. No claim is made that this documentation
change completes that browser check.

## Validation of the Exercise 7 change

- Bundled Skill Creator `quick_validate.py`: Skill is valid.
- `npm test`: all 10 tests passed, zero failures.
- Full staged diff reviewed; `git diff --cached --check` passed.
- Changes are limited to the Skill, this review record, and the two matching
  verification summaries. Application code, dependencies, and criteria did not change.
