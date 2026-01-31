# Task 2: Typography Analysis & 6-Role System Definition
## h1–h6 Typography Roles for 2.0 (Spec/Plan Only — No Code Changes)

**Date:** 2026-01-30
**Scope:** Analyze current typography usage, define 6-role system (h1-h6), identify violations of 16px minimum
**Authority:** DEV_DesignTokensSystem_1.4 + V10.2.1 Architecture + Typography Constraints
**Hard Constraints:**
- Exactly 6 roles: h1, h2, h3, h4, h5, h6 (NO other named sizes)
- Normal body text = 18px (h5)
- Smallest text on page = 16px (h6)
- **NO text below 16px under any circumstance**
- h1 = largest; h5 = body size; h6 = smallest

---

## STEP 1: CURRENT TYPOGRAPHY SNAPSHOT

### 1.1 Font Sizes in Use (fortunetell-frontend + simple-budget-site)

| Size (px) | Size (rem) | 1.4 Token Name | Status vs 16px Floor | Usage Count | Where Used |
|-----------|-----------|----------------|---------------------|-------------|------------|
| **12px** | 0.75rem | `--font-size-xs` | **VIOLATION** (-4px) | 15+ occurrences | CalendarMonth day labels, DockBar labels, HistoryPanel timestamps, TxList timestamps |
| **14px** | 0.875rem | `--font-size-sm` | **VIOLATION** (-2px) | 12+ occurrences | CalendarMonth secondary text, HistoryPanel meta info, small UI labels |
| **16px** | 1rem | `--font-size-md` | ✓ COMPLIANT (floor) | 25+ occurrences | SettingsView, TxForm, DockBar, default body text, simple-budget-site base |
| **18px** | 1.125rem | `--font-size-lg` | ✓ COMPLIANT (body) | 18+ occurrences | HistoryPanel, TxForm labels, simple-budget-site paragraphs |
| **20px** | 1.25rem | `--font-size-xl` | ✓ COMPLIANT | 10+ occurrences | AppHeader titles, AddTransactionPanel headings, simple-budget-site h3 |
| **24px** | 1.5rem | `--font-size-2xl` | ✓ COMPLIANT | 8+ occurrences | LoginForm title, AccountView balance display, page.module.css headings |
| **30px** | 1.875rem | `--font-size-3xl` | ✓ COMPLIANT | 6+ occurrences | SettingsView title, page.module.css hero, simple-budget-site h2 |
| **36px** | 3rem | `--font-size-4xl` | ✓ COMPLIANT | 3+ occurrences | page.module.css hero headings, simple-budget-site h1 (base, responsive up to 48px/60px) |

**Violations Summary:**
- **2 sizes below 16px floor:** 12px (xs) and 14px (sm)
- **27+ violation instances** across components
- **Primary violators:** CalendarMonth (day labels, timestamps), DockBar, HistoryPanel, TxList

### 1.2 Font Weights in Use

| Weight | 1.4 Status | Usage Count | Where Used |
|--------|-----------|-------------|------------|
| **400** (normal) | ✓ COMPLIANT | 15+ occurrences | page.module.css body, AppHeader, AccountView body text, simple-budget-site body/paragraphs |
| **500** (medium) | **OUT-OF-POLICY** | 8+ occurrences | simple-budget-site links, global.css .high-contrast, page.module.css emphasis |
| **600** (semibold) | **OUT-OF-POLICY** | 6+ occurrences | simple-budget-site h4, buttons, CalendarMonth emphasis, SettingsView labels |
| **700** (bold) | ✓ COMPLIANT | 8+ occurrences | LoginForm, CalendarMonth, TxList headings, simple-budget-site h1-h3 |

**Note:** 1.4 spec allows only **400** and **700**. Weights 500/600 must be eliminated in 2.0.

### 1.3 Current Typography Contexts (Where Text Appears)

