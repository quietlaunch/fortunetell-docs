# Task 2: Color Usage Analysis & Semantic Alias Removal Plan
## Strict Color Application Rules for 2.0 (Spec/Plan Only — No Code Changes)

**Date:** 2026-01-30
**Scope:** Analyze current semantic color aliases, define 2.0 color application rules, create alias removal plan
**Authority:** DEV_DesignTokensSystem_1.4 + V10.2.1 Architecture
**Constraint:** Only 11 base color tokens allowed; no semantic alias layer survives

---

## STEP 0: THE 11 BASE COLORS (1.4 Spec — Non-Negotiable)

### Theme Colors (5)
- `--theme-primary`: #000000 (black)
- `--theme-secondary`: #3f3f3f (dark gray)
- `--theme-tertiary`: #7f7f7f (medium gray)
- `--theme-accent`: #b7b7b7 (light gray)
- `--theme-background`: #ffffff (white)

### Semantic Colors (6 — Forecast-Driven)
- `--semantic-color-safe`: #10b981 (emerald green)
- `--semantic-color-safe-hover`: (derived from safe)
- `--semantic-color-warning`: #f6e823 (yellow)
- `--semantic-color-warning-hover`: (derived from warning)
- `--semantic-color-danger`: #ff1a1a (red)
- `--semantic-color-danger-hover`: (derived from danger)

**All other color tokens must be expressed as `var(...)` references to one of these 11.**

---

## STEP 1: CURRENT COLOR USAGE & SEMANTIC ALIASES

### 1.1 Semantic Alias Inventory (fortunetell-frontend)

**Current implementation uses 50+ semantic alias tokens organized in a 4-layer hierarchy:**

#### Layer 1: Brand Base Values (Raw Hex)
| Token Name | Current Value | Maps to 1.4 Base | Status |
|-----------|---------------|------------------|--------|
| `--brand-green` | #10b981 | `--semantic-color-safe` | Direct match |
| `--brand-yellow` | #f6e823 | `--semantic-color-warning` | Direct match |
| `--brand-red` | #ff1a1a | `--semantic-color-danger` | Direct match |
| `--brand-green-dark` | #047857 | (none) | OUT-OF-POLICY hover variant |
| `--brand-green-light` | #6ee7b7 | (none) | OUT-OF-POLICY variant |

#### Layer 2: Neutral Base Values (Raw Hex)
| Token Name | Current Value | Maps to 1.4 Base | Status |
|-----------|---------------|------------------|--------|
| **Backgrounds** |
| `--neutral-bg-0` | #ffffff | `--theme-background` | Direct match |
| `--neutral-bg-1` | #f9fafb | (none) | OUT-OF-POLICY light gray |
| `--neutral-bg-2` | #f3f4f6 | (none) | OUT-OF-POLICY light gray |
| `--neutral-bg-dark-0` | #0b1120 | (none) | OUT-OF-POLICY dark blue |
| `--neutral-bg-dark-1` | #020617 | (none) | OUT-OF-POLICY dark blue |
| **Borders** |
| `--neutral-border-subtle` | #e2e8f0 | (none) | OUT-OF-POLICY light gray |
| `--neutral-border-strong` | #cbd5f5 | (none) | OUT-OF-POLICY light gray |
| **Text on Light** |
| `--neutral-text-strong` | #0f172a | `--theme-primary` | Approximate match (not exact black) |
| `--neutral-text-body` | #334155 | `--theme-secondary` | Approximate match (not exact) |
| `--neutral-text-muted` | #475569 | (none) | OUT-OF-POLICY gray |
| `--neutral-text-low` | #64748b | `--theme-tertiary` | Approximate match |
| **Text on Dark/Brand** |
| `--neutral-text-on-dark` | #f9fafb | `--theme-background` | Approximate match (off-white) |
| `--neutral-text-on-brand` | #ffffff | `--theme-background` | Direct match |

