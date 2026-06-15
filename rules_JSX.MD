# AI Guidelines & Rules — Niek | JSX

---

## General

- Avoid using third-party libraries

---

## Import Style

**Order:**

1. Standard libraries (Node, etc.)
2. Third-party imports — in order from framework down to smaller libraries (React, React Router, Jotai, etc.)
3. Absolute project imports (`/global/..`, config, utils, etc.)
4. Relative imports (`../components/..` — Header.tsx, Navigation.tsx, etc.)
5. Image imports
6. Style imports

**Rules:**

- Imports must always follow the same hierarchy as they appear in the code, top to bottom
- Always use an empty line between each group/section
- Imports must be alphabetically ordered within the same group
- Type-only imports can be grouped with their module, prefixed by `type`
- Remove unused imports
- Always use a space inside braces

**Example:**

```tsx
import React from 'react'
import { useState, useEffect } from 'react'
import { useNavigate, NavLink } from 'react-router-dom'
import { Helmet } from 'react-helmet-async'

import { useAtom } from 'jotai'
import { emailRegexAtom, languageAtom } from '../../global/atoms/atoms'

import SwitchLanguage from '../../components/SwitchLanguage'

import { authApi } from '../../global/api/api'

import { type MessageKey, messages, locales } from '../../global/translations'

import { Logo, PasswordHide, PasswordShow, ArrowIcon } from '../../assets'

import '../../styles/global/global.scss'
import '../../styles/site/login.scss'
```

---

## General Coding Style

- Never use semicolons at the end of lines
- Always use a 4-space tab index
- Use single quotes `'` in TypeScript/JavaScript, double quotes `"` in JSX attributes
- Keep hooks and state declarations at the top of the component, followed by helpers, then the JSX `return`
- Use template literals for dynamic classNames: `` className={`input ${hasError ? 'input_error' : ''}`} ``
- Limit lines to a readable length and break long JSX props over multiple lines
- Don't use inline styles

### JSX Templating Style

- Use empty lines to visually separate logical sections of code
- Use clear class prefixes to express role and hierarchy — e.g. `container_*` for layout wrappers, `link_*` for navigation links, `icon_*` for icons, etc.
- Use double quotes for JSX attributes
- Break JSX props onto multiple lines when there are more than 3 props
- `onClick`, `onSubmit`, `onChange` always go at the beginning; `className` always goes last for best readability

**Example:**

```tsx
<div className="container_login">
    <h1 className="title">{locale.login.title}</h1>

    <form noValidate onSubmit={login} className="form_login">
        <input
            onChange={e => setEmail(e.target.value)}
            type="email"
            spellCheck="false"
            placeholder={locale.login.email}
            className={`input_email ${emailError ? 'input_error' : ''}`}
        />

        <div className="container_input">
            <input
                onChange={e => setPassword(e.target.value)}
                type={showPassword ? 'text' : 'password'}
                placeholder={locale.login.password}
                className={`input_password ${passwordError ? 'input_error' : ''}`}
            />

            <img onClick={toggleShowPassword} src={showPassword ? PasswordShow : PasswordHide} className="icon_TogglePassword" />
        </div>

        <span className={`text_error ${errorMessage ? 'visible' : ''}`}>{errorMessage}</span>

        <div className="container_button">
            <button type="submit" className="button_submit">
                {locale.login.submit}
            </button>

            <img src={ArrowIcon} className="icon_Arrow" />
        </div>

        <div className="container_links">
            <NavLink to={'/forgot'} className="link_forgot">
                {locale.login.forgot_password}
            </NavLink>

            <NavLink to={'/register'} className="link_register">
                {locale.login.register_link}
            </NavLink>
        </div>
    </form>
</div>
```

```tsx
<input
    value={email}
    onChange={handleEmailChange}
    type="email"
    spellCheck="false"
    placeholder={locale.login.emailPlaceholder}
    className={emailError ? 'input_error' : ''}
/>

// or:

<img alt="Vetly" src={Logo} className="image_Logo" />
```

---

## Conditional Rendering

- Do not use complex logic directly inside JSX
- Move calculations, filters, and conditions into variables above the `return`
- Use ternary operators only for simple two-state rendering
- Use variables for multiple conditions or nested logic
- Avoid nested ternary operators inside JSX
- Prefer early returns for completely different layouts or states
- Use `&&` only when rendering a component or element based on a condition being true
- Never use `&&` with values that can be `0`, `null`, or `undefined` unless the behavior is intended
- Avoid rendering empty fragments (`<></>`) only to wrap a single element
- Keep conditional sections separated with empty lines for readability

