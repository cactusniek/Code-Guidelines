# AI Guidelines & Rules by Niek | JSX

---

## General

- avoid using third party libraries, unless it makes the code cleaner and smaller
- keep all code consistent and follow the same style across the entire codebase

---

## Project Structure

- keep a clear separation between:
    - `pages` - page components that represent routes
    - `components` - reusable components
    - `global` - shared application logic
    - `assets` - static files
    - `styles` - SCSS files
- if there are multiple apps in the same project, create an `/apps` folder and keep each app in a separate folder

**Example:**

```
├── eslint.config.js
├── index.html
├── package.json
├── public
│   └── favicon.svg
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

## main.tsx

- `main.tsx` is the entry point; keep it minimal, only wrap the app with providers and mount the root
- always render inside `<StrictMode>`
- wrap the app in `<HelmetProvider>` when using `react-helmet-async` for page titles and meta tags
- import the global stylesheet here so it applies to the entire app
- the router is imported from `./global/router/router` and rendered as `<AppRouter />`

**Example:**

```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'

import { HelmetProvider } from 'react-helmet-async'

import { AppRouter } from './global/router/router'

import './styles/global/global.scss'

createRoot(document.getElementById('root')!).render(
    <StrictMode>
        <HelmetProvider>
            <AppRouter />
        </HelmetProvider>
    </StrictMode>,
)
```

---

## 3rd Party Libraries

- for global state management always use **Jotai** this keeps state minimal and co-located without boilerplate
- for client-side routing always use **React Router DOM** and define all routes in `global/router/router.tsx` and export as `AppRouter`
- for page titles and meta tags always use **React Helmet Async** and wrap the app in `<HelmetProvider>` in `main.tsx` and use `<Helmet>` per page
- avoid introducing a new library if the same result can be achieved with a few lines of native code

**Examples:**

```tsx
// Jotai for reading and writing global state
const [language, setLanguage] = useAtom(languageAtom)
```

```tsx
// React Router DOM for router setup in global/router/router.tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom'

import Landing from '../../apps/site/Landing'
import Login from '../../apps/site/Login'

export function AppRouter() {
    return (
        <BrowserRouter>
            <Routes>
                <Route path="/" element={<Landing />} />
                <Route path="/login" element={<Login />} />
            </Routes>
        </BrowserRouter>
    )
}
```

```tsx
// React Helmet Async for setting the page title per page
import { Helmet } from 'react-helmet-async'

<Helmet>
    <title>Login — Vetly</title>
</Helmet>
```

---

## Prettier Config

**`.prettierrc.json`:**

```json
{
    "printWidth": 240,
    "tabWidth": 4,
    "useTabs": false,
    "singleQuote": true,
    "semi": false,
    "arrowParens": "avoid"
}
```

---

## Import Style

**Order:**

1. standard libraries (Node, etc.)
2. third party imports in order from framework down to smaller libraries (React, React Router, Jotai, etc.)
3. absolute project imports (`/global/..`, config, utils, etc.)
4. relative imports (`../components/..` Header.tsx, Navigation.tsx, etc.)
5. image imports
6. style imports

**Rules:**

- imports must always follow the same hierarchy as they appear in the code, top to bottom
- always use an empty line between each group / section
- imports must be alphabetically ordered within the same group
- type only imports can be grouped with their module, prefixed by `type`
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
- always use a 4 space tab index
- use single quotes `'` in TypeScript / JavaScript, double quotes `"` in JSX attributes or if dynamic values are used ````
- keep hooks and state declarations at the top of the component, followed by helpers, then the JSX `return`
- use template literals for dynamic classnames: `` className={`input ${hasError ? 'input_error' : ''}`} ``
- limit lines to a readable length and break long JSX props over multiple lines
- avoid using inline styles, only if absolutely necessary
- always use `const` instead of `let` wherever possible
- when using try / catch always put `(err)` in the catch block

### JSX Templating Style