| Context | Current Size(s) | Current Weight(s) | Repo/Component |
|---------|----------------|-------------------|----------------|
| **Page hero titles** | 36px-60px (responsive) | 700 | simple-budget-site h1, page.module.css hero |
| **Section headers** | 24px-36px | 700 | simple-budget-site h2, page.module.css section titles |
| **Card/panel titles** | 18px-24px | 600-700 | AccountView, HistoryPanel, AddTransactionPanel headers |
| **Body paragraphs** | 16px-20px | 400 | All components, simple-budget-site p |
| **Form labels** | 16px-18px | 500-600 | TxForm, LoginForm, SettingsView labels |
| **Helper text / hints** | **12px-14px** | 400-500 | CalendarMonth, HistoryPanel meta, TxList timestamps |
| **Button text** | 16px | 600 | globals.css buttons, simple-budget-site .btn |
| **Navigation / dock items** | **12px-16px** | 400-600 | DockBar labels, AppHeader items |
| **Small labels / overlines** | **12px** | 500-700 | CalendarMonth day-of-week, timestamps |
| **Timestamps / metadata** | **12px-14px** | 400 | HistoryPanel, TxList, AccountView dates |

**Violation Contexts:** Helper text, navigation labels, timestamps, small labels (all currently <16px)

---

## STEP 2: DEFINE THE 6 TYPOGRAPHY ROLES (h1–h6)

### 2.1 The 6-Role System

| Role | Font Size | Rem | Weight | Line Height | Primary Use Cases |
|------|-----------|-----|--------|-------------|-------------------|
| **h1** | 36px | 3rem | 700 (bold) | 1.2 | Page hero titles, landing page main headings, top-level marketing content |
| **h2** | 30px | 1.875rem | 700 (bold) | 1.25 | Section headers, major subsection titles, feature headings |
| **h3** | 24px | 1.5rem | 700 (bold) | 1.3 | Card titles, panel headers, subsection titles within sections |
| **h4** | 20px | 1.25rem | 700 (bold) | 1.4 | Small card titles, emphasized labels, inline section headers |
| **h5** | 18px | 1.125rem | 400 (normal) | 1.6 | **BODY TEXT** (default paragraph size), form labels, standard UI text |
| **h6** | 16px | 1rem | 400 (normal) | 1.5 | **SMALLEST ALLOWED** - helper text, metadata, timestamps, small labels, button text |

**Key Principles:**
1. **h1-h4 use weight 700** for hierarchy and emphasis (headings are bold)
2. **h5-h6 use weight 400** for readability (body text is normal weight)
3. **h5 (18px) is the default body text size** - most readable for sustained reading
4. **h6 (16px) is the absolute floor** - used sparingly for secondary/meta content
5. **No role uses weight 500/600** (eliminates out-of-policy weights)
6. **Line heights** are generous for readability (1.5-1.6 for body, tighter for headings)

### 2.2 Role Assignment Rules

#### When to Use Each Role:

**h1 (36px, bold):**
- Landing page hero title
- Marketing page main headline
- App launch screen title (if any)
- **Never used in app UI components**

**h2 (30px, bold):**
- Landing page section titles ("Features", "Pricing", "How It Works")
- Marketing content major sections
- **Rarely used in app UI** (possibly settings page title)

**h3 (24px, bold):**
- Modal/dialog titles
- Full-page view titles (LoginForm title, SettingsView title)
- Panel/drawer main headers (AddTransactionPanel header)
- Major data display labels (AccountView balance label)

**h4 (20px, bold):**
- Card headers within panels
- Section headers within complex views (HistoryPanel section headers)
- Form section titles
- Emphasized inline labels

**h5 (18px, normal) — BODY TEXT:**
- Default paragraph text
- Form input labels
- List item primary text
- Navigation labels (if not h6)
- Standard UI text throughout app
- **This is the workhorse size**

**h6 (16px, normal) — FLOOR:**
- Timestamps / metadata ("2 hours ago", "Jan 15, 2026")
- Helper text / hints ("Optional", "Leave blank for...", placeholders)
- Secondary labels (day-of-week in calendar, small overlines)
- Button text (small buttons)
- Footer text
- **Use sparingly** - only when h5 feels too large

**What NOT to do:**
- ❌ No text below h6 (16px) under any circumstance
- ❌ No intermediate sizes between roles (no "17px for this one special case")
- ❌ No using h1-h4 with weight 400 (headings must be bold for hierarchy)
- ❌ No using h5-h6 with weight 700 (body text must be normal weight for readability)

