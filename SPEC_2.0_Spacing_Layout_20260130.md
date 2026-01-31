# 2.0 Spacing & Layout Specification
## Reduced Scale + Structural Rules (Target Specification)

**Date:** 2026-01-30
**Status:** Target specification for 2.0 implementation
**Authority:** Derived from 1.4 primitives + usage analysis
**Goal:** Minimal, coherent spacing system with clear structural rules

---

## PRINCIPLE: REDUCTION & CONSISTENCY

**Design Philosophy:**
- **Fewer spacing values** = more consistency, less decision paralysis
- **Repeated spacing patterns** = visual rhythm and predictability
- **Intentional constraints** = force simplicity over micro-optimization
- **16px as gravitational center** = most spacing decisions default to this value

**Non-Goals:**
- Preserving every existing spacing usage
- Accommodating one-off layout needs with new tokens
- Creating a comprehensive utility library (avoid Tailwind-style explosion)

---

## PART 1: REDUCED SPACING SCALE

### 1.1 Core Spacing Values (8 Values)

| Token Name | Value (rem) | Value (px) | Primary Role | When to Use |
|-----------|-------------|-----------|--------------|-------------|
| `--space-0` | 0 | 0 | Resets | Margin/padding resets, flush layouts |
| `--space-1` | 0.25rem | 4px | **Micro-spacing** | Icon-to-text, tight label groups, calendar micro-layouts |
| `--space-2` | 0.5rem | 8px | **Tight spacing** | Compact UI elements, small gaps, dense lists |
| `--space-4` | 1rem | 16px | **Standard spacing (PRIMARY)** | Default card padding, form field gaps, list items, most use cases |
| `--space-6` | 1.5rem | 24px | **Card padding** | Standard panel/card internal padding, medium gaps |
| `--space-8` | 2rem | 32px | **Large spacing** | Large panel padding (responsive), content block stacking, major gaps |
| `--space-12` | 3rem | 48px | **Extra-large spacing** | Page horizontal gutters (desktop), rare large vertical gaps |
| **Marketing-Only Section Rhythm:** |
| `--space-section-base` | 5rem | 80px | Section vertical padding (mobile/tablet) | Marketing page section-to-section rhythm |
| `--space-section-lg` | 6rem | 96px | Section vertical padding (desktop) | Marketing page section-to-section rhythm at large breakpoints |

**Total Core Values:** 7 general-purpose + 2 marketing-specific = **9 spacing values**

### 1.2 Deprecated / Legacy Values (Do Not Use in New Work)

| Token Name | Value (rem) | Value (px) | Status | Migration Path |
|-----------|-------------|-----------|--------|----------------|
| `--space-2xs` | 0.125rem | 2px | ❌ **DROP** | Round up to `--space-1` (4px) |
| `--space-3` | 0.75rem | 12px | ⚠️ **OPTIONAL (Legacy)** | Use `--space-2` (8px) or `--space-4` (16px) |
| `--space-5` | 1.25rem | 20px | ⚠️ **OPTIONAL (Legacy)** | Use `--space-4` (16px) or `--space-6` (24px) |
| `--space-10` | 2.5rem | 40px | ⚠️ **OPTIONAL (Legacy)** | Use `--space-8` (32px) or `--space-12` (48px) |
| `--space-3xl` | 4rem | 64px | ❌ **DROP** | Use `--space-12` (48px) or `--space-section-base` (80px) |

**Legacy Aliases (Backward Compatibility — Remove in 3.0):**
```
--space-xs:  var(--space-1)   // 4px
--space-sm:  var(--space-2)   // 8px
--space-md:  var(--space-4)   // 16px - PRIMARY
--space-lg:  var(--space-6)   // 24px
--space-xl:  var(--space-8)   // 32px
--space-2xl: var(--space-12)  // 48px
```

**Migration Strategy:**
- Existing code may continue using legacy aliases until 3.0
- New code MUST use numeric tokens (`--space-4`, not `--space-md`)
- 2px usages MUST be rounded up to 4px
- 12px, 20px, 40px, 64px usages MUST migrate to nearest core value

