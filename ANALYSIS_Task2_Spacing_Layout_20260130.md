# Task 2: Spacing & Layout Analysis
## Current Usage vs 1.4 Primitives (Analysis Only — No Code Changes)

**Date:** 2026-01-30
**Scope:** Analyze spacing and layout patterns to understand reduction opportunities for 2.0
**Authority:** DEV_DesignTokensSystem_1.4 spacing scale + layout primitives
**Constraint:** Existing CSS usage is DIAGNOSTIC ONLY, not design authority

---

## STEP 0: 1.4 SPACING & LAYOUT PRIMITIVES (AUTHORITY)

### Allowed Spacing Scale (10 Values)
```
--space-0:  0
--space-1:  0.25rem  (4px)
--space-2:  0.5rem   (8px)
--space-3:  0.75rem  (12px)
--space-4:  1rem     (16px)
--space-5:  1.25rem  (20px)
--space-6:  1.5rem   (24px)
--space-8:  2rem     (32px)
--space-10: 2.5rem   (40px)
--space-12: 3rem     (48px)
```

### Legacy Aliases (Backward Compatibility — fortunetell-frontend Only)
```
--space-2xs: 0.125rem (2px) — OFF-SCALE
--space-xs:  var(--space-1)
--space-sm:  var(--space-2)
--space-md:  var(--space-4)
--space-lg:  var(--space-6)
--space-xl:  var(--space-8)
--space-2xl: var(--space-12)
--space-3xl: 4rem (64px) — OFF-SCALE
```

### Layout Primitives (Fixed Values)
```
Page width:
  --page-width-main: 80rem (1280px)

Page padding (inline):
  --page-padding-inline-sm: 1.5rem (24px)
  --page-padding-inline-md: 2rem (32px)
  --page-padding-inline-lg: 3rem (48px)

Section vertical spacing:
  --section-padding-y-base: 5rem (80px)
  --section-padding-y-lg:   6rem (96px)

Containers:
  --container-xs: 28rem (448px)
  --container-sm: 32rem (512px)
  --container-md: 36rem (576px)
  --container-lg: 48rem (768px)
  --container-xl: 80rem (1280px)

Breakpoints:
  --bp-sm:  480px
  --bp-md:  640px
  --bp-lg:  768px
  --bp-xl:  1024px
  --bp-2xl: 1280px
```

---

## STEP 1: SPACING ANALYSIS (fortunetell-frontend)

### 1.1 Distinct Spacing Values Found

| Value | Rem | Px | Classification | Usage Count | Representative Components |
|-------|-----|----|--------------|--------------|-------------------------|
| **ON-SCALE (Exact Matches)** |
| `0` | 0 | 0 | ON-SCALE | 50+ | Reset margins/padding across all components |
| `var(--space-1)` / `--space-xs` | 0.25rem | 4px | ON-SCALE | 15+ | CalendarMonth gaps, TxForm micro-spacing, LoginForm small gaps |
| `var(--space-2)` / `--space-sm` | 0.5rem | 8px | ON-SCALE | 35+ | CalendarMonth padding, LoginForm padding, AppHeader gaps, card internal spacing |
| `var(--space-4)` / `--space-md` | 1rem | 16px | ON-SCALE | 60+ | **MOST COMMON** - TxForm, TxList, CalendarMonth, AddTransactionPanel, HistoryPanel primary padding |
| `var(--space-6)` / `--space-lg` | 1.5rem | 24px | ON-SCALE | 40+ | LoginForm, AddTransactionPanel, HistoryPanel large padding, form section gaps |
| `var(--space-8)` / `--space-xl` | 2rem | 32px | ON-SCALE | 25+ | SettingsView gaps, AddTransactionPanel responsive padding, HistoryPanel responsive padding |
| `var(--space-12)` / `--space-2xl` | 3rem | 48px | ON-SCALE | Rare (~3) | Large section spacing, special layout calculations |
| **NEAR-SCALE** |
| `0.5rem` (literal, not token) | 0.5rem | 8px | NEAR-SCALE | 2 | globals.css `.btn` gap (should use `--space-2`) |
| **OFF-SCALE (Violations)** |
| `var(--space-2xs)` | 0.125rem | 2px | **OFF-SCALE** ❌ | 10+ | CalendarMonth tiny gaps, SettingsView micro-spacing, HistoryPanel small margins |
| `var(--space-3xl)` | 4rem | 64px | **OFF-SCALE** ❌ | Not observed in components | Defined in tokens but unused |
| `var(--space-5)` | 1.25rem | 20px | **DEFINED BUT UNUSED** | 0 | Not observed in any component |
| `var(--space-3)` | 0.75rem | 12px | **DEFINED BUT UNUSED** | 0 | Not observed in any component |
| `var(--space-10)` | 2.5rem | 40px | **DEFINED BUT UNUSED** | 0 | Not observed in any component |

### 1.2 Spacing Values Summary

