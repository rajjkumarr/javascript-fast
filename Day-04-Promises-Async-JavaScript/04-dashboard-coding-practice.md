# Day 4 — Dashboard Coding Practice and Review

## Exercise: Load dashboard data

Requirements:
1. Fetch user details.
2. Fetch orders.
3. Fetch payments.
4. Wait for all three operations using Promise.all().
5. Return all three results together.
6. Handle and propagate errors.

### Sample operations

~~~js
const getUser = () => Promise.resolve("User Data");
const getOrders = () => Promise.resolve("Orders Data");
const getPayments = () => Promise.resolve("Payments Data");
~~~

### Initial solution

~~~js
function getDashboardData() {
  Promise.all([getUser(), getOrders(), getPayments()])
    .then((results) => {
      console.log(results);
    })
    .catch((error) => {
      console.log(error);
    });
}

getDashboardData();
~~~

This correctly calls all three functions and logs the combined results. However, it does not return the Promise, so callers cannot consume the result from getDashboardData().

### Improved solution

~~~js
function getDashboardData() {
  return Promise.all([
    getUser(),
    getOrders(),
    getPayments()
  ])
    .then((results) => results)
    .catch((error) => {
      console.error("Dashboard error:", error);
      throw error;
    });
}

getDashboardData()
  .then((data) => console.log(data))
  .catch((error) => console.error(error));
~~~

Expected successful result:
~~~js
["User Data", "Orders Data", "Payments Data"]
~~~

Why return the Promise?
- The caller can use then() or await to access the data.
- Errors can propagate to the caller for handling.
- The function becomes reusable instead of only logging internally.

Note: then((results) => results) is redundant; returning Promise.all([...]) alone is sufficient. It is shown above to make value flow explicit.

### Cleaner version

~~~js
function getDashboardData() {
  return Promise.all([
    getUser(),
    getOrders(),
    getPayments()
  ]).catch((error) => {
    console.error("Dashboard error:", error);
    throw error;
  });
}
~~~

## Key takeaways

- Return asynchronous work when callers need its result.
- Use Promise.all() when every result is required.
- Catch errors at a meaningful boundary and propagate them when the caller also needs to react.
- Logging an error does not automatically make a failed operation successful.

## Day 4 topics covered

- Synchronous vs asynchronous JavaScript
- Callbacks and Callback Hell
- Promise states: pending, fulfilled, rejected
- resolve() and reject()
- Promise chaining and return values
- catch() and finally()
- Promise.all(), Promise.allSettled(), Promise.race(), Promise.any()
- Dashboard data aggregation with Promise.all()

## Revision tasks

1. Explain why asynchronous JavaScript is useful in a MERN application.
2. Explain Callback Hell and why Promises improve readability.
3. Explain the difference between Promise.all() and Promise.allSettled().
4. Explain the difference between Promise.race() and Promise.any().
5. Modify the dashboard example so one API failure is handled without losing successful results (consider Promise.allSettled()).

## Topics to strengthen next

- Implement a simplified Promise.all() manually.
- Practice Promise behavior with non-Promise values and an empty array.
- Continue with async/await, the event loop, microtasks, and macrotasks in Day 5.