---

## PART 2: SPACING RULES BY CONTEXT

### 2.1 App Shell

| Element | Spacing Rule | Token | Notes |
|---------|-------------|-------|-------|
| **AppHeader vertical padding** | 16px | `--space-4` | Consistent header height |
| **AppHeader element gaps** | 8px | `--space-2` | Between logo, nav items, buttons |
| **Main content horizontal padding** | 16px mobile, 24px tablet+ | `--space-4` → `--space-6` | Responsive page gutters |
| **DockBar vertical padding** | 8px | `--space-2` | Compact bottom nav |
| **DockBar item gaps** | 8px | `--space-2` | Between dock icons |

**Rule:** Shell uses tight spacing (8-16px). No large spacing in navigation.

### 2.2 Page Body (App Content Area)

| Element | Spacing Rule | Token | Notes |
|---------|-------------|-------|-------|
| **Component vertical stacking** | 32px | `--space-8` | Between major components on a page |
| **Section spacing (within components)** | 16px | `--space-4` | Between related content blocks |
| **Tight groupings** | 8px | `--space-2` | Between tightly related items |

**Rule:** Page-level spacing defaults to 16px for related content, 32px for component separation.

### 2.3 Cards / Panels

| Element | Spacing Rule | Token | Notes |
|---------|-------------|-------|-------|
| **Card internal padding** | 16px mobile, 24px tablet+ | `--space-4` → `--space-6` | Standard card padding |
| **Large panel internal padding** | 24px mobile, 32px tablet+ | `--space-6` → `--space-8` | HistoryPanel, AddTransactionPanel, modals |
| **Card header-to-body gap** | 16px | `--space-4` | Between header and content |
| **Card element gaps (internal)** | 8px to 16px | `--space-2` to `--space-4` | Depends on element density |

**Rule:** Cards default to 16-24px padding. Large panels use 24-32px. Internal gaps use 8-16px.

### 2.4 Lists / Tables

| Element | Spacing Rule | Token | Notes |
|---------|-------------|-------|-------|
| **List item padding** | 16px | `--space-4` | TxList, HistoryPanel rows |
| **Vertical spacing between list items** | 0 (border-separated) OR 8px | `--space-0` OR `--space-2` | Prefer borders over gaps |
| **List item internal gaps** | 4px to 8px | `--space-1` to `--space-2` | Icon-to-text, label-to-value |

**Rule:** List rows use 16px padding. Items separated by borders (no gap) or 8px gap if no borders.

### 2.5 Forms & Filters

| Element | Spacing Rule | Token | Notes |
|---------|-------------|-------|-------|
| **Field vertical stacking** | 16px | `--space-4` | Between form fields |
| **Field group spacing** | 24px | `--space-6` | Between major form sections |
| **Label-to-input gap** | 4px | `--space-1` | Tight label positioning |
| **Helper text spacing** | 4px | `--space-1` | Below input, above next field |
| **Form section spacing** | 24px to 32px | `--space-6` to `--space-8` | Between sections with headers |

**Rule:** Forms use 16px field stacking, 4px micro-spacing for labels/helpers, 24px section separation.

### 2.6 Calendar / Grid Layouts

| Element | Spacing Rule | Token | Notes |
|---------|-------------|-------|-------|
| **Grid cell padding** | 8px | `--space-2` | CalendarMonth day cells |
| **Grid gaps (tight)** | 4px | `--space-1` | Calendar day grid |
| **Grid gaps (standard)** | 16px | `--space-4` | Feature grids, dashboards |
| **Grid gaps (spacious)** | 32px | `--space-8` | Marketing feature sections |

**Rule:** Grids use 4px for tight layouts (calendar), 16px for standard dashboards, 32px for marketing.

### 2.7 Hero & Marketing Sections