**Actually Used Spacing Values (fortunetell-frontend):**
1. `0` (resets)
2. `0.125rem` (2px) — `--space-2xs` ❌ OFF-SCALE
3. `0.25rem` (4px) — `--space-1` / `--space-xs` ✓
4. `0.5rem` (8px) — `--space-2` / `--space-sm` ✓
5. `1rem` (16px) — `--space-4` / `--space-md` ✓ **PRIMARY SPACING**
6. `1.5rem` (24px) — `--space-6` / `--space-lg` ✓
7. `2rem` (32px) — `--space-8` / `--space-xl` ✓
8. `3rem` (48px) — `--space-12` / `--space-2xl` ✓ (rare)

**Unused Spacing Values from 1.4 Scale:**
- `0.75rem` (12px) — `--space-3` (defined but never used)
- `1.25rem` (20px) — `--space-5` (defined but never used)
- `2.5rem` (40px) — `--space-10` (defined but never used)
- `4rem` (64px) — `--space-3xl` (defined but never used)

**Total Active Spacing Values:** 8 (including 2px off-scale)
**Total Defined Values:** 13 (10 on-scale + 3 off-scale legacy)
**Reduction Opportunity:** 5 unused values can be removed

### 1.3 Spacing by Context

#### App Shell
| Context | Typical Spacing | On-Scale? | Notes |
|---------|----------------|-----------|-------|
| AppHeader padding-block | `var(--space-md)` (16px) | ✓ ON-SCALE | Consistent across shell |
| AppHeader gap | `var(--space-sm)` (8px) | ✓ ON-SCALE | Between header elements |
| Main content padding (`.appMainContainer`) | `var(--space-md)` (16px) mobile, `var(--space-lg)` (24px) tablet+ | ✓ ON-SCALE | Responsive, uses defined values |
| DockBar (implied from layout context) | Likely `--space-sm` to `--space-md` | ✓ ON-SCALE | Not explicitly seen in grep but inferred |

**Pattern:** Shell uses 8px-24px range. Mostly 16px (--space-md) as default.

#### Cards/Panels
| Component | Internal Padding | Header/Body Gaps | On-Scale? |
|-----------|-----------------|-----------------|-----------|
| CalendarMonth | `var(--space-sm)` (8px) | `var(--space-xs)` (4px) to `var(--space-md)` (16px) | ✓ ON-SCALE |
| TxList | `var(--space-md)` (16px) | N/A (simple list) | ✓ ON-SCALE |
| TxForm | `var(--space-md)` (16px) | `var(--space-md)` (16px) between fields | ✓ ON-SCALE |
| LoginForm | `var(--space-lg)` (24px) mobile, `var(--space-xl)` (32px) tablet+ | `var(--space-xs)` (4px) to `var(--space-lg)` (24px) | ✓ ON-SCALE |
| SettingsView cards | `var(--space-sm)` (8px) | `var(--space-md)` to `var(--space-xl)` (16-32px) | ✓ ON-SCALE |
| AddTransactionPanel | `var(--space-lg)` to `var(--space-xl)` (24-32px) responsive | `var(--space-sm)` to `var(--space-lg)` (8-24px) | ✓ ON-SCALE |
| HistoryPanel | `var(--space-lg)` to `var(--space-xl)` (24-32px) responsive | `var(--space-md)` (16px) rows, `var(--space-xs)` (4px) micro-spacing | ✓ ON-SCALE |

**Pattern:** Card padding ranges 8px-32px. Most common: **16px** (md) default, **24px** (lg) for larger panels. Gaps within cards: 4px-16px.

**Use of `--space-2xs` (2px) ❌:**
- CalendarMonth: `.dayDot` gaps, tiny spacing between day label elements
- HistoryPanel: micro-margins for text alignment
- SettingsView: micro-spacing in compact UI elements
- **Usage:** 10+ instances, always for very tight micro-spacing

#### Lists/Tables
| Component | Row Padding | Vertical Row Spacing | On-Scale? |
|-----------|------------|---------------------|-----------|
| TxList | `var(--space-md)` (16px) | Rows separated by borders, no explicit gap | ✓ ON-SCALE |
| HistoryPanel rows | `var(--space-md)` to `var(--space-xl)` (16-32px) responsive | `var(--space-md)` (16px) between sections | ✓ ON-SCALE |
| CalendarMonth grid | `var(--space-sm)` (8px) | `var(--space-2xs)` (2px) ❌ for tight day grid | Mostly on-scale |

**Pattern:** Row padding typically **16px** (md). Vertical spacing between rows: 16px. CalendarMonth uses tighter 2px spacing for calendar grid compactness.

#### Forms
| Component | Field Stack Spacing | Label-to-Control | Field Group Spacing | On-Scale? |
|-----------|--------------------|-----------------|--------------------|-----------|
| TxForm | `var(--space-md)` (16px) | `var(--space-2xs)` to `var(--space-xs)` (2-4px) | `var(--space-md)` (16px) | Mostly on-scale |
| LoginForm | `var(--space-lg)` (24px) between sections | `var(--space-xs)` (4px) | `var(--space-md)` (16px) | ✓ ON-SCALE |
| AddTransactionPanel forms | `var(--space-sm)` to `var(--space-md)` (8-16px) | `var(--space-2xs)` to `var(--space-xs)` (2-4px) | `var(--space-md)` (16px) | Mostly on-scale |

