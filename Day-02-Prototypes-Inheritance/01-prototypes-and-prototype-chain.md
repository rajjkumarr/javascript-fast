# JavaScript Prototypes and Prototype Chain

## What is a prototype?

A prototype is an object from which another object can inherit properties and methods. JavaScript uses prototype-based inheritance.

Prototypes let multiple objects share behavior instead of storing duplicate copies of the same method on every instance. This improves code reuse and can reduce memory usage.

## How property lookup works

When accessing `obj.property`, JavaScript:

1. Checks whether the property exists directly on the object.
2. If not found, checks the object's prototype.
3. Continues up the prototype chain.
4. Returns `undefined` if the property is not found before the chain ends at `null`.

```js
const user = { name: "Raj" };

console.log(user.name);       // "Raj"
console.log(user.age);        // undefined
console.log(user.toString()); // "[object Object]"
```

`toString()` is inherited from `Object.prototype`.

## prototype vs __proto__

- `Person.prototype` is a property on a constructor function. It is the object that instances created with `new Person()` inherit from.
- `user.__proto__` is a legacy accessor that exposes the object's internal prototype.
- Prefer `Object.getPrototypeOf(user)` to inspect an object's prototype.

```js
function Person(name) {
  this.name = name;
}

const user = new Person("Raj");

console.log(Person.prototype === user.__proto__); // true
console.log(Object.getPrototypeOf(user) === Person.prototype); // true
```

## Own properties vs inherited properties

```js
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function () {
  return `Hello, ${this.name}`;
};

const user = new Person("Raj");

console.log(user.hasOwnProperty("name"));  // true
console.log(user.hasOwnProperty("greet")); // false
console.log(user.greet());                 // "Hello, Raj"
```

The `name` property belongs directly to the instance. The `greet` method is inherited from `Person.prototype`.

## Interview points

- JavaScript inheritance is prototype-based.
- Objects can inherit properties and methods through the prototype chain.
- If a property is not found, lookup continues up the chain.
- The chain ends at `null`.
- `Object.getPrototypeOf()` is preferred over directly using `__proto__`.

## Common mistakes

- Saying a missing property returns `null`; it normally returns `undefined`.
- Confusing `Person.prototype` with an instance's prototype.
- Assuming inherited methods are own properties of an instance.

## Quick revision

A prototype enables objects to share behavior. JavaScript searches an object's prototype chain when a requested property is not found directly on that object.
