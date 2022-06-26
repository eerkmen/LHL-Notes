
# Arrow Functions Revisited

Arrow functions in JavaScript are not just a shorthand for writing functions; they have specific features that are particularly useful in certain contexts.

## Arrow Functions Have No "this"

Unlike regular functions, arrow functions do not have their own `this`. The `this` value of the enclosing lexical context is used. For example:

```javascript
let group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],
  showList() {
    this.students.forEach(
      student => alert(this.title + ': ' + student)
    );
  }
};
group.showList();
```

Using arrow functions eliminates common errors associated with `this` in callbacks.

## Arrow Functions Can’t Run with "new"

Arrow functions cannot be used as constructors and cannot be called with `new`.

## Arrow Functions vs. Bind

There is a difference between an arrow function and a regular function called with `.bind(this)`:
- `.bind(this)` creates a bound version of the function.
- An arrow function does not create any binding. The `this` value is taken from the outer lexical context.

## Arrows Have No "arguments"

Arrow functions do not have an `arguments` object. This makes them suitable for decorators that need to forward calls with the current `this` and `arguments`.

## Summary

Arrow functions:
- Do not have `this`.
- Do not have `arguments`.
- Cannot be called with `new`.
- Do not have `super`.

These properties make arrow functions ideal for non-method functions, especially where they do not define their own context but operate within the current one.
