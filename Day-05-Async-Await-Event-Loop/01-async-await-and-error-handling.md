# Async/Await and Error Handling

## Async/Await

`async/await` is syntax built on Promises that makes asynchronous code easier to read.

- An `async` function always returns a Promise.
- Returning a normal value from an async function fulfills its returned Promise with that value.
- `await` pauses the current async function until the awaited value settles.
- It does not block the JavaScript main thread.

```js
async function getUserData() {
  const user = await getUser();
  return user;
}
```

## Error handling

Use `try/catch/finally` to handle rejected Promises and synchronous errors inside the try block.

```js
async function getUserData() {
  try {
    const user = await fetchUser();
    return user;
  } catch (error) {
    console.error("Failed to fetch user:", error);
    throw error;
  } finally {
    console.log("Request completed");
  }
}
```

- `try`: executes the operation.
- `catch`: handles the error.
- `finally`: runs after success or failure.
- Rethrow with `throw error` when the caller must also know the operation failed.

### Important distinction

If an error is caught and not rethrown, the async function normally fulfills with `undefined` if the catch block has no return value. Without a catch, the async function's returned Promise rejects.

A rejected Promise does not automatically crash the application. An unhandled rejection should still be avoided.

## Interview answer

“Async/await is built on Promises and provides a cleaner way to write asynchronous code. The await keyword pauses the current async function until the Promise settles, without blocking the main thread. I use try/catch for error handling and rethrow errors when the caller needs to handle them.”
