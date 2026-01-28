

## **DEV\_DesignArchitectureAddendum\_10.2.1\_20260122**

**Icon Infrastructure Canonicalization & Token Correction**

### **0\. Status and Intent**

This addendum:

1. Fills gaps in V10.2 around icon infrastructure where V10 was previously silent.  
2. Freezes the **current implementation pattern** (as of 2026-01-22) as the canonical icon infrastructure for the 10.2 line.  
3. Clarifies the correct **token hierarchy for icon colors**, and records that the current implementation violates that hierarchy and must be corrected in a small, scoped change.

This document is authoritative for icon-related architectural decisions in the 10.2 line unless superseded by a later addendum.

---

### **1\. Scope**

This addendum applies to:

* The FortuneTell **frontend** codebase (`/frontend`).  
* All icon components under `src/icons/`.  
* All usage of icons in application code, including:  
  * Dock / primary navigation  
  * Headers and primary actions  
  * Panels, overlays, dialogs, and inline actions

Backend code, marketing surfaces, and other properties are out of scope.

---

### **2\. Gaps Previously Unspecified in V10.2**

V10.2 core and prior addenda did **not** specify:

* A canonical **icon library** (Material vs anything else).  
* A required **implementation pattern** (base component vs wrappers vs raw SVG).  
* Naming conventions for icon components.  
* SVG concerns such as:  
  * Required `viewBox`  
  * Inline `<path>` vs external sources  
  * Where SVGs should live in the tree

This addendum fills those gaps by treating the **current implementation** (as of 2026-01-22) as the canonical icon infrastructure.

---

### **3\. Canonical Icon Infrastructure (Current Implementation Is Spec)**

#### **3.1 Icon Source and Location**

**Current and canonical for 10.2:**

1. **Icon source**  
* Icons are based on **Material Design** glyphs, represented as inline SVG paths.  
* All icons use `viewBox="0 0 24 24"` (standard Material 24×24 grid).  
2. **Location & structure**  
* All UI icons live under `src/icons/`, one component per file.  
* Naming convention for icon components:  
  `src/icons/[Name]Icon.tsx`  
  Examples: `HomeIcon`, `WalletIcon`, `SettingsIcon`, `HistoryIcon`, etc.  
3. **Library dependency**  
* There is **no runtime dependency** on Material Icon fonts or external icon libraries (no webfont, no sprite sheet).  
* Icon glyphs are stored as inline `<path>` data in these components.

This pattern is frozen as canonical for the 10.2 line.

Note: This addendum does not decide whether a future base icon abstraction will exist. It only states that the current “wrapper-only” pattern is canonical for 10.2.

#### **3.2 Wrapper Component Pattern (Canonical)**

Every icon component in `src/icons/` follows the same pattern:

import type React from 'react';

export function \[IconName\](props: React.SVGProps\<SVGSVGElement\>) {  
  return (  
    \<svg  
      viewBox="0 0 24 24"  
      fill="currentColor"  
      aria-hidden="true"  
      {...props}  
    \>  
      \<path d="\[Material Design icon path data\]" /\>  
    \</svg\>  
  );  
}

Normative characteristics:

* **Typing**  
  * Signature: `export function [IconName](props: React.SVGProps<SVGSVGElement>)`.  
  * Props are forwarded onto `<svg>` via `{...props}`.  
* **Geometry**  
  * `viewBox="0 0 24 24"` is required for all icons.  
* **Color**  
  * `fill="currentColor"` must be used on `<svg>` so that color is controlled by CSS/tokens, not hard-coded per icon.  
* **Accessibility**  
  * `aria-hidden="true"` is required by default. Icons are decorative; semantics are provided by the surrounding button/link/etc.  
  * If any icon is made semantic, that must be a documented exception in a future addendum.  
* **Glyph embedding**  
  * One or more `<path>` elements, with `d` coming from Material glyph data.  
  * Multiple `<path>`s are allowed where the Material glyph requires them; wrapper pattern itself must remain intact.  
* **Base component**  
  * There is **no base icon component** (`BaseIcon`, `MaterialIcon`, etc.) in the current code.  
  * For the 10.2 line, standalone wrappers in `src/icons/` are the canonical pattern; any future base abstraction would require a dedicated architecture decision and addendum.

#### **3.3 Sizing – Tokens and Utilities**

Icon size is controlled exclusively by **design tokens** via global size utilities.

1. **Token intention**  
* `--icon-size-sm`: small inline icons.  
* `--icon-size-md`: secondary/inline control icons.  
* `--icon-size-lg`: large/system icons (dock, major actions, etc.).  
2. **Global utilities**

Located in `src/styles/globals.css`:

.icon-sm { width: var(--icon-size-sm); height: var(--icon-size-sm); }  
.icon-md { width: var(--icon-size-md); height: var(--icon-size-md); }  
.icon-lg { width: var(--icon-size-lg); height: var(--icon-size-lg); }

Normative rules:

* Icons in application code must derive width/height only via `.icon-sm`, `.icon-md`, or `.icon-lg`.  
* For dock icons and other “system-scale” surfaces, `.icon-lg` is the canonical size for the 10.2 line.  
* Inline `width`, `height`, or `style={{ width/height }}` on icons are not allowed.

#### **3.4 Color – High-Level Rules**

