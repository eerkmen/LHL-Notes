
# Object Methods and -this-

This activity explores methods within objects in JavaScript and the usage of the keyword `this` in those methods. Experimentation with provided code examples is recommended for better understanding rather than just passive reading.

## Part 1: Functions As Object Properties (AKA Methods!)

Objects in JavaScript often contain properties that reference various data types. It's possible to include functions as properties within these objects, allowing these functions to act as methods.

Here's a traditional object representation in JavaScript:

```javascript
const person = {
  firstName: 'Bob',
  lastName: 'Smith'
}
```

If we want to greet the person, traditionally we might concatenate their first and last name:

```javascript
console.log('Hello, ' + person.firstName + ' ' + person.lastName);
```

However, we can simplify this by assigning a function to an object as a property, which then becomes a method:

```javascript
const person = {
  firstName: 'Bob',
  lastName: 'Smith',
  fullName: function() {
    return this.firstName + ' ' + this.lastName;
  }
}

console.log('Hello, ' + person.fullName());
```

By doing this, the `fullName` function becomes a method of `person`. The keyword `this` within the method refers to the object the method belongs to, in this case, `person`.

## Part 2: A Limited Introduction To -this-

The keyword `this` is a complex concept in JavaScript. It's used within methods to refer to the object the method is a part of. While the explanation here is simplified, it's enough for an introductory understanding. In the context of an object's method, `this` points to the object that houses the function.

## Part 3: But What Does -this- Really Mean?

The keyword `this` can be confusing and is a sophisticated aspect of JavaScript. For beginners, it is sufficient to understand `this` in the context of an object's method. For a more detailed explanation, it's advisable to refer to the MDN documentation, specifically the section titled "As an object method."

Remember, understanding the full scope of `this` comes with time and experience in JavaScript programming.
