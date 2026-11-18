# Day 3.1 — Array Methods: map, filter, reduce, find, some, every

## 1. map()

- Transforms every element and returns a **new array**.
- Does not mutate the original array.
- The returned array has the same length as the input array.

```js
const numbers = [10, 20, 30];
const result = numbers.map((num, index) => {
  console.log(index);
  return num + 5;
});
// Logs: 0, 1, 2
// result: [15, 25, 35]
```

Use it when the goal is to transform data, such as extracting names from API results.

## 2. filter()

- Returns a new array containing elements for which the callback returns a truthy value.
- Does not mutate the original array.
- The result can be shorter than the original array, including an empty array.

```js
const numbers = [1, 2, 3, 4, 5, 6];
const result = numbers.filter((num, index) => num % 2 === 0 && index > 2);
// [4, 6]
```

## 3. reduce()

Processes array elements and accumulates them into a single result. The result can be a number, object, array, or another value.

```js
const numbers = [5, 10, 15];
const result = numbers.reduce((acc, num) => acc * num, 2);
// Accumulator: 2 -> 10 -> 100 -> 1500
// result: 1500
```

The initial value is important. Here it is `2`.

### Frequency counter with reduce

```js
const numbers = [1, 2, 2, 3, 1, 2, 4, 3];

const frequency = numbers.reduce((acc, num) => {
  acc[num] = (acc[num] || 0) + 1;
  return acc;
}, {});

// { 1: 2, 2: 3, 3: 2, 4: 1 }
```

- `acc`: accumulator object.
- `num`: current number.
- `acc[num] || 0`: existing count or zero.
- Return the accumulator so the next iteration receives it.

## 4. find()

Returns the **first element** that satisfies the condition. If no element matches, it returns `undefined`.

```js
const users = [
  { id: 1, active: false },
  { id: 2, active: true },
  { id: 3, active: true }
];

const result = users.find(user => user.active);
// { id: 2, active: true }
```

It stops searching after the first match.

## 5. some()

Returns a boolean. It returns `true` if **at least one** element satisfies the condition; otherwise, `false`.

```js
const numbers = [1, 3, 5, 8, 9];
numbers.some(num => num % 2 === 0); // true
```

Stops when it finds a match.

## 6. every()

Returns a boolean. It returns `true` only if **all** elements satisfy the condition. It returns `false` as soon as one fails.

```js
[2, 4, 6, 8].every(num => num % 2 === 0); // true
[2, 4, 5, 8].every(num => num % 2 === 0); // false
```

## Quick comparison

| Method | Return value | Main purpose |
|---|---|---|
| `map()` | New array | Transform every element |
| `filter()` | New array | Keep matching elements |
| `reduce()` | Accumulated value | Build one result |
| `find()` | Element or `undefined` | First matching element |
| `some()` | Boolean | At least one matches |
| `every()` | Boolean | All must match |

## Interview reminders

- `find()` returns an element, not an array of all matches.
- `filter()` returns all matching elements in an array.
- `some()` and `every()` return booleans, not matching elements.
- `map()` preserves the number of elements.
- `reduce()` can produce many kinds of results depending on the accumulator.

## Interview Q&A

**Q: What is the difference between map and filter?**  
A: `map()` transforms every element and returns an array of the same length. `filter()` selects matching elements and may return a shorter array.

**Q: What happens when find has no match?**  
A: It returns `undefined`.

**Q: What is the difference between some and every?**  
A: `some()` checks whether at least one element matches; `every()` checks whether all elements match.

**Q: Why provide an initial value to reduce?**  
A: It defines the accumulator's starting value and helps make the result predictable, including for empty arrays.
