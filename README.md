What is async programming? Its when the function executes, and continues to work on in the background.

Lets consider that we have an expensive calculation (__Some function thats takes a while to run__)
```ts
function expensive() {
  return new Promise(async (resolve, reject) => {
    setTimeout(() => resolve(), 5000);
  });
}

// This is an IIFE, its a function that executes as soon as its created.
// We are using an IIFE because you might be running this code in a commonjs environment and they don't support the await keyword if its not inside a function body.
(async () => {
  console.log('Executing calculation');

  expensive().then(() => {
    console.log('Expensive calculation finished');
  });

  console.log('The caclulation is still running, but this line got executed anyways (You can thank async for that)');
})();
```

Now lets say you want to pause the current block, and only continue when `expensive()` has finished working.
Well then, lets take a look at this new example

```ts
function expensive() {
  return new Promise(async (resolve, reject) => {
    setTimeout(() => resolve(), 5000);
  });
}

// This is an IIFE, its a function that executes as soon as its created.
// We are using an IIFE because you might be running this code in a commonjs environment and they don't support the await keyword if its not inside a function body.
(async () => {
  console.log('Executing calculation');

  // Await means wait for the asyncronous code to finish working, this will pause this block scope code, when resolve is called the promise/async function is done!
  await expensive();

  console.log('Expensive function is done');
})();
```

Now you may notice we are returning `new Promise()`, we did this because setTimeout is non I/O blocking, and so even when the timeout hasnt ended, code bellow it continues to execute.
We can change this though. in this example I will convert setTimeout into a promise/async function
```ts
const wait = (ms) => return new Promise((res) => setTimeout(res, ms));

// We put async before the function keyword to make it a promise/async function.
async function expensive() {
  await wait(5000);
}

// This is an IIFE, its a function that executes as soon as its created.
// We are using an IIFE because you might be running this code in a commonjs environment and they don't support the await keyword if its not inside a function body.
(async () => {
  console.log('Running async...');
  await expensive();
  console.log('Expensive function has finished');
})();
```
