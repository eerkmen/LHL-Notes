
# Introduction to Modules

## Overview
This module introduces the concept of sharing functions between different files in Node.js, moving from a copy/paste approach to a more efficient method. This involves using modules to organize code and avoid redundancy.

## Key Concepts

1. **Every File in Node is a Module**: 
    - Each file run by Node is considered a separate module and has access to a module object.
    - Use `console.log(module);` to see details about the file.

2. **Exporting Modules**: 
    - JavaScript files must export parts that they want to be importable.
    - Typically, files export an object or a function.
    - Example: Assign a function `sayHelloTo` to `module.exports` in `myModule.js` to export it.

3. **Requiring a Module**: 
    - Use `require` to import a module.
    - Syntax: `const sayHelloTo = require('./myModule');`
    - The `.js` extension is optional and commonly omitted.

4. **Understanding Module Exports**: 
    - `module.exports` dictates what Node exports from a file, which defaults to `{}` (an empty object).
    - Importing a file that doesn't properly export its contents results in an empty object.

5. **Recursion and DRY Principle**: 
    - Modules help in writing DRY (Don't Repeat Yourself) code, avoiding redundant information with abstractions or data normalization.
    - DRY principle opposes WET (Write Everything Twice or We Enjoy Typing), which leads to redundancy.

6. **AHA Principle**: 
    - AHA (Avoid Hasty Abstractions) focuses on optimizing for change first.
    - It suggests abstracting only when necessary, avoiding premature optimization and overly rigid software.

## Conclusion
Understanding modules in Node.js is crucial for organizing code into individual files, making code more maintainable, and adhering to the DRY principle. This knowledge enables developers to create modular, efficient, and scalable applications.
