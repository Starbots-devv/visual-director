# Findings template

Copy this structure for every audit. Keep it short: one entry per finding, evidence first, no padding.

````markdown
# Visual audit: <project or scope>

- Date: <YYYY-MM-DD>
- Commit / branch: <sha or branch>
- Scope: <routes, components or diff reviewed>
- Themes: <dark, light>
- Viewports: <widths in px>
- Source of truth: <files used, e.g. tokens file, DESIGN.md, components/ui>

## Summary

<2–4 sentences: overall state, most important defects, what was not covered.>

| Group | blocker | error | warning | advisory |
|---|---|---|---|---|
| Verified defects | 0 | 0 | 0 | — |
| Needs review | — | — | 0 | — |
| Suggestions | — | — | — | 0 |

## Verified defects

### VD-01 [RULE-ID] <short title>

- Rule source: documented (<file:line>) | universal (<e.g. WCAG 2.2 AA 1.4.3>)
- Severity: blocker | error | warning
- Confidence: high | medium
- Location: <file:line> and/or <route + selector>
- Context: <theme, viewport, state (hover, focus, open, error)>
- Evidence: <code excerpt, computed value, measurement, screenshot name, test output>
- Observed: <what happens>
- Expected: <what the rule requires>
- Proposed fix: <smallest change, reusing existing tokens or components>
- Fix safety: safe | needs review | needs design decision
- Re-test: <exact check to run after the fix>

## Needs review

<Same fields. Explain what is missing: rule is only inferred, evidence is partial, or intent is unclear.>

## Suggestions

<Subjective or aesthetic ideas. One line of rationale each. Never blocking.>

## Coverage

| Area | Status | How / why |
|---|---|---|
| Tokens | CHECKED / PARTIAL / NOT_RUN | <tool or reason> |
| Color and contrast | ... | ... |
| Typography | ... | ... |
| Spacing | ... | ... |
| Buttons and components | ... | ... |
| Icons | ... | ... |
| Layout and responsive | ... | ... |
| Themes | ... | ... |
| Accessibility basics | ... | ... |
| Motion | ... | ... |
| Visual regressions | ... | ... |

Commands run: <list each command and its result>
Not run: <each skipped check and why>

## Decisions needed

<Questions only a human can answer: unclear rules, new tokens or variants, baseline approvals.>
````

## Status values

- `CHECKED`: the check ran and its result is reported.
- `PARTIAL`: the check ran on part of the scope; say which part.
- `NOT_RUN`: the check did not run. Never turn `NOT_RUN` into a pass.

## Writing rules

- Every verified defect needs evidence someone else can reproduce.
- Separate pre-existing debt from new regressions when reviewing a diff.
- Do not include personal data from screenshots or logs; crop or redact.
- Do not report scores or percentages you did not measure.
