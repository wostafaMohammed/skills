---
name: ui-review
description: Reviews interfaces for usability, consistency, accessibility, and responsive behavior.
---

# Universal Interface Review & Certification Skill v2

> A platform-adaptive UI/UX quality gate for reviewing interfaces across web, mobile, desktop, and design tools.
> This skill is read-only by default. It adapts its rules to the detected platform, framework, styling system, component library, design tokens, available runtime tools, and project conventions.

---

## 0. Purpose

Review an interface as **one system**, not as disconnected audits.

A strong interface review must connect:

- Accessibility
- Interaction behavior
- Layout and hierarchy
- Writing and terminology
- Typography
- Component consistency
- Design-system compliance
- Responsive / adaptive behavior
- Localization and RTL
- Motion
- Performance UX
- Visual consistency
- Runtime verification
- Regression risk

The result is one prioritized verdict backed by evidence.

The skill must work across:

- Web applications
- Mobile applications
- Desktop applications
- Cross-platform applications
- Figma or other design artifacts
- Source-only repositories when runtime access is unavailable

Never assume a web stack unless evidence shows one.

---

# 1. Operating Contract

## 1.1 Universal-first behavior

Before applying any rule, determine whether it is:

1. **Universal** — applies to all interfaces.
2. **Platform-specific** — applies only to web, iOS, Android, Flutter, React Native, desktop, Figma, etc.
3. **Tool-specific** — requires a browser, screenshot engine, Figma API, accessibility tree, test runner, or other capability.

Never force a platform-specific implementation onto another platform.

Examples:

- `<button>` / `<a href>` semantics are Web-specific.
- VoiceOver and Dynamic Type are iOS-specific.
- TalkBack semantics are Android-specific.
- `Semantics` is Flutter-specific.
- Figma node creation is Figma-specific.
- CSS, Tailwind, Motion, Framer Motion, and browser DevTools rules apply only when those technologies are present.

## 1.2 Preserve the project

Before proposing any fix:

- Detect the existing styling system.
- Detect the component library.
- Detect design tokens.
- Detect typography rules.
- Detect motion conventions.
- Detect spacing and density.
- Detect naming and terminology.
- Detect localization conventions.
- Detect test and preview commands.

Express every recommendation in the project's existing system.

Do **not** introduce:

- a second CSS strategy,
- a new component library,
- a new animation dependency,
- a new icon library,
- a new token system,

unless the user explicitly asks for architectural migration.

## 1.3 Read-only by default

A review request is read-only.

Do not mutate code, designs, tokens, assets, or configuration unless the user explicitly asks to implement the findings.

If implementation is requested:

1. Preserve the review report as the approved change scope.
2. Implement only the approved scope.
3. Re-run verification.
4. Re-run applicable visual checks.
5. Report regressions separately.
6. Never silently expand scope.

---

# 2. Environment Detection

Before judgment, resolve the environment.

## 2.1 Detect

Identify, where possible:

- Product/platform type
- Web / mobile / desktop / design-only
- Framework
- Rendering model
- Styling system
- Component library
- Design-system package(s)
- Design tokens
- Icon system
- Motion/animation system
- Localization/i18n setup
- RTL support
- Supported viewports / breakpoints
- Browser/runtime availability
- Figma/design-tool availability
- Screenshot capability
- Test runner
- E2E runner
- Accessibility testing tools
- Preview/build commands
- Existing visual-regression setup
- Existing CI quality gates

## 2.2 Capability declaration

At the beginning of the output, declare only what is actually available:

| Capability | Status | Evidence |
|---|---|---|
| Source inspection | Available / Unavailable | repo/files |
| Runtime preview | Available / Unavailable | command / URL |
| Browser interaction | Available / Unavailable | tool |
| Screenshot comparison | Available / Unavailable | tool/setup |
| Accessibility tree | Available / Unavailable | tool |
| Figma annotation | Available / Unavailable | file/tool |
| Automated tests | Available / Unavailable | command |
| RTL/localization test | Available / Unavailable | setup |

Do not pretend a capability exists.

When unavailable, mark related verification as **NOT VERIFIED** rather than converting it into a finding.

---

# 3. Review Modes

Resolve the requested scope and mode first.