| Element | Spacing Rule | Token | Notes |
|---------|-------------|-------|-------|
| **Section-to-section vertical** | 80px mobile, 96px desktop | `--space-section-base` → `--space-section-lg` | Consistent section rhythm |
| **Section internal content stacking** | 32px | `--space-8` | Between heading, paragraphs, CTAs |
| **Section horizontal padding** | 24px mobile, 32px tablet, 48px desktop | `--space-6` → `--space-8` → `--space-12` | Spacious page gutters |
| **Hero vertical padding** | 48px to 80px | `--space-12` to `--space-section-base` | Large breathing room |
| **Feature card gaps** | 16px to 32px | `--space-4` to `--space-8` | Grid gaps for feature grids |

**Rule:** Marketing sections use large rhythm (80-96px vertical), spacious gutters (24-48px), 32px content stacking.

---

## PART 3: LAYOUT PATTERNS & CONTAINERS

### 3.1 Container Sizes (Reduced Set)

**Primary Containers (Keep):**

| Token Name | Value (rem) | Value (px) | Primary Use Case |
|-----------|-------------|-----------|-----------------|
| `--container-xs` | 28rem | 448px | Small modals, compact forms, alerts |
| `--container-md` | 36rem | 576px | **App primary content width**, narrow forms, article content |
| `--container-lg` | 48rem | 768px | Wide article pages, guide content, feature sections |
| `--container-xl` | 80rem | 1280px | **Marketing primary width**, full-width sections |

**Deprecated Containers:**

| Token Name | Value (rem) | Value (px) | Status | Migration Path |
|-----------|-------------|-----------|--------|----------------|
| `--container-sm` | 32rem | 512px | ⚠️ **UNUSED (Consider Dropping)** | Use `--container-xs` (28rem) or `--container-md` (36rem) |

**Off-Scale (Do Not Use):**
- `max-w-5xl` (64rem / 1024px) in current marketing site → **Use `--container-lg` (48rem) or `--container-xl` (80rem)**

### 3.2 Layout Patterns

#### Pattern 1: App Shell (Narrow Column)
```
Structure: Fixed header + scrollable content + fixed footer
Content width: --container-md (36rem / 576px)
Horizontal padding: --space-4 mobile (16px), --space-6 tablet+ (24px)
Vertical scroll: Component-level (panels scroll internally)

Breakpoint behavior:
- 480px+: Increase padding to 24px
- No layout reflow (always single column)
```

**Use for:** fortunetell-frontend app pages, tools, dashboards

#### Pattern 2: Marketing Page (Wide Sections)
```
Structure: Full-width sections with constrained content
Section width: --container-xl (80rem / 1280px)
Horizontal padding: --space-6 mobile (24px), --space-8 tablet (32px), --space-12 desktop (48px)
Vertical rhythm: --space-section-base (80px), --space-section-lg (96px) at 1024px+

Breakpoint behavior:
- 640px+: Increase padding to 32px
- 1024px+: Increase padding to 48px, section rhythm to 96px
```

**Use for:** simple-budget-site landing pages, marketing pages

#### Pattern 3: Article Content (Medium Column)
```
Structure: Centered narrow column for readability
Content width: --container-lg (48rem / 768px)
Horizontal padding: --space-4 mobile (16px), --space-6 tablet+ (24px)

Breakpoint behavior:
- 640px+: Increase padding to 24px
- No multi-column layout (always single column for readability)
```

**Use for:** Blog posts, guides, documentation, long-form content

#### Pattern 4: Feature Grid
```
Structure: Responsive grid (1-col mobile, 2-col tablet, 2-3 col desktop)
Grid gap: --space-4 (16px) or --space-8 (32px)

Breakpoint behavior:
- Mobile: 1 column, gap-4 (16px)
- 768px+: 2 columns, gap-4 (16px) or gap-8 (32px)
- 1024px+: 2-3 columns (depends on content), gap-8 (32px)
```

**Use for:** Feature sections, pricing cards, FAQ grids

#### Pattern 5: Form Container
```
Structure: Centered small-to-medium width form
Form width: --container-xs (28rem) for compact, --container-md (36rem) for standard
Horizontal padding: --space-4 (16px)
Field stacking: --space-4 (16px)

Breakpoint behavior:
- No width changes (forms remain narrow for usability)
- Padding increases slightly on larger screens if needed
```

