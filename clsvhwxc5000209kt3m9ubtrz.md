---
title: "5 Small (Yet Easily Fixable) Mistakes Junior Frontend Developers Make With React Memoization"
datePublished: Wed Feb 21 2024 07:51:32 GMT+0000 (Coordinated Universal Time)
cuid: clsvhwxc5000209kt3m9ubtrz
slug: 5-small-yet-easily-fixable-mistakes-junior-frontend-developers-make-with-react-memoization
canonical: https://dev.to/_ndeyefatoudiop/5-small-yet-easily-fixable-mistakes-junior-frontend-developers-make-with-react-memoization-h7l
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708501857511/8409eefc-b5fb-47d3-b16f-1a57bf38e34a.jpeg
tags: mistakes, reactjs, beginners, memoization, frontend-development

---

I have reviewed more than 1,000 frontend pull requests.

Like many junior developers, I made some common mistakes when I started, **especially regarding memoization**.

If you're in the same boat, here are 5 small mistakes you can quickly fix to do memoization properly in React.

> 💡 Tip: You can catch a lot of these issues with ESLint and plugins like [eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks).

---

### Mistake #1: Forgetting to include the dependencies list for `useMemo` or `useCallback`

If you're using `useMemo` or `useCallback` without depending on any component props or state, move the value returned by `useMemo` or the function returned by `useCallback` outside the component.

Otherwise, make sure to specify the dependency array as a second argument. Without it, the `useMemo` or `useCallback` function will run every time your component renders.

![Mistake 1](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/74poosfywdybf5onpx3a.gif)

### Mistake #2: Adding dependencies that change on every render to your `useMemo` or `useCallback` hook

When a dependency changes, the function in `useMemo` or `useCallback` reruns. So, if a dependency changes with each render, memoization becomes useless.

This issue commonly arises when:

* Non-memoized arrays/objects created inside the component are used as dependencies
    
* Non-memoized arrays/objects props are used as dependencies

So make sure you always memoize the passed dependencies, pass fewer ones, etc.


![Mistake 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f650ft17n58q9rafx2td.gif)

### Mistake #3: Using `useMemo` or `useCallback` unnecessarily

You only need useMemo and useCallback in the following cases:

* If you're computing a value that is not a symbol or an object (e.g., string, number, etc.), only use `useMemo` if the computation is expensive.
    
* If you're returning a function, use `useCallback` only when the function is a prop of a component that is wrapped in `memo` or if the function will be in the dependency list of a hook (for example, `useEffect`)
    
* If you're returning a symbol or an object, only use `useMemo` when the value returned is a prop of a component that is wrapped in `memo` or if the function will be in the dependency list of a hook (for example, `useEffect`)
    
* Else, you don't need them.
    

![Mistake 3](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c1jbcwhcnujsocg6x2zb.gif)

### Mistake #4: Using `memo(Component)` with props that change on every render

[memo](https://react.dev/reference/react/memo) helps avoid re-rendering a component when its props remain unchanged. However, if the props keep changing, memoization becomes useless.

Unfortunately, this can happen (without you realizing) when:

* You pass an empty array or object to your component.
    
* You pass callbacks that are not wrapped in `useCallback` or non-primitive values that are not wrapped in `useMemo`
    
* …
    

So make sure the props don't change on every render. If that is not possible, drop the memo.


![Mistake 4](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ltmfpckapvcdk8s68suf.gif)

### Mistake #5: Using `memo(Component)` for a component that will render anyway if its parent renders

There are situations where using `memo` isn't necessary because the component will automatically re-render when its parent renders:

* **Situation #1:** If your memoized component takes children created within the parent, and these children change with every render, your component will also re-render since `children` is just another prop.
    
* **Situation #2:** If your component depends on a context value that changes when its parent renders, it will also re-render, even if the specific changed part of the context isn't utilized in your component.


![Mistake 5](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lv5b7dj2gtye022r3l9h.gif)

---

Thank you for reading this post 🙏.

Leave a comment 📩 to share a mistake you made and how you overcame it.

And Don't forget to Drop a "💖🦄🔥".

If you like articles like this, join my **FREE** newsletter, **[FrontendJoy](https://frontendjoy.substack.com/)**, or find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).