| Mode | Coverage | Finding cap |
|---|---|---:|
| `quick` | Primary flow and highest-impact states; report HIGH and MEDIUM only | 5 |
| `full` | Entire requested scope and all applicable domains | 15 |
| `certify` | Full review + required runtime verification + state matrix + regression checks | No artificial padding |

Use `full` when no mode is supplied.

If scope is too large to inspect credibly:

- narrow to the highest-traffic complete flow,
- state the boundary,
- never imply that uninspected surfaces were reviewed.

---

# 4. Review Order

Foundational failures must not be hidden by polish.

Review in this order:

1. Accessibility & input methods
2. Interaction behavior
3. Layout & hierarchy
4. Writing & terminology
5. Typography
6. Design-system compliance
7. Component states
8. Responsive / adaptive behavior
9. Localization & RTL
10. Motion
11. Performance UX
12. Cross-page consistency
13. Visual polish
14. Regression risk

When two domains overlap, report the root cause once and mention secondary effects in **Why**.

---

# 5. Evidence Policy

Every finding must be backed by the strongest evidence available.

## 5.1 Source evidence

For code-level findings:

- cite `path/to/file:line`,
- show the current implementation,
- identify the root cause.

Do not make a code-level claim from appearance alone.

## 5.2 Runtime evidence

For behavior or visual findings:

- inspect the rendered state when possible,
- identify the exact screen, component, route, and state,
- reproduce the behavior.

Do not make a runtime claim from source alone when runtime determines the outcome.

## 5.3 Design-only evidence

When reviewing a design artifact without source:

- cite the exact frame/screen/component,
- use design-layer evidence,
- do not pretend source implementation was inspected.

## 5.4 Verification gaps

A missing verification path is not automatically a product defect.

Use:

- `VERIFIED`
- `PARTIALLY VERIFIED`
- `NOT VERIFIED`

Never convert a verification gap into `PASS`.

---

# 6. Core Universal Rules

## 6.1 Accessibility & input methods

Accessibility is foundational.

Every interface must support the relevant platform input methods:

- pointer/mouse where applicable,
- keyboard where applicable,
- touch where applicable,
- screen reader / assistive semantics,
- visible focus or selection state,
- non-color status cues,
- scalable text where supported,
- reduced motion where supported.

Prefer native platform controls and semantics over custom reimplementations.

### Universal checks

- Every actionable control has a clear accessible name.
- Every interaction has an appropriate non-pointer path where the platform requires it.
- Focus/selection is visible.
- Destructive actions are explicit.
- Color is never the only status cue.
- Disabled state is understandable.
- Errors are announced or surfaced near the failure.
- Touch/click targets are usable.
- Zoom/text scaling must not destroy the layout.
- Reduced-motion preferences are honored where the platform exposes them.

Platform-specific accessibility rules are defined later in adapters.

---

## 6.2 Interaction behavior

Every interactive element must communicate:

- What is actionable
- Current state
- Hover state when pointer input exists
- Focus state when keyboard input exists
- Pressed/active state
- Disabled state
- Loading state
- Error state
- Success state when relevant
- Selected state when relevant

Interactions must be interruptible where user intent can change mid-action.

Critical actions must never rely on ambiguous buttons such as:

- OK
- Yes
- Continue

when the consequence should be stated explicitly.

---

## 6.3 Layout & hierarchy

Layout communicates before text is read.

Review:

- grouping,
- spacing,
- alignment,
- leading/trailing order,
- content priority,
- clipping,
- overflow,
- density,
- action reachability,
- empty space,
- adaptive behavior.

Prefer grouping with spacing before adding separator lines.

As a starting principle:

> inter-group spacing should be visibly larger than intra-group spacing.

Do not force fixed widths/heights onto text containers when content can grow.

Use direction-aware layout concepts (`leading` / `trailing`) instead of assuming left/right.

---

## 6.4 Writing & terminology

The product should have one voice.

Review:

- terminology consistency,
- button labels,
- link labels,
- errors,
- empty states,
- destructive confirmations,
- localization risk,
- capitalization policy,
- device-neutral wording where appropriate.

Rules:

