# Task 1: Corrected Token Inventory (Analysis Only)
## Descriptive Inventory of Token Usage vs DEV_DesignTokensSystem_1.4

**Date:** 2026-01-30
**Scope:** Purely descriptive analysis - no recommendations
**Authority:** DEV_DesignTokensSystem_1.4 + V10.2.1 Architecture
**Note:** This replaces invalid recommendation sections from previous reports

---

## PART A: TOKEN CATEGORY INVENTORIES (1.4 Checklist)

### A.1 COLORS (11-Color Palette from 1.4)

| Token Name (1.4 Spec) | Used in Code Today? | Where It Appears | Notes |
|----------------------|---------------------|------------------|-------|
| **THEME COLORS (5)** |
| `--theme-primary` (#000000) | Partially | fortunetell-frontend HC mode (surface-page), various text-strong roles | Code uses similar dark grays (#0f172a) that are NOT this exact value |
| `--theme-secondary` (#3f3f3f) | No | Not observed in either repo | Currently unused; code uses different gray values |
| `--theme-tertiary` (#7f7f7f) | No | Not observed in either repo | Currently unused; code uses different gray values |
| `--theme-accent` (#b7b7b7) | No | Not observed in either repo | Currently unused; code uses different gray values |
| `--theme-background` (#ffffff) | Yes | fortunetell-frontend (neutral-bg-0), simple-budget-site (bg-white), extensive page/card backgrounds | Exact match, heavily used |
| **SEMANTIC COLORS (6)** |
| `--semantic-color-safe` (#10b981) | Yes | fortunetell-frontend (brand-green, financial-green, bg-safe, button-primary-bg), simple-budget-site (bg-emerald-600, text-emerald-600) | Exact match, heavily used |
| `--semantic-color-safe-hover` | Yes | fortunetell-frontend (bg-safe-hover, button-primary-bg-hover) | Derived from safe color, used in hover states |
| `--semantic-color-warning` (#f6e823) | Yes | fortunetell-frontend (brand-yellow, financial-yellow, bg-warning) | Exact match, used for warning states |
| `--semantic-color-warning-hover` | Yes | fortunetell-frontend (bg-warning-hover) | Derived from warning color, used in hover states |
| `--semantic-color-danger` (#ff1a1a) | Yes | fortunetell-frontend (brand-red, financial-red, bg-danger, button-danger-bg), simple-budget-site (text-rose-600 similar) | Exact match, used for danger/destructive actions |
| `--semantic-color-danger-hover` | Yes | fortunetell-frontend (bg-danger-hover, button-danger-bg-hover) | Derived from danger color, used in hover states |

**Coverage:** fortunetell-frontend tokens.css, globals.css, component modules; simple-budget-site HTML (Tailwind utilities), global.css

---

### A.2 TYPOGRAPHY - FONT SIZES (8 Sizes from 1.4)

| Token Name (1.4 Spec) | Value | Used in Code Today? | Where It Appears | Notes |
|----------------------|-------|---------------------|------------------|-------|
| `--font-size-xs` | 12px | Yes | fortunetell-frontend (CalendarMonth labels, DockBar, HistoryPanel, TxList timestamps) | Exact match; 15+ occurrences |
| `--font-size-sm` | 14px | Yes | fortunetell-frontend (CalendarMonth, HistoryPanel secondary text) | Exact match; 12+ occurrences |
| `--font-size-md` | 16px | Yes | fortunetell-frontend (SettingsView, TxForm, DockBar, default body text) | Exact match; 25+ occurrences |
| `--font-size-lg` | 18px | Yes | fortunetell-frontend (HistoryPanel, TxForm labels), simple-budget-site (global.css, Tailwind text-lg) | Exact match; 18+ occurrences |
| `--font-size-xl` | 20px | Yes | fortunetell-frontend (AppHeader, AddTransactionPanel headings), simple-budget-site (Tailwind text-xl, global.css h3) | Exact match; 10+ occurrences |
| `--font-size-2xl` | 24px | Yes | fortunetell-frontend (LoginForm title, AccountView balance, page.module.css headings) | Exact match; 8+ occurrences |
| `--font-size-3xl` | 30px | Yes | fortunetell-frontend (SettingsView title, page.module.css hero), simple-budget-site (global.css h2) | Exact match; 6+ occurrences |
| `--font-size-4xl` | 36px | Yes | fortunetell-frontend (page.module.css hero headings), simple-budget-site (Tailwind text-4xl, global.css h1) | Exact match; 3+ occurrences |

**Coverage:** fortunetell-frontend component modules, simple-budget-site global.css + HTML (Tailwind utilities)

---

### A.3 TYPOGRAPHY - FONT WEIGHTS (2 Weights from 1.4)

| Token Name (1.4 Spec) | Value | Used in Code Today? | Where It Appears | Notes |
|----------------------|-------|---------------------|------------------|-------|
| `--font-weight-normal` | 400 | Yes | fortunetell-frontend (page.module.css, AppHeader, AccountView body text), simple-budget-site (global.css body, Tailwind font-normal) | Exact match; 15+ occurrences |
| `--font-weight-bold` | 700 | Yes | fortunetell-frontend (LoginForm, CalendarMonth, TxList headings), simple-budget-site (global.css h1-h3, Tailwind font-bold) | Exact match; 8+ occurrences |

**Notes:** Code ALSO uses intermediate weights (500, 600) that are NOT in 1.4 spec - see out-of-policy section

**Coverage:** fortunetell-frontend component modules, simple-budget-site global.css + HTML

---

### A.4 SPACING (10-Step Scale from 1.4)

| Token Name (1.4 Spec) | Value | Used in Code Today? | Where It Appears | Notes |
|----------------------|-------|---------------------|------------------|-------|
| `--space-0` | 0 | Yes | fortunetell-frontend (explicit margin:0, padding:0 resets across components) | Exact match; 20+ occurrences |
| `--space-2xs` | 4px (0.25rem) | Yes | fortunetell-frontend (CalendarMonth dots, SettingsView tight gaps, DockBar) | Exact match; 8+ occurrences |
| `--space-xs` | 8px (0.5rem) | Yes | fortunetell-frontend (LoginForm, CalendarMonth gaps, TxForm) | Exact match; 18+ occurrences |
| `--space-sm` | 12px (0.75rem) | Yes | fortunetell-frontend (CalendarMonth, AddTransactionPanel, DockBar padding) | Exact match; 22+ occurrences |
| `--space-md` | 16px (1rem) | Yes | fortunetell-frontend (TxForm, CalendarMonth, SettingsView default padding), simple-budget-site (Tailwind p-4, px-4) | Exact match; 35+ occurrences |
| `--space-lg` | 24px (1.5rem) | Yes | fortunetell-frontend (LoginForm, AddTransactionPanel section padding), simple-budget-site (Tailwind py-6, gap-6) | Exact match; 15+ occurrences |
| `--space-xl` | 32px (2rem) | Yes | fortunetell-frontend (SettingsView, AddTransactionPanel large gaps), simple-budget-site (Tailwind py-8) | Exact match; 12+ occurrences |
| `--space-2xl` | 40px (2.5rem) | Yes | fortunetell-frontend (SettingsView, DockBar calculations - rare) | Exact match; 3 occurrences |
| `--space-*` | 48px (3rem) | Yes | fortunetell-frontend (ShellLayout.module.css header height calculation as --space-12) | Exact match; 1 occurrence |
| (1.4 has more?) | ... | Unknown | Not inventoried | 1.4 may define additional spacing steps beyond what was observed |

**Notes:** Code also uses non-spec values (20px/1.25rem, 28px/1.75rem from Tailwind) - see out-of-policy section

**Coverage:** fortunetell-frontend component modules, simple-budget-site global.css + HTML (Tailwind)

---

### A.5 BORDER RADII (from 1.4 - exact token names/values not provided in checklist)

**Note:** 1.4 checklist did not specify exact radius token names/values. Based on fortunetell-frontend tokens.css:

| Token Name (Observed) | Value | Used in Code Today? | Where It Appears | Notes |
|----------------------|-------|---------------------|------------------|-------|
| `--radius-sm` | 4px (0.25rem) | Yes | fortunetell-frontend (SettingsView badges, page.module.css small cards, globals.css inputs) | Heavily used |
| `--radius-md` | 8px (0.5rem) | Yes | fortunetell-frontend (CalendarMonth, TxList, TxForm, AddTransactionPanel cards) | Heavily used |
| `--radius-lg` | 12px (0.75rem) | Yes | fortunetell-frontend (BetaNoticeModal), simple-budget-site (Tailwind rounded-xl) | Moderately used |
| `--radius-pill` | 9999px | Yes | fortunetell-frontend (CalendarMonth circular day dots) | Used for circular elements |
| `--radius-card` | 16px (1rem) | Rare | simple-budget-site (Tailwind rounded-2xl, custom rounded-card class) | Observed in static site |
| `--radius-sharp` | 12px (0.75rem) | Duplicate | fortunetell-frontend tokens.css | DUPLICATE of --radius-lg (same value) |

**Coverage:** fortunetell-frontend tokens.css, component modules; simple-budget-site HTML (Tailwind utilities), global.css

---

### A.6 BORDERS - WIDTHS & COLORS

**Border Widths (Observed):**

| Value | Used in Code Today? | Where It Appears | Notes |
|-------|---------------------|------------------|-------|
| 0 (border: none) | Yes | fortunetell-frontend (SettingsView, AccountView explicit removals) | Explicit no-border declarations |
| 1px | Yes | fortunetell-frontend (cards, inputs, forms, globals.css buttons), simple-budget-site (Tailwind border) | Standard border width |

**Border Colors (Token Aliases to 11-Color Palette):**

| Token Name (Observed) | Type | Used in Code Today? | Where It Appears | Notes |
|----------------------|------|---------------------|------------------|-------|
| `--app-border` | Alias | Yes | fortunetell-frontend (cards, inputs, general UI separation) | Aliases to structural color |
| `--border-subtle` | Alias | Yes | fortunetell-frontend (dividers, light borders) | Aliases to structural color |
| `--button-secondary-border` | Alias | Yes | fortunetell-frontend (secondary button outlines) | Aliases to semantic/structural color |
| `--button-danger-border` | Alias | Yes | fortunetell-frontend (danger button outlines) | Aliases to semantic danger |
| `--input-border` | Alias | Yes | fortunetell-frontend (input field borders) | Aliases to structural color |

**Coverage:** fortunetell-frontend globals.css, tokens.css, component modules; simple-budget-site HTML

---

### A.7 ICON TOKENS (from 1.4)

| Token Name (1.4 Spec) | Value | Used in Code Today? | Where It Appears | Notes |
|----------------------|-------|---------------------|------------------|-------|
| **ICON SIZES** |
| `--icon-size-sm` | 16px | Yes | fortunetell-frontend (tokens.css, globals.css .icon-sm utility) | Aliases to --font-size-md |
| `--icon-size-md` | 20px | Yes | fortunetell-frontend (tokens.css, globals.css .icon-md utility) | Aliases to --font-size-xl |
| `--icon-size-lg` | 30px | Yes | fortunetell-frontend (tokens.css, globals.css .icon-lg utility, DockBar icons) | Aliases to --font-size-2xl, canonical dock size |
| **ICON COLORS** |
| `--icon-color-default` | Alias | Yes | fortunetell-frontend tokens.css | Aliases to text color token |
| `--icon-color-strong` | Alias | Yes | fortunetell-frontend tokens.css | Aliases to strong text color |
| `--icon-color-inverse` | Alias | Yes | fortunetell-frontend tokens.css | Aliases to on-dark text color |

**Icon Implementation:** fortunetell-frontend uses inline SVG components (Material Design 24×24 viewBox) with `fill="currentColor"` for CSS-driven color

**Coverage:** fortunetell-frontend tokens.css, globals.css, icons directory

---

### A.8 BUTTON & INPUT PRIMITIVES (from 1.4)

**Button Role Tokens:**

| Token Name (Observed) | Type | Used in Code Today? | Where It Appears | Notes |
|----------------------|------|---------------------|------------------|-------|
| `--button-primary-*` | bg/text/border/hover | Yes | fortunetell-frontend (globals.css .btn-primary, component usage) | Maps to semantic-color-safe |
| `--button-secondary-*` | bg/text/border/hover | Yes | fortunetell-frontend (globals.css .btn-secondary, component usage) | Maps to neutral/structural colors |
| `--button-danger-*` | bg/text/border/hover | Yes | fortunetell-frontend (globals.css .btn-danger, component usage) | Maps to semantic-color-danger |
| `--button-radius` | Alias | Yes | fortunetell-frontend globals.css | Aliases to radius token |

**Input Primitives:**

| Token Name (Observed) | Type | Used in Code Today? | Where It Appears | Notes |
|----------------------|------|---------------------|------------------|-------|
| `--input-border` | Alias | Yes | fortunetell-frontend (globals.css input styles, tokens.css) | Aliases to border-subtle |
| `--input-radius` | Alias | Yes | fortunetell-frontend (globals.css input styles, tokens.css) | Aliases to radius-sm |
| `--input-focus-border` | Alias | Yes | fortunetell-frontend tokens.css | Aliases to semantic-color-safe |
| `--input-bg` | Alias | Yes | fortunetell-frontend tokens.css | Aliases to background color |

**Coverage:** fortunetell-frontend globals.css, tokens.css
**Note:** simple-budget-site uses custom .btn-primary and .input classes in global.css (not token-based)

---

### A.9 LAYOUT PRIMITIVES (from 1.4 - specifics not detailed in checklist)

**Note:** 1.4 checklist mentioned layout primitives (page width, padding, section padding, breakpoints) but did not specify exact token names/values.

**Observed Layout Patterns (NOT tokens):**

| Pattern | Value(s) Observed | Where It Appears | Notes |
|---------|-------------------|------------------|-------|
| Content max-width | 48rem, 64rem, 80rem | simple-budget-site (.max-w-3xl, .max-w-5xl, .max-w-6xl Tailwind) | NOT tokenized, Tailwind utilities |
| Page padding | 16px, 24px | Both repos (px-4, px-6 Tailwind; component padding) | NOT tokenized, inline values |
| Section vertical spacing | ~64px | simple-budget-site (.section class) | NOT tokenized, global.css |
| Header height | calc-based | fortunetell-frontend (ShellLayout.module.css) | Calculated from spacing + font-size tokens |
| Safe area insets | env(safe-area-inset-bottom) | fortunetell-frontend (DockBar.module.css) | Uses CSS env() function |

**Status:** Layout patterns exist in code but are NOT tokenized per 1.4 token system

---

## PART B: OUT-OF-POLICY VALUES INVENTORY

### B.1 OUT-OF-POLICY COLORS (Not in 11-Color Palette)

**Colors in Code That Conflict with 1.4:**

| Hex Value | Current Role in Code | Conflict Type | Where It Appears |
|-----------|----------------------|---------------|------------------|
| **NEUTRAL GRAYS (not in 11-color palette)** |
| `#0f172a` | neutral-text-strong, dark text | Not in 11-color palette | fortunetell-frontend tokens.css, simple-budget-site (text-slate-900) |
| `#334155` | neutral-text-body, body text | Not in 11-color palette | fortunetell-frontend tokens.css, simple-budget-site (text-slate-700) |
| `#475569` | neutral-text-muted, muted text | Not in 11-color palette | fortunetell-frontend tokens.css, simple-budget-site (text-slate-600) |
| `#64748b` | neutral-text-low, low-emphasis text | Not in 11-color palette | fortunetell-frontend tokens.css, simple-budget-site (text-slate-500) |
| `#1e293b` | Dark gray | Not in 11-color palette | simple-budget-site (text-slate-800) |
| `#e2e8f0` | neutral-border-subtle, light borders | Not in 11-color palette | fortunetell-frontend tokens.css, simple-budget-site (border-slate-200) |
| `#cbd5f5` | neutral-border-strong, strong borders | Not in 11-color palette | fortunetell-frontend tokens.css |
| `#f9fafb` | neutral-bg-1, very light gray background | Not in 11-color palette | fortunetell-frontend tokens.css |
| `#f3f4f6` | neutral-bg-2, light gray background | Not in 11-color palette | fortunetell-frontend tokens.css |
| `#f8fafc` | Very light gray | Not in 11-color palette | simple-budget-site (bg-slate-50) |
| `#f1f5f9` | Light gray | Not in 11-color palette | simple-budget-site (bg-slate-100) |
| **BRAND VARIATIONS (not in 11-color palette)** |
| `#047857` | brand-green-dark, dark green variant | Not in 11-color palette | fortunetell-frontend tokens.css, simple-budget-site (text-emerald-700) |
| `#6ee7b7` | brand-green-light, light green variant | Not in 11-color palette | fortunetell-frontend tokens.css |
| `#065f46` | Darker emerald | Not in 11-color palette | simple-budget-site (text-emerald-800) |
| **ADDITIONAL SEMANTIC COLORS (not in 11-color palette)** |
| `#e11d48` | Rose/pink red variant | Not in 11-color palette | simple-budget-site (text-rose-600) |
| `#d97706` | Amber/orange warning variant | Not in 11-color palette | simple-budget-site (text-amber-600) |
| **TINTED BACKGROUNDS (not in 11-color palette)** |
| `#ecfdf5` | Emerald tint background | Not in 11-color palette | simple-budget-site (bg-emerald-50) |
| `#d1fae5` | Emerald light background | Not in 11-color palette | simple-budget-site (bg-emerald-100, border-emerald-100) |
| `#a7f3d0` | Emerald lighter border | Not in 11-color palette | simple-budget-site (border-emerald-200) |
| **DARK MODE / LANDING COLORS (not in 11-color palette)** |
| `#0b1120` | neutral-bg-dark-0, dark landing background | Not in 11-color palette | fortunetell-frontend tokens.css |
| `#020617` | neutral-bg-dark-1, darker landing background | Not in 11-color palette | fortunetell-frontend tokens.css |
| **HIGH-CONTRAST MODE OVERRIDES (not in 11-color palette)** |
| `#050816` | HC surface-subtle | Not in 11-color palette | fortunetell-frontend tokens.css HC mode |
| `#e5e7eb` | HC text-body | Not in 11-color palette | fortunetell-frontend tokens.css HC mode |
| `#94a3b8` | HC border-subtle | Not in 11-color palette | fortunetell-frontend tokens.css HC mode |
| **OPACITY-BASED COLORS** |
| `rgba(0, 0, 0, 0.5)` | overlay-backdrop, modal overlays | Not in 11-color palette (opacity) | fortunetell-frontend globals.css |
| **GRADIENTS** |
| `bg-gradient-to-r from-emerald-500 to-blue-600` | Logo gradient | Not in 11-color palette (gradient) | simple-budget-site HTML |

**Total Out-of-Policy Colors:** 25+ distinct hex/rgba values not in 11-color palette

**Coverage:** fortunetell-frontend tokens.css, globals.css; simple-budget-site HTML (Tailwind), global.css

---

### B.2 OUT-OF-POLICY FONT WEIGHTS (Not in 2-Weight Spec)

**1.4 Spec Defines:** 2 weights (400 normal, 700 bold)

**Code Uses:** 4 weights (400, 500, 600, 700)

| Weight Value | Role in Code | Conflict Type | Where It Appears |
|--------------|--------------|---------------|------------------|
| 500 (medium) | Links, secondary buttons, slight emphasis | Not in 1.4 (only 400, 700 allowed) | fortunetell-frontend (LoginForm, SettingsView, TxList, DockBar - 12+ uses), simple-budget-site (global.css, Tailwind font-medium) |
| 600 (semibold) | Section headers, form labels, calendar day numbers | Not in 1.4 (only 400, 700 allowed) | fortunetell-frontend (SettingsView, CalendarMonth, TxForm - 10+ uses), simple-budget-site (global.css, Tailwind font-semibold) |

**Coverage:** fortunetell-frontend component modules; simple-budget-site global.css, HTML

---

### B.3 OUT-OF-POLICY SPACING VALUES (Not in 10-Step Scale)

**1.4 Spec Defines:** 10-step scale (0, 4, 8, 12, 16, 24, 32, 40, 48, possibly more)

**Code Uses Additional Values:**

| Spacing Value | Role in Code | Conflict Type | Where It Appears |
|---------------|--------------|---------------|------------------|
| 20px (1.25rem) | Tailwind space-5, py-5, h3 font-size equivalent spacing | Not in 1.4 scale (scale is 16→24, no 20) | simple-budget-site (Tailwind utilities, global.css) |
| 28px (1.75rem) | Tailwind gap-7 | Not in 1.4 scale (scale is 24→32, no 28) | simple-budget-site (Tailwind utilities) |

**Coverage:** simple-budget-site HTML (Tailwind utilities), global.css

---

### B.4 OUT-OF-POLICY SHADOWS (Shadowless Design Violation)

**1.4 Spec:** Shadowless design - **ALL shadows forbidden**

**Code Uses Shadows:**

| Shadow Definition | Role in Code | Conflict Type | Where It Appears |
|-------------------|--------------|---------------|------------------|
| `0 10px 15px -3px rgba(0,0,0,0.10), 0 4px 6px -4px rgba(0,0,0,0.10)` | --shadow-soft-base, soft shadows | FORBIDDEN (shadowless design) | fortunetell-frontend tokens.css, simple-budget-site global.css |
| `0 10px 15px -3px rgba(15,23,42,0.14), 0 4px 6px -4px rgba(15,23,42,0.12)` | --shadow-card-base, card shadows | FORBIDDEN (shadowless design) | fortunetell-frontend tokens.css, simple-budget-site global.css |
| `0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06)` | --shadow-banner-base, banner shadows | FORBIDDEN (shadowless design) | fortunetell-frontend tokens.css, simple-budget-site HTML (shadow-banner) |
| `0 10px 20px rgba(16,185,129,0.25)` | --button-primary-shadow, button glow | FORBIDDEN (shadowless design) | fortunetell-frontend tokens.css |
| `0 0 0 3px rgba(5,150,105,0.2)` | --shadow-focus-ring, focus indication | FORBIDDEN (shadowless design) | fortunetell-frontend tokens.css, simple-budget-site global.css |

**Total Shadow Definitions:** 5 distinct shadow tokens + Tailwind shadow utilities (shadow-md, shadow-lg)

**Coverage:** fortunetell-frontend tokens.css, globals.css; simple-budget-site global.css, HTML (Tailwind)

---

### B.5 OUT-OF-POLICY FONT SIZES (Not Observed, But Possible)

**1.4 Spec Defines:** 8 sizes (xs through 4xl: 12, 14, 16, 18, 20, 24, 30, 36)

**Code Observations:**

| Font Size Value | Role in Code | Status |
|-----------------|--------------|--------|
| 11.2px (0.7rem) | Logo tagline | OUT-OF-POLICY (below xs minimum 12px) | simple-budget-site global.css |
| 11px | Arbitrary value | OUT-OF-POLICY (below xs minimum 12px) | simple-budget-site HTML (text-[11px]) |
| 48px, 60px | Large hero sizes | POTENTIALLY OUT-OF-POLICY if not in 1.4 extended scale | simple-budget-site global.css (h1 responsive sizes) |

**Note:** Observed size usage mostly aligns with 1.4; only a few arbitrary/sub-minimum values found

**Coverage:** simple-budget-site global.css, HTML

---

### B.6 OUT-OF-POLICY RADII (Duplicates or Non-Spec Values)

**Observed Issues:**

| Radius Value | Issue | Where It Appears |
|--------------|-------|------------------|
| 12px (0.75rem) | DUPLICATE: --radius-lg and --radius-sharp both define 12px | fortunetell-frontend tokens.css |
| 16px (1rem) | --radius-card rarely used, unclear if in 1.4 spec | fortunetell-frontend tokens.css, simple-budget-site (rounded-2xl) |

**Coverage:** fortunetell-frontend tokens.css; simple-budget-site HTML (Tailwind)

---

### B.7 OUT-OF-POLICY LAYOUT/MOTION/ASSET VALUES (Non-Tokenized Patterns)

**Layout Patterns (Not Tokenized per 1.4):**

| Pattern | Values Used | Where It Appears | Notes |
|---------|-------------|------------------|-------|
| Content max-width | 48rem, 64rem, 72rem, 80rem | simple-budget-site (Tailwind .max-w-*) | NOT tokenized; inline Tailwind utilities |
| Page padding | 16px (1rem), 24px (1.5rem) | Both repos (Tailwind px-4, px-6; component padding) | NOT tokenized; inline values |
| Section spacing | ~64px (4rem) | simple-budget-site (.section class) | NOT tokenized; global.css |
| Responsive breakpoints | sm:, md:, lg: (Tailwind defaults) | simple-budget-site HTML | NOT tokenized; Tailwind defaults |

**Motion Patterns (Not Tokenized per 1.4):**

| Pattern | Values Used | Where It Appears | Notes |
|---------|-------------|------------------|-------|
| Transition durations | 150ms, 200ms | fortunetell-frontend globals.css, simple-budget-site HTML (Tailwind duration-200) | NOT tokenized; inline values |
| Transition easings | ease-out, ease-in-out | fortunetell-frontend globals.css, simple-budget-site animation | NOT tokenized; inline values |
| Animation | 4s ease-in-out infinite (floating) | simple-budget-site HTML inline style | NOT tokenized; one-off animation |

**Asset Patterns (Partially Tokenized):**

| Pattern | Values Used | Where It Appears | Notes |
|---------|-------------|------------------|-------|
| Icon sizes | 16px, 20px, 30px | fortunetell-frontend (TOKENIZED as --icon-size-sm/md/lg) | COMPLIANT with 1.4 |
| Logo sizes | 40px, 48px (2.5rem, 3rem) | simple-budget-site (Tailwind .w-10, .w-12) | NOT tokenized; Tailwind utilities |
| SVG viewBox | 24×24 | fortunetell-frontend icons directory | Hardcoded in SVG components |

**Coverage:** Both repos

---

## PART C: COVERAGE NOTES AND LIMITATIONS

### C.1 Files Fully Analyzed

**fortunetell-frontend:**
- `/src/styles/tokens.css` - Full read
- `/src/styles/globals.css` - Full read
- `/src/components/*.module.css` - Sampled (CalendarMonth, TxList, TxForm, SettingsView, LoginForm, AppHeader, AccountView, AddTransactionPanel, HistoryPanel, DockBar, Divider)
- `/src/app/page.module.css` - Full read
- `/src/app/_shell/ShellLayout.module.css` - Partial read

**simple-budget-site:**
- `/styles/global.css` - Full read
- `/build/tailwind.config.js` - Full read
- `/index.html` - Full read
- Other HTML files - Partial read (sampled guide pages)

### C.2 Potential Coverage Gaps

**fortunetell-frontend:**
- Not all component `.module.css` files were individually read (used grep with head_limit for patterns)
- Possible additional component-specific styling in unsampled modules
- Dynamic styles in `.tsx` files (e.g., inline styles, styled-components) not analyzed

**simple-budget-site:**
- Not all HTML guide pages individually read (sampled representative pages)
- Tailwind config extensions not fully analyzed (only base config read)
- Generated `/assets/tailwind.css` file not read (very large, Tailwind defaults)

**Both Repos:**
- Motion/animation usage may be incomplete (focused on static CSS, not JS-driven animations)
- Responsive breakpoint patterns not exhaustively cataloged
- Focus states and pseudo-class usage not fully inventoried

### C.3 Grep Analysis Methodology

**Approach Used:**
- Grep with `head_limit` to manage large outputs
- Focused searches by file type (e.g., `**/*.module.css`, `**/*.html`)
- Representative sampling of component files
- Full reads of token/global CSS files

**Limitations:**
- Large grep results (e.g., node_modules) were excluded or limited
- Some component modules may not have been individually inspected
- Arbitrary values (e.g., `text-[11px]`) may not be fully cataloged

---

## PART D: FORMS AND INPUTS (Placeholder)

**Note:** As identified in corrections, forms and inputs require dedicated, thorough analysis. This section is a placeholder pending separate forms/inputs analysis.

**Observed So Far:**
- fortunetell-frontend has input tokens (--input-border, --input-radius, --input-focus-border, --input-bg)
- simple-budget-site uses custom .input class in global.css (not token-based)
- Both repos have form elements (inputs, buttons, selects) but comprehensive form control analysis not yet performed

**Required:** Dedicated forms/inputs analysis covering:
- Input types (text, email, number, date, etc.)
- Select/dropdown styling
- Checkbox/radio styling
- Form validation states
- Label/hint text patterns
- Form layout patterns
- Accessibility attributes

---

## SUMMARY

### What This Inventory Shows

**Alignment with 1.4 Spec:**
- **Colors:** 6 of 11 base colors are used in code; 3 theme colors (secondary/tertiary/accent) currently unused; 25+ out-of-policy colors in code
- **Typography (sizes):** All 8 font sizes are used in code; a few out-of-policy sizes (11px, 60px)
- **Typography (weights):** 2 of 2 spec weights are used; 2 additional out-of-policy weights (500, 600) heavily used
- **Spacing:** 8-9 of 10 spec steps are used; 2 out-of-policy values (20px, 28px) from Tailwind
- **Radii:** 4-5 radius values used; 1 duplicate (--radius-sharp = --radius-lg)
- **Borders:** Standard 0/1px widths used; colors are token aliases
- **Icons:** All 3 size tokens used; implementation uses inline SVG with currentColor
- **Buttons/Inputs:** Role tokens used in fortunetell-frontend; simple-budget-site uses global classes
- **Shadows:** 5+ shadow definitions in code; ALL violate 1.4 shadowless design
- **Layout/Motion/Asset:** Patterns exist but mostly not tokenized per 1.4 spec

### What This Inventory Does NOT Do

- Does NOT recommend which tokens to keep/remove/merge
- Does NOT propose changes to the 11-color palette
- Does NOT suggest new token values or expansions
- Does NOT design the 2.0 spec
- Does NOT treat current CSS as design authority

### Current CSS is Diagnostic Input

- Observations about usage patterns inform breakage risk, not token retention
- Out-of-policy values indicate refactoring work required, not spec changes needed
- Unused spec tokens (secondary/tertiary/accent) are allowed; may be used in future
- Spec constraints (1.4 + V10.2.1) are authoritative, not negotiable based on current code

---

**END OF CORRECTED INVENTORY**

**Status:** Purely descriptive analysis - no recommendations
**Next Steps:** Forms/inputs dedicated analysis (separate); Task 2 spec work (uses this inventory as input)
