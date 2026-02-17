# Lumos Framework v2.2.2 Update - Changelog Addendum

**Release Date:** January 31, 2026  
**Release Type:** Minor Release  
**Status:** Current Version

---

## Overview

This document captures updates introduced in Lumos v2.2.2 and v2.2.1, supplementing the main reference documentation. Use this for version-specific features, changes, and migration guidance.

---

## v2.2.2 - January 31, 2026

**Theme:** Webflow Native Features Integration

This minor release integrates Webflow's new platform features (Conditional Visibility and Switch props) into Lumos components, streamlining component properties and removing deprecated utilities.

---

### Webflow Introduces New Conditional Visibility

Webflow's Conditional Visibility feature allows elements to show/hide based on component property values without custom code. Lumos v2.2.2 integrates this into core components.

---

## Components (v2.2.2)

### Updated Components

#### Global Clickable
**Change:** Button tag automatically removes when link not set using Webflow's Conditional Visibility

**Impact:**
- Cleaner markup when clickable functionality disabled
- No manual visibility management needed
- Better semantic HTML output

**Before (v2.2.1):**
```html
<div class="clickable_wrap">
    <button style="display: none;">Button</button>
    <a href="#">Link</a>
</div>
```

**After (v2.2.2):**
```html
<div class="clickable_wrap">
    <!-- Button removed via Conditional Visibility when link set -->
    <a href="#">Link</a>
</div>
```

#### Modal
**Change:** Updated true/false props to use Webflow's new Switch prop

