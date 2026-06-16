# AI Guidelines & Rules — Niek | JSX

---

## General

- avoid using third-party libraries
- keep all code consistent and follow the same style across the entire codebase
---

## Import Style

**Order:**

1. standard libraries (Node, etc.)
2. third-party imports — in order from framework down to smaller libraries (React, React Router, Jotai, etc.)
3. absolute project imports (`/global/..`, config, utils, etc.)
4. relative imports (`../components/..` — Header.tsx, Navigation.tsx, etc.)
5. image imports
6. style imports

**Rules:**

- imports must always follow the same hierarchy as they appear in the code, top to bottom
- always use an empty line between each group / section
- imports must be alphabetically ordered within the same group
- type-only imports can be grouped with their module, prefixed by `type`
- remove unused imports
- always use a space inside braces

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

- never use semicolons at the end of lines
- always use a 4-space tab index
- use single quotes `'` in TypeScript / JavaScript, double quotes `"` in JSX attributes
- keep hooks and state declarations at the top of the component, followed by helpers, then the JSX `return`
- use template literals for dynamic classnames: `` className={`input ${hasError ? 'input_error' : ''}`} ``
- limit lines to a readable length and break long JSX props over multiple lines
- avoid using inline styles, only if absolutely necessary
- when using try / catch always put (err) in the catch block

### JSX Templating Style

- use empty lines to visually separate logical sections of code
- use clear class prefixes to express role and hierarchy — e.g. `container_*` for layout wrappers, `link_*` for navigation links, `icon_*` for icons, etc.
- use double quotes for JSX attributes
- break JSX props onto multiple lines when there are more than 3 props
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

<img alt="Vetly" src={Logo} className="image_Logo" />
```

---

## Conditional Rendering

- do not use complex logic directly inside JSX
- move calculations, filters, and conditions into variables above the `return`
- use ternary operators only for simple two-state rendering
- use variables for multiple conditions or nested logic
- avoid nested ternary operators inside JSX
- prefer early returns for completely different layouts or states
- use `&&` only when rendering a component or element based on a condition being true
- never use `&&` with values that can be `0`, `null`, or `undefined` unless the behavior is intended
- avoid rendering empty fragments (`<></>`) only to wrap a single element
- keep conditional sections separated with empty lines for readability

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

- all hooks — e.g. `useState`, `useEffect`, `useAtom` — must always be declared at the top of the function and grouped by related state; never use hooks inside `if`/`else` or loops
- call event handlers always by name: `handleSubmit`, `handleEmailChange`, `toggleShowPassword`, or something else that makes the action clear
- group related state together, for example: `isLoading`, `searchProgress`, `searchMessage` should be declared as one block, seperated by an empty line from unrelated state
- always use `e.preventDefault()` on `<form />`
- every component file must follow this order:

1. imports
2. types / interfaces
3. constants
4. component function
   - hooks + state
   - derived values
   - helper functions
   - event handlers
   - JSX return
5. `export default`, always at the bottom

**Example:**

```tsx
const [email, setEmail] = useState('')
const [emailError, setEmailError] = useState(false)
```

---

## Naming

- components must be in PascalCase — e.g. `UserCard.tsx`, `SearchBar.tsx`
- utilities must be in camelCase — e.g. `formatDate.ts`, `validateEmail.ts`
- single-word variables must be in lowercase; multi-word variables must be in camelCase

**Examples:**

```tsx
const result = await authApi.Forgot(email)
```

```tsx
const [errorMessage, setErrorMessage] = useState('')
```

---

## Comments

- comments must explain *why* something exists, not *what* it does — the code should be self-explanatory
    - only use comments to explain (more / very) complex logic
- remove unnecessary comments
- always keep comments minimal; don't overpollute the code with comments

---

## Folder Structure

- keep a clear separation between:
    - `pages` — page components that represent routes
    - `components` — reusable components
    - `global` — shared application logic
    - `assets` — static files
    - `styles` — SCSS files
- if there are multiple apps in the same project, create an `/apps` folder and keep each app in a separate folder

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

---

## API Files

- all api files must be located in `/global/api/` and named `api.ts`
- API files use a static class per domain for example: authApi, settingsApi, only make new files if the file is getting too large or if the domain is completely unrelated to the other domains, in that case also rename api.ts to the domain name
- each class must have one private static async fetchData() method that handles the actual fetch and error logging
- public methods call fetchData and handle their own try / catch, always returning a typed response
- always use const for the response variable, only use let if reassignment is needed within the same scope

**Example:**

```tsx
export class authApi {
    private static serverUrl: string = 'http://localhost:4000/auth'

    private static async fetchData(endpoint: string, method: string, body?: any): Promise<any> {
        try {
            const response = await fetch(this.serverUrl + endpoint, {
                method: method,
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                },
                // credentials: 'include',
                body: body ? JSON.stringify(body) : undefined,
            })

            return response.json()
        } catch (err) {
            console.error(`Failed to fetch: error: ${err}, endpoint: ${endpoint}, method: (${method})`)
            throw err
        }
    }

    public static async Register(companyname: string, contactperson: string, phonenumber: string, email: string, password: string): Promise<AuthResponse> {
        const body: any = { companyname: companyname, contactperson: contactperson, phonenumber: phonenumber, email: email, password: password }

        try {
            const response = await this.fetchData('/register', 'POST', body)

            return response
        } catch (err) {
            console.error('API fetch error: ', err)
            return { success: false, message: 'Something went wrong, please try again' }
        }
    }
}
```

## Error Handling

- use a variable like this `password_too_short: 'Password is too short'` that later gets translated instead of hardcoding the error message in the API response, only use hardcoded messages for generic errors such as `Something went wrong, please try again`
- always log the actual error for dedugging purposes, but return a user friendly message to the frontend, never return the actual error message from the server to the frontend

## Translations

- when translations are neededd they must be in `/global/translations/` and split into locales.ts and messages.ts
- locales.ts contains the actual translations, messages.ts contains the keys that are used to access the translations

**Examples:**

```tsx 
export const messages = {
    en: {
        password_too_short: 'Password is too short',
        register_success: 'Registration successful',
        register_error: 'Registration failed',
        user_exists: 'User already exists',
    },

    nl: {
        password_too_short: 'Wachtwoord is te kort',
        register_success: 'Registratie succesvol',
        register_error: 'Registratie mislukt',
        user_exists: 'Gebruiker bestaat al',
    },
} as const

export type MessageLanguage = keyof typeof messages
export type MessageKey = keyof typeof messages.en
```

```tsx
export const locales = {
    en: {
        register: {
            title: 'Sign up',
            verify: 'Verify',
            companyname: 'companyname',
            contactperson: 'contactperson',
            phonenumber: 'phonenumber',
            email: 'e-mail',
            password: 'password',
        },

        login: {
            title: 'Login',
            email: 'e-mail',
            password: 'password',
            submit: 'Login',
            forgot_password: 'forgot password?',
            register_link: 'register instead',
        },
    },

    nl: {
        register: {
            title: 'Registreer',
            verify: 'Verifieer',
            companyname: 'bedrijfsnaam',
            contactperson: 'contactpersoon',
            phonenumber: 'telefoonnummer',
            email: 'e-mailadres',
            password: 'wachtwoord',
        },

        login: {
            title: 'Login',
            email: 'e-mailadres',
            password: 'wachtwoord',
            submit: 'Login',
            forgot_password: 'wachtwoord vergeten?',
            register_link: 'of registreer',
        },
    }
} as const

export type { LocaleLanguage }
```


