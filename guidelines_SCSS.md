# AI Guidelines & Rules by Niek | SCSS

---

## General

- never use third party CSS libraries unless absolutely necessary, except for SCSS
- keep all SCSS consistent and follow the same style across the entire codebase
- always use `rem` for spacing and sizing, never `px` (exception: borders, box-shadows, and font-size in base `html`)
- avoid inline styles in components, all styling belongs in SCSS files; only use inline styles when the value is truly dynamic (e.g. calculated from data or index) and expressing it with class modifiers would require an impractical number of classes to cover every possible variation

---

## Styles Folder Structure

- keep a clear separation between global shared styles and component / page specific styles
- global partials are prefixed with `_` and imported through a single `global.scss` entry file
- component and page files import what they need directly using `@use`

**Example:**

```
└── styles
    ├── global
    │   ├── _animations.scss
    │   ├── _colors.scss
    │   ├── _layout.scss
    │   ├── _spacing.scss
    │   ├── _typography.scss
    │   └── global.scss
    ├── site
    │   ├── landing.scss
    │   ├── login.scss
    │   └── register.scss
    └── components
        ├── footer.scss
        └── header.scss
```

---

## global.scss

- `global.scss` is the single entry point for all global styles, imported once in `main.tsx`
- it uses `@use` to pull in all partials and applies the CSS reset and base `html` styles
- never put component or page specific styles here

**Example:**

```scss
@use './colors' as *;
@use './typography' as *;
@use './spacing' as *;
@use './layout' as *;
@use './animations' as *;

*,
*::before,
*::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html {
    font-family: 'Nexa', sans-serif;
    font-size: 16px;
    color: $color-black;
    font-weight: 400;
    line-height: 1;
    letter-spacing: 0.02em;
    caret-color: $color-black;
    text-underline-offset: 2px;
}

input,
textarea,
select,
button {
    font: inherit;
}
```

---

## Variables

### Colors (`_colors.scss`)

- all colors are SCSS variables prefixed with `$color-` or `$background-`
- define every color used in the project here, never hardcode hex values outside this file
- keep them simple and readable, don't use `$color-lightgreen-text` never use `text-green` either

**Example:**

```scss
$background-site: #fbfff7;
$background-widget: #ffffff;

$color-black: #2c2c2c;
$color-blue: #94c6ff;
$color-green: #9ee8c0;
$color-grey: #75706f;
$color-lightblue: #ecf8ff;
$color-lightgreen: #f3ffe9;
$color-lightgrey: #949494;
$color-red: #ff8080;
```

### Spacing (`_spacing.scss`)

- border-radius values use a size suffix: `-xs`, `-sm`, `-md`, `-lg`

**Example:**

```scss
$border-radius-xs: 15px;
$border-radius-sm: 25px;
$border-radius-md: 30px;
```

---

## Typography (`_typography.scss`)

- all custom fonts are declared with `@font-face` here
- reference font files from `../../assets/fonts/` using the correct format string

**Example:**

```scss
@font-face {
    font-family: 'Nexa';
    src: url('../../assets/fonts/Nexa-Trial-Regular.ttf') format('truetype');
    font-style: normal;
}

@font-face {
    font-family: 'Gelica';
    src: url('../../assets/fonts/Gelica-Trial-Regular.otf') format('opentype');
    font-style: normal;
}
```

---

## Breakpoints & Mixins (`_layout.scss`)

- breakpoints are defined in a `$breakpoints-width` map with named keys
- use the `respond-up-width` mixin for mobile-first responsive styles
- always write mobile styles first, then override with the mixin for larger screens

**Example:**

```scss
@use 'sass:map';

$breakpoints-width: (
    mobile-small: 320px,
    mobile-medium: 440px,
    mobile-large: 540px,
    tablet-small: 640px,
    tablet-large: 740px,
    desktop: 1140px,
);

@mixin respond-up-width($size) {
    $breakpoint: map.get($breakpoints-width, $size);

    @media (min-width: $breakpoint) {
        @content;
    }
}
```

```scss
// usage
.header {
    padding-inline: 1rem;

    @include respond-up-width(tablet-large) {
        padding-inline: 2.5rem;
    }
}
```

---

## Animations (`_animations.scss`)

- all `@keyframes` definitions live in `_animations.scss`
- name keyframes in PascalCase after the element or effect they animate

**Example:**

```scss
@keyframes Background {
    from { opacity: 0; }
    to { opacity: 1; }
}

@keyframes Medicine {
    from {
        opacity: 0;
        transform: translateY(0.5rem);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
```

---

## Import Style

