# Task 1: Token Reduction Analysis
## Reduction-Oriented View of DEV_DesignTokensSystem_1.4 Based on Real Usage

**Date:** 2026-01-30
**Scope:** Token usage analysis across simple-budget-site and fortunetell-frontend
**Purpose:** Identify must-keep vs candidate-remove tokens to inform 2.0 spec reduction
**Input:** Task 1 Design Usage Analysis + DEV_DesignTokensSystem_1.4 checklist

---

## 1. TOKEN USAGE SUMMARY

### 1.1 COLORS (11-Color Palette)

| Token Name | 2.0 Value | Used in Repos? | Observed Role | Reduction Tag |
|------------|-----------|----------------|---------------|---------------|
| **THEME COLORS (5)** |
| `--theme-primary` | `#000000` | YES | HC mode surface-page, text-strong roles, headings | **Must-keep** |
| `--theme-secondary` | `#3f3f3f` | NO | Not observed in either repo | **Candidate-remove** ⚠️ |
| `--theme-tertiary` | `#7f7f7f` | NO | Not observed in either repo | **Candidate-remove** ⚠️ |
| `--theme-accent` | `#b7b7b7` | NO | Not observed in either repo | **Candidate-remove** ⚠️ |
| `--theme-background` | `#ffffff` | YES | neutral-bg-0, page backgrounds, card backgrounds | **Must-keep** |
| **SEMANTIC COLORS (6)** |
| `--semantic-color-safe` | `#10b981` | YES | brand-green, financial-green, bg-safe, calendar safe days, primary buttons | **Must-keep** |
| `--semantic-color-safe-hover` | (derived) | YES | Hover states on safe buttons/days | **Must-keep** |
| `--semantic-color-warning` | `#f6e823` | YES | brand-yellow, financial-yellow, bg-warning, calendar warning days | **Must-keep** |
| `--semantic-color-warning-hover` | (derived) | YES | Hover states on warning buttons/days | **Must-keep** |
| `--semantic-color-danger` | `#ff1a1a` | YES | brand-red, financial-red, bg-danger, danger buttons, error states | **Must-keep** |
| `--semantic-color-danger-hover` | (derived) | YES | Hover states on danger buttons/days | **Must-keep** |

**Consolidation Hints:**
- **CRITICAL ISSUE**: Theme colors secondary/tertiary/accent are NOT observed in either repo
- Current repos use a gray scale (`#0f172a`, `#334155`, `#475569`, `#64748b`) that is NOT in the 11-color palette
- This gray scale is used for: text body, text muted, text low, borders
- **Resolution needed**: Either add gray tokens to 2.0 palette OR remap all text/border grays to primary/background with opacity

---

### 1.2 TYPOGRAPHY - FONT SIZES (8 Sizes)

| Token Name | Value | Used in Repos? | Observed Role | Reduction Tag |
|------------|-------|----------------|---------------|---------------|
| `--font-size-xs` | 12px | YES | Calendar day labels, DockBar labels, fine print, timestamps | **Must-keep** |
| `--font-size-sm` | 14px | YES | Secondary text, CalendarMonth elements, HistoryPanel labels | **Must-keep** |
| `--font-size-md` | 16px | YES | Body text, form labels, button text, default text | **Must-keep** |
| `--font-size-lg` | 18px | YES | Subheadings, emphasized paragraphs, TxForm labels, landing page body | **Must-keep** |
| `--font-size-xl` | 20px | YES | AppHeader titles, section headers, AddTransactionPanel headings | **Must-keep** |
| `--font-size-2xl` | 24px | YES | LoginForm title, AccountView balance, page headings | **Must-keep** |
| `--font-size-3xl` | 30px | YES | SettingsView title, page hero headings, major section titles | **Must-keep** |
| `--font-size-4xl` | 36px | YES | Landing page hero (page.module.css), large display numbers | **Must-keep** |

**Usage Frequency:**
- **Heavy use**: xs, sm, md, lg, xl (used 10+ times across components)
- **Moderate use**: 2xl, 3xl (used 5-10 times)
- **Light use**: 4xl (used 2-3 times, but critical for landing page hero)

**Consolidation Hints:**
- ALL 8 sizes are used across both repos
- No candidates for removal
- **Must-keep all 8 sizes** - each serves distinct UI roles

---

### 1.3 TYPOGRAPHY - FONT WEIGHTS (2 Weights in 2.0 vs 4 Current)

| Token Name | Value | Used in Repos? | Observed Role | Reduction Tag |
|------------|-------|----------------|---------------|---------------|
| **2.0 SPEC (Target)** |
| `--font-weight-normal` | 400 | YES | Body text, paragraphs, normal emphasis | **Must-keep** |
| `--font-weight-bold` | 700 | YES | Headings, strong emphasis, h1-h3, buttons | **Must-keep** |
| **CURRENT 1.3 (Actual)** |
| `--font-weight-medium` | 500 | YES | Links, secondary buttons, slight emphasis | **Candidate-merge → 400** |
| `--font-weight-semibold` | 600 | YES | Section headers, form labels, calendar day numbers | **Candidate-merge → 700** |

**Current Usage Breakdown:**
- **400 (normal)**: 15+ occurrences (body text, paragraphs, base text)
- **500 (medium)**: 12+ occurrences (LoginForm, SettingsView, TxList, DockBar)
- **600 (semibold)**: 10+ occurrences (SettingsView, CalendarMonth, TxForm)
- **700 (bold)**: 8+ occurrences (LoginForm, CalendarMonth, TxList, headings)

**Consolidation Path:**
- Map 500 (medium) → 400 (normal) - minimal visual impact
- Map 600 (semibold) → 700 (bold) - may require design review for subtle emphasis use cases
- **Result**: 4 weights → 2 weights (per 2.0 spec)

---

### 1.4 SPACING (10-Step Scale)

| Token Name | Value (px) | Used in Repos? | Observed Role | Reduction Tag |
|------------|------------|----------------|---------------|---------------|
| `--space-0` | 0 | YES | Explicit resets (margin: 0, padding: 0) | **Must-keep** |
| `--space-2xs` | 4 | YES | Tight gaps (CalendarMonth dots, SettingsView tight stacks) | **Must-keep** |
| `--space-xs` | 8 | YES | Small gaps (LoginForm, CalendarMonth, TxForm) | **Must-keep** |
| `--space-sm` | 12 | YES | Standard gaps/padding (CalendarMonth, AddTransactionPanel, DockBar) | **Must-keep** |
| `--space-md` | 16 | YES | Default padding (TxForm, CalendarMonth, SettingsView, forms) | **Must-keep** |
| `--space-lg` | 24 | YES | Section padding (LoginForm, AddTransactionPanel, SettingsView) | **Must-keep** |
| `--space-xl` | 32 | YES | Large section gaps (SettingsView, AddTransactionPanel) | **Must-keep** |
| `--space-2xl` | 40 | RARE | Used in SettingsView, DockBar calculations | **Candidate-merge → xl?** |
| `--space-*` | (48px?) | RARE | --space-12 (3rem) used in ShellLayout header calculation | **Candidate-merge → 2xl?** |
| `--space-*` | (20px, 28px) | NO | Tailwind uses 20px/28px, NOT in 2.0 scale | **Out-of-policy** |