**Impact:**
- Native toggle interface instead of text input
- Clearer true/false state representation
- Better UX for editors in Build Mode
- Type safety (can't enter invalid values)

**Migration:** Existing instances continue working. New instances use Switch interface.

#### Slider
**Change:** Updated true/false props to use Webflow's new Switch prop

**Same benefits as Modal component above.**

#### Tabs
**Change:** Updated true/false props to use Webflow's new Switch prop

**Same benefits as Modal component above.**

#### Accordion
**Change:** Updated true/false props to use Webflow's new Switch prop

**Same benefits as Modal component above.**

---

### New Components (v2.2.2)

#### Dropdown
**New component added to Lumos core library**

**Purpose:** Standard dropdown/select menu component with consistent styling

**Usage:** Provides dropdown functionality across Lumos projects

**Properties:** (Details available in official documentation when released)

---

## Utility Classes (v2.2.2)

### Updated Utilities

#### u-spacer
**Change:** Now has default top and bottom padding that automatically removes when spacer component added inside

**Behavior:**
```css
.u-spacer {
    padding-top: var(--space-default);
    padding-bottom: var(--space-default);
}

.u-spacer:has(.spacer_component) {
    padding-top: 0;
    padding-bottom: 0;
}
```

**Impact:**
- Spacer divs work automatically with default padding
- Adding Spacer component inside overrides with component-specific spacing
- More flexible spacing control

**Usage:**
```html
<!-- Auto-padding when empty -->
<div class="u-spacer"></div>

<!-- Component padding when spacer inside -->
<div class="u-spacer">
    <Spacer Component />
</div>
```

---

### Removed Utilities (v2.2.2)

**Order & Display Utilities Cleanup**

Approximately 50 order and display utilities removed. These were redundant with container query-based responsive utilities and Webflow's native controls.

**Removed utilities include:**
- u-order-first-mobile
- u-order-last-medium
- u-order-space-medium
- u-order-first-small
- u-order-last-small
- u-order-first-xsmall
- u-order-last-xsmall
- u-order-auto-small
- u-display-contents-medium
- u-display-contents-small
- u-display-none-medium
- u-display-block-small
- u-display-block-medium
- u-display-grid-medium
- u-display-grid-small
- u-display-flex-small
- u-display-flex-medium
- u-display-none-small
- *(~30 additional order/display utilities)*

**Replacement Strategy:**

Use responsive container query utilities:
```html
<!-- Instead of u-order-first-small -->
<div data-small-columns="1" style="order: -1;">Content</div>
```

Or custom CSS with container queries:
```css
@container (width < 35em) {
    .my-element { order: -1; }
}
```

**Migration:**
- Review projects using removed utilities
- Replace with data attributes: `[data-medium-columns]`, `[data-small-columns]`
- Use custom CSS with container queries for complex ordering
- Webflow's native responsive controls also available

---

### New Utilities (v2.2.2)

#### u-alignment-inherit
**Purpose:** Allows elements to inherit alignment from parent context

**Usage:**
```html
<div class="u-alignment-center">
    <div class="child u-alignment-inherit">
        <!-- Inherits center alignment from parent -->
    </div>
</div>
```

**Use Cases:**
- Nested components matching parent alignment
- Flexible layouts adapting to container alignment
- Reducing alignment class duplication

---

## Variables (v2.2.2)

### Removed Variables

#### Button Style Collection
**Major Change:** Buttons now reference Theme collection directly

**Before (v2.2.1 and earlier):**
```
Button Style Collection
├── Primary (references Theme/button-primary)
├── Secondary (references Theme/button-secondary)
└── Tertiary (references Theme/button-tertiary)
```

**After (v2.2.2):**
Buttons reference Theme collection directly:
```
Theme Collection
├── button-primary/background
├── button-primary/text
├── button-secondary/background
├── button-secondary/text
└── button-tertiary/background
```

**Impact:**
- One less layer of variable indirection
- Simpler variable structure
- Easier theme customization
- Existing projects remap automatically (backward compatible)

**Usage:**
```css
/* v2.2.2 approach - direct reference */
.button {
    background: var(--theme--button-primary--background);
    color: var(--theme--button-primary--text);
}
```

**Migration:**
- New projects: Reference Theme collection for button variables
- Existing projects: No action required (backward compatible)

---

### New Variables (v2.2.2)

**Font Size Variables**

Added granular font size variables for flexible typography control:

- `--font-micro` - Smallest text size
- `--font-xsmall` - Extra small text
- `--font-small` - Small text
- `--font-medium` - Medium text (base)
- `--font-large` - Large text
- `--font-xlarge` - Extra large text

**Purpose:**
- More typography size options
- Better semantic variable naming
- Consistent size scale across projects

**Usage:**
```css
.text-micro { font-size: var(--font-micro); }
.text-base { font-size: var(--font-medium); }
.text-display { font-size: var(--font-xlarge); }
```

**Integration with Text Styles:**
Use in combination with existing u-text-style utilities for complete typography control.

---

## Migration Guide: v2.2.1 → v2.2.2

### Breaking Changes

**None.** This is a fully backward-compatible minor release.

**Deprecations:**
- Order utilities removed (use container queries or data attributes)
- Button Style variable collection removed (use Theme collection directly)

---

### Automatic Migrations (No Action Required)

✅ **Component property changes** - Switch props work with existing boolean values  
✅ **Button Style variables** - Automatically remap to Theme collection  
✅ **Global Clickable visibility** - Conditional Visibility applies automatically  
✅ **u-spacer behavior** - Works automatically with or without spacer component

---

### Manual Migrations (Action Required)

#### 1. Replace Removed Order Utilities

**Find instances of:**
```html
<div class="u-order-first-mobile">Content</div>
<div class="u-display-none-small">Content</div>
```

**Replace with data attributes:**
```html
<div data-xsmall-columns="1" style="order: -1;">Content</div>
<div class="u-display-none-small">Content</div> <!-- Use custom CSS -->
```

**Or custom CSS with container queries:**
```css
@container (width < 35em) {
    .my-element { 
        order: -1;
        display: none; 
    }
}
```

#### 2. Review Button Styling

If you have custom button code referencing Button Style variables:

**Old approach (still works but deprecated):**
```css
.custom-button {
    background: var(--button-style--primary--background);
}
```

**New approach (v2.2.2+):**
```css
.custom-button {
    background: var(--theme--button-primary--background);
}
```

#### 3. Test Component Switch Props

For Modal, Slider, Tabs, Accordion components:
- Open component instances
- Verify Switch props display correctly
- Test toggling on/off
- Confirm behavior matches expectations

---

## Best Practices (v2.2.2)

### Component Properties

**DO:**
- ✅ Use Switch props for boolean values (Modal, Tabs, Slider, Accordion)
- ✅ Leverage Conditional Visibility for dynamic element display
- ✅ Reference Theme collection directly for button styling
- ✅ Use new u-alignment-inherit for nested alignment

**DON'T:**
- ❌ Don't use removed order utilities (migrate to alternatives)
- ❌ Don't create custom Button Style variables (use Theme collection)
- ❌ Don't manually toggle visibility (use Conditional Visibility)

---

### Variable Structure

**Simplified Button Theming:**
```css
/* v2.2.2 - Direct Theme reference (RECOMMENDED) */
.button-primary {
    background: var(--theme--button-primary--background);
    color: var(--theme--button-primary--text);
}

/* Use new font size variables */
.heading-large {
    font-size: var(--font-xlarge);
}
```

---

### Responsive Design

**Use Container Queries & Data Attributes:**
```html
<div 
    class="layout"
    data-large-columns="4"
    data-medium-columns="2"
    data-small-columns="1"
>
    Grid children
</div>
```

**Avoid removed order utilities** - Use custom CSS with container queries:
```css
@container (width < 50em) {
    .element { order: 2; }
}
```

---

## Resources (v2.2.2)

**Official Documentation:**
- [Lumos Framework Documentation](https://lumos.timothyricks.com)
- [Changelog](https://lumos.timothyricks.com/changelog)
- [Webflow Conditional Visibility Guide](https://webflow.com)
- [Webflow Switch Props Documentation](https://webflow.com)

**Framework Files:**
- Cloneable: https://try.webflow.com/tricks?path=lumos-v2-beta
- Preview: (Check official site for v2.2.2 preview link)

---

## Quick Reference - What Changed in v2.2.2

**✅ Added:**
- Webflow Conditional Visibility integration (Global Clickable)
- Switch props for boolean properties (Modal, Slider, Tabs, Accordion)
- Dropdown component (new)
- u-alignment-inherit utility
- 6 new font size variables (--font-micro through --font-xlarge)
- Auto-padding behavior for u-spacer

**🔄 Updated:**
- Global Clickable uses Conditional Visibility
- Modal, Slider, Tabs, Accordion use Switch props
- u-spacer has smart padding behavior
- Button variables reference Theme collection directly

**❌ Removed:**
- ~50 order and display utilities (migrate to container queries)
- Button Style variable collection (use Theme collection directly)

---

## v2.2.1 - October 23, 2025

*[Previous v2.2.1 changelog content remains below for reference...]*

---

## Overview (v2.2.1)

v2.2.1 introduced new utility classes, component updates, and Visual component aspect ratio variants.

---

## New Utility Classes (v2.2.1)

### Animations & Transitions

**u-fade-in**
- Fade animation for sections
- Applied to Section component
- Uses color-only transparency instead of background-color when promoted to 100% opacity
- Enables smooth fade-in effects without additional code

**u-hover-opacity**
- Cursor opacity control on hover
- Applied to interactive elements
- Provides visual feedback for clickable items

**u-transition-variable**
- Layout component transition control
- Connected to variant system
- Enables smooth transitions between layout states
- Applied via `data-variant` attribute

**u-clickable_wrapper**
- Background cursor styling for clickable wrappers
- Applied via `data-wrapper` attribute
- Enhances clickable area visual feedback

### Updated Utilities (v2.2.1)

**u-bg-transition**
- Updated to use color-only transparency
- More performant than background-color transitions
- Maintains visual consistency with fade effects

**Height & Width Utilities**
- `height: 100vh` utility added
- `width: 100%` utility added
- Improved full-viewport and full-width controls

### Removed Utilities (v2.2.1)

**u-clickable_elem:cursor[data-actionable]**
- Removed in favor of cleaner cursor system
- Replaced with u-hover-opacity and u-clickable_wrapper pattern

---

## Component Updates (v2.2.1)

### Global Styles Component

**Margin Utilities Auto-Application:**
- Margin utilities now automatically apply to:
  - `u-container` class
  - `u-wrapper` class
  - Any class containing text like `hero-text-wrapper` or `footer-text-wrapper`
- Reduces manual class application
- More consistent spacing across components

**Grid Auto-fit Code:**
- Removed manual code for `grid-auto-fit` class inclusion
- Now handled automatically by component

**Rich Text Clamp:**
- Upgraded clamp code to work with rich text elements
- Better text overflow handling in CMS content

**Filter Empty State:**
- Automatically hides `filter-empty-state` classes when empty
- Cleaner UI for filtered lists with no results

### Content Wrapper Component

**Property Updates:**
- `background-size` removed from default value of class prop
- Cleaner default state
- Background sizing now explicit when needed

### Section Component

**Variant Naming:**
- Variant names standardized
- Old "Inherit" renamed to "Variant"
- More consistent naming across all components

**Fade-in Support:**
- `u-fade-in` class added to background slot
- Enables smooth section entrance animations
- Color-only transparency for better performance

**Unique Variants:**
- Updated all variants to have unique ordinal values
- Improves variant switching reliability
- Better CSS specificity control

**Removed Defaults:**
- `u-container` and `u-wrapper` removed from default container class prop values
- Must be explicitly added when needed
- More flexible component configuration

### Modal Component

**Variant Property:**
- Variant Name renamed from "Size" to "Variant"
- Consistent with other components
- Clearer property purpose

### Tab Component

**CSS Embed:**
- Added CSS embed for horizontal variant native styles
- Replaces custom code approach
- More maintainable and performant

### Visual Image Component

**New Aspect Ratio Variants:**
- Wide 7-1
- Wide 16-9
- Wide 8-3
- Wide 7-2
- Wide 5-4
- Wide 5-2
- Square 1-1
- Tall 4-5
- Tall 2-3
- Tall 3-7

**Usage:** Select from variant dropdown to set image aspect ratio
**Benefit:** Consistent image sizing across site without custom CSS

### Visual Video Component

**New Aspect Ratio Variants:**
- Wide 7-2
- Wide 16-9
- Wide 8-3
- Wide 5-2
- Wide 5-4
- Square 1-1
- Tall 4-5
- Tall 2-3
- Tall 3-7

**Usage:** Select from variant dropdown to set video aspect ratio
**Benefit:** Maintains video proportions responsively

### Form Input Component

**Attribute Removal:**
- `pattern` attribute removed from default
- Set explicitly when validation needed
- Cleaner default form inputs

### Layout Component (NEW in v2.2.1)

**Purpose:** Global style control variants for layout patterns

**Features:**
- Variants for different layout styles
- Global style control
- Reusable layout patterns

**Usage:** Use instead of creating custom layout divs for common patterns

---

## Component Best Practices (v2.2.1)

### Section Component Usage

**Fade-in Animation:**
```html
<section class="section_wrap u-fade-in">
  <!-- Section content -->
</section>
```

**Explicit Container:**
```html
<!-- OLD (v2.2.0 and earlier) -->
<section class="section_wrap">
  <div class="section_container">
    <!-- Container was default -->
  </div>
</section>

<!-- NEW (v2.2.1+) -->
<section class="section_wrap">
  <div class="section_container u-container">
    <!-- Must explicitly add u-container -->
  </div>
</section>
```

### Visual Component Aspect Ratios

**Consistent Image Sizing:**
```html
<!-- Use variant instead of custom CSS -->
<Visual Image variant="Wide 16-9">
  <!-- Image maintains 16:9 ratio -->
</Visual Image>

<!-- Responsive video embeds -->
<Visual Video variant="Wide 16-9">
  <!-- Video maintains 16:9 ratio -->
</Visual Video>
```

### Transition Effects

**Layout Transitions:**
```html
<div class="layout_wrap u-transition-variable" data-variant="horizontal">
  <!-- Smooth transition when variant changes -->
</div>
```

**Hover Effects:**
```html
<div class="card_wrap u-hover-opacity">
  <!-- Opacity changes on hover -->
</div>
```

---

## Migration Notes (v2.2.0 → v2.2.1)

### Breaking Changes

**None.** All changes are additive or rename for clarity.

### Recommended Updates

**1. Section Components:**
- Review sections using container/wrapper classes
- Explicitly add `u-container` or `u-wrapper` if needed (no longer default)
- Test that spacing remains correct

**2. Modal Components:**
- Property name changed from "Size" to "Variant"
- No functional change, just rename for consistency
- Update any documentation referencing "Size" property

**3. Form Inputs:**
- Pattern attribute removed from defaults
- Add explicitly if using HTML5 validation patterns
- Check any forms relying on default patterns

**4. Visual Components:**
- New aspect ratio variants available
- Consider using variants instead of custom CSS for aspect ratios
- More consistent and maintainable

### Optional Enhancements

**Add Fade Animations:**
```html
<!-- Enhance section entrances -->
<section class="hero_wrap u-fade-in">
  ...
</section>
```

**Use New Aspect Ratios:**
```html
<!-- Replace custom ratio CSS -->
<Visual Image variant="Wide 16-9" />
<Visual Video variant="Wide 16-9" />
```

**Implement Hover Effects:**
```html
<!-- Add opacity hover to cards -->
<div class="card_wrap u-hover-opacity">
  ...
</div>
```

---

## Utility Class Reference (v2.2.1 Additions)

### Complete List of New/Updated Classes

```css
/* New in v2.2.1 */
u-fade-in                    // Section fade animation
u-hover-opacity              // Hover opacity effect
u-transition-variable        // Layout transition control
u-clickable_wrapper          // Clickable wrapper styling

/* Updated in v2.2.1 */
u-bg-transition             // Color-only transparency
height: 100vh               // Full viewport height
width: 100%                 // Full width

/* Removed in v2.2.1 */
u-clickable_elem:cursor[data-actionable]  // Use u-hover-opacity instead
```

---

## Framework Version Compatibility

**v2.2.2 Compatibility:**
- Fully backward compatible with v2.2.1
- Projects on v2.2.1 can upgrade without breaking changes
- New features are opt-in
- Removed utilities require migration

**v2.2.1 Compatibility:**
- Fully backward compatible with v2.2.0
- Projects on v2.2.0 can upgrade without changes
- New features are opt-in

**Upgrading from v2.1.x:**
- Review changelog for v2.2.0 changes first
- Test thoroughly before upgrading production sites
- New features require explicit implementation

**Downgrading:**
- Not recommended
- v2.2.2-specific features won't work in older versions
- Remove Switch props and new utilities before downgrading

---

## Known Issues & Limitations

**None reported as of February 6, 2026.**

If you encounter issues:
1. Check official changelog: https://lumos.timothyricks.com/changelog
2. Verify framework version matches expectations
3. Review component property bindings
4. Check for conflicting custom code

---

## Integration with Main Reference

**This addendum supplements:** lumos-complete-reference.md

**When to use this document:**
- Implementing v2.2.2 or v2.2.1-specific features
- Migrating between versions
- Troubleshooting version-specific behavior
- Understanding changelog updates

**Main reference covers:**
- Core framework concepts (still accurate)
- Class naming system (unchanged)
- Component architecture (core principles same)
- Variables system (structure unchanged)
- Typography/Color systems (foundation unchanged)
- Breakpointless approach (unchanged)

---

**Last Updated:** February 6, 2026  
**Current Framework Version:** v2.2.2  
**Addendum Version:** 2.0