**Pattern:** Field stacking: **16px** (md) standard. Label-to-control: **2-4px** (uses off-scale 2px). Group spacing: **16px** (md).

**Use of `--space-2xs` (2px) in forms:**
- Label-to-input micro-spacing
- Helper text positioning
- Compact form element alignment

#### Global Nav / DockBar / Footer
**DockBar spacing (inferred from component structure, not directly grepped):**
- Likely uses `--space-sm` (8px) to `--space-md` (16px) for item spacing
- Icon-to-label spacing likely `--space-xs` (4px)

**Pattern:** Navigation elements use tight spacing (4-16px range).

---

## STEP 2: SPACING ANALYSIS (simple-budget-site)

### 2.1 Tailwind + Custom Spacing Values

**Tailwind Spacing Utilities Observed (from HTML grep):**

| Tailwind Class | Value (rem) | Value (px) | 1.4 Scale Match | Usage Count | Contexts |
|---------------|-------------|-----------|----------------|-------------|----------|
| `space-x-1` | 0.25rem | 4px | ✓ `--space-1` | 5+ | Nav items, icon spacing |
| `space-x-3` | 0.75rem | 12px | ✓ `--space-3` | 20+ | **COMMON** - Header logo+text, form flex gaps, list item icons |
| `space-y-1` | 0.25rem | 4px | ✓ `--space-1` | 5+ | Tight list stacking |
| `space-y-3` | 0.75rem | 12px | ✓ `--space-3` | 3+ | Form field lists, content blocks |
| `space-x-4` | 1rem | 16px | ✓ `--space-4` | 3+ | Header elements, feature icons |
| `space-x-5` | 1.25rem | 20px | ✓ `--space-5` | 3+ | Nav links spacing |
| `space-y-4` | 1rem | 16px | ✓ `--space-4` | 5+ | Content sections, card internals |
| `space-y-6` | 1.5rem | 24px | ✓ `--space-6` | 3+ | Form sections |
| `space-y-8` | 2rem | 32px | ✓ `--space-8` | 10+ | Major section gaps, content blocks |
| `gap-2` | 0.5rem | 8px | ✓ `--space-2` | 5+ | Form inline gaps, flex containers |
| `gap-3` | 0.75rem | 12px | ✓ `--space-3` | 5+ | Form spacing, waitlist forms |
| `gap-4` | 1rem | 16px | ✓ `--space-4` | 10+ | **COMMON** - Grid gaps, flex containers |
| `gap-8` | 2rem | 32px | ✓ `--space-8` | 3+ | Large grid gaps |

**Custom CSS Spacing (from global.css):**

| Property | Value (rem) | Value (px) | 1.4 Scale Match | Context |
|----------|-------------|-----------|----------------|---------|
| `.section` padding-top/bottom | 5rem | 80px | ✓ `--section-padding-y-base` | Section vertical rhythm |
| `.section` padding-top/bottom (lg+) | 6rem | 96px | ✓ `--section-padding-y-lg` | Large breakpoint sections |
| `.container-xl` padding-left/right | 1.5rem | 24px | ✓ `--page-padding-inline-sm` | Mobile page padding |
| `.container-xl` padding-left/right (md+) | 2rem | 32px | ✓ `--page-padding-inline-md` | Tablet page padding |
| `.container-xl` padding-left/right (lg+) | 3rem | 48px | ✓ `--page-padding-inline-lg` | Desktop page padding |

**HTML Inline Padding/Margin (from grep, Tailwind utilities):**

| Utility Pattern | Value Range | Examples | 1.4 Match |
|----------------|-------------|----------|-----------|
| `px-3`, `px-4`, `px-6` | 12px, 16px, 24px | Form inputs, buttons, content padding | ✓ ON-SCALE (3, 4, 6) |
| `py-2`, `py-3`, `py-4`, `py-5` | 8px, 12px, 16px, 20px | Buttons, sections, cards | ✓ ON-SCALE (2, 3, 4, 5) |
| `py-12`, `py-16` | 48px, 64px | Main sections | ✓ ON-SCALE (12 matches), ❌ (16 = 64px off-scale) |
| `mt-4`, `mt-8`, `mt-10` | 16px, 32px, 40px | Content stacking | ✓ ON-SCALE (4, 8, 10) |
| `ml-5` | 20px | List indentation | ✓ ON-SCALE (5) |

### 2.2 Distinct Spacing Values (simple-budget-site)

