# Call Stack and Event Loop

## Call Stack

The call stack is a LIFO (Last In, First Out) data structure used by JavaScript to manage function execution.

- When a function is called, its execution context is pushed onto the stack.
- When the function returns, its execution context is popped.
- Variables are stored in execution contexts; individual variables are not themselves pushed onto the call stack.

```js
function first() {
  second();
}
function second() {
  console.log("Second");
}
first();
```

The global execution context is active first. Calling `first` pushes its context; calling `second` pushes another context on top. When `second` returns, its context is removed and execution resumes in `first`.

## Event Loop

The event loop coordinates execution between the call stack and queued callbacks.

A useful simplified model:
1. Run synchronous JavaScript on the call stack.
2. When the stack is clear, process the microtask queue until it is empty.
3. Take the next eligible task (often called a macrotask), run it, then process microtasks again.

The JavaScript runtime environment provides APIs such as timers and networking. The event loop is not simply “the call stack having higher priority”; it coordinates when queued work can run.

## setTimeout

```js
console.log("Start");
setTimeout(() => console.log("Timer"), 0);
console.log("End");
```

Output:
```text
Start
End
Timer
```

A zero delay does not mean the callback runs immediately. The callback becomes eligible after the timer delay and runs as a task when the stack and required microtask checkpoint allow it.

## Interview answer

“The call stack is a LIFO structure that manages function execution contexts. The event loop coordinates queued asynchronous callbacks with the call stack. Once synchronous execution finishes, microtasks are processed before the next task is taken.”
