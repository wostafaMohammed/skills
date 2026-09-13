# UI Review — Usage Guide

## What this skill is

UI Review is a reusable UI/UX review and certification system.

It does not build a product by itself. It acts as a quality gate that can review:

- Web applications
- Mobile applications
- Desktop applications
- Cross-platform applications
- Figma/design artifacts
- Source-only repositories

It adapts its rules to the detected platform and available tools.

---

# 1. Recommended use

Use the skill after:

- building a new page,
- building or changing shared components,
- changing a design system,
- changing navigation,
- redesigning an existing flow,
- before merging a major UI change,
- before releasing a UI-heavy feature.

Recommended flow:

```text
Build
  ↓
Review
  ↓
Fix HIGH
  ↓
Fix MEDIUM
  ↓
Verify
  ↓
Certify
  ↓
Merge / Reuse
```

---

# 2. Review modes

## Quick

Use when you want only the most important issues.

Prompt:

```text
Use the UI Review skill.

Mode: quick
Scope: the current page / feature.

Review only.
Do not change code.
Report only confirmed HIGH and MEDIUM findings.
```

## Full

Use for a serious page/component review.

Prompt:

```text
Use the UI Review skill.

Mode: full
Scope: [page / flow / component set].

Do not change code.

First detect:
- platform
- framework
- styling system
- component library
- design tokens
- available runtime/test capabilities

Then perform the complete review and provide evidence for every finding.
```

## Certify

Use before declaring a component/page ready.

Prompt:

```text
Use the UI Review skill.

Mode: certify
Scope: [page / flow / component set].

Do not change code.

Run every applicable certification gate.
Use runtime/browser/testing tools when available.
Do not convert unavailable verification into PASS.

End with:
Block / Needs changes / Approve / Cannot certify.
```

---

# 3. Fixing findings

After reviewing, do not ask the agent to blindly "fix everything".

Use:

```text
Use the previous UI Review report as the approved change scope.

Implement only findings:
#1
#2
#4

Preserve the existing design system and project conventions.

After implementation:
1. rerun affected tests,
2. verify affected states,
3. rerun responsive/RTL checks when applicable,
4. run visual regression when available,
5. rerun the skill in certify mode.

Do not expand scope without reporting it first.
```

---

# 4. Shared component workflow

Use this for:

- Button
- Input
- Select
- Dropdown
- Modal
- Drawer
- Table
- Card
- Tabs
- Toast
- Tooltip
- Date picker
- File upload
- Navigation components

Prompt:

```text
Use UI Review.

Mode: certify

Scope:
[component names]

Treat these as shared design-system components.

Require:
- design-system compliance
- accessibility
- full component state matrix
- responsive/adaptive behavior
- RTL/localization where applicable
- runtime verification
- cross-page consistency
- visual regression when available

Do not modify code.
```

Only reuse a component broadly after it passes the applicable gates.

---

# 5. Full page workflow

Prompt:

```text
Review this page with UI Review.

Mode: full.

Inspect:
- accessibility
- interaction
- layout
- writing
- typography
- design-system compliance
- component states
- responsive behavior
- RTL/localization
- motion
- performance UX
- cross-page consistency
- regression risk

Use source + runtime evidence where available.

Do not modify anything.
```

---

# 6. Whole application workflow

Do not review a huge application as one vague task.

Use staged scopes:

```text
Stage 1 — Design-system primitives
Stage 2 — Navigation / shell
Stage 3 — Highest-traffic flows
Stage 4 — Secondary modules
Stage 5 — Cross-page consistency
Stage 6 — Release certification
```

For each stage, run `full` or `certify`.

---

# 7. Web projects

Examples:

- React
- Next.js
- Vue
- Angular
- Svelte
- plain HTML/CSS/JS
- Electron/Tauri rendered web UI

Prompt:

```text
Use UI Review.

Detect the web stack first.
Apply the Web Adapter and any detected framework adapter.

Do not assume Tailwind, React, Motion, or any library unless the repository proves it.

Preserve the existing styling and component system.
```

---

# 8. Mobile projects

## iOS

Prompt:

```text
Use UI Review.

Platform: detect and confirm iOS.
Apply the iOS Adapter.

Prioritize:
- VoiceOver
- Dynamic Type
- focus/order
- touch interaction
- safe areas
- keyboard behavior
- platform conventions
- Reduce Motion
- light/dark appearance

Do not apply HTML/CSS rules unless this is actually a web-based surface.
```

