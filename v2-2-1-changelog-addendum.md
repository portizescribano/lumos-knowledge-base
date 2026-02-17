# Lumos Framework v2.2.1 Update - Changelog Addendum

**Release Date:** October 23, 2025  
**Release Type:** Minor Release  
**Status:** Current Version

---

## Overview

This document captures specific updates introduced in Lumos v2.2.1 that may not be fully detailed in the main reference documentation. Use this as a supplement to the complete reference.

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

### Updated Utilities

**u-bg-transition**
- Updated to use color-only transparency
- More performant than background-color transitions
- Maintains visual consistency with fade effects

**Height & Width Utilities**
- `height: 100vh` utility added
- `width: 100%` utility added
- Improved full-viewport and full-width controls

### Removed Utilities

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

### Layout Component (NEW)

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

<!-- NEW (v2.2.1) -->
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
- New utilities/variants won't work in older versions
- Remove v2.2.1-specific features before downgrading

---

## Known Issues & Limitations (v2.2.1)

**None reported as of January 20, 2026.**

If you encounter issues:
1. Check official changelog: https://lumos.timothyricks.com/changelog
2. Verify framework version matches expectations
3. Review component property bindings
4. Check for conflicting custom code

---

## Resources

**Official Links:**
- Changelog: https://lumos.timothyricks.com/changelog
- Documentation: https://lumos.timothyricks.com
- Version Docs: https://timothyricks.notion.site/Lumos-v2-2-1

**Framework Files:**
- Cloneable: https://try.webflow.com/tricks?path=lumos-v2-beta
- Preview: https://preview.webflow.com/preview/lumos-v2-beta

---

## Quick Reference - What Changed

**✅ Added:**
- u-fade-in utility
- u-hover-opacity utility
- u-transition-variable utility
- u-clickable_wrapper utility
- Layout component
- Multiple aspect ratio variants (Visual Image/Video)
- Auto-margin utility application
- Rich text clamp support

**🔄 Updated:**
- Section component variant naming
- Modal variant property naming
- u-bg-transition (color-only)
- Global Styles automation

**❌ Removed:**
- Default u-container/u-wrapper from sections
- Form Input pattern attribute (from defaults)
- u-clickable_elem:cursor utility
- background-size from Content Wrapper default

---

## Integration with Main Reference

**This addendum supplements:** lumos-complete-reference.md

**When to use this document:**
- Implementing v2.2.1-specific features
- Migrating from v2.2.0 to v2.2.1
- Troubleshooting v2.2.1 behavior
- Understanding changelog updates

**Main reference covers:**
- Core framework concepts (still accurate)
- Class naming system (unchanged)
- Component architecture (core principles same)
- Variables system (unchanged)
- Typography/Color systems (unchanged)
- Breakpointless approach (unchanged)

---

**Last Updated:** January 20, 2026  
**Framework Version:** v2.2.1  
**Addendum Version:** 1.0
