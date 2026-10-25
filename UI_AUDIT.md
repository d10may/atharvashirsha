# UI & Technical Audit Report

Comprehensive technical quality, accessibility, performance, and implementation integrity audit for the Bhajan Jam / Atharvashirsha web application.

---

## Audit Health Score

| # | Dimension | Score | Key Finding |
|---|-----------|:-----:|-------------|
| 1 | **Accessibility (A11y)** | 2/4 | Severe contrast failures in light mode (down to 1.9:1) and unannounced progress bar |
| 2 | **Performance** | 3/4 | Lean vanilla implementation; missing font preconnects and smooth-scroll layout churn |
| 3 | **Theming** | 2/4 | Inconsistent token contrast across light/dark modes; stereotypical beige palette |
| 4 | **Responsive Design** | 3/4 | Fluid typography and large 60px controls; card grid overflows on screens < 360px |
| 5 | **Implementation Integrity** | 2/4 | 5 verified AI slop patterns (eyebrow chips, side-tab accents, top stripes, clashing borders) |
| **Total** | | **12/20** | **Acceptable (Significant work needed)** |

---

## Implementation Integrity Verdict

**Status: Conditional Pass with Remediation Required**

The recitation player core ([`atharvashirsha.html`](atharvashirsha.html) and [`gayatri-aarti.html`](gayatri-aarti.html)) expresses authentic domain functionality tailored to sacred chanting (mātrā-based pacing, Devanagari/IAST synchronicity, and dedicated TV projector mode). 

However, the landing surface ([`index.html`](index.html)) and recent visual styling heavily exhibit generic AI-generated design tells detected by the Impeccable mechanical scanner:
1. **Hero eyebrow chip**: Tracked uppercase pill (`.hero-tag`) floating directly above the `<h1>`.
2. **Side-tab accent borders & top stripes**: `.card::before` gradient stripe and `.card-excerpt` with `border-left: 3px`.
3. **Clashing border accents on rounded containers**: `#sheetPanel` mixing `border-top: 2px` with large `border-radius: 22px`.
4. **Zero-offset colored halos**: Defaulting to `text-shadow: 0 0 18px` for word highlights.

---

## Executive Summary

- **Audit Health Score**: **12/20** (Acceptable)
- **Total Issues Found**: 9 (0 P0, 4 P1, 3 P2, 2 P3)
- **Top Critical Issues**:
  1. Low-contrast unchanted text and secondary labels in Light Mode (1.9:1–3.2:1 vs 4.5:1 required).
  2. Silent progress bar (`#progress`) missing ARIA role and live region announcements for assistive technologies.
  3. Settings modal (`#sheet`) lacks focus trapping and escape focus return.
  4. Multiple AI layout artifacts (`.hero-tag`, `.card::before`, side borders) degrading aesthetic credibility.
- **Recommended Next Step**: Run `$impeccable harden` for accessibility and contrast remediation, followed by `$impeccable distill` to eliminate AI styling clichés.

---

## Detailed Findings by Severity

### [P1] Light Mode Text Contrast Violations
- **Location**: [`index.html:24-33`](index.html), [`atharvashirsha.html:24-33`](atharvashirsha.html), [`gayatri-aarti.html:24-33`](gayatri-aarti.html)
- **Category**: Accessibility / Theming
- **Impact**: In bright ambient light or for users with low vision, secondary text (`.brand-sub`, `.hero-desc`, `.card-excerpt`, `.pill`) and upcoming stotra text (`.w`) are unreadable. Contrast drops as low as 1.9:1 (`#B9AA90` on `#F6EBD6`) and 2.9:1 (`#8F806D` on `#EEDFC3`).
- **WCAG**: WCAG 2.1 AA — 1.4.3 Contrast (Minimum)
- **Recommendation**: Recalibrate `--ink-dim` in light mode to at least `#5C4D3C` (contrast 6.1:1 on `#F6EBD6`) and `--gold` to `#75530B`. Ensure unchanted words (`.w`) remain readable at >= 4.5:1.
- **Suggested Command**: `$impeccable colorize`

