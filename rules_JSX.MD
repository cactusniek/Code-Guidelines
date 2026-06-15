# AI guidelines & rules by Niek | JSX

## import style

1. standard libraries (node, etc.) 
2. third party imports always in order from framework down to the smaller libraries (React, React Router, Jotai, etc.)
3. absolute project imports ('/global/..', config, utils, etc.)
4. relative imports ('../components/..' Header.tsx, Navigation.tsx, etc.) 
5. image imports 
6. style imports

- imports must always be in the same hiearchy as they are ordered in the code, top to bottom.
- always use a empty line for each group / section of imports
- type-only imports can be grouped with their module, prefixed by `type`
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

### conditional examples:

```tsx 
    const activeUsers = users.filter(user => user.active)

    <div>{activeUsers.length}</div>
```

```tsx 
    const status = loading ? 'loading' : error ? 'error' : 'done'
```


## function structure 

- all hooks, for example: `useState`, `useEffect`, `useAtom` must always be above in the function and must be grouped per related states. never use inside if else loops
- call event handlers always by name, like `handleSubmit`, `handleEmailChange`, `toggleShowPassword` or something that makes the action clear
- always use e.preventDefault at `<form />`

### function example: 

```tsx 
    const [email, setEmail] = useState('')
    const [emailError, setEmailError] = useState(false)
```