- always place an empty line between sibling JSX elements; every direct child of a container gets its own visual breathing room, regardless of how small or related they are; the only exception is a single-expression `.map()` where each item is one self closing tag
- use empty lines to visually separate logical sections of code
- use clear class prefixes to express role and hierarchy for example `container_*` for layout wrappers, `link_*` for navigation links, `icon_*` for icons, etc.
- use double quotes for JSX attributes
- break JSX props onto multiple lines when there are more than 3 props
- `onClick`, `onSubmit`, `onChange` always go at the beginning; `className` always goes last for best readability
- always add an `alt` attribute to `<img>` — use a descriptive value for meaningful images, empty string `alt=""` for decorative ones

**Examples:**

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

## Component Structure

- all hooks, for example: `useState`, `useEffect`, `useAtom` must always be declared at the top of the function and grouped by related state; never use hooks inside `if`/`else` or loops
- call event handlers always by name: `handleSubmit`, `handleEmailChange`, `toggleShowPassword`, or something else that makes the action clear
- group related state together, for example: `isLoading`, `searchProgress`, `searchMessage` should be declared as one block, separated by an empty line from unrelated state
- always use `e.preventDefault()` on `<form />`

**Order:**

1. imports
2. types / interfaces
3. constants
4. component function
   - use export default inline with the function declaration
   - hooks + state
   - derived values
   - helper functions
   - event handlers
   - JSX return

**Example:**

```tsx
import { useState } from 'react'

import { useAtom } from 'jotai'
import { languageAtom } from '../../global/atoms/atoms'

import { locales } from '../../global/translations'

import '../../styles/site/profile.scss'

interface ProfileProps {
    userId: string
}

export default function Profile({ userId }: ProfileProps) {
    const [language] = useAtom(languageAtom)
    const [name, setName] = useState('')

    const locale = locales[language].profile

    function handleNameChange(e: React.ChangeEvent<HTMLInputElement>) {
        setName(e.target.value)
    }

    return (
        <div className="container_profile">
            <h1 className="title">{locale.title}</h1>

            <input
                onChange={handleNameChange}
                type="text"
                placeholder={locale.name}
                className="input_name"
            />
        </div>
    )
}
```

---

## TypeScript

- prefer `interface` for object shapes and component props; use `type` for unions, intersections, or aliases of primitives
- never use `any` use `unknown` if the type is truly unknown and narrow it before use
- always annotate the return type of non-trivial functions; simple one liners can rely on inference
- always type component props with an `interface`, defined directly above the component
- use PascalCase for naming interfaces & types

**Examples:**

```ts
interface UserCardProps {
    name: string
    age: number
    isActive?: boolean
}

type Status = 'idle' | 'loading' | 'error' | 'success'
```

```ts
async function fetchUser(id: string): Promise<User> {
    const result = await userApi.Get(id)
    return result
}
```

```ts
async function parseResponse(raw: unknown): Promise<ApiResponse> {
    if (typeof raw !== 'object' || raw === null) throw new Error('Invalid response')
    return raw as ApiResponse
}
```

---

## Naming

- components must be in PascalCase: `UserCard.tsx`, `SearchBar.tsx`
- utilities must be in camelCase: `formatDate.ts`, `validateEmail.ts`
- single word variables must be in lowercase; multiword variables must be in camelCase

**Examples:**

```tsx
const result = await authApi.Forgot(email)
```

```tsx
const [errorMessage, setErrorMessage] = useState('')
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
- avoid rendering empty fragments (`<> </>`) only to wrap a single element
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

## List Rendering

- always provide a stable, unique `key` prop when rendering lists — never use the array index as key
- when the render logic per item is more than a single line, extract it into a named function above the `return`
- keep the `.map()` call itself clean don't use logic inside, just a function call, except when the logic can be kept minimal, so that "more simple" logic isn't going to be above the return

**Examples:**

```tsx
// simple inline render
{items.map(item => (
    <span key={item.id} className="tag">{item.label}</span>
))}
```

```tsx
// extracted render function for more complex items
function renderNavItem(item: NavItem) {
    return (
        <a key={item.id} href={item.href} className="link_nav">
            {item.label}
        </a>
    )
}

