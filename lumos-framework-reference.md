# LUMOS FRAMEWORK - Complete Reference Guide

**Version:** v2.2.1 (October 2025)  
**Documentation Date:** January 20, 2026  
**Framework Author:** Timothy Ricks & Caleb Raney  
**Official Docs:** https://lumos.timothyricks.com

---

## TABLE OF CONTENTS

1. [Quick Reference](#quick-reference)
2. [Framework Overview](#framework-overview)
3. [Class Naming System](#class-naming-system)
4. [Variables System](#variables-system)
5. [Typography System](#typography-system)
6. [Color System](#color-system)
7. [Components Architecture](#components-architecture)
8. [Page Structure](#page-structure)
9. [Breakpointless Design](#breakpointless-design)
10. [Size & Spacing](#size--spacing)
11. [Grid System](#grid-system)
12. [Flexbox System](#flexbox-system)
13. [Trigger & State](#trigger--state)
14. [Utility Classes Reference](#utility-classes-reference)
15. [Common Patterns](#common-patterns)
16. [Troubleshooting](#troubleshooting)

---

## QUICK REFERENCE

### Class Naming Cheat Sheet
- **Custom Classes:** `component_variation_element` (underscores)
- **Utility Classes:** `u-style-name` (starts with `u-`, dashes)
- **Combo Classes:** `is-modifier` (starts with `is-`, dashes)

### Core Principles
1. Every element gets a custom class
2. Utility classes stack on custom classes
3. Variables control all design tokens
4. Components are the building blocks
5. Breakpointless responsive design
6. CMS integration through proper property exposure

### Essential Utilities (Most Common)
```
u-container                    # Max-width container
u-grid-autofit                 # Auto-responsive grid
u-grid-autofill                # Auto-responsive grid (with empty space)
u-text-style-h1 through h6     # Heading styles
u-text-style-p1 through p3     # Paragraph styles
u-theme-light, u-theme-dark    # Theme switching
u-margin-trim                  # Remove first/last child margins
u-display-contents             # Make wrapper invisible
```

### File Structure Essentials
```
Body
└── page_wrap
    ├── Global Styles (component)
    ├── Global Guides (component)
    ├── Nav (component)
    ├── page_main (slot - unique page content)
    └── Footer (component)
```

### Variable Application Methods
1. **Utility class:** Add `u-text-style-h2` to element
2. **Style panel:** Click purple + icon → select variable
3. **Custom properties:** Add property name + `var(--variable-name)`

---

## FRAMEWORK OVERVIEW

### What is Lumos?

Lumos is a cutting-edge framework for building Webflow sites designed with efficiency, scalability, and accessibility at its core. It brings together modern web development best practices in a way specific to Webflow's platform.

### Core Systems

Lumos is built on three interconnected systems:

1. **Variables** - Design tokens that keep the site consistent and globally manageable
2. **Components** - Reusable building blocks for all site elements
3. **Utility Classes** - Pre-built styles for common CSS patterns

### Philosophy

- **Component-First:** Everything is a component, from typography to full sections
- **Variable-Driven:** All design decisions flow through variables
- **Breakpointless:** Responsive design using rem-based fluid values, not px breakpoints
- **Accessible by Default:** Built-in support for font scaling and accessibility
- **Scalable:** Easy to maintain and update across large sites

### Learning Prerequisites

**You should know:**
- How to navigate Webflow interface
- How to create custom classes
- How to use Webflow variable panel
- How to create Webflow Components
- How to setup CMS collections

**Framework will teach you:**
- Lumos class naming conventions
- Variable system architecture
- Typography and color theming
- Component property patterns
- Breakpointless responsive techniques
- CMS integration with components

---

## CLASS NAMING SYSTEM

### Overview

Classes in Lumos serve three distinct purposes:
1. **Custom Classes** - Define element role within a component
2. **Utility Classes** - Apply reusable styles
3. **Combo Classes** - Modify specific custom class instances

### 1. Custom Classes

**Purpose:** Identify an element's role within component structure

**Naming Pattern:** `type_variation_element`
- Type: Component category (e.g., `hero`, `card`, `nav`)
- Variation: Optional subtype (e.g., `secondary`, `featured`)
- Element: Specific part (e.g., `wrap`, `title`, `text`)

**Rules:**
- Use underscores between words
- Maximum 3 underscores
- Always first class on element
- `_wrap` marks start of new component

**Examples:**
```
hero_wrap                      # Hero section wrapper
hero_layout                    # Hero layout container
hero_content                   # Hero content area
hero_title                     # Hero heading
hero_text                      # Hero paragraph

hero_secondary_wrap            # Variation of hero
hero_secondary_title           # Title in secondary hero

card_wrap                      # Card wrapper
card_visual                    # Card image area
card_content                   # Card text area
card_heading                   # Card heading
```

**Child Groups:**
When elements need nesting, create child groups to avoid naming conflicts:

```
footer_wrap
  footer_link_wrap             # Child group
    footer_link_icon           # Element in child group
    footer_link_text           # Element in child group
```

**Exceptions:**
- Slots with `u-display-contents` don't need custom classes
- Simple base components (1-2 elements) may use only utilities

### 2. Utility Classes

**Purpose:** Apply reusable, common CSS patterns

**Naming Pattern:** `u-style-description`
- Always starts with `u-`
- Uses dashes between words
- Describes the style applied

**Rules:**
- Stack on top of custom classes
- Can be used across any component
- Limit to 4 utilities per element maximum
- Listed after custom class

**Examples:**
```
u-container                    # Max-width container
u-text-style-h2                # Heading 2 typography
u-theme-dark                   # Dark color theme
u-grid-autofit                 # Auto-fitting grid
u-gap-4                        # Gap spacing size 4
u-margin-bottom-3              # Bottom margin size 3
```

**Creating New Utilities:**
1. Create on element with no other classes
2. Start with `u-` followed by broadest search term
3. Example: `u-text-` shows all text utilities
4. Add to styleguide page to prevent cleanup removal
5. Reorder in stylesheet using Finsweet Extension (utilities above custom classes)

**Utility Overrides:**
When utility is stacked on custom class, you can override it without affecting other instances:
```
.hero_title.u-text-style-h2 { 
  /* Override only affects this specific combination */
}
```

**Utility Renaming:**
Rename utilities when stacked on custom classes to swap styles. Overrides remain in place.

If Webflow says "already exists," rename to `u-temp` first, then rename to target.

### 3. Combo Classes

**Purpose:** Modify specific instances of custom classes

**Naming Pattern:** `is-modifier`
- Always starts with `is-`
- Uses dashes between words
- Describes the modification

**Rules:**
- Created on top of component class (not utilities)
- Only affects that specific combination
- Used for state changes or variations

**Examples:**
```
.card_wrap.is-featured         # Featured card variant
.nav_link.is-active            # Active navigation link
.button.is-disabled            # Disabled button state
.hero_layout.is-reversed       # Reversed layout
```

---

## VARIABLES SYSTEM

### Overview

Variables are reusable values used throughout the design system. They provide:
- Global consistency
- Easy site-wide updates
- Scalable design system
- Fluid responsive values

Variables are managed in two places:
1. **Webflow Variables Panel** - Most variables
2. **Custom Code Component** - Some advanced variables

### Variable Collections

#### Default Collection
**Core site-wide values:**
- Swatches (raw color values)
- Max Width, Gutter, Margin
- Border Radius
- Base spacing values

#### Typography Collection
**Font system values:**
- Font families
- Font weights
- Font sizes (fluid)
- Line heights
- Letter spacing
- Text transforms
- Trim values (top/bottom)

#### Text Style Collection
**Complete typography styles:**
- Heading styles (h1-h6)
- Paragraph styles (p1-p3)
- Combines size, weight, spacing, family

#### Theme Collection
**Color theme modes:**
- Light mode colors
- Dark mode colors
- Custom theme variations
- Section backgrounds
- Text colors
- Border colors

#### Button Style Collection
**Button theme variations:**
- Primary button styles
- Secondary button styles
- References Theme collection

#### Column Count Collection
**Grid column configurations:**
- Used in grid utilities
- Controls responsive column behavior

#### Alignment Collection
**Layout alignment modes:**
- Start, Center, End
- Controls text and flex alignment together

### When to Update Variables

**Initial Setup (First Time):**
- Swatches (brand colors)
- Max Width, Gutter, Margin
- Border Radius
- Typography values
- Theme modes

**During Rebranding:**
- All typography values
- All color swatches and themes
- Spacing if needed

**Generally Don't Modify:**
- Column Count
- Alignment modes
- System utility variables

### Applying Variables

#### Method 1: Utility Classes
Stack utility class on custom class:
```
<div class="hero_title u-text-style-h2">
```

Advantages:
- Quick application
- Can override specific properties
- Visible in class list

#### Method 2: Style Panel Reference
1. Click into style input (e.g., margin-top)
2. Click purple + icon
3. Select variable from dropdown

Example:
```
Margin Top: size/4
```

#### Method 3: Custom Properties
For properties not yet supported in style panel:

1. Scroll to Custom Properties section
2. Add property name
3. Reference variable:
```
text-transform: var(--_typography---text-transform--uppercase)
```

### Variables vs Custom Styles

**Use Variables For:**
1. Typography (family, size, weight, transform, spacing, height)
2. Color (text, background, border, outline, stroke)
3. Page structure (padding, max-widths, section spacing)
4. Sizing (padding, margin, gaps)
5. Layout (grid columns, gap, alignment)
6. Miscellaneous (radius, border width, effects)

**Use Custom Styles/Utilities For:**
1. Display (block, flex, grid)
2. Layout (grid/flexbox positioning)
3. Width and Height (specific values)
4. Miscellaneous (opacity, overflow, z-index)

### Key Variable Patterns

#### Fluid Variables
Many size variables scale between viewport-min and viewport-max:
```
--size--1   # Scales from min to max viewport
--size--8   # Larger fluid scale
```

Edit with Fluid Builder App: https://fluidbuilder.timothyricks.com/

#### Theme Variables
Theme switching works through variable mode changes:
```
--theme--background    # Changes with theme mode
--theme--text          # Changes with theme mode
```

Apply with utilities:
```
u-theme-light          # Light mode
u-theme-dark           # Dark mode
```

#### Text Style Variables
Complete typography packages:
```
--text-style--h2       # All h2 properties
  ├── font-size
  ├── line-height
  ├── font-weight
  ├── letter-spacing
  ├── text-transform
  ├── margin
  └── trim values
```

---

## TYPOGRAPHY SYSTEM

### Overview

Typography in Lumos is managed through a three-tier system:
1. **Typography Variables** - Raw font values (families, weights, sizes)
2. **Text Style Variables** - Complete type styles (h1-h6, p1-p3)
3. **Components** - Heading and Paragraph components with variants

### Setup Process

#### 1. Upload Fonts to Webflow
- Upload all font weights needed
- Note exact weight values for each

#### 2. Configure Typography Collection

**Font Families:**
Set family variables for each font:
```
--_typography---font-family--primary
--_typography---font-family--secondary
```

**Font Weights:**
Set weight variables:
```
--_typography---font-weight--primary-regular: 400
--_typography---font-weight--primary-medium: 500
--_typography---font-weight--primary-bold: 700
```

**Line Height Trim:**
Set trim values for each family (removes unwanted space):
```
--_typography---trim-top--primary: 0.15em
--_typography---trim-bottom--primary: 0.15em
```

Find trim values: https://line-height-trim.webflow.io/

**Letter Spacing:**
Variable names match their values:
```
--_typography---letter-spacing--0: 0em
--_typography---letter-spacing--0-01: 0.01em
```

**Line Height:**
Variable names match their values:
```
--_typography---line-height--1: 1
--_typography---line-height--1-5: 1.5
```

**Font Sizes:**
Use Fluid Builder App to create responsive sizes:
```
--_typography---font-size--h1
--_typography---font-size--h2
--_typography---font-size--p1
```

Edit at: https://fluidbuilder.timothyricks.com/

#### 3. Configure Text Style Collection

Create mode for each text style (h1-h6, p1-p3) and apply:
- Font size
- Line height
- Letter spacing
- Text transform
- Margin bottom
- Font family
- Font weight
- Trim values
- Text wrap (for headings)

Example h2 mode:
```
font-size: --_typography---font-size--h2
line-height: --_typography---line-height--1-2
font-weight: --_typography---font-weight--primary-bold
margin-bottom: --space--3
```

#### 4. Create Utility Classes

Duplicate existing text-style classes and connect to new modes:
```
.u-text-style-h1  → Text Style h1 mode
.u-text-style-h2  → Text Style h2 mode
.u-text-style-p1  → Text Style p1 mode
```

#### 5. Update Components

Open Heading and Paragraph components, create variants for new sizes, connect to modes.

### Line Height Trim

**What It Does:**
Removes unwanted space from top and bottom of text boxes, enabling:
- Better alignment with other elements
- Consistent spacing regardless of line-height
- Predictable text box dimensions

**How It Works:**
Global Styles component automatically applies trim to:
- All `u-text-style-*` classes
- All text element tags (h1-h6, p)

Each font family needs unique trim values.

**Display: flow-root**
Each `u-text-style-*` class uses `display: flow-root` to ensure text box hugs characters.

**Figma Equivalent:**
Use "Vertical trim" under Type settings to replicate in designs.

### Adding New Font Sizes

1. Add size variable in Typography collection
2. Edit with Fluid Builder if needed
3. Create new mode in Text Style collection
4. Apply related variables (size, height, spacing, etc.)
5. Duplicate text-style utility class
6. Connect to new mode
7. Add variant to Heading/Paragraph components
8. Connect component variant to new mode

### Typography Spacing

**Bottom Margins:**
Typography components automatically apply bottom margin (defined in Text Style modes).

**Display Block Requirement:**
Parent elements wrapping text should use `display: block` (not flex) to allow margin collapse.

**Margin Collapse:**
When `display: block`, vertical margins overlap:
```
Heading (margin-bottom: 2rem)
Paragraph (margin-top: 1rem)
= 2rem total space (margins collapse)
```

Margins don't collapse in `display: flex` parents.

---

## COLOR SYSTEM

### Overview

The color system enables dynamic theme switching where sections, cards, or pages can change themes, and all child elements automatically update colors.

### Three-Tier Structure

1. **Swatches** - Raw color values
2. **Themes** - Context-aware color modes (light/dark)
3. **Button Styles** - Button-specific theme variations

### Setup Process

#### 1. Configure Swatches (Default Collection)

Add core brand colors:
```
--swatch--brand-500           # Primary brand color
--swatch--brand-text          # Text on brand background
--swatch--dark-800            # Dark neutral
--swatch--light-100           # Light neutral
```

**Color Pairs:**
Create text color for each background swatch:
```
--swatch--brand-500           # Background
--swatch--brand-text          # Text on that background
```

**Using color-mix:**
Generate shades/opacities from base colors:
```
color-mix(in srgb, var(--swatch--brand-500) 80%, white)
```

#### 2. Configure Theme Collection

Create modes: Light, Dark, (+ custom themes)

**For Each Mode Set:**
```
--theme--background           # Section background
--theme--text                 # Text color
--theme--border               # Border color
--theme--button-primary-bg    # Primary button background
--theme--button-primary-text  # Primary button text
--theme--button-secondary-bg  # Secondary button background
--theme--button-secondary-text # Secondary button text
```

Add additional folders if needed:
```
--theme--card-background
--theme--input-border-hover
--theme--input-border-focus
```

#### 3. Configure Button Style Collection

Create modes: Primary, Secondary, (+ custom)

Reference theme variables so buttons auto-update with theme:
```
Button Style Primary mode:
--button-style--background: var(--theme--button-primary-bg)
--button-style--text: var(--theme--button-primary-text)
```

#### 4. Connect to Components

**Theme to Components:**
1. Create component variants: Inherit, Light, Dark, (custom)
2. "Inherit" variant has no theme applied
3. Apply theme mode to each variant:
   - Light variant → Theme Light mode
   - Dark variant → Theme Dark mode

**Button Style to Components:**
1. Create button variants: Primary, Secondary
2. Apply button style mode to each variant:
   - Primary variant → Button Style Primary mode
   - Secondary variant → Button Style Secondary mode

### Using Themes

**Apply to Sections:**
```
<Section variant="Dark">
  <!-- All children inherit dark theme -->
</Section>
```

**Apply to Cards:**
```
<Card variant="Light">
  <!-- Card uses light theme regardless of section -->
</Card>
```

**Inherit from Parent:**
```
<Card variant="Inherit">
  <!-- Uses whatever theme parent has -->
</Card>
```

### Animating Themes with GSAP

**Standard Script:**
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/lumosframework/scripts@v1.1.1/theme-collector.js"></script>
<script>
document.addEventListener("colorThemesReady", () => {
  gsap.to(".my-element", { ...colorThemes.getTheme("dark", "3") });
});
</script>
```

**With Per Page CSS:**
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/lumosframework/scripts@v1.1.2/themecollector.js"></script>
<script>
document.addEventListener("colorThemesReady", () => {
  gsap.to(".my-element", { ...colorThemes.getTheme("dark", "orange") });
});
</script>
```

**Theme + Brand Switching:**
If using classes like `u-theme-dark` and `u-brand-orange`:
```javascript
colorThemes.getTheme("dark", "orange")
```

**Theme Only:**
```javascript
colorThemes.getTheme("dark")
```

---

## COMPONENTS ARCHITECTURE

### Overview

Components are the building blocks of Lumos sites, providing:
1. Global control over structures and styles
2. Easy site-wide updates
3. Ability for clients to build pages in Build Mode
4. Consistent, scalable design system

### Component Strategy

Two approaches exist:

#### Open Components (Default)
- Uses component slots extensively
- Any element can be added/removed freely
- Ideal for workflow speed and flexibility
- Best for client page building

**Structure Example:**
```
Section (component)
└── Layout (component)
    ├── Column 1 Slot
    │   ├── Eyebrow (component)
    │   ├── Heading (component)
    │   ├── Paragraph (component)
    │   └── Button Wrapper (component)
    │       └── Button (component)
    └── Column 2 Slot
        ├── Visual Image (component)
        └── Visual Overlay (component, optional)
```

#### Closed Components
- Uses component properties to show/hide elements
- Content stays consistent across instances
- Ideal for global elements (nav, footer, modals)
- More rigid but ensures consistency

**When to Use:**
- Modal with form (fields should be same everywhere)
- CTA section (heading/button text consistent)
- Navigation (structure consistent across pages)

### Converting Open to Closed

1. Build as open component first
2. Wrap entire component in div
3. Turn wrapper div into component
4. Surface only properties that should vary per instance
5. Keep global properties in main component

**Nested Slots:**
To access parent component slots, nest slot inside existing slot:
```
<slot class="u-display-contents">
  <slot name="original-slot-name"></slot>
</slot>
```

### Component Naming

**Principle:** Start with broadest term for filtering

**Examples:**
```
Section                        # Shows all sections
Section Hero                   # Shows all hero sections
Section Hero Split             # Specific hero variant

Button                         # Shows all buttons
Button Primary                 # Primary button variant

Icon                          # Shows all icons
Icon Social                   # Social media icons

Form                          # Shows all form elements
Form Field                    # Form input fields
```

**Keywords:**
- `Section` - Prebuilt full sections
- `Layout` - Reusable layout structures
- `Card` - Elements used in slots (repeating items)
- `Icon` - SVG icon elements
- `Form` - Form-related elements
- `CMS` - Collection List components

### Component Slots

**Naming Slots:**
Double-click slot label to name it.

Slot name should match the desired child component:
```
Slot name: "Card Hero"
Expected child: Card Hero component
```

**Purpose:**
Helps clients know which components to add where.

### Component Properties

**Organization Order:**
1. Visibility
2. Variants
3. Tag
4. Content (Text, Link, Image, Alt Text)
5. Role
6. Style (Max Width, Classes, Style Attribute)
7. Other Attributes
8. Slots

**Property Groups (for sections):**
- `Section` folder first (section-level controls)
- Element folders next (in order they appear)

**Naming:**
Be descriptive but concise:
```
Heading Text
Heading Style
Paragraph Text
Image Source
Button Link
```

### Component Variants

**Why Use Variants:**
- Avoid component bloat in panel
- Easy style switching without content re-entry
- Better client experience (one component, multiple styles)

**Naming Variant Property:**
Always name as "Variant" (not "Style" or "Type")

**Why:**
- Consistency across components
- Affects data attributes for targeting
- Makes custom code predictable

**Managing Variants:**
- Duplicate to create new variant
- Delete unused variants
- Keep variant count reasonable (5-7 max typically)

### Duplicating Components

1. Duplicate from components panel
2. Rename component
3. **CRITICAL:** Duplicate all custom classes inside
4. Ensures styles don't transfer between components

### Editing Main Component

**How to Enter Edit Mode:**
- Double-click instance
- Right-click → Edit component
- Select → Click pencil icon
- Select → Click Edit component button
- Select → Press Enter key

**Dark overlay indicates edit mode** - changes affect all instances.

**Caution:**
Generally clients should NOT edit main components. Stay in Build Mode to prevent accidental global changes.

---

## PAGE STRUCTURE

### Overview

Consistent page structure enables:
- Easy component addition in Build Mode
- Predictable element hierarchy
- Global elements (nav/footer) consistency
- Proper semantic HTML

### Standard Structure

```
Body
└── page_wrap
    ├── Global Styles (component)
    ├── Global Guides (component, optional)
    ├── Nav (component)
    ├── page_main (slot, id="main", tag=main)
    └── Footer (component)
```

### Element Breakdown

#### page_wrap
**Purpose:** Contains all page content

**Styles:**
- `overflow: clip` - Prevents horizontal scroll
- `min-height: 100svh` - Keeps footer at bottom
- `display: flex` - Enables footer anchoring

**Usage:**
Select and copy to duplicate content to new page.

#### Global Styles
**Purpose:** Contains CSS used across all pages

**Contents:**
- Line height trim code
- Global resets
- Site-wide utilities
- Custom CSS

#### Global Guides (Optional)
**Purpose:** Visual overlay to check alignment across sections

**Behavior:**
- Automatically hidden in Preview mode
- Automatically hidden on published site
- Visible only in Designer

#### Nav
**Purpose:** Site navigation component

**Characteristics:**
- Used across every page (typically)
- Usually has no component properties
- Consistent structure site-wide

#### page_main (Slot)
**Purpose:** Contains unique page content

**Setup:**
- Is a component slot (enables Build Mode editing)
- Has `id="main"` for skip-to-main links
- Tagged as `<main>` for SEO and screen readers
- Does NOT contain nav or footer

**Usage:**
Clients add section components here in Build Mode.

#### Footer
**Purpose:** Site footer component

**Characteristics:**
- Used across every page (typically)
- Usually has no component properties
- Consistent structure site-wide
- Anchored to bottom via page_wrap flex

### Variations

**Additional Elements:**
Your project may include:
- Cookie consent components
- Chat widgets
- Announcement bars
- Loading screens

Add these inside page_wrap as needed.

---

## BREAKPOINTLESS DESIGN

### Overview

Lumos uses a breakpointless approach that creates responsive designs using rem-based fluid values instead of Webflow's pixel-based breakpoints.

### Why Breakpointless?

**Problem with Pixel Breakpoints:**
When users increase their browser font size (accessibility setting):
- Layout doesn't wrap (breakpoints based on px, not font size)
- Text overflows containers
- Design breaks for users who need larger text
- Violates accessibility standards

**Breakpointless Solution:**
- Uses rem units (scale with user font size)
- Layout wraps when text gets larger
- Maintains design integrity across all font sizes
- Improves accessibility automatically

### Benefits

1. **Accessibility** - Layouts wrap when users scale font size
2. **Less Code** - Auto-responsive styles vs manual breakpoint styles
3. **Faster Development** - Styles work responsively automatically
4. **Easier Maintenance** - Updates in one place, not per breakpoint
5. **Better UX** - Smoother transitions across all screen sizes

### Techniques

#### 1. Enable Flexbox Wrapping

**Instead of:** Fixed flex layouts that overflow

**Do this:** Allow wrapping
```
display: flex
flex-wrap: wrap
```

**Result:**
- Browser decides when to wrap
- Adapts to content changes
- Works with font size increases
- Handles language translation (longer text)

#### 2. Use Lumos Grid Utilities

**Auto-Responsive Grids:**
```
u-grid-autofit              # Stretches cards into space
u-grid-autofill             # Leaves empty space (for filtering)
```

**How They Work:**
- Columns adjust based on available space
- Minimum column width is defined
- Auto-wraps when needed
- No breakpoint management required

**Benefits:**
- Content changes? Auto-adjusts
- Font size changes? Auto-wraps
- Language changes? Handles longer text

#### 3. Use Fluid Typography

**Problem with Breakpoint Font Sizes:**
- Changes designed for smaller screens don't trigger for users with large font size
- Text overflows at user's preferred size
- Creates accessibility barriers

**Fluid Typography:**
- Smoothly scales between min and max viewport
- Works with browser zoom AND font size changes
- Easier to maintain within type scale

**Setup:**
Use Fluid Builder App to create fluid sizes:
https://fluidbuilder.timothyricks.com/

#### 4. Avoid All px Units

**Why px is Problematic:**
- Scales with browser zoom but NOT user font size
- Makes borders/shadows feel smaller when users increase font
- Can cause overflow in rem-based containers
- Similar issues to vw units

**Use Instead:**
- `rem` for most spacing, sizing
- `em` for values relative to font size
- `%` for proportional values
- Variables that use rem

**Exception:**
Hairline borders (1px) are acceptable but consider `0.0625rem` instead.

#### 5. Use Responsive Variable System

**Lumos Responsive Variables:**
Custom properties that change based on container size:
```
--responsive-size-1          # Changes at defined breakpoints
--responsive-size-2          # Larger responsive value
```

**Usage:**
Apply from main Webflow breakpoint, values auto-adjust.

**Benefits:**
- Control specific component variants responsively
- No need to switch Webflow breakpoints
- Component-level responsive control
- Works with container queries

See: Responsive Variable System documentation

#### 6. Use Container Queries

**When Needed:**
For component-specific responsive behavior.

**Setup:**
Add custom property to container or parent:
```
container-type: inline-size;
```

**Custom CSS:**
```css
@container (width < 40em) {
  .hero_layout { 
    display: flex; 
  }
  .hero_content { 
    order: -1; 
  }
}
```

**Best Practice:**
Use same container sizes as responsive variable system for global updateability.

**GSAP with Container Queries:**

Use match-container.js:
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/ScrollTrigger.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/flowtricks/script@v1.0.4/match-container.js"></script>
<script>
document.querySelectorAll(".hero_section").forEach(function (section) {
  const heading = section.querySelector(".hero_heading");
  const paragraph = section.querySelector(".hero_paragraph");
  
  heading.observeContainer("(width < 40em)", (match) => {
    if (match) {
      gsap.to(heading, { color: "red" });
      gsap.to(paragraph, { opacity: 0.5 });
    } else {
      gsap.to(heading, { color: "green" });
      gsap.to(paragraph, { opacity: 1 });
    }
  });
});
</script>
```

### Development Workflow

**Rule:** Apply no styles to tablet, mobile landscape, or mobile portrait breakpoints.

**Instead:**
1. Build on desktop breakpoint only
2. Use techniques above for responsive behavior
3. Test by resizing browser and changing font size
4. Adjust fluid values and min/max as needed

---

## SIZE & SPACING

### Overview

Lumos uses fluid spacing that automatically scales between minimum and maximum viewport sizes, creating:
- Visual consistency across devices
- Smooth scaling transitions
- Better zoom experience
- Reduced overflow and cut-off content

### Core Site Variables

#### site/viewport-max
**Default:** `90rem`

Max screen size where fluid sizes stop scaling up.

#### site/viewport-min
**Default:** `20rem`

Min screen size where fluid sizes stop scaling down.

#### site/margin
**Purpose:** Left/right space outside container

**Default:** Fluid value (changes with viewport)

**Edit:** Use Fluid Builder App

**Prevents:** Container touching screen edges

#### site/gutter
**Purpose:** Default gap between grid columns

**Default:** Static value (`1rem`)

**Can Make Fluid:** Use Fluid Builder App

#### site/column-count
**Purpose:** Main column count for grid system

**Default:** `12`

**Usage:** Referenced by grid utilities and components

#### site/column-width
**Purpose:** Calculation variable for breakout grid

**DO NOT ADJUST** - Auto-calculated

### Space Variables (1-8)

**Purpose:** General spacing for elements within sections

**Usage:**
- Separate elements within a section
- Padding values
- Gap values
- Margins between elements

**Range:**
- `--space--1` (smallest)
- `--space--8` (largest, still smaller than section spacing)

**Important:** Using space folder values will never make elements seem like separate sections.

**Edit:** Use Fluid Builder App

**Application:**
```
margin-bottom: var(--space--3)
gap: var(--space--4)
padding: var(--space--2)
```

### Section Space Variables

**Purpose:** Top/bottom spacing for sections

**Values:**
```
--section-space--1           # Smallest section spacing
--section-space--2
--section-space--3
--section-space--4           # Largest section spacing
--section-space--page-top    # Top of page (accounts for nav)
```

**Usage:**
Applied to section components for vertical rhythm.

**Edit:** Use Fluid Builder App

### Margin vs Gap

**Use Gap For:**
- Spacing list items
- Separating grid/flex children
- Uniform spacing between siblings

**Use Margin For:**
- Spacing text elements
- Creating unique space between element types
- More flexible control without extra wrappers

**Why Margin for Text:**
Allows different spacing between headings and paragraphs without extra divs.

### Margin Trim

**Purpose:** Remove unwanted margins from first/last children

**Usage:**
```html
<div class="content_area u-margin-trim">
  <h2>First child margin-top removed</h2>
  <p>Middle child margins intact</p>
  <p>Last child margin-bottom removed</p>
</div>
```

**Effect:**
- Removes `margin-top` from first child
- Removes `margin-bottom` from last child
- Cleaner section edges

### Collapsing Margins

**How It Works:**
When two vertical margins meet, they collapse into one (the larger value):

```
<h2 style="margin-bottom: 2rem">Heading</h2>
<p style="margin-top: 1rem">Paragraph</p>
```
Result: `2rem` total space (not 3rem)

**Requirements for Collapse:**
- Parent must be `display: block`
- Only vertical margins collapse
- Doesn't work in flex or grid parents

**When Margins DON'T Collapse:**
- `display: inline, inline-block, inline-flex, inline-grid`
- Parent is `display: flex` or `display: grid`
- Horizontal margins never collapse

**Best Practice:**
Apply margins to `display: block` elements for predictable collapsing.

---

## GRID SYSTEM

### Overview

Lumos provides:
1. Grid Component (primary layout tool)
2. Grid utilities for auto-responsive grids
3. Column/row utilities for grid children

### Setup

#### 1. Set Global Column Count

Variable: `--site--column-count`  
Default: `12`

Most designs use 12. Some use 10 or 24.

**Used by:**
- Grid utilities
- Grid component defaults
- Global grid guides

#### 2. Set Global Gutter

Variable: `--site--gutter`  
Default: `1rem`

Used as default gap in all grid utilities.

**Can be fluid:** Use Fluid Builder App to make responsive

### Grid Component

**Primary use:** Creating custom layouts in Lumos

**Features:**
- Responsive column control per breakpoint
- Variants: Autofit, Autofill, Contained
- Full property control via component properties

**Properties:**
- Column count (per responsive breakpoint)
- Gap settings
- Minimum column width
- Alignment options
- Custom classes

**Variants:**
- Default (custom columns)
- Autofit (stretch into space)
- Autofill (leave empty space)
- Contained (respects column count exactly)

See: Grid component documentation for full details

### Grid Utilities

#### u-grid-autofit

**Behavior:** Stretches cards to fill available space

**Use when:**
- Fixed number of items
- Want cards to expand into extra space
- Gallery or portfolio layouts

**Configuration:**
```
Column Count mode: Sets max columns
Gap mode: Sets horizontal and vertical gap
Custom Property: --min-column-width (minimum before wrap)
```

**Example:**
```html
<div class="u-grid-autofit" style="--min-column-width: 20rem">
  <!-- Cards stretch to fill space -->
</div>
```

#### u-grid-autofill

**Behavior:** Leaves empty space when fewer items

**Use when:**
- Dynamic number of items (CMS, filtering)
- Want consistent card size regardless of count
- Blog posts, products with filtering

**Configuration:**
Same as autofit.

**Example:**
```html
<div class="u-grid-autofill" style="--min-column-width: 18rem">
  <!-- Cards maintain size, empty columns appear -->
</div>
```

#### u-grid-custom

**Behavior:** 12-column grid (or custom count)

**Use when:**
- Need manual column spanning
- Complex layouts with different-sized items
- More control than autofit/autofill

**Override column count:**
```css
@container (width < 40em) {
  .blog_cms_list { 
    --_column-count---value: 2 !important; 
  }
}

@container (width < 20em) {
  .blog_cms_list { 
    --_column-count---value: 1 !important; 
  }
}
```

#### u-grid-breakout

**Purpose:** Elements break out of container to screen edge

**Setup:**
- Grid with extra columns at start/end
- Outer columns expand to fill screen width
- NOT used inside `.u-container`

**Column Positioning:**
```css
grid-column-start: full;      /* Start at screen edge */
grid-column-start: content;   /* Start inside container */
grid-column-end: full;        /* End at screen edge */
grid-column-end: content;     /* End inside container */
```

**Use case:**
Full-width background with contained content:
```html
<div class="u-grid-breakout">
  <div style="grid-column: full / full; background: blue;">
    <div style="grid-column: content / content;">
      Contained content with full-width background
    </div>
  </div>
</div>
```

#### u-grid-subgrid

**Purpose:** Create grid inside parent grid cell

**Behavior:**
Element spanning 8 parent columns becomes an 8-column grid.

**Browser Support:**
Newly supported in major browsers.  
Check: https://caniuse.com/?search=subgrid

**Example:**
```html
<div class="u-grid-custom">
  <div class="u-grid-subgrid" style="grid-column: span 6;">
    <!-- This is now a 6-column grid -->
  </div>
</div>
```

### Column & Row Utilities

**Purpose:** Control how grid children span/position

**Span Utilities:**
```
u-column-span-1 through 12    # Span X columns
u-row-span-1 through 12       # Span X rows
```

**Start Utilities:**
```
u-column-start-1 through 12   # Start at column X
u-row-start-1 through 12      # Start at row X
```

**Order Utilities:**
```
u-order-1 through 12          # Visual order in grid
```

**Responsive Column Spans:**
Can change spans at different container sizes using custom CSS.

---

## FLEXBOX SYSTEM

### Overview

Lumos provides flexbox utilities for:
- Layout direction and wrapping
- Alignment and justification
- Item-specific positioning
- Integration with alignment variable modes

### Alignment Variable Collection

**Purpose:** Sync text alignment with flex alignment globally

**Modes:**
- Start (left align)
- Center
- End (right align)

**Usage:**
Apply to parent to set alignment for all children:
```html
<div class="section_content u-alignment-center">
  <!-- Text and flex children are centered -->
</div>
```

**Benefits:**
- Change entire section alignment from one place
- Text and layout align together
- Easy responsive alignment changes
- Child components inherit alignment

### Display Flex Utilities

#### u-flex-horizontal-wrap
```
display: flex
flex-direction: row
flex-wrap: wrap
align-items: var(--alignment-mode)
```

**Use:** Horizontal flex with wrapping, aligned to mode

#### u-flex-vertical-nowrap
```
display: flex
flex-direction: column
flex-wrap: nowrap
```

**Use:** Vertical flex, no wrapping

#### u-flex-horizontal-nowrap
```
display: flex
flex-direction: row
flex-wrap: nowrap
align-items: var(--alignment-mode)
```

**Use:** Horizontal flex, no wrapping

#### u-flex-vertical-wrap
```
display: flex
flex-direction: column
flex-wrap: wrap
```

**Use:** Vertical flex with wrapping (rare use case)

### Align Items Utilities

**Controls:** Cross-axis alignment

**Horizontal Flex:** Aligns children vertically  
**Vertical Flex:** Aligns children horizontally

```
u-align-items-variable       # Uses alignment mode
u-align-items-start          # Align to start
u-align-items-center         # Center alignment
u-align-items-end            # Align to end
u-align-items-stretch        # Stretch to fill
```

### Justify Content Utilities

**Controls:** Main-axis distribution

**Horizontal Flex:** Distributes horizontally  
**Vertical Flex:** Distributes vertically

```
u-justify-content-variable   # Uses alignment mode
u-justify-content-start      # Group at start
u-justify-content-center     # Group at center
u-justify-content-end        # Group at end
u-justify-content-between    # Space between items
u-justify-content-around     # Space around items
```

### Align Self Utilities

**Purpose:** Override parent alignment for specific child

**Apply to:** Child element in flex or grid

```
u-align-self-variable        # Uses alignment mode
u-align-self-start           # Align this child to start
u-align-self-center          # Align this child to center
u-align-self-end             # Align this child to end
u-align-self-stretch         # Stretch this child
```

### Flex Basis Utilities

```
u-flex-shrink                # Allow shrinking
u-flex-grow                  # Allow growing
u-flex-noshrink              # Prevent shrinking
```

**Common Use:**
```html
<div class="u-flex-horizontal-wrap">
  <div class="u-flex-grow">Content that grows</div>
  <div class="u-flex-noshrink">Fixed width content</div>
</div>
```

---

## TRIGGER & STATE

### Overview

The Trigger & State system enables CSS-only interactions without JavaScript, using data attributes to control element states based on user interaction or element state.

**Benefits:**
- Rename classes without breaking interactions
- All styles visible in style panel (no hidden code)
- Easy override with component variants
- No custom code embeds needed

### State Attributes

Elements become active when condition is met:

#### data-state="checked"
**Active when:** Element is/contains checked checkbox or radio

**Example:**
```html
<label data-state="checked">
  <input type="checkbox" checked>
  Checked item
</label>
```

#### data-state="current"
**Active when:** Element is/contains:
- Link to current page
- Anchor link to current section

**Example:**
```html
<a href="/about" data-state="current">
  About (active when on /about page)
</a>
```

#### data-state="open"
**Active when:** Element is/contains:
- Open menu button
- Open dropdown toggle
- Open dropdown list

**Example:**
```html
<div data-state="open">
  Dropdown content
</div>
```

#### data-state="expanded"
**Active when:** Element is/contains child with `aria-expanded="true"`

Includes:
- Open dropdown toggles
- Custom accordions

**Example:**
```html
<button aria-expanded="true" data-state="expanded">
  Accordion trigger
</button>
```

#### data-state="external"
**Active when:** Element is/contains link opening in new tab

**Example:**
```html
<a href="https://external.com" target="_blank" data-state="external">
  External link
</a>
```

#### is-active (class)
**Active when:** Element has class `is-active`

**Example:**
```html
<div class="tab is-active">
  Active tab
</div>
```

**Usage:** Toggled via JavaScript for custom interactions

### Trigger Attributes

Elements become active based on interaction:

#### data-trigger="focus"
**Active when:** Element or child is focused

**Example:**
```html
<div data-trigger="focus">
  <input type="text">
  Container active when input focused
</div>
```

#### data-trigger="hover"
**Active when:** Element is hovered

**Example:**
```html
<button data-trigger="hover">
  Hover me
</button>
```

#### data-trigger="hover-if-clickable"
**Active when:** Element contains Clickable Component

**Purpose:** Hover state only if element has interactive child

#### data-trigger="mobile"
**Active on:** Touch devices only

**Example:**
```html
<div data-trigger="mobile">
  Mobile-only content
</div>
```

#### data-trigger="preview"
**Active in:** Webflow Designer only (not live site)

**Example:**
```html
<div data-trigger="preview">
  Designer-only guides
</div>
```

#### data-trigger="group"
**Purpose:** Enable sibling interaction triggers

**Apply to:** Parent container

**Example:**
```html
<div data-trigger="group">
  <div data-trigger="hover">Item 1</div>
  <div data-trigger="hover-other">Item 2 (active when 1 hovered)</div>
</div>
```

#### data-trigger="hover-other"
**Active when:** Sibling within `data-trigger="group"` is hovered

**Requires:** Parent with `data-trigger="group"`

#### data-trigger="focus-other"
**Active when:** Sibling within `data-trigger="group"` is focused

**Requires:** Parent with `data-trigger="group"`

### Applying in Style Panel

Uses CSS variables that change between 0 (inactive) and 1 (active):
```
--_trigger---on   # 1 when active, 0 when inactive
--_trigger---off  # 0 when active, 1 when inactive
```

#### Opacity Changes

**Reduce to 60%:**
```css
calc(1 - 0.4 * var(--_trigger---on))
```

**Result:** 100% normally, 60% when triggered

#### Color Switching

**Switch between two colors:**
```css
color-mix(in srgb,
  var(--swatch--brand-500) calc(100% * var(--_trigger---on)),
  var(--swatch--dark-800) calc(100% * var(--_trigger---off))
)
```

**Result:** Brand color when triggered, dark when not

#### Font Weight Changes

**Switch from regular to bold:**
```css
calc(
  var(--_typography---font-weight--primary-regular) * var(--_trigger---off) +
  var(--_typography---font-weight--primary-bold) * var(--_trigger---on)
)
```

#### Transform: Scale

**Scale to 120%:**
```css
scale(calc(1 + 0.2 * var(--_trigger---on)))
```

**Scale to 80%:**
```css
scale(calc(1 - 0.2 * var(--_trigger---on)))
```

#### Transform: Rotate

**Rotate to 45deg:**
```css
rotate(calc(45deg * var(--_trigger---on)))
```

#### Transform: Translate

**Move horizontally:**
```css
translateX(calc(2rem * var(--_trigger---on)))
```

**Move vertically:**
```css
translateY(calc(2rem * var(--_trigger---on)))
```

### Combined Trigger OR State

**If either active, apply style:**

Uses CSS variables that check both:
```css
/* If trigger OR state is active */
opacity: calc(1 - 0.5 * var(--_trigger-or-state---on))
```

**Example use:** Button should be transparent when EITHER hovered OR active

---

## UTILITY CLASSES REFERENCE

### Structure Utilities

```
u-container                    # Max-width container with margin
u-display-contents             # Wrapper becomes invisible
u-display-block                # Display block
u-display-flex                 # Display flex
u-display-grid                 # Display grid
u-display-none                 # Hide element
```

### Grid Utilities

```
u-grid-autofit                 # Auto-fit grid (stretch cards)
u-grid-autofill                # Auto-fill grid (empty space)
u-grid-custom                  # 12-column custom grid
u-grid-breakout                # Breakout grid (full-width)
u-grid-subgrid                 # Subgrid
```

### Grid Column/Row

```
u-column-span-1 through 12     # Span columns
u-row-span-1 through 12        # Span rows
u-column-start-1 through 12    # Start at column
u-row-start-1 through 12       # Start at row
u-order-1 through 12           # Visual order
```

### Flexbox Utilities

```
u-flex-horizontal-wrap         # Flex row, wrap
u-flex-horizontal-nowrap       # Flex row, no wrap
u-flex-vertical-wrap           # Flex column, wrap
u-flex-vertical-nowrap         # Flex column, no wrap

u-align-items-variable         # Align via mode
u-align-items-start            # Align start
u-align-items-center           # Align center
u-align-items-end              # Align end
u-align-items-stretch          # Align stretch

u-justify-content-variable     # Justify via mode
u-justify-content-start        # Justify start
u-justify-content-center       # Justify center
u-justify-content-end          # Justify end
u-justify-content-between      # Space between
u-justify-content-around       # Space around

u-align-self-variable          # Self align via mode
u-align-self-start             # Self align start
u-align-self-center            # Self align center
u-align-self-end               # Self align end
u-align-self-stretch           # Self align stretch

u-flex-grow                    # Allow grow
u-flex-shrink                  # Allow shrink
u-flex-noshrink                # Prevent shrink
```

### Typography Utilities

```
u-text-style-h1 through h6     # Heading styles
u-text-style-p1 through p3     # Paragraph styles
```

### Color Utilities

```
u-theme-light                  # Light theme
u-theme-dark                   # Dark theme
u-theme-inherit                # Inherit parent theme
```

### Spacing Utilities

```
u-gap-1 through 8              # Gap spacing
u-margin-top-1 through 8       # Top margin
u-margin-bottom-1 through 8    # Bottom margin
u-margin-left-1 through 8      # Left margin
u-margin-right-1 through 8     # Right margin
u-margin-trim                  # Remove first/last margins
u-padding-1 through 8          # Padding all sides
```

### Alignment Utilities

```
u-alignment-start              # Left align + flex start
u-alignment-center             # Center align + flex center
u-alignment-end                # Right align + flex end
```

### Responsive Utilities

```
u-hide-mobile                  # Hide on mobile
u-hide-tablet                  # Hide on tablet
u-hide-desktop                 # Hide on desktop
u-show-mobile                  # Show only mobile
u-show-tablet                  # Show only tablet
u-show-desktop                 # Show only desktop
```

### Miscellaneous Utilities

```
u-pointer-events-none          # Disable pointer events
u-overflow-hidden              # Hide overflow
u-overflow-scroll              # Enable scrolling
u-position-relative            # Position relative
u-position-absolute            # Position absolute
u-z-index-1 through 10         # Z-index layers
```

---

## COMMON PATTERNS

### Pattern: Basic Content Section

**Structure:**
```
Section (component, variant: Light/Dark)
└── Layout (component)
    ├── Column 1
    │   ├── Eyebrow (component)
    │   ├── Heading (component, variant: h2)
    │   ├── Paragraph (component, variant: p1)
    │   └── Button Wrapper (component)
    │       └── Button (component, variant: Primary)
    └── Column 2
        ├── Visual Image (component)
        └── Visual Overlay (component, optional)
```

**Classes:**
```
.section_wrap.u-theme-light
  .layout_wrap.u-grid-custom
    .layout_column-1
      .eyebrow_wrap.u-text-style-p3
      .heading_wrap.u-text-style-h2
      .paragraph_wrap.u-text-style-p1
      .button_wrapper.u-flex-horizontal-wrap
        .button.u-button-primary
    .layout_column-2
      .visual_image
```

### Pattern: CMS Collection Grid

**Structure:**
```
Collection List Wrapper
└── Collection List (u-grid-autofit)
    └── Collection Item
        └── Card (component)
            ├── Card Visual
            ├── Card Heading
            └── Card Text
```

**Classes:**
```
.blog_cms_wrap
  .blog_cms_list.u-grid-autofit
    .blog_cms_item (Collection Item)
      .card_blog_wrap
        .card_blog_visual
        .card_blog_heading.u-text-style-h4
        .card_blog_text.u-text-style-p2
```

**Key Points:**
- Grid utility on Collection List
- Card component as Collection Item child
- CMS fields bound inside card component

### Pattern: Dynamic Linking in CMS

**CORRECT Structure:**
```
Collection List
└── Collection Item
    └── Clickable Component (Lumos default)
        └── Card content
```

**INCORRECT Structure:**
```
Custom Component Wrapper (without exposed properties)
└── Collection List
    └── Collection Item
        └── Card (CMS data doesn't flow)
```

**Critical Rule:**
Never wrap Collection List in component without exposing Collection Item property. Use Lumos Clickable component instead.

### Pattern: Responsive Container

**Structure:**
```html
<div class="section_wrap u-theme-dark">
  <div class="section_container u-container">
    <div class="section_layout u-grid-custom">
      <!-- Content -->
    </div>
  </div>
</div>
```

**Hierarchy:**
1. Section wrap (theme, background)
2. Container (max-width, margins)
3. Layout (grid/flex structure)
4. Content

### Pattern: Nested Themes

**Structure:**
```html
<section class="section_wrap u-theme-dark">
  <!-- Dark theme section -->
  
  <div class="card_wrap u-theme-light">
    <!-- Light theme card inside dark section -->
  </div>
  
  <div class="card_wrap u-theme-inherit">
    <!-- Inherits dark theme from section -->
  </div>
</section>
```

### Pattern: Hover Card with Group

**Structure:**
```html
<div class="cards_wrapper u-flex-horizontal-wrap" data-trigger="group">
  
  <div class="card_wrap" data-trigger="hover">
    <!-- This card -->
  </div>
  
  <div class="card_wrap" data-trigger="hover-other">
    <!-- Other cards (fade when sibling hovered) -->
  </div>
  
</div>
```

**CSS:**
```css
.card_wrap[data-trigger="hover-other"] {
  opacity: calc(1 - 0.5 * var(--_trigger---on));
}
```

---

## TROUBLESHOOTING

### CMS Dynamic Links Not Working

**Symptom:**
- CMS dynamic links show placeholder
- Child components don't receive CMS data
- Slider/collection items show identical content

**Cause:**
Collection List wrapped in component without exposed Collection Item property.

**Solution:**
1. Use Lumos Clickable component (framework default)
2. Ensure Collection Item is exposed as component property
3. Never wrap Collection List without proper property exposure

**Correct Structure:**
```
Collection List
└── Collection Item
    └── Clickable Component (with properties exposed)
        └── Child components with CMS bindings
```

### Text Overflowing Container

**Symptom:**
Text breaks out of container on smaller screens or when users increase font size.

**Cause:**
Using pixel breakpoints instead of fluid/breakpointless approach.

**Solution:**
1. Enable flex wrapping: `flex-wrap: wrap`
2. Use fluid typography (not fixed sizes per breakpoint)
3. Use `u-grid-autofit` or `u-grid-autofill` for automatic wrapping
4. Ensure container uses rem units, not px

### Margins Not Collapsing

**Symptom:**
More space between elements than expected.

**Cause:**
Parent element is `display: flex` or `display: grid`.

**Solution:**
1. Use `display: block` for parent of text elements
2. Or use gap instead of margins in flex/grid parents
3. Ensure margins are on block-level elements

### Component Properties Not Showing

**Symptom:**
Can't see/edit component properties in right panel.

**Cause:**
- Not selected on canvas
- In edit component mode (editing main, not instance)
- Component has no properties set up

**Solution:**
1. Select component instance on canvas (not in navigator)
2. Exit edit component mode (ESC key)
3. Check if component main has properties configured

### Utility Class Not Working

**Symptom:**
Adding utility class has no visible effect.

**Cause:**
- Wrong context (e.g., gap class on non-flex/grid)
- Overridden by custom class styles
- Utility needs additional setup (like grid utilities need container size)

**Solution:**
1. Verify element display type matches utility purpose
2. Check if custom class has conflicting styles
3. For grid utilities, ensure `--min-column-width` is set
4. Remove utility if it's not needed (don't leave ineffective classes)

### Theme Not Applying to Children

**Symptom:**
Child elements don't update when section theme changes.

**Cause:**
Child elements using fixed color values instead of theme variables.

**Solution:**
1. Ensure child components use `var(--theme--text)` not fixed colors
2. Check Button Style collection references Theme collection
3. Verify component variants are connected to theme modes

### Flexbox/Grid Gap Not Working

**Symptom:**
Gap property has no effect.

**Cause:**
- Element is not `display: flex` or `display: grid`
- Using gap on wrong element

**Solution:**
1. Apply gap to parent flex/grid container, not children
2. Ensure parent has `display: flex` or `display: grid`
3. Check if overridden by custom class styles

---

## APPENDIX

### Official Resources

**Framework Documentation:**
https://lumos.timothyricks.com

**Changelog:**
https://lumos.timothyricks.com/changelog

**Fluid Builder App:**
https://fluidbuilder.timothyricks.com/

**Line Height Trim Tool:**
https://line-height-trim.webflow.io/

**Chrome Extension:**
Lumos Framework extension for better class management

**Cloneable:**
https://try.webflow.com/tricks?path=lumos-v2-beta

### Framework Versions

Current: v2.2.1 (October 23, 2025)
- Latest stable release
- Minor improvements

Previous major releases available in changelog.

### Common Terms

**Component** - Reusable Webflow component  
**Custom Class** - Element identifier (underscores)  
**Utility Class** - Reusable style (u- prefix)  
**Combo Class** - Modifier (is- prefix)  
**Variable** - Design token in variables panel  
**Mode** - Variable state/variation  
**Slot** - Component slot for child elements  
**Property** - Component property for configuration  
**Variant** - Component style variation  
**Fluid** - Scales between min/max viewport  
**Breakpointless** - Responsive without px breakpoints  
**Trim** - Line-height space removal  
**Theme** - Color mode (light/dark/custom)  
**Swatch** - Raw color value  

### Best Practices Summary

1. **Always use variables** for colors, typography, spacing
2. **Custom class first**, utilities after
3. **Components for everything** (even simple elements)
4. **Breakpointless approach** (no tablet/mobile breakpoint styles)
5. **Fluid typography** over fixed sizes
6. **Expose Collection Item** property when wrapping Collection Lists
7. **Use framework defaults** (like Clickable) before custom solutions
8. **Document project-specific** additions to framework
9. **Test with font scaling** (accessibility)
10. **Keep utility count low** (max 4 per element)

---

## DOCUMENT END

**Last Updated:** January 20, 2026  
**Framework Version:** v2.2.1  
**Maintained by:** Pedro (via Claude AI)  
**Purpose:** AI-optimized reference for Lumos Framework development

**Usage Notes:**
- This document optimized for AI parsing and human reading
- Use CMD/CTRL+F to search specific topics
- Reference official docs for latest updates
- Add project-specific learnings to separate file
- Version control this document as framework evolves

**Next Steps:**
1. Use this reference for current development
2. Create project-specific learning logs (JSON format)
3. Build pattern library from real implementations
4. Split into modular files when patterns emerge
5. Update as framework versions release
