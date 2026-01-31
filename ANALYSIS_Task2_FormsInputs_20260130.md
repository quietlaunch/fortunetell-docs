# Task 2: Forms & Inputs Analysis & 2.0 System Specification
## Cross-Surface Forms & Inputs System for 2.0 (Spec/Plan Only — No Code Changes)

**Date:** 2026-01-30
**Scope:** Inventory, verify, and document complete Forms & Inputs system for 2.0
**Authority:** DEV_DesignTokensSystem_1.4 + V10.2.1 Architecture + Color/Typography constraints
**Constraints:**
- 11 base colors only
- No shadows
- Typography: 16px minimum, 18px body default
- 2 weights only (400, 700)
- Spacing from 1.4 scale

---

## STEP 1: FRONTEND FORM TOKEN INVENTORY

### 1.1 Button Tokens (tokens.css Section 6.2)

| Token Name | Controls | Value | 2.0 Compliance |
|-----------|----------|-------|----------------|
| **Layout & Structure** |
| `--button-radius` | Border radius for all buttons | `9999px` (pill shape) | ✓ COMPLIANT (radius value) |
| `--button-padding-y` | Vertical padding | `0.5rem` (8px) | ✓ COMPLIANT (--space-xs) |
| `--button-padding-x` | Horizontal padding | `1rem` (16px) | ✓ COMPLIANT (--space-md) |
| **Primary Button** |
| `--button-primary-bg` | Background (default) | `var(--brand-green)` → #10b981 | Maps to `--semantic-color-safe` ✓ |
| `--button-primary-bg-hover` | Background (hover) | `var(--brand-green-dark)` → #047857 | ❌ OUT-OF-POLICY (not in 11 colors) |
| `--button-primary-text` | Text color | `var(--text-on-brand)` → #ffffff | Maps to `--theme-background` ✓ |
| `--button-primary-shadow` | Box shadow | `0 10px 20px rgba(16, 185, 129, 0.25)` | ❌ VIOLATION (shadows forbidden in 2.0) |
| **Secondary Button** |
| `--button-secondary-bg` | Background (default) | `transparent` | ✓ COMPLIANT (not a color) |
| `--button-secondary-border` | Border color | `var(--border-subtle)` → #e2e8f0 | Maps to `--theme-accent` (approximate) |
| `--button-secondary-text` | Text color | `var(--text-strong)` → #0f172a | Maps to `--theme-primary` ✓ |
| **Danger Button** |
| `--button-danger-bg` | Background (default) | `var(--bg-danger)` → #ff1a1a | Maps to `--semantic-color-danger` ✓ |
| `--button-danger-bg-hover` | Background (hover) | `var(--bg-danger-hover)` → #e60000 | ❌ OUT-OF-POLICY (not in 11 colors) |
| `--button-danger-text` | Text color | `var(--text-on-semantic)` → #ffffff | Maps to `--theme-background` ✓ |
| `--button-danger-border` | Border color | `var(--bg-danger)` → #ff1a1a | Maps to `--semantic-color-danger` ✓ |
| `--button-danger-shadow` | Box shadow | `var(--button-primary-shadow)` | ❌ VIOLATION (shadows forbidden) |

### 1.2 Input Tokens (tokens.css Section 6.3)

| Token Name | Controls | Value | 2.0 Compliance |
|-----------|----------|-------|----------------|
| `--input-bg` | Background color | `var(--surface-page)` → #ffffff | Maps to `--theme-background` ✓ |
| `--input-border` | Border color (default) | `var(--border-subtle)` → #e2e8f0 | Maps to `--theme-accent` (approximate) |
| `--input-radius` | Border radius | `0.75rem` (12px) | ✓ COMPLIANT (--radius-lg) |
| `--input-padding-y` | Vertical padding | `0.5rem` (8px) | ✓ COMPLIANT (--space-xs) |
| `--input-padding-x` | Horizontal padding | `1rem` (16px) | ✓ COMPLIANT (--space-md) |
| `--input-font-size` | Text size | `var(--font-size-lg)` → 18px | Maps to h5 ✓ (body text size) |
| `--input-text` | Text color | `var(--text-body)` → #334155 | Maps to `--theme-secondary` ✓ |
| `--input-placeholder` | Placeholder color | `var(--text-placeholder)` → #94a3b8 | Maps to `--theme-accent` (approximate) |
| `--input-focus-border` | Border color (focus) | `var(--brand-green)` → #10b981 | Maps to `--semantic-color-safe` ✓ |
| `--input-focus-ring` | Focus ring (shadow) | `var(--focus-ring-base)` → 0 0 0 3px rgba(...) | ❌ PARTIAL VIOLATION (shadow-like, but accessibility-critical) |

