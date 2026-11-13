# Constructor Functions and Classes

## Constructor functions

A constructor function is a function conventionally used with `new` to create and initialize objects.

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function () {
  return `Hello, ${this.name}`;
};

const user1 = new Person("Raj", 25);
const user2 = new Person("John", 30);
```

### What does new do?

Conceptually, calling `new Person(...)`:

1. Creates a new object.
2. Links its internal prototype to `Person.prototype`.
3. Calls `Person` with `this` referring to the new object.
4. Returns the new object (unless the constructor explicitly returns another object).

## Why define methods on the prototype?

```js
// A new function is created for each instance.
function PersonA(name) {
  this.name = name;
  this.greet = function () {
    return `Hello ${this.name}`;
  };
}

// All instances share one prototype method.
function PersonB(name) {
  this.name = name;
}

PersonB.prototype.greet = function () {
  return `Hello ${this.name}`;
};
```

With the second approach, instances share the same `greet` function rather than each storing a separate function.

## JavaScript classes

Classes provide cleaner syntax over JavaScript's prototype-based inheritance system.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello ${this.name}`;
  }
}

const user = new Person("Raj");

console.log(user.hasOwnProperty("name")); // true
console.log(user.hasOwnProperty("greet")); // false
console.log(Person.prototype.hasOwnProperty("greet")); // true
```

Ordinary methods defined in a class body are placed on the class's prototype, so instances share them.

## Classes vs constructor functions

They use the same underlying prototype mechanism, but class syntax has additional semantics:

- Classes must be called with `new`.
- Class bodies execute in strict mode.
- Class declarations are subject to the temporal dead zone.
- Class methods are non-enumerable by default.

## Interview answer

"JavaScript classes are syntactic sugar over the existing prototype-based inheritance system. They provide a cleaner syntax for creating objects and implementing inheritance. Methods defined in a class are placed on its prototype, allowing instances to share those methods."

## Common mistakes

- Saying classes are a completely separate inheritance mechanism.
- Assuming class methods are copied onto every instance.
- Confusing the constructor's own properties with prototype methods.
