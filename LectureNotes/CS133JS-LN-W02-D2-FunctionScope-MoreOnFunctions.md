---
title: Variable Scope
description: Learn the difference between local and global variables and what scope means. Learn about the scope of function parameters and variables declared within functions.
keywords: function, parameter, let, const, scope, global variable, local variable
material: Lecture Notes
generator: Typora
author: Brian Bird
---

<h1>Functions and Variable Scope</h1>

**CS133JS Beginning Programming: JavaScript**

| Topics by Week                                               |                    |
| ------------------------------------------------------------ | ------------------ |
| 1. Intro to JavaScript programming                           | 6. Arrays          |
| 2. Functions, <mark>Variable scope</mark>, Operators and Expressions | 7. Functions again |
| 3. Conditional Statements                                    | 8. Objects         |
| 4. Loops                                                     | 9. DOM             |
| 5. Midterm                                                   | 10. Final          |



<h2>Table of Contents</h2>

[TOC]

------

# Introduction

## Q and A

- How is part 1 of this week's lab assignment going?
- Does anyone have any general questions about anything?

## Anouncements

- Lab 1 "production" version is due Thursday.
- Lab 2 "beta" version is due Friday.
  - Post your draft version for part 2 even if it isn't complete.
- Lab 2 code review is due Tuesday.
  - Do code reviews in "round-robin" fashion:
    - Person A reviews person B's code.
    - Person B reviews person C's code.
    - Person C reviews person A's code.



------

# Review

- What is a function? 

- What is meant by “defining a function”? 
  - What are the names of the different parts of a function definition?

- What does it mean to "call" a function?
  - How do you do it?
  - What do you call the thing between the parenthesis in a function call?

- Do you have to write all the functions you use? 
- What are the names of the parts of the function below? 
  - How would you call it?


```javascript
 // This function returns the sum of three numbers
 function total(n1, n2, n3)
 {
  var t = n1 + n2 + n3;
  return t;
 }
```



------

# Variable Scope

*Scope* means the part of a program in which a variable is recognized by JavaScript. 

## Example

1. If you haven't already, put the function definition above into the console. Try using the variable `t` outside of the function. Is it valid there?

   ```
   console.log(t);
   ```

   Note that in the console it says the variable is "not defined", which is different from "undefined". Wow, don't you love this terminology?

   - *Not defined* means that JavaScript doesn't know about the variable in that scope.
   - *Undefined* means that no value has been assigned to the variable, but it is a valid variable in that scope.

2. Now try moving the definition of `t` outside the function.  Is the variable `t` valid both inside and outside the function now?

   ```javascript
    var t = 0;  // The variable is declared here.
    
    function total(n1, n2, n3)
    {
     t = n1 + n2 + n3;   // It is used here, but not declared--no var keyword is used.
     return t;
    }
   
   console.log(t);
   ```

## Local and Global Variables

- Variables declared with `var` inside a function are called *local variables*.

- Variables that are declared outside of any function are called *global variables*.

  Note: When using `var` to declare variables, local scope will only be for functions. Later, (see below) we'll use `let` to declare variables which will allow us to declare local variables in other blocks of code that we'll learn about in the coming weeks.

### Exercise

```javascript
console.log(t);    // is t valid here?
var t = 0;

function total(n1, n2, n3)
{
    t = n1 + n2 + n3;   // is t valid here?
    return t;
}
console.log(t);      // is t valid here?
```



## Function Parameters and Scope

- Function parameters are local variables in the body of the function

   ```javascript
function total(n1, n2, n3)  
{
    var t = n1 + n2 + n3; 
    return t;
}
var sum = total(2, 3, 6);
console.log(sum);
console.log(n1); // Are n1, n2 and n3 valid here?
   ```

- Variables defined outside the function, with the same name as the function parameters, are different variables. Below we have two sets of variables with the same names, but in different scopes:

   ```javascript
  function total(n1, n2, n3)  {
     var t = n1 + n2 + n3; 
     return t;
  }
   var n1 = 2, n2 = 3, n3 = 6;  // These are new variables!
   var sum = total(n1, n2, n3);
   console.log(sum);
  ```
  
  - How can we prove that n1, n2, and n3, which are defined outside the function, are not the same variables as n1, n2, and n3 defined as parameters of the function?
  
  ```javascript
  function total(n1, n2, n3)  
  {
       var t = n1 + n2 + n3; 
       n1 = 0;   // set this local variable to 0
       return t;
  }
  
  var n1 = 2, n2 = 3, n3 = 6;
  var sum = total(n1, n2, n3);
  console.log(sum);
  console.log(n1);  // will this be 0 or 2? Which n1 is this?
  ```
  
  

