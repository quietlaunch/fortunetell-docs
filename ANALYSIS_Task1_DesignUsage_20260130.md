# Task 1: Design Usage Analysis Report
## Analysis of simple-budget-site and fortunetell-frontend against 2.0 Token System Constraints

**Date:** 2026-01-30
**Scope:** Design element inventory across simple-budget-site and fortunetell-frontend
**Purpose:** Identify real usage patterns to inform 2.0 token system reduction (Task 2)
**Constraint Reference:** DEV_DesignTokensSystem_1.4 + V10.2.1 Architecture

---

## Executive Summary

This analysis inventories all design elements across **simple-budget-site** (Tailwind-based static site) and **fortunetell-frontend** (Next.js app with 1.3 token system) against the proposed 2.0 token system constraints.

**Key Findings:**
- **simple-budget-site** uses Tailwind CSS extensively with heavy reliance on Tailwind's color/spacing/shadow utilities (requires full removal per Task 5)
- **fortunetell-frontend** has an existing 1.3-style token system with 20+ hex colors that need classification against the 11-color palette
- **Both repositories have shadow violations** (2.0 is shadowless)
- **Typography usage is consistent** across both repos and aligns well with 2.0 scale
- **Spacing patterns** show consistent use of a 4px-increment scale

---

## 1. COLORS

### 1.1 Color Discovery

#### fortunetell-frontend
**Location:** `/src/styles/tokens.css`

**Brand Colors (3):**
- `#10b981` - brand-green (canonical green)
- `#f6e823` - brand-yellow (canonical yellow)
- `#ff1a1a` - brand-red (canonical red)

**Brand Variations (2):**
- `#047857` - brand-green-dark
- `#6ee7b7` - brand-green-light

**Neutral Palette - Light Mode (8):**
- `#ffffff` - neutral-bg-0
- `#f9fafb` - neutral-bg-1
- `#f3f4f6` - neutral-bg-2
- `#e2e8f0` - neutral-border-subtle
- `#cbd5f5` - neutral-border-strong
- `#0f172a` - neutral-text-strong
- `#334155` - neutral-text-body
- `#475569` - neutral-text-muted
- `#64748b` - neutral-text-low

**Neutral Palette - Dark Mode (2):**
- `#0b1120` - neutral-bg-dark-0
- `#020617` - neutral-bg-dark-1

**High-Contrast Mode Override (5+):**
- `#000000` - surface-page (HC mode)
- `#050816` - surface-subtle (HC mode)
- `#ffffff` - text-strong (HC mode)
- `#e5e7eb` - text-body (HC mode)
- `#94a3b8` - border-subtle (HC mode)

**Other:**
- `rgba(0, 0, 0, 0.5)` - overlay-backdrop (globals.css)

**Total Distinct Hex Colors:** 20+

#### simple-budget-site
**Locations:** Tailwind utility classes in HTML files, `styles/global.css`

**Tailwind Color Utilities Used (from HTML analysis):**
- `bg-white` - #ffffff
- `bg-slate-50` - #f8fafc
- `bg-slate-100` - #f1f5f9
- `bg-slate-200` - #e2e8f0
- `bg-emerald-50` - #ecfdf5
- `bg-emerald-100` - #d1fae5
- `bg-emerald-600` - #10b981
- `bg-gradient-to-r from-emerald-500 to-blue-600` - gradient
- `text-slate-500` - #64748b
- `text-slate-600` - #475569
- `text-slate-700` - #334155
- `text-slate-800` - #1e293b
- `text-slate-900` - #0f172a
- `text-emerald-600` - #10b981
- `text-emerald-700` - #047857
- `text-emerald-800` - #065f46
- `text-rose-600` - #e11d48
- `text-amber-600` - #d97706
- `border-slate-100` - #f1f5f9
- `border-slate-200` - #e2e8f0
- `border-emerald-100` - #d1fae5
- `border-emerald-200` - #a7f3d0