#### Layer 3: Semantic State Aliases (Point to Brand)
| Token Name | Current Value | Used In | Maps to 1.4 Base |
|-----------|---------------|---------|------------------|
| `--financial-green` | var(--brand-green) | AccountView, HistoryPanel, TxList (positive amounts) | `--semantic-color-safe` |
| `--financial-yellow` | var(--brand-yellow) | (defined but barely used) | `--semantic-color-warning` |
| `--financial-red` | var(--brand-red) | AccountView, HistoryPanel, TxList (negative amounts) | `--semantic-color-danger` |
| `--bg-safe` | var(--brand-green) | CalendarMonth (safe days) | `--semantic-color-safe` |
| `--bg-warning` | var(--brand-yellow) | CalendarMonth (warning days) | `--semantic-color-warning` |
| `--bg-danger` | var(--brand-red) | CalendarMonth (danger days), LoginForm error, TxForm error, button-danger | `--semantic-color-danger` |
| `--bg-safe-hover` | #22c664 | CalendarMonth hover | OUT-OF-POLICY hover variant |
| `--bg-warning-hover` | #f5e50a | CalendarMonth hover | OUT-OF-POLICY hover variant |
| `--bg-danger-hover` | #e60000 | CalendarMonth hover | OUT-OF-POLICY hover variant |
| `--text-on-semantic` | var(--neutral-text-on-brand) | CalendarMonth text on safe/warning/danger backgrounds | `--theme-background` |

#### Layer 4: Role Aliases (Point to Neutral or Semantic)
| Token Category | Token Names | Points To | Used In |
|---------------|-------------|-----------|---------|
| **Text Roles** | `--text-strong`, `--text-body`, `--text-muted`, `--text-low`, `--text-on-dark`, `--text-on-brand`, `--text-placeholder` | Neutral text bases | All components (15+ files) |
| **Surface Roles** | `--surface-page`, `--surface-subtle`, `--surface-muted`, `--surface-dark-page`, `--surface-dark-panel` | Neutral bg bases | globals.css, SettingsView, AccountView |
| **Border/Divider Roles** | `--border-subtle`, `--border-strong`, `--divider-strong`, `--divider-soft` | Neutral border bases | All card/panel components |
| **App Roles** | `--app-surface-0`, `--app-surface-1`, `--app-border`, `--app-text-primary`, `--app-text-secondary`, `--app-text-muted`, `--app-card`, `--app-card-muted`, `--app-card-border`, `--app-accent`, `--app-accent-foreground` | Surface/text/semantic bases | All app components (20+ files) |
| **Landing Roles** | `--landing-bg`, `--landing-surface`, `--landing-text-primary`, `--landing-text-muted`, `--landing-card-bg`, `--landing-card-border`, `--landing-card-shadow` | Surface/text bases | page.module.css (landing page) |
| **Button Roles** | `--button-primary-bg`, `--button-primary-bg-hover`, `--button-primary-text`, `--button-secondary-bg`, `--button-secondary-border`, `--button-secondary-text`, `--button-danger-bg`, `--button-danger-bg-hover`, `--button-danger-text`, `--button-danger-border` | Brand/neutral bases | globals.css button classes |
| **Input Roles** | `--input-bg`, `--input-border`, `--input-text`, `--input-placeholder`, `--input-focus-border` | Neutral/brand bases | globals.css input classes |
| **Icon Roles** | `--icon-color-default`, `--icon-color-strong`, `--icon-color-inverse` | App text bases | tokens.css (not directly used in components) |

**Total Semantic Alias Tokens:** ~50 tokens organized in 4 abstraction layers
**Out-of-Policy Colors:** 15+ colors that don't map to any of the 11 base colors

### 1.2 simple-budget-site Color Usage

**No semantic token system defined.** Uses:
- Raw Tailwind utilities: `bg-emerald-600`, `text-emerald-600`, `bg-white`, `text-slate-900`, etc.
- Raw hex in global.css: `#059669`, `#0f172a`, `#334155`, `#64748b`, `#e2e8f0`, etc.
- All color usage is OUT-OF-POLICY (Tailwind palette, not 1.4 base colors)
- Will require full Tailwind removal and replacement with base color references (Task 5)

