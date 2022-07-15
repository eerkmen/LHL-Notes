# Asynchronous Control Flow

## Synchronous and Asynchronous Code

### Synchronous Code

* The code you've been writing thus far is synchronous.
* This type of code runs statement-after-statement, line-after-line, in order.
* A statement must complete execution before proceeding to the next.
* Synchronous code is referred to as "**BLOCKING**," as later lines will not, and cannot, run until the current one is completed; it "BLOCKS" the upcoming lines.
* Some languages, like [Ruby](https://www.ruby-lang.org/en/) or [PHP](https://php.net/), are largely - if not completely - synchronous; while others like [JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous) ([Node](https://nodejs.org/en/)) and [modern C#](https://docs.microsoft.com/en-us/dotnet/csharp/async) have adopted built-in asynchronous features.

## Asynchronous Code

* In a web browser, consider a website like Facebook.
  * As you scroll down the page, new content appears in the feed.
  * Imagine if the requests for more posts was **synchronous**.
  * In the synchronous case, all JavaScript related to the website's UI would cease until the new content is loaded...
  * Because Facebook's UI is nearly entirely JS-based, this means everything from a *Like 👍* button to viewing an image, simply couldn't work (or would experience a serious delay.)
  * This is where asynchronous JavaScript often comes into play:
    * Instead of the JS-based UI locking up, the request is made alongside the rest of the code.
    * A function is set, basically, to run when the new content is received by the browser: causing the new posts to generate without stopping, or, "BLOCKING" the rest of the script(s).
* Asynchronous code is considered "**NON-BLOCKING**," as the lines that follow the asynchronous statement(s) will be executed straight away, despite how long the asynchronous statement takes to run.
* Asynchronous statements are typically used exclusively for code that is likely to take a while to run, or that may take an unknown amount of time to run.
  * Most often, use-cases are related to:
    * Calling to a remote server or API for information in real-time.
    * Database operations.
    * Opening, writing to, and closing files.
    * Running or displaying timers, or ensuring a set delay between program actions.
* JavaScript makes a point of allowing use of this essential feature both in the browser, and through server-side use in NodeJS, making the language extremely flexible for a wide array of programs and use-cases.

## Synchronous Code Example

Run the following test code. What order does it run in? Why is this?

```JavaScript
console.log('Beginning of script.');
console.log('Middle of script.');
console.log('Bottom of script.');
```

Modify the existing file, or create a new one, accordingly:

```JavaScript
console.log('Beginning of script.');

for (let i = 0; i < 30; i++) {
  const date = new Date();
  console.log(date);
}

console.log('Bottom of script.');
```

Now what order does it run in? Note that it takes time for the `for` loop to run through its iterations, did this effect when the "Bottom of script" message appeared in the terminal? Why or why not?

So far, this is synchronous ("blocking") code. Each statement must complete before the next one is allowed to run.

Note that JavaScript in its current form, on modern computers, is usually capable of running a handful or more operations per millisecond. Try increasing the iterations in the loop and you'll see how fast your JavaScript parser is able to execute your code! See how many logs output between milliseconds.

## Asynchronous Code Examples

### [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)

Let's try another example, this time adding a new function to our developer toolbelt: `setTimeout()`. this function has two parameters:

* `function`: A named, or anonymous, function describing what should happen after the set delay.
* `delay`: An integer representing the number of milliseconds that should pass before executing the provided function.

In a new file, try the following:

```JavaScript
console.log('Beginning of script.');

setTimeout(() => {
  console.log('Timeout has run.');
}, 3000);

console.log('Bottom of script.');
```

What order do you see the console outputs in now? Is this the order you expected?

`setTimeout()` runs asynchronously. This means that lines after a setTimeout will execute right away, and the timeout delay is kept track of separately in the background. Notice in the example's output how this presents itself in practice!

Modify the above as follows to further illustrate this point.

```JavaScript
console.log('Beginning of script.');

setTimeout(() => {
  console.log('Timeout has run. (50ms)');
}, 50);

setTimeout(() => {
  console.log('Timeout has run. (3000ms)');
}, 3000);

setTimeout(() => {
  console.log('Timeout has run. (2000ms)');
}, 2000);

setTimeout(() => {
  console.log('Timeout has run. (1000ms)');
}, 1000);

console.log('Bottom of script.');
```

Again, consider the order of the output you receive, and ask yourself *why*.

This can take some getting used to, and may not always be obvious! Practice will help you identify more clearly and easily how and when asynchronous code is likely to execute.

A more advanced example can involve nesting. If you're feeling brave, try the following!

```JavaScript
console.log('Beginning of script.');

setTimeout(() => {
  console.log('Timeout has run. (50ms)');
}, 50);

setTimeout(() => {
  console.log('Timeout has run. (3000ms)');
}, 3000);

setTimeout(() => {
  console.log('Timeout has run. (2000ms)');

  // Nested example:
  setTimeout(() => {
    console.log('Timeout inside (2000ms). (500ms)');
  }, 500);
}, 2000);

setTimeout(() => {
  console.log('Timeout has run. (1000ms)');
}, 1000);

console.log('Bottom of script.');
```

Where does the output from the nested example appear? Is it where you expected? Read the millisecond delays set in each statement carefully, therein lies the answer! First the 2000ms delay occurs, and only *then* is the 500ms delay registered. Nesting these can be tricky, and will likely not come up often, but it is a very solid way to see asynchronous JavaScript in action.

#### When does Asynchronous Code run in JavaScript?

Alright, so we've made note that `setTimeout()` runs after a specified delay, but there is another thing we'll want to keep in mind when it comes to JavaScript. The following sample will show us this:

```JavaScript
setTimeout(()=>{
  console.log('Timeout ran.');
}, 1);

for (let i = 0; i < 100000; i++) {
  console.log('For loop:', i);
}

```

When did we see the "Timeout ran" output? Why is this?

Imagine you're in a supermarket. Each customer has to line up at checkout in order to pay for their goods and leave. Each transaction takes a bit of time to execute, but they are dealt with in the order they arrive.

When an asynchronous function is called, it is registered in that moment but takes its place at the back of the synchronous line-up. This is important to keep in mind, lest the order of programmed operations catch you by surprise! All asynchronous operations actually *execute*, despite time of registration, only after the synchronous script has completed.

This can cause unexpected blocking if you're not careful about how your script is written. Most instructions are so quick, you'll often not notice a difference, but for times in which there is a long initial execution time in your file it will be very apparent.

#### [High Order Functions](https://en.wikipedia.org/wiki/Higher-order_function) and `setTimeout()`

A high order function does one of or both of:

1. Takes in one (or more) functions as arguments.
2. Returns a function.

The order of operations, at first, can get trickier and harder to follow when mixing high order functions and timeouts. Let's work on a fairly basic example and see if we can trace what happens, when, and why.

Before running the following, lay out the order that you think the `console.log()`s will display in.

```JavaScript
const highOrderFunction = (callbackFunction) => {
  const data = {
    name: 'Ada Lovelace',
    hobbies: ['Flying']
  };

  console.log('1) Before setTimeout.');

  setTimeout(() => {
    data.hobbies.push('Programming');
    callbackFunction(); // Execute callback.
  }, 3000);

  console.log('2) After setTimeout.');
};

console.log('3) Before the high order execution.');
highOrderFunction(() => {
  console.log('4) Inside the callback.');
});
console.log('5) After the high order execution.');
```

How'd you do in your guess? Remember we're doing this to learn. In the real world, it helps to add logs, run the code, and see if it ran as you'd expect. Of course, if it didn't, you can make changes and try, try again!

Let's see how `return` would fit into a use-case like this for a high-order function containing an asynchronous instruction. Modify the code like so, feel free to move the return around and see how it might affect your result.

```JavaScript
const highOrderFunction = (callbackFunction) => {
  const data = {
    name: 'Ada Lovelace',
    hobbies: ['Flying']
  };

  console.log('1) Before setTimeout.');

  setTimeout(() => {
    data.hobbies.push('Programming');
    callbackFunction(); // Execute callback.

    // Can we return HERE; why or why not?
  }, 3000);

  console.log('2) After setTimeout.');

  // What will data.hobbies contain in the return?
  return data;
};

console.log('3) Before the high order execution.');
const highOrderReturnValue = highOrderFunction(() => {
  console.log('4) Inside the callback.');
});
console.log('5) After the high order execution.');

console.log('6) Value returned from highOrderFunction():', highOrderReturnValue);
```

Does the data returned contain the change to the hobbies property? Why or why not? What if you move it inside of the `setTimeout()`? Why does that work, or fail?

Let's try something else to get the updated property value.

```JavaScript
const highOrderFunction = (callbackFunction) => {
  const data = {
    name: 'Ada Lovelace',
    hobbies: ['Flying']
  };

  console.log('1) Before setTimeout.');

  setTimeout(() => {
    data.hobbies.push('Programming');
    callbackFunction(); // Execute callback.
  }, 3000);

  console.log('2) After setTimeout.');

  return data;
};

console.log('3) Before the high order execution.');
const highOrderReturnValue = highOrderFunction(() => {
  console.log('4) Inside the callback.');
});
console.log('5) After the high order execution.');

console.log('6) Value returned from highOrderFunction():', highOrderReturnValue);

setTimeout(() => {
  console.log('7) highOrderReturnValue after 2s (2000ms):', highOrderReturnValue);
}, 2000);
```

This is bad practice, however. This will always cause an additional delay. In the real world, we try to avoid patterns like this, as we're usually calling upon external APIs, reading/writing database entries, or reading/writing information from files which take an unknown amount of time. With a hard timeout delay, we risk either having our operation wait too long, causing the user to wait, or not wait long enough, often causing errors or empty data.

In the real world, in a case like this, you'll want your callback to accept a value. This way, as soon as your asynchronous instruction reaches the callback step, it is able to pass updated data in real-time to the next set of instructions. You'll see patterns like this very often soon in the program as we start to explore topics related to AJAX and database operations.

```JavaScript
const highOrderFunction = (callbackFunction) => {
  const data = {
    name: 'Ada Lovelace',
    hobbies: ['Flying']
  };

  console.log('1) Before setTimeout.');

  setTimeout(() => {
    data.hobbies.push('Programming');

    // Pass in data object to callback!
    callbackFunction(data); // Execute callback.
  }, 3000);

  console.log('2) After setTimeout.');

  // Old approach:
  // return data;
};

console.log('3) Before the high order execution.');
const highOrderReturnValue = highOrderFunction((data) => {
  // Add a parameter to your callback and access the argument value:
  console.log('4) Inside the callback. Data contains:', data);
});
console.log('5) After the high order execution.');

console.log('6) Value returned from highOrderFunction():', highOrderReturnValue);

// Old approach:
// setTimeout(() => {
//   console.log('7) highOrderReturnValue after 2s (2000ms):', highOrderReturnValue);
// }, 2000);
```

#### Multiple Timeouts Sample

Perhaps, a more easily adjustable approach to writing a timeout, could look like this:

```JavaScript
const delay = 2000;

setTimeout(() => {
  console.log('Timeout completed:', delay);
}, delay);
```

Would would, in this case, be able to change the delay in one place easily for experimentation. In the same vein, let's try working with an array of delays instead.

```JavaScript
const delays = [5000, 1000, 6000, 98, 1500, 1, 3002, 3000];

for (let delay of delays) {
  setTimeout(() => {
    console.log('Timeout ran with delay of:', delay);
  }, delay);
}
```

In what order are these registered? In what order will they display?

Remember: `setTimeout()` itself registers synchronously, but the callback passed in is run asynchronously at a later time (determined by the delay you pass in.)

### [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/setInterval)

The `setInterval()` function is *similar* to `setTimeout()`, with one big key difference: it is repeating based on the delay.

Every time the set delay time passes, the passed-in function executes again.

That means in a case like `setInterval(yourFunction, 3000)`, you'll find that `yourFunction` runs every three seconds (3000ms.)

It will keep running again, and again, *and again* forever unless you terminate the program or run [`clearInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/clearInterval) in your script.

Let's run a simple example to get used to the idea, and how it differs from our experience with timeouts.

```JavaScript
setTimeout(() => { // Runs once after 1s.
  console.log('1000ms timeout ran.');
}, 1000);

setInterval(() => { // Runs EVERY second.
  console.log('1000ms interval ran.');
}, 1000);
```

To stop this file from running, you'll have to `CTRL`+`C` and terminate the script manually.

Now it is running again and again for the interval. What's a real-world example use-case for this? The original Rudder queue worked a little differently than the current one. `setInterval()` was actually used to have JavaScript reach out to the Rudder server every 7000ms to check if new students were in queue (opposed to a mentor refreshing the page every moment or two.)

We don't always want to run an interval *forever* though. That's where `clearInterval()` comes in. Let's have our interval run a few times, and then stop it dead in its tracks.

```JavaScript
let intervalCount = 0;

// We'll need to use a variable to keep track of our interval; we need to be able to tell JS which interval we want to clear later.
const myInterval = setInterval(() => {
  intervalCount++;
  console.log('Interval has run '+intervalCount+' time(s).');
  if (intervalCount >= 3) clearInterval(myInterval);
}, 1000);
```

Note that it *is* possible to [`clearTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/clearTimeout) as well, it is just much less commonly used.

## [NodeJS `fs` Module](https://nodejs.org/api/fs.html#file-system)

### What is the `fs` Module?

NodeJS is capable of reading, writing, and deleting files on the host machine. Note, however, that we don't have access to this feature by default. We must first `require` the `fs` module, and then we're good to go!

Today we'll be looking at the fs module [callback API](https://nodejs.org/api/fs.html#callback-example) way of doing things, but note there are alternative syntaxes and options available as well.

Of these file actions, the following actively change an existing file:

* Create
* Update
* Delete (Unlink)

The least destructive action we can take, is **Read**, so let's start with that! Firstly, we'll need a file that we can read from.

### Sample Text ([Lorem Ipsum](https://en.wikipedia.org/wiki/Lorem_ipsum))

When you need placeholder content, you can mash keys all over the keyboard, but this is inconsistent, often doesn't realistically show how words would appear on a page or in a program, and can damage your equipment. Professionally, you'll want to explore other options. The most common of which, for both developers and designers, is [Lorem Ipsum](https://loremipsum.io/). There are plenty of generators available online, so it is common to [run a search for one](https://duckduckgo.com/?q=lorem+ipsum&t=h_&ia=answer&iax=answer) or [install an extension in your code editor](https://marketplace.visualstudio.com/items?itemName=Tyriar.lorem-ipsum) for convenience.

By default, in Visual Studio Code, you can even get away with typing `lorem` followed by a number of words, and hitting `tab` or `enter` inside of an `html` file and it will populate the line with that many words. For example, if you had a file called `index.html` and opened it in Visual Studio Code, you'd be able to simply type `lorem50` in a blank line, hit enter, and there would be 50 random latin words populating that line.

Create a text file in your project folder with some sample text.

**sample.txt**

```
Minus voluptas sequi est corrupti laboriosam eum. Nemo corporis et a perferendis enim. Blanditiis enim praesentium rerum eius quia magnam quisquam doloribus.

Et nobis est ut tempora aperiam minima. Animi voluptatem cupiditate a quidem beatae quasi eligendi. Dolores mollitia voluptatum possimus doloribus tempore voluptas qui. Tempora tenetur laborum nemo qui.

Quas itaque sit sunt molestiae perspiciatis quasi. Quasi fuga excepturi nostrum sunt et. Earum et error beatae illo recusandae repudiandae. Dolor beatae voluptatem quia.

Aut pariatur id quo. Hic aspernatur recusandae natus est ipsum tempora maiores molestiae. In deserunt asperiores eum iste maxime. Ratione ea eligendi ullam.

Nulla dolore velit id sed sed ut. Incidunt officia minima omnis id consectetur eveniet vel minima. Iste vel accusamus eos quis ea officia sapiente. Dolorem quo sed est sequi esse itaque consequatur. Tempora voluptatem aut sed ut possimus. Similique consequatur inventore omnis.
```

Now, we'll have something to read from.

### Reading from a File (Synchronously)

The `fs` module has a `readFileSync` method that reads a file synchronously. Let's give that a try first:

```JavaScript
const fs = require('fs');

console.log('Before reading file.');

const fileContents = fs.readFileSync('./sample.txt');
console.log('Contents of file:', fileContents);

console.log('After reading file.');
```

Was there anything unexpected about the way the file contents printed in the terminal? It printed in the order we might expect, but it is in [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal)... this is difficult for humans to read. We're seeing the raw value buffered in your system's memory.

It turns out, the `readFileSync` method takes in an additional parameter for options. One of the properties we can adjust is `encoding`, which decides which standard for character-mapping should be used in the returned string. By default it is assigned `null`, and so the method will return the information in its rawest form. An encoding more friendly for us to understand, however, is that which we use daily in writing web pages and many other text-oriented files, [`UTF-8`](https://en.wikipedia.org/wiki/UTF-8). Let's give that a try:


```JavaScript
const fs = require('fs');

console.log('Before reading file.');

const fileContents = fs.readFileSync('./sample.txt', {encoding: 'utf-8'});
console.log('Contents of file:', fileContents);

console.log('After reading file.');
```

Much more readable!

### Reading from a File (Asynchronously)

For an asynchronous approach with `fs`, we'll instead use the `readFile` method. Let's make the change:

```JavaScript
const fs = require('fs');

console.log('Before reading file.');

const fileContents = fs.readFile('./sample.txt', {encoding: 'utf-8'}, (fileContents) => {
  console.log('Contents of file:', fileContents);
});

console.log('After reading file.');
```

Aha! Now we see our `console.log` outputs in the order we'd expect for an asynchronous operation... however now our `fileContents` aren't displaying as we'd expect. What gives? [Let's read the documentation](https://nodejs.org/api/fs.html#fsreadfilepath-options-callback).

`readFile`'s callback function parameter will be given 2 arguments:

1. An `err` object with details we can use to troubleshoot, should something go wrong.
2. A `data` value, representing the contents of the file.

With this in mind, let's adjust our code.

```JavaScript
const fs = require('fs');

console.log('Before reading file.');

const fileContents = fs.readFile('./sample.txt', {encoding: 'utf-8'}, (error, fileContents) => {
  if (error) {
    console.error('An error was encountered:', error);
  } else {
    console.log('Contents of file:', fileContents);
  }
});

console.log('After reading file.');
```

Much better. Try changing your read file path to one that doesn't exist, if you'd like to view an error.

### Writing a File

Writing a file looks similar. Keep in mind, depending on user permissions assigned to NodeJS, you may encounter errors when attempting to create or edit files.

```JavaScript
const fs = require('fs');

const myString = 'Hello, World!\n';

console.log('Before writing to file.');

fs.writeFile('./new-file.txt', myString, {encoding: 'utf-8'}, (error) => {
  if (error) {
    console.error('An error was encountered:', error);
  } else {
    console.log('File saved successfully.');
  }
});

console.log('After writing to file.');
```

### Deleting a File

Even simpler to write, but the most dangerous of all: file deletion. In filesystems, we usually refer to deletion as unlinking, thus the method is named as such to reflect this.

```JavaScript
const fs = require('fs');

console.log('Before deletion of file.');

fs.unlink('./new-file.txt', (error) => {
  if (error) {
    console.error('An error was encountered:', error);
  } else {
    console.log('File deleted successfully.');
  }
});

console.log('After deletion of file.');
```

## Resources

* [MDN: Synchronous](https://developer.mozilla.org/en-US/docs/Glossary/Synchronous) vs [MDN: Asynchronous](https://developer.mozilla.org/en-US/docs/Glossary/Asynchronous)
* [MDN: Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)
* [Blocking versus Non-Blocking](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/)
* [MDN: `setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)
  * [MDN: `clearTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/clearTimeout)
* [MDN: `setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/setInterval)
  * [MDN: `clearInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/clearInterval)
* [Wikipedia: Lorem Ipsum](https://en.wikipedia.org/wiki/Lorem_ipsum)
* [NodeJS: `fs` Module](https://nodejs.org/api/fs.html#file-system)