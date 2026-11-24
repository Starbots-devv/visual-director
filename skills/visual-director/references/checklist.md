# Visual consistency checklist

Universal checks for auditing a frontend against its own design system. Use only the sections that match the request. Rule IDs are stable labels for reports; adapt thresholds to the project's documented rules.

**How to check:** `static` = code search or linters · `runtime` = browser DOM, computed styles or screenshots · `test` = the project's existing test suites · `manual` = human or agent judgment with evidence.

**Default severity:** `error` = verified violation of a documented rule or WCAG 2.2 AA · `warning` = inconsistency or risk · `advisory` = subjective. Escalate to `blocker` only when content or a primary action becomes unreadable or unreachable.

## Tokens

| ID | Check | How | Default |
|---|---|---|---|
| TOK-01 | Hex/RGB/HSL color literals in UI code where a token exists | static | error |
| TOK-02 | Raw palette utilities (e.g., `bg-violet-600`) used instead of semantic tokens (e.g., `bg-primary`) | static | error |
| TOK-03 | Reference to a token that does not exist or is deprecated | static | error |
| TOK-04 | Token defined for one theme but missing in another | static, runtime | error |
| TOK-05 | Ad hoc shadows, borders, radii, gradients or blur outside the defined set | static | warning |
| TOK-06 | Arbitrary z-index values instead of a layering scale | static | warning |
| TOK-07 | `!important` used to override tokens or component styles | static | warning |

## Color and contrast

| ID | Check | How | Default |
|---|---|---|---|
| CLR-01 | Body text below 4.5:1 contrast (3:1 for large text) in any theme | runtime, test | error |
| CLR-02 | UI component boundaries and icons that convey meaning below 3:1 | runtime | error |
| CLR-03 | Text contrast fails in a specific state (hover, disabled, selected, on image/glass) | runtime | error |
| CLR-04 | Color used as the only way to convey information (errors, status) | manual | error |
| CLR-05 | Brand/accent color used where the design system reserves it for other roles | static, manual | warning |

## Typography

| ID | Check | How | Default |
|---|---|---|---|
| TYP-01 | Font size outside the type scale (e.g., `text-[17px]`, inline `font-size`) | static | error |
| TYP-02 | Font family not in the approved set, or missing fallback | static, runtime | error |
| TYP-03 | Same heading role rendered with different sizes or weights across pages | runtime | warning |
| TYP-04 | Line height too tight for the role, or outside the defined values | static, runtime | warning |
| TYP-05 | More than one top-level heading per page, or skipped heading levels, without a documented reason | runtime | warning |
| TYP-06 | Text clipped by fixed height or `overflow: hidden` | runtime | error |

## Spacing and layout containers

| ID | Check | How | Default |
|---|---|---|---|
| SPC-01 | Margin, padding or gap values outside the spacing scale | static | warning |
| SPC-02 | Same type of page or section uses different container widths or padding | runtime | warning |
| SPC-03 | Grids or columns outside the documented layout patterns | runtime | warning |
| SPC-04 | Border radius outside the radius scale | static | warning |

## Buttons and components

| ID | Check | How | Default |
|---|---|---|---|
| CMP-01 | Native `<button>`/`<a>` styled locally where a shared Button component exists | static | error |
| CMP-02 | Variant or size that the component does not define | static | error |
| CMP-03 | Local class overrides that change a shared component's core look | static, runtime | warning |
| CMP-04 | Interactive component without visible focus, disabled, loading or error states | runtime, test | error |
| CMP-05 | Near-duplicate components (cards, modals, dropdowns) with diverging styles | static | warning |
| CMP-06 | Same role (primary action, card, badge) looks different across routes | runtime | warning |
| CMP-07 | New UI dependency added without review of license and overlap with existing components | static | warning |

## Icons

| ID | Check | How | Default |
|---|---|---|---|
| ICN-01 | Icons imported from a library other than the approved family | static | error |
| ICN-02 | Inconsistent icon sizes or stroke weights for the same role | static, runtime | warning |
| ICN-03 | Icon-only control without an accessible name | static, runtime | error |
| ICN-04 | Icons or images that do not adapt to the active theme | runtime | warning |

## Layout and responsive

