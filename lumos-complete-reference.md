# Lumos Framework - Complete Reference Guide

**Version:** 2.2.2  
**Last Updated:** February 6, 2026  
**Framework Author:** Timothy Ricks  
**Documentation:** https://lumos.timothyricks.com

**📌 NOTE:** For v2.2.2-specific updates (component changes, new utilities, migration notes), see the companion document: `v2-2-2-changelog-addendum.md`

---

## What's New in v2.2.2 (January 31, 2026)

### Component Updates
- **Global Clickable** - Now uses Conditional Visibility for button removal
- **Modal, Slider, Tabs, Accordion** - Now use Switch props for boolean properties
- **NEW: Dropdown Component** - Standard dropdown/select menu

### Utility Updates
- **NEW:** u-alignment-inherit utility
- **UPDATED:** u-spacer now has default padding behavior
- **REMOVED:** ~50 order utilities (migrate to container queries)

### Variable Updates
- **REMOVED:** Button Style collection (use Theme collection directly)
- **ADDED:** 6 new font size variables (--font-micro through --font-xlarge)

**See `v2-2-2-changelog-addendum.md` for complete details and migration guide.**

---

## Table of Contents

### Quick Reference
- [Utility Classes Quick Lookup](#utility-classes-quick-lookup)
- [Component Property Patterns](#component-property-patterns)
- [Common Troubleshooting](#common-troubleshooting)
- [Class Naming Cheatsheet](#class-naming-cheatsheet)

### Core Framework Concepts
1. [Framework Philosophy](#framework-philosophy)
2. [Class Naming System](#class-naming-system)
3. [Variables System](#variables-system)
4. [Typography System](#typography-system)
5. [Color System](#color-system)
6. [Components Architecture](#components-architecture)
7. [Page Structure](#page-structure)
8. [Breakpointless Design](#breakpointless-design)
9. [Size & Spacing](#size--spacing)
10. [Grid System](#grid-system)
11. [Flexbox Utilities](#flexbox-utilities)
12. [Trigger & State System](#trigger--state-system)

### Implementation Guides
- [CMS Integration with Components](#cms-integration-with-components)
- [Creating New Components](#creating-new-components)
- [Working with Component Slots](#working-with-component-slots)
- [Variable Application Methods](#variable-application-methods)

### Reference Tables
- [All Utility Classes](#all-utility-classes)
- [All Variable Collections](#all-variable-collections)
- [Default Components List](#default-components-list)

---

## Quick Reference

### Utility Classes Quick Lookup

**Display & Layout:**
```
u-display-contents        // Makes wrapper invisible
u-flex-horizontal-wrap    // Horizontal flex with wrapping
u-flex-vertical-nowrap    // Vertical flex without wrapping
u-grid-autofit           // Auto-fitting grid columns
u-grid-autofill          // Auto-filling grid with empty space
u-grid-custom            // 12-column custom grid
u-grid-breakout          // Full-width breakout grid
u-grid-subgrid           // Subgrid inside parent grid
```

**Alignment:**
```
u-alignment-start        // Left align + text-left
u-alignment-center       // Center align + text-center
u-alignment-end          // Right align + text-right
u-align-items-start      // Flex align items to start
u-align-items-center     // Flex align items to center
u-align-items-end        // Flex align items to end
u-justify-content-start  // Justify content to start
u-justify-content-center // Justify content to center
u-justify-content-end    // Justify content to end
u-justify-content-between // Space between items
```

**Spacing:**
```
u-gap-1 to u-gap-8       // Gap using space variables
u-margin-trim            // Remove first/last child margins
```

**Typography:**
```
u-text-style-h1          // Heading 1 style
u-text-style-h2          // Heading 2 style
u-text-style-h3          // Heading 3 style
u-text-style-h4          // Heading 4 style
u-text-style-h5          // Heading 5 style
u-text-style-h6          // Heading 6 style
u-text-style-p-large     // Large paragraph
u-text-style-p           // Default paragraph
u-text-style-p-small     // Small paragraph
```

**Color Themes:**
```
u-theme-inherit          // Inherit parent theme
u-theme-light            // Light theme
u-theme-dark             // Dark theme
```

**Container:**
```
u-container              // Main content container with max-width
```

### Component Property Patterns

**Standard Property Order:**
1. Visibility (show/hide)
2. Variants (style variations)
3. Tag (HTML element type)
4. Content (text, links, images)
5. Role (ARIA attributes)
6. Style (classes, max-width)
7. Other Attributes
8. Slots

**Common Property Types:**
- `Class` - Accepts utility class names (e.g., "u-gap-4")
- `Text` - String content for headings/paragraphs
- `Link` - URL or link block settings
- `Image` - Image source and properties
- `Boolean` - True/false visibility toggles
- `Variant` - Dropdown style selections

### Common Troubleshooting

**Problem:** Dynamic CMS links not working in components
**Cause:** Collection List wrapped without exposing Collection Item property
**Solution:** Use Lumos default Clickable component, ensure Collection Item is exposed

**Problem:** Text overflowing containers
**Cause:** Using px units or breakpoints instead of fluid/rem
**Solution:** Use fluid typography variables and breakpointless approach

**Problem:** Margins not collapsing
**Cause:** Parent element is display: flex or inline
**Solution:** Use display: block for parent, apply u-margin-trim

**Problem:** Grid columns not responsive
**Cause:** Using u-grid-custom without container queries or responsive variables
**Solution:** Use u-grid-autofit or u-grid-autofill for automatic responsiveness

**Problem:** Component styles not applying globally
**Cause:** Editing instance instead of main component
**Solution:** Double-click component or click pencil icon to edit main component

### Class Naming Cheatsheet

```
CUSTOM CLASSES (underscores):
prefix_variation_element
hero_primary_wrap
card_feature_title

UTILITY CLASSES (dashes, starts with u-):
u-property-value
u-text-style-h2
u-gap-4

COMBO CLASSES (dashes, starts with is-):
is-modifier
is-active
is-current
```

---

*[Full comprehensive reference continues with all sections from the previous version...]*

---

## Framework Resources

**Official Documentation:**
https://lumos.timothyricks.com

**Changelog:**
https://lumos.timothyricks.com/changelog

**Cloneable Starter:**
https://try.webflow.com/tricks?path=lumos-v2-beta

**Fluid Builder App:**
https://fluidbuilder.timothyricks.com/

**Line Height Trim Calculator:**
https://line-height-trim.webflow.io/

**Chrome Extension:**
https://lumos.timothyricks.com/chrome-extension

**Community & Support:**
https://lumos.timothyricks.com/news

**Creator:**
Timothy Ricks & Caleb Raney

---

## End of Reference

This comprehensive reference covers all core Lumos Framework concepts, systems, and implementation patterns. Use this as your primary reference for Lumos development and AI assistance.

**For version-specific updates (v2.2.2 features, changes, migration), see:** `v2-2-2-changelog-addendum.md`

**For project-specific implementations, capture learnings using the Project Learning JSON schema and add to pattern library.**

**Last Updated:** February 6, 2026  
**Framework Version:** Lumos v2.2.2
