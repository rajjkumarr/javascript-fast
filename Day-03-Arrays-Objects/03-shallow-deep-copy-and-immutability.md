# Day 3.3 — Shallow Copy, Deep Copy, and Immutability

## 1. References and shallow copy

Objects and arrays are reference values. A shallow copy creates a new outer object, but nested objects and arrays remain shared references.

```js
const user1 = {
  name: "Raj",
  address: { city: "Hyderabad" }
};

const user2 = { ...user1 };

user2.name = "John";
user2.address.city = "Bangalore";

console.log(user1.name);         // "Raj"
console.log(user1.address.city); // "Bangalore"
```

Why?
- The outer objects are different.
- Both `address` properties point to the same nested object.
- Changing a nested property through either reference affects that shared object.

The same issue occurs with nested arrays:

```js
const original = { name: "Raj", skills: ["React", "Node"] };
const copy = { ...original };

copy.skills.push("MongoDB");

console.log(original.skills); // ["React", "Node", "MongoDB"]
console.log(copy.skills);     // ["React", "Node", "MongoDB"]
```

## 2. Deep copy

A deep copy duplicates nested data too, so the copied structure does not share those nested object/array references with the original.

```js
const original = {
  name: "Raj",
  skills: ["React", "Node"]
};

const copy = structuredClone(original);
copy.skills.push("MongoDB");

console.log(original.skills); // ["React", "Node"]
console.log(copy.skills);     // ["React", "Node", "MongoDB"]
```

### structuredClone() notes

- Supports many structured-cloneable values, including nested objects, arrays, Date, Map, Set, and circular references.
- Does not clone functions or DOM nodes.
- Does not preserve custom class prototypes and methods.
- It can throw a DataCloneError for unsupported values.

Do not assume JSON stringify/parse is a universal deep-copy solution: it loses or changes values such as undefined properties, functions, dates, maps, sets, and circular references cannot be serialized.

## 3. Object.assign() vs spread

Both can make a shallow copy.

```js
const user = { name: "Raj", age: 25 };

const copy1 = { ...user };
const copy2 = Object.assign({}, user);
```

Both create a new outer object. Neither performs a deep copy.

```js
const user1 = {
  name: "Raj",
  address: { city: "Hyderabad" }
};

const user2 = Object.assign({}, user1);
user2.address.city = "Bangalore";

console.log(user1.address.city); // "Bangalore"
```

## 4. Mutability vs immutability

- Mutable operation: changes the existing object or array.
- Immutable update: creates a new value while leaving the previous value unchanged.

```js
const user = { name: "Raj", age: 25 };

// Mutation
user.age = 26;

// Immutable-style update
const updatedUser = { ...user, age: 27 };
```

Array examples:

```js
const numbers = [1, 2, 3];

// Mutates original
numbers.push(4);

// Creates a new array
const nextNumbers = [...numbers, 5];
```

## 5. Why immutability matters in React

Treat React state as immutable. Do not modify state objects or arrays directly. Instead, create a new object or array and pass it to the setter.

Avoid:

```js
user.age = 26;
setUser(user);
```

Prefer:

```js
setUser(prev => ({
  ...prev,
  age: 26
}));
```

React uses `Object.is()` to compare the previous and next state. If the same object reference is passed back, React may skip rendering. Creating a new reference makes the update detectable.

For nested state, copying only the outer object is not enough if you then mutate a nested object. Copy each level that needs to change:

```js
setUser(prev => ({
  ...prev,
  address: {
    ...prev.address,
    city: "Bangalore"
  }
}));
```

## Interview Q&A

**Q: What is the difference between shallow and deep copy?**  
A: A shallow copy creates a new outer object but shares nested references. A deep copy also creates independent copies of nested data.

**Q: Does Object.assign perform a deep copy?**  
A: No. It performs a shallow copy.

**Q: Why avoid direct state mutation in React?**  
A: It preserves the same reference and can prevent React from detecting the update. Immutable updates create a new reference.

**Q: Is spread enough to safely update nested React state?**  
A: Only if all changed nested levels are also copied. Copy each level along the path to the property being updated.
