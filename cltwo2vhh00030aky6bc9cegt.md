---
title: "React: 5 Small (Yet Easily Fixable) Mistakes Junior Frontend Developers Make With React Refs"
datePublished: Mon Mar 18 2024 08:11:36 GMT+0000 (Coordinated Universal Time)
cuid: cltwo2vhh00030aky6bc9cegt
slug: react-5-small-yet-easily-fixable-mistakes-junior-frontend-developers-make-with-react-refs
canonical: https://dev.to/_ndeyefatoudiop/react-5-small-yet-easily-fixable-mistakes-junior-frontend-developers-make-with-react-refs-3in8
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1710749437785/d84ae2c1-c760-4307-833e-2794ddbc4ea7.jpeg
tags: mistakes, reactjs, junior-developer, refs, refs-in-react

---

I have reviewed more than 1,000 front-end pull requests.

Like many junior developers, I made some common mistakes when I started, **especially while using refs.**

If you're in the same boat, here are 6 small mistakes you can quickly fix to use refs properly in React:

## Mistake #1: Using a state when a ref would be a better choice

One common mistake is having a state when a ref would have been more suitable. Since ref updates don't trigger re-renders, they are perfect for situations where this behavior is unnecessary, resulting in better app performance.

![Mistake 1](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kkdaqk5s2uvmi0tljo2n.gif)

----

**❌ Bad:** We are storing the interval in the state and triggering an unnecessary re-render.
```jsx
import { useState, useEffect } from "react";

function Timer() {
  const [time, setTime] = useState(new Date());
  const [intervalId, setIntervalId] = useState();

  useEffect(() => {
    const intervalId = setInterval(() => {
      setTime(new Date());
    }, 1_000);
    // Storing the id in the state
    setIntervalId(intervalId);
    return () => {
      clearInterval(intervalId);
    };
  }, []);

  const stopTimer = () => {
    intervalId && clearInterval(intervalId);
  };

  return (
    <>
      <h2>Current time: {time.toLocaleTimeString()}</h2>
      <button onClick={stopTimer}>Stop timer</button>
    </>
  );
}
```

**✅ Good:** We store the interval ID in a ref and don't trigger a re-render.
```jsx
import { useState, useEffect, useRef } from "react";

function Timer() {
  const [time, setTime] = useState(new Date());
  const intervalRef = useRef(undefined);

  useEffect(() => {
    const intervalId = setInterval(() => {
      setTime(new Date());
    }, 1_000);
    intervalRef.current = intervalId;
    return () => {
      clearInterval(intervalId);
    };
  }, []);

  const stopTimer = () => {
    intervalRef.current && clearInterval(intervalRef.current);
  };

  return (
    <>
      <h2>Current time: {time.toLocaleTimeString()}</h2>
      <button onClick={stopTimer}>Stop timer</button>
    </>
  );
}
```

## Mistake #2: Using `ref.current` vs. ref or using the ref value before it's set

When I first began, I often stumbled upon this mistake. I would use `ref.current` before the value was actually passed, or I'd pass `ref.current` to my functions instead of using ref directly. In the latter case, the ref object would change while I still held onto the value stored in `ref.current`.

![Mistake 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b1jl7k9gmdn8uw8f99st.gif)

----

**❌ Bad:** The code below won't work since `ref.current` is null initially. As a result, when the effect runs, `element` is null. 
```jsx
import { useState, useEffect, useRef } from "react";

export default function App() {
  const ref = useRef();
  const isHovered = useIsHovered(ref.current);

  return (
    <div ref={ref}>
      Hovered: {`${isHovered}`}
    </div>
  );
}

function useIsHovered(element) {
  const [isHovered, setIsHovered] = useState(false);
  useEffect(() => {
    if (element == null) {
      return;
    }
    const onMouseEnter = () => setIsHovered(true);
    const onMouseLeave = () => setIsHovered(false);
    element.addEventListener("mouseenter", onMouseEnter);
    element.addEventListener("mouseleave", onMouseLeave);
    return () => {
      element.removeEventListener("mouseenter", onMouseEnter);
      element.removeEventListener("mouseleave", onMouseLeave);
    };
  }, [element]);
  return isHovered;
}
```

**✅ Good:** The code below will work because since we pass `ref` to `useIsHovered`, when the effect runs `ref.current` is well defined.
```jsx
import { useState, useEffect, useRef } from "react";

export default function App() {
  const ref = useRef();
  const isHovered = useIsHovered(ref);

  return (
    <div ref={ref}>
      Hovered: {`${isHovered}`}
    </div>
  );
}

function useIsHovered(ref) {
  const [isHovered, setIsHovered] = useState(false);
  useEffect(() => {
    if (ref.current == null) {
      return;
    }
    const onMouseEnter = () => setIsHovered(true);
    const onMouseLeave = () => setIsHovered(false);
    ref.current.addEventListener("mouseenter", onMouseEnter);
    ref.current.addEventListener("mouseleave", onMouseLeave);
    return () => {
      ref.current.removeEventListener("mouseenter", onMouseEnter);
      ref.current.removeEventListener("mouseleave", onMouseLeave);
    };
  }, [ref]);
  return isHovered;
}
```

### Mistake #3: Forgetting to use `forwardRef`

