# Day 6 — DOM Events and Event Propagation

## What is a DOM event?
A browser event is an action or occurrence that JavaScript can respond to, such as a click, input, submit, or page load.

```js
button.addEventListener("click", () => {
  console.log("Clicked");
});
```

## Event propagation phases
1. **Capturing:** event travels from ancestors toward the target.
2. **Target:** event reaches the element where it originated.
3. **Bubbling:** event travels from the target back through ancestors.

By default, `addEventListener()` listens during bubbling.

```js
parent.addEventListener("click", handler); // bubbling
parent.addEventListener("click", handler, true); // capturing
parent.addEventListener("click", handler, { capture: true });
```

## stopPropagation vs preventDefault
- `event.stopPropagation()`: stops further propagation; does not cancel the browser's default action.
- `event.preventDefault()`: cancels a cancelable default action (e.g. link navigation); does not stop propagation.

## Event delegation
Attach one listener to a parent and handle bubbled events from child elements. Useful for large lists and dynamically added elements.

```js
list.addEventListener("click", (event) => {
  const button = event.target.closest("button");
  if (!button || !list.contains(button)) return;
  console.log(button.dataset.id);
});
```

Benefits: fewer listeners, centralized handling, works for children added later.

## Interview checklist
- Explain capturing, target, bubbling.
- How to register a capture listener?
- Difference between stopPropagation and preventDefault?
- Why is event delegation useful?