## Bad Things

### Undeclared Variables

Variables used without declaring them become *global variables*. But this is a bad way to do things because it is not obvious that this is a global variable.  This can lead to making mistakes in your code.

```javascript
function total(n1, n2, n3) 
{
   t = n1 + n2 + n3;         // t is not declared. This is bad practice.
   return t;
}
var sum = total(2, 3, 6);
console.log(sum);
console.log(t);           // t is a global variable.
```

#### Strict mode (a good thing)

You can set JavaScript to prevent you from using undeclared (not defined) variables by enabling strict mode with the statement, `"use strict"`. Here's an example:

```javascript
"use strict";
hobbit = "Frodo";   // This variable isn't declared, it will cause an error.
var home = "Bag End";
console.log(hobbit + " lives at " + home);
```

**Note:** in the browser console, you need to enter `"use strict";` and the code that follows it without hitting enter. Use shift-enter at the end of each line.

### Hoisting

Hoisting is a feature (quirk?) of JavaScript that lets you use a variable before you declare it with `var`. This is true whether your code is inside or outside a function. When you declare the variable anywhere in a given scope, it will be as if you had declared it at the top of the scope. Try this example:

```javascript
hobbit = "Frodo";
var home = "Bag End";
console.log(hobbit + " lives at " + home);
var hobbit;   // This variable declaration gets "hoisted" to the top of the code.
```

#### Declaring variables with `let` (another good thing)

You can use the keyword `let` to declare variables instead of `var`. This will prevent variable hoisting as well as do some other good thing we will discuss later. Try this example:

```javascript
hobbit = "Frodo";  // This will cause an error
let home = "Bag End";
console.log(hobbit + " lives at " + home);
let hobbit;   // The variable should have been declared before using  it.
```

#### Declaring variables with `const` (yet another good thing)

Variables declared with `const` cannot have a new value assigned to them and they have *block scope* (like variables declared with `let`).

Here's an example:

```javascript
const inchToCentemeter = 2.54;
let centemeters = inchToCentementer * 12; // this works
let inchToCentemeter = 2.54;  // Error, redeclaration is not allowed!
inchToCententemeter = 2.54;  // Error, assigning a new value is not allowed!

const centemeterToInch;       // Error, a const must have a value assigned to it!
centemeterToInch = 0.39370079;
```

##### Literal vs. named constants

There are actually two kinds of constants:

- *Literal constants* are just "hard coded" values like:

  ```javascript
  let length = 12 * 2.54;  // 2.54 is a literal constant
  let slogan = "Loki" + " for president";  // "Loki" is a literal constant
  ```

- *Named constants* are declared with `const` and are like variables in that they provide named storage for a value. For example:

  ```javascript
  const inchToCentemeter = 2.54;
  let length = 12 * inchToCentemeter; 
  
  const candidate = "Loki";
  let slogan = candidate + " for president";  
  ```

##### Why use constants?

- They prevent the coder from making a mistakes:

  -  You can't assign a value to a variable that should not be changed.
  - You can't forget to assign a value to a constant.

- They give you an alternative to "magic numbers" (literal constants).

  - If you use a literal constant in your code, someone else who looks at it may not be able to figure out what the number is for.

    ```javascript
    // Magic numbers
    let price = 10 * 1.25;
    // Meaning made clear with variable and constant names
    const usToCanadianDollar = 1.25;
    let costInUsDollar = 10;
    let price = costInUsDollar * usToCanadianDollar;
    ```

  - They give you a way to change a value in one place that is used in many places in your code.

  

------

# Reference

### W3Schools

[JavaScript Scope](https://www.w3schools.com/js/js_scope.asp)

[JavaScript Hoisting](https://www.w3schools.com/js/js_hoisting.asp)

[JavaScript Strict Mode](https://www.w3schools.com/js/js_strict.asp)

### Free Code Camp

[Var, Let, and Const – What's the Difference?](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference)



------

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/) Beginning JavaScript Lecture Notes by [Brian Bird](https://profbird.dev), written in 2018, revised in <time>2024</time>, are licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/). 

------------
   ```

   ```