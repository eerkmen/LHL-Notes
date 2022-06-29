
# Summing with Recursion (Example)

## Introduction

Recursion is an alternative to traditional loops for solving problems. It involves a function calling itself with a modified input.

## The Exercise: Sum from n to 1

Objective: Write a function `sumToOne(n)` that returns the sum of every whole number from `n` down to 1.

## Base Case

The base case is the simplest instance of the problem:

```javascript
function sumToOne(n) {
  if (n === 1) {
    return 1;
  }
}
```

When `n` equals 1, the function returns 1, as there are no further numbers to add.

## Recursive Case

The recursive case handles larger values of `n`:

```javascript
function sumToOne(n) {
  if (n === 1) {
    return 1;
  }
  return n + sumToOne(n - 1);
}
console.log(sumToOne(4)); // Output: 10
```

This implements the concept: `sumToOne(n) = n + sumToOne(n-1)`.

## Conclusion

Recursion can be a powerful alternative to loops. While it's not always the best solution, it can simplify certain problems, especially those that are inherently recursive in nature.
