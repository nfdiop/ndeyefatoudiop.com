---
title: "5 Dead-Simple Tips to Make Your Code More Readable as a Junior Frontend Developer (And Get Your Pull Requests Approved Faster) 🎉"
datePublished: Mon Jan 22 2024 22:34:35 GMT+0000 (Coordinated Universal Time)
cuid: clrpi8zj0000109jq0q30hqtt
slug: 5-dead-simple-tips-to-make-your-code-more-readable-as-a-junior-frontend-developer-and-get-your-pull-requests-approved-faster
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705962854015/06e274cf-8f3d-433e-91e8-33284d6b89c6.jpeg
tags: beginners, frontend-development, clean-code, junior-developer

---

I have reviewed more than **1,000 frontend pull requests**.

I've noticed some common comments made to junior developers, which I also got when starting out. Dealing with these comments often makes the review process longer.

So, if you're a junior developer just starting:

## Here are 5 simple tips to make your code more readable.

**1. Save meaningful or shared values as constants. **If a value is significant in your code or used in multiple places, store it in a constant variable in ALL UPPERCASE. This helps give the variable context and makes it easier to locate or modify.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jn4gx832juezjh3frinm.gif)

**2. Include the unit in variable names when relevant.** You saved your meaningful variables in constants. Great 🎉. Now, make sure to specify the unit if applicable. For example, instead of naming a variable `DEBOUNCE_TIME`, use `DEBOUNCE_TIME_MS` to make it more straightforward.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r7f4dvtnhqljks2w4b44.gif)

**3. Use the JavaScript numeric separator when possible.** Improve the readability of numbers like `1000` or `20000` by writing them as `1_000` or `20_000`.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d9vlfsw5i1xiy1qroz5n.gif)

**4. Return as early as possible in functions.** Whenever feasible, exit a function early to enhance the code clarity. This will drastically increase the code readability.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ihhoqgk4uf0t84ugllrm.gif)

**5. Avoid negative conditionals.** Instead of naming a variable isWebsiteNotAccessible, name it isWebsiteAccessible and use !isWebsiteAccessible. Positive conditions are generally easier to understand.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fusjvci6b10ico65uwxu.gif)

---

Thank you for reading this post 🙏.

Leave a comment 📩 to share a tip.

And Don't forget to Drop a "💖🦄🔥".

If you like articles like this, join my **FREE** newsletter, **[FrontendJoy](https://frontendjoy.substack.com/)**, or find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).