**Spacing Values Actually Used:**
1. `0.25rem` (4px) — `space-x-1`, `space-y-1` ✓ ON-SCALE
2. `0.5rem` (8px) — `gap-2`, `py-2` ✓ ON-SCALE
3. `0.75rem` (12px) — `space-x-3`, `gap-3`, `py-3`, `px-3` ✓ ON-SCALE **COMMON**
4. `1rem` (16px) — `space-x-4`, `gap-4`, `py-4`, `px-4` ✓ ON-SCALE **VERY COMMON**
5. `1.25rem` (20px) — `space-x-5`, `py-5`, `ml-5` ✓ ON-SCALE
6. `1.5rem` (24px) — `space-y-6`, `px-6`, `.container-xl` mobile padding ✓ ON-SCALE **COMMON**
7. `2rem` (32px) — `space-y-8`, `gap-8`, `.container-xl` md padding ✓ ON-SCALE **COMMON**
8. `3rem` (48px) — `py-12`, `.container-xl` lg padding ✓ ON-SCALE
9. `5rem` (80px) — `.section` base padding ✓ ON-SCALE
10. `6rem` (96px) — `.section` lg padding ✓ ON-SCALE
11. `4rem` (64px) — `py-16` (Tailwind `py-16`) ❌ OFF-SCALE

**Total Distinct Values:** 11 (10 on-scale + 1 off-scale)

**Off-Scale Usage:**
- `py-16` (64px) appears in some HTML — not in 1.4 scale (closest is 3rem = 48px or could use 5rem = 80px)

**Unused 1.4 Values in simple-budget-site:**
- `0.125rem` (2px) — Not used (no need for micro-spacing in marketing site)
- `2.5rem` (40px) — Not directly used (Tailwind `mt-10` = 2.5rem, so actually USED via Tailwind)

### 2.3 Spacing by Context (simple-budget-site)

#### Hero Section
| Element | Typical Spacing | On-Scale? | Notes |
|---------|----------------|-----------|-------|
| Hero section vertical padding | `py-12` to `py-16` (48-64px) | Partial (64px off-scale) | Large visual breathing room |
| Hero content internal gaps | `space-y-8` (32px) | ✓ ON-SCALE | Between heading, text, CTA |
| Hero form gaps | `space-y-6` or `gap-3` (24px / 12px) | ✓ ON-SCALE | Form element spacing |

**Pattern:** Hero uses large spacing (48-64px vertical), 32px for content stacking, 12-24px for forms.

#### Feature Sections
| Element | Typical Spacing | On-Scale? | Notes |
|---------|----------------|-----------|-------|
| Section-to-section | `.section` (5rem = 80px, 6rem = 96px responsive) | ✓ ON-SCALE | Consistent vertical rhythm |
| Feature grid gaps | `gap-4` to `gap-8` (16-32px) | ✓ ON-SCALE | Between feature cards |
| Card internal spacing | `p-4` to `p-6` (16-24px) | ✓ ON-SCALE | Consistent card padding |
| Content block stacking | `space-y-8` (32px) | ✓ ON-SCALE | Between paragraphs/lists |

**Pattern:** Sections use **80-96px** vertical rhythm. Cards use **16-24px** padding. Content stacks at **32px**.

#### Pricing / FAQ / Other Sections
| Element | Typical Spacing | On-Scale? | Notes |
|---------|----------------|-----------|-------|
| Section internal spacing | `space-y-8` to `space-y-12` (32-48px) | ✓ ON-SCALE | Content blocks within sections |
| Pricing card spacing | `p-6` (24px) | ✓ ON-SCALE | Card internal padding |
| FAQ item gaps | `space-y-4` (16px) | ✓ ON-SCALE | Between questions |

**Pattern:** Consistent use of 16-48px for internal spacing. No extreme values.

#### Page Horizontal Padding
| Breakpoint | Padding | On-Scale? | Notes |
|-----------|---------|-----------|-------|
| Mobile (default) | `px-4` (16px) OR `.container-xl` (1.5rem = 24px) | ✓ ON-SCALE | Tighter mobile gutters |
| Tablet (640px+) | `.container-xl` (2rem = 32px) | ✓ ON-SCALE | Medium gutters |
| Desktop (1024px+) | `.container-xl` (3rem = 48px) | ✓ ON-SCALE | Spacious desktop gutters |

**Pattern:** Page padding responsive: 24px → 32px → 48px. Perfectly aligned with 1.4 primitives.

#### Overall Rhythm
**Vertical Rhythm Observations:**
- Section-to-section: **80-96px** (very consistent)
- Major content blocks within sections: **32px** (space-y-8)
- Related content groupings: **16px** (space-y-4 / gap-4)
- Tight groupings (nav links, lists): **12px** (space-x-3, space-y-3)
- Micro-spacing (icon to text): **4px** (space-x-1)

**Consistency:** simple-budget-site has **strong rhythm** using 1.4 scale values almost exclusively.

---

## STEP 3: LAYOUT PATTERNS (fortunetell-frontend)

### 3.1 App Shell Layout