- always use `@use`, never `@import` (deprecated in modern SCSS)
- use `as *` to expose variables and mixins without a namespace prefix
- import only what a file actually uses, component files import directly from the global partials, not through `global.scss`

**Example:**

```scss
@use '../global/colors' as *;
@use '../global/typography' as *;
@use '../global/spacing' as *;
@use '../global/layout' as *;
@use '../global/animations' as *;
```

---

## General Coding Style

- always use a 4 space tab index
- use empty lines to visually separate logical groups of properties within a rule, group them by concern, not one line between every property
- separate distinct logical sections (positioning, layout, sizing, visuals, typography, interaction) with an **empty line between each group**

**Property order within a rule:**

1. **Box Model / Spacing** — `margin`, `padding`, `box-sizing`
2. **Layout & Positioning** — `display`, `flex` / `grid`, `gap`, `align-items`, `justify-content`, `position`, `top` / `right` / `bottom` / `left`, `z-index`, `overflow`
3. **Sizing** — `width`, `height`, `min-width`, `max-width`, `min-height`, `max-height`, `aspect-ratio`
4. **Typography** — `font` / `font-family`, `font-size`, `font-weight`, `line-height`, `letter-spacing`, `text-align`, `color`, `white-space`
5. **Visual** — `background`, `border`, `border-radius`, `box-shadow`, `opacity`
6. **Interaction / Effects** — `cursor`, `transition`, `transform`, `animation`

**Example:**

```scss
.container_login {
    padding-top: 3.125rem;
    padding-bottom: 3.75rem;

    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    z-index: 10;

    width: 33rem;
    height: 26.25rem;

    font-size: 16px;
    color: $color-black;

    background-color: rgba($color-lightblue, 0.5);
    border-radius: $border-radius-md;
    box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.05);

    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    transition: opacity 0.3s ease-in-out;
}
```

---

## Nesting & Selectors

- use nesting for child elements and state variants (`&:hover`, `&:focus`, `&.active`, `&.modifier`)
- keep nesting to a maximum of 3 levels deep, deeper nesting is a sign the structure should be split
- use `&::placeholder`, `&:focus`, `&:hover` for pseudo-classes and pseudo-elements
- modifier classes (e.g. `.input_error`, `.visible`, `.show`) are applied via `&.modifier` nesting

**Example:**

```scss
.input_email {
    border: 2px solid $color-grey;
    border-radius: $border-radius-md;
    transition: border-color 0.3s ease-in-out;

    &::placeholder {
        color: $color-grey;
        user-select: none;
    }

    &:focus,
    &:hover {
        border: 2px solid $color-black;
    }

    &.input_error {
        border: 2px solid $color-red;
    }
}
```

---

## Naming

- class names follow the same prefix convention as JSX: `container_*`, `icon_*`, `link_*`, `button_*`, `image_*`, `text_*`
- modifier / state classes are lowercase: `.active`, `.visible`, `.show`, `.error`, `.rotate`
- keyframe names are PascalCase: `Background`, `Medicine`, `Navigation`
- SCSS variable names are kebab-case: `$color-black`, `$border-radius-md`, `$background-site`

---

## Transitions

- always use `transition` with an explicit property, duration, and easing; never `transition: all`
- default easing is `ease-in-out`; use `ease` for subtle fades
- when transitioning multiple properties, list them comma-separated

**Example:**

```scss
// single property
transition: border-color 0.3s ease-in-out;

// multiple properties
transition:
    opacity 0.5s ease 0.3s,
    max-height 0.3s ease,
    height 0.3s ease;
```

---

## Glassmorphism / Blur Panels

- always pair `backdrop-filter` with `-webkit-backdrop-filter` for Safari support
- use `box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.05)` as a subtle border instead of a real border on blur panels

**Example:**

```scss
background-color: rgba($color-lightblue, 0.5);
backdrop-filter: blur(10px);
-webkit-backdrop-filter: blur(10px);

border-radius: $border-radius-xs;
box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.05);
```

---

## Inputs & Buttons

- always reset inputs with `appearance: none; -webkit-appearance: none;` to remove browser defaults
- use `all: unset; box-sizing: border-box;` on buttons that need a fully clean base
- always set `outline: none` on inputs and handle focus state manually with `border` or `box-shadow`

**Example:**

```scss
.input_email {
    outline: none;
    appearance: none;
    -webkit-appearance: none;
    box-sizing: border-box;
}

.button_submit {
    all: unset;
    box-sizing: border-box;
}
```

---

## Comments

- same rule as in TypeScript: only comment the *why*, not the *what*
- use comments to mark logical sections within large files if needed, but keep them minimal
