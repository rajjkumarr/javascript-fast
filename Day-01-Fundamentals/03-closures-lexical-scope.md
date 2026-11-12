# Closures and Lexical Scope

## 1. Lexical scope

Lexical scope means JavaScript determines variable accessibility from the structure and location of code in the source, not from the place where a function is later called.

```js
let message = "Global";

function outer() {
  let message = "Outer";

  return function inner() {
    console.log(message);
  };
}

const fn = outer();

function execute(callback) {
  let message = "Execute";
  callback();
}

execute(fn); // Outer
```

The inner function was defined inside `outer`, so its identifier lookup follows that lexical environment. Calling it from `execute` does not change its lexical scope.

## 2. Closure

A closure is a function together with its retained access to the lexical environment in which it was created. This lets the function continue to access outer variables after the outer function has returned, as long as that environment remains reachable.

```js
function createCounter() {
  let count = 0;

  return function increment() {
    count += 1;
    return count;
  };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
```

The returned function retains access to the same `count` binding.

## 3. Multiple closures sharing state

```js
function createCounter() {
  let count = 0;

  return {
    increment() {
      count++;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
counter.increment();
counter.increment();

console.log(counter.getCount()); // 2
```

Both methods close over the same `count` variable. This pattern can provide encapsulated state: callers use the exposed methods rather than directly accessing the local variable.

## 4. Common uses

- Encapsulating/private state.
- Function factories.
- Callbacks and event handlers.
- Maintaining state between function calls.

## 5. Interview distinction

- **Lexical scope:** where variable access is determined.
- **Closure:** a function retaining access to its surrounding lexical environment.
- A closure retains access to a variable binding; it is not simply a snapshot or copied value.
