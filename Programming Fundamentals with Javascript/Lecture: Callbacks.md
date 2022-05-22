# Callbacks

### Functions
- Functions to describe instructions for our program, store them, and run them at one or more points in our program. They are great for re-usability!

### Functions as Values
- In JavaScript, functions are "first-class values", i.e. they can be stored in variables just like any other value

```js
//addTwo is a variable that holds a _reference_ to a function.
const addTwo = function(num) {
  return num + 2;
}
```

- They can also be passed around just like any other variables

```js
const addDeux = addTwo; //alias `addTwo` as `addDeux`.
console.log(addTwo);
console.log(addDeux);
addDeux(3); // equivalent to calling myFunction()
```

### Function calling vs function passing
- Note that the () is only used when invoking (or calling) a function

```js
const addDeux = addTwo; // assigning a function to variable "addDeux"
...
const result = addDeux(3); // invoke the function, and assign its return value to variable "result"
```

- And they can be passed to other functions as arguments

```js
const addTwo = function(num) {
    return num + 2;
}

const sumThenApplyCallback = function(nums, callback) {
    let sum = 0;
    for (const num of nums) {
        sum += num;
    }
    console.log("sum", sum);
    return callback(sum);
}

const result = sumThenApplyCallback([1, 2, 3], addTwo);
console.log("result", result);

> sum 6
> result 8
```

### Callbacks and Higher Order Functions
- A callback is a function that gets passed to another function to be executed by that function
- A **higher order function** is a function that:
  * accepts another function as an argument
  * returns a function (as a value)
- Why useful? Callbacks encapsulate reusable code that can be passed around, and so they keep (higher order) functions simple and for each function to have a "single responsibility"

```js
const myFunction = function() {
  // do something
}

const myHigherOrderFunction = function(callback) {
  callback(); // equivalent to myFunction()
}

myHigherOrderFunction(myFunction);
```


### Anonymous Functions
- Anonymous functions are simply functions that do not have a name
- Why? [Naming things is hard](https://martinfowler.com/bliki/TwoHardThings.html)

```js
const myHigherOrderFunction = function(callback) {
  callback();
}

// the function we pass as an argument has no name
myHigherOrderFunction(function() {});
```


### Arrow Functions
- Arrow functions are an alternative way to define functions

```js
// function declaration
function addTwo(num) {
    return num + 2;
}

// (classic) function expression
const addTwo = function(num) {
    return num + 2;
}

// arrow function
// We exclude the "function" keyword *before* the parentheses.
// We add an arrow *after* the parentheses.
const addTwo = (num) => {
    return num + 2;
}

// `()`s are optional for functions with exactly one parameter
const addTwo = num => {
    return num + 2;
}

// For one-line functions, the return statement becomes implicit
const addTwo = num => num + 2

// For functions with zero parameters, `()` is mandatory
const alwaysTrue = () => true;

// For functions with multiple parameters. `()` are also mandatory
const addTwoNumbers = (num1, num2) => num1 + num2;
```

- Arrow functions are often used as callbacks because they are shorter/cleaner to type

```js
arr.forEach(function(element) {});

// vs
arr.forEach((element) => {});
```

### Array iteration functions
There are a few useful and common array iteration functions:
 - filter
 - map
 - reduce
 - forEach

Filter returns a new array with only the values that pass a conditional check

```js
const evens = [1, 2, 3, 4].filter(num => num % 2 === 0);
console.log(evens); // > [2, 4]

const properNouns = ["Ian", "cat", "dog", "Taiwo"].filter(name => name[0].toUpperCase() === name[0]);
console.log(properNouns); // > ["Ian", "Taiwo"]
```

How could we implement our own version of `filter`?

```js
// accepts a callback that returns a boolean
const filter = function (array, callback) {
    const result = [];
    for (const item of array) {
        if (callback(item)) {
            result.push(item);
        } else {
            // DO NOTHING
        }
    }
    return result;
}
```


Map returns a new array containing all items, but with the specified function applied to each item

```js
const doubled = [1, 2, 3, 4].map(num => num * 2);
console.log(doubled);

const capitalized = ["ian", "dog", "cat", "Taiwo"].map(name => name[0].toUpperCase() + name.slice(1));
```

forEach is just like a `for ... of` loop, but the loop body is a callback:

```js
// To print each value
[1, 2, 3, 4].forEach(num => console.log(num));
```
- **NOTE**: I discourage the use of `forEach`, because it is quite error-prone. Here is a [good summary](https://www.designcise.com/web/tutorial/when-not-to-use-the-javascript-foreach-loop) of its problems, (we have not yet had covered enough material for the 4th reason, but just the first 3 are good-enough reasons to avoid `forEach`).

- Reduce is another loop iteration function, with which we can build all of the other functions. It is _powerful_ and _generic_, so a result it can be rather confusing. I recommend weighing the benefits of using it with readability.
```js
// Read the documentation of [Array.reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce), and guess what these lines do. Then try running it, and read the doc again.
[1, 2, 3].reduce((accum, num) => num + accum, 0);
[1, 2, 3].reduce((accum, num) => num > 1 ? accum.concat(num) : accum, [])
```

### Useful Links
* [Wikipedia: Callbacks](https://en.wikipedia.org/wiki/Callback_(computer_programming))
* [MDN: Callback Function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
* [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
* [When not to use the JavaScript forEach loop](https://www.designcise.com/web/tutorial/when-not-to-use-the-javascript-foreach-loop)