### [P1] Missing Screen Reader Semantics on Recitation Progress Bar
- **Location**: [`atharvashirsha.html:181`](atharvashirsha.html), [`gayatri-aarti.html:181`](gayatri-aarti.html)
- **Category**: Accessibility
- **Impact**: Visually impaired users using screen readers get zero indication of how many verses have been recited or where they are in the hymn. The `#progress` container consists solely of empty `<i>` tags without text or attributes.
- **WCAG**: WCAG 2.1 AA — 4.1.2 Name, Role, Value
- **Recommendation**: Add `role="progressbar"`, `aria-valuenow="${v+1}"`, `aria-valuemin="1"`, `aria-valuemax="${DATA.length}"`, and `aria-label="Recitation progress: verse ${v+1} of ${DATA.length}"` to `#progress`.
- **Suggested Command**: `$impeccable harden`

### [P1] Accessible Dialog Focus Management in Settings Sheet
- **Location**: [`atharvashirsha.html:210-247`](atharvashirsha.html), [`gayatri-aarti.html:210-247`](gayatri-aarti.html)
- **Category**: Accessibility
- **Impact**: When the Settings sheet opens (`role="dialog"`), keyboard focus remains behind the modal backdrop, allowing users to tab into background buttons while the modal is open. When closed, focus is lost rather than returning to `#settingsBtn`.
- **WCAG**: WCAG 2.1 AA — 2.4.3 Focus Order, 2.1.2 No Keyboard Trap
- **Recommendation**: Trap focus within `#sheetPanel` when open, autofocus the first interactive control, and return focus to `#settingsBtn` upon dismissal.
- **Suggested Command**: `$impeccable harden`

### [P1] Horizontal Overflow on Small Mobile Devices (< 360px)
- **Location**: [`index.html:131-136`](index.html)
- **Category**: Responsive Design
- **Impact**: On compact mobile screens (e.g. 320px–350px width), `.grid` with `grid-template-columns: repeat(auto-fit, minmax(320px, 1fr))` plus 20px padding causes cards to overflow the screen horizontally, creating unwanted horizontal scrollbars.
- **WCAG**: WCAG 2.1 AA — 1.4.10 Reflow
- **Recommendation**: Adjust grid min-width from `minmax(320px, 1fr)` to `minmax(min(100%, 280px), 1fr)`.
- **Suggested Command**: `$impeccable adapt`

### [P2] Artificial AI Design Markers (Eyebrow Chips & Accent Stripes)
- **Location**: [`index.html:112-136`](index.html), [`index.html:149-161`](index.html)
- **Category**: Implementation Integrity
- **Impact**: The UI displays generic AI SaaS aesthetics (tracked pill label `.hero-tag` right above the heading, absolute 4px top gradient stripes on cards, and 3px left border on card excerpts). These detract from the authentic sacred devotional aesthetic of the hymns.
- **Recommendation**: Remove `.hero-tag` or integrate it as a subtle subtitle; replace card pseudo-element gradient stripes with authentic clean typography and refined border elevation.
- **Suggested Command**: `$impeccable distill`

### [P2] Incomplete `prefers-reduced-motion` Coverage
- **Location**: [`atharvashirsha.html:94-95`](atharvashirsha.html), [`atharvashirsha.html:327`](atharvashirsha.html)
- **Category**: Accessibility
- **Impact**: While the mic pulse is disabled for reduced-motion users, `scrollIntoView({ behavior: "smooth" })` and word lift transforms (`transform: translateY(-.06em) scale(1.08)`) continue to trigger repeatedly on every single word step.
- **WCAG**: WCAG 2.1 AA — 2.3.3 Animation from Interactions
- **Recommendation**: Check `window.matchMedia('(prefers-reduced-motion: reduce)').matches` before invoking smooth scrolling (fallback to `"auto"`), and disable `.w.now` scale/translate effects under `@media (prefers-reduced-motion: reduce)`.
- **Suggested Command**: `$impeccable harden`