- Prefer plain words over clever wording.
- Buttons should name the action.
- Errors should explain what to do next.
- Empty states should orient the user and provide one clear next step.
- Avoid sentence construction that breaks localization.
- Never blame the user.
- Security and data-loss messages must be serious and explicit.

---

## 6.5 Typography

Review typography as a system:

- font families,
- sizes,
- weights,
- heading hierarchy,
- line height,
- measure,
- truncation,
- numeric stability,
- text scaling.

Prefer a small semantic type scale over one-off sizes.

Long-form copy should not span excessively wide lines.

Changing numeric values should use stable-width digits when the platform supports them.

Never truncate important content without a way to recover the full value.

---

# 7. Design-System Compliance Gate

This is a required domain in `full` and `certify`.

Detect the project's official or de facto:

- color tokens,
- semantic colors,
- spacing scale,
- radius scale,
- typography scale,
- shadows/elevation,
- z-index/elevation levels,
- icons,
- control sizes,
- motion tokens,
- component primitives.

Then review for design drift.

## 7.1 Flag

Flag only when confirmed:

- arbitrary values bypassing an established token,
- duplicate components solving the same role differently,
- inconsistent icon libraries on one surface,
- duplicated local styles that should use a shared primitive,
- inconsistent component states,
- inconsistent density,
- local overrides that break shared behavior.

## 7.2 Do not flag

Do not flag:

- intentional exceptions documented by the project,
- platform-required variations,
- values that are part of an established compact/dense mode,
- temporary migration code unless it causes a user-facing inconsistency.

## 7.3 Systemic issues outrank local issues

A shared token or primitive defect has higher leverage than the same symptom in one leaf component.

---

# 8. Component State Matrix

For every important shared component, inspect applicable states.

| State | Required when applicable |
|---|---|
| Default | Yes |
| Hover | Pointer platforms |
| Focus | Keyboard platforms |
| Pressed / active | Interactive controls |
| Disabled | If component supports disabling |
| Loading | Async actions |
| Error | Inputs / async actions |
| Success | Flows where success is persistent |
| Selected | Tabs, filters, toggles, nav |
| Empty | Data containers |
| Long content | Text-bearing components |
| Narrow viewport | Adaptive UIs |
| RTL | Localized products |
| High text scale | Platforms that support text scaling |

A component is not certifiable if a required state is missing from evidence.

---

# 9. Responsive & Adaptive Stress Testing

Do not assume standard breakpoints are correct.

Breakpoints should follow content failure, not device folklore.

Test the smallest and largest supported sizes first.

When runtime access exists, test representative widths/sizes appropriate to the platform.

For web, a useful stress set is:

- 320
- 375
- 768
- 1024
- 1440
- 1920

Use the project's actual supported widths when defined.

Check:

- overflow,
- clipping,
- fixed heights,
- hidden actions,
- table behavior,
- dialogs,
- nav collapse,
- text wrapping,
- safe areas,
- orientation changes,
- keyboard appearance on mobile,
- zoom/text scale.

---

# 10. Localization & RTL Gate

Run when the project supports or plans localization.

Inspect:

- direction-aware spacing,
- mirrored navigation icons where appropriate,
- text growth,
- pluralization,
- concatenated strings,
- fixed-width controls,
- number/date formatting,
- translated button labels,
- bidirectional text,
- truncation,
- alignment,
- layout growth.

Test at least:

- base language,
- one language with longer strings,
- RTL language when supported.

For RTL:

- mirror directional navigation glyphs,
- do not mirror logos, checkmarks, clocks, or physical-object icons unless meaning requires it,
- use logical layout properties where the platform supports them.

---

# 11. Motion Gate

Motion must earn its place.

Review:

- trigger frequency,
- interruptibility,
- duration,
- direction,
- reduced-motion behavior,
- loading feedback,
- exit behavior,
- first-load behavior.

Rules:

- interactive state transitions should be interruptible,
- repeated actions should avoid theatrical motion,
- exits should usually be quieter than entrances,
- do not animate default state on first render unless intentional,
- never animate every possible property,
- honor reduced-motion preferences.

Platform-specific implementation belongs in adapters.

---

# 12. Performance UX Gate

This gate focuses on perceived interface performance.

Inspect, when tools allow:

