# AI guidelines & rules by Niek | JSX

## general

- avoid using third libraries
- 

## import style

1. standard libraries (node, etc.) 
2. third party imports always in order from framework down to the smaller libraries (React, React Router, Jotai, etc.)
3. absolute project imports ('/global/..', config, utils, etc.)
4. relative imports ('../components/..' Header.tsx, Navigation.tsx, etc.) 
5. image imports 
6. style imports

- imports must always be in the same hiearchy as they are ordered in the code, top to bottom.
- always use a empty line for each group / section of imports, imports must be alphabetically ordered within the same group
- type-only imports can be grouped with their module, prefixed by `type`
- remove unused imports
- always use a space inside braces

### import example:

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

## general coding style

- never use semicolons at the end of lines
- always use a 4 space tab index
- use single quotes `'` in TypeScript / JavaScript, double quotes `"` in JSX attributes
- keep hooks and state declarations at the top of the component, followed by helpers, then JSX `return`
- use template literals for dynamic classNames: ``className={`input ${hasError ? 'input_error' : ''}`}``
- limit lines to a readable length and break long JSX props over multiple lines
- don't use inline styles

### JSX templating style

- use empty lines to visually separate logical sections of code, as shown in the example
- use clear class prefixes to express role and hierarchy, e.g. `container_*` for layout wrappers, `link_*` for navigation links, `icon_*` for icons, etc.”
- use double quotes for JSX attributes:
- break JSX props onto multiple lines when there are more than 3 props.
- the attributes should be like in the example, `onClick`, `onSubmit`, `onChange` always the at the beginning and className always as last for best readability

### code examples:

```tsx
    <div className="container_login">
        <h1 className="title">{locale.login.title}</h1>

        <form noValidate onSubmit={login} className="form_login">
            <input onChange={e => setEmail(e.target.value)} type="email" spellCheck="false" placeholder={locale.login.email} className={`input_email ${emailError ? 'input_error' : ''}`} />

            <div className="container_input">
                <input onChange={e => setPassword(e.target.value)} type={showPassword ? 'text' : 'password'} placeholder={locale.login.password} className={`input_password ${passwordError ? 'input_error' : ''}`} />

                <img src={showPassword ? PasswordShow : PasswordHide} className="icon_TogglePassword" onClick={toggleShowPassword} />
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

### conditional rendering

- do not use complex logic directly inside JSX
- move calculations, filters and conditions into variables above the `return`
- use ternary operators only for simple two-state rendering
- use variables for multiple conditions or nested logic
- avoid nested ternary operators inside JSX
- prefer early returns for completely different layouts or states
- use `&&` only when rendering a component or element when a condition is true
- never use `&&` with values that can be `0`, `null` or `undefined` unless the behavior is intended
- avoid rendering empty fragments (`<> </>`) only to wrap a single element
- keep conditional sections separated with empty lines for readability

### conditional examples:

```tsx 
    const activeUsers = users.filter(user => user.active)

    <div>{activeUsers.length}</div>
```

```tsx 
    const status = loading ? 'loading' : error ? 'error' : 'done'
```

## component structure 

- all hooks, for example: `useState`, `useEffect`, `useAtom` must always be above in the function and must be grouped per related states. never use inside if else loops
- call event handlers always by name, like `handleSubmit`, `handleEmailChange`, `toggleShowPassword` or something that makes the action clear
- always use e.preventDefault at `<form />`
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
5. export default

### component example:

```tsx 
    const [email, setEmail] = useState('')
    const [emailError, setEmailError] = useState(false)
```

## naming

- components must be in PascalCase, for example: `UserCard.tsx` `SearchBar.tsx`
- utilities must be in camelCase, for example: `formatDate.ts` `validateEmail.ts`
- single word variables must be in lowercase, multiword variables must be in camelCase

### naming example: 

```tsx
    const result = await authApi.Forgot(email)
```

```tsx
    const [errorMessage, setErrorMessage] = useState('')
```

## comments 

- comments must explain why something exists, not particularly what it does, the code should be self-explanatory
- remove unnecessary comments
- always keep comments minimal, don't overpolute the code with comments

## folder structure

- keep a clear separation between:
    - `pages` for page components that represent routes
    - `components` for reusable components
    - `global` for shared application logic
    - `assets` for static files
    - `styles` for SCSS files
    - if there are multiple apps in the same project, create `/apps` folder and keep each app in a separate folder

### folder structure example:

```md
    ├── eslint.config.js
    ├── index.html
    ├── package.json
    ├── src
    │   ├── apps
    │   │   ├── site
    │   │   │   ├── Forgot.tsx
    │   │   │   ├── Landing.tsx
    │   │   │   ├── Login.tsx
    │   │   │   ├── Register.tsx
    │   │   │   ├── Reset.tsx
    │   │   │   └── Verify.tsx
    │   │   └── widget
    │   │       └── Widget.tsx
    │   ├── assets
    │   │   ├── backgrounds
    │   │   │   ├── background-cats.svg
    │   │   │   ├── background-large.svg
    │   │   │   └── background-small.svg
    │   │   ├── fonts
    │   │   │   ├── Gelica-Trial-Regular.otf
    │   │   │   └── Nexa-Trial-Regular.ttf
    │   │   ├── images
    │   │   │   ├── cat.png
    │   │   │   ├── searchbar.svg
    │   │   │   ├── stars.svg
    │   │   │   ├── sustainable.svg
    │   │   │   └── video-placeholder.png
    │   │   ├── index.ts // for exporting assets as modules
    │   │   ├── logo
    │   │   │   └── logo.svg
    │   │   └── ui
    │   │       ├── arrow-toggle.svg
    │   │       ├── arrow.svg
    │   │       ├── password-hide.svg
    │   │       ├── password-show.svg
    │   │       ├── search.svg
    │   │       └── whatsapp.svg
    │   ├── components
    │   │   ├── Footer.tsx
    │   │   ├── Header.tsx
    │   │   ├── Search.tsx
    │   │   └── SwitchLanguage.tsx
    │   ├── global
    │   │   ├── api
    │   │   │   └── api.ts
    │   │   ├── atoms
    │   │   │   └── atoms.ts
    │   │   ├── mappings
    │   │   │   └── doeldieren.ts
    │   │   ├── router
    │   │   │   └── router.tsx
    │   │   ├── translations
    │   │   │   ├── index.ts
    │   │   │   ├── locales.ts
    │   │   │   └── messages.ts
    │   │   └── types
    │   │       └── types.ts
    │   ├── main.tsx
    │   └── styles
    │       ├── global
    │       │   ├── _animations.scss
    │       │   ├── _colors.scss
    │       │   ├── _layout.scss
    │       │   ├── _spacing.scss
    │       │   ├── _typography.scss
    │       │   └── global.scss
    │       ├── site
    │       │   ├── footer.scss
    │       │   ├── header.scss
    │       │   ├── landing.scss
    │       │   ├── login.scss
    │       │   ├── register.scss
    │       │   ├── switchlanguage.scss
    │       │   └── verify.scss
    │       └── widget
    │           ├── search.scss
    │           └── widget.scss
    ├── tsconfig.app.json
    ├── tsconfig.json
    ├── tsconfig.node.json
    └── vite.config.ts
```