---

## STEP 3: MAP CURRENT USAGE TO 6 ROLES

### 3.1 Context → 2.0 Role Mapping

| Current Context | Current Size | 2.0 Role | Visual Change |
|----------------|--------------|----------|---------------|
| **Page hero titles** | 36px-60px (responsive) | **h1** (36px base) | May need responsive scaling preserved (sm:h1-lg, md:h1-xl) as CSS utility, not token |
| **Section headers (landing)** | 30px-36px | **h2** (30px) | Slight reduction on some headers (acceptable) |
| **Modal/dialog titles** | 24px | **h3** (24px) | No change |
| **Card/panel titles** | 18px-24px | **h3** (24px) or **h4** (20px) | Some cards gain size (18px→20px or 24px) |
| **Body paragraphs** | 16px-20px | **h5** (18px) | All body text standardizes at 18px (some gain, some same) |
| **Form labels** | 16px-18px | **h5** (18px) | Labels standardize at 18px (currently mixed) |
| **Helper text / hints** | **12px-14px** ❌ | **h6** (16px) | **+2-4px increase** (12px→16px, 14px→16px) |
| **Button text** | 16px | **h6** (16px) | No change |
| **Navigation / dock labels** | **12px-16px** ❌ | **h6** (16px) | **DockBar labels increase** (12px→16px) |
| **Timestamps / metadata** | **12px-14px** ❌ | **h6** (16px) | **+2-4px increase** (more legible, less cramped) |
| **Calendar day labels** | **12px** ❌ | **h6** (16px) | **+4px increase** (day-of-week headers, day numbers larger) |
| **Small overlines / labels** | **12px** ❌ | **h6** (16px) | **+4px increase** (all small labels grow) |

### 3.2 Violations List (Current Usage Below 16px)

| Component/File | Current Usage | Current Size | 2.0 Role | Visual Impact |
|----------------|---------------|--------------|----------|---------------|
| **CalendarMonth.module.css** | Day-of-week labels (Mon, Tue, etc.) | **12px** (xs) | **h6** (16px) | **+4px** - labels more prominent, may need layout adjustment |
| **CalendarMonth.module.css** | Day number cells (dots/text) | **12px** (xs) | **h6** (16px) | **+4px** - calendar cells may need size increase to accommodate |
| **DockBar.module.css** | Dock icon labels | **12px** (xs) | **h6** (16px) | **+4px** - dock text more readable, may affect dock height |
| **HistoryPanel.module.css** | Transaction timestamps | **12px-14px** (xs-sm) | **h6** (16px) | **+2-4px** - timestamps more legible |
| **TxList.module.css** | List item timestamps | **12px** (xs) | **h6** (16px) | **+4px** - metadata text larger |
| **AccountView.module.css** | Date labels | **14px** (sm) | **h6** (16px) | **+2px** - slightly larger dates |
| **simple-budget-site** | Various utility text | **14px** (sm) via Tailwind | **h6** (16px) | **+2px** - all small text increases |

**Total Violations:** 27+ instances across 6+ components

**Conceptual Resolution:**
1. **All sub-16px text becomes h6 (16px)** - no exceptions
2. **Layout adjustments required:**
   - CalendarMonth grid may need larger cells (currently tight with 12px text)
   - DockBar may need slight height increase or icon spacing adjustment
   - Timestamps/metadata gain prominence (acceptable - improves accessibility)
3. **Design impact:** Interfaces become slightly more spacious, less cramped. This is **intentional per V10.2.1 minimalism** - readability over density.

### 3.3 Weight Violations (500/600 → 400/700)

| Current Usage | Current Weight | 2.0 Weight | Rationale |
|---------------|---------------|------------|-----------|
| Body text emphasis | 500 (medium) | 400 (normal) | Emphasis should come from semantic colors or h4/h3 sizing, not intermediate weight |
| Form labels | 500-600 (medium-semibold) | 400 (normal) | Labels are h5 (body text), use normal weight |
| Button text | 600 (semibold) | 400 (normal) | Buttons use h6 (16px), normal weight. Emphasis comes from bg color, not weight |
| Small headers | 600 (semibold) | 700 (bold) | If it's a header (h4), use bold. If it's body text (h5), use normal. No middle ground. |
| simple-budget-site h4 | 600 (semibold) | 700 (bold) | h4 must use bold for hierarchy |

