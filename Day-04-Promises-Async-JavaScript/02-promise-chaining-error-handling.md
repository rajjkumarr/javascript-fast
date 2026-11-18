# Day 4 — Promise Chaining and Error Handling

## Promise chaining

Each call to then() returns a new Promise. The value returned by one handler becomes the fulfillment value received by the next then().

~~~js
Promise.resolve(10)
  .then((result) => {
    console.log(result); // 10
    return result * 2;
  })
  .then((result) => {
    console.log(result); // 20
    return result * 3;
  })
  .then((result) => console.log(result)); // 60
~~~

If a handler has no explicit return, it returns undefined. The next handler therefore receives undefined.

~~~js
Promise.resolve(10)
  .then((result) => {
    console.log(result); // 10
    result * 2; // evaluated, but not returned
  })
  .then((result) => console.log(result)); // undefined
~~~

## Error handling with catch()

A rejected Promise skips fulfillment handlers until a rejection handler is found. An error thrown inside a then() handler rejects the Promise returned by that then().

~~~js
Promise.resolve(10)
  .then(() => {
    throw new Error("Something went wrong");
  })
  .then((result) => console.log("Success:", result)) // skipped
  .catch((error) => console.log(error.message));
~~~

Output: Something went wrong.

A catch(handler) is shorthand for then(undefined, handler). A catch handler can recover by returning a value, or keep the chain rejected by throwing or returning a rejected Promise.

## finally()

finally() runs when the Promise settles, whether fulfilled or rejected. It is useful for cleanup, such as hiding a loading indicator.

~~~js
Promise.resolve("Success")
  .then((result) => console.log(result))
  .catch((error) => console.error(error))
  .finally(() => console.log("Request completed"));
~~~

Output:
~~~text
Success
Request completed
~~~

- finally() does not receive the fulfillment value or rejection reason as an argument.
- Normally it preserves the original outcome unless its handler throws or returns a rejected Promise.

## Practical React example

~~~js
setLoading(true);

fetch("/api/users")
  .then((response) => {
    if (!response.ok) throw new Error("Failed to fetch users");
    return response.json();
  })
  .then((users) => setUsers(users))
  .catch((error) => setError(error.message))
  .finally(() => setLoading(false));
~~~

## Interview summary

"Promise chaining lets us sequence asynchronous work. Each then() returns a new Promise, and its returned value is passed to the next handler. Errors thrown in a handler reject the chain and propagate to the next catch(). finally() is useful for cleanup because it runs after either success or failure."
