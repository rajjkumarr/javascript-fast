# Day 6 — Browser Storage

## localStorage
- Persistent for the same origin until cleared or browser storage is removed.
- Shared by same-origin pages/tabs.
- Stores strings; synchronous API.

## sessionStorage
- Scoped to same origin and a particular tab's page session.
- Survives reloads; normally cleared when the tab session ends.
- Stores strings; synchronous API.

## Cookies
- Small key/value data sent with matching HTTP requests, subject to domain/path and cookie attributes.
- Can have expiry and security attributes.
- Commonly used for session/authentication cookies.

## Store objects as JSON
```js
localStorage.setItem("user", JSON.stringify(user));
const user = JSON.parse(localStorage.getItem("user"));
```

Handle missing or invalid JSON in production with appropriate checks/try-catch.

## Security
JavaScript can read localStorage and sessionStorage, so an XSS vulnerability can expose their contents. Avoid storing sensitive tokens there without understanding the risks.

HttpOnly cookies cannot be read by JavaScript, reducing token exposure to scripts. Cookie authentication still needs appropriate Secure, SameSite and CSRF protections.

## Use cases
- localStorage: theme or non-sensitive persistent preferences.
- sessionStorage: temporary tab-specific state.
- Cookies: server-related session data and authentication when configured securely.
