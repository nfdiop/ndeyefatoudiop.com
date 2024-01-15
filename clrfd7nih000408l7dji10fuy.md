---
title: "5 ways to Shoot Yourself in the Foot With Typescript (And What To Do Instead) 🔫🦶🏽"
datePublished: Mon Jan 15 2024 20:15:53 GMT+0000 (Coordinated Universal Time)
cuid: clrfd7nih000408l7dji10fuy
slug: 5-ways-to-shoot-yourself-in-the-foot-with-typescript-and-what-to-do-instead
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705349610751/f8346122-a047-42ba-8f47-385fbe122392.jpeg
tags: typescript, frontend-development, beginners-learningtocode-100daysofcode

---

TypeScript is great!

But as a beginner, there are at least 5 ways you can shoot yourself in the foot and end up with a broken app.

Find them below, along with what to do instead 👇🏽:

## Mistake #1: Using any recklessly 😵

Using `any` inadequately removes all the benefits of Typescript. 

For example, `(a, b) => a+b` won't catch bad arguments passed at compile time.

Instead, use the correct types,  unknown, or a mixed codebase (Javascript + Typescript).

## Mistake #2: Having `strictNullChecks` turned off 🔕

When turned off,  the following code will compile fine but fail in production.

```
function printUser(user: User) {...}
const me = users.find((u) => u.name === "Ndeye Fatou Diop")  
printUser(user) <- This will throw in if an user is not found
```

So make sure to turn it on.

## Mistake #3: Having `noUncheckedIndexedAccess` turned off 🔇

When turned off, the following code will compile fine but fail in production.

```
const usersByLocation = {"France": [], "USA": []}
usersByLocation["India"].push(...) <- This will fail
print(usersByLocation["France"][0].name) <- This will fail
```

So please do yourself a favor and turn it on.

## Mistake #4: Using the exclamation mark inappropriately 🫢

Using `!`, the non-null assertion operator, is rarely a good idea. Your code won't catch when the value is undefined.

Instead, handle null/undefined values properly or rewrite your code.

## Mistake #5: Abusing of type assertion 🤥

Using `value as MyType` is tempting when your code doesn't compile. But, this is risky because the type assertion may be false at runtime, leading to potential UI crashes.

Instead, use this sparingly and only in the relevant context.

---

That's it 😁

Thank you for reading this post 🙏, I hope it will help you in your journey!

Leave a comment 📩 and add other mistakes you made with TypeScript.

If you like articles like this, join my **FREE** newsletter, **[FrontendJoy](https://ndeyefatoudiop.substack.com/)**, or find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).