**Use for:** Login forms, waitlist forms, contact forms, settings

---

## PART 4: BREAKPOINT USAGE RULES

### 4.1 Core Breakpoints (5 Values)

| Token Name | Value (px) | Primary Purpose | When to Use |
|-----------|-----------|----------------|-------------|
| `--bp-sm` | 480px | **App mobile-first transitions** | App padding increase, compact→comfortable UI |
| `--bp-md` | 640px | **Typography & basic responsiveness** | Font size increases, padding tweaks, form layouts |
| `--bp-lg` | 768px | **Layout reflow (PRIMARY SHARED)** | 1-col → 2-col grids, major layout changes, both app & marketing |
| `--bp-xl` | 1024px | **Desktop refinements** | Larger spacing, 2-col → 3-col grids, section padding increases |
| `--bp-2xl` | 1280px | **Extra-wide displays** | Rare; max-width enforcement, no major layout changes |

**Total Breakpoints:** 5 (reduced from potential proliferation)

### 4.2 Breakpoint Behavior Rules

#### At 480px (sm) — App-Focused
**Allowed changes:**
- Increase page horizontal padding: `--space-4` (16px) → `--space-6` (24px)
- Adjust compact UI spacing (calendar, tight lists)
- Do NOT change layout structure (no column changes)

**Use primarily in:** fortunetell-frontend app

#### At 640px (md) — Typography & Padding
**Allowed changes:**
- Increase font sizes (h1-h6 responsive scaling)
- Increase page horizontal padding (marketing): `--space-6` (24px) → `--space-8` (32px)
- Change form layouts: stack → inline (e.g., email + button inline)
- Do NOT change major layout structure (wait for lg)

**Use primarily in:** simple-budget-site marketing, typography adjustments

#### At 768px (lg) — Layout Reflow (SHARED PRIMARY)
**Allowed changes:**
- **1-column → 2-column grids** (feature sections, dashboards)
- **Flex direction changes** (stack → row for hero layouts)
- Adjust grid gaps: `--space-4` (16px) → `--space-8` (32px)
- Increase card padding (optional): `--space-4` (16px) → `--space-6` (24px)

**Use in:** BOTH app and marketing for major layout changes

#### At 1024px (xl) — Desktop Refinements
**Allowed changes:**
- Increase page horizontal padding (marketing): `--space-8` (32px) → `--space-12` (48px)
- Increase section vertical rhythm (marketing): `--space-section-base` (80px) → `--space-section-lg` (96px)
- **2-column → 3-column grids** (wide feature grids only)
- Increase panel padding (optional): `--space-6` (24px) → `--space-8` (32px)

**Use primarily in:** simple-budget-site marketing for spacious desktop layouts

#### At 1280px (2xl) — Rarely Used
**Allowed changes:**
- Enforce max-width constraints on ultra-wide displays
- Do NOT introduce new spacing or layout changes at this breakpoint

**Status:** ⚠️ **Minimal usage; consider deprecating in 3.0**

### 4.3 Breakpoint Usage Constraints

**Rules:**
1. **Do NOT introduce spacing changes at every breakpoint** — limit to 2-3 breakpoints per property
2. **Prioritize 768px (lg) for layout changes** — it's the shared breakpoint for both app and marketing
3. **App uses 480px and 768px primarily** — marketing uses 640px, 768px, 1024px primarily
4. **No new breakpoints** — use only the 5 defined values
5. **Mobile-first approach** — define base styles, enhance progressively

---

## PART 5: CROSS-SURFACE ALIGNMENT RULES

### 5.1 Mandatory Alignment (MUST Match)

**Both fortunetell-frontend and simple-budget-site MUST align on:**

