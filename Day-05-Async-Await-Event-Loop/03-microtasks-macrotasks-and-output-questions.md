# Microtasks, Tasks, and Event Loop Output Questions

## Common scheduling examples

| Operation | Scheduling |
|---|---|
| `Promise.then/catch/finally` callbacks | Microtask |
| Continuation after `await` | Microtask |
| `queueMicrotask()` | Microtask |
| `setTimeout()` callback | Task (often called macrotask) |
| `setInterval()` callback | Task (often called macrotask) |

## Basic ordering

```js
console.log("A");
setTimeout(() => console.log("B"), 0);
Promise.resolve().then(() => console.log("C"));
console.log("D");
```

Output:
```text
A
D
C
B
```

Synchronous statements run first. The Promise callback runs at the microtask checkpoint before the timer task.

## Microtasks can schedule more microtasks

```js
console.log("Start");

Promise.resolve().then(() => {
  console.log("Promise 1");
  Promise.resolve().then(() => console.log("Promise 2"));
});

setTimeout(() => console.log("Timer"), 0);
console.log("End");
```

Output:
```text
Start
End
Promise 1
Promise 2
Timer
```

The microtask queue is drained before the next task is run.

## Timer callback that schedules a Promise

```js
console.log("A");

setTimeout(() => {
  console.log("B");
  Promise.resolve().then(() => console.log("C"));
}, 0);

Promise.resolve().then(() => {
  console.log("D");
  setTimeout(() => console.log("E"), 0);
});

console.log("F");
```

Output:
```text
A
F
D
B
C
E
```

After the first timer task logs B, its Promise callback C runs at the microtask checkpoint before the next timer task E.

## Await schedules a continuation

An async function runs synchronously until it reaches an `await`. Its continuation is scheduled as a microtask, even if the awaited Promise is already fulfilled.

```js
async function test() {
  console.log("A");
  await Promise.resolve();
  console.log("B");
  await Promise.resolve();
  console.log("C");
}

console.log("Start");
test();
Promise.resolve().then(() => console.log("D"));
console.log("End");
```

Output:
```text
Start
A
End
B
D
C
```

The first await schedules B. The following Promise schedules D. When B runs, the second await schedules C behind D.

## Interview checklist

- Never assume `setTimeout(fn, 0)` executes immediately.
- Promise callbacks and await continuations are microtasks.
- Timer callbacks are tasks, not microtasks.
- Microtasks scheduled by a running microtask are processed before the next task.
- Each await can schedule a separate continuation.