**Examples:**

```tsx
const activeUsers = users.filter(user => user.active)

<div>{activeUsers.length}</div>
```

```tsx
const status = loading ? 'loading' : error ? 'error' : 'done'
```

---

## Component Structure

- All hooks — e.g. `useState`, `useEffect`, `useAtom` — must always be declared at the top of the function and grouped by related state; never use hooks inside `if`/`else` or loops
- Call event handlers always by name: `handleSubmit`, `handleEmailChange`, `toggleShowPassword`, or something else that makes the action clear
- Always use `e.preventDefault()` on `<form />`
- Every component file must follow this order:

1. Imports
2. Types / interfaces
3. Constants
4. Component function
   - Hooks + state
   - Derived values
   - Helper functions
   - Event handlers
   - JSX return
5. `export default`

**Example:**

```tsx
const [email, setEmail] = useState('')
const [emailError, setEmailError] = useState(false)
```

---

## Naming

- Components must be in PascalCase — e.g. `UserCard.tsx`, `SearchBar.tsx`
- Utilities must be in camelCase — e.g. `formatDate.ts`, `validateEmail.ts`
- Single-word variables must be in lowercase; multi-word variables must be in camelCase

**Examples:**

```tsx
const result = await authApi.Forgot(email)
```

```tsx
const [errorMessage, setErrorMessage] = useState('')
```

---

## Comments

- Comments must explain *why* something exists, not *what* it does — the code should be self-explanatory
- Remove unnecessary comments
- Always keep comments minimal; don't overpollute the code with comments

---

## Folder Structure

Keep a clear separation between:

- `pages` — page components that represent routes
- `components` — reusable components
- `global` — shared application logic
- `assets` — static files
- `styles` — SCSS files
- If there are multiple apps in the same project, create an `/apps` folder and keep each app in a separate folder

**Example:**

```
├── eslint.config.js
├── index.html
├── package.json
├── src
│   ├── apps
│   │   ├── site
│   │   │   ├── Forgot.tsx
│   │   │   ├── Landing.tsx
│   │   │   ├── Login.tsx
│   │   │   ├── Register.tsx
│   │   │   ├── Reset.tsx
│   │   │   └── Verify.tsx
│   │   └── widget
│   │       └── Widget.tsx
│   ├── assets
│   │   ├── backgrounds
│   │   │   ├── background-cats.svg
│   │   │   ├── background-large.svg
│   │   │   └── background-small.svg
│   │   ├── fonts
│   │   │   ├── Gelica-Trial-Regular.otf
│   │   │   └── Nexa-Trial-Regular.ttf
│   │   ├── images
│   │   │   ├── cat.png
│   │   │   ├── searchbar.svg
│   │   │   ├── stars.svg
│   │   │   ├── sustainable.svg
│   │   │   └── video-placeholder.png
│   │   ├── index.ts
│   │   ├── logo
│   │   │   └── logo.svg
│   │   └── ui
│   │       ├── arrow-toggle.svg
│   │       ├── arrow.svg
│   │       ├── password-hide.svg
│   │       ├── password-show.svg
│   │       ├── search.svg
│   │       └── whatsapp.svg
│   ├── components
│   │   ├── Footer.tsx
│   │   ├── Header.tsx
│   │   ├── Search.tsx
│   │   └── SwitchLanguage.tsx
│   ├── global
│   │   ├── api
│   │   │   └── api.ts
│   │   ├── atoms
│   │   │   └── atoms.ts
│   │   ├── mappings
│   │   │   └── doeldieren.ts
│   │   ├── router
│   │   │   └── router.tsx
│   │   ├── translations
│   │   │   ├── index.ts
│   │   │   ├── locales.ts
│   │   │   └── messages.ts
│   │   └── types
│   │       └── types.ts
│   ├── main.tsx
│   └── styles
│       ├── global
│       │   ├── _animations.scss
│       │   ├── _colors.scss
│       │   ├── _layout.scss
│       │   ├── _spacing.scss
│       │   ├── _typography.scss
│       │   └── global.scss
│       ├── site
│       │   ├── footer.scss
│       │   ├── header.scss
│       │   ├── landing.scss
│       │   ├── login.scss
│       │   ├── register.scss
│       │   ├── switchlanguage.scss
│       │   └── verify.scss
│       └── widget
│           ├── search.scss
│           └── widget.scss
├── tsconfig.app.json
├── tsconfig.json
├── tsconfig.node.json
└── vite.config.ts
```