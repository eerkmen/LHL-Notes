
# More Recursion

Recursion can often be a more elegant solution to problems that are hard to solve using loops. This exercise focuses on a case where recursion is more favorable than looping.

## Assignment: Print Out All Elements in an Array

The task is to build a function that prints out all items in an array, including items in nested arrays.

## Recursive Function Implementation

Here's the implementation of the recursive function:

```javascript
const printItems = function(array) {
  array.forEach((item) => {
    if (Array.isArray(item)) {
      printItems(item); // Recursive call
    } else {
      console.log(item);
    }
  });
}
```

This function checks if an item is an array. If it is, it calls itself with that array, otherwise, it prints the item.

## Understanding Recursion in the Function

- **Recursive Case**: When the item is an array.
- **Base Case**: When the item is not an array.

## Example

```javascript
const array = ["A", [["B", ["C"]], [[["D"]], "E"]]];
printItems(array);
```

The function efficiently handles multiple levels of nested arrays.

## Iteration vs. Recursion

An iterative solution would involve more complex and less readable code compared to the recursive solution presented here.

## Conclusion

Recursion simplifies solutions for problems with multiple nested structures. It's a valuable skill for developers, especially for problems that are instances of a smaller problem.