| ID | Check | How | Default |
|---|---|---|---|
| LAY-01 | Unintended horizontal scroll at any supported width | runtime | error |
| LAY-02 | Interactive elements overlapping or covering each other unintentionally | runtime | error |
| LAY-03 | Primary action or navigation unreachable or off-screen | runtime | blocker |
| LAY-04 | Sticky header or banner hides focused content or anchor targets | runtime | error |
| LAY-05 | Images or logos distorted or unexpectedly cropped | runtime | warning |
| LAY-06 | Inputs, labels or long words overflowing their container | runtime | error |
| LAY-07 | Noticeable layout shift caused by fonts, images or animation | runtime | warning |
| LAY-08 | Global `overflow-x: hidden` on body hiding layout bugs | static | warning |

## Themes (dark/light)

| ID | Check | How | Default |
|---|---|---|---|
| THM-01 | Contrast issues that appear in only one theme | runtime | error |
| THM-02 | Components or surfaces that do not switch with the theme | runtime | error |
| THM-03 | Theme preference not persisted, or wrong-theme flash on load | runtime | warning |
| THM-04 | Glass, blur or transparent surfaces unreadable over real backgrounds | runtime | warning |

## Accessibility basics

| ID | Check | How | Default |
|---|---|---|---|
| A11Y-01 | Verified automated violations (axe or equivalent) | test | error |
| A11Y-02 | Focus indicator missing or invisible | runtime | error |
| A11Y-03 | Control not reachable or not operable by keyboard; focus order or trap broken | runtime, manual | blocker |
| A11Y-04 | Missing alt text or accessible name | static, runtime | error |
| A11Y-05 | Form errors not associated with their fields | runtime | error |
| A11Y-06 | Touch targets smaller than 24×24 CSS px (WCAG 2.2 AA) or the project's stricter target | runtime | warning |
| A11Y-07 | Critical flow breaks at 200% zoom or 320 px width | runtime | error |
| A11Y-08 | Hover-only interactions with no touch or keyboard equivalent | manual | warning |

## Motion

| ID | Check | How | Default |
|---|---|---|---|
| MOT-01 | Non-essential animation ignores `prefers-reduced-motion` | static, runtime | error |
| MOT-02 | Infinite or large motion near content the user is reading | manual | warning |
| MOT-03 | Durations or easings outside the motion tokens | static | warning |
| MOT-04 | Heavy effects (video, WebGL, parallax) without a reduced-motion or low-power fallback | static, manual | warning |

## Visual regressions

| ID | Check | How | Default |
|---|---|---|---|
| VIS-01 | Screenshot differs from the approved baseline beyond tolerance | test | error |
| VIS-02 | Baseline changed in the diff without explicit human approval | static | error |
| VIS-03 | No baseline exists for a critical view, theme or viewport | test | warning |
| VIS-04 | Dynamic content (dates, avatars, counters, ads) not masked in captures | test | warning |
| VIS-05 | Shared chrome (navbar, footer) changes unexpectedly between routes | runtime | warning |

## Exceptions

Literal values are often legitimate in logos, illustrations, data visualizations with semantic palettes, canvas/3D renders, third-party embeds and test fixtures. Before reporting, check whether the project documents an exception. A good exception names the rule, the path or selector, the reason, who approved it and when it should be revisited.

Never accept as exceptions: blanket rule disabling, `!important` to bypass tokens, global `overflow-x: hidden` to hide overflow bugs, auto-updating snapshots in CI without review, or a `PASS` report with skipped checks.

## Evidence hygiene

- Wait for web fonts and lazy content before capturing; pause or disable animations; mask dynamic regions.
- Record browser, viewport, theme, state and commit for every runtime finding.
- Rectangle overlap alone is not a defect: confirm visibility, intent and state.
- Screenshot diffs show change, not quality; a human decides whether a change is acceptable.
- AI-assisted visual critique is advisory only. It never decides pass/fail and never runs on user data without consent.

## Suggested coverage matrix

Adapt to the project's breakpoints. Run a representative subset per pull request and the full matrix before a release.

| Theme | 360 | 768 | 1024 | 1440 |
|---|---|---|---|---|
| dark | critical | review | review | critical |
| light | critical | review | review | critical |

Also include key states: hover, focus, open menus and dialogs, form errors, empty and loading states, and reduced motion.
