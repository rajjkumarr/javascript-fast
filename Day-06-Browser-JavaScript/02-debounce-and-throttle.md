# Day 6 — Debounce and Throttle

## Debouncing
Delays execution until calls stop for a specified delay. Each new call clears the previous timer.

Common use: search input, autosave after typing.

```js
function debounce(callback, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => callback.apply(this, args), delay);
  };
}
```

Without `clearTimeout`, every scheduled timer may execute, causing repeated work/API calls.

## Throttling
Limits how often a function executes during continuous events. A leading-edge throttle can execute immediately, then ignore calls until the interval passes.

```js
function throttle(callback, delay) {
  let lastCall = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      lastCall = now;
      callback.apply(this, args);
    }
  };
}
```

This implementation executes at most once per delay and does not guarantee a trailing call.

## Difference
- Debounce: wait for inactivity; useful for search.
- Throttle: enforce a maximum frequency; useful for scroll/resize tracking.

## Common mistakes
- Calling the wrapper recursively instead of calling `callback`.
- Case-sensitive variable typos (`lastCall` vs `lastcall`).
- Forgetting to pass arguments.
- Assuming every throttle implementation has trailing execution; behavior depends on implementation.
