# Day 4 — Callbacks and Promises

## Why asynchronous JavaScript?

Timers, network requests, and database queries take time. Blocking JavaScript while waiting can make an application slow or unresponsive. Asynchronous JavaScript lets an operation start while other work continues; its result is handled when it becomes available.

~~~js
console.log("Start");
setTimeout(() => console.log("Timer completed"), 1000);
console.log("End");
~~~

Output: Start, End, Timer completed.

JavaScript execution is commonly single-threaded. The browser or Node.js runtime provides APIs and coordinates asynchronous work.

## Callbacks

A callback is a function passed as an argument to another function, so the receiving function can invoke it.

~~~js
function calculate(a, b, callback) {
  const result = a + b;
  callback(result);
}

function display(value) {
  console.log(value);
}

calculate(10, 20, display); // 30
~~~

Passing display passes the function reference. Calling display() would execute it immediately.

### Asynchronous callback

~~~js
function getUser(callback) {
  setTimeout(() => {
    const user = { name: "Raj" };
    callback(user);
  }, 1000);
}

getUser((user) => console.log(user.name));
~~~

The user object becomes available inside the timer callback. Returning from the outer function cannot return a value that only exists later.

## Callback Hell

When asynchronous operations depend on previous results, nested callbacks can become difficult to read and maintain.

~~~js
getUser((user) => {
  getOrders(user.id, (orders) => {
    getPayment(orders[0].id, (payment) => {
      console.log(payment);
    });
  });
});
~~~

Problems include deep nesting, poor readability, difficult maintenance, and complicated error handling. Callbacks do not inherently create loops, and one failure does not automatically fail every later operation; errors must be handled and propagated deliberately.

## Promises

A Promise is a JavaScript object representing the eventual completion or failure of an asynchronous operation.

Three states:
1. **Pending** — initial state; operation is not settled.
2. **Fulfilled** — operation completed successfully.
3. **Rejected** — operation failed.

A Promise settles only once. After fulfillment or rejection, its state cannot change.

~~~js
const promise = new Promise((resolve, reject) => {
  const success = true;
  if (success) resolve("Operation successful");
  else reject(new Error("Operation failed"));
});

promise
  .then((value) => console.log(value))
  .catch((error) => console.error(error.message));
~~~

- resolve(value) fulfills the Promise.
- reject(reason) rejects it.
- then() handles fulfillment.
- catch() handles rejection.

## Interview summary

"Asynchronous JavaScript allows an application to continue doing other work while time-consuming operations are pending. A callback is a function passed to another function to be invoked later. Deeply nested callbacks can cause Callback Hell. Promises provide a structured way to represent an operation's eventual success or failure."
