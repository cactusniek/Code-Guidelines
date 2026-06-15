# AI guidelines & rules by Niek

### import style

always use a empty line for each group of imports

1. standard libraries (node, etc.) 
2. third party imports always in order from framework down to the smaller libraries (React, React Router, Jotai, etc.)
3. absolute project imports ('/global/..', config, utils, etc.)
4. relative imports ('../components/..' Header.tsx, Navigation.tsx, etc.) 
5. image imports 
6. style imports

imports must always be in the same hiearchy as they are ordered in the code, top to bottom.

- Always use a space inside braces, for example:
- use a hiearchy, most important imports above such as React followed by a empty line and then another library, for every section
- `import React, { useState, useEffect } from 'react'`
- type-only imports can be grouped with their module, prefixed by `type`

example: 

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

## general code style

- never use semicolons at the end of lines
- use single quotes `'` in TypeScript / JavaScript, double quotes `"` in JSX attributes
- keep hooks and state declarations at the top of the component, followed by helpers, then JSX `return`
- use template literals for dynamic classNames: ``className={`input ${hasError ? 'input_error' : ''}`}``
- limit lines to a readable length and break long JSX props over multiple lines


### HTML / JSX style

- use empty lines to visually separate logical sections of code, as shown in the example
- use clear class prefixes to express role and hierarchy, e.g. `container_*` for layout wrappers, `link_*` for navigation links, `icon_*` for icons, etc.”



- In het algemeen: 

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

- Use double quotes for JSX attributes:
- Break JSX props onto multiple lines when there are more than 3 props.
- the attributes should be as followed: (class(Name) always as the last for best readability)



```tsx
    <input
        value={email}
        onChange={handleEmailChange}
        type="email"
        spellCheck="false"
        placeholder={locale.login.emailPlaceholder}
        className={emailError ? 'input_error' : ''}
    />

    // or

    <img alt="Vetly" src={Logo} className="image_Logo" />
```

