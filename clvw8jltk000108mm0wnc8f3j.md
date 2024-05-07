---
title: "🎉🌿 Back to Frontend Fundamentals: JavaScript Building Blocks"
datePublished: Tue May 07 2024 10:16:07 GMT+0000 (Coordinated Universal Time)
cuid: clvw8jltk000108mm0wnc8f3j
slug: back-to-frontend-fundamentals-javascript-building-blocks
canonical: https://dev.to/_ndeyefatoudiop/back-to-frontend-fundamentals-javascript-building-blocks-315a
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715076921920/a7ae4691-cf24-4d5d-afd8-f62fc7646826.avif
tags: javascript, building-blocks

---

For a long time, I struggled to learn React.

Because I didn't master JavaScript.

So, I am starting this series to help any JavaScript newbie master the language.

Ready? Let's go 💪.

## Anatomy of a JavaScript (JS) program

Let's look at the following JavaScript program 👇:

```javascript
let selectedPost = undefined;
const id = Symbol("id");
const me = {
  name: "Ndeye Fatou Diop",
  age: 30,
  [id]: "123",
  hasPremium: true,
  rank: 25n,   
  linkedAccounts: null,
};
const greet = (user) => console.log(`Hello ${user.name}.  
You are ${user.age > 18 ? "major" : "minor"}.`);
greet(me); // prints "Hello Ndeye Fatou Diop. You are major."
```

Every JavaScript program is a series of `commands` for the machine (browser, computer, etc.). In the example above, the browser will print `Hello Ndeye Fatou Diop. You are major.` on the console when the program is done executing.

---

Every program is made out of:

### 1\. Comments

Comments help you document your code or note things to do later. They are ignored by the browser or machine and are only for humans.

There are 2 kinds of comments:

**1\. Inline comments**

```javascript
// This is a single-line comment
```

**2\. Multiline comments**

```javascript
/*
This is a comment
that spans multiple lines.
Great for making long code clear!
*/
```

> Even though computers ignore comments, some editors, like VSCode, can use special comments like [JSDoc](https://jsdoc.app/) to give helpful hints.

### 2\. Values

A value is any information the code uses, such as numbers or text. Each value has a type or **data type** that tells JavaScript what it is.

Our code has different values: `undefined,` `30`, `true`,…

```javascript
undefined // "undefined" type
"Ndeye Fatou Diop" // "string" type
Symbol("id") -> "symbol" type
me // "object" type
30 // "number" type
true // "boolean" type
25n // "bigint" type
null // "null" type
greet // "object" type
```

> 💡 You can find a value's type using `typeof value`, except for `null`, which gives "object". Watch out for that! 😅

### 3\. Variables

Variables are like labels for values. They let us work with data by giving it a name.

Think of variables as people's names. You're not your name, and when I say things like `${PutYourNameHere} is late` or `Give 100$ to ${PutYourNameHere}`, I am referring to you.

There are **3** ways to declare a variable in JavaScript:

* Using `let`: Variables declared with `let` can be re-assigned (i.e., we can change the value stored in the variable)
    

```javascript
let name = "Ndeye Fatou Diop";
name = "Joyancefa"; // We can change the value stored in `name`
```

* Using `const`: Variables declared with `const` cannot be re-assigned (i.e., once the variable is assigned, we cannot re-assign it)
    

```javascript
const name = "Ndeye Fatou Diop";
name = "Joyancefa"; // ❌ Uncaught TypeError: Assignment to constant variable.
```

* Using `var`: **Don't use this!** It's the old way of declaring variables and has some unexpected behaviors. Variables declared with `var` can also be re-assigned.
    

```javascript
var name = "Ndeye Fatou Diop";
name = "Joyancefa"; // We can change the value stored in `name`
```

> 💡 When a value is written as is in the code without any changes, it's called a **literal**. In the example above, `30`, `"Ndeye Fatou Diop"`,… are literals.

### 4\. Operators

Operators let you perform operations on one or more values. There are different types of operators:

* **unary** operators: `typeof`, `!`, `--`, ...
    
* **binary** operator: `=`, `+`, `%`,...
    
* **ternary** operator: `condition? exprIfTrue : exprIfFalse`
    
* etc.
    

In the example above, we use the ternary operator to print the correct string when greeting me.

```javascript
console.log(`Hello ${user.name}. You are ${user.age > 18 ? "major" : "minor"}.`
```

> 💡 You can learn more about operators [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_operators).

### 5\. Keywords

Keywords are special words with specific meanings in JavaScript. For example, `for` is a keyword used in loops.

Some are `reserved` (i.e., you can't use them for variable names).

Even if not all of them are (e.g., `undefined`), avoiding using keyword names for variables is good practice.

> 💡 You can find the list of reserved keywords more [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#reserved_words). **No need to remember them all!** Your code editor should yell at you when you use them.

### 6\. Expressions

Expressions are bits of code that produce a value. Any expression can be formed of operators, values, variables, expressions, etc.

In the example above, we have the following expressions:

* All the values mentioned earlier (`30`, `true`,…)
    
* All the assignments (`selectedPost = undefined`, `id = Symbol("id")`,…)
    
* [`user.name`](http://user.name)
    
* `${user.age > 18 ? "major" : "minor"}`
    
* `Hello ${`[`user.name`](http://user.name)`}. You are ${user.age > 18 ? "major" : "minor"`
    
* `(user) => console.log(...)`
    
* `greet(me)`
    

> 💡 Check this great [post](https://www.joshwcomeau.com/javascript/statements-vs-expressions/) to learn more about the distinction between `statements` and `expressions`.

### 7\. Statements

A statement is just a collection of expressions. It is an instruction to the program to do something like an order in the real world.

***Examples***

```javascript
return
for...of
if...else
do...while
```

Statements cannot be used when expressions are expected (for example, in function arguments).

**❌ Bad:** This won't work

```javascript
function greet(name) {
    return "Hello, " + name;
}

// Incorrect usage: Using a statement (if) instead of an expression
greet(if (Math.random() < 0.5) { "John" } else { "Jane" }); // Uncaught SyntaxError: Unexpected token 'if'
```

The example above has **5** statements (one on each line).

> 💡 The semi-colon after the statement is optional, but I prefer to add it to keep the code clear. Find the complete list of statements [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements).

---

Thank you for reading this post 🙏.

Leave a comment 📩 to share what you think.

And Don't forget to Drop a "💖🦄🔥".

If you like articles like this, join my **FREE** newsletter, [**FrontendJoy**](https://frontendjoy.substack.com/).

If you want daily tips, find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).