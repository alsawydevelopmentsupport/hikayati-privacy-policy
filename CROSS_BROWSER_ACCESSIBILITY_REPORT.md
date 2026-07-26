# Cross-Browser & Accessibility Validation Report — Privacy Policy Page

Date: 2026-07-26
Page: `index.html` (+ `style.css`), served at `https://alsawydevelopmentsupport.github.io/hikayati-privacy-policy/`

## HTML validation — W3C Nu HTML Checker

Submitted the full rendered `index.html` to `validator.w3.org/nu`.

**Result: 0 errors, 0 warnings.**

(One real issue was found and fixed during this process: the initial inline-SVG favicon used an unencoded `data:` URI with raw spaces, which the validator correctly rejected — `Illegal character after "ta:". Space is not allowed.` Fixed by switching to a base64-encoded `data:image/svg+xml;base64,...` URI, which re-validated clean.)

## CSS validation — W3C CSS Validator (Jigsaw), profile css3svg

**Result: 0 errors, `"validity": true`.**

5 informational-only warnings, all expected and harmless:
- 1× `-webkit-font-smoothing is a vendor extension` — intentional, standard progressive-enhancement prefix for font rendering.
- 4× `CSS variables are currently not statically checked` — the validator's standard disclaimer about `var()`/custom properties; not an actual defect.

## WCAG contrast ratios (computed, relative luminance per WCAG 2.x formula)

| Pair | Ratio | WCAG AA (4.5:1 text / 3:1 large) |
|---|---|---|
| Light: body text on card background | 9.28:1 | ✅ Pass (AAA) |
| Light: heading green on card background | 6.58:1 | ✅ Pass (AAA for large text, AA for normal) |
| Light: white hero text on green gradient | 5.13:1 | ✅ Pass AA |
| Dark: body text on dark background | 15.76:1 | ✅ Pass (AAA) |
| Dark: secondary text on dark card | 9.88:1 | ✅ Pass (AAA) |
| Dark: heading green on dark card | 9.44:1 | ✅ Pass (AAA) |

All text/background pairs used on the page clear WCAG AA; most clear AAA.

## Accessibility checks performed

- **Semantic structure**: `<header>`, `<nav>` (×2, labelled), `<main>`, `<section>` per policy topic, `<footer>` — verified via the accessibility tree (not just visual layout).
- **Heading hierarchy**: single `h1` per language block, all section headings are `h2` with no skipped levels.
- **Keyboard navigation**: the language switch is two native `<input type="radio">` elements (visually hidden via clip technique, not `display:none`, so they remain focusable). Verified by scripting focus onto `#lang-ar`, sending `ArrowRight`, and confirming `#lang-en` became `checked` and its content pane's computed `display` changed from `none` to `block` — full switch happens via keyboard alone, no mouse required.
- **No-JS toggle**: the language switch uses only a CSS `:checked ~` sibling selector — verified working with JavaScript entirely absent from the page (there is no `<script>` tag in `index.html`).
- **Skip link**: `.skip-link` present, targets `#main`, becomes visible on focus.
- **Focus indicators**: `:focus-visible` rule applied globally plus a dedicated rule for the language-switch labels, using a high-contrast outline color distinct from the theme.
- **prefers-color-scheme**: verified programmatically — with the browser's emulated color scheme set to `dark`, computed `body` background/text resolved to the dark-palette values, confirming the media query is live (not just present in source).
- **prefers-reduced-motion / prefers-contrast**: rules included to disable smooth-scroll and flatten transitions/borders respectively for users who request them.
- **Responsive layout**: verified no horizontal scroll at 375px viewport width (mobile), and confirmed the table-of-contents column count changes from 2→1 below the 700px breakpoint via computed style.

## Cross-browser testing — what was actually verified vs. inferred

**Actually rendered and interacted with** (via the available Chromium-based automated browser): page load, both language panes, the CSS-only language toggle (mouse click and keyboard), responsive resize to mobile width, dark-mode media query, console (zero errors), and focus/keyboard behavior.

**Not independently tested in this environment** (no real Firefox, Safari, or Android Chrome instance was available to drive): actual rendering in Firefox, Safari (macOS/iOS), or Android Chrome. Being transparent about this rather than claiming a false positive.

**Why the risk is low regardless**: every CSS/HTML feature used on this page is long-established and has had universal support across Chrome, Edge, Firefox, and Safari for several years — general sibling combinator (`~`), CSS custom properties, Flexbox, `position: sticky`, `prefers-color-scheme`, `prefers-reduced-motion`, `prefers-contrast`, and `scroll-behavior`. Nothing on the page depends on a cutting-edge or experimental API, a JavaScript framework, or a browser-specific behavior. Edge is Chromium-based and will render identically to the tested browser. If you want, I can also load the live GitHub Pages URL once published in your own Chrome/Edge/Firefox for a final visual pass — but I cannot drive an actual Safari or Android Chrome instance from this environment.

## Zero external dependencies — confirmed

`index.html` and `style.css` are the only two files needed to serve the page. No CDN links, no web fonts, no analytics scripts, no cookies, no third-party requests of any kind. Verified by inspecting the source: the only `<link>` tags are `canonical`, the base64-embedded favicon, and the same-origin `style.css`.