## Android

Prompt:

```text
Use UI Review.

Apply the Android Adapter.

Prioritize:
- TalkBack
- semantics
- touch targets
- focus traversal
- text scaling
- system back behavior
- IME/keyboard
- edge-to-edge layout
- platform conventions
```

## Flutter / React Native

Do not force web implementation rules.

Let the skill detect the platform and activate only the matching adapter.

---

# 9. Figma / design review

Prompt:

```text
Use UI Review.

Scope: selected Figma frames.

Mode: full.

Review the design artifact without changing the product frames.

If annotation tools are available:
add findings in a separate "Interface review" layer.

If annotation is unavailable:
return the report only and mark annotation as unavailable.

Do not infer source-code implementation.
```

---

# 10. Source-only review

If no runtime/browser is available:

```text
Use UI Review.

Perform the strongest source-backed review possible.

For anything that depends on runtime appearance or behavior:
mark it NOT VERIFIED.

Do not infer PASS.
Do not manufacture runtime evidence.
```

Expected verdict may be:

`Cannot certify`

This is correct when the code looks good but required runtime proof is unavailable.

---

# 11. RTL and Arabic products

For Arabic/English products, add:

```text
RTL/localization is release-critical.

Test:
- English
- Arabic
- long strings
- direction-aware spacing
- navigation icons
- truncation
- text growth
- tables/forms/modals
- number/date formatting

Do not certify RTL without runtime evidence when runtime tools are available.
```

---

# 12. Visual regression

When screenshots or visual testing are available:

```text
Compare before and after.

Classify every meaningful difference as:
- Intended
- Approved side effect
- Regression
- Unable to determine

Do not treat pixel difference alone as a defect.
```

When tooling is not available:

`Visual regression: NOT VERIFIED`

---

# 13. Recommended independent-review workflow

For important releases, use two agents if available.

Example:

```text
Agent A → implementation
Agent B → independent review/certification
```

The certifying agent should not assume the implementer's report is correct.

It should verify the evidence independently.

---

# 14. Best prompt for everyday use

```text
Use UI Review.

Mode: full.

Scope: the UI changes in the current branch.

Review only. Do not modify code.

First detect the platform, framework, styling system, component library,
design tokens, localization/RTL support, and available runtime/test capabilities.

Then review all applicable domains.

Every finding must contain evidence.
Do not infer runtime behavior from source when runtime verification is required.
Do not mark unavailable checks as PASS.

Prioritize root causes and shared design-system fixes over leaf symptoms.

End with:
Block / Needs changes / Approve / Cannot certify.
```

---

# 15. Best prompt before release

```text
Run UI Review in certify mode.

Scope: all UI changes included in this release.

Certification must cover every applicable gate:
- Accessibility
- Interaction
- Design-system compliance
- Component states
- Responsive/adaptive behavior
- Localization/RTL
- Motion
- Performance UX
- Cross-page consistency
- Visual regression

Use source, runtime, browser, screenshots, and tests when available.

Any unavailable required evidence must be marked NOT VERIFIED.

Do not modify the implementation.

Return the Certification Matrix and final verdict.
```

---

# 16. When not to use it

Do not use this skill as the primary tool for:

- backend architecture review,
- database security,
- API correctness,
- infrastructure security,
- business logic auditing,
- financial/accounting correctness.

It can review the UI that exposes those systems, but not replace their specialist audits.

---

# 17. Recommended rule

For serious products:

> No shared component becomes "stable" and no UI-heavy release becomes "approved" until it passes the applicable certification gates.

This prevents UI drift from spreading across the application.

---

# 18. Simple mental model

Think of the skill as:

```text
Designer
+ Accessibility reviewer
+ UX reviewer
+ Design-system auditor
+ QA reviewer
+ Runtime verifier
+ Release gate
```

It is not tied to one framework.

The adapters make it platform-aware.

---

# 19. Final workflow

```text
DETECT
  ↓
REVIEW
  ↓
EVIDENCE
  ↓
PRIORITIZE
  ↓
FIX
  ↓
VERIFY
  ↓
REGRESSION CHECK
  ↓
CERTIFY
```

That is the intended way to use UI Review.
