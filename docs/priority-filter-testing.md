# Priority filter verification

Run `npm test` for storage and filtering regression checks. Run `npm start`
and open `http://127.0.0.1:8000` for the browser checks below.

The procedures below describe how to test; they are not a record that every
procedure has been executed.

Use a test browser profile with requests ordered High, missing priority, Low,
High. Leave Normal without any matching requests. A missing-priority record
can be prepared by removing its `priority` property from the existing local
storage array in browser developer tools before reloading. Use test data only.

| Criterion | How to test | Expected result |
| --- | --- | --- |
| 1. Initial view | Open or reload the page. | All is active; every request appears in its original order. |
| 2. Matching results | Select High, then Low. | All and only matching records appear immediately, without reloading. |
| 3. Switching priorities | Switch repeatedly between High and Low. | Results update immediately; the two High records retain their relative order. |
| 4. Empty results | Select Normal in the fixture above. | “No requests found for this priority” appears and all filter buttons remain usable. |
| 5. Reset | Select All after each priority, including an empty result. | The complete list returns in its original order. |
| 6. Missing priority | Compare All with each specific priority. | The missing-priority record appears only under All. Its existing Normal display is retained. |
| 7. Data integrity | Capture the local storage array, repeatedly filter and reset, then compare it again. | Stored contents and count are unchanged; no duplicate rows appear after reset. |
| 8. Client-side operation | After loading the page, clear the Network panel and switch filters; repeat in offline mode. | Filtering still works with no new network requests or login. |
| 9. Keyboard access | Tab to the labeled filter buttons and activate each with Space or Enter. | Focus is visible and results update as they do with a mouse. |
| 10. Visual indicator | Select each button, including All. | Exactly one button has the distinct purple background and border; its `aria-pressed` value is true. |

Also add a matching and a nonmatching request while a filter is active. Confirm
that the filter remains active, the visible list reflects the new data, All
restores every record newest first, and the request form resets to Normal.

Automated store tests cover exact matches, switching, empty results, reset,
relative order, missing priorities, repeated filtering without mutation, and
unchanged stored data. Existing validation, creation, persistence, and legacy
default tests remain in place.

## Verification Results

This evidence summary matches [the specification](priority-filter-spec.md).

Evidence sources:

- **Automated (user-reported Windows run):** all 10 automated tests passed with
  zero failures. The existing store tests cover exact matching, order, reset,
  empty results, missing priorities, persistence, and nonmutation. These are
  store-level tests, not automated browser tests.
- **Manual (user-reported Windows browser testing):** Low, Normal, High, and All
  filtering; request preservation after browser refresh; active filter
  highlighting; keyboard accessibility; and empty-results handling.
- **Manual (user-reported Windows Chrome Network-panel check):** after clearing
  existing requests, selecting Low, Normal, High, and All without refreshing
  left the Network panel empty throughout. Filtering generated no network requests.
- All previously reported manual checks were reconfirmed as passed by the user.
  No offline test or manual missing-priority test was reported. No new browser
  testing was performed by the assistant for this update.

**Status convention:** Pass means the cited evidence supports the criterion.
Nine criteria are Pass based on the automated and historical manual evidence
below. Criterion 1 is Partial: code and store tests support it, but the reported
browser results do not explicitly establish initial selection and full row order.
Partial identifies missing verification evidence, not a confirmed code defect.

| Acceptance criterion | Status | Automated evidence | Manual evidence and limits |
| --- | --- | --- | --- |
| 1. Initial view | **Partial** | All/default filtering returns the complete list in original order; storage round-trip tests preserve records. | All filtering and request preservation after refresh verified on Windows. The report does not separately describe checking the initial active button and full row order; reconfirm those in the final walkthrough. |
| 2. Matching results | **Pass** | Exact-match filtering assertions cover High and Low; priority persistence tests cover Low, Normal, and High. | Low, Normal, and High filtering verified on Windows. |
| 3. Switching priorities | **Pass** | Repeated High/Low filtering preserves the relative order of matching records. | Switching among priority filters verified on Windows; row-order assertions are automated evidence. |
| 4. Empty results | **Pass** | An unmatched priority returns an empty array. | Empty-results handling verified on Windows. |
| 5. Reset | **Pass** | All, empty-string, and default selections restore the complete original array after matching and empty results. | All filtering verified on Windows. The UI resets through All; there is no separate Clear button. |
| 6. Missing priority | **Pass** | Missing-priority records remain under All and are excluded from specific matches; loaded legacy records are explicitly tested against Normal. | Not manually tested; this Pass is based on automated store tests only. |
| 7. Data integrity | **Pass** | Repeated filtering leaves frozen records unchanged; legacy filtering leaves stored contents unchanged; persistence tests cover round trips. | Request preservation after browser refresh verified on Windows. A before/after browser storage comparison was not reported. |
| 8. Client-side operation | **Pass** | Store tests run with in-memory fixtures/storage; browser network behavior is verified manually. | User cleared Chrome DevTools Network requests, then clicked Low, Normal, High, and All without refreshing. The panel remained empty throughout, confirming filtering generated no network requests. Offline mode was not tested. |
| 9. Keyboard access | **Pass** | Not covered by the automated store tests. | Keyboard accessibility verified on Windows. |
| 10. Visual indicator | **Pass** | Not covered by the automated store tests. | Active-filter highlighting verified on Windows. Automated accessibility-attribute assertions and manual attribute inspection were not reported. |

### Manual specification improvement

The required manual improvement is included: the specification's first Functional Rule requires
an unmistakable visual highlight for the active filter, and acceptance criterion
10 makes that behavior testable. The Windows manual report confirms highlighting.

### Remaining checks before submission

- Criterion 8 is closed by the reported Chrome Network-panel check. No known
  failing acceptance criteria remain. Offline testing is optional additional
  evidence and is not claimed as completed.
- In the final walkthrough, explicitly reconfirm that a fresh page starts with
  All active, restores original row order, and resets from an empty result.
- A browser check of a missing-priority record is recommended to complement the
  automated store coverage; it has not been performed according to the report.
- Adding matching/nonmatching requests while filtered and checking the form's
  Normal reset remain recommended regression checks, not reported manual results.
- Review the submission diff and include the feature code, specification, tests,
  and verification guide. Do not treat the earlier blocked cloud preview as a
  successful manual test.