- first meaningful render,
- delayed interactions,
- janky animation,
- layout shift,
- input latency,
- modal/drawer open latency,
- route transition latency,
- large image/media impact,
- avoidable re-render behavior,
- blocking loading states.

Do not invent numeric performance claims without measurements.

Report:

- observed symptom,
- reproducible interaction,
- available measurement,
- likely source only when evidence supports it.

---

# 13. Cross-Page Consistency Audit

For shared controls and patterns, compare across screens.

Examples:

- buttons,
- inputs,
- cards,
- tables,
- tabs,
- dialogs,
- toasts,
- filters,
- page headers,
- empty states,
- destructive flows,
- navigation.

Look for:

- same role, different appearance,
- same appearance, different behavior,
- different copy for the same action,
- inconsistent state handling,
- inconsistent spacing/density.

Prefer fixing the shared primitive or token when possible.

---

# 14. Visual Regression Gate

Use when screenshot tooling or existing visual-regression infrastructure is available.

## 14.1 Baseline behavior

Compare:

- before vs after implementation,
- shared components,
- changed pages,
- high-traffic adjacent pages.

## 14.2 Classify differences

Every visual difference should be classified as:

- Intended
- Approved side effect
- Regression
- Unable to determine

Never treat pixel difference alone as a defect.

## 14.3 When tooling is unavailable

Mark:

`Visual regression: NOT VERIFIED`

Do not fabricate screenshot evidence.

---

# 15. Platform Adapters

Apply only the adapter(s) that match the detected environment.

---

## 15.1 Web Adapter

Use when the product is rendered with browser technologies.

### Native semantics first

Prefer:

- `<button>` for actions,
- `<a href>` for navigation,
- semantic form controls,
- native labels.

Avoid clickable generic containers when a native element exists.

### Keyboard

Every pointer interaction that requires keyboard parity must have a keyboard path.

Typical expectations:

- Escape closes overlays,
- arrows navigate composite widgets,
- Tab moves between widgets,
- Enter/Space activate controls where appropriate.

Avoid positive `tabindex`.

### Focus

Use visible focus indicators.

Do not remove outline without a verified replacement.

### Labels

Placeholders are not labels.

Inputs should have:

- accessible label,
- useful `name`,
- meaningful `autocomplete` when relevant,
- correct `type`,
- correct `inputmode`.

Never block paste.

### Mobile input size

Prevent browser zoom caused by undersized form text.

### CSS/styling

Use the project's styling system:

- Tailwind if the project uses Tailwind,
- CSS Modules if it uses CSS Modules,
- styled-components if established,
- StyleX if established,
- plain CSS if established.

Never introduce a second approach for a local fix.

### Web motion

Prefer CSS transitions for interruptible state changes.

Avoid `transition: all`.

Use compositor-friendly properties where possible.

### Web RTL

Prefer logical properties:

- `padding-inline-*`
- `margin-inline-*`
- logical positioning where available.

---

## 15.2 React / Next.js Adapter

Apply in addition to Web Adapter when detected.

Review:

- shared component primitives,
- prop/state behavior,
- hydration-safe UI states,
- route transitions,
- server/client boundaries where they affect UX,
- duplicate primitives,
- animation dependency consistency.

If `motion` or `framer-motion` exists:

- follow the package already used nearby,
- do not add a new dependency solely for a local effect.

---

## 15.3 iOS Adapter

Use for SwiftUI/UIKit interfaces.

Review:

- native control semantics,
- VoiceOver labels and order,
- Dynamic Type,
- minimum usable touch targets,
- safe-area behavior,
- keyboard handling,
- navigation conventions,
- destructive confirmation patterns,
- Reduce Motion,
- light/dark appearance,
- content-size stress.

Prefer native platform behaviors before custom reimplementation.

Do not apply HTML/ARIA/CSS-specific requirements.

---

## 15.4 Android Adapter

Use for Jetpack Compose / Android Views.

Review:

- TalkBack semantics,
- content descriptions,
- focus traversal,
- touch targets,
- font scaling,
- system back behavior,
- IME/keyboard behavior,
- navigation conventions,
- edge-to-edge layout,
- reduced-motion/accessibility settings where relevant.

Do not apply web-only semantics.

---