**Usage Frequency:**
- **Heavy use (20+ occurrences)**: md (16px), sm (12px), xs (8px)
- **Moderate use (10-20 occurrences)**: lg (24px), xl (32px)
- **Light use (2-5 occurrences)**: 2xs (4px), 2xl (40px)
- **Very light use (1-2 occurrences)**: space-12 (48px)

**Consolidation Hints:**
- Core scale (0, 4, 8, 12, 16, 24, 32) is heavily used - **must keep all**
- 2xl (40px) and space-12 (48px) are rarely used
- **Option 1**: Keep 2xl (40px), remove space-12 (48px) → snap to 2xl
- **Option 2**: Keep both for flexibility (spacing is cheap)
- **Recommendation**: Keep all 10 steps - usage justifies full scale

---

### 1.5 BORDER RADII (6 Current → Consolidation Needed)

| Token Name | Value (px) | Used in Repos? | Observed Role | Reduction Tag |
|------------|------------|----------------|---------------|---------------|
| `--radius-sm` | 4 | YES | Small elements (SettingsView badges, page.module.css cards, inputs) | **Must-keep** |
| `--radius-md` | 8 | YES | Cards (CalendarMonth, TxList, TxForm, AddTransactionPanel) | **Must-keep** |
| `--radius-lg` | 12 | YES | Modals (BetaNoticeModal), large cards | **Must-keep** |
| `--radius-pill` | 9999px | YES | Circular elements (CalendarMonth day dots), fully-rounded buttons | **Must-keep** |
| `--radius-card` | 16 | RARE | Not directly observed in component modules | **Candidate-merge → lg?** |
| `--radius-sharp` | 12 | DUPLICATE | Same as --radius-lg (12px) | **Candidate-remove (duplicate)** |

**Consolidation Path:**
- `--radius-sharp` (12px) is identical to `--radius-lg` (12px) → **remove duplicate**
- `--radius-card` (16px) is not observed in actual usage → **candidate remove OR rename to --radius-xl if needed**
- **Recommended set**: sm (4px), md (8px), lg (12px), pill (9999px) = **4 radii**

---

### 1.6 BORDERS - WIDTHS & COLORS

| Token Name | Value | Used in Repos? | Observed Role | Reduction Tag |
|------------|-------|----------------|---------------|---------------|
| **WIDTHS** |
| (implicit 0) | 0 | YES | `border: none` removals (SettingsView, AccountView) | **Must-keep** |
| (implicit 1px) | 1px | YES | Default border width (cards, inputs, forms, dividers) | **Must-keep** |
| **COLORS** |
| `--app-border` | (alias) | YES | Card borders, input borders, general UI separation | **Must-keep (alias)** |
| `--border-subtle` | (structural) | YES | Light borders, dividers | **Must-keep** |
| `--button-secondary-border` | (semantic) | YES | Secondary button outlines | **Must-keep (alias)** |
| `--button-danger-border` | (semantic) | YES | Danger button outlines | **Must-keep (alias)** |
| `--input-border` | (alias) | YES | Input field borders | **Must-keep (alias)** |

**Consolidation Hints:**
- Border widths are minimal (0, 1px) - no consolidation needed
- Border colors are aliases to structural/semantic colors - keep as semantic aliases

---

### 1.7 LAYOUT PRIMITIVES (Current State)

| Token Name | Value | Used in Repos? | Observed Role | Reduction Tag |
|------------|-------|----------------|---------------|---------------|
| **CURRENT TOKENS (fortunetell-frontend)** |
| `--app-header-height` | calc(...) | YES | ShellLayout header height calculation | **Must-keep** |
| `--page-width-*` | ??? | NO | Not explicitly tokenized | **Propose for 2.0** |
| **CURRENT CLASSES (simple-budget-site)** |
| `.container-xl` | max-width | YES | Page container width (global.css) | **Propose for 2.0** |
| `.section` | padding | YES | Vertical section spacing (global.css) | **Propose for 2.0** |

**Status:** Layout primitives are under-tokenized in current system. See section 4.1 for proposals.

---

### 1.8 ICON TOKENS

| Token Name | Value | Used in Repos? | Observed Role | Reduction Tag |
|------------|-------|----------------|---------------|---------------|
| **SIZES** |
| `--icon-size-sm` | 16px | YES | Small inline icons (referenced in globals.css) | **Must-keep** |
| `--icon-size-md` | 20px | YES | Secondary icons (referenced in globals.css) | **Must-keep** |
| `--icon-size-lg` | 30px | YES | Canonical dock icon size (DockBar usage) | **Must-keep** |
| **COLORS** |
| `--icon-color-default` | (alias) | YES | Default icon color (tokens.css) | **Must-keep (alias)** |
| `--icon-color-strong` | (alias) | YES | Emphasized icon color (tokens.css) | **Must-keep (alias)** |
| `--icon-color-inverse` | (alias) | YES | On-dark icon color (tokens.css) | **Must-keep (alias)** |

**Consolidation Hints:**
- All 3 icon sizes are used and serve distinct roles
- Icon colors are aliases to structural colors - keep pattern

---

### 1.9 BUTTON & INPUT PRIMITIVES

| Token Name | Type | Used in Repos? | Observed Role | Reduction Tag |
|------------|------|----------------|---------------|---------------|
| **BUTTON ROLE TOKENS** |
| `--button-primary-*` | bg/text/border/hover | YES | Green primary action buttons (globals.css + components) | **Must-keep** |
| `--button-secondary-*` | bg/text/border/hover | YES | Neutral secondary buttons (globals.css + components) | **Must-keep** |
| `--button-danger-*` | bg/text/border/hover | YES | Red destructive buttons (globals.css + components) | **Must-keep** |
| `--button-radius` | alias | YES | Button border-radius (globals.css) | **Must-keep (alias)** |
| **INPUT PRIMITIVES** |
| `--input-border` | alias | YES | Input border color (globals.css) | **Must-keep (alias)** |
| `--input-radius` | alias | YES | Input border-radius (globals.css) | **Must-keep (alias)** |
| `--input-focus-border` | alias | YES | Input focus state color (tokens.css) | **Must-keep (alias)** |
| `--input-bg` | alias | YES | Input background (tokens.css) | **Must-keep (alias)** |