**Note on focus ring:** Focus ring is technically a shadow but is **accessibility-critical** for keyboard navigation. May warrant exception or CSS outline replacement.

### 1.3 Checkbox/Radio Tokens (tokens.css Section 6.x)

| Token Name | Controls | Value | 2.0 Compliance |
|-----------|----------|-------|----------------|
| `--control-checkbox-size` | Checkbox width/height | `1.1rem` (17.6px) | ✓ COMPLIANT (>16px floor) |
| `--control-radio-size` | Radio width/height | `1.1rem` (17.6px) | ✓ COMPLIANT (>16px floor) |

**Additional styling from globals.css:**
- `accent-color: var(--brand-green)` (checkbox/radio checked color) → Maps to `--semantic-color-safe` ✓
- Border: `1px solid var(--border-subtle)` → Maps to `--theme-accent`
- Background: `var(--surface-page)` → Maps to `--theme-background` ✓

### 1.4 Global Form Styles (globals.css Lines 20-68)

**Input Types Covered:**
- `input[type="text"]`
- `input[type="email"]`
- `input[type="password"]`
- `input[type="number"]`
- `input[type="date"]`
- `input[type="search"]`
- `input[type="tel"]`
- `select`
- `textarea`

**States Defined:**
- Default: Uses input tokens (bg, border, text, padding, radius, font-size)
- Placeholder: `::placeholder` with `--input-placeholder` color
- Focus: `*:focus` with outline using `--app-accent` (green)
- Disabled: **NOT explicitly styled in globals.css** ❌ GAP

### 1.5 Global Button Styles (globals.css Lines 193-299)

**Button Classes Defined:**
- `.btn` (base layout + default secondary styling)
- `.btn-primary` (green background)
- `.btn-secondary` (white/transparent background)
- `.btn-icon` (icon-only button)
- `.btn-danger` (red background)

**States Defined:**
- Default: All variants defined ✓
- Hover: `:hover` for primary, danger ✓
- Active: `:active` for primary, danger (opacity + transform) ✓
- Focus: `:focus-visible` with outline ✓
- Disabled: `:disabled` with opacity + pointer-events ✓

**Typography:**
- Font size: `var(--font-size-lg)` → 18px ✓ COMPLIANT (h5)
- Font weight: `var(--font-weight-medium)` → 500 ❌ VIOLATION (out-of-policy weight)

### 1.6 High-Contrast Mode (globals.css Lines 147-178)

**HC Input Overrides:**
- Background: `var(--app-surface-1)` → remaps to dark surface
- Text: `var(--app-text-primary)` → remaps to white
- Border: `var(--app-border)` → remaps to lighter gray
- Placeholder: `var(--app-text-muted)` → remaps to gray
- Focus outline: Uses `--app-border`
- Autofill: Custom `-webkit-autofill` styles with inset shadow (workaround for browser behavior)

✓ HC mode correctly remaps structural tokens only; semantic colors remain unchanged (per 2.0 color rules)

### 1.7 Token Summary

**Total Form-Related Tokens:** 22 tokens (11 button, 9 input, 2 checkbox/radio)

**Violations Found:**
1. **Shadows:** Button shadows (`--button-primary-shadow`, `--button-danger-shadow`) ❌
2. **Hover colors:** Out-of-policy hover variants (`--button-primary-bg-hover`, `--button-danger-bg-hover`) ❌
3. **Font weight:** Button text uses weight 500 (medium) ❌
4. **Focus ring:** Input focus uses box-shadow (accessibility-critical, may need exception)

**Missing Definitions:**
1. Input error state (border, text, background) ❌ GAP
2. Input disabled state (explicit styling) ❌ GAP
3. Helper/validation text tokens ❌ GAP

---

## STEP 2: FRONTEND FORM SYSTEM COMPLETENESS CHECK

### 2.1 Coverage Table

