# this, call(), apply() and bind()

## 1. this in regular functions

For a regular function, `this` is determined by how the function is called (the call site), subject to strict-mode and other invocation rules.

### Implicit binding

```js
const user = {
  name: "Raj",
  greet() {
    console.log(this.name);
  }
};

user.greet(); // Raj
```

The method is invoked through `user`, so `this` refers to `user`.

If the method is extracted and called as a standalone function, the original object binding is lost:

```js
const greetFn = user.greet;
greetFn();
```

In a browser non-strict script, `this` is the global object; in strict mode, `this` is `undefined`. Accessing `this.name` in strict mode would throw a TypeError.

## 2. Arrow functions and this

Arrow functions do not have their own `this`. They capture `this` lexically from the surrounding scope. They are useful when a callback should retain the surrounding function's `this`.

```js
const user = {
  name: "Raj",
  greet() {
    const inner = () => console.log(this.name);
    inner();
  }
};

user.greet(); // Raj
```

Avoid using an arrow function as an object method when you expect `this` to refer to that object.

In React function components, arrow functions are commonly used for concise callbacks and event handlers. Function components do not have the class-component `this` binding issue.

## 3. Explicit binding

### call()

Invokes a function immediately with a specified `this` value and arguments passed individually.

```js
function greet(city, country) {
  console.log(`${this.name} from ${city}, ${country}`);
}

const user = { name: "Raj" };
greet.call(user, "Hyderabad", "India");
```

### apply()

Invokes a function immediately with a specified `this` value and arguments supplied as an array or array-like object.

```js
greet.apply(user, ["Hyderabad", "India"]);
```

### bind()

Returns a new function with `this` bound to the supplied value. It does not invoke the function immediately. It can also pre-fill arguments.

```js
const boundGreet = greet.bind(user, "Hyderabad");
boundGreet("India");
```

## 4. Comparison

| Method | Arguments | Executes immediately? |
|---|---|---|
| `call()` | Individually | Yes |
| `apply()` | Array/array-like | Yes |
| `bind()` | Individually (can be pre-filled) | No; returns a function |

A bound function's `this` cannot be replaced by calling that bound function with another `thisArg` via `call` or `apply`.

```js
const user1 = { name: "Raj" };
const user2 = { name: "Kumar" };

function greet() {
  console.log(this.name);
}

const boundGreet = greet.bind(user1);
boundGreet.call(user2); // Raj
```

## 5. Interview summary

- Regular-function `this` depends on invocation.
- Arrow-function `this` is lexical.
- `call` and `apply` invoke immediately.
- `bind` returns a new function with bound `this`.