High-level rules for icon color:

* Icon color must be driven by design tokens, not by hard-coded values.  
* Color is applied via:  
  * Token-backed global icon color utilities (e.g. `.icon-default`, `.icon-strong`, `.icon-inverse`, `.icon-danger`), and/or  
  * Inheriting `currentColor` from a parent element that itself respects the token-based color system.

Any inline `fill`, hard-coded `color` values, or non-token-based color sources on the icon are disallowed.

Detailed corrections to the token chain are defined in Section 4\.

---

### **4\. Token Hierarchy Correction for Icon Colors**

#### **4.1 Design Error (Existing Implementation)**

Analysis exposed a design error in the icon color tokens:

* `--icon-color-default` and related icon color tokens currently derive from **app-level text tokens** (e.g. `--app-text-secondary`).  
* This is incorrect from an architectural standpoint:  
  color semantics should be defined at the **root theme layer**, and app-level tokens should derive from theme tokens, not the other way around.

In other words:

* Current chain is effectively:  
  `icon → app semantic text token → theme`  
* Correct chain should be:  
  `icon → theme → (optionally reused by app text tokens)`

This is a spec violation introduced by prior AI-generated design decisions; it is not the intended architecture.

#### **4.2 Correct Token Architecture (Canonical Spec)**

For the 10.2 line, the **correct architectural rule** is:

1. **Root theme tokens are the only authoritative source of color.**  
   * Theme-level color tokens (e.g. `--color-fg-default`, `--color-fg-muted`, `--color-fg-strong`, etc.) must be defined at `:root` and represent the palette.  
2. **App-level semantic tokens (e.g. `--app-text-*`) and icon-level tokens must derive from theme tokens.**  
   * App text tokens:  
     `--app-text-*` → `--color-fg-*` (theme)  
   * Icon color tokens:  
     `--icon-color-*` → `--color-fg-*` (theme)  
   * `--icon-color-*` must **not** depend on `--app-text-*`. Both are peers under the theme palette, not parents of each other.  
3. **Utilities must follow that chain.**  
   * `.icon-default`, `.icon-strong`, `.icon-inverse`, `.icon-danger`, etc., must be backed by `--icon-color-*` tokens.  
   * No icon utility may directly reference app-level tokens.

#### **4.3 Required Correction (Architectural, Not Executed Here)**

This addendum records that:

* The current encoded values of `--icon-color-*` that derive from `--app-text-*` are architecturally wrong.  
* A small, focused correction is required in a separate implementation step to:  
  * Introduce or confirm the proper root theme color tokens.  
  * Re-point `--icon-color-*` to those theme tokens.  
  * Ensure app text tokens derive from theme tokens, not vice versa.

This addendum does **not** execute that change; it only defines the correct architecture and declares the current state non-compliant.

---

### **5\. Canonical Usage Examples (Current Implementation)**

These examples are not new rules; they document current patterns that conform to this addendum and can be used as references.

#### **5.1 DockBar – Primary Navigation**

* File: `src/components/DockBar.tsx`  
* Each dock item:  
  * Renders exactly one icon component from `src/icons/`.  
  * Uses `className={`icon-lg ${styles.icon}`}` or an equivalent pattern:  
    * `icon-lg` → `--icon-size-lg` (canonical dock size).  
    * `styles.icon` handles layout (e.g. `flex-shrink: 0`).  
  * Relies on `fill="currentColor"` and button/text tokens for color.

This is the canonical usage pattern for dock icons.

#### **5.2 Header Actions – Account / Settings / Main**

* Files:  
  * `src/components/AccountView.tsx`  
  * `src/components/SettingsView.tsx`  
  * `src/app/page.tsx` (main header)  
* Pattern:  
  * Uses wrapper icons from `src/icons/` (e.g. `HomeIcon`, `HistoryIcon`).  
  * Applies `className="icon-lg icon-default"` for header actions.  
  * No inline sizing or colors.

This is canonical for header-scale controls.

#### **5.3 Panels and Overlays**

* Files:  
  * `src/components/HistoryPanel.tsx`  
  * `src/components/AddTransactionPanel.tsx`  
  * `src/components/EditTransactionPanel.tsx`  
  * `src/components/CalendarMonth.tsx`  
  * `src/components/ConfirmDialog.tsx`

Patterns:

* Smaller panel close icons:  
  * Use `CloseIcon` with `icon-md icon-default` \+ module-scoped positioning class.  
* Larger/system actions:  
  * Use `CloseIcon` or `DeleteIcon` with `icon-lg` and color context (e.g. `icon-danger` for destructive actions).

These are canonical examples of how to apply different icon sizes to different surface scales using the token system.

---

### **6\. Summary**

For the 10.2 line:

* The **current inline SVG wrapper pattern in `src/icons/`** is canonical icon infrastructure.  
* Icons are Material glyphs in `viewBox="0 0 24 24"` SVGs that accept `React.SVGProps<SVGSVGElement>`, use `fill="currentColor"`, and are `aria-hidden="true"`.  
* Size is controlled only by `icon-sm/icon-md/icon-lg` → `--icon-size-*` tokens.  
* Color must be controlled by icon color tokens that derive from root theme tokens; the present chain (icon → app text tokens) is a **known architectural error** and must be fixed.