**Resolution:** All weights become 400 or 700. No intermediate weights allowed.

---

## STEP 4: 2.0 TYPOGRAPHY SPEC SUMMARY

### The 6-Role Typography System

**Core Principles:**
- **6 roles only:** h1, h2, h3, h4, h5, h6 (no other named sizes)
- **18px body text (h5)** for optimal readability
- **16px absolute minimum (h6)** - no text below this floor
- **2 weights only:** 400 (normal) and 700 (bold)
- **Headings (h1-h4) are bold;** body text (h5-h6) is normal weight

### Role Definitions

```
h1: 36px (3rem), weight 700, line-height 1.2
    → Page hero titles, top-level marketing headings

h2: 30px (1.875rem), weight 700, line-height 1.25
    → Section headers, major subsection titles

h3: 24px (1.5rem), weight 700, line-height 1.3
    → Modal titles, panel headers, card titles

h4: 20px (1.25rem), weight 700, line-height 1.4
    → Small card headers, emphasized labels, section headers

h5: 18px (1.125rem), weight 400, line-height 1.6
    → BODY TEXT (default), form labels, standard UI text

h6: 16px (1rem), weight 400, line-height 1.5
    → FLOOR - timestamps, metadata, helper text, small labels
```

### Default Role Assignments

**Headings:**
- Page hero (landing) → h1
- Section titles (landing) → h2
- Modal/dialog titles → h3
- Panel/card headers → h3 or h4 (depending on prominence)
- Inline section headers → h4

**Body & UI Text:**
- Paragraphs → h5
- Form labels → h5
- List item text → h5
- Navigation labels → h5 or h6
- Button text → h6

**Metadata & Supporting Text:**
- Timestamps ("2 hours ago") → h6
- Helper text ("Optional") → h6
- Placeholders → h6
- Footer text → h6
- Day-of-week labels → h6

### Hard Constraints

1. **16px minimum floor:**
   - NO text below 16px under any circumstance
   - All current 12px/14px text becomes 16px (h6)
   - Visual: Interfaces become more spacious and accessible

2. **18px body default:**
   - Default paragraph text is 18px (h5), not 16px
   - Improves sustained reading comfort
   - 16px (h6) is for secondary/meta content only, not primary text

3. **2 weights only:**
   - Headings (h1-h4): weight 700 (bold)
   - Body text (h5-h6): weight 400 (normal)
   - No 500/600 weights allowed

4. **No intermediate sizes:**
   - Only the 6 defined sizes exist
   - No "17px for this one component" exceptions
   - Consistency over pixel-perfect design preservation

### Migration Impact

**Components requiring layout adjustment:**
- **CalendarMonth:** Day labels/numbers increase 12px→16px; grid cells may need enlargement
- **DockBar:** Labels increase 12px→16px; dock may need slight height increase
- **HistoryPanel, TxList, AccountView:** Timestamps increase 12px-14px→16px; row heights may adjust
- **All forms:** Labels standardize at 18px (h5); some grow, some shrink to match

**Visual changes:**
- More spacious, less cramped interfaces
- Improved accessibility (larger minimum text)
- Slightly less information density (acceptable tradeoff for readability)
- Consistent typography hierarchy (no arbitrary size variations)

**Acceptable per V10.2.1 design principles:** Minimalism prioritizes clarity and readability over maximal density.

---

## IMPLEMENTATION NOTES (FOR FUTURE TASKS)

### Token System Changes (Task 3)

**Delete from tokens.css:**
- `--font-size-xs` (12px) - VIOLATION
- `--font-size-sm` (14px) - VIOLATION
- `--font-size-md` (16px) - becomes h6
- `--font-size-lg` (18px) - becomes h5
- `--font-size-xl` (20px) - becomes h4
- `--font-size-2xl` (24px) - becomes h3
- `--font-size-3xl` (30px) - becomes h2
- `--font-size-4xl` (36px) - becomes h1
- `--font-weight-medium` (500) - OUT-OF-POLICY
- `--font-weight-semibold` (600) - OUT-OF-POLICY

