// Part 1: The Execution Script
console.log("1. Start");

setTimeout(() => {
  console.log("2. Timer Code");
}, 0); 

Promise.resolve().then(() => {
  console.log("3. Promise Code");
});

console.log("4. End");


// =========================================================================
// PART 2: THE VISUAL MENTAL MODEL (Why the output is 1 -> 4 -> 3 -> 2)
// =========================================================================
/*
  When this script executes, the JavaScript Engine processes it like this:

  1. SYNCHRONOUS TASK RUNNERS (Call Stack):
     - console.log("1. Start") executes immediately. -> [Prints 1]
     - setTimeout(..., 0) is encountered. It registers a timer with the browser APIs.
     - Promise.resolve().then(...) is encountered. It registers a callback.
     - console.log("4. End") executes immediately. -> [Prints 4]

  2. THE ASYNC QUEUES (Behind the scenes):
     
     ┌────────────────────────────────────────────────────────┐
     │  MICROTASK QUEUE (VIP High Priority Line)              │
     │  👉 [ () => console.log("3. Promise Code") ]           │
     └────────────────────────────────────────────────────────┘
                                 ▲
                                 │ The Event Loop empties this WHOLE queue first!
                                 │
     ┌────────────────────────────────────────────────────────┐
     │  MACROTASK QUEUE (Regular Callback Line)               │
     │  👉 [ () => console.log("2. Timer Code") ]             │
     └────────────────────────────────────────────────────────┘

  3. THE EVENT LOOP EXECUTION ORDER:
     - Step 1: Call Stack is clear.
     - Step 2: Check Microtask Queue. Found Promise Code. Run it! -> [Prints 3]
     - Step 3: Microtask Queue is now completely empty.
     - Step 4: Check Macrotask Queue. Found Timer Code. Run it! -> [Prints 2]

  FINAL OUTPUT DETECTED: 1 -> 4 -> 3 -> 2
*/