**Total Distinct Tailwind Colors:** 20+ (from Tailwind's default palette)

---

### 1.2 Color Classification

#### fortunetell-frontend

**COMPLIANT (matches 2.0 exactly):**
- `#000000` (primary) - HC mode surface-page ✓
- `#ffffff` (background) - neutral-bg-0 ✓

**SNAP CANDIDATE (close to 2.0 palette, can be snapped):**
- `#10b981` → snap to semantic safe color (already canonical green)
- `#f6e823` → snap to semantic warning color (already canonical yellow)
- `#ff1a1a` → snap to semantic danger color (already canonical red)
- `#0f172a` → snap to `#000000` (primary) - very close to black
- `#f9fafb` → snap to `#ffffff` (background) - very light gray, can use background
- `#f3f4f6` → snap to `#ffffff` (background) or neutral accent if needed

**OUT-OF-POLICY (violations requiring decisions):**
- `#047857` (brand-green-dark) - needs decision: snap to primary (#000000) or remove
- `#6ee7b7` (brand-green-light) - needs decision: snap to safe hover or remove
- `#334155`, `#475569`, `#64748b` (text body/muted/low) - needs gray scale strategy
- `#e2e8f0`, `#cbd5f5` (borders) - needs border color strategy
- `#0b1120`, `#020617` (dark backgrounds) - landing-specific, may need retention or snap to primary
- `#050816`, `#e5e7eb`, `#94a3b8` (HC overrides) - needs HC strategy review
- `rgba(0, 0, 0, 0.5)` (overlay backdrop) - needs opacity strategy or removal

**Total Colors:** 20+ distinct hex values → 2.0 target is 11 colors

#### simple-budget-site

**COMPLIANT:**
- None directly (all are Tailwind utilities, not token-based)

**SNAP CANDIDATE:**
- `bg-white` / `text-slate-900` - can snap to 2.0 background/primary
- `bg-emerald-600` / `text-emerald-600` - can snap to safe semantic
- `text-rose-600` - can snap to danger semantic
- `text-amber-600` - can snap to warning semantic

**OUT-OF-POLICY:**
- All Tailwind color utilities are violations (2.0 requires token-only)
- Entire Tailwind system must be removed (Task 5)
- All slate-50/100/200/etc shades need remapping to 2.0 palette
- Gradients (`bg-gradient-to-r from-emerald-500 to-blue-600`) - OUT-OF-POLICY (2.0 prohibits gradients)

**Total Tailwind Colors Used:** 20+ → full Tailwind removal required

---

### 1.3 Color Usage Patterns

#### fortunetell-frontend
**Pattern:** Token-based, three-layer system (tokens.css → globals.css → component modules)
- Component modules reference tokens via `var(--app-border)`, `var(--text-body)`
- Semantic mapping is present but incomplete (e.g., button colors map to brand colors)
- HC mode uses structural token remapping (good pattern to retain)

**Files Using Colors:**
- `tokens.css` - all color definitions (20+ hex colors)
- `globals.css` - references tokens, defines overlay backdrop rgba
- Component modules - all reference tokens via CSS variables (GOOD - compliant pattern)

#### simple-budget-site
**Pattern:** Tailwind utility-first, no token abstraction
- HTML contains direct Tailwind classes (`bg-white`, `text-slate-900`)
- `global.css` contains some raw font-size/font-weight but minimal color definitions
- No component modules (static HTML site)

**Files Using Colors:**
- All `.html` files - heavy Tailwind utility usage
- `styles/global.css` - minimal, mostly typography overrides
- No token system present

---

## 2. TYPOGRAPHY

### 2.1 Font Size Discovery

#### fortunetell-frontend
**Token-Defined Sizes (tokens.css):**
- `--font-size-xs` - 12px
- `--font-size-sm` - 14px
- `--font-size-md` - 16px
- `--font-size-lg` - 18px
- `--font-size-xl` - 20px
- `--font-size-2xl` - 24px
- `--font-size-3xl` - 30px
- `--font-size-4xl` - 36px

**Usage Pattern:** Component modules consistently use token references:
- `font-size: var(--font-size-3xl)` (SettingsView, page.module.css)
- `font-size: var(--font-size-2xl)` (LoginForm, AccountView)
- `font-size: var(--font-size-xl)` (AddTransactionPanel, page.module.css)
- `font-size: var(--font-size-lg)` (HistoryPanel, TxForm)
- `font-size: var(--font-size-md)` (SettingsView, TxForm, DockBar)
- `font-size: var(--font-size-sm)` (CalendarMonth, HistoryPanel)
- `font-size: var(--font-size-xs)` (CalendarMonth, TxList, HistoryPanel, DockBar)

**Total Distinct Sizes:** 8

#### simple-budget-site
**Raw Values in global.css:**
- `3.75rem` (60px) - h1 large breakpoint
- `3rem` (48px) - h1 medium breakpoint
- `2.25rem` (36px) - h1 base, h2 medium breakpoint
- `1.875rem` (30px) - h2 base
- `1.25rem` (20px) - h3, p responsive
- `1.125rem` (18px) - h4, p responsive
- `1rem` (16px) - base, various elements
- `0.7rem` (11.2px) - logo tagline

**Tailwind Utility Classes (from HTML):**
- `text-4xl` (2.25rem / 36px)
- `text-5xl` (3rem / 48px)
- `text-6xl` (3.75rem / 60px)
- `text-3xl` (1.875rem / 30px)
- `text-2xl` (1.5rem / 24px)
- `text-xl` (1.25rem / 20px)
- `text-lg` (1.125rem / 18px)
- `text-sm` (0.875rem / 14px)
- `text-xs` (0.75rem / 12px)
- `text-[11px]` (arbitrary value)

**Total Distinct Sizes:** 10+

---

### 2.2 Font Weight Discovery

#### fortunetell-frontend
**Token-Defined Weights (tokens.css):**
- `--font-weight-normal` - 400
- `--font-weight-medium` - 500
- `--font-weight-semibold` - 600
- `--font-weight-bold` - 700

**Usage Pattern:** Component modules use token references:
- `font-weight: var(--font-weight-normal)` (page.module.css, AppHeader, AccountView)
- `font-weight: var(--font-weight-medium)` (SettingsView, LoginForm, TxList, DockBar)
- `font-weight: var(--font-weight-semibold)` (SettingsView, CalendarMonth, TxForm)
- `font-weight: var(--font-weight-bold)` (LoginForm, CalendarMonth, TxList)

**Total Distinct Weights:** 4

#### simple-budget-site
**Raw Values in global.css:**
- `700` - h1, h2, h3
- `600` - h4, various buttons
- `500` - links, buttons
- `400` - body text, p

**Tailwind Utility Classes (from HTML):**
- `font-bold` (700)
- `font-semibold` (600)
- `font-medium` (500)
- `font-normal` (400)

**Total Distinct Weights:** 4

---

### 2.3 Typography Classification

#### fortunetell-frontend

**COMPLIANT:**
- Existing 1.3 token system already defines 8 size levels (xs through 4xl) - aligns with 2.0 requirement
- 4 weight levels (400, 500, 600, 700) - aligns with 2.0 (though 2.0 specifies 400, 700 only)

**SNAP CANDIDATE:**
- `--font-weight-medium (500)` → snap to 400 (normal)
- `--font-weight-semibold (600)` → snap to 700 (bold)
- Reduce 4 weights → 2 weights (400, 700) per 2.0 spec

**OUT-OF-POLICY:**
- None (typography is well-structured and token-based)

#### simple-budget-site

**COMPLIANT:**
- None (all Tailwind utilities, not token-based)

**SNAP CANDIDATE:**
- Tailwind size utilities map reasonably to 2.0 scale (text-xs through text-6xl)
- Tailwind weight utilities (400, 500, 600, 700) → snap to (400, 700)

**OUT-OF-POLICY:**
- All Tailwind utilities are violations (must be replaced with tokens)
- Raw rem values in global.css (must be replaced with token references)
- Arbitrary value `text-[11px]` - should use predefined token

---

## 3. SPACING

### 3.1 Spacing Discovery

#### fortunetell-frontend
**Token-Defined Spacing (tokens.css):**
- `--space-0` - 0
- `--space-2xs` - 0.25rem (4px)
- `--space-xs` - 0.5rem (8px)
- `--space-sm` - 0.75rem (12px)
- `--space-md` - 1rem (16px)
- `--space-lg` - 1.5rem (24px)
- `--space-xl` - 2rem (32px)
- `--space-2xl` - 2.5rem (40px)
- `--space-12` - 3rem (48px)

**Usage Pattern:** Component modules extensively use spacing tokens:
- `padding: var(--space-md)` (TxForm, CalendarMonth, AddTransactionPanel)
- `padding: var(--space-lg)` (LoginForm, AddTransactionPanel)
- `padding: var(--space-xl)` (AddTransactionPanel)
- `gap: var(--space-md)` (SettingsView, TxForm, CalendarMonth)
- `gap: var(--space-xl)` (SettingsView)
- `gap: var(--space-sm)` (AddTransactionPanel, CalendarMonth)
- `gap: var(--space-xs)` (LoginForm, CalendarMonth, TxForm)
- `gap: var(--space-2xs)` (SettingsView, CalendarMonth, DockBar)
- `margin-top: var(--space-md)` (TxForm)
- `margin-bottom: var(--space-xl)` (SettingsView)

**Total Distinct Spacing Values:** 10

#### simple-budget-site
**Tailwind Utility Classes (from HTML):**
- `space-x-3` (0.75rem / 12px)
- `space-x-4` (1rem / 16px)
- `space-x-5` (1.25rem / 20px)
- `space-y-1` (0.25rem / 4px)
- `space-y-3` (0.75rem / 12px)
- `space-y-4` (1rem / 16px)
- `space-y-5` (1.25rem / 20px)
- `space-y-6` (1.5rem / 24px)
- `space-y-8` (2rem / 32px)
- `gap-2` (0.5rem / 8px)
- `gap-3` (0.75rem / 12px)
- `gap-4` (1rem / 16px)
- `gap-6` (1.5rem / 24px)
- `gap-7` (1.75rem / 28px)
- `px-3` (0.75rem / 12px)
- `px-4` (1rem / 16px)
- `px-6` (1.5rem / 24px)
- `py-2` (0.5rem / 8px)
- `py-3` (0.75rem / 12px)
- `py-4` (1rem / 16px)
- `py-5` (1.25rem / 20px)
- `py-8` (2rem / 32px)
- `p-4` (1rem / 16px)
- `p-5` (1.25rem / 20px)
- `p-8` (2rem / 32px)
- `mb-3`, `mb-4`, `mb-6`, `mt-3`, `mt-4`, `mt-8`, `mt-10`

**Distinct Values:** 15+ (0.25rem through 2.5rem in 0.25rem increments)

---

### 3.2 Spacing Pattern Analysis

#### fortunetell-frontend

**Pattern:** 4px-based scale (0, 4, 8, 12, 16, 24, 32, 40, 48)
- Consistent with 2.0 spacing scale requirement
- Token-based usage throughout component modules (GOOD)
- Uses logical properties (padding-block, padding-inline) where appropriate

**Special Spacing:**
- `env(safe-area-inset-bottom)` - DockBar (mobile safe area, should retain)
- Calculated values: `calc(var(--space-xl) * 4)` (SettingsView), `calc(var(--space-xs) + var(--space-2xs))` (CalendarMonth)

#### simple-budget-site

**Pattern:** Tailwind's default spacing scale (0.25rem increments)
- More granular than 2.0 (has 0.25rem, 0.5rem, 0.75rem, 1rem, 1.25rem, 1.5rem, 1.75rem, 2rem...)
- Includes values NOT in 2.0 scale: 1.25rem (20px), 1.75rem (28px)
- All spacing via Tailwind utilities (must be replaced with tokens)

---

### 3.3 Spacing Classification

#### fortunetell-frontend

**COMPLIANT:**
- Existing spacing tokens align with 2.0 scale (4px increments)
- Token-based usage throughout (GOOD pattern)

**SNAP CANDIDATE:**
- None (spacing already aligns with 2.0)

**OUT-OF-POLICY:**
- None (well-structured spacing system)

#### simple-budget-site

**COMPLIANT:**
- None (all Tailwind utilities)

**SNAP CANDIDATE:**
- Most Tailwind spacing utilities map to 2.0 scale (space-3 = 12px, space-4 = 16px, etc.)
- Non-standard values (1.25rem, 1.75rem) → snap to nearest 2.0 value

**OUT-OF-POLICY:**
- All Tailwind spacing utilities (must be replaced with tokens)
- Some values don't exist in 2.0 scale (1.25rem/20px, 1.75rem/28px)

---

## 4. BORDERS & RADII

### 4.1 Border Discovery

#### fortunetell-frontend
**Border Width Patterns:**
- `border: 1px solid` - most common (cards, inputs, forms)
- `border: none` - explicit removals (SettingsView, AccountView)
- `border-width: 1px` - globals.css button styling

**Border Color Patterns:**
- `border-color: var(--app-border)` (cards, inputs)
- `border-color: var(--border-subtle)` (various)
- `border-color: var(--button-secondary-border)` (buttons)
- `border-color: var(--button-danger-border)` (danger buttons)
- `border-color: var(--bg-danger-hover)` (LoginForm error state)

**Total Border Widths:** 2 (0, 1px)
**Total Border Colors:** Token-based (5+ distinct token references)

#### simple-budget-site
**Border Patterns (from HTML):**
- `border-b` (Tailwind: border-bottom)
- `border-t` (Tailwind: border-top)
- `border` (Tailwind: 1px solid)
- `border-slate-100`, `border-slate-200`, `border-emerald-100`, `border-emerald-200`

**Total Border Widths:** 1px (default Tailwind)
**Total Border Colors:** 4+ (via Tailwind color utilities)

---

### 4.2 Border Radius Discovery

#### fortunetell-frontend
**Token-Defined Radii (tokens.css):**
- `--radius-sm` - 0.25rem (4px)
- `--radius-md` - 0.5rem (8px)
- `--radius-lg` - 0.75rem (12px)
- `--radius-pill` - 9999px
- `--radius-card` - 1rem (16px)
- `--radius-sharp` - 0.75rem (12px)

**Usage Pattern:**
- `border-radius: var(--radius-md)` (CalendarMonth, AddTransactionPanel, TxList, TxForm, page.module.css)
- `border-radius: var(--radius-sm)` (SettingsView, CalendarMonth, LoginForm, page.module.css, globals.css)
- `border-radius: var(--radius-lg)` (BetaNoticeModal)
- `border-radius: 0` (SettingsView, AccountView - explicit zero for certain layouts)
- `border-radius: 50%` (CalendarMonth - circular elements)
- `border-radius: var(--input-radius)`, `var(--button-radius)` (globals.css)

**Total Distinct Radii:** 6 (including 0 and 50%)

#### simple-budget-site
**Tailwind Utility Classes (from HTML):**
- `rounded-xl` (0.75rem / 12px)
- `rounded-2xl` (1rem / 16px)
- `rounded-card` (custom, likely 1rem)
- `rounded-sharp` (custom, likely 0.75rem)
- `rounded-lg` (0.5rem / 8px)
- `rounded-full` (9999px)

**Total Distinct Radii:** 5+

---

### 4.3 Border & Radius Classification

#### fortunetell-frontend

**COMPLIANT:**
- Border widths are simple (0, 1px) - aligns with minimal design
- Radii are token-based and follow consistent scale (4px, 8px, 12px, 16px)

**SNAP CANDIDATE:**
- `--radius-card (1rem)` and `--radius-sharp (0.75rem)` → consolidate if they're duplicates
- Consider whether 6 radius tokens are necessary (2.0 spec doesn't specify, but simplification may be possible)

**OUT-OF-POLICY:**
- None (borders and radii are well-structured)

#### simple-budget-site

**COMPLIANT:**
- None (all Tailwind utilities)

**SNAP CANDIDATE:**
- Tailwind radius utilities map to similar values as fortunetell-frontend
- `rounded-xl` → snap to --radius-lg
- `rounded-2xl` → snap to --radius-card

**OUT-OF-POLICY:**
- All Tailwind border/radius utilities (must be replaced with tokens)

---

## 5. SHADOWS

### 5.1 Shadow Discovery

#### fortunetell-frontend
**Token-Defined Shadows (tokens.css):**
```css
--shadow-soft-base: 0 10px 15px -3px rgba(0, 0, 0, 0.10), 0 4px 6px -4px rgba(0, 0, 0, 0.10);
--shadow-card-base: 0 10px 15px -3px rgba(15, 23, 42, 0.14), 0 4px 6px -4px rgba(15, 23, 42, 0.12);
--shadow-banner-base: 0 1px 3px rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06);
--button-primary-shadow: 0 10px 20px rgba(16, 185, 129, 0.25);
--shadow-focus-ring: 0 0 0 3px rgba(5, 150, 105, 0.2);
```

**Usage Locations:**
- tokens.css - 5 shadow definitions
- globals.css - references to shadow tokens
- Component usage via button classes and card styling

**Total Shadow Definitions:** 5

#### simple-budget-site
**Shadow Definitions (global.css):**
```css
box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.1); /* shadow-soft */
box-shadow: 0 10px 15px -3px rgba(15, 23, 42, 0.14), 0 4px 6px -4px rgba(15, 23, 42, 0.12); /* shadow-card */
box-shadow: 0 0 0 3px rgba(5, 150, 105, 0.2); /* focus ring */
```

**Tailwind Utility Classes (from HTML):**
- `shadow-soft` (custom utility)
- `shadow-card` (custom utility)
- `shadow-banner` (custom utility)
- `shadow-md` (Tailwind default)
- `shadow-lg` (Tailwind default)

**Total Shadow Definitions:** 5+

---

### 5.2 Shadow Classification

#### fortunetell-frontend

**COMPLIANT:**
- **NONE** - all shadows are violations per 2.0 shadowless design

**OUT-OF-POLICY (ALL SHADOWS - must be removed):**
- `--shadow-soft-base` - VIOLATION (shadowless design)
- `--shadow-card-base` - VIOLATION (shadowless design)
- `--shadow-banner-base` - VIOLATION (shadowless design)
- `--button-primary-shadow` - VIOLATION (shadowless design)
- `--shadow-focus-ring` - VIOLATION (must replace with border-based focus indication)

**Action Required:** Remove all shadow tokens and usages, replace with border-based visual separation

#### simple-budget-site

**COMPLIANT:**
- **NONE** - all shadows are violations per 2.0 shadowless design

**OUT-OF-POLICY (ALL SHADOWS - must be removed):**
- All shadow utilities (shadow-soft, shadow-card, shadow-banner, shadow-md, shadow-lg) - VIOLATIONS
- All rgba shadow definitions in global.css - VIOLATIONS

**Action Required:** Remove all shadows, replace with border-based visual separation (Task 5)

---

## 6. LAYOUT, COMPOSITION, MOTION, & ASSET CONSTANTS

### 6.1 Layout Constants

#### fortunetell-frontend
**Custom Layout Variables:**
- `--app-header-height: calc(2 * var(--space-md) + var(--font-size-xl))` - ShellLayout.module.css
- Container utilities: `.appMainContainer`, `.appView` - globals.css
- Dock padding: `calc(var(--space-sm) + env(safe-area-inset-bottom))` - DockBar.module.css

**Flexbox/Grid Usage:**
- Heavy flexbox usage (flex, flex-col, gap) in component modules
- Grid usage: CalendarMonth (grid layout for calendar days)
- Logical properties: `padding-block`, `padding-inline`, `margin-inline`

**Breakpoint Usage:**
- No explicit breakpoint tokens found (Next.js uses CSS media queries)
- Responsive patterns in page.module.css and component modules

#### simple-budget-site
**Custom Layout Classes (global.css):**
- `.container-xl` - max-width container
- `.section` - vertical section padding
- `.grid-responsive` - responsive grid layout
- `.banner` - hero/banner layout

**Tailwind Layout Utilities (from HTML):**
- `flex`, `flex-col`, `flex-row`, `items-center`, `items-start`, `justify-between`, `justify-center`
- `grid`, `grid-cols-2`, `grid-cols-3`, `md:grid-cols-2`, `md:grid-cols-3`
- `max-w-xs`, `max-w-md`, `max-w-xl`, `max-w-2xl`, `max-w-3xl`, `max-w-5xl`, `max-w-6xl`
- `mx-auto` - center alignment
- Responsive prefixes: `sm:`, `md:`, `lg:`

---

### 6.2 Motion Constants

#### fortunetell-frontend
**Transition Definitions (globals.css):**
```css
transition: background-color 150ms ease-out, border-color 150ms ease-out, color 150ms ease-out, box-shadow 150ms ease-out;
```

**No Motion Tokens Defined** - transitions are inline values (150ms, ease-out)

#### simple-budget-site
**Animation Definitions (index.html inline):**
```css
.floating {
  animation: floating 4s ease-in-out infinite;
}
@keyframes floating {
  0%   { transform: translateY(0); }
  50%  { transform: translateY(-6px); }
  100% { transform: translateY(0); }
}
```

**Tailwind Transitions:**
- `transition-all duration-200` (buttons, inputs)
- Backdrop blur: `backdrop-blur-xl`

**No Motion Tokens Defined** - all motion is inline or Tailwind utility

---

### 6.3 Asset Constants (Icons)

#### fortunetell-frontend
**Icon Tokens (tokens.css):**
- `--icon-size-sm: var(--font-size-md)` (16px)
- `--icon-size-md: var(--font-size-xl)` (20px)
- `--icon-size-lg: var(--font-size-2xl)` (30px - canonical dock icon size)
- `--icon-color-default`, `--icon-color-strong`, `--icon-color-inverse`

**Icon Implementation:**
- Inline SVG components in `src/icons/` (Material Design 24×24 viewBox)
- Global utilities: `.icon-sm`, `.icon-md`, `.icon-lg` (globals.css)
- Color driven by `fill="currentColor"` (CSS control)

#### simple-budget-site
**Icon Implementation:**
- Emoji-based icons (📶, 📡, 🔋) - index.html mockup
- Single-letter logo placeholders (F, D, W) - HTML
- No icon token system (static site)

---

### 6.4 Classification

#### fortunetell-frontend

**COMPLIANT:**
- Icon tokens (size and color) - well-structured and aligned with 2.0 intent
- Layout uses logical properties (padding-block, margin-inline) - modern best practice
- Safe area inset handling (DockBar) - mobile-first, should retain

**SNAP CANDIDATE:**
- Motion timing (150ms) → could define `--transition-duration-fast: 150ms` token
- Motion easing (ease-out) → could define `--transition-easing: ease-out` token

**OUT-OF-POLICY:**
- No motion tokens defined (should be added per 2.0 spec if motion is used)
- Inline transition values (should be tokenized)

#### simple-budget-site

**COMPLIANT:**
- None (static site with Tailwind, no token system)

**OUT-OF-POLICY:**
- All layout utilities (Tailwind, must be replaced)
- Inline animation (should be removed or tokenized if retained)
- No icon system (not needed for static site, but future app integration will need tokens)

---

## 7. MODULE-LEVEL VS TOKEN-LEVEL STYLING

### 7.1 Current Patterns

#### fortunetell-frontend
**TOKEN-LEVEL STYLING (tokens.css):**
- All color values (#hex, rgba)
- All typography scales (font-size, font-weight, line-height)
- All spacing scales (padding, margin, gap)
- All radius values (border-radius)
- All shadow values (box-shadow) - **to be removed in 2.0**
- Icon sizes and colors
- Semantic aliases (button-primary-bg, input-border, etc.)

**GLOBALS-LEVEL STYLING (globals.css):**
- Base element resets (body, h1-h6, p, a, button, input)
- Global button classes (.btn, .btn-primary, .btn-secondary, .btn-danger)
- Global icon utilities (.icon-sm, .icon-md, .icon-lg)
- Global card styles
- Divider utilities
- Focus states

**MODULE-LEVEL STYLING (*.module.css):**
- Component layout (flexbox, grid, positioning)
- Component structure (dimensions, overflow, z-index)
- Component-specific spacing using tokens (padding: var(--space-md))
- Component-specific visual semantics using tokens (border-color: var(--app-border))

**PATTERN COMPLIANCE:** ✓ **GOOD**
- fortunetell-frontend follows V10.2.1 token-only, component-scoped architecture
- No raw style values in component modules (all reference tokens)
- Clear separation of concerns (tokens → globals → modules)

#### simple-budget-site
**TOKEN-LEVEL STYLING:**
- **NONE** - no token system exists

**GLOBALS-LEVEL STYLING (global.css):**
- Typography overrides (h1-h6 font-size, font-weight)
- Button classes (.btn-primary)
- Input classes (.input)
- Card classes (.card)
- Link classes (.guide-link)
- Container utilities (.container-xl, .section, .grid-responsive, .banner)
- Raw style values (font-size: 2.25rem, font-weight: 700)

**COMPONENT-LEVEL STYLING:**
- **NONE** - all styling is Tailwind utilities in HTML or global.css classes

**PATTERN COMPLIANCE:** ✗ **VIOLATION**
- simple-budget-site does NOT follow token-only architecture
- Heavy Tailwind usage (utility-first anti-pattern per V10.2.1)
- Raw style values in global.css (non-token color, spacing, typography)
- No component modules (static HTML, but future SPA migration will need modules)

---

### 7.2 Token Reference Patterns

#### fortunetell-frontend
**GOOD PATTERNS (to retain):**
```css
/* Component module using tokens */
.container {
  padding: var(--space-md);
  gap: var(--space-xl);
  border: 1px solid var(--app-border);
  border-radius: var(--radius-md);
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-semibold);
}
```

**Pattern Count:**
- **100+ token references** across component modules (all compliant)
- **0 raw hex colors** in component modules (GOOD)
- **0 raw font-size values** in component modules (GOOD)

#### simple-budget-site
**ANTI-PATTERNS (must be removed):**
```html
<!-- Tailwind utility anti-pattern -->
<div class="bg-white text-slate-900 px-4 py-2 rounded-xl shadow-soft">
```

```css
/* Raw value anti-pattern in global.css */
h1 {
  font-size: 2.25rem;
  font-weight: 700;
}
```

**Pattern Count:**
- **500+ Tailwind utility classes** across HTML files (all violations)
- **30+ raw style values** in global.css (all violations)
- **0 token references** (no token system)

---

### 7.3 Classification

#### fortunetell-frontend

**COMPLIANT:**
- Token-only architecture ✓
- Component-scoped styling ✓
- No raw style values in modules ✓
- Semantic token mapping (partial, needs completion)

**REFINEMENT NEEDED:**
- Reduce color tokens (20+ → 11 per 2.0 spec)
- Remove all shadow tokens (shadowless design)
- Consolidate font-weight tokens (4 → 2 per 2.0 spec)
- Add motion tokens if transitions are retained

**OUT-OF-POLICY:**
- Shadow tokens (must be removed)

#### simple-budget-site

**COMPLIANT:**
- **NONE**

**OUT-OF-POLICY:**
- Entire Tailwind system (must be removed)
- All raw style values in global.css (must be replaced with tokens)
- No component modules (must be added for future SPA)

---

## 8. SUMMARY & NEXT STEPS

### 8.1 High-Level Findings

| Aspect | fortunetell-frontend | simple-budget-site |
|--------|----------------------|-------------------|
| **Colors** | 20+ hex colors → needs reduction to 11 | Tailwind colors → must remove all |
| **Typography** | 8 sizes, 4 weights → reduce to 8 sizes, 2 weights | Tailwind utilities → must replace with tokens |
| **Spacing** | 10-step scale (4px increments) → compliant | Tailwind utilities → must replace with tokens |
| **Borders/Radii** | Token-based, 6 radii → compliant (may consolidate) | Tailwind utilities → must replace with tokens |
| **Shadows** | 5 shadow tokens → **REMOVE ALL** | Tailwind shadows → **REMOVE ALL** |
| **Layout/Motion** | Icon tokens ✓, no motion tokens | Tailwind utilities, inline motion → must replace |
| **Styling Pattern** | Token-only ✓, component-scoped ✓ | Tailwind utilities ✗ → must refactor |

---

### 8.2 Actionable Insights for Task 2 (2.0 Spec Reduction)

**Color Reduction Strategy:**
1. Retain semantic colors: safe (#10b981 green), warning (#f6e823 yellow), danger (#ff1a1a red) + hover variants (6 colors)
2. Retain structural colors: primary (#000000), secondary (#3f3f3f), tertiary (#7f7f7f), accent (#b7b7b7), background (#ffffff) (5 colors)
3. **Total: 11 colors** (matches 2.0 target)
4. All other neutrals (text-body, text-muted, borders) → remap to 5 structural colors
5. HC mode overrides → remap structural tokens only

**Typography Simplification:**
1. Retain 8 size levels (xs through 4xl) - already compliant
2. Reduce 4 weights → 2 weights (400 normal, 700 bold)
3. Remap medium (500) → normal (400), semibold (600) → bold (700)

**Shadow Removal:**
1. Remove all 5 shadow tokens from fortunetell-frontend
2. Remove all shadow utilities from simple-budget-site
3. Replace with border-based visual separation (1px solid borders, varied radii)

**Spacing Consolidation:**
1. fortunetell-frontend spacing scale is already compliant (4px increments)
2. simple-budget-site → map Tailwind spacing to 10-step token scale

**Module-Level Styling:**
1. fortunetell-frontend → minor refinements (remove shadows, reduce color tokens)
2. simple-budget-site → **FULL REFACTOR** (remove Tailwind, add token system, add component modules)

---

### 8.3 Task Dependencies

**Task 2 (Develop 2.0 Spec):**
- Use color reduction strategy above (20+ → 11)
- Specify weight reduction (4 → 2)
- Specify shadow removal (all → none)
- Confirm spacing scale (10-step, 4px increments)

**Task 3 (Canonical tokens.css):**
- Implement 11-color palette
- Implement 8-size, 2-weight typography
- Implement 10-step spacing
- Remove all shadow tokens
- Add motion tokens (if transitions retained)

**Task 4 (fortunetell-site Implementation):**
- Use Task 3 tokens.css
- Follow fortunetell-frontend component-scoped pattern

**Task 5 (simple-budget-site Refactor):**
- **FULL TAILWIND REMOVAL**
- Implement Task 3 tokens.css
- Convert global.css raw values → token references
- Add component modules (if migrating to SPA)

**Task 6 (fortunetell-frontend Refactor):**
- Reduce colors (20+ → 11)
- Reduce weights (4 → 2)
- **REMOVE ALL SHADOWS**
- Update globals.css and component modules to reference new 2.0 tokens

---

## END OF REPORT

**Analysis Completed:** 2026-01-30
**Next Task:** Task 2 - Develop 2.0 Token System Specification (based on this analysis)
**Status:** ✓ ANALYSIS COMPLETE - NO CODE CHANGES MADE
