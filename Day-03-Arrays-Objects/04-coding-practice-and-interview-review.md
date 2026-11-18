# Day 3.4 — Coding Practice and Interview Review

## Exercise 1: Remove duplicates with filter and indexOf

```js
const arr = [1, 2, 3, 2, 4, 1, 5];

const unique = arr.filter((value, index) => {
  return arr.indexOf(value) === index;
});

console.log(unique); // [1, 2, 3, 4, 5]
```

The first occurrence has an index equal to `indexOf(value)`. Later duplicates have a different current index and are filtered out.

- Time complexity: O(n²), because `indexOf()` can scan the array for each element.
- Result space: O(n) in the worst case.

## Exercise 2: Count frequency with a loop

```js
function getFrequency(numbers) {
  const frequency = {};

  for (let i = 0; i < numbers.length; i++) {
    frequency[numbers[i]] = (frequency[numbers[i]] || 0) + 1;
  }

  return frequency;
}

console.log(getFrequency([1, 2, 2, 3, 1, 2, 4, 3]));
// { 1: 2, 2: 3, 3: 2, 4: 1 }
```

A conditional is not necessary: `frequency[value] || 0` supplies zero when no count exists yet. Returning the object makes the function reusable.

## Exercise 3: Count frequency with reduce

```js
const numbers = [1, 2, 2, 3, 1, 2, 4, 3];

const frequency = numbers.reduce((acc, num) => {
  acc[num] = (acc[num] || 0) + 1;
  return acc;
}, {});

console.log(frequency);
// { 1: 2, 2: 3, 3: 2, 4: 1 }
```

- Time complexity: O(n) expected for ordinary object-key access.
- Space complexity: O(k), where k is the number of distinct values.

## Exercise 4: Filter and map API data

Task: return names of active users whose age is at least 18.

```js
const users = [
  { id: 1, name: "Raj", age: 25, active: true },
  { id: 2, name: "John", age: 17, active: false },
  { id: 3, name: "Sam", age: 30, active: true },
  { id: 4, name: "David", age: 16, active: true }
];

const names = users
  .filter(user => user.active && user.age >= 18)
  .map(user => user.name);

console.log(names); // ["Raj", "Sam"]
```

First select matching user objects with `filter()`, then extract their names with `map()`.

## Interview explanation practice

Use this structure when explaining a solution:
1. State the goal.
2. Explain why you chose the method.
3. Explain how it works.
4. Mention time and space complexity when relevant.

Example:
"I first use filter to select active users whose age is at least 18. Then I use map to extract their names and return a new array."

## Common mistakes to avoid

- Expecting `forEach()` to return an array.
- Thinking `find()` returns all matches.
- Confusing `some()` (at least one) with `every()` (all).
- Assuming spread or Object.assign creates a deep copy.
- Forgetting to return the accumulator from a reduce callback.
- Forgetting that destructuring defaults apply to `undefined`, not `null`.
- Mutating nested React state after copying only the outer object.
- Logging a function result instead of returning it when the function is meant to be reusable.

## Day 3 checklist

- [x] map, filter, reduce
- [x] find, some, every
- [x] forEach vs map
- [x] Spread and rest
- [x] Object and array destructuring
- [x] Shallow copy and deep copy
- [x] Object.assign and structuredClone
- [x] Mutability and immutability
- [x] Remove duplicates
- [x] Frequency counter
- [x] Filter and map API data

## Day 3 takeaway

Choose array methods based on the result you need:
- Transform: `map()`
- Select: `filter()`
- Accumulate: `reduce()`
- First match: `find()`
- At least one: `some()`
- All: `every()`
- Side effects: `forEach()`

Understand references before copying nested data, and use immutable updates for React state.