## 15.5 Flutter Adapter

Review:

- `Semantics`,
- focus traversal,
- keyboard shortcuts where applicable,
- responsive constraints,
- text scaling,
- directionality,
- safe areas,
- platform adaptation,
- state consistency,
- theming/token usage.

Do not require CSS/Tailwind/DOM constructs.

---

## 15.6 React Native Adapter

Review:

- accessibility props,
- labels/hints/roles,
- touch target behavior,
- dynamic text scaling,
- safe areas,
- keyboard avoidance,
- platform-specific interaction patterns,
- RTL,
- component reuse,
- theming/tokens.

Do not assume browser keyboard patterns unless targeting web.

---

## 15.7 Desktop Adapter

Use for Electron, Tauri, native desktop, or desktop-first cross-platform apps.

Review:

- keyboard-first navigation,
- window resizing,
- high-density layouts,
- menus,
- shortcuts,
- focus management,
- pointer + keyboard parity,
- native window behavior,
- high-DPI rendering,
- platform conventions.

Electron/Tauri may additionally use the Web Adapter for rendered content.

---

## 15.8 Figma / Design Artifact Adapter

Apply only when reviewing Figma or a design artifact.

The review remains read-only unless annotation is explicitly requested.

When annotation capability exists:

- add findings without modifying reviewed frames,
- keep annotations on a separate top-level layer,
- never reparent or restyle the product design,
- replace old review annotations on rerun instead of stacking them.

When annotation tools are unavailable:

- produce the report only,
- state `Figma annotation: NOT AVAILABLE`.

Do not require Figma API operations in non-Figma environments.

---

# 16. Optional Tool Adapters

## 16.1 Browser available

When browser interaction is available:

- walk the primary flow,
- use keyboard-only navigation where relevant,
- inspect hover/focus/active/loading/error/empty states,
- resize viewport,
- inspect motion,
- test overlays,
- test back/forward/navigation,
- capture screenshots when useful.

## 16.2 Source-only

When only source is available:

- perform source-backed findings,
- mark runtime-dependent claims `NOT VERIFIED`,
- do not infer appearance or runtime behavior beyond what source proves.

## 16.3 Test runner available

Run safe, relevant checks.

Examples:

- unit tests,
- component tests,
- accessibility tests,
- E2E tests,
- type checks,
- build.

Never treat unrelated passing tests as proof of UI correctness.

---

# 17. Severity

Use one shared scale.

- `HIGH` — blocks a task, misleads the user, hides critical content or controls, causes data-loss/security risk, prevents accessibility for a core flow, or represents a repeated systemic failure.
- `MEDIUM` — meaningfully harms comprehension, efficiency, adaptability, consistency, localization, or usability.
- `LOW` — isolated polish issue with limited task impact. Include only in `full` or `certify`.

Within one severity, rank by:

1. Reach
2. User impact
3. Systemic leverage
4. Frequency
5. Regression risk

A token/shared-component fix outranks a one-off leaf fix.

---

# 18. Output Format

Always use these sections.

## 18.1 Scope and Coverage

State:

- mode,
- exact scope,
- platform,
- framework,
- styling,
- design system,
- runtime availability,
- review boundaries.

Then show:

| Domain | Evidence inspected | Result |
|---|---|---|
| Accessibility | ... | Clear / findings / Not reviewed |
| Interaction | ... | ... |
| Layout | ... | ... |
| Writing | ... | ... |
| Typography | ... | ... |
| Design system | ... | ... |
| Component states | ... | ... |
| Responsive/adaptive | ... | ... |
| Localization/RTL | ... | ... |
| Motion | ... | ... |
| Performance UX | ... | ... |
| Cross-page consistency | ... | ... |
| Visual regression | ... | ... |

`Clear` means actually inspected.

`Not reviewed` must explain why.

---

## 18.2 Findings

Use one table:

| # | Severity | Domain | Location | Before | After | Why | Evidence |
|---|---|---|---|---|---|---|---|

Rules:

- one row = one root cause,
- combine confirmed occurrences of the same root cause,
- no duplicate findings across domains,
- do not pad the report,
- preserve the mode's cap,
- if no findings exist, say: `No actionable interface findings.`

---