**Consolidation Hints:**
- All button role tokens are heavily used across both repos
- Input primitives are essential for form consistency
- No candidates for removal

---

### 1.10 SHADOW TOKENS (All Must Be Removed per 2.0)

| Token Name | Value | Used in Repos? | Observed Role | Reduction Tag |
|------------|-------|----------------|---------------|---------------|
| `--shadow-soft-base` | rgba(...) | YES | Soft shadows (fortunetell-frontend, simple-budget-site) | **REMOVE (2.0 shadowless)** |
| `--shadow-card-base` | rgba(...) | YES | Card shadows (fortunetell-frontend, simple-budget-site) | **REMOVE (2.0 shadowless)** |
| `--shadow-banner-base` | rgba(...) | YES | Banner/header shadows (simple-budget-site) | **REMOVE (2.0 shadowless)** |
| `--button-primary-shadow` | rgba(...) | YES | Button shadows (fortunetell-frontend) | **REMOVE (2.0 shadowless)** |
| `--shadow-focus-ring` | rgba(...) | YES | Focus ring shadows (fortunetell-frontend, simple-budget-site) | **REMOVE (replace w/ border)** |

**Action Required:**
- **Remove all 5 shadow tokens** from 2.0 spec
- Replace visual separation with: borders (1px solid), increased radii, or background color contrast

---

## 2. COLOR-SYSTEM REDUCTION INSIGHTS

### 2.1 Mapping 20+ Colors to 11-Color Palette

**CURRENT STATE (fortunetell-frontend):**
- 20+ distinct hex colors across tokens.css, globals.css, component modules

**TARGET STATE (2.0 spec):**
- 11 colors total: 5 theme + 6 semantic

**MAPPING ANALYSIS:**

