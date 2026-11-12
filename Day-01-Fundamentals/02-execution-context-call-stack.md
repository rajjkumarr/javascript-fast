# Execution Context and Call Stack

## 1. Execution context

An execution context is the environment in which JavaScript code is evaluated. It contains the information needed to run the code, including its lexical environment and relevant execution state.

Common execution-context types:
- **Global execution context:** created for the script.
- **Function execution context:** created when a function is invoked.
- **Eval execution context:** associated with evaluated `eval` code (generally avoid using eval).

## 2. Creation and execution

A useful learning model divides execution into two broad phases:

1. **Creation/instantiation:** bindings are established; declarations are handled according to their rules.
2. **Execution:** statements run and assignments/expressions are evaluated.

Example:

```js
console.log(a); // undefined
var a = 10;
console.log(a); // 10
```

## 3. Call stack

The call stack tracks active execution contexts in last-in, first-out order.

```js
function first() {
  second();
}

function second() {
  console.log("Hello");
}

first();
```

The global context is active, then `first()` is added, then `second()`. When `second()` completes it is removed, followed by `first()`.

## 4. Important distinction

- **Execution context:** the environment and state needed to execute code.
- **Call stack:** the runtime stack tracking active execution contexts.
- **Scope/lexical environment:** determines how identifiers are resolved.

Variables are not individually pushed onto the call stack. Function calls create execution contexts that are tracked by the stack.