// in JSX
{navItems.map(item => renderNavItem(item))}
```

---

## useEffect

- always specify a dependency array; omitting it means the effect runs after every render
- include every value used inside the effect in the dependency array — if that causes an infinite loop, the logic needs to be restructured, not the deps
- clean up subscriptions, timers, and event listeners by returning a cleanup function
- never use `useEffect` to sync state that can be derived directly from existing state or props — compute it inline instead
- for data fetching, call the async function inside the effect, not by making the effect itself async

**Examples:**

```tsx
useEffect(() => {
    async function load() {
        const data = await userApi.Get(userId)
        setUser(data)
    }

    load()
}, [userId])
```

```tsx
useEffect(() => {
    const handler = () => setScrolled(window.scrollY > 0)
    window.addEventListener('scroll', handler)

    return () => window.removeEventListener('scroll', handler)
}, [])
```

---

## API Files

- all API files must be located in `/global/api/` and named `api.ts`
- API files use a static class per domain, for example: `authApi`, `settingsApi`; only create new files if the file is getting too large or if the domain is completely unrelated, in that instance also rename `api.ts` to the domain name
- each class must have one `private static async fetchData()` method that handles the actual fetch and error logging
- public methods call `fetchData` and handle their own try / catch, always returning a typed response
- always use `const` for the response variable; only use `let` if reassignment is needed within the same scope

**Example:**

```ts
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
                body: body ? JSON.stringify(body) : undefined,
            })

            return response.json()
        } catch (err) {
            console.error(`Failed to fetch: error: ${err}, endpoint: ${endpoint}, method: (${method})`)
            throw err
        }
    }

    public static async Register(companyname: string, contactperson: string, phonenumber: string, email: string, password: string): Promise<AuthResponse> {
        const body = { companyname, contactperson, phonenumber, email, password }

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

---

## Atom Files

- atom files live in `/global/atoms/atoms.ts`
- group related atoms together, separated by empty lines
- atom names must be in camelCase suffixed with `Atom` for example: `languageAtom`, `showNavAtom`
- constants or values imported from `types` can be re-exported directly from the atom file using `export { }` keeping types and atoms co-located at one import path

**Example:**

```ts
export const showNavAtom = atom(false)
export const disappearNavAtom = atom(false)

export const languageAtom = atom<LocaleLanguage>('nl')

export { supportedLanguages }
```

---

## Assets

- all asset exports must go through a barrel `index.ts` in `/assets/` so components only need one import path
- group exports by category with a comment label for readability

**Example:**

```ts
// logo
export { default as Logo } from './logo/logo.svg'

// ui
export { default as ArrowIcon } from './ui/arrow.svg'
export { default as PasswordHide } from './ui/password-hide.svg'
export { default as PasswordShow } from './ui/password-show.svg'
```

```tsx
// when used in a component
import { Logo, ArrowIcon } from '../../assets'
```

---

## Translations

- when translations are needed they must be in `/global/translations/` and split into `locales.ts` and `messages.ts`
- `messages.ts` contains feedback/error keys and their translations per language
- `locales.ts` contains the UI string translations per language, grouped by page or feature

**Examples:**

```ts
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

```ts
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
    },
} as const

export type { LocaleLanguage }
```

---

## Error Handling

- use translation keys instead of hardcoded strings for error messages, for example: `password_too_short` for generic errors such as `'Something went wrong, please try again'` in that case also use a hardcoded string, for example: `generic_error`
- always log the actual error for debugging purposes, but return a user friendly message to the frontend; never return the raw server error to the frontend

---

## Environment Variables

- in Vite, environment variables with a `VITE_` prefix are embedded into the bundle at build time and are publicly visible — **never put secrets, API keys, or passwords in `VITE_` variables**
- non-sensitive config such as a public API base URL is fine: `VITE_API_URL=https://api.example.com`
- access them via `import.meta.env.VITE_*`
- keep actual secret values server-side only, never in the frontend bundle

**Example:**

```ts
VITE_API_URL=https://api.vetly.nl
```

```ts
const baseUrl = import.meta.env.VITE_API_URL
```

---

## Comments

- comments must explain *why* something exists, not *what* it does, the code itself should be self explanatory
- only use comments to explain complex logic
- remove unnecessary comments
- always keep comments minimal; never overuse comments in code