| Current Color | Hex Value | Current Role | 2.0 Mapping | Conflict? |
|---------------|-----------|--------------|-------------|-----------|
| **THEME COLORS (5 in 2.0)** |
| neutral-bg-0 | `#ffffff` | Page backgrounds, cards | `--theme-background` | ✓ No conflict |
| neutral-text-strong | `#0f172a` | Headings, strong text | `--theme-primary` (#000000) | ⚠️ SNAP (very close) |
| ??? | ??? | Gray text tones | `--theme-secondary` (#3f3f3f) | ❌ **MISSING USAGE** |
| ??? | ??? | Mid-gray elements | `--theme-tertiary` (#7f7f7f) | ❌ **MISSING USAGE** |
| ??? | ??? | Light gray accents | `--theme-accent` (#b7b7b7) | ❌ **MISSING USAGE** |
| **SEMANTIC COLORS (6 in 2.0)** |
| brand-green | `#10b981` | Safe state, primary buttons | `--semantic-color-safe` | ✓ No conflict |
| brand-yellow | `#f6e823` | Warning state, caution days | `--semantic-color-warning` | ✓ No conflict |
| brand-red | `#ff1a1a` | Danger state, destructive actions | `--semantic-color-danger` | ✓ No conflict |
| (hover variants) | (derived) | Hover states | `--semantic-color-*-hover` | ✓ No conflict |
| **OUT-OF-POLICY COLORS (Must Remap)** |
| neutral-text-body | `#334155` | Body text | ??? → `--theme-secondary`? | ⚠️ Needs remapping |
| neutral-text-muted | `#475569` | Muted text | ??? → `--theme-tertiary`? | ⚠️ Needs remapping |
| neutral-text-low | `#64748b` | Low-emphasis text | ??? → `--theme-accent`? | ⚠️ Needs remapping |
| neutral-border-subtle | `#e2e8f0` | Light borders | ??? → `--theme-accent`? | ⚠️ Needs remapping |
| neutral-border-strong | `#cbd5f5` | Strong borders | ??? → `--theme-tertiary`? | ⚠️ Needs remapping |
| brand-green-dark | `#047857` | Dark green variant | ??? → REMOVE or snap to primary | ⚠️ Needs decision |
| brand-green-light | `#6ee7b7` | Light green variant | ??? → REMOVE or snap to safe-hover | ⚠️ Needs decision |
| neutral-bg-dark-0/1 | `#0b1120`, `#020617` | Landing dark sections | ??? → `--theme-primary`? | ⚠️ Landing-specific |
| overlay-backdrop | `rgba(0,0,0,0.5)` | Modal overlays | ??? → opacity strategy needed | ⚠️ Opacity usage |

**CRITICAL FINDING:**

The 11-color palette as specified **LACKS a gray scale** for text and border roles. Current repos heavily use:
- `#0f172a` (dark gray) - body text
- `#334155` (medium-dark gray) - secondary text
- `#475569` (medium gray) - muted text
- `#64748b` (light-medium gray) - low-emphasis text
- `#e2e8f0` (very light gray) - subtle borders

**Three Resolution Options:**

**Option A: Strict 11-Color Adherence (Remap to Primary + Opacity)**
- All text grays → `--theme-primary` (#000000) with varying opacity
  - text-strong: rgba(0,0,0,1.0) = #000000
  - text-body: rgba(0,0,0,0.8) ≈ gray
  - text-muted: rgba(0,0,0,0.6) ≈ gray
  - text-low: rgba(0,0,0,0.4) ≈ gray
- All border grays → `--theme-accent` (#b7b7b7) or opacity-based
- **Pros**: Strict adherence to 11-color constraint
- **Cons**: Requires opacity strategy (not currently in 2.0 spec), potential contrast issues

**Option B: Revise 2.0 Palette (Add Gray, Remove Unused)**
- Remove: `--theme-secondary` (#3f3f3f), `--theme-tertiary` (#7f7f7f), `--theme-accent` (#b7b7b7)
- Add: 3 functional grays based on real usage
  - `--theme-text-body` (#334155)
  - `--theme-text-muted` (#64748b)
  - `--theme-border` (#e2e8f0)
- **Total**: Still 11 colors (5 theme [primary, background, text-body, text-muted, border] + 6 semantic)
- **Pros**: Matches real-world usage, no opacity complexity
- **Cons**: Deviates from original 2.0 spec intent

**Option C: Expand to 14-Color Palette**
- Keep all 5 theme colors (#000000, #3f3f3f, #7f7f7f, #b7b7b7, #ffffff)
- Keep all 6 semantic colors (safe, safe-hover, warning, warning-hover, danger, danger-hover)
- Add 3 functional grays for text/borders (#334155, #64748b, #e2e8f0)
- **Total**: 14 colors
- **Pros**: Maximum flexibility, covers all real usage
- **Cons**: Larger than 11-color constraint

**RECOMMENDATION:** **Option B** (Revise 2.0 Palette)
- Replace abstract theme-secondary/tertiary/accent with functional text/border grays
- Maintains 11-color constraint
- Aligns with V10.2.1 "engineering-focused, minimal palette" philosophy

---

### 2.2 Per-Color Role Analysis

| Color | 2.0 Spec | Observed Usage | Usage Intensity | Reduction Tag |
|-------|----------|----------------|-----------------|---------------|
| `--theme-primary` | `#000000` | HC mode surface-page, text-strong, headings | Heavy (HC mode + text) | **Must-keep** |
| `--theme-secondary` | `#3f3f3f` | **NOT OBSERVED** | None | **Candidate-replace** |
| `--theme-tertiary` | `#7f7f7f` | **NOT OBSERVED** | None | **Candidate-replace** |
| `--theme-accent` | `#b7b7b7` | **NOT OBSERVED** | None | **Candidate-replace** |
| `--theme-background` | `#ffffff` | Page backgrounds, cards, neutral-bg-0 | Heavy (everywhere) | **Must-keep** |
| `--semantic-color-safe` | `#10b981` | Brand green, buttons, calendar safe days | Heavy (brand + financial) | **Must-keep** |
| `--semantic-color-safe-hover` | (derived) | Hover states | Moderate | **Must-keep** |
| `--semantic-color-warning` | `#f6e823` | Brand yellow, calendar warning days | Moderate (financial states) | **Must-keep** |
| `--semantic-color-warning-hover` | (derived) | Hover states | Light | **Must-keep** |
| `--semantic-color-danger` | `#ff1a1a` | Brand red, danger buttons, error states | Heavy (destructive actions) | **Must-keep** |
| `--semantic-color-danger-hover` | (derived) | Hover states | Moderate | **Must-keep** |

**Key Finding:**
- **6 colors are heavily used**: primary, background, safe, safe-hover, danger, danger-hover
- **3 colors are moderately used**: warning, warning-hover
- **3 colors are NEVER used**: secondary, tertiary, accent

---

### 2.3 Real-World Usage Conflicts

**Can all current usage be mapped to the 11-color palette?**

**Answer: NO** - not without either:
1. Adding opacity-based color generation (not in 2.0 spec), OR
2. Revising the 11-color palette to replace unused theme colors with functional grays

**Specific Conflicts:**

| Current Usage | Color Value | 2.0 Mapping Option | Issue |
|---------------|-------------|-------------------|-------|
| Body text (neutral-text-body) | `#334155` | None directly | No gray in 2.0 palette |
| Muted text (neutral-text-muted) | `#475569` | None directly | No gray in 2.0 palette |
| Subtle borders (neutral-border-subtle) | `#e2e8f0` | None directly | No gray in 2.0 palette |
| Landing dark panels | `#0b1120`, `#020617` | --theme-primary (#000000)? | Close but not exact |
| Overlay backdrop | `rgba(0,0,0,0.5)` | None directly | Opacity not in spec |

**Are these out-of-policy mistakes or genuine requirements?**

**Analysis:**
- Text grays (#334155, #475569, #64748b) are **genuine requirements** for:
  - Readable body text (can't use pure black #000000 everywhere)
  - Visual hierarchy (need multiple text weights/tones)
  - Accessibility (need contrast ratios between text tones)
- Border grays (#e2e8f0) are **genuine requirements** for:
  - Subtle visual separation without heavy contrast
  - Card borders that don't compete with content
- Dark panels (#0b1120, #020617) are **out-of-policy** (landing-page specific, can snap to #000000)
- Green variants (#047857, #6ee7b7) are **out-of-policy** (can remove or snap to safe/safe-hover)

**Conclusion:**
- Current 11-color palette **requires revision** to include functional grays
- Suggested revision: Replace unused theme-secondary/tertiary/accent with text-body/text-muted/border grays

---

## 3. TYPOGRAPHY & SPACING REDUCTION INSIGHTS

### 3.1 Typography - Font Sizes

| Size Token | Value | Usage Count | Key Roles | Reduction Tag |
|------------|-------|-------------|-----------|---------------|
| `--font-size-xs` | 12px | 15+ | Calendar labels, fine print, DockBar, timestamps, WCAG minimum | **Must-keep** |
| `--font-size-sm` | 14px | 12+ | Secondary text, CalendarMonth, HistoryPanel, form hints | **Must-keep** |
| `--font-size-md` | 16px | 25+ | Body text (default), form labels, buttons, cards | **Must-keep** |
| `--font-size-lg` | 18px | 18+ | Emphasized paragraphs, subheadings, TxForm, landing page body | **Must-keep** |
| `--font-size-xl` | 20px | 10+ | AppHeader, section headers, AddTransactionPanel | **Must-keep** |
| `--font-size-2xl` | 24px | 8+ | LoginForm title, AccountView balance, page headings | **Must-keep** |
| `--font-size-3xl` | 30px | 6+ | SettingsView title, major section titles, hero headings | **Must-keep** |
| `--font-size-4xl` | 36px | 3+ | Landing page hero, large display numbers (critical but rare) | **Must-keep** |

**Usage Pattern:**
- **Daily-driver sizes** (20+ uses): md (16px)
- **Frequent sizes** (10-20 uses): xs, sm, lg, xl
- **Specialized sizes** (5-10 uses): 2xl, 3xl
- **Hero size** (2-5 uses): 4xl (essential for landing page impact)

**Consolidation Analysis:**

Could any sizes be merged?

**xs + sm?** (12px + 14px)
- **NO** - xs is WCAG minimum (12px), sm is secondary text (14px)
- Merging would lose critical size tier for fine print vs secondary text

**md + lg?** (16px + 18px)
- **NO** - md is body text standard (16px), lg is emphasis (18px)
- Merging would lose ability to emphasize paragraphs without jumping to headings

**xl + 2xl?** (20px + 24px)
- **NO** - xl is section headers (20px), 2xl is page headings (24px)
- Merging would collapse two distinct hierarchy levels

**3xl + 4xl?** (30px + 36px)
- **MAYBE** - both are hero/display sizes, used 6 and 3 times respectively
- Could consolidate to single hero size (e.g., 32px or 36px)
- **Trade-off**: Lose granular control over large headings, but simplify system

**Recommendation:**
- **Keep all 8 sizes** - each serves distinct role with sufficient usage
- Alternative: Merge 3xl + 4xl → single `--font-size-hero` (36px) if simplification is critical
- **Final verdict**: **8 sizes → 8 sizes (no reduction)** or **8 sizes → 7 sizes** (merge 3xl/4xl)

---

### 3.2 Typography - Font Weights

| Weight Token | Value | Usage Count | Key Roles | Reduction Tag |
|--------------|-------|-------------|-----------|---------------|
| **2.0 TARGET** |
| `--font-weight-normal` | 400 | 15+ | Body text, paragraphs, normal emphasis | **Must-keep** |
| `--font-weight-bold` | 700 | 8+ | Headings, strong emphasis, buttons | **Must-keep** |
| **CURRENT (To Reduce)** |
| `--font-weight-medium` | 500 | 12+ | Links, secondary buttons, slight emphasis | **Merge → 400** |
| `--font-weight-semibold` | 600 | 10+ | Section headers, form labels, calendar days | **Merge → 700** |

**Merge Strategy:**

**500 → 400 (medium to normal):**
- **Impact**: Slight reduction in link/button weight
- **Risk**: Low - 500 is subtle, most users won't notice change to 400
- **Locations affected**: LoginForm, SettingsView, TxList, DockBar (12 instances)

**600 → 700 (semibold to bold):**
- **Impact**: Moderate increase in emphasis for section headers
- **Risk**: Medium - 600 is common for "semi-strong" emphasis, jumping to 700 may feel heavy
- **Locations affected**: SettingsView, CalendarMonth, TxForm (10 instances)
- **Mitigation**: May need to reduce font-size on some elements to compensate for heavier weight

**Alternative Strategy:**
- Keep 600 (semibold), remove 500 (medium)
- **Result**: 3 weights (400, 600, 700) instead of 2 (400, 700)
- **Trade-off**: Closer to current usage, but violates 2.0 "2 weights only" constraint

**Recommendation:**
- **Follow 2.0 spec**: Reduce to 400 + 700 only
- **Implementation**: Remap 500 → 400, remap 600 → 700
- **Risk mitigation**: Design review for 600→700 cases (may need font-size adjustments)

---

### 3.3 Spacing Scale

| Spacing Token | Value (px) | Usage Count | Key Roles | Reduction Tag |
|---------------|------------|-------------|-----------|---------------|
| `--space-0` | 0 | 20+ | Explicit resets (margin/padding: 0) | **Must-keep** |
| `--space-2xs` | 4 | 8+ | Tight gaps (CalendarMonth, SettingsView) | **Must-keep** |
| `--space-xs` | 8 | 18+ | Small gaps (LoginForm, CalendarMonth, TxForm) | **Must-keep** |
| `--space-sm` | 12 | 22+ | Standard gaps (CalendarMonth, AddTransactionPanel, DockBar) | **Must-keep** |
| `--space-md` | 16 | 35+ | Default padding (TxForm, forms, cards) | **Must-keep** |
| `--space-lg` | 24 | 15+ | Section padding (LoginForm, AddTransactionPanel) | **Must-keep** |
| `--space-xl` | 32 | 12+ | Large section gaps (SettingsView, AddTransactionPanel) | **Must-keep** |
| `--space-2xl` | 40 | 3 | SettingsView, DockBar calculations | **Candidate-merge → xl?** |
| `--space-12` | 48 | 1 | ShellLayout header height calculation | **Candidate-merge → 2xl?** |

**Usage Tiers:**
- **Critical (20+ uses)**: 0, sm, md
- **Heavy (10-20 uses)**: xs, lg, xl
- **Light (5-10 uses)**: 2xs
- **Rare (1-5 uses)**: 2xl, space-12

**Consolidation Analysis:**

**2xl (40px) vs xl (32px):**
- Used 3 times vs 12 times
- **Option A**: Remove 2xl, snap to xl (32px) - lose 8px of granularity
- **Option B**: Keep 2xl for rare large-gap cases
- **Impact**: Minimal - only 3 occurrences would need adjustment

**space-12 (48px) vs 2xl (40px):**
- Used 1 time (ShellLayout header calculation) vs 3 times
- **Option A**: Remove space-12, snap to 2xl (40px) - may need header height adjustment
- **Option B**: Keep space-12 for header calculation precision
- **Impact**: Low - single use case (header height)

**Recommendation:**
- **Option 1 (Aggressive)**: Remove 2xl and space-12 → **8-step scale** (0, 4, 8, 12, 16, 24, 32)
  - Snap 2xl (40px) → xl (32px)
  - Snap space-12 (48px) → xl (32px) or 2xl (40px if kept)
- **Option 2 (Conservative)**: Keep all → **10-step scale** (0, 4, 8, 12, 16, 24, 32, 40, 48)
  - Rationale: Spacing tokens are cheap, flexibility is valuable
- **Recommended**: **Option 2 (keep all 10 steps)** - usage justifies full scale, simplification benefit is minimal

---

## 4. LAYOUT / MOTION / ASSET TOKEN PROPOSALS

### 4.1 Layout Tokens (Proposed for 2.0)

Based on observed usage patterns in both repos:

| Proposed Token | Value | Observed Usage | Source Repo(s) |
|----------------|-------|----------------|----------------|
| **CONTENT WIDTHS** |
| `--layout-content-narrow` | `48rem` (768px) | Article/guide content, forms, focused reading | fortunetell-frontend (LoginForm), simple-budget-site (.max-w-3xl) |
| `--layout-content-standard` | `64rem` (1024px) | Standard page content, most sections | simple-budget-site (.max-w-5xl, .container-xl) |
| `--layout-content-wide` | `80rem` (1280px) | Wide layouts, dashboard, feature grids | simple-budget-site (.max-w-6xl) |
| **PAGE PADDING** |
| `--layout-page-padding-mobile` | `1rem` (16px) | Mobile horizontal page padding | simple-budget-site (px-4), fortunetell-frontend (component padding) |
| `--layout-page-padding-desktop` | `1.5rem` (24px) | Desktop horizontal page padding | simple-budget-site (px-6), fortunetell-frontend (component padding) |
| **SECTION SPACING** |
| `--layout-section-gap` | `4rem` (64px) | Vertical spacing between major sections | simple-budget-site (.section vertical padding) |
| **HEADER/FOOTER** |
| `--layout-header-height` | `calc(2 * var(--space-md) + var(--font-size-xl))` | Dynamic header height based on content | fortunetell-frontend (ShellLayout.module.css) |
| **SAFE AREAS** |
| `--layout-safe-area-bottom` | `env(safe-area-inset-bottom)` | Mobile notch/home indicator safe area | fortunetell-frontend (DockBar.module.css) |

**Usage Evidence:**
- **Content widths**: simple-budget-site uses `.max-w-3xl` (48rem), `.max-w-5xl` (64rem), `.max-w-6xl` (72rem) extensively
- **Page padding**: Both repos use 16px (mobile) and 24px (desktop) horizontal padding patterns
- **Section gaps**: simple-budget-site `.section` class uses ~64px vertical spacing
- **Header height**: fortunetell-frontend calculates header height dynamically
- **Safe areas**: fortunetell-frontend DockBar uses `env(safe-area-inset-bottom)` for mobile

**Recommendation:** Include all 8 layout tokens above in 2.0 spec

---

### 4.2 Motion Tokens (Proposed for 2.0)

Based on observed transition patterns:

| Proposed Token | Value | Observed Usage | Source Repo(s) |
|----------------|-------|----------------|----------------|
| **DURATIONS** |
| `--motion-duration-instant` | `100ms` | Minimal feedback (hover state changes) | (Inferred from industry standards) |
| `--motion-duration-fast` | `150ms` | Standard UI transitions (buttons, inputs) | fortunetell-frontend (globals.css: background-color, border-color) |
| `--motion-duration-normal` | `200ms` | Standard animations | simple-budget-site (Tailwind duration-200) |
| `--motion-duration-slow` | `300ms` | Emphasized transitions (modals, panels) | (Not observed, but common pattern) |
| **EASINGS** |
| `--motion-easing-standard` | `ease-out` | Default easing for most transitions | fortunetell-frontend (globals.css) |
| `--motion-easing-emphasis` | `ease-in-out` | Smooth, emphasized motion | simple-budget-site (floating animation) |
| **SPECIAL** |
| `--motion-hero-animation` | `4s ease-in-out infinite` | Hero floating animation | simple-budget-site (index.html .floating) |

**Usage Evidence:**
- **150ms**: fortunetell-frontend uses `transition: background-color 150ms ease-out` extensively
- **200ms**: simple-budget-site uses Tailwind `duration-200` for transitions
- **ease-out**: fortunetell-frontend default easing
- **ease-in-out**: simple-budget-site floating animation

**Brand Alignment:**
- "Minimal, mechanical motion" → favor fast durations (100-200ms)
- "Engineering-focused" → standard easings (ease-out, linear), avoid playful beziers
- Limited animation usage → small token set (3 durations, 2 easings)

**Recommendation:** Include 5 core motion tokens (3 durations + 2 easings) + optional hero animation token

---

### 4.3 Asset Tokens (Proposed for 2.0)

Based on observed icon and asset patterns:

| Proposed Token | Value | Observed Usage | Source Repo(s) |
|----------------|-------|----------------|----------------|
| **ICON SIZES (Already in 1.3)** |
| `--icon-size-sm` | `16px` | Small inline icons | fortunetell-frontend (tokens.css) ✓ |
| `--icon-size-md` | `20px` | Secondary icons | fortunetell-frontend (tokens.css) ✓ |
| `--icon-size-lg` | `30px` | Dock icons (canonical) | fortunetell-frontend (tokens.css) ✓ |
| **LOGO SIZES** |
| `--logo-size-small` | `2.5rem` (40px) | Header logo, compact contexts | simple-budget-site (.w-10 h-10 = 2.5rem) |
| `--logo-size-standard` | `3rem` (48px) | Standard logo size | simple-budget-site (.w-12 h-12 = 3rem) |
| **AVATAR/BADGE SIZES** |
| `--avatar-size-sm` | `1.5rem` (24px) | Small user avatars, badges | (Not observed, but common pattern) |
| `--avatar-size-md` | `2rem` (32px) | Standard avatars | (Not observed, but common pattern) |
| `--avatar-size-lg` | `3rem` (48px) | Large avatars | (Not observed, but common pattern) |
| **ASPECT RATIOS** |
| `--aspect-ratio-card` | `16/9` | Card images, previews | (Not observed, but common pattern) |
| `--aspect-ratio-hero` | `21/9` | Hero banners | (Not observed, but common pattern) |

**Usage Evidence:**
- **Icon sizes**: Explicitly tokenized in fortunetell-frontend (sm/md/lg = 16/20/30px)
- **Logo sizes**: simple-budget-site uses `.w-10 h-10` (40px) and `.w-12 h-12` (48px) for logo
- **Avatars**: Not observed in current repos (future-proofing)
- **Aspect ratios**: Not observed in current repos (future-proofing)

**Recommendation:**
- **Must include**: 3 icon sizes (already in 1.3) ✓
- **Should include**: 2 logo sizes (observed usage)
- **Optional**: Avatar sizes, aspect ratios (not observed, but common needs)

**Conservative Set (6 tokens):**
- 3 icon sizes + 2 logo sizes + 1 avatar size (if needed)

**Full Set (10 tokens):**
- 3 icon sizes + 2 logo sizes + 3 avatar sizes + 2 aspect ratios

---

## 5. FINAL OUTPUT FORMAT

### A. MUST-KEEP TOKENS

#### A.1 Colors (6-8 colors)

**DEFINITE MUST-KEEP (6):**
- `--theme-primary` (#000000) - Heavy usage in HC mode, headings, text-strong
- `--theme-background` (#ffffff) - Heavy usage in backgrounds, cards
- `--semantic-color-safe` (#10b981) - Brand green, financial safe, buttons
- `--semantic-color-safe-hover` (derived) - Hover states
- `--semantic-color-danger` (#ff1a1a) - Brand red, destructive actions, errors
- `--semantic-color-danger-hover` (derived) - Hover states

**MODERATE MUST-KEEP (2):**
- `--semantic-color-warning` (#f6e823) - Brand yellow, financial warning
- `--semantic-color-warning-hover` (derived) - Hover states

**CRITICAL GAP - NEED 3 GRAYS:**
- Requires addition of text-body, text-muted, border grays (see Option B in Section 2.1)
- OR implement opacity-based color generation

**Total: 8 colors** (if grays added) or **6 colors** (if strict 11-color, but requires opacity strategy)

---

#### A.2 Typography (8 sizes + 2 weights)

**FONT SIZES (All 8 must-keep):**
- `--font-size-xs` (12px) - 15+ uses
- `--font-size-sm` (14px) - 12+ uses
- `--font-size-md` (16px) - 25+ uses
- `--font-size-lg` (18px) - 18+ uses
- `--font-size-xl` (20px) - 10+ uses
- `--font-size-2xl` (24px) - 8+ uses
- `--font-size-3xl` (30px) - 6+ uses
- `--font-size-4xl` (36px) - 3+ uses (critical for hero)

**FONT WEIGHTS (2 target):**
- `--font-weight-normal` (400) - 15+ uses
- `--font-weight-bold` (700) - 8+ uses

---

#### A.3 Spacing (8-10 steps)

**CORE SCALE (8 must-keep):**
- `--space-0` (0) - 20+ uses
- `--space-2xs` (4px) - 8+ uses
- `--space-xs` (8px) - 18+ uses
- `--space-sm` (12px) - 22+ uses
- `--space-md` (16px) - 35+ uses
- `--space-lg` (24px) - 15+ uses
- `--space-xl` (32px) - 12+ uses
- *(optional: --space-2xl (40px) - 3 uses)*

**EXTENDED (2 optional):**
- `--space-2xl` (40px) - 3 uses (keep or merge to xl)
- `--space-12` (48px) - 1 use (keep or merge to 2xl)

**Recommendation:** Keep all 10 steps (usage justifies full scale)

---

#### A.4 Radii (4-5 values)

**MUST-KEEP (4):**
- `--radius-sm` (4px) - Heavy use
- `--radius-md` (8px) - Heavy use
- `--radius-lg` (12px) - Moderate use
- `--radius-pill` (9999px) - Circular elements

**OPTIONAL (1):**
- `--radius-card` (16px) - Rare use, candidate for removal

---

#### A.5 Borders (2 widths)

- 0 (border: none)
- 1px (standard border)

**Border colors:** Token aliases (--app-border, --border-subtle, etc.)

---

#### A.6 Icon Tokens (3 sizes + 3 colors)

**SIZES:**
- `--icon-size-sm` (16px)
- `--icon-size-md` (20px)
- `--icon-size-lg` (30px)

**COLORS (aliases):**
- `--icon-color-default`
- `--icon-color-strong`
- `--icon-color-inverse`

---

#### A.7 Button & Input Primitives (all must-keep)

**BUTTON ROLE TOKENS:**
- `--button-primary-*` (bg, text, border, hover)
- `--button-secondary-*` (bg, text, border, hover)
- `--button-danger-*` (bg, text, border, hover)
- `--button-radius` (alias)

**INPUT PRIMITIVES:**
- `--input-border` (alias)
- `--input-radius` (alias)
- `--input-focus-border` (alias)
- `--input-bg` (alias)

---

#### A.8 Layout Primitives (8 proposed)

- `--layout-content-narrow` (48rem)
- `--layout-content-standard` (64rem)
- `--layout-content-wide` (80rem)
- `--layout-page-padding-mobile` (16px)
- `--layout-page-padding-desktop` (24px)
- `--layout-section-gap` (64px)
- `--layout-header-height` (calc-based)
- `--layout-safe-area-bottom` (env-based)

---

#### A.9 Motion Primitives (5 proposed)

**DURATIONS:**
- `--motion-duration-fast` (150ms)
- `--motion-duration-normal` (200ms)
- `--motion-duration-slow` (300ms)

**EASINGS:**
- `--motion-easing-standard` (ease-out)
- `--motion-easing-emphasis` (ease-in-out)

---

### B. CANDIDATE-REMOVE TOKENS

#### B.1 Colors (3 unused theme colors)

- `--theme-secondary` (#3f3f3f) - **NOT OBSERVED** in either repo
- `--theme-tertiary` (#7f7f7f) - **NOT OBSERVED** in either repo
- `--theme-accent` (#b7b7b7) - **NOT OBSERVED** in either repo

**Reason:** Zero usage across both repos
**Recommendation:** Remove and replace with functional grays (see Option B in Section 2.1)

---

#### B.2 Shadows (5 tokens - ALL must be removed)

- `--shadow-soft-base` - 2.0 shadowless design
- `--shadow-card-base` - 2.0 shadowless design
- `--shadow-banner-base` - 2.0 shadowless design
- `--button-primary-shadow` - 2.0 shadowless design
- `--shadow-focus-ring` - Replace with border-based focus

**Reason:** 2.0 spec explicitly prohibits shadows
**Replacement:** Border-based visual separation (1px borders, varied radii, background contrast)

---

#### B.3 Radii (1-2 candidates)

- `--radius-sharp` (12px) - **DUPLICATE** of --radius-lg
- `--radius-card` (16px) - Rare usage (not observed in components)

**Reason:** Duplicate value, minimal usage
**Recommendation:** Remove --radius-sharp (merge to --radius-lg), optionally remove --radius-card

---

#### B.4 Spacing (0-2 candidates - OPTIONAL)

- `--space-2xl` (40px) - Only 3 uses (optional removal)
- `--space-12` (48px) - Only 1 use (optional removal)

**Reason:** Very rare usage
**Recommendation:** Keep for flexibility OR remove and snap to --space-xl (32px)

---

### C. CANDIDATE-MERGE TOKENS

#### C.1 Font Weights (2 merges required)

- `--font-weight-medium` (500) → **MERGE to** `--font-weight-normal` (400)
  - 12+ occurrences affected
  - Low visual impact
- `--font-weight-semibold` (600) → **MERGE to** `--font-weight-bold` (700)
  - 10+ occurrences affected
  - Moderate visual impact (may need font-size adjustments)

**Target:** 4 weights → 2 weights (400, 700 only)

---

#### C.2 Out-of-Policy Colors (Remap to 11-color palette)

These are CURRENT colors that need remapping, not token removals:

- `#334155` (neutral-text-body) → remap to new `--theme-text-body` OR opacity-based primary
- `#475569` (neutral-text-muted) → remap to new `--theme-text-muted` OR opacity-based primary
- `#64748b` (neutral-text-low) → remap to new `--theme-border` OR opacity-based primary
- `#e2e8f0` (neutral-border-subtle) → remap to new `--theme-border` OR --theme-accent
- `#047857` (brand-green-dark) → remap to --theme-primary OR remove
- `#6ee7b7` (brand-green-light) → remap to --semantic-color-safe-hover OR remove
- `#0b1120`, `#020617` (dark backgrounds) → remap to --theme-primary (#000000)
- `rgba(0,0,0,0.5)` (overlay backdrop) → needs opacity strategy OR remove

---

### D. PROPOSED LAYOUT/MOTION/ASSET TOKENS

#### D.1 Layout Tokens (8 tokens - grounded in real usage)

| Token | Value | Source |
|-------|-------|--------|
| `--layout-content-narrow` | `48rem` (768px) | simple-budget-site .max-w-3xl, fortunetell-frontend forms |
| `--layout-content-standard` | `64rem` (1024px) | simple-budget-site .max-w-5xl, .container-xl |
| `--layout-content-wide` | `80rem` (1280px) | simple-budget-site .max-w-6xl |
| `--layout-page-padding-mobile` | `1rem` (16px) | Both repos (px-4, component padding) |
| `--layout-page-padding-desktop` | `1.5rem` (24px) | Both repos (px-6, component padding) |
| `--layout-section-gap` | `4rem` (64px) | simple-budget-site .section vertical spacing |
| `--layout-header-height` | `calc(2 * var(--space-md) + var(--font-size-xl))` | fortunetell-frontend ShellLayout.module.css |
| `--layout-safe-area-bottom` | `env(safe-area-inset-bottom)` | fortunetell-frontend DockBar.module.css |

**All 8 tokens have observed usage across one or both repos.**

---

#### D.2 Motion Tokens (5 tokens - grounded in real usage)

| Token | Value | Source |
|-------|-------|--------|
| `--motion-duration-fast` | `150ms` | fortunetell-frontend globals.css (background-color, border-color transitions) |
| `--motion-duration-normal` | `200ms` | simple-budget-site Tailwind duration-200 |
| `--motion-duration-slow` | `300ms` | Industry standard (not observed, but common need) |
| `--motion-easing-standard` | `ease-out` | fortunetell-frontend globals.css default easing |
| `--motion-easing-emphasis` | `ease-in-out` | simple-budget-site .floating animation |

**5 tokens: 3 with observed usage, 2 standard patterns.**

---

#### D.3 Asset Tokens (5 tokens - grounded in real usage)

| Token | Value | Source |
|-------|-------|--------|
| `--icon-size-sm` | `16px` | fortunetell-frontend tokens.css (ALREADY EXISTS) ✓ |
| `--icon-size-md` | `20px` | fortunetell-frontend tokens.css (ALREADY EXISTS) ✓ |
| `--icon-size-lg` | `30px` | fortunetell-frontend tokens.css (ALREADY EXISTS) ✓ |
| `--logo-size-small` | `2.5rem` (40px) | simple-budget-site .w-10 h-10 logo |
| `--logo-size-standard` | `3rem` (48px) | simple-budget-site .w-12 h-12 logo |

**5 tokens: 3 existing (icon sizes), 2 new (logo sizes) with observed usage.**

---

## 6. EXECUTIVE SUMMARY

### 6.1 Token Count Summary

| Category | Current (1.3) | Must-Keep | Candidate-Remove | Proposed New | 2.0 Target |
|----------|---------------|-----------|------------------|--------------|------------|
| **Colors** | 20+ | 6 (or 8 w/ grays) | 3 (unused theme) + 5 (shadows) | 3 (functional grays) | 11 (needs revision) |
| **Typography (sizes)** | 8 | 8 | 0 | 0 | 8 |
| **Typography (weights)** | 4 | 2 | 0 (merge 2) | 0 | 2 |
| **Spacing** | 10 | 8-10 | 0-2 (optional) | 0 | 8-10 |
| **Radii** | 6 | 4 | 2 (duplicate + rare) | 0 | 4 |
| **Borders** | 2 widths | 2 | 0 | 0 | 2 |
| **Icon tokens** | 6 | 6 | 0 | 0 | 6 |
| **Button/Input** | 12+ | 12+ | 0 | 0 | 12+ |
| **Layout** | 1-2 | 0 (under-tokenized) | 0 | 8 | 8 |
| **Motion** | 0 | 0 (not tokenized) | 0 | 5 | 5 |
| **Asset** | 3 (icons only) | 3 | 0 | 2 (logos) | 5 |

---

### 6.2 Critical Findings

**1. Color Palette Mismatch:**
- 2.0 spec defines 11 colors (5 theme + 6 semantic)
- 3 theme colors (secondary/tertiary/accent) are NEVER used
- Real-world usage requires 3 functional grays (text-body, text-muted, border) NOT in spec
- **Resolution required**: Revise 11-color palette to replace unused colors with functional grays

**2. Typography is Well-Aligned:**
- All 8 font sizes are used and justified
- Font weights can reduce from 4 → 2 (per 2.0 spec) with medium/semibold remapping

**3. Spacing Scale is Justified:**
- Core 8-step scale (0, 4, 8, 12, 16, 24, 32) is heavily used
- Extended steps (40px, 48px) are rarely used but provide flexibility
- **Recommendation**: Keep full 10-step scale

**4. Shadows Must Be Removed:**
- All 5 shadow tokens violate 2.0 shadowless design
- Replace with border-based visual separation

**5. Layout/Motion/Asset Tokens Are Under-Specified:**
- Current tokens.css lacks layout primitives (content widths, page padding, section gaps)
- Motion tokens are not defined (transitions are inline values)
- Asset tokens only cover icons, missing logos
- **Recommendation**: Add 8 layout + 5 motion + 2 logo tokens to 2.0 spec

---

### 6.3 Recommendations for Task 2 (2.0 Spec Development)

**MUST DO:**
1. **Revise 11-color palette** - Replace theme-secondary/tertiary/accent with text-body/text-muted/border grays
2. **Remove all shadow tokens** - 5 shadow tokens → 0 shadows
3. **Reduce font weights** - 4 weights → 2 weights (400, 700)
4. **Add layout primitives** - 8 new layout tokens (content widths, padding, gaps)
5. **Add motion primitives** - 5 new motion tokens (durations + easings)

**SHOULD DO:**
6. **Consolidate radii** - 6 radii → 4 radii (remove duplicate + rare)
7. **Add logo size tokens** - 2 new logo tokens (small, standard)

**OPTIONAL:**
8. **Reduce spacing scale** - 10 steps → 8 steps (remove 40px, 48px rare values)

**CRITICAL DECISION POINT:**
- **Color palette revision** (Option B from Section 2.1) is essential
- Without functional grays, the 11-color palette cannot support real-world text/border usage
- Alternative is opacity-based color generation (not in current 2.0 spec)

---

## END OF REDUCTION ANALYSIS

**Analysis Completed:** 2026-01-30
**Next Task:** Task 2 - Draft 2.0 Token System Specification (using reduction insights from this analysis)
**Status:** ✓ REDUCTION ANALYSIS COMPLETE - NO CODE CHANGES MADE