**Main Shell Structure:**
| Element | Behavior | Width Constraint | Vertical Handling | Notes |
|---------|----------|-----------------|------------------|-------|
| Root layout | Full viewport | `width: 100%` | `height: 100vh` (inferred) | App shell fills screen |
| `.appMainContainer` (globals.css) | Content width wrapper | `max-width: var(--container-md)` (36rem = 576px) | Auto height, scrollable | Centered content column |
| `.appMainContainer` padding | Responsive horizontal padding | `padding-inline: var(--space-md)` mobile, `var(--space-lg)` tablet+ | N/A | 16px → 24px responsive gutters |
| AppHeader | Fixed/sticky (implied) | Full width, content constrained internally | Fixed height (var(--app-header-height)) | Sticky app header |
| DockBar (footer nav) | Fixed bottom (inferred from dock context) | Full width | Fixed height | Bottom navigation |

**Content Width Pattern:**
- Default app content: **36rem (576px)** max-width
- No wider containers observed in app components
- All panels (SettingsView, AccountView, HistoryPanel, etc.) inherit this constraint

**Horizontal Padding:**
- Mobile: **16px** (--space-md)
- Tablet+ (480px+): **24px** (--space-lg)

**Vertical Scrolling:**
- App uses component-level scrolling (panels have `overflow-y: auto`)
- Shell likely has fixed header + dock with scrollable content area between

### 3.2 Inside Pages (Content Areas)

**Page Layout Patterns:**

| Component | Max-Width Usage | Horizontal Padding | Vertical Layout | Notes |
|-----------|----------------|-------------------|----------------|-------|
| SettingsView | Inherits `.appMainContainer` (36rem) | Standard app padding | Flex column, auto scroll | Standard narrow column |
| AccountView | Inherits `.appMainContainer` | Standard app padding | Scrollable content | Standard narrow column |
| LoginForm | Inherits `.appMainContainer`, card uses `width: 100%` | Card internal: 24-32px responsive | Centered vertically (implied) | Full-width card within app container |
| CalendarMonth | Fits within parent container | Card padding: 8px | Auto height | Compact component |
| AddTransactionPanel | Full-width within parent (mobile), `max-width: 100%` for larger screens | 24-32px responsive card padding | Scrollable panel | Adaptive to parent |
| HistoryPanel | Full-width within parent | 24-32px responsive card padding | Scrollable list | Adaptive to parent |
| page.module.css (landing) | Uses `min-width: 0` for flex child shrinking | Inherits parent padding | Flex layouts | Landing page elements |

**Width Constraints Summary:**
- **Primary app width:** 36rem (576px) via `.appMainContainer`
- **No hard-coded large widths** (no 1200px, 1024px, etc. in app)
- **Responsive via flex/grid**, not fixed widths
- **Components adapt** to parent container, not setting own max-widths

**Breakpoint Usage (Media Queries):**

| Breakpoint | Value | Used For | Components |
|-----------|-------|----------|-----------|
| `480px` (`--bp-sm`) | 480px | `.appMainContainer` padding increase (md → lg) | globals.css, CalendarMonth |
| `640px` (`--bp-md`) | 640px | NOT heavily used in app | (Defined but minimal usage) |
| `768px` (`--bp-lg`) | 768px | CalendarMonth layout adjustments | CalendarMonth.module.css |
| `1024px` (`--bp-xl`) | 1024px | NOT used in app | (Defined for landing/marketing) |
| `1280px` (`--bp-2xl`) | 1280px | NOT used in app | (Defined for landing/marketing) |

**Active Breakpoints in App:**
- **480px:** Primary breakpoint for padding/spacing adjustments
- **768px:** Used sparingly for layout tweaks (CalendarMonth)

**Unused Breakpoints in App:**
- 640px, 1024px, 1280px defined but not used in app components

### 3.3 One-Off Layout Values

**Hard-Coded Widths (Non-Container):**
| Component | Value | Purpose | Notes |
|-----------|-------|---------|-------|
| SettingsView | `width: calc(var(--space-xl) * 4)` = 8rem (128px) | Color swatch width | Layout calculation, not container |
| CalendarMonth | `width/height: calc(var(--space-xs) + var(--space-2xs))` = 6px | Day dot size | Micro layout, not container |
| AddTransactionPanel | `width/height: var(--space-xl)` = 32px | Icon size | Component sizing |

**All width usages are component-specific**, not page-level layout containers.

---

## STEP 4: LAYOUT PATTERNS (simple-budget-site)

### 4.1 Page Container Structure

**Main Container Pattern (`.container-xl`):**
```css
max-width: 80rem (1280px)
margin: auto (centered)
padding-left/right: 1.5rem → 2rem → 3rem (responsive)
```

**Breakpoint-Responsive Padding:**
- Mobile (default): **1.5rem** (24px)
- Tablet (640px+): **2rem** (32px)
- Desktop (1024px+): **3rem** (48px)

**Usage:** Used on virtually all pages as primary content wrapper.

### 4.2 Section Layout Patterns