### 1.3 Classification Summary

| Category | Count | Example Tokens |
|----------|-------|----------------|
| **Direct uses of 11 base tokens** | 3 | `--brand-green`, `--brand-yellow`, `--brand-red` (exact matches to semantic colors) |
| **Semantic aliases on top of base tokens** | ~45 | `--financial-green`, `--bg-safe`, `--text-strong`, `--app-text-primary`, `--button-primary-bg`, etc. |
| **Out-of-policy colors** | 15+ | `--neutral-bg-1`, `--neutral-text-muted`, `--bg-safe-hover`, `--brand-green-dark`, all simple-budget-site colors |

---

## STEP 2: STRICT COLOR APPLICATION RULES (2.0)

**Constraint:** Only 11 base color tokens exist. No semantic alias layer survives.

### 2.1 Text Roles

| Use Case | 2.0 Base Color | Rationale |
|----------|----------------|-----------|
| **Main body text** | `--theme-secondary` (#3f3f3f) | Dark gray provides readable contrast on white without harshness of pure black |
| **Headings / high-emphasis text** | `--theme-primary` (#000000) | Pure black for maximum contrast and hierarchy |
| **Muted / secondary text** | `--theme-tertiary` (#7f7f7f) | Medium gray for de-emphasized content |
| **Low-contrast labels / helper text** | `--theme-accent` (#b7b7b7) | Light gray for lowest-priority text |
| **Text on dark surfaces** | `--theme-background` (#ffffff) | White text on dark backgrounds for contrast |
| **Text on semantic backgrounds (safe/warning/danger)** | `--theme-background` (#ffffff) OR `--theme-primary` (#000000) | Depends on semantic color luminance; white on emerald/red, black on yellow |
| **Error / validation messages** | `--semantic-color-danger` (#ff1a1a) | Red text for inline errors |
| **Success messages** | `--semantic-color-safe` (#10b981) | Green text for success states |

### 2.2 Surface Roles

| Use Case | 2.0 Base Color | Rationale |
|----------|----------------|-----------|
| **Page background** | `--theme-background` (#ffffff) | White base for light mode |
| **Card / panel backgrounds** | `--theme-background` (#ffffff) | Same as page (flat design, no surface elevation) |
| **Muted / striped row backgrounds** | `--theme-accent` (#b7b7b7) @ low opacity OR use `--theme-background` with border only | Light gray for subtle differentiation |
| **Dark panels (if needed)** | `--theme-primary` (#000000) | Black for inverted sections (landing hero, dark mode) |
| **Semantic backgrounds (badges, calendar day states, alerts)** | `--semantic-color-safe`, `--semantic-color-warning`, `--semantic-color-danger` | Use semantic colors directly for state communication |

**Note on hover states:** 2.0 does NOT define separate hover colors. Hover effects must use:
- Opacity changes (e.g., `opacity: 0.8`)
- Mix-blend-mode or filter effects (e.g., `filter: brightness(0.9)`)
- Border color changes using existing base colors

### 2.3 Lines / Dividers / Borders

| Use Case | 2.0 Base Color | Rationale |
|----------|----------------|-----------|
| **Neutral borders / dividers** | `--theme-accent` (#b7b7b7) | Light gray provides subtle separation |
| **Emphasized borders (active, selected, focus)** | `--semantic-color-safe` (#10b981) | Green for interactive focus states |
| **Danger borders (error inputs)** | `--semantic-color-danger` (#ff1a1a) | Red for validation errors |

### 2.4 Buttons

| Button Type | Property | 2.0 Base Color | Notes |
|------------|----------|----------------|-------|
| **Primary** | Background (default) | `--semantic-color-safe` | Green (currently --brand-green) |
| | Background (hover) | `--semantic-color-safe` @ 90% brightness | NO separate hover color; use filter or opacity |
| | Text | `--theme-background` | White on green |
| | Border | `--semantic-color-safe` | Same as background |
| **Secondary** | Background (default) | `--theme-background` OR transparent | White/transparent |
| | Background (hover) | `--theme-accent` @ low opacity | Subtle gray tint on hover |
| | Text | `--theme-primary` | Black text |
| | Border | `--theme-accent` | Light gray border |
| **Danger** | Background (default) | `--semantic-color-danger` | Red (currently --brand-red) |
| | Background (hover) | `--semantic-color-danger` @ 90% brightness | NO separate hover color; use filter or opacity |
| | Text | `--theme-background` | White on red |
| | Border | `--semantic-color-danger` | Same as background |

### 2.5 High-Contrast Mode Behavior

**Constraint:** Semantic base values DO NOT change in HC mode. Only structural/text/surface tokens remap.

| Token | Normal Mode | HC Mode | Notes |
|-------|-------------|---------|-------|
| **Surfaces** |
| Page background | `--theme-background` (#ffffff) | `--theme-primary` (#000000) | Inverts to black |
| Card backgrounds | `--theme-background` (#ffffff) | `--theme-primary` (#000000) | Inverts to black |
| **Text** |
| Body text | `--theme-secondary` (#3f3f3f) | `--theme-background` (#ffffff) | Inverts to white |
| Headings | `--theme-primary` (#000000) | `--theme-background` (#ffffff) | Inverts to white |
| Muted text | `--theme-tertiary` (#7f7f7f) | `--theme-accent` (#b7b7b7) | Remains gray (higher contrast variant) |
| **Borders** |
| Borders/dividers | `--theme-accent` (#b7b7b7) | `--theme-tertiary` (#7f7f7f) | Darker gray for visibility on black |
| **Semantic Colors** |
| Safe/warning/danger | UNCHANGED | UNCHANGED | Green/yellow/red remain the same for state communication |
| Text on semantic | `--theme-background` (#ffffff) | `--theme-primary` (#000000) | Yellow requires black text for contrast |

**HC Implementation:** CSS remaps structural tokens via `:root.ft-high-contrast` class; semantic colors remain static.

---

## STEP 3: ALIAS REMOVAL PLAN (NO CODE CHANGES YET)

### 3.1 Removal Strategy Overview

**Goal:** Replace 50+ semantic alias tokens with direct references to 11 base colors.

**Approach:**
1. **Delete all alias token definitions** from tokens.css
2. **Update component CSS** to reference base colors directly via `var(--theme-*)` or `var(--semantic-color-*)`
3. **HC mode remapping** moved to `:root.ft-high-contrast` block (maps base tokens only, not aliases)

### 3.2 Alias Removal Table

#### 3.2.1 Brand & Neutral Base Aliases (Layer 1-2)

| Existing Token | Current Value | 2.0 Base Color to Use | Migration Action |
|---------------|---------------|----------------------|------------------|
| **Brand Bases** |
| `--brand-green` | #10b981 | `--semantic-color-safe` | Replace all references with `var(--semantic-color-safe)`, delete token |
| `--brand-yellow` | #f6e823 | `--semantic-color-warning` | Replace all references with `var(--semantic-color-warning)`, delete token |
| `--brand-red` | #ff1a1a | `--semantic-color-danger` | Replace all references with `var(--semantic-color-danger)`, delete token |
| `--brand-green-dark` | #047857 | OUT-OF-POLICY | Cannot map cleanly; replace hover logic with `filter: brightness(0.85)` on `--semantic-color-safe` |
| `--brand-green-light` | #6ee7b7 | OUT-OF-POLICY | Unused in code; delete token without replacement |
| **Neutral Backgrounds** |
| `--neutral-bg-0` | #ffffff | `--theme-background` | Replace all references, delete token |
| `--neutral-bg-1` | #f9fafb | OUT-OF-POLICY | Cannot map cleanly; use `--theme-background` (flat design, no surface tiers) |
| `--neutral-bg-2` | #f3f4f6 | OUT-OF-POLICY | Cannot map cleanly; use `--theme-accent` @ 10% opacity OR `--theme-background` with border |
| `--neutral-bg-dark-0` | #0b1120 | OUT-OF-POLICY | Replace with `--theme-primary` (pure black for dark surfaces) |
| `--neutral-bg-dark-1` | #020617 | OUT-OF-POLICY | Replace with `--theme-primary` (flat design, no dark tiers) |
| **Neutral Borders** |
| `--neutral-border-subtle` | #e2e8f0 | OUT-OF-POLICY | Replace with `--theme-accent` (#b7b7b7); may require opacity adjustment for lighter appearance |
| `--neutral-border-strong` | #cbd5f5 | OUT-OF-POLICY | Replace with `--theme-tertiary` (#7f7f7f) for emphasized borders |
| **Neutral Text on Light** |
| `--neutral-text-strong` | #0f172a | `--theme-primary` | Replace with `--theme-primary` (pure black); slightly different shade but acceptable |
| `--neutral-text-body` | #334155 | `--theme-secondary` | Replace with `--theme-secondary` (#3f3f3f); slightly different shade |
| `--neutral-text-muted` | #475569 | OUT-OF-POLICY | Replace with `--theme-tertiary` (#7f7f7f); slightly lighter gray |
| `--neutral-text-low` | #64748b | `--theme-tertiary` | Replace with `--theme-tertiary` (approximate match) |
| **Neutral Text on Dark/Brand** |
| `--neutral-text-on-dark` | #f9fafb | `--theme-background` | Replace with `--theme-background` (pure white); off-white becomes white |
| `--neutral-text-on-brand` | #ffffff | `--theme-background` | Direct match; replace with `var(--theme-background)`, delete alias |

#### 3.2.2 Semantic State Aliases (Layer 3)

| Existing Token | Current Value | 2.0 Base Color to Use | Migration Action |
|---------------|---------------|----------------------|------------------|
| `--financial-green` | var(--brand-green) | `--semantic-color-safe` | Replace with `var(--semantic-color-safe)`, delete token |
| `--financial-yellow` | var(--brand-yellow) | `--semantic-color-warning` | Replace with `var(--semantic-color-warning)`, delete token |
| `--financial-red` | var(--brand-red) | `--semantic-color-danger` | Replace with `var(--semantic-color-danger)`, delete token |
| `--bg-safe` | var(--brand-green) | `--semantic-color-safe` | Replace with `var(--semantic-color-safe)`, delete token |
| `--bg-warning` | var(--brand-yellow) | `--semantic-color-warning` | Replace with `var(--semantic-color-warning)`, delete token |
| `--bg-danger` | var(--brand-red) | `--semantic-color-danger` | Replace with `var(--semantic-color-danger)`, delete token |
| `--bg-safe-hover` | #22c664 | OUT-OF-POLICY | Replace hover logic with `filter: brightness(1.1)` on `--semantic-color-safe`, delete token |
| `--bg-warning-hover` | #f5e50a | OUT-OF-POLICY | Replace hover logic with `filter: brightness(0.95)` on `--semantic-color-warning`, delete token |
| `--bg-danger-hover` | #e60000 | OUT-OF-POLICY | Replace hover logic with `filter: brightness(0.9)` on `--semantic-color-danger`, delete token |
| `--text-on-semantic` | var(--neutral-text-on-brand) | `--theme-background` | Replace with `var(--theme-background)`, delete token |

#### 3.2.3 Role Aliases (Layer 4)

| Existing Token | Points To | 2.0 Base Color to Use | Migration Action |
|---------------|-----------|----------------------|------------------|
| **Text Roles** |
| `--text-strong` | var(--neutral-text-strong) | `--theme-primary` | Delete alias; update all usages to `var(--theme-primary)` |
| `--text-body` | var(--neutral-text-body) | `--theme-secondary` | Delete alias; update all usages to `var(--theme-secondary)` |
| `--text-muted` | var(--neutral-text-muted) | `--theme-tertiary` | Delete alias; update all usages to `var(--theme-tertiary)` |
| `--text-low` | var(--neutral-text-low) | `--theme-tertiary` | Delete alias; update all usages to `var(--theme-tertiary)` (merge with --text-muted) |
| `--text-on-dark` | var(--neutral-text-on-dark) | `--theme-background` | Delete alias; update all usages to `var(--theme-background)` |
| `--text-on-brand` | var(--neutral-text-on-brand) | `--theme-background` | Delete alias; update all usages to `var(--theme-background)` |
| `--text-placeholder` | var(--text-placeholder-base) | `--theme-accent` | Delete alias; update all usages to `var(--theme-accent)` |
| **Surface Roles** |
| `--surface-page` | var(--neutral-bg-0) | `--theme-background` | Delete alias; update all usages to `var(--theme-background)` |
| `--surface-subtle` | var(--neutral-bg-1) | `--theme-background` | Delete alias; use `--theme-background` (no surface tiers in 2.0) |
| `--surface-muted` | var(--neutral-bg-2) | `--theme-background` OR `--theme-accent` @ opacity | Delete alias; use `--theme-background` with borders instead |
| `--surface-dark-page` | var(--neutral-bg-dark-0) | `--theme-primary` | Delete alias; update all usages to `var(--theme-primary)` |
| `--surface-dark-panel` | var(--neutral-bg-dark-1) | `--theme-primary` | Delete alias; use `--theme-primary` (no dark tiers) |
| **Border/Divider Roles** |
| `--border-subtle` | var(--neutral-border-subtle) | `--theme-accent` | Delete alias; update all usages to `var(--theme-accent)` |
| `--border-strong` | var(--neutral-border-strong) | `--theme-tertiary` | Delete alias; update all usages to `var(--theme-tertiary)` |
| `--divider-strong` | var(--border-subtle) | `--theme-accent` | Delete alias; use `var(--theme-accent)` |
| `--divider-soft` | var(--neutral-bg-2) | `--theme-accent` @ opacity | Delete alias; use `var(--theme-accent)` with opacity or borders |
| `--divider-color` | var(--divider-strong) | `--theme-accent` | Delete alias; use `var(--theme-accent)` |
| `--divider-color-soft` | var(--divider-soft) | `--theme-accent` @ opacity | Delete alias; use `var(--theme-accent)` |
| **App Roles** |
| `--app-surface-0` | var(--surface-page) | `--theme-background` | Delete alias; update all usages to `var(--theme-background)` |
| `--app-surface-1` | var(--surface-subtle) | `--theme-background` | Delete alias; use `--theme-background` (no surface tiers) |
| `--app-border` | var(--border-subtle) | `--theme-accent` | Delete alias; update all usages to `var(--theme-accent)` |
| `--app-text-primary` | var(--text-strong) | `--theme-primary` | Delete alias; update all usages to `var(--theme-primary)` |
| `--app-text-secondary` | var(--text-body) | `--theme-secondary` | Delete alias; update all usages to `var(--theme-secondary)` |
| `--app-text-muted` | var(--text-muted) | `--theme-tertiary` | Delete alias; update all usages to `var(--theme-tertiary)` |
| `--app-card` | var(--surface-page) | `--theme-background` | Delete alias; use `var(--theme-background)` |
| `--app-card-muted` | var(--surface-muted) | `--theme-background` | Delete alias; use `--theme-background` with borders |
| `--app-card-border` | var(--border-subtle) | `--theme-accent` | Delete alias; use `var(--theme-accent)` |
| `--app-accent` | var(--brand-green) | `--semantic-color-safe` | Delete alias; update all usages to `var(--semantic-color-safe)` |
| `--app-accent-foreground` | var(--text-on-brand) | `--theme-background` | Delete alias; use `var(--theme-background)` |
| **Landing Roles** |
| `--landing-bg` | var(--surface-dark-page) | `--theme-primary` | Delete alias; use `var(--theme-primary)` |
| `--landing-surface` | var(--surface-dark-panel) | `--theme-primary` | Delete alias; use `var(--theme-primary)` |
| `--landing-text-primary` | var(--text-on-dark) | `--theme-background` | Delete alias; use `var(--theme-background)` |
| `--landing-text-muted` | var(--text-muted) | `--theme-tertiary` | Delete alias; use `var(--theme-tertiary)` |
| `--landing-card-bg` | var(--surface-dark-panel) | `--theme-primary` | Delete alias; use `var(--theme-primary)` |
| `--landing-card-border` | var(--border-subtle) | `--theme-accent` | Delete alias; use `var(--theme-accent)` |
| **Button Roles** |
| `--button-primary-bg` | var(--brand-green) | `--semantic-color-safe` | Delete alias; use `var(--semantic-color-safe)` |
| `--button-primary-bg-hover` | var(--brand-green-dark) | `--semantic-color-safe` + filter | Delete alias; replace hover with `filter: brightness(0.85)` on safe color |
| `--button-primary-text` | var(--text-on-brand) | `--theme-background` | Delete alias; use `var(--theme-background)` |
| `--button-secondary-bg` | transparent | transparent | Keep as-is (not a color token) |
| `--button-secondary-border` | var(--border-subtle) | `--theme-accent` | Delete alias; use `var(--theme-accent)` |
| `--button-secondary-text` | var(--text-strong) | `--theme-primary` | Delete alias; use `var(--theme-primary)` |
| `--button-danger-bg` | var(--bg-danger) | `--semantic-color-danger` | Delete alias; use `var(--semantic-color-danger)` |
| `--button-danger-bg-hover` | var(--bg-danger-hover) | `--semantic-color-danger` + filter | Delete alias; replace hover with `filter: brightness(0.9)` |
| `--button-danger-text` | var(--text-on-semantic) | `--theme-background` | Delete alias; use `var(--theme-background)` |
| `--button-danger-border` | var(--bg-danger) | `--semantic-color-danger` | Delete alias; use `var(--semantic-color-danger)` |
| **Input Roles** |
| `--input-bg` | var(--surface-page) | `--theme-background` | Delete alias; use `var(--theme-background)` |
| `--input-border` | var(--border-subtle) | `--theme-accent` | Delete alias; use `var(--theme-accent)` |
| `--input-text` | var(--text-body) | `--theme-secondary` | Delete alias; use `var(--theme-secondary)` |
| `--input-placeholder` | var(--text-placeholder) | `--theme-accent` | Delete alias; use `var(--theme-accent)` |
| `--input-focus-border` | var(--brand-green) | `--semantic-color-safe` | Delete alias; use `var(--semantic-color-safe)` |
| **Icon Roles** |
| `--icon-color-default` | var(--app-text-secondary) | `--theme-secondary` | Delete alias; use `var(--theme-secondary)` |
| `--icon-color-strong` | var(--app-text-primary) | `--theme-primary` | Delete alias; use `var(--theme-primary)` |
| `--icon-color-inverse` | var(--text-on-dark) | `--theme-background` | Delete alias; use `var(--theme-background)` |

### 3.3 Out-of-Policy Colors (Cannot Map Cleanly)

| Current Usage | Current Value | Issue | Migration Strategy |
|--------------|---------------|-------|-------------------|
| **Neutral gray tiers** | #f9fafb, #f3f4f6, #e2e8f0, #cbd5f5, #475569, #64748b, #94a3b8 | Too many gray shades; 1.4 only has 4 grays | Consolidate to 4 theme grays; use borders instead of surface tiers; accept slight color shifts |
| **Dark blue surfaces** | #0b1120, #020617 | Not in 1.4 palette | Replace with pure black (#000000 = --theme-primary) |
| **Hover variants** | --brand-green-dark (#047857), --bg-safe/warning/danger-hover (#22c664, #f5e50a, #e60000) | 1.4 does NOT define hover colors | Replace with CSS filters: `brightness(0.85-1.1)` or opacity changes |
| **simple-budget-site colors** | All Tailwind colors (slate, emerald, rose, etc.) | Entire Tailwind palette is out-of-policy | Full Tailwind removal (Task 5); replace with base color references |

**Note:** Out-of-policy colors will cause visual changes when migrated. Some surfaces will lose subtle tonal variation. This is expected and acceptable per 2.0 minimalist design.

---

## STEP 4: IMPLEMENTATION NOTES (FOR FUTURE TASKS)

### 4.1 Component CSS Updates (Task 3)

**For each component module:**
1. Search for all `var(--<alias-name>)` references
2. Replace with corresponding `var(--theme-*)` or `var(--semantic-color-*)` per removal table
3. Update hover states to use `filter` or `opacity` instead of separate hover colors

**Example:**
```css
/* Before */
.button {
  background-color: var(--button-primary-bg);
  color: var(--button-primary-text);
}
.button:hover {
  background-color: var(--button-primary-bg-hover);
}

/* After */
.button {
  background-color: var(--semantic-color-safe);
  color: var(--theme-background);
  transition: filter 150ms ease;
}
.button:hover {
  filter: brightness(0.85);
}
```

### 4.2 HC Mode Remapping (Task 3)

**Update `:root.ft-high-contrast` block:**
- Remove all alias remappings
- Only remap the 11 base tokens (structural tokens only)
- Semantic colors remain unchanged

**Example:**
```css
/* Before (invalid) */
:root.ft-high-contrast {
  --surface-page: #000000;
  --text-strong: #ffffff;
  --app-surface-0: var(--surface-page);
  --app-text-primary: var(--text-strong);
}

/* After (valid) */
:root.ft-high-contrast {
  /* Structural theme colors remap */
  --theme-primary: #ffffff;    /* was black, now white */
  --theme-background: #000000; /* was white, now black */
  --theme-tertiary: #b7b7b7;   /* lighter for dark bg */
  --theme-accent: #7f7f7f;     /* darker for dark bg */

  /* Semantic colors UNCHANGED */
  /* --semantic-color-safe, warning, danger remain as-is */
}
```

### 4.3 simple-budget-site Migration (Task 5)

- Remove Tailwind entirely
- Replace all Tailwind utilities with component classes referencing base colors
- Create new CSS file with base token references
- Will be addressed in dedicated Task 5 prompt

---

## SUMMARY

**Current State:**
- fortunetell-frontend: 50+ semantic alias tokens in 4-layer hierarchy
- simple-budget-site: No token system; raw Tailwind utilities (all out-of-policy)
- 15+ out-of-policy colors that don't map to 1.4 base palette

**2.0 Target State:**
- Only 11 base color tokens: 5 theme + 6 semantic
- No semantic alias layer (no --text-*, --bg-*, --app-*, --button-*, etc.)
- All color references are direct `var(--theme-*)` or `var(--semantic-color-*)`
- Hover states use CSS filters/opacity, not separate color tokens
- HC mode remaps base tokens only; semantic colors unchanged

**Migration Impact:**
- ~50 token deletions from tokens.css
- ~200-300 CSS property updates across components
- Visual changes expected: gray tonal variations will flatten, some surfaces lose subtle differentiation
- Acceptable per V10.2.1 minimalist design principles

**Next Steps (Future Tasks):**
- Task 3: Implement alias removal + component CSS updates
- Task 4: fortunetell-site alignment (TBD)
- Task 5: simple-budget-site Tailwind removal + base color migration

---

**END OF ANALYSIS**
**Status:** Color alias inventory complete, 2.0 rules defined, removal plan ready
**Authority:** DEV_DesignTokensSystem_1.4 (11-color palette is fixed and non-negotiable)
**Scope:** Analysis/planning only - no code changes yet
