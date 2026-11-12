# Variables, Scope, Hoisting and TDZ

## 1. Variable declarations

| Keyword | Scope | Reassignment | Redeclaration in same scope |
|---|---|---|---|
| `var` | Function (or global when declared globally) | Yes | Yes |
| `let` | Block | Yes | No |
| `const` | Block | No | No |

Use `const` by default. Use `let` when reassignment is required. Avoid `var` in modern JavaScript unless there is a specific reason.

A `const` binding cannot be reassigned, but an object or array stored in a const binding can still be mutated:

```js
const user = { name: "Raj" };
user.name = "Kumar"; // Allowed
// user = {};        // TypeError
```

## 2. Scope

Scope determines where a variable can be accessed.

- **Global scope:** available in the applicable global environment.
- **Function scope:** variables declared with `var` are scoped to the function.
- **Block scope:** `let` and `const` are scoped to the nearest enclosing block.

Lexical scope means access to variables is determined by where code is written, not where a function is called.

## 3. Hoisting

JavaScript creates bindings before executing statements in a scope, but different declarations are initialized differently.

- `var` is initialized to `undefined`.
- `let` and `const` are created but remain uninitialized until their declaration is evaluated.
- Function declarations are available according to their declaration-instantiation rules.

Example:

```js
console.log(a); // undefined
var a = 10;

console.log(b); // ReferenceError
let b = 20;
```

The first log succeeds because `a` is initialized to `undefined`. Accessing `b` before initialization throws a ReferenceError.

## 4. Temporal Dead Zone (TDZ)

The TDZ is the period from entering a scope until a `let` or `const` binding is initialized. Accessing the binding during this period throws a ReferenceError.

## 5. Interview points

- `let` and `const` are hoisted, but are not initialized to `undefined`.
- `const` cannot be redeclared or reassigned in the same scope.
- Block scope and function scope are different.
- Prefer precise wording: the global execution context is pushed onto the call stack; variables themselves are not pushed onto the call stack.
