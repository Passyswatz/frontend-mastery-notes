# Browser Internals: Microtasks vs. Macrotasks

A technical breakdown of how the browser engine schedules asynchronous execution priorities, focusing on the behavioral mechanics of the Event Loop.

## The Execution Paradox

```javascript
console.log("1. Script Start");

setTimeout(() => {
  console.log("2. Macrotask (setTimeout)");
}, 0);

Promise.resolve().then(() => {
  console.log("3. Microtask (Promise)");
});

console.log("4. Script End");
