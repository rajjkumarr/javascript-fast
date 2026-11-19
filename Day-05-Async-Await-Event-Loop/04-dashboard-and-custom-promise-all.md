# Async Coding Practice: Dashboard and Custom Promise.all

## Dashboard requests in parallel

Start all independent requests before awaiting them. `Promise.all` preserves input order in its result array, regardless of completion order.

```js
const getUser = () => Promise.resolve("User Data");
const getOrders = () => Promise.resolve("Orders Data");
const getPayments = () => Promise.resolve("Payments Data");

async function getDashboardData() {
  try {
    const [user, orders, payments] = await Promise.all([
      getUser(),
      getOrders(),
      getPayments()
    ]);

    return { user, orders, payments };
  } catch (error) {
    console.error("Failed to load dashboard data:", error);
    throw error;
  }
}
```

If any input rejects, `Promise.all` rejects. It does not automatically cancel the other operations.

## Implementing a simplified Promise.all

Requirements:
- Accept Promises and ordinary values.
- Preserve input order.
- Fulfill when every input fulfills.
- Reject when an input rejects.
- An empty input array fulfills with an empty array.

```js
function myPromiseAll(promises) {
  return new Promise((resolve, reject) => {
    if (promises.length === 0) {
      resolve([]);
      return;
    }

    const results = new Array(promises.length);
    let completedCount = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then((value) => {
          results[index] = value;
          completedCount++;

          if (completedCount === promises.length) {
            resolve(results);
          }
        })
        .catch(reject);
    });
  });
}
```

Example:
```js
myPromiseAll([
  Promise.resolve(10),
  20,
  Promise.resolve(30)
]).then(console.log);

// [10, 20, 30]
```

### Why the index matters

Promises may settle in a different order from the input. Assigning each result to `results[index]` preserves the original input order.

### Why Promise.resolve matters

`Promise.resolve(value)` adopts an input Promise's eventual state, or fulfills with a normal value. This lets the implementation handle both Promises and non-Promise values uniformly.

### Complexity

For n inputs, the bookkeeping is O(n) time and O(n) result space, excluding the work performed by the input operations.

### Interview caveat

This is a learning implementation of the core behavior. The native `Promise.all` also follows the Promise resolution procedure for thenables and iterable inputs; this exercise intentionally accepts an array and demonstrates the central ordering, fulfillment, rejection, and empty-array behavior.