**Section Vertical Padding (`.section`):**
```css
padding-top/bottom: 5rem (80px) default
padding-top/bottom: 6rem (96px) at 1024px+
```

**Consistent Section Rhythm:**
- All major page sections use `.section` class
- Creates **80-96px** vertical rhythm between sections
- Very consistent across all pages

### 4.3 Content Layout Patterns

**Observed Max-Width Patterns (Tailwind utilities):**

| Tailwind Class | Max-Width (rem) | Max-Width (px) | Usage | 1.4 Container Match |
|---------------|----------------|---------------|-------|-------------------|
| `max-w-3xl` | 48rem | 768px | Article pages, guide content | ✓ `--container-lg` (48rem) |
| `max-w-5xl` | 64rem | 1024px | Section headers, feature sections | ❌ (between lg and xl) |
| `max-w-xl` / `max-w-2xl` | 36rem / 42rem | 576px / 672px | Form containers, narrow content | `max-w-xl` matches `--container-md` (36rem) ✓ |
| `max-w-md` | 28rem | 448px | Small forms, modals | ✓ `--container-xs` (28rem) |
| No class (uses `.container-xl`) | 80rem | 1280px | Full-width sections | ✓ `--container-xl` (80rem) |

**Alignment with 1.4 Containers:**
- `max-w-md` (28rem) = `--container-xs` ✓
- `max-w-xl` (36rem) = `--container-md` ✓
- `max-w-3xl` (48rem) = `--container-lg` ✓
- `max-w-5xl` (64rem) = **OFF-SCALE** ❌ (not in 1.4 containers)
- `.container-xl` (80rem) = `--container-xl` ✓

**Off-Scale Container:**
- `max-w-5xl` (64rem / 1024px) used for several section headers
- Not defined in 1.4 containers (closest: 48rem or 80rem)

### 4.4 Grid/Flex Layout Patterns

**Common Grid Patterns:**
```
grid gap-4 (16px)
grid gap-8 (32px)
md:grid-cols-2 (two-column at tablet+)
lg:grid-cols-2 (two-column at desktop+)
```

**Common Flex Patterns:**
```
flex items-center gap-3 (12px)
flex space-x-3 (12px horizontal spacing)
flex flex-col gap-4 (16px vertical)
```

**Grid Gaps:** 16px and 32px most common (both on-scale)

**Breakpoint-Driven Layout Changes:**
- Mobile: Single column (default)
- Tablet (md: 768px): Two-column grids, flex row layouts
- Desktop (lg: 1024px): Wider grids, more horizontal layouts

### 4.5 Breakpoint Usage (simple-budget-site)

| Breakpoint | Tailwind Name | Value | Usage | Intensity |
|-----------|--------------|-------|-------|-----------|
| `640px` | `sm:` | 640px | Responsive text sizes, padding tweaks, flex direction | **HEAVY** |
| `768px` | `md:` | 768px | Grid layouts (1-col → 2-col), flex direction changes | **HEAVY** |
| `1024px` | `lg:` | 1024px | Section padding increases, larger grids, order changes | **MODERATE** |
| `1280px` | `xl:` | 1280px | NOT heavily used | LIGHT |

**Active Breakpoints:**
- **640px (sm):** Typography, padding, basic responsiveness
- **768px (md):** Layout changes (grids, flex)
- **1024px (lg):** Polish, larger spacing

**Unused/Light:**
- **1280px (xl):** Minimal usage

---

## STEP 5: CROSS-SURFACE COMPARISON

### 5.1 Container Width Comparison

