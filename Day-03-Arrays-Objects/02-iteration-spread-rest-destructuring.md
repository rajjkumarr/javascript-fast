# Day 3.2 — Iteration, Spread, Rest, and Destructuring

## 1. forEach() vs map()

### map()

Use `map()` when transforming elements and needing the transformed values in a new array.

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(num => num * 2);
// [2, 4, 6]
```

### forEach()

Use `forEach()` to perform an action for each element, such as logging or triggering a side effect.

```js
const numbers = [1, 2, 3];

const result = numbers.forEach(num => {
  console.log(num * 2);
});

console.log(result); // undefined
```

Even if the callback has a `return`, `forEach()` ignores the callback's returned value. The callback returns from itself; it does not become the return value of `forEach()`.

## 2. Spread operator (...)

Spread expands iterable elements or enumerable object properties.

### Array spread

```js
const first = [1, 2, 3];
const combined = [...first, 4, 5];
// [1, 2, 3, 4, 5]
```

### Object spread

```js
const user = { name: "Raj", role: "Developer" };
const updatedUser = { ...user, experience: 5 };
// { name: "Raj", role: "Developer", experience: 5 }
```

Spread makes a shallow copy when used this way; nested references are not independently cloned.

## 3. Rest operator (...)

Rest collects remaining values into an array. It is used in function parameters and destructuring.

```js
function displayUser(name, ...details) {
  console.log(name);
  console.log(details);
}

displayUser("Raj", "Developer", "MERN");
// Raj
// ["Developer", "MERN"]
```

The same `...` syntax is spread or rest depending on its context:
- Spread expands values.
- Rest collects remaining values.

## 4. Object destructuring

Extract properties into variables.

```js
const user = { name: "Raj", age: 25, role: "Developer" };
const { name, age } = user;
// name === "Raj", age === 25
```

### Default values

```js
const user = { name: "Raj" };
const { name, salary = 30000 } = user;
// salary === 30000
```

A destructuring default is used when the property value is `undefined`. It is not used for `null`.

```js
const { salary = 30000 } = { salary: null };
console.log(salary); // null
```

## 5. Array destructuring

Array destructuring assigns values by position.

```js
const colors = ["red", "green", "blue"];
const [first, second] = colors;
// first === "red", second === "green"
```

Skip an element with an empty position:

```js
const numbers = [10, 20, 30, 40];
const [first, , third] = numbers;
// first === 10, third === 30
```

The second element is skipped; it is not assigned to a variable.

## Interview Q&A

**Q: When should you use map instead of forEach?**  
A: Use `map()` when you need a new array of transformed values. Use `forEach()` for side effects when you do not need a returned array.

**Q: What is the difference between spread and rest?**  
A: Spread expands values; rest collects remaining values.

**Q: Does object spread create a deep copy?**  
A: No. It creates a shallow copy.

**Q: When is a destructuring default value applied?**  
A: When the extracted value is `undefined`, not when it is `null`.