1. **Primary spacing unit:** `--space-4` (16px) as default spacing for most use cases
2. **Card padding range:** 16-24px (`--space-4` to `--space-6`) for standard cards
3. **Form container widths:** `--container-xs` (28rem) to `--container-md` (36rem) for forms
4. **Breakpoint at 768px:** Both use this for major layout changes (grids, flex direction)
5. **Typography roles:** Both use h1-h6 system (18px body, 16px floor)
6. **Color system:** Both use 11 base colors (5 theme + 6 semantic)
7. **No shadows:** Both eliminate shadows (except focus outlines)

### 5.2 Intentional Divergence (ALLOWED Differences)

**Structural differences permitted between app and marketing:**

| Aspect | fortunetell-frontend (App) | simple-budget-site (Marketing) | Reason |
|--------|---------------------------|-------------------------------|--------|
| **Primary content width** | Narrow (36rem / 576px) | Wide (80rem / 1280px) | App prioritizes readability; marketing prioritizes visual impact |
| **Page horizontal padding** | Modest (16-24px) | Spacious (24-48px responsive) | App is narrow, doesn't need large gutters; marketing has wide content |
| **Section vertical rhythm** | N/A (no large sections) | 80-96px rhythm | App has component-level layouts; marketing has section-based pages |
| **Primary breakpoints** | 480px, 768px | 640px, 768px, 1024px | App mobile-first; marketing follows Tailwind conventions |
| **Grid gaps** | 8-16px (tighter) | 16-32px (spacious) | App is dense/compact; marketing is spacious/visual |

**Rationale:** These divergences reflect different content purposes (tool vs marketing) and should NOT be forced to converge.

### 5.3 Migration Targets (Align in 2.0 Implementation)

**Currently misaligned but MUST converge:**

1. **Eliminate off-scale values:**
   - App: Remove 2px (`--space-2xs`), round up to 4px
   - Marketing: Remove 64px (`py-16`), use 48px or 80px

2. **Consolidate unused values:**
   - App: Remove unused 12px, 20px, 40px token definitions
   - Marketing: Avoid using 12px, 20px in new work (use 8px, 16px, or 24px instead)

3. **Align button radius:**
   - Currently: App uses pill (9999px), marketing uses 12px
   - Target: Both use `--radius-lg` (12px) for consistency

4. **Align form spacing:**
   - Both use `--space-4` (16px) for field stacking
   - Both use `--space-1` (4px) for label-to-input gaps
   - Both use `--space-6` (24px) for section separation

---

## PART 6: IMPLEMENTATION RULES

### 6.1 Spacing Decision Tree

**When choosing spacing, follow this decision tree:**

1. **Is it a reset or flush layout?** → Use `--space-0` (0)
2. **Is it icon-to-text or micro-alignment?** → Use `--space-1` (4px)
3. **Is it tight/compact UI (dense lists, calendar)?** → Use `--space-2` (8px)
4. **Is it standard card padding, form fields, list items?** → Use `--space-4` (16px) ✓ **DEFAULT**
5. **Is it card internal padding or medium gaps?** → Use `--space-6` (24px)
6. **Is it large panel padding or content stacking?** → Use `--space-8` (32px)
7. **Is it page gutters (desktop) or rare large gaps?** → Use `--space-12` (48px)
8. **Is it marketing section rhythm?** → Use `--space-section-base` (80px) or `--space-section-lg` (96px)

**Default choice when uncertain:** `--space-4` (16px)

### 6.2 Layout Decision Tree

**When choosing layout pattern, follow this decision tree:**

1. **Is it an app page/tool/dashboard?** → Use Pattern 1 (App Shell, 36rem)
2. **Is it a marketing landing page?** → Use Pattern 2 (Marketing Page, 80rem)
3. **Is it long-form content (blog, guide)?** → Use Pattern 3 (Article Content, 48rem)
4. **Is it a grid of features/cards?** → Use Pattern 4 (Feature Grid)
5. **Is it a form (login, contact, waitlist)?** → Use Pattern 5 (Form Container, 28-36rem)

### 6.3 Breakpoint Decision Rules

**When adding responsive behavior:**

1. **Is it an app-specific adjustment (padding, compact UI)?** → Use 480px (sm)
2. **Is it typography or basic padding?** → Use 640px (md)
3. **Is it a layout reflow (1-col → 2-col)?** → Use 768px (lg) ✓ **PRIMARY**
4. **Is it desktop refinement (spacing increase, 2→3 col)?** → Use 1024px (xl)
5. **Is it ultra-wide enforcement only?** → Use 1280px (2xl) — RARE

**Default choice for layout changes:** 768px (lg)

### 6.4 Anti-Patterns (DO NOT)

❌ **Do NOT:**
1. Create new spacing values outside the 8 core values
2. Use 2px, 12px, 20px, 40px, or 64px in new code (legacy only)
3. Create container widths outside the 4 primary values (28, 36, 48, 80 rem)
4. Add new breakpoints (use only the 5 defined)
5. Use spacing tokens for font-size, border-radius, or other non-spacing properties
6. Create "one-off" spacing for a single component (use nearest core value)
7. Use `--space-section-base/lg` in app contexts (marketing only)
8. Apply different spacing patterns within the same context (e.g., cards all use same padding)

---

## PART 7: SPEC SUMMARY

### 7.1 Token Reduction Summary

**Spacing:**
- **Before (1.4):** 13 values (10 on-scale + 3 off-scale)
- **After (2.0):** 9 values (7 general + 2 marketing-specific)
- **Reduction:** 4 fewer values (31% reduction)

**Containers:**
- **Before (1.4):** 5 containers
- **After (2.0):** 4 primary containers (1 deprecated)
- **Reduction:** Effectively 1 fewer container

**Breakpoints:**
- **Before (1.4):** 5 defined, usage scattered
- **After (2.0):** 5 kept, but clear usage rules (2-3 per surface)
- **Improvement:** Clarified purpose, reduced arbitrary usage

### 7.2 Core System (Quick Reference)

**Spacing Scale (7 Core + 2 Marketing):**
```
0, 4px, 8px, 16px*, 24px, 32px, 48px  (*primary)
+ 80px, 96px (marketing sections only)
```

**Containers (4 Primary):**
```
28rem (xs), 36rem (md)*, 48rem (lg), 80rem (xl)  (*app default)
```

**Breakpoints (5 Values, 3 Primary):**
```
480px (app), 640px (typography), 768px* (layout), 1024px (desktop), 1280px (rare)
(*shared primary)
```

**Spacing by Context:**
- Shell/Nav: 8-16px
- Cards/Panels: 16-24px padding, 8-16px gaps
- Forms: 16px stacking, 4px micro-spacing
- Lists: 16px padding, borders or 8px gaps
- Marketing: 80-96px sections, 24-48px gutters, 32px content

**Layout Patterns (5 Patterns):**
1. App Shell (36rem, narrow)
2. Marketing Page (80rem, wide)
3. Article Content (48rem, medium)
4. Feature Grid (responsive 1-2-3 col)
5. Form Container (28-36rem, compact)

### 7.3 Migration Priorities

**High Priority (Breaking Changes):**
1. Eliminate 2px usage → round to 4px (affects CalendarMonth, HistoryPanel, forms)
2. Eliminate 64px usage → use 48px or 80px (affects marketing sections)
3. Change button radius: pill → 12px (visual change across all buttons)

**Medium Priority (Cleanup):**
4. Remove unused spacing tokens: 12px, 20px, 40px definitions
5. Replace off-scale container: `max-w-5xl` (64rem) → use 48rem or 80rem
6. Consolidate breakpoint usage to 2-3 per surface

**Low Priority (Long-Term):**
7. Deprecate legacy aliases (`--space-xs`, etc.) — remove in 3.0
8. Consider dropping `--container-sm` (32rem) if truly unused
9. Consider dropping `--bp-2xl` (1280px) if minimal usage continues

---

**END OF 2.0 SPACING & LAYOUT SPECIFICATION**
**Status:** Target specification ready for implementation (Task 3+)
**Next Steps:** Use this spec to drive token.css updates, component CSS migrations, and cross-surface alignment