## 18.3 Considered but Rejected

Record real candidates that were inspected but deliberately rejected.

| Location | Candidate | Rejected because |
|---|---|---|

Typical reasons:

- project convention is intentional,
- evidence is insufficient,
- proposed change adds complexity without user value,
- platform convention justifies the implementation,
- existing token/component is already consistent.

---

## 18.4 Verification

List each verification action:

| Check | Exact action | Result |
|---|---|---|
| Runtime flow | ... | PASS / FAIL / NOT VERIFIED |
| Keyboard | ... | ... |
| Responsive | ... | ... |
| RTL | ... | ... |
| Tests | ... | ... |
| Build | ... | ... |
| Visual regression | ... | ... |

Do not convert a verification gap into a pass.

---

## 18.5 Certification Matrix

Required in `certify` mode.

| Gate | Status |
|---|---|
| Accessibility | PASS / FAIL / NOT VERIFIED |
| Interaction | PASS / FAIL / NOT VERIFIED |
| Design-system compliance | PASS / FAIL / NOT VERIFIED |
| Component state matrix | PASS / FAIL / NOT VERIFIED |
| Responsive/adaptive | PASS / FAIL / NOT VERIFIED |
| Localization/RTL | PASS / FAIL / N/A / NOT VERIFIED |
| Motion | PASS / FAIL / N/A / NOT VERIFIED |
| Performance UX | PASS / FAIL / NOT VERIFIED |
| Cross-page consistency | PASS / FAIL / NOT VERIFIED |
| Visual regression | PASS / FAIL / NOT VERIFIED |

---

## 18.6 Verdict

End with exactly one:

- `Block` — one or more HIGH findings remain.
- `Needs changes` — only MEDIUM/LOW findings remain, or required certification gates fail.
- `Approve` — no actionable findings remain for the claimed scope and required verification was completed.
- `Cannot certify` — no blocking finding is proven, but required certification evidence is unavailable.

Do not use `Approve` when required checks are still `NOT VERIFIED`.

---

# 19. Implementation Workflow

When the user asks to fix the findings, use:

```text
Audit
  ↓
Approve fix scope
  ↓
Implement
  ↓
Run component/state verification
  ↓
Run responsive/localization checks
  ↓
Run visual regression when available
  ↓
Re-audit changed scope
  ↓
Certify
```

Never mix discovery and uncontrolled implementation.

---

# 20. Recommended Component Certification Workflow

For a design system or shared component library:

```text
Build component
  ↓
Check design-system tokens
  ↓
Run state matrix
  ↓
Check accessibility
  ↓
Check adaptive behavior
  ↓
Check RTL/localization
  ↓
Runtime verification
  ↓
Visual regression
  ↓
Certify
  ↓
Reuse across product
```

This is preferred for:

- Button
- Input
- Select
- Checkbox
- Radio
- Toggle
- Tabs
- Dropdown
- Tooltip
- Popover
- Modal
- Drawer
- Toast
- Table
- Card
- Empty state
- Pagination
- Date picker
- File upload
- Navigation primitives

---

# 21. Common Universal Mistakes

| Mistake | Better approach |
|---|---|
| Review UI only from source | Inspect runtime when behavior/appearance determines correctness |
| Review only screenshots | Inspect source when implementation determines correctness |
| Introduce a new styling system for one fix | Preserve project conventions |
| Treat unavailable verification as PASS | Mark NOT VERIFIED |
| Fix every local symptom | Fix shared token/primitive when root cause is systemic |
| Design only for default state | Review full component state matrix |
| Test only one viewport | Stress smallest/largest supported layouts |
| Ignore localization | Test long strings and RTL where relevant |
| Use color as the only state cue | Add redundant text/icon/shape cue |
| Add motion by default | Add it only when it improves comprehension |
| Approve with missing certification evidence | Use Cannot certify |
| Mix audit and mutation | Review first, implement only when requested |

---

# 22. Web-Specific Reference Rules

The following rules apply only when the Web Adapter is active.

## 22.1 Native elements first

Use native HTML semantics when possible.

- `<button>` for actions
- `<a href>` for navigation
- real form elements
- real labels

Avoid rebuilding semantics with generic containers.

## 22.2 Focus

