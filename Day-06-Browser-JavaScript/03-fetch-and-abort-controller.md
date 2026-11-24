# Day 6 — Fetch API and AbortController

## Fetch basics
`fetch()` returns a Promise for a Response. `response.json()` also returns a Promise.

```js
async function getUsers() {
  const response = await fetch("/api/users");
  if (!response.ok) {
    throw new Error(`HTTP Error: ${response.status}`);
  }
  return response.json();
}
```

## HTTP errors vs network errors
Fetch does **not** reject automatically for HTTP statuses such as 404 or 500. Check `response.ok` (or `status`) and throw if appropriate. Network failures generally reject the fetch Promise.

## AbortController
Use it to cancel a fetch that is no longer needed.

```js
const controller = new AbortController();
fetch("/api/users", { signal: controller.signal });
controller.abort();
```

An aborted fetch rejects with an `AbortError`.

## Debounced search pattern
- Clear the previous debounce timer when input changes.
- Abort an in-flight prior request when a newer query supersedes it.
- After the delay, fetch using `encodeURIComponent(query)`.
- Check `response.ok`, parse JSON, handle AbortError separately.

Abort cancels the browser-side request; it does not guarantee the server stops work if the request already reached it.

## Interview points
- Fetch and JSON parsing are asynchronous.
- HTTP error responses are still valid Response objects.
- AbortController can also coordinate cancellation for other APIs that support AbortSignal.
