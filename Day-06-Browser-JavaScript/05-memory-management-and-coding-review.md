# Day 6 — Memory Management and Coding Review

## Garbage collection
JavaScript engines automatically reclaim memory that is no longer reachable from live references.

## Memory leak
A leak occurs when memory that is no longer useful remains reachable, preventing garbage collection.

Common causes:
- Intervals/timeouts that are never cleared.
- Event listeners that are never removed.
- Unneeded references retained by closures or long-lived objects.
- Unclosed subscriptions or connections.

## React cleanup
```js
useEffect(() => {
  const handleResize = () => console.log(window.innerWidth);
  window.addEventListener("resize", handleResize);

  const timer = setInterval(() => console.log("tick"), 1000);

  return () => {
    window.removeEventListener("resize", handleResize);
    clearInterval(timer);
  };
}, []);
```

Cleanup prevents unnecessary work and releases references when they are no longer needed. A timer/listener does not automatically imply a leak in every case, but retaining it beyond its intended lifetime can cause leaks or performance issues.

## Day 6 coding review: debounced fetch search
A complete solution should:
1. Keep a debounce timer in a closure or shared scope.
2. Clear the previous timer on input.
3. Abort the previous in-flight request when superseded.
4. Use `async/await` and `await fetch(...)`.
5. Check `response.ok`.
6. Parse `await response.json()`.
7. Handle `AbortError` separately from other errors.
8. Use a valid template literal and `encodeURIComponent(query)`.

## Key distinctions
- `response.ok` is on the Response object, not `data.response.ok`.
- `fetch()` returns a Promise; await it before reading response properties.
- Call the original `callback` in debounce/throttle, not the wrapper recursively.
