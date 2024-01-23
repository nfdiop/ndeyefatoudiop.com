---
title: "5 Small (Yet Easily Fixable) Mistakes Junior Frontend Developers Often Make in Their React Code"
datePublished: Tue Jan 23 2024 20:19:06 GMT+0000 (Coordinated Universal Time)
cuid: clrqsuln2000009ku6eke6bok
slug: 5-small-yet-easily-fixable-mistakes-junior-frontend-developers-often-make-in-their-react-code
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706041115879/61638de1-e07e-4179-92c3-fdfbd2f53423.jpeg
tags: mistakes, reactjs, beginners

---

I have reviewed more than 1,000 frontend pull requests.

Like many junior developers, I made some common mistakes when I started.

If you're in the same boat, here are 5 small mistakes you can quickly fix to keep your code clean and performant:

**Mistake #1: Creating a state that can be derived from props + existing state**
If you can calculate a value from existing props, please do so. Creating a state for it adds complexity and raises the risk of state discrepancies (some updated, others not).


![Mistake 1](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/we15n7lxxknam44x35j6.gif)

**Mistake #2: Using `items.length && <MyComponent />`**
Let's say you only want to render a component when a list is non-empty. So you use `items.length && <MyComponent />`. The problem is that this code will print 0 on the screen when the list is empty. Instead, use `items.length > 0 && <MyComponent />`.


![Mistake 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qrhpdbwtjutrff0trbpv.gif)

**Mistake #3: Using `useEffect` unnecessarily or dangerously**
Be cautious with the dependencies list in useEffect; if poorly set, it might crash your app or lead to inconsistent UI. Additionally, try to minimize its use for better performance and code readability.

![Mistake 3](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/knqg993gjbaul42b7wt5.gif)

**Mistake #4: Having multiple setState vs. combining into a single state or using reducer**
If you have multiple pieces of state that are strongly correlated, avoid multiple `setState`. Instead, opt for a single setState or use useReducer to maintain cleaner code.

![Mistake 4](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d68snusslrhgw9bl0i8b.gif)

**Mistake #5: Accidentally breaking the component memoization**
A common mistake, especially for beginners, is breaking the memoization created by `React.memo`. Make sure you don't accidentally pass objects or arrow functions, as it renders memoization ineffective.


![Mistake 5](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gyzbvs3wj8m1zgauha3h.gif)

---

Thank you for reading this post 🙏.

Leave a comment 📩 to share a mistake that you made as a junior developer.

And Don't forget to Drop a "💖🦄🔥".

If you like articles like this, join my **FREE** newsletter, **[FrontendJoy](https://frontendjoy.substack.com/)**, or find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).