We probably all made this mistake. In fact, React doesn't let you pass a ref to a function component unless it's wrapped with `forwardRef`. **The fix?** Simply wrap the component receiving the ref in forwardRef or use another name for your ref prop.

![Mistake 3](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yxslbiinywt1hu2bwlf0.gif)

----

**❌ Bad:** The code below won't work since function components like `NameInput` cannot be given refs this way.
```jsx
import { useState, useEffect, useRef } from "react";

export default function App() {
  const ref = useRef();
  useEffect(function autoFocusInput() {
    ref.current?.focus();
  }, []);

  return (
    <>
      Name: <NameInput ref={ref} />
    </>
  );
}

const NameInput = ({ ref }) => {
  const [value, setValue] = useState("");
  return (
    <input ref={ref} value={value} onChange={(e) => setValue(e.target.value)} />
  );
};
```

**✅ Good:** We wrap the component in `forwardRef`.
```jsx
import { useState, useEffect, useRef, forwardRef } from "react";

export default function App() {
  const ref = useRef();
  useEffect(function autoFocusInput() {
    ref.current?.focus();
  }, []);

  return (
    <>
      Name: <NameInput ref={ref} />
    </>
  );
}

const NameInput = forwardRef((_, ref) => {
  const [value, setValue] = useState("");
  return (
    <input ref={ref} value={value} onChange={(e) => setValue(e.target.value)} />
  );

```

### Mistake #4: Initializing the ref with an "expensive" function call

When you call a function to set the ref initial value, that function will be called with each render. If the function is expensive, this will unnecessarily affect your app performance. **The solution?** Memoize the function or initialize the ref during render (after checking that values are not set yet).

![Mistake 4](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h0xb9axav8jlxt7k2qx4.gif)

----

**❌ Bad:** We will read from the local storage whenever this component re-renders (e.g., when `inputValue` changes).

```jsx
import { useState, useRef, useEffect } from "react";

export default function App() {
  const ref = useRef(window.localStorage.getItem("demo-last-connected"));
  const [inputValue, setInputValue] = useState("");

  useOnBeforeUnload(() => {
    const date = new Date().toUTCString();
    console.log("Date", date);
    window.localStorage.setItem("demo-last-connected", date);
  });

  return (
    <>
      <div>
        Last connected: <strong>{ref.current}</strong>
      </div>
      Name:{" "}
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
    </>
  );
}

function useOnBeforeUnload(callback) {
  useEffect(() => {
    window.addEventListener("beforeunload", callback);
    return () => window.removeEventListener("beforeunload", callback);
  }, [callback]);
}
```

**✅ Good:** We read from the local storage once.

```jsx
export default function App() {
  const ref = useRef(null);
  if (ref.current === null) {
    ref.current = window.localStorage.getItem("demo-last-connected");
  }
  const [inputValue, setInputValue] = useState("");

  useOnBeforeUnload(() => {
    const date = new Date().toUTCString();
    console.log("Date", date);
    window.localStorage.setItem("demo-last-connected", date);
  });

  return (
    <>
      <div>
        Last connected: <strong>{ref.current}</strong>
      </div>
      Name:{" "}
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
    </>
  );
}
/// ...
```

### Mistake #5: Using a ref callback function that changes on every render

Ref callbacks can tidy up your code. However, be aware that React calls the ref callback whenever it changes. This means that when a component re-renders, the previous function is called with null as the argument, while the next function is called with the DOM node. This can lead to some unwanted flickering in the UI. The solution? Make sure to memoize the ref callback function.

![Mistake 5](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lupk2p9ccy9ni32ecunc.gif)

---
**❌ Bad:** The code below won't work properly because whenever `inputValue` or `currentTime` changes, the ref callback function will run again, and the input will be focused again.

```jsx
import { useEffect, useState } from "react";

export default function App() {
  const ref = (node) => {
    node?.focus();
  };
  const [nameValue, setNameValue] = useState("");
  const currentTime = useCurrentTime();

  return (
    <>
      <h2>Time {currentTime}</h2>
      <label htmlFor="name">Name: </label>
      <input
        id="name"
        ref={ref}
        value={nameValue}
        onChange={(e) => setNameValue(e.target.value)}
      />
    </>
  );
}

function useCurrentTime() {
  const [time, setTime] = useState(new Date());
  useEffect(() => {
    const intervalId = setInterval(() => {
      setTime(new Date());
    }, 1_000);
    return () => clearInterval(intervalId);
  });
  return time.toString();
}
```

**✅ Good:** The function is wrapped in `useCallback`, and the input will be focused only when it is mounted.

```jsx
export default function App() {
  // Wrap in useCallback
  const ref = useCallback((node) => {
    node?.focus();
  }, []);
  const [nameValue, setNameValue] = useState("");
  const currentTime = useCurrentTime();

  return (
    <>
      <h2>Time {currentTime}</h2>
      <label htmlFor="name">Name: </label>
      <input
        id="name"
        ref={ref}
        value={nameValue}
        onChange={(e) => setNameValue(e.target.value)}
      />
    </>
  );
}
```
---

Thank you for reading this post 🙏.

Leave a comment 📩 to share a mistake you made with refs and how you overcame it.

And Don't forget to Drop a "💖🦄🔥".

If you like articles like this, join my **FREE** newsletter, **[FrontendJoy](https://frontendjoy.substack.com/)**, or find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).