Prefer `:focus-visible` for custom focus styling.

Never use `outline: none` without a verified replacement.

## 22.3 Keyboard

Use native keyboard behavior first.

Composite widgets should follow appropriate accessibility patterns.

Avoid positive tabindex.

## 22.4 Target size

Respect WCAG minimum target expectations and project density.

Aim for larger touch areas in touch-heavy contexts when feasible.

## 22.5 Forms

Every input needs a label.

Use meaningful:

- `name`
- `autocomplete`
- `type`
- `inputmode`

Never block paste.

## 22.6 Color

Status should not rely on color alone.

Measure rendered contrast when claiming a contrast failure.

## 22.7 Reduced motion

Honor `prefers-reduced-motion`.

## 22.8 Logical properties

Prefer logical CSS properties in localizable layouts.

## 22.9 Long content

Avoid fixed dimensions for text-bearing containers.

Keep critical actions reachable.

## 22.10 Errors

Errors should be actionable and adjacent to the failing field where possible.

## 22.11 Empty states

Explain:

- what the area is,
- why it is empty,
- what the user can do next.

## 22.12 Type scale

Use a small semantic type scale.

Avoid one-off font sizes unless intentionally documented.

## 22.13 Line height and measure

Use readable line height and reasonable text measure.

## 22.14 Numeric stability

Use tabular numerals for changing numeric values when available.

## 22.15 Truncation

If missing text matters, make the full value discoverable.

## 22.16 Image outlines

Use neutral outlines only if they fit the project's established visual system.

Do not force this rule when the project intentionally uses borderless imagery.

## 22.17 Press feedback

If the project uses scale-on-press, keep it subtle and consistent.

Do not force `0.96` across non-web platforms or projects with a different established motion system.

## 22.18 Transition specificity

Never use `transition: all`.

Specify only the properties that actually animate.

## 22.19 `will-change`

Use only when measured/observed first-frame stutter justifies it.

Do not apply preemptively everywhere.

## 22.20 Icon consistency

Use one optical strategy per surface.

Avoid mixing icon libraries with visibly incompatible stroke conventions.

Use `currentColor` where the icon system supports it.

---

# 23. Optional Web Motion Recipe

Use only when:

- Web Adapter is active,
- the project already uses Motion/Framer Motion or CSS transitions,
- the interaction benefits from motion.

Contextual icon transitions may use:

- opacity
- scale
- blur

but should follow the project's existing motion tokens first.

Do not force one recipe as a universal platform rule.

---

# 24. Optional Figma Annotation Protocol

Apply only if:

- the scope is a Figma/design artifact,
- annotation was requested or is part of the review workflow,
- Figma tooling is available.

Create a top-level review layer named:

`Interface review`

Rules:

- annotations are additive,
- never modify reviewed frames,
- never cover the product design,
- group each finding annotation,
- connect only when useful,
- remove previous review annotations before rerunning.

Suggested annotation card content:

1. Severity
2. Finding number + short title
3. One- or two-sentence action

Exact Figma API implementation is tool-specific and should live in the Figma adapter/tool instructions, not in the universal core.

---

# 25. Certification Examples

## Example A — Source only

```text
Accessibility: PARTIALLY VERIFIED
Runtime interaction: NOT VERIFIED
Responsive: NOT VERIFIED
Design-system compliance: PASS
Visual regression: NOT VERIFIED

Verdict: Cannot certify
```

This is better than inventing a PASS.

## Example B — Web app with browser and tests

```text
Accessibility: PASS
Interaction: PASS
Design-system compliance: PASS
Component states: PASS
Responsive: PASS
RTL: PASS
Motion: PASS
Performance UX: PASS
Visual regression: PASS

Verdict: Approve
```

## Example C — Blocking issue

```text
HIGH: Modal cannot be completed without a pointer.
All other findings are secondary.

Verdict: Block
```

---

# 26. Final Principle

The goal is not to make every interface look the same.

The goal is to make every interface:

- coherent,
- usable,
- accessible,
- consistent with its own design system,
- resilient across states,
- adaptable across supported environments,
- verifiable with evidence,
- safe to reuse and extend.

**Adapt the review to the product. Do not force the product to adapt to the review tool.**
