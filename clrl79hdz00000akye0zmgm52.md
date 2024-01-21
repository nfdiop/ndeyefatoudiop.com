---
title: "Why the Heck Do We Need Bundlers, Compilers, or Build Tools in Frontend Development? Unveiling 10 Reasons Why They Exist ✨"
datePublished: Fri Jan 19 2024 22:15:58 GMT+0000 (Coordinated Universal Time)
cuid: clrl79hdz00000akye0zmgm52
slug: why-the-heck-do-we-need-bundlers-compilers-or-build-tools-in-frontend-development-unveiling-10-reasons-why-they-exist
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705702780937/41d81feb-9768-4774-904a-8a440642b06e.jpeg
tags: webpack, beginners, frontend-development, bundler, vite

---

As a junior dev, bundlers (e.g., Webpack), compilers (e.g., Babel), and build tools (e.g., Vite) were confusing as hell. 

I couldn't understand why I needed them and why my React code wouldn't just run on the browser 😬.

Sounds familiar? Here are 10 concrete reasons why these tools are essential:

## Reason #1: The browser can't understand the code you write

Your browser speaks HTML, CSS, and JS. If you're using JSX (React), TS files, or Vue files, you need a tool to convert that into browser-friendly code.

## Reason #2: It could take ages for your website to load if you have multiple files

For complex projects, splitting code into different files is great for organization but not for user experience since the more files the user has to download, the slower your website. Bundlers step in to bundle those files together for quicker download.

## Reason #3: You probably want to optimize your code before shipping it to users

The code you write should be neat with comments and clear variables. But those don't need to be shipped to the end-user. Why? More code means a slower download for users. Tools like Webpack can help. They shrink your code before it goes live, making it faster to load on the browser.

## Reason #4: You want to iterate faster on your code and see changes instantly

Picture this: you're tackling a big project, and each time you make some changes, you need to go back and refresh the page. Not ideal, huh? But with tools like Vite, Snowpack, Webpack, etc., you can fire up a server. It monitors file changes, automatically updating what you see in the browser. Much smoother, right?

## Reason #5: You want to use cool new language features, but your code still needs to run on old browsers

I wish everyone had the newest Chrome or ditched Internet Explorer, but the reality is that some still use different browsers. Your code has to work on those. Good news: you can still use the latest JavaScript. How? Transpilers like Babel come to the rescue, converting your code into something older browsers can handle.

## Reason #6: You have some unused code that you probably don't need to ship to the user

Let's be real. We all have code that we wrote ages ago and never touched. Luckily, tools like Webpack completely ignore that unused code when they create the files sent to users. That means a faster website for you.

## Reason #7: You have some code that you want users to load conditionally

Imagine this: you've got a hefty component, but not every user needs it. Think of exclusive features for premium users. Those who won't use these features don't have to load that code. You can already do this with Vanilla JS but these tools make it handy thanks to lazy-loading and code splitting.

## Reason #8: You want to import non-JS assets inside your Javascript file

Sadly, browsers won't let you import some files directly into your JavaScript. Fortunately, these tools come to the rescue. They allow you to define how to load specific files using loaders.

## Reason #9: You want to use great tools (e.g., Typescript) without the hassle of having to configure it

Tech like React, TypeScript, Vue, etc., is fantastic but can be a headache to set up. Enter tools like Vite. They let you swiftly create a working environment with zero setup fuss. Easy peasy!

## Reason #10: You want your code to work in different environments

Want your code to run on different environments (e.g., Node and Web)? Use tools like Webpack ;)

---

Thank you for reading this post 🙏.

Leave a comment 📩 if you use these tools for other reasons!

And Don't forget to Drop a "💖🦄🔥".

If you like articles like this, join my **FREE** newsletter, **[FrontendJoy](https://ndeyefatoudiop.substack.com/)**, or find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).