| Control Type | Required States | Token/Style Exists? | Gaps/Violations |
|-------------|----------------|-------------------|----------------|
| **Text Inputs** |
| Default | ✓ YES | All input tokens defined | Font size compliant (18px = h5) |
| Focus | ✓ YES | `--input-focus-border`, focus outline | Focus ring uses shadow (accessibility concern) |
| Error | ❌ NO | Not defined | **GAP:** No error border/text/bg tokens |
| Disabled | ❌ NO | Not explicitly styled | **GAP:** Browser defaults only |
| **Textareas** |
| Default | ✓ YES | Uses same input tokens | Same as text inputs |
| Focus | ✓ YES | Same as text inputs | Same as text inputs |
| Error | ❌ NO | Not defined | Same gap as text inputs |
| Disabled | ❌ NO | Not explicitly styled | Same gap as text inputs |
| **Select Dropdowns** |
| Default | ✓ YES | Uses same input tokens | Same as text inputs |
| Focus | ✓ YES | Same as text inputs | Same as text inputs |
| Error | ❌ NO | Not defined | Same gap as text inputs |
| Disabled | ❌ NO | Not explicitly styled | Same gap as text inputs |
| **Checkboxes/Radios** |
| Default | ✓ YES | Size tokens + globals.css styling | Size compliant (>16px) |
| Checked | ✓ YES | `accent-color` uses green | Uses semantic-safe color |
| Focus | ✓ YES | Browser default focus ring | Uses global `*:focus` outline |
| Disabled | ❌ NO | Browser defaults only | **GAP:** Not explicitly styled |
| **Primary Button** |
| Default | ✓ YES | All tokens defined | Shadow violation, weight 500 violation |
| Hover | ✓ YES | Defined | Out-of-policy hover color |
| Active | ✓ YES | Opacity + transform | Compliant |
| Focus | ✓ YES | Outline with green | Compliant |
| Disabled | ✓ YES | Opacity 0.6 + no pointer | Compliant |
| **Secondary Button** |
| Default | ✓ YES | All tokens defined | Compliant |
| Hover | ❌ NO | Not defined in globals.css | **GAP:** No hover state |
| Active | ❌ NO | Not defined | **GAP:** No active state |
| Focus | ✓ YES | Outline | Compliant |
| Disabled | ✓ YES | Opacity + no pointer | Compliant |
| **Danger Button** |
| Default | ✓ YES | All tokens defined | Shadow violation, weight 500 violation |
| Hover | ✓ YES | Defined | Out-of-policy hover color |
| Active | ✓ YES | Opacity + transform | Compliant |
| Focus | ✓ YES | Outline | Compliant |
| Disabled | ✓ YES | Opacity + no pointer | Compliant |
| **Helper/Validation Text** |
| Default | ❌ NO | Not tokenized | **GAP:** No tokens for helper text |
| Error | ❌ NO | Not tokenized | **GAP:** No tokens for error messages |
| Success | ❌ NO | Not tokenized | **GAP:** No tokens for success messages |

### 2.2 One-Off Styles / Component Overrides

**Searched for component-level form overrides:**
- No component CSS modules appear to have significant form control overrides
- Forms use the global `.btn` and input element styles consistently
- TxForm, LoginForm, SettingsView all rely on globals.css + tokens.css

✓ No significant one-off form styles found (good consistency)

### 2.3 Violations Summary

| Violation Type | Count | Examples | Impact |
|---------------|-------|----------|--------|
| **Shadows** | 3 | Button primary/danger shadows, input focus ring | Must remove or replace (focus ring needs alternative) |
| **Out-of-policy colors** | 2 | Button hover colors (#047857, #e60000) | Must replace with filter/opacity effects |
| **Out-of-policy weights** | 1 | Button text weight 500 | Must change to 400 or 700 |
| **Missing states** | 7 | Input error/disabled, secondary button hover/active, helper text tokens | Must add missing tokens/styles |

---

## STEP 3: SIMPLE-BUDGET-SITE FORMS SNAPSHOT

### 3.1 Form Patterns Identified

**Pattern 1: Email Capture Form (Hero/Waitlist)**
- **Markup:** `<form><input type="email"><button type="submit">`
- **Input classes:** `.input`
- **Button classes:** `.btn-primary` (sometimes with inline Tailwind: `px-4 py-2`, `text-sm`, etc.)
- **Locations:** index.html:315, all guide pages

**Pattern 2: Secondary CTA Buttons**
- **Markup:** `<a href="..." class="btn-primary">` or `<button class="btn-secondary">`
- **Classes:** `.btn-primary`, `.btn-secondary`
- **Locations:** Guide page headers, footer CTAs

### 3.2 simple-budget-site .input Class (global.css Lines 267-287)

| Property | Value | 2.0 Compliance |
|----------|-------|----------------|
| `width` | `100%` | ✓ Layout (not a constraint) |
| `border-radius` | `0.75rem` (12px) | ✓ COMPLIANT (--radius-lg) |
| `border` | `1px solid #e2e8f0` | ❌ OUT-OF-POLICY color (not in 11-color palette) |
| `background-color` | `#ffffff` | Maps to `--theme-background` ✓ |
| `padding` | `0.875rem 1rem` (14px 16px) | ✓ COMPLIANT (approx --space-xs + --space-md) |
| `font-size` | `1rem` (16px) | ❌ VIOLATION (should be 18px = h5 for input text) |
| `color` | `#0f172a` | Maps to `--theme-primary` (approximate) ✓ |
| `line-height` | `1.5` | ✓ Reasonable |
| `outline` | `none` | ⚠️ Accessibility concern (removes default focus) |
| `transition` | `border-color 150ms ease, box-shadow 150ms ease` | ✓ Acceptable |
| `font-weight` | `400` | ✓ COMPLIANT |

**Placeholder:**
- `color: #94a3b8` → Maps to `--theme-accent` (approximate) ✓

**Focus:**
- `border-color: #059669` → Maps to `--semantic-color-safe` ✓
- `box-shadow: 0 0 0 3px rgba(5, 150, 105, 0.2)` → ❌ SHADOW (accessibility-critical focus ring)

### 3.3 simple-budget-site .btn-primary Class (global.css Lines 248-255)

| Property | Value | 2.0 Compliance |
|----------|-------|----------------|
| `background-color` | `#059669` | Maps to `--semantic-color-safe` ✓ |
| `color` | `#ffffff` | Maps to `--theme-background` ✓ |
| `box-shadow` | `0 10px 15px -3px rgba(15, 118, 110, 0.3), ...` | ❌ VIOLATION (shadows forbidden) |
| **Hover:** |
| `background-color` | `#047857` | ❌ OUT-OF-POLICY (not in 11 colors) |

**Shared .btn styles (lines 232-246):**
- `font-size: 1rem` (16px) → ✓ COMPLIANT (h6 for button text)
- `font-weight: 600` → ❌ VIOLATION (should be 400 or 700)
- `padding: 0.875rem 1.75rem` (14px 28px) → ✓ COMPLIANT (approx spacing scale)
- `border-radius: 0.75rem` (12px) → ⚠️ DIFFERENT from frontend (frontend uses pill: 9999px)

### 3.4 simple-budget-site .btn-secondary Class (global.css Lines 257-265)

| Property | Value | 2.0 Compliance |
|----------|-------|----------------|
| `background-color` | `#ffffff` | Maps to `--theme-background` ✓ |
| `color` | `#0f172a` | Maps to `--theme-primary` (approximate) ✓ |
| `border` | `1px solid #e2e8f0` | ❌ OUT-OF-POLICY color |
| **Hover:** |
| `background-color` | `#f8fafc` | ❌ OUT-OF-POLICY (not in 11 colors) |
| `border-color` | `#cbd5e1` | ❌ OUT-OF-POLICY (not in 11 colors) |

### 3.5 Tailwind Utility Overrides

**Common inline Tailwind overrides observed:**
- `px-4 py-2` (padding overrides)
- `text-sm` (14px font size) ❌ VIOLATION (<16px floor)
- `text-xs` (12px font size) ❌ VIOLATION (<16px floor)
- `shadow-soft` (custom shadow class) ❌ VIOLATION
- `w-full sm:w-auto` (responsive width)

### 3.6 Misalignment Summary

| Dimension | simple-budget-site | fortunetell-frontend | Misalignment |
|-----------|-------------------|---------------------|--------------|
| **Button radius** | `0.75rem` (12px) | `9999px` (pill) | ❌ INCONSISTENT |
| **Input font-size** | `1rem` (16px) | `1.125rem` (18px) | ❌ VIOLATION (should be 18px = h5) |
| **Button font-size** | `1rem` (16px) | `1.125rem` (18px) | ❌ Frontend violates (should be 16px = h6) |
| **Button font-weight** | `600` (semibold) | `500` (medium) | ❌ BOTH violate (should be 400 or 700) |
| **Colors** | Raw hex (10+ colors) | Aliases to base tokens | ❌ Must tokenize to 11-color palette |
| **Shadows** | Button + input focus | Button + input focus | ❌ BOTH violate (must remove) |
| **Hover colors** | Raw hex variants | Out-of-policy tokens | ❌ BOTH violate (must use filters) |
| **Inline overrides** | Tailwind `text-sm`, `text-xs` | Minimal | ❌ Simple-budget-site has <16px violations |

---

## STEP 4: CROSS-SURFACE FORMS & INPUTS SPEC (2.0)

### 4.1 Token Set (By Role)

#### 4.1.1 Input Controls

**Default State:**
| Token Name | Maps to Base Color | Typography Role | Spacing |
|-----------|-------------------|----------------|---------|
| `--input-bg` | `--theme-background` (#ffffff) | - | - |
| `--input-border` | `--theme-accent` (#b7b7b7) | - | - |
| `--input-text` | `--theme-secondary` (#3f3f3f) | h5 (18px, weight 400) | - |
| `--input-placeholder` | `--theme-accent` (#b7b7b7) | h5 (18px, weight 400) | - |
| `--input-padding-y` | - | - | `--space-xs` (8px) |
| `--input-padding-x` | - | - | `--space-md` (16px) |
| `--input-radius` | - | - | `--radius-lg` (12px) |

**Focus State:**
| Token Name | Maps to Base Color | Notes |
|-----------|-------------------|-------|
| `--input-focus-border` | `--semantic-color-safe` (#10b981) | Green for interactive focus |
| `--input-focus-outline` | `--semantic-color-safe` (#10b981) | **NEW:** CSS outline replacement for box-shadow focus ring |
| `--input-focus-outline-offset` | - | **NEW:** 2px offset for visibility |

**Error State (NEW - Required):**
| Token Name | Maps to Base Color | Typography Role | Notes |
|-----------|-------------------|----------------|-------|
| `--input-error-border` | `--semantic-color-danger` (#ff1a1a) | - | Red border for validation errors |
| `--input-error-text` | `--semantic-color-danger` (#ff1a1a) | h6 (16px, weight 400) | Error message text color |
| `--input-error-bg` | `--theme-background` (#ffffff) | - | Background remains white (no tint) |

**Disabled State (NEW - Required):**
| Token Name | Maps to Base Color | Notes |
|-----------|-------------------|-------|
| `--input-disabled-bg` | `--theme-background` (#ffffff) | Background remains white |
| `--input-disabled-border` | `--theme-accent` (#b7b7b7) | Border remains same as default |
| `--input-disabled-text` | `--theme-accent` (#b7b7b7) | Text becomes light gray (disabled appearance) |
| `--input-disabled-opacity` | - | `0.6` (dimmed appearance) |

#### 4.1.2 Buttons

**Primary Button:**
| Token Name | Maps to Base Color | Typography Role | Spacing | Notes |
|-----------|-------------------|----------------|---------|-------|
| `--button-primary-bg` | `--semantic-color-safe` (#10b981) | - | - | Green background |
| `--button-primary-text` | `--theme-background` (#ffffff) | h6 (16px, weight 400) | - | White text on green |
| `--button-primary-border` | `--semantic-color-safe` (#10b981) | - | - | Same as background (no visible border) |
| `--button-padding-y` | - | - | `--space-xs` (8px) | Vertical padding |
| `--button-padding-x` | - | - | `--space-md` (16px) | Horizontal padding |
| `--button-radius` | - | - | `--radius-lg` (12px) | **CHANGED:** Rounded corners instead of pill |

**Primary Button Hover (2.0):**
- **NO separate hover color token**
- Use `filter: brightness(0.85)` on `--semantic-color-safe` for hover darkening
- Transition: `filter 150ms ease`

**Secondary Button:**
| Token Name | Maps to Base Color | Typography Role | Notes |
|-----------|-------------------|----------------|-------|
| `--button-secondary-bg` | `transparent` OR `--theme-background` | - | Transparent or white |
| `--button-secondary-text` | `--theme-primary` (#000000) | h6 (16px, weight 400) | Black text |
| `--button-secondary-border` | `--theme-accent` (#b7b7b7) | - | Light gray border |

**Secondary Button Hover (2.0 - NEW):**
- Background: `--theme-accent` @ 10% opacity (subtle gray tint)
- Border: `--theme-tertiary` (#7f7f7f) (darker gray for emphasis)
- Transition: `background-color 150ms ease, border-color 150ms ease`

**Danger Button:**
| Token Name | Maps to Base Color | Typography Role | Notes |
|-----------|-------------------|----------------|-------|
| `--button-danger-bg` | `--semantic-color-danger` (#ff1a1a) | - | Red background |
| `--button-danger-text` | `--theme-background` (#ffffff) | h6 (16px, weight 400) | White text on red |
| `--button-danger-border` | `--semantic-color-danger` (#ff1a1a) | - | Same as background |

**Danger Button Hover (2.0):**
- **NO separate hover color token**
- Use `filter: brightness(0.9)` on `--semantic-color-danger` for hover darkening

**All Buttons Disabled:**
- Opacity: `0.6`
- Pointer events: `none`
- Cursor: `not-allowed`

**All Buttons Active:**
- Opacity: `0.9`
- Transform: `translateY(1px)` (subtle press effect)

**All Buttons Focus:**
- Outline: `2px solid` using button's primary color (green/red) OR `--semantic-color-safe`
- Outline offset: `2px`

#### 4.1.3 Checkboxes & Radios

| Token Name | Maps to Base Color | Size | Notes |
|-----------|-------------------|------|-------|
| `--control-checkbox-size` | - | `1.1rem` (17.6px) | Size compliant (>16px) |
| `--control-radio-size` | - | `1.1rem` (17.6px) | Size compliant (>16px) |
| `--control-accent-color` | `--semantic-color-safe` (#10b981) | - | Checked state color (native `accent-color`) |
| `--control-border` | `--theme-accent` (#b7b7b7) | - | Unchecked border |
| `--control-bg` | `--theme-background` (#ffffff) | - | Unchecked background |

**Disabled:** Browser defaults (dimmed) + opacity 0.6

#### 4.1.4 Helper & Validation Text (NEW - Required)

| Token Name | Maps to Base Color | Typography Role | Use Case |
|-----------|-------------------|----------------|----------|
| `--helper-text-default` | `--theme-tertiary` (#7f7f7f) | h6 (16px, weight 400) | Helper hints below inputs ("Optional", "Max 100 characters") |
| `--helper-text-error` | `--semantic-color-danger` (#ff1a1a) | h6 (16px, weight 400) | Error messages ("Email is required") |
| `--helper-text-success` | `--semantic-color-safe` (#10b981) | h6 (16px, weight 400) | Success messages ("Saved successfully") |

---

### 4.2 State Behavior

#### 4.2.1 Input States

**Default → Focus:**
- Border color changes: `--theme-accent` → `--semantic-color-safe`
- Outline appears: `2px solid --semantic-color-safe` @ 2px offset
- No shadow (accessibility via outline instead)

**Default → Error:**
- Border color changes: `--theme-accent` → `--semantic-color-danger`
- Error helper text appears below input (red, h6 size)
- Background remains white (no error tint)

**Default → Disabled:**
- Text color changes: `--theme-secondary` → `--theme-accent` (light gray)
- Opacity: `0.6` (dimmed)
- Cursor: `not-allowed`
- Border/background remain same

#### 4.2.2 Button States

**Default → Hover:**
- **Primary/Danger:** Background darkens via `filter: brightness(0.85-0.9)`
- **Secondary:** Background gains subtle tint (`--theme-accent` @ 10% opacity), border darkens to `--theme-tertiary`
- Transition: `150ms ease`

**Default → Active (pressed):**
- Opacity: `0.9`
- Transform: `translateY(1px)` (button "depresses")
- Duration: `100ms`

**Default → Focus (keyboard):**
- Outline appears: `2px solid` using button color or green
- Outline offset: `2px`
- No box-shadow

**Enabled → Disabled:**
- Opacity: `0.6` (dimmed)
- Pointer events: `none`
- Cursor: `not-allowed`
- Background/text colors remain same (just dimmed)

---

### 4.3 Cross-Surface Alignment Rules

#### 4.3.1 Mandatory Consistency

**All forms and inputs across fortunetell-frontend and simple-budget-site MUST:**
1. Use the exact same token set defined in Section 4.1
2. Reference only the 11 base color tokens (5 theme + 6 semantic)
3. Use only h5 (18px) for input text and h6 (16px) for button text
4. Use only weight 400 for all form text (no 500/600/700 in form controls)
5. Use the same spacing tokens (`--space-xs`, `--space-md`) for padding
6. Use `--radius-lg` (12px) for all form control border-radius (NOT pill shape)
7. Use CSS filters (`brightness`) for hover effects, NOT separate hover color tokens
8. Use CSS `outline` for focus states, NOT box-shadow (except where browser-required)
9. Include all 4 states for inputs (default, focus, error, disabled)
10. Include all 5 states for buttons (default, hover, active, focus, disabled)

#### 4.3.2 Allowed Variations

**Marketing forms (simple-budget-site) MAY:**
- Adjust layout (flex vs grid, stacking order, responsive breakpoints)
- Use different form widths/max-widths
- Add additional wrapper elements for spacing
- Use different form submission handlers (JS logic)

**Marketing forms MUST NOT:**
- Use different colors outside the 11-base palette
- Use different typography sizes/weights
- Use different border-radius values
- Use shadows (even for marketing emphasis)
- Override core token values with inline styles or Tailwind utilities

#### 4.3.3 Migration Priority

**fortunetell-frontend (Task 3):**
1. Remove button shadows (`--button-primary-shadow`, `--button-danger-shadow`)
2. Replace hover colors with filter effects
3. Change button font-weight from 500 → 400
4. Add missing error state tokens
5. Add missing disabled state tokens
6. Add helper text tokens
7. Replace focus box-shadow with CSS outline
8. Change button radius from pill (9999px) → `--radius-lg` (12px)

**simple-budget-site (Task 5):**
1. Remove all raw hex colors, replace with base token references
2. Remove all shadows
3. Replace Tailwind utilities with token-based classes
4. Change input font-size from 16px → 18px (h5)
5. Change button font-weight from 600 → 400
6. Replace hover color hex with filter effects
7. Add error/disabled states
8. Ensure all text ≥16px (remove `text-sm`, `text-xs` violations)

---

### 4.4 Violation Summary (simple-budget-site vs Frontend Tokens)

| Violation Category | simple-budget-site Issues | fortunetell-frontend Issues | 2.0 Resolution |
|-------------------|---------------------------|----------------------------|----------------|
| **Colors** | 10+ raw hex colors (slate, emerald variants) | Aliases to out-of-policy colors | Consolidate to 11 base tokens only |
| **Shadows** | Button shadows, input focus shadows | Button shadows, input focus shadows | Remove all shadows; replace focus shadow with outline |
| **Hover colors** | Raw hex hover variants (#047857, #f8fafc, #cbd5e1) | Out-of-policy hover tokens | Use `filter: brightness()` instead |
| **Typography - Size** | Input: 16px (should be 18px) | Input: 18px ✓ | Inputs use h5 (18px), buttons use h6 (16px) |
| **Typography - Weight** | Buttons: 600 (semibold) | Buttons: 500 (medium) | All form text uses weight 400 |
| **Typography - Violations** | Inline `text-sm` (14px), `text-xs` (12px) | None | Remove all <16px text |
| **Border Radius** | Buttons: 12px (rounded-xl) | Buttons: 9999px (pill) | Standardize to 12px (`--radius-lg`) |
| **Missing States** | Error, disabled states not defined | Error, disabled, secondary hover not defined | Add all required states to token system |
| **Inline Overrides** | Heavy Tailwind utility usage | Minimal | Remove Tailwind, use token classes only |

---

### 4.5 Implementation Checklist (For Future Tasks)

#### fortunetell-frontend (Task 3)

**tokens.css Changes:**
- [ ] Delete `--button-primary-shadow`, `--button-danger-shadow`
- [ ] Delete `--button-primary-bg-hover`, `--button-danger-bg-hover`
- [ ] Change `--button-primary-bg` to reference `--semantic-color-safe` (not `--brand-green`)
- [ ] Change button radius: `--button-radius: 0.75rem` (was `9999px`)
- [ ] Add error state tokens: `--input-error-border`, `--input-error-text`, `--input-error-bg`
- [ ] Add disabled state tokens: `--input-disabled-bg`, `--input-disabled-border`, `--input-disabled-text`, `--input-disabled-opacity`
- [ ] Add helper text tokens: `--helper-text-default`, `--helper-text-error`, `--helper-text-success`
- [ ] Replace `--input-focus-ring` (shadow) with `--input-focus-outline` + offset

**globals.css Changes:**
- [ ] Update `.btn` font-weight: `400` (was `500`)
- [ ] Update `.btn` font-size: `var(--type-h6-size)` (16px)
- [ ] Add `.btn-primary:hover` with `filter: brightness(0.85)` (remove bg-color hover)
- [ ] Add `.btn-secondary:hover` with background tint + border darkening
- [ ] Add `.btn-danger:hover` with `filter: brightness(0.9)` (remove bg-color hover)
- [ ] Remove `box-shadow` from `.btn-primary`, `.btn-danger`
- [ ] Add input error state styles (`:invalid`, `[aria-invalid="true"]`, or custom class)
- [ ] Add input disabled state styles (`:disabled`)
- [ ] Replace input `*:focus` box-shadow with CSS `outline`
- [ ] Update input font-size: confirm uses `var(--input-font-size)` → 18px (h5)

#### simple-budget-site (Task 5)

**global.css Changes:**
- [ ] Replace all raw hex colors with `var(--theme-*)` or `var(--semantic-color-*)` references
- [ ] Remove `.input` box-shadow on focus, add CSS outline
- [ ] Remove `.btn-primary`, `.btn-secondary` box-shadows
- [ ] Update `.input` font-size: `1.125rem` (was `1rem`)
- [ ] Update `.btn` font-weight: `400` (was `600`)
- [ ] Add `.btn-primary:hover`, `.btn-secondary:hover` with filter effects (not color changes)
- [ ] Update button border-radius: confirm uses `0.75rem` (already correct)
- [ ] Add error/disabled state styles for inputs
- [ ] Remove custom shadow classes (`.shadow-soft`, etc.)

**HTML Changes:**
- [ ] Remove all Tailwind `text-sm`, `text-xs` utilities (use h6 classes instead)
- [ ] Remove inline `px-*`, `py-*` overrides (use standard button padding)
- [ ] Add error/validation markup patterns
- [ ] Import/reference token-based CSS instead of Tailwind

---

## SUMMARY

**Current State:**
- fortunetell-frontend: 22 form tokens, mostly compliant, but with shadow/hover/weight violations
- simple-budget-site: Custom CSS with 10+ out-of-policy colors, shadows, weight 600, <16px text
- Inconsistent button radius (pill vs rounded)
- Missing error/disabled states in both repos
- Missing helper text tokens

**2.0 Target State:**
- Single unified token set (11 base colors, h5/h6 typography, 2 weights)
- All 4 input states defined (default, focus, error, disabled)
- All 5 button states defined (default, hover, active, focus, disabled)
- No shadows (CSS outline for focus instead)
- No out-of-policy hover colors (filter effects instead)
- Consistent border-radius (12px rounded, not pill)
- Helper/validation text tokens for all states

**Migration Impact:**
- fortunetell-frontend: ~15 token changes, ~20 CSS rule updates
- simple-budget-site: ~30+ color replacements, full Tailwind removal, ~25 CSS rule updates
- Visual changes: Buttons become rounded instead of pill-shaped, focus rings become outlines, some colors shift slightly to conform to 11-color palette

**Cross-Surface Alignment:**
- Both repos will use identical form token system
- Marketing forms may vary layout but NOT core styling
- Ensures consistent user experience across landing pages and app

---

**END OF FORMS & INPUTS ANALYSIS**
**Status:** Complete token inventory, completeness check, cross-surface spec documented
**Authority:** DEV_DesignTokensSystem_1.4 (11 colors, no shadows) + Typography constraints (16px floor, 18px body, 400/700 weights)
**Scope:** Analysis/verification/documentation only - no code changes yet
