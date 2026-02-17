# Lumos Framework — Video Tutorial Insights
## Supplemental Knowledge Base

**Source:** 27 Timothy Ricks video tutorials (compiled)  
**Version Scope:** Lumos v2.1 through v2.2.2  
**Role:** Supplemental — adds only content not covered in `lumos-complete-reference.md` or `v2-2-2-changelog-addendum.md`  
**Last Updated:** February 17, 2026

> **Version Notes:** Content marked `[v2.1]` applies to v2.1 and later. Content marked `[v2.2+]` is specific to v2.2.0 and later. Content marked `[2026]` reflects 2026 Webflow platform standards. Unmarked content applies to all versions.

---

## Table of Contents

1. [Framework Architecture & Inheritance Rules](#1-framework-architecture--inheritance-rules)
2. [Utility Classes — Undocumented & Extended](#2-utility-classes--undocumented--extended)
3. [Layout Component — Deep Implementation](#3-layout-component--deep-implementation)
4. [Number Props & calc() Logic](#4-number-props--calc-logic)
5. [Trigger & State Machine — Advanced](#5-trigger--state-machine--advanced)
6. [Conditional Visibility — 2026 Engine](#6-conditional-visibility--2026-engine)
7. [Interactions & Animations](#7-interactions--animations)
8. [CMS Integration Patterns](#8-cms-integration-patterns)
9. [Component System Governance](#9-component-system-governance)
10. [AI-to-Webflow Workflow](#10-ai-to-webflow-workflow)
11. [Chrome Extension Shortcuts](#11-chrome-extension-shortcuts)
12. [Lumos Libraries & Migration](#12-lumos-libraries--migration)
13. [Figma-to-Webflow Workflow](#13-figma-to-webflow-workflow)
14. [Performance & Semantic Standards (2026)](#14-performance--semantic-standards-2026)
15. [Deprecation Log](#15-deprecation-log)
16. [Logic Cross-Reference Table](#16-logic-cross-reference-table)

---

## 1. Framework Architecture & Inheritance Rules

### Page Structure Hierarchy

```
page-wrap (overflow: clip)
└── nav (z-index: 1000, position: fixed or sticky)
└── main#main (id="main" required for skip-link accessibility)
    └── sections (u-section)
        └── containers (u-container)
```

**Key rules:**
- `overflow: clip` on page-wrap prevents horizontal scroll issues without affecting sticky positioning (unlike `overflow: hidden`)
- `id="main"` on the `<main>` element is required for WCAG skip-link functionality
- Nav z-index: `1000` ensures it stays above all `position: relative` sections

*Source: 005, 001*

---

### Theme Inheritance — The Mode Hierarchy

Themes applied at the `Page Wrap` level (Light/Dark) are inherited by all children. Override by applying a specific theme class to a section:

```
page-wrap [u-theme-light]
└── section [u-theme-dark]     ← overrides page theme
    └── component slot content ← inherits the COMPONENT's variable mode
                                  NOT the global page state
```

**The Slot Rule:** Content dropped into a component slot inherits the **Variable Mode of the component instance**, not the global page state. This is a common source of unexpected color behavior.

**Nav Color Inheritance:** Navbars use `color: inherit` on links to automatically match the parent section's Theme mode (Light/Dark) without manual overrides.

*Source: 017, 023*

---

### Container Query Thresholds

Lumos uses four container size thresholds, not viewport breakpoints:

| Name | Value | Variable |
|------|-------|----------|
| Large | > 75em | `--threshold-large` |
| Medium | 50em | `--threshold-medium` (var: `--flex-medium`) |
| Small | 35em | `--threshold-small` (var: `--flex-small`) |
| Extra Small | 20em | `--threshold-xs` (var: `--flex-xsmall`) |

**Implementation:**
```css
/* Layout swapping via container query variables */
display: var(--display-medium, grid);
/* Above 50em: grid. Below 50em: inherits flex-column from threshold logic */

/* Show/hide based on container width */
display: var(--none-medium);   /* hidden below 50em */
display: var(--flex-medium);   /* visible below 50em */
```

**Container query setup:**
```css
.layout_component { container-type: inline-size; }
```

*Source: 002, 008, 017*

---

### Active State Binary — Inheritance Rule

**Critical distinction:**
- **Data Triggers** (hover): inherit from parents — a hovered parent triggers child hover states
- **Data States** (binary active, e.g., `data-state="open"`): do NOT inherit — a checkbox inside an active slider does not appear "checked" merely because the slide is active

*Source: 007*

---

## 2. Utility Classes — Undocumented & Extended

### u-child-contain `[v2.1]`

Forces elements with `max-width` to respect the parent's alignment variables via vertical flex.

**Purpose:** When you set a `max-width` on a text block, it needs a vertical flex context to respond to alignment variables correctly.

```css
.u-child-contain {
    display: flex;
    flex-direction: column;
    align-items: var(--alignment-mode); /* inherits from parent u-alignment-* class */
}
```

**Use case:** Apply to any container that holds max-width constrained text blocks and needs to respond to alignment changes.

*Source: 001, 017*

---

### u-cover `[2026]`

**Replaces CSS background-image for performance and SEO.**

```css
.u-cover {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
}
```

**Implementation:**
```html
<!-- Parent must be position: relative -->
<div class="hero_background" style="position: relative;">
    <img class="u-cover" src="..." alt="Descriptive alt text" loading="lazy">
    <!-- content layered above with z-index -->
</div>
```

**Why img over background-image:**
- WebP conversion applied automatically by Webflow
- Alt text improves SEO and AEO (Answer Engine Optimization)
- Better Core Web Vitals (LCP optimization)
- Eligible for `loading="lazy"` and `fetchpriority` attributes

*Source: 005, 017*

---

### Proportional Scaling — rem for Non-Typography `[v2.1]`

Apply `rem` units to non-typographic elements to maintain design integrity at 200% browser zoom (WCAG 1.4.4):

```css
/* Use rem — NOT px — for these properties */
border-width: 0.0625rem;    /* 1px equivalent */
border-radius: 0.5rem;       /* 8px equivalent */
box-shadow: 0 0.25rem 1rem rgba(0,0,0,0.1);
```

**Rule:** If a design element should scale proportionally when the user increases browser font size, use `rem`. Decorative elements with `px` become disproportionately large or small at high zoom levels.

*Source: 004*

---

## 3. Layout Component — Deep Implementation

### Column 2 Auto-Cleanup Script

The Layout Component includes detection logic that prevents visual cropping when non-visual content is in Column 2:

```javascript
// Logic: Check if Column 2 contains media elements
if (!column2.querySelector('img, video, iframe')) {
    column2.style.aspectRatio = 'unset';
    column2.style.overflow = 'visible';
}
```

**Behavior:**
- If Column 2 has `img`, `video`, or `iframe` → maintains `aspect-ratio` and `overflow: hidden` (prevents media from bleeding)
- If Column 2 has text-only content → unsets `aspect-ratio` and `overflow: hidden` (text flows naturally)

*Source: 002, 008*

---

### 12-Column Grid Implementation

```css
.layout_12col {
    display: grid;
    grid-template-columns: repeat(12, minmax(0, 1fr));
    /* minmax(0, 1fr) is critical — prevents columns from having a minimum content width */
    /* Without this, long words or images can break the grid */
}
```

**Column span via Number Props `[v2.2+]`:**
```css
.layout_component > * {
    grid-column: span var(--column-span, 12);
    /* Default: full width (12). Override via Number Prop per instance */
}
```

*Source: 002, 017*

---

### Sticky-to-Relative Responsive Positioning

Sidebars use sticky positioning at large sizes, switching to relative when the layout wraps. This happens automatically via container query threshold — no custom code required.

```css
/* Handled by Layout Component's threshold variables */
/* Large: position: sticky; top: var(--sticky-top) */
/* Medium and below: position: relative */
```

*Source: 002, 008*

---

## 4. Number Props & calc() Logic

### Data-Number Attribute Mapping `[v2.2+]`

Component props map to numeric CSS variables via `data-[attribute]`:

```html
<!-- Component instance with Number Prop -->
<div class="layout_component" data-columns="3" data-gap="4">
```

```css
/* CSS reads the data attribute */
.layout_component {
    grid-template-columns: repeat(var(--columns, 12), 1fr);
    gap: calc(var(--gap, 0) * 1rem);
}
```

**Inheritance Protection:** Setting a Number Prop to `-1` resets the variable to its initial state, preventing unintended inheritance from parent containers:

```css
/* -1 sentinel value = reset to framework default */
/* Useful when a nested component shouldn't inherit parent grid gap */
```

*Source: 003, 006*

---

### Unit Math Pattern

Multiply numeric props by unit values for type-safe CSS:

```css
/* Text max-width using 'ch' units (character widths) */
max-width: calc(var(--text-width) * 1ch);

/* Spacing using rem */
padding: calc(var(--spacing-value) * 1rem);

/* Overlay opacity */
opacity: calc(var(--overlay-strength) * 0.01); /* prop 0-100 → opacity 0-1 */
```

**Rule:** Keep `calc()` to a **single variable multiplier**. Nested calc() strings cause browser stack errors:

```css
/* ✅ Correct */
padding: calc(var(--space) * 1rem);

/* ❌ Avoid — stack error risk */
padding: calc(calc(var(--space) * 1rem) + calc(var(--extra) * 1rem));
```

*Source: 005, 006*

---

### Nested Grid Gap Inheritance

Child grids inherit the parent's `gap` variable automatically unless an explicit component-level override is applied via Number Prop:

```css
/* Parent grid sets gap */
.section_grid { --grid-gap: 4; }

/* Child grid inherits unless overridden */
.card_grid { 
    gap: calc(var(--grid-gap, 2) * 1rem); 
    /* Without Number Prop override, inherits parent's 4 */
}
```

*Source: 006*

---

## 5. Trigger & State Machine — Advanced

### Binary Variable Logic

The Trigger Machine uses binary numeric variables (`1`/`0`) multiplied by property values to toggle styles entirely within the Webflow style panel:

```css
/* State variables */
--state-true: 1;
--state-false: 0;
--on: 1;
--off: 0;

/* Property only renders when state is active */
transform: translateY(calc(-1rem * var(--on)));

/* Combined — element moves only when --on is 1 */
opacity: calc(0.5 + (0.5 * var(--on)));
/* --on: 0 → opacity: 0.5 | --on: 1 → opacity: 1 */
```

*Source: 007, 017*

---

### Color-Mix State Transitions

Animate between two colors based on state without JS:

```css
/* Full formula */
color: color-mix(
    in srgb,
    var(--color-active) calc(100% * var(--state-true)),
    var(--color-default) calc(100% * var(--state-false))
);

/* When --state-true: 1 → full active color */
/* When --state-true: 0 → full default color */
/* Intermediate values create smooth transitions */
```

**Transparency via color-mix:**
```css
/* 60% opacity without rgba() */
background: color-mix(in srgb, var(--color-brand) 60%, transparent);
```

*Source: 007, 014*

---

### data-trigger vs data-state

| Attribute | Type | Inheritance | Use case |
|-----------|------|-------------|----------|
| `data-trigger` | Hover/focus | Inherits from parent | Hover cards, focus rings |
| `data-state="open"` | Binary toggle | Does NOT inherit | Nav menus, modals |
| `data-current` | Active indicator | Does NOT inherit | Nav links, tabs |

**Touch device fix:** `data-trigger` attributes resolve the iOS "hover flash" bug that occurs when using native `:hover` pseudo-classes on touch devices.

*Source: 007*

---

### Hamburger Transform Math

SVG hamburger lines use variable-based transforms for precise animation:

```css
/* Line thickness drives transform calculation */
--line-height: 2px; /* in rem */
--line-gap: 8px;    /* in rem */

/* Top line rotation origin */
transform-origin: center;
transform: rotate(calc(45deg * var(--on))) 
           translateY(calc(var(--line-gap) * var(--on)));
```

*Source: 007*

---

## 6. Conditional Visibility — 2026 Engine

### Prop-Driven Visibility

Elements are bound to `Boolean` or `Option` props and removed from the DOM when conditions are not met (SSR/server-side):

**Types:**
- `Boolean` prop → Show/hide single element
- `Option` prop → Switch between variants based on value match

**Implementation:**
```
Component Property: "Show Badge" (Boolean)
Element: .badge_wrap
Condition: Show if "Show Badge" = true
Result: Element omitted from DOM when false (not just hidden — improves Core Web Vitals)
```

*Source: 009*

---

### The Wrapper Visibility Pattern

Use a visibility condition on the **parent container** that triggers only if **child content exists**:

```
.u-button-wrapper [Visible if: "Button Link" prop is not empty]
└── .u-button [content]
```

**Why wrapper, not element:** If the element itself has padding/margin, hiding it still affects layout. Hiding the wrapper removes it from flow entirely.

*Source: 009*

---

### Variant Switching via Conditions

Logic can switch between component **variants** based on conditional matches:

```
If "Theme" prop == "Dark" → show Variant: Dark
If "Theme" prop == "Light" → show Variant: Light
```

This replaces manually managing multiple visibility rules for alternate visual states.

*Source: 009*

---

### AND/OR Multi-Condition Logic

2026 Webflow supports compound conditions:

```
Show if: "Title" is not empty AND "Description" is not empty
Hide if: "Badge" is empty OR "Badge Count" = 0
```

*Source: 009*

---

## 7. Interactions & Animations

### Horizontal Scroll — Full Implementation

**DOM Structure:**
```html
<div class="scroll-track_parent" style="height: 300vh;">
    <!-- Height defines scroll duration -->
    <div class="scroll-track_sticky" style="position: sticky; top: 0; overflow: hidden;">
        <div class="scroll-track_flex" style="display: flex;">
            <!-- Cards as flex children -->
        </div>
    </div>
</div>
```

**IX Setup:**
- Trigger: "While scrolling in view" on `.scroll-track_parent`
- Action: Move `.scroll-track_flex` from `0%` to `-100%` (translateX)
- Unit: `%` (not px — ensures it scales with content count)

**Adjustment for last panel:**
The track moves `translateX(-100% + 100vw)` to ensure the last panel is fully visible. This correction prevents the last card from stopping mid-viewport.

**Mobile:** Disable the IX at small container sizes using Lumos threshold variables — fall back to normal vertical scroll.

*Source: 010*

---

### List/Grid View Toggle with localStorage

**Pattern:**

```javascript
// On DOMContentLoaded — apply saved state instantly
document.addEventListener('DOMContentLoaded', () => {
    const savedView = localStorage.getItem('viewMode') || 'grid';
    document.querySelector('.collection_wrap').classList.add(`is-${savedView}`);
});

// On toggle button click
toggleBtn.addEventListener('click', () => {
    const current = localStorage.getItem('viewMode') === 'list' ? 'grid' : 'list';
    localStorage.setItem('viewMode', current);
    container.classList.toggle('is-list');
    container.classList.toggle('is-grid');
});
```

**CSS state classes:**
```css
.collection_wrap.is-grid { 
    display: grid; 
    grid-template-columns: repeat(3, 1fr); 
}
.collection_wrap.is-list { 
    display: grid; 
    grid-template-columns: 1fr; 
}
```

**Webflow IX layer:** Animate the class toggle with Native IX for smooth visual transitions. The JS handles state persistence; IX handles the visual animation.

*Source: 012*

---

### Accordion — Grid-Template-Rows Animation

Native auto-height accordion without JS height calculations:

```css
.accordion_content {
    display: grid;
    grid-template-rows: 0fr;         /* Closed: 0 height */
    transition: grid-template-rows 0.3s ease;
    overflow: hidden;
}

.accordion_content > * {
    min-height: 0; /* Required — children must allow collapse */
}

/* When open (via is-open class or data-state) */
.accordion_wrap.is-open .accordion_content {
    grid-template-rows: 1fr;         /* Open: natural height */
}
```

**Why this works:** `grid-template-rows` is animatable. The inner element collapses because its `min-height: 0` allows it. No JS height measurement needed.

*Source: 018, 023*

---

### Inline SVG with Text (display: contents)

Make SVG behave as an inline character within a text string:

```html
<p class="hero_heading">
    Build faster
    <span class="arrow_wrap">  <!-- display: contents -->
        <svg class="arrow_icon">...</svg>  <!-- display: inline-block -->
    </span>
    with Lumos
</p>
```

```css
.arrow_wrap {
    display: contents; /* Wrapper becomes invisible to layout — SVG joins text flow */
}
.arrow_icon {
    display: inline-block;
    margin-inline: 0.25em; /* Character-width spacing */
    vertical-align: middle;
}
```

*Source: 023*

---

### Infinite Stacked Card Animation — GSAP

**DOM Structure:**
```html
<div class="cards_parent" style="height: 400vh;">
    <div class="cards_sticky" style="position: sticky; top: 0;">
        <div class="card" data-index="0">...</div>
        <div class="card" data-index="1">...</div>
        <div class="card" data-index="2">...</div>
    </div>
</div>
```

**Index-based offset:**
```css
.card {
    top: calc(var(--card-index) * 2rem);
    /* Each card sits 2rem lower than the previous while stuck */
}
```

**GSAP ScrollTrigger pattern:**
```javascript
gsap.context(() => {
    cards.forEach((card, i) => {
        if (i === cards.length - 1) return; // Last card doesn't animate out
        
        gsap.to(card, {
            scale: 0.9,
            opacity: 0,
            scrollTrigger: {
                trigger: card,
                start: "top top",
                end: "bottom top",
                scrub: true,
            }
        });
    });
});
```

**Cleanup:** Always use `gsap.context()` to kill and re-initialize animations on resize/breakpoint changes.

*Source: 027, 015*

---

### GSAP — data-gsap-trigger Attribute Pattern

Target elements for GSAP without hardcoded class selectors:

```html
<div data-gsap-trigger="spotlight">...</div>
<div data-gsap-trigger="parallax">...</div>
```

```javascript
// JS reads attributes — no class dependency
const spotlight = document.querySelector('[data-gsap-trigger="spotlight"]');
gsap.to(spotlight, { ... });
```

**Benefit:** Decouples animation logic from CSS class names. Classes can change without breaking JS.

*Source: 015*

---

## 8. CMS Integration Patterns

### Swiper.js CMS Slider — Full Pattern

**Required Webflow class structure:**
```html
<div class="cms-slider_component swiper">
    <div class="cms-slider_list swiper-wrapper">
        <!-- CMS Collection List items get class: swiper-slide -->
        <div class="cms-slider_item swiper-slide">...</div>
    </div>
</div>
```

**Gutter Sync with Lumos variables:**
```css
/* Slide padding synced to site gutter */
.swiper-slide {
    padding-inline: calc(var(--site-gutter) * 0.5);
}

/* Wrapper negative margin aligns first slide with container edge */
.swiper {
    margin-inline: calc(var(--site-gutter) * -0.5);
    overflow: visible; /* Allows slides to bleed edge */
}
```

**Specificity — overriding Swiper inline styles:**
```css
/* Swiper injects inline styles. Use combo class to override */
.cms-slider_component.is-swiper .swiper-slide {
    height: auto !important; /* Force auto height */
    width: var(--slide-width); /* Custom width */
}
```

**JS Initialization (scoped to component):**
```javascript
const swiperEl = document.querySelector('.cms-slider_component');
new Swiper(swiperEl, {
    slidesPerView: 'auto',
    followFinger: true,
    freeMode: true,
    watchSlidesProgress: true,
    navigation: {
        nextEl: '.slider_btn-next',
        prevEl: '.slider_btn-prev',
    }
});
```

*Source: 020*

---

### Interactive Components — CMS Binding

**Slider state syncing across instances:**

Use `data-state` attribute to sync visual indicators (dots, counters) across separate slider instances:

```html
<div class="slider_component" data-state="0">
    <!-- data-state = current slide index -->
</div>
<div class="slider_dots">
    <div class="dot is-active" data-target="0"></div>
    <div class="dot" data-target="1"></div>
</div>
```

```javascript
// Sync dots to slider state
slider.addEventListener('slideChange', (e) => {
    document.querySelector('.slider_component').dataset.state = e.activeIndex;
    dots.forEach(dot => dot.classList.toggle('is-active', 
        dot.dataset.target == e.activeIndex));
});
```

**Accordion Math — Binary State:**
```css
.accordion_item { --is-open: 0; }
.accordion_item.is-open { --is-open: 1; }

.accordion_icon {
    transform: rotate(calc(45deg * var(--is-open)));
    /* 0 = 0deg (closed) | 1 = 45deg (open) */
}
```

*Source: 018*

---

## 9. Component System Governance

### The Wrapper Div Rule

**Never convert a functional element directly into a component.**

```
❌ Wrong: Select <button> → Make Component
✅ Correct: Wrap <button> in <div> → Select <div> → Make Component
```

**Why:** The parent div holds component identity. The child class (`button`) remains a global class, allowing sitewide updates to the button style without detaching instances.

**Rule:** The component boundary should always be a structurally neutral wrapper, never a functional element.

*Source: 019*

---

### Component Prop Organization

Group component props by functional area for client-friendliness and LLM auditability:

```
Group: "Content"
  ├── Title (Text)
  ├── Description (Text)
  └── Link (Link)

Group: "Visibility"
  ├── Show Badge (Boolean)
  ├── Show Button (Boolean)
  └── Show Icon (Boolean)

Group: "Style"
  ├── Theme (Option: Light, Dark, Brand)
  ├── Size (Option: Small, Medium, Large)
  └── Max Width (Number)
```

**Benefit:** Clients only see relevant controls. LLM audits can identify prop groups for systematic updates.

*Source: 019*

---

### The Universal Component Pattern

Build components with exhaustive Props so a single component serves multiple visual intents across the site:

```
Section_Master component:
├── Layout prop (Number: 1-12 column span)
├── Theme prop (Option: Light/Dark/Brand)
├── Title prop (Text)
├── Body prop (Rich Text)
├── Media prop (Image/Video)
├── CTA Link prop (Link)
├── CTA Label prop (Text)
├── Show CTA (Boolean)
├── Reverse Layout (Boolean)
└── Padding Top/Bottom (Number)
```

**Result:** One component powers 50+ unique page sections by varying props, not by creating new components. Reduces class bloat, improves sitewide consistency.

*Source: 013, 019*

---

### Global Style Changes Inside Components

When modifying base classes inside a component, use "Shift + Up Arrow" (Lumos Chrome Extension) to select the base class specifically. This ensures responsive changes propagate to all combo-class instances rather than only affecting the component-level override.

*Source: 014, 019*

---

## 10. AI-to-Webflow Workflow

### The Master Prompt (Production-Ready)

Use this exact prompt structure for AI-generated code compatible with Lumos:

> *"Write vanilla HTML, CSS, and JS using REM units instead of PX (16px = 1rem base), BEM-style class naming with underscores (e.g., `.section_header-wrapper`), class-only styling (no element selectors or IDs), no descendant selectors, and simple selectors. Avoid heavy utility class usage and complex pseudo-selectors. Use `display: grid` with `grid-template-columns: repeat(12, 1fr)` for layouts. Keep `<style>` blocks separate from HTML structure."*

*Source: 026, 011*

---

### Post-Injection Binding Checklist

After pasting AI-generated code into Webflow via Moden Club:

```
□ Spacing: Bind padding/margin → --section-spacing, --site-margin
□ Colors: Replace hex codes → var(--color-dark), var(--color-light)
□ Typography: Replace font-size px → var(--font-medium) or rem
□ Containers: Wrap content in .u-container to inherit max-widths
□ Grid: Ensure columns use minmax(0, 1fr) not just 1fr
□ Borders/Radius: Convert px → rem for proportional scaling
```

*Source: 011, 026*

---

### Moden Club Tool

**Purpose:** HTML-to-Webflow clipboard converter. Pastes AI-generated or manually written HTML/CSS directly into the Webflow Designer.

**Workflow:**
1. AI generates HTML + CSS (using master prompt above)
2. Paste into Moden Club clipboard tool
3. Click paste target in Webflow Designer
4. Structure appears as Webflow elements
5. Bind variables manually (colors, spacing)

**Requirement:** Style blocks must be separate from HTML (not inline styles) for clean parsing.

*Source: 011, 026*

---

### Variable Binding Priority (Post-AI)

Order of operations when binding AI code to Lumos variables:

1. **Colors first** — most visible, prevents design drift
2. **Spacing second** — padding/margin/gap → Lumos space variables
3. **Typography third** — font-size/weight → Lumos type variables
4. **Structure last** — max-width, columns → structural variables

*Source: 025, 026*

---

## 11. Chrome Extension Shortcuts

**Extension:** Lumos Chrome Extension (v5.1+)  
**Install:** https://lumos.timothyricks.com/chrome-extension

### Key Shortcuts

| Input | Action | Result |
|-------|--------|--------|
| `Number` + `Space` | PX to REM | `16` → `1rem` (divides by 16) |
| `Target/Container` + `Space` | % Width | `562/1440` → `39.02%` |
| `--var-name` + `Tab` | Variable wrap | `--color-brand` → `var(--color-brand)` |
| `--variable%60` + `Tab` | Transparency | → `color-mix(in srgb, var(--variable) 60%, transparent)` |
| `Shift` + `Up Arrow` | Select base class | Navigates from combo class to parent base class |
| `Right Arrow` (in selector) | Auto-fill parent name | Auto-suggests parent component name for BEM naming |

### Style Inheritance via Shift+Up

Using Shift+Up to select the base class before making responsive changes ensures the change propagates correctly to **all combo-class instances**. Without this, responsive changes apply only to the specific class being edited, breaking other instances.

*Source: 014*

---

## 12. Lumos Libraries & Migration

### Variable ID Mapping — Critical Rule

Lumos components rely on specific GUIDs for "Theme" and "Typography" variable collections. If IDs mismatch between the library and local project, Webflow creates duplicate classes with numerical suffixes:

```
❌ Result of ID mismatch:
.u-section 2
.u-container 3
.u-margin-trim 2
```

**Prevention:** Before importing, audit the local Variables panel. Ensure collection names and IDs match the library source.

*Source: 021*

---

### Migration Strategy — Older Projects

For projects predating v2.2 Library structure:

**Preferred: Copy/Paste + Manual Rebind**
```
1. Copy individual component from Library source
2. Paste into project
3. Manually rebind variables to local collection
4. Test all states and responsive behavior
```

**Avoid: Bulk Library Sync**
- Bulk sync causes class duplication if variable IDs don't match
- Results in a bloated CSS head that's difficult to clean
- Reserve bulk sync for new projects only

*Source: 021*

---

### Converting Library Components to Site Components

When bringing a library component into an older project:

1. Insert library component
2. Right-click → "Convert to Site Component"
3. Component detaches from global library source
4. Now editable locally without affecting the library

**When to do this:** When you need to customize a component beyond what props allow, or when the project is on an older variable structure.

*Source: 021*

---

## 13. Figma-to-Webflow Workflow

### Moden Club Layout Wizard

**Purpose:** Converts Figma absolute-positioned layouts into responsive CSS using `em` units relative to container width.

**Output:** CSS using `translate` with `em` units, anchored to container threshold variables. This keeps collage-style layouts in sync as the container resizes.

**Workflow:**
1. Define bounding box (parent container dimensions)
2. Set anchor points for each element (top-left, center, bottom-right)
3. Export → CSS copied to clipboard
4. Paste into Webflow Custom Properties panel
5. Set parent to `container-type: inline-size`

**Responsive Anchor Rule:** When an element is "Anchored Bottom," the transform origin shifts to ensure additional text in the parent pushes the graphic without breaking alignment.

*Source: 022*

---

### Figma Reset

Double-clicking an element in the Layout Wizard resets it:
```
transform: translate(0, 0) rotate(0deg)
```

*Source: 022*

---

## 14. Performance & Semantic Standards (2026)

### Z-Index Layering System

```
z-index: 1000  → Fixed nav (always on top)
z-index: 100   → Modals and overlays
z-index: 10    → Sticky elements (non-nav)
z-index: 1     → Elevated cards/components
z-index: 0     → Default flow
z-index: -1    → Background elements (behind content)
```

**Rule:** `position: fixed` nav must use `z-index: 1000` to stay above all `position: relative` sections.

*Source: 005*

---

### Margin Collapsing — Enforced Pattern

Use margin-based spacing (not padding) between top-level components to allow native CSS margin collapsing:

```css
/* ✅ Use margin for vertical spacing between sections */
.section_wrap {
    margin-top: var(--section-spacing);
    margin-bottom: var(--section-spacing);
}

/* Adjacent sections collapse to single margin automatically */
/* No double-spacing between components */
```

**Why not padding:** Padding doesn't collapse. Using `padding-top` + `padding-bottom` on adjacent sections creates double the intended spacing.

*Source: 005*

---

### Variable Governance — Panel Assignment

| Panel | Use For |
|-------|---------|
| **Style Panel** | Standard variable binding (Color, Font, static values) |
| **Custom Properties Panel** | Dynamic container queries (`display: var(--flex-medium, grid)`), calc() strings |

**Do not mix:** Binding container query variables through the Style Panel loses the dynamic cascade logic. Use Custom Properties for any value that changes based on container width.

*Source: 005*

---

### Background Parallax — "Bottom-of-View" Pattern

For scroll-based scale/rotate transforms on background elements:

```
IX Trigger: "While scrolling in view" 
Start: Element enters viewport bottom
End: Element exits viewport top
Action: Scale 1.1 → 1.0, Rotate 5deg → 0deg
```

**Performance:** Apply transforms only to elements with `will-change: transform` to trigger GPU compositing. Avoid triggering on layout properties (width, height, top/left).

*Source: 005*

---

## 15. Deprecation Log

### Removed in v2.2.0

| Class | Replacement |
|-------|-------------|
| `u-grid-above` | `var(--flex-medium)` in Custom Properties panel |
| `u-grid-below` | `var(--none-medium)` in Custom Properties panel |

### Removed in v2.2.2

| Item | Replacement |
|------|-------------|
| `~50 order utilities` (`u-order-first-mobile`, etc.) | Data attributes `[data-medium-columns]`, or container queries |
| `Button Style variable collection` | Direct Theme collection reference (`--theme--button-primary--background`) |

**Active State Binary Variables — Renamed:**

| Old | New |
|-----|-----|
| `--on` / `--off` | `--state-true` / `--state-false` |

*(Both may be in use depending on project version. `--on`/`--off` syntax still valid in v2.2.x projects)*

*Source: 016*

---

## 16. Logic Cross-Reference Table

| Utility Class | Controlling Variable | Behavior |
|---------------|---------------------|----------|
| `.u-section` | `--section-spacing` | Drives top/bottom fluid padding |
| `.u-container` | `--site-margin` | `width: calc(100% - var(--site-margin) * 2)` |
| `.u-child-contain` | `--alignment-mode` | Forces max-width text to respect parent flex-align |
| `.u-margin-trim` | `:first-child` / `:last-child` | Clears margin-top on first, margin-bottom on last child |
| `.u-cover` | `object-fit: cover` | Replaces background-image with positioned img element |
| `.layout_component` | `--column-span` | `grid-column: span var(--column-span, 12)` |
| `[data-state="open"]` | `--state-true` / `--state-false` | Binary state toggle for interactive components |
| `color-mix()` | `--state-true` | Animated color transitions without JS |
| `display: var(--flex-medium, grid)` | `--threshold-medium` (50em) | Layout swap at container width threshold |

*Source: 017*

---

## Quick-Reference: When to Use What

**Responsive layout:**
→ Use container queries (`var(--flex-medium)`) NOT viewport breakpoints

**Image backgrounds:**
→ Use `<img class="u-cover">` NOT CSS `background-image`

**Accordion open/close:**
→ Use `grid-template-rows: 0fr → 1fr` NOT JS height calculation

**State transitions:**
→ Use `color-mix()` with binary variables NOT JS class toggle

**CMS Slider:**
→ Use Swiper.js with Lumos gutter sync NOT Webflow's native slider for CMS

**AI-generated code:**
→ Run through Moden Club after generation, always rebind variables

**New component:**
→ Always wrap in neutral div first, then componentize

**Library import into older project:**
→ Copy/paste + manual rebind, NOT bulk library sync

---

**Document Version:** 1.0  
**Covers:** Lumos v2.1 through v2.2.2  
**Source Videos:** 27 Timothy Ricks tutorials (2024–2026)  
**Classification:** Supplemental — use alongside `lumos-complete-reference.md`