### [P2] Non-Button Clickable Words Lack Keyboard Accessibility
- **Location**: [`atharvashirsha.html:300`](atharvashirsha.html), [`gayatri-aarti.html:300`](gayatri-aarti.html)
- **Category**: Accessibility
- **Impact**: Mouse and touch users can tap any Sanskrit word to jump playback directly to that word, but keyboard-only users cannot focus or activate individual words.
- **WCAG**: WCAG 2.1 AA — 2.1.1 Keyboard
- **Recommendation**: Provide keyboard step navigation commands or make phrase units focusable when navigated via keyboard.
- **Suggested Command**: `$impeccable harden`

### [P3] Missing Preconnect for Google Fonts
- **Location**: [`index.html:7`](index.html), [`atharvashirsha.html:7`](atharvashirsha.html), [`gayatri-aarti.html:7`](gayatri-aarti.html)
- **Category**: Performance
- **Impact**: Delays initial webfont download by 100–250ms on mobile connections, causing a brief flash of unstyled Sanskrit fallback font.
- **Recommendation**: Add `<link rel="preconnect" href="https://fonts.googleapis.com">` and `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`.
- **Suggested Command**: `$impeccable optimize`

### [P3] Smooth-Scroll DOM Churn During Rapid Playback
- **Location**: [`atharvashirsha.html:327`](atharvashirsha.html), [`gayatri-aarti.html:327`](gayatri-aarti.html)
- **Category**: Performance
- **Impact**: When playback speed is set to 2.5×–5×, triggering `scrollIntoView({ behavior: 'smooth' })` every 100–150ms creates conflicting scroll animations in the browser engine.
- **Recommendation**: Throttle auto-scrolling to trigger only when the active verse or word crosses viewport thresholds rather than on every word tick.
- **Suggested Command**: `$impeccable optimize`

---

## Patterns & Systemic Issues

1. **Token Contrast Calibration**: Light mode tokens (`--ink-dim`, `--ink-done`, `--gold`) were created without verifying WCAG contrast against the light page background (`#F6EBD6` / `#EEDFC3`), resulting in widespread legibility deficits.
2. **AI Slop Clichés on Landing Pages**: Standardizing on cards with side tabs, top accent bars, and pill chips above headers rather than crafting authentic editorial presentation for devotional literature.

---

## Positive Findings

1. **Zero Runtime Dependencies**: The app is built with pure semantic HTML5, CSS custom properties, and native vanilla JS (~40KB total footprint). It boots instantly and has zero bundle overhead.
2. **Touch Sizing on Core Controls**: Main recitation controls (`.ctrl`, `#resetBtn`, `#playBtn`) exceed touch target standards at 60×60px, making them accessible and easy to use for elders.
3. **Thoughtful Devotional Features**: Domain-specific features (mātrā metric timing calculation, Devanagari/Roman simultaneous view, TV fullscreen presentation) are well-architected.
4. **Synchronized Theme Storage**: Dark/light theme state is cleanly shared across all three pages via `localStorage`.

---

## Recommended Actions

In priority order:

1. **[P1] `$impeccable harden`**: Fix contrast ratios across light and dark tokens to meet WCAG AA (>= 4.5:1), add ARIA semantics to `#progress`, implement focus trapping for the Settings dialog, and add `prefers-reduced-motion` guards to scrolling.
2. **[P1] `$impeccable adapt`**: Update mobile grid definitions in `index.html` to eliminate overflow on screens smaller than 360px.
3. **[P2] `$impeccable distill`**: Strip out AI-generated visual tells (hero eyebrow pill, card top gradient stripes, card excerpt left border) in favor of clean devotional typography and sacred aesthetics.
4. **[P3] `$impeccable optimize`**: Add font preconnect tags and throttle high-speed auto-scrolling.
5. **`$impeccable polish`**: Final quality review and visual harmonization across all three pages.