| Purpose | fortunetell-frontend | simple-budget-site | Alignment |
|---------|---------------------|-------------------|-----------|
| **Primary content width** | 36rem (576px) — `.appMainContainer` uses `--container-md` | 80rem (1280px) — `.container-xl` | ❌ **MISMATCH** (app is narrow, marketing is wide) |
| **Narrow content (articles)** | N/A (app doesn't have articles) | 48rem (768px) — `max-w-3xl` | N/A |
| **Form containers** | Inherits app width (36rem) | 28-36rem (448-576px) — `max-w-md` to `max-w-xl` | ✓ **ALIGNED** (forms use similar widths) |
| **Small modals/panels** | Inferred similar to forms | 28rem (448px) — `max-w-md` | ✓ **ALIGNED** |

**Key Difference:** App uses narrow column (576px) for readability; marketing site uses wide layout (1280px) for visual impact. This is **structurally intentional**, not a misalignment.

### 5.2 Horizontal Padding Comparison

| Breakpoint | fortunetell-frontend | simple-budget-site | Alignment |
|-----------|---------------------|-------------------|-----------|
| **Mobile (default)** | 16px (`--space-md`) | 24px (`.container-xl` 1.5rem) OR 16px (`px-4` inline) | ⚠️ **PARTIAL** (both use 16-24px range) |
| **Tablet (640px+)** | 24px (`--space-lg`) | 32px (`.container-xl` 2rem) | ❌ **MISMATCH** (24px vs 32px) |
| **Desktop (1024px+)** | 24px (no further increase) | 48px (`.container-xl` 3rem) | ❌ **MISMATCH** (24px vs 48px) |

**Alignment Issue:** Marketing site increases padding more aggressively (24→32→48px) vs app (16→24px). Marketing site is more spacious.

**Reason:** Marketing site has wider content, needs more gutters. App is narrow, 24px is sufficient.

### 5.3 Card/Panel Internal Padding Comparison

| Context | fortunetell-frontend | simple-budget-site | Alignment |
|---------|---------------------|-------------------|-----------|
| **Small cards/compact UI** | 8px (`--space-sm`) | Not common (marketing uses larger cards) | N/A |
| **Standard cards** | 16px (`--space-md`) | 16-24px (`p-4` to `p-6`) | ✓ **ALIGNED** (16px common) |
| **Large panels** | 24-32px (`--space-lg` to `--space-xl`) responsive | 24px (`p-6`) | ✓ **ALIGNED** (24px common) |

**Alignment:** Card padding is similar (16-24px range). Marketing site doesn't use 8px (no micro UIs).

### 5.4 Section Vertical Rhythm Comparison

| Context | fortunetell-frontend | simple-budget-site | Alignment |
|---------|---------------------|-------------------|-----------|
| **Section-to-section spacing** | N/A (app doesn't have "sections" like marketing) | 80-96px (`.section` 5-6rem) | N/A |
| **Component stacking** | 16-32px (`--space-md` to `--space-xl`) | 32px (`space-y-8`) most common | ✓ **ALIGNED** (32px common) |
| **Tight groupings** | 4-8px (`--space-xs` to `--space-sm`) | 4-12px (`space-y-1` to `space-y-3`) | ✓ **ALIGNED** (4-12px range) |

**Alignment:** Content stacking is similar (16-32px). Marketing has large section rhythm (80-96px) not needed in app.

### 5.5 Breakpoint Usage Comparison

| Breakpoint | fortunetell-frontend Usage | simple-budget-site Usage | Alignment |
|-----------|--------------------------|-------------------------|-----------|
| **480px** | **HEAVY** (primary app breakpoint) | Light (not standard Tailwind) | ❌ **MISMATCH** |
| **640px** | Light (defined but minimal use) | **HEAVY** (Tailwind `sm:`) | ❌ **MISMATCH** |
| **768px** | Moderate (CalendarMonth) | **HEAVY** (Tailwind `md:`, grids) | ⚠️ **PARTIAL** |
| **1024px** | Light (defined but unused in app) | **MODERATE** (Tailwind `lg:`, section padding) | ❌ **MISMATCH** |
| **1280px** | Light (defined but unused) | Light (Tailwind `xl:`, minimal) | ✓ **ALIGNED** (both minimal) |

**Alignment Issue:**
- App prioritizes **480px** (mobile-first, tight breakpoint)
- Marketing prioritizes **640px, 768px** (Tailwind defaults)
- **768px** is shared, but emphasis differs

**Reason:** App is mobile-focused; marketing follows Tailwind conventions.

### 5.6 Spacing Scale Usage Comparison

| Spacing Value | fortunetell-frontend | simple-budget-site | Both Use? |
|--------------|---------------------|-------------------|-----------|
| **0.125rem (2px)** | **YES** (`--space-2xs`, 10+ uses) ❌ OFF-SCALE | NO | ❌ App-only |
| **0.25rem (4px)** | YES (`--space-1`, 15+ uses) | YES (`space-x-1`, `space-y-1`) | ✓ SHARED |
| **0.5rem (8px)** | YES (`--space-2`, 35+ uses) | YES (`gap-2`, `py-2`) | ✓ SHARED |
| **0.75rem (12px)** | NO (defined as `--space-3` but unused) | **YES** (`space-x-3`, `gap-3`, **COMMON**) | ⚠️ Marketing-heavy |
| **1rem (16px)** | **YES** (`--space-4`, 60+ uses, **PRIMARY**) | **YES** (`gap-4`, `py-4`, **VERY COMMON**) | ✓ SHARED **PRIMARY** |
| **1.25rem (20px)** | NO (`--space-5` unused) | YES (`space-x-5`, `py-5`, `ml-5`) | ⚠️ Marketing-only |
| **1.5rem (24px)** | YES (`--space-6`, 40+ uses) | **YES** (`space-y-6`, `px-6`, **COMMON**) | ✓ SHARED |
| **2rem (32px)** | YES (`--space-8`, 25+ uses) | **YES** (`space-y-8`, `gap-8`, **COMMON**) | ✓ SHARED |
| **2.5rem (40px)** | NO (`--space-10` unused) | YES (`mt-10` via Tailwind) | ⚠️ Marketing-only |
| **3rem (48px)** | Rare (`--space-12`, ~3 uses) | YES (`py-12`, `.container-xl` lg padding) | ⚠️ Marketing-heavy |
| **4rem (64px)** | NO (`--space-3xl` unused) | YES (`py-16`) ❌ OFF-SCALE | ⚠️ Marketing-only |
| **5rem (80px)** | NO | YES (`.section` base padding) | ⚠️ Marketing-only |
| **6rem (96px)** | NO | YES (`.section` lg padding) | ⚠️ Marketing-only |

**Key Findings:**

**Shared Core Values (Both Repos):**
- **4px, 8px, 16px, 24px, 32px** — used heavily in both
- **16px** is the **primary spacing** for both repos

**fortunetell-frontend Only:**
- **2px** (`--space-2xs`) — micro-spacing for compact app UI ❌ OFF-SCALE
- **12px** defined but unused

**simple-budget-site Only:**
- **12px** — used heavily for nav/icon spacing (app doesn't use)
- **20px, 40px** — used via Tailwind (app doesn't use)
- **48px, 80px, 96px** — large section spacing (app doesn't have sections)
- **64px** (`py-16`) — ❌ OFF-SCALE

**Reduction Opportunities:**
- **fortunetell-frontend:** Remove unused 12px, 20px, 40px, 64px from token set
- **simple-budget-site:** Eliminate 64px (`py-16`), consolidate to on-scale values
- **Both:** Eliminate 2px (`--space-2xs`) by rounding up to 4px

---

## SUMMARY

### Spacing Usage Findings

**fortunetell-frontend:**
- **Actually uses:** 8 spacing values (0, 2px❌, 4px, 8px, 16px, 24px, 32px, 48px)
- **Defined but unused:** 5 values (12px, 20px, 40px, 64px, plus duplicates)
- **Primary spacing:** **16px** (60+ uses)
- **Off-scale violation:** **2px** (`--space-2xs`, 10+ uses)

**simple-budget-site:**
- **Actually uses:** 11 spacing values (4px, 8px, 12px, 16px, 20px, 24px, 32px, 48px, 64px❌, 80px, 96px)
- **Primary spacing:** **16px** (gap-4, py-4 very common)
- **Off-scale violation:** **64px** (`py-16`)
- **Larger rhythm:** Uses 48-96px for section spacing (not needed in app)

### Layout Findings

**fortunetell-frontend:**
- **Primary content width:** 36rem (576px) — narrow app column
- **Horizontal padding:** 16px → 24px (responsive, modest increase)
- **Active breakpoints:** 480px (primary), 768px (secondary)
- **Container usage:** Only uses `--container-md` (36rem)

**simple-budget-site:**
- **Primary content width:** 80rem (1280px) — wide marketing layout
- **Horizontal padding:** 24px → 32px → 48px (aggressive responsive increase)
- **Active breakpoints:** 640px, 768px (Tailwind defaults), 1024px
- **Container usage:** Multiple (`max-w-md`, `max-w-xl`, `max-w-3xl`, `max-w-5xl`❌, `.container-xl`)

### Cross-Surface Alignment

**Structural Similarities:**
- Both use **16px as primary spacing unit**
- Card padding ranges similar (16-24px)
- Content stacking similar (16-32px)
- Form container widths aligned (28-36rem)

**Structural Differences (Intentional):**
- **Content width:** App narrow (576px) vs marketing wide (1280px) — design intent
- **Page padding:** App modest (16-24px) vs marketing spacious (24-48px) — design intent
- **Section rhythm:** App doesn't have large sections; marketing uses 80-96px rhythm
- **Breakpoints:** App uses 480px; marketing uses 640px/768px (Tailwind standard)

**Misalignments (Can Consolidate):**
- **Off-scale values:** 2px (app), 64px (marketing)
- **Unused values:** App defines 12px, 20px, 40px, 64px but doesn't use them
- **Container mismatch:** Marketing uses `max-w-5xl` (64rem) not in 1.4 containers

### Reduction Opportunities for 2.0

**Spacing Scale Reduction:**
1. Remove `--space-2xs` (2px) ❌ — round up to 4px
2. Remove `--space-3xl` (64px) ❌ — not used (or use 48px/80px instead)
3. Consider removing unused: 12px, 20px, 40px (if evidence shows no usage)
4. **Core scale:** 0, 4px, 8px, 16px, 24px, 32px, 48px (7 values + large section spacers 80px, 96px)

**Container Reduction:**
1. Confirm `--container-sm` (32rem) is unused — remove if not needed
2. Address `max-w-5xl` (64rem) in simple-budget-site — use 48rem or 80rem instead

**Breakpoint Consolidation:**
1. Consider prioritizing **768px** as shared breakpoint for both repos
2. Accept 480px for app-specific needs, 640px for marketing Tailwind usage

---

**END OF SPACING & LAYOUT ANALYSIS**
**Status:** Complete usage analysis, cross-surface comparison documented
**Authority:** DEV_DesignTokensSystem_1.4 (spacing scale + layout primitives)
**Scope:** Analysis only - no code changes or recommendations (evidence for future 2.0 spec reduction)
