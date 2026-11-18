# Day 4 — Promise Combinators

Promise combinators coordinate multiple Promises. They do not necessarily start operations themselves; operations usually begin when Promises are created or functions are called.

## Promise.all()

- Fulfills when every input fulfills.
- Rejects as soon as an input rejection is observed.
- Fulfillment values are returned in the same order as the input iterable, not completion order.
- It does not cancel the other operations when one rejects.

~~~js
const profile = Promise.resolve("Profile");
const orders = Promise.resolve("Orders");
const payments = Promise.resolve("Payments");

Promise.all([profile, orders, payments])
  .then((results) => console.log(results))
  .catch((error) => console.error(error));
~~~

Output: ["Profile", "Orders", "Payments"].

Useful when every result is required, such as loading a dashboard's profile, orders, and payment summary.

## Promise.allSettled()

- Waits until every input Promise settles (fulfills or rejects).
- Fulfills with an array describing every outcome.
- Each item has status; fulfilled items have value, rejected items have reason.

~~~js
Promise.allSettled([
  Promise.resolve("Profile"),
  Promise.reject("Orders API failed"),
  Promise.resolve("Payments")
]).then((results) => console.log(results));
~~~

Result shape:
~~~js
[
  { status: "fulfilled", value: "Profile" },
  { status: "rejected", reason: "Orders API failed" },
  { status: "fulfilled", value: "Payments" }
]
~~~

The combined Promise fulfills with these outcome records, so an input rejection does not send it to catch().

## Promise.race()

- Settles as soon as the first input Promise settles.
- The first settlement may be fulfillment or rejection.

~~~js
const first = new Promise((resolve) =>
  setTimeout(() => resolve("Server 1"), 2000)
);
const second = new Promise((resolve) =>
  setTimeout(() => resolve("Server 2"), 1000)
);

Promise.race([first, second]).then(console.log); // Server 2
~~~

## Promise.any()

- Fulfills with the first input Promise that fulfills.
- Ignores rejections while waiting for a fulfillment.
- If every input rejects, it rejects with an AggregateError.

~~~js
const first = Promise.reject("Server 1 failed");
const second = new Promise((resolve) =>
  setTimeout(() => resolve("Server 2"), 2000)
);
const third = new Promise((resolve) =>
  setTimeout(() => resolve("Server 3"), 1000)
);

Promise.any([first, second, third]).then(console.log); // Server 3
~~~

## Quick comparison

| Method | Combined Promise fulfills when | Combined Promise rejects when |
|---|---|---|
| Promise.all() | All inputs fulfill | Any input rejects |
| Promise.allSettled() | All inputs settle | Normally not due to input rejections |
| Promise.race() | First settlement is fulfillment | First settlement is rejection |
| Promise.any() | First input fulfills | All inputs reject |

## Interview summary

"Promise.all() is useful when every result is required. Promise.allSettled() gives the outcome of every operation, including failures. Promise.race() follows the first settled Promise, while Promise.any() follows the first fulfilled Promise."