**Add to tokens.css:**
```css
/* Typography roles (2.0) */
--type-h1-size: 3rem;        /* 36px */
--type-h1-weight: 700;
--type-h1-line-height: 1.2;

--type-h2-size: 1.875rem;    /* 30px */
--type-h2-weight: 700;
--type-h2-line-height: 1.25;

--type-h3-size: 1.5rem;      /* 24px */
--type-h3-weight: 700;
--type-h3-line-height: 1.3;

--type-h4-size: 1.25rem;     /* 20px */
--type-h4-weight: 700;
--type-h4-line-height: 1.4;

--type-h5-size: 1.125rem;    /* 18px - BODY */
--type-h5-weight: 400;
--type-h5-line-height: 1.6;

--type-h6-size: 1rem;        /* 16px - FLOOR */
--type-h6-weight: 400;
--type-h6-line-height: 1.5;

/* Alias for body text (most common) */
--type-body-size: var(--type-h5-size);
--type-body-weight: var(--type-h5-weight);
--type-body-line-height: var(--type-h5-line-height);
```

### Component CSS Updates (Task 3)

**For each component:**
1. Replace `font-size: var(--font-size-xs)` with `font-size: var(--type-h6-size)` (16px)
2. Replace `font-size: var(--font-size-sm)` with `font-size: var(--type-h6-size)` (16px)
3. Replace `font-size: var(--font-size-md)` with `font-size: var(--type-h6-size)` or `var(--type-h5-size)` depending on context
4. Replace `font-size: var(--font-size-lg)` with `font-size: var(--type-h5-size)` (body)
5. Replace `font-weight: 500` or `600` with `400` or `700` (eliminate intermediate weights)
6. Add `line-height: var(--type-hX-line-height)` for consistency

**Example:**
```css
/* Before */
.timestamp {
  font-size: var(--font-size-xs);  /* 12px */
  font-weight: 400;
}

/* After */
.timestamp {
  font-size: var(--type-h6-size);      /* 16px */
  font-weight: var(--type-h6-weight);  /* 400 */
  line-height: var(--type-h6-line-height); /* 1.5 */
}
```

### Layout Adjustments (Task 3)

**CalendarMonth:**
- Increase grid cell min-height to accommodate 16px text
- Adjust spacing between day numbers and balance indicators
- Day-of-week header row may need increased padding

**DockBar:**
- Increase dock item height or adjust icon-label spacing
- 16px labels may require more vertical space

**HistoryPanel, TxList:**
- Row heights auto-adjust with larger timestamps
- May need slight padding tweaks

---

## SUMMARY

**Current State:**
- 8 font sizes (12px-48px+), 2 below 16px floor ❌
- 4 weights (400, 500, 600, 700), 2 out-of-policy ❌
- 27+ instances of sub-16px text across components
- Inconsistent body text sizes (16px-20px mixed)

**2.0 Target State:**
- 6 roles (h1-h6) with fixed sizes (36px, 30px, 24px, 20px, 18px, 16px)
- 2 weights (400 for body, 700 for headings)
- 16px absolute minimum (h6)
- 18px default body text (h5)
- All sub-16px text eliminated

**Migration Impact:**
- 27+ text elements increase in size (12px/14px → 16px)
- Layout spacing adjustments required in CalendarMonth, DockBar, timestamps
- Improved readability and accessibility
- Slightly reduced information density (acceptable per minimalist design)

**Next Steps (Future Tasks):**
- Task 3: Implement typography token changes + component CSS updates
- Task 3: Adjust layouts to accommodate larger minimum text size
- Task 5: Apply 6-role system to simple-budget-site (remove Tailwind text-* utilities)

---

**END OF TYPOGRAPHY ANALYSIS**
**Status:** Typography role system defined, violations identified, migration plan ready
**Authority:** Hard constraints from project owner (6 roles, 18px body, 16px floor)
**Scope:** Analysis/planning only - no code changes yet
