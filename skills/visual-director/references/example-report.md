# Example report (fictional project)

This example shows the expected level of detail. The project, files and values are invented for illustration.

````markdown
# Visual audit: notes-app, /pricing and /settings

- Date: 2026-01-15
- Commit / branch: feature/pricing-refresh
- Scope: /pricing, /settings and components changed in the branch
- Themes: dark, light
- Viewports: 360, 1440
- Source of truth: src/styles/tokens.css, src/components/ui/, DESIGN.md

## Summary

The new pricing page bypasses the design tokens in two places and overflows horizontally on small screens.
One helper text in settings fails contrast in light mode. Accessibility was checked only for contrast, focus and
reduced motion; no automated accessibility tool is configured, and there are no visual baselines.

| Group | blocker | error | warning | advisory |
|---|---|---|---|---|
| Verified defects | 0 | 4 | 0 | — |
| Needs review | — | — | 1 | — |
| Suggestions | — | — | — | 1 |

## Verified defects

### VD-01 [TOK-01] Pricing CTA uses a hard-coded color

- Rule source: documented (DESIGN.md:24, "use semantic color tokens only")
- Severity: error
- Confidence: high
- Location: src/components/pricing/PlanCard.tsx:42
- Context: dark theme, all viewports
- Evidence: `className="bg-[#4f46e5] text-white"`; `--color-primary` is #4f46e5 in light and #818cf8 in dark (src/styles/tokens.css:12, :41)
- Observed: in dark mode this button stays #4f46e5 while every other primary button renders #818cf8
- Expected: primary actions use the `Button` component with `variant="primary"`
- Proposed fix: replace the local `<button>` with `<Button variant="primary">`
- Fix safety: safe
- Re-test: search the folder for `bg-[#`; compare dark and light screenshots of /pricing

### VD-02 [CLR-01] Helper text below contrast minimum in light mode

- Rule source: universal (WCAG 2.2 AA 1.4.3)
- Severity: error
- Confidence: high
- Location: src/app/settings/page.tsx:88, `p.text-gray-400`
- Context: light theme, all viewports
- Evidence: computed color #9ca3af on #ffffff = 2.54:1
- Observed: 2.54:1 for 14px text
- Expected: at least 4.5:1; the `text-muted-foreground` token gives #6b7280 = 4.83:1
- Proposed fix: replace `text-gray-400` with `text-muted-foreground`
- Fix safety: safe
- Re-test: recompute contrast in both themes

### VD-03 [LAY-01] Horizontal scroll on /pricing at 360 px

- Rule source: universal (WCAG 2.2 AA 1.4.10 Reflow)
- Severity: error
- Confidence: high
- Location: src/components/pricing/Comparison.tsx:17, `table.comparison { min-width: 412px }`
- Context: both themes, 360 px
- Evidence: `document.documentElement.scrollWidth` = 412 at a 360 px viewport; screenshot pricing-360-light.png
- Observed: the whole page scrolls sideways
- Expected: no horizontal page scroll; wide tables scroll inside their own container
- Proposed fix: wrap the table in the existing `ScrollArea` component
- Fix safety: safe
- Re-test: measure `scrollWidth` at 360 px in both themes

### VD-04 [MOT-01] Floating shapes ignore reduced motion

- Rule source: documented (DESIGN.md:51) and universal
- Severity: error
- Confidence: high
- Location: src/components/pricing/FloatingShapes.tsx:9, `animate-float` (infinite)
- Context: both themes, emulated `prefers-reduced-motion: reduce`
- Evidence: animation still running with reduced motion emulated
- Observed: continuous movement next to the pricing table
- Expected: non-essential animation stops when reduced motion is requested
- Proposed fix: add `motion-reduce:animate-none`
- Fix safety: safe
- Re-test: emulate reduced motion and confirm the shapes are static

## Needs review

### NR-01 [TYP-03] Section heading size differs between pages

- Rule source: inferred (9 of 11 section headings use `text-3xl`; no written rule)
- Location: src/app/pricing/page.tsx:31 uses `text-4xl`
- Question: should section headings always use `text-3xl`, or is pricing intentionally larger?

## Suggestions

- The three plan cards feel dense at 1440 px; more space between features could help scanning. Subjective.

## Coverage

| Area | Status | How / why |
|---|---|---|
| Tokens | CHECKED | code search in src/ |
| Color and contrast | PARTIAL | computed styles on /pricing and /settings only |
| Typography | CHECKED | code search plus computed styles |
| Spacing | CHECKED | code search |
| Buttons and components | CHECKED | code search plus usage of components/ui |
| Icons | CHECKED | imports |
| Layout and responsive | PARTIAL | 360 and 1440 only; 768 and 1024 out of agreed scope |
| Themes | CHECKED | screenshots in both themes |
| Accessibility basics | PARTIAL | contrast, focus and reduced motion only; no axe in project |
| Motion | CHECKED | reduced-motion emulation |
| Visual regressions | NOT_RUN | no approved baselines exist |

Commands run: `npm run lint` (passed), `npm run typecheck` (passed), browser checks at 360 and 1440 px in both themes.
Not run: automated accessibility scan (no tool installed; not added without approval), visual regression (no baselines).

## Decisions needed

1. Confirm the section heading size rule (NR-01).
2. Approve adding an accessibility scanner, or keep manual checks.
3. Approve creating first visual baselines for /pricing.
````
