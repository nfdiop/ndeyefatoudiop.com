---
title: "101 React Tips & Tricks For Beginners To Experts ✨"
datePublished: Thu Aug 08 2024 06:32:20 GMT+0000 (Coordinated Universal Time)
cuid: clzkwi177001w0al642bybfot
slug: 101-react-tips-tricks-for-beginners-to-experts
canonical: https://dev.to/_ndeyefatoudiop/101-react-tips-tricks-for-beginners-to-experts-4m11
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723098619980/9d8d487a-ba6a-4543-83cd-534d513bbd26.jpeg
tags: javascript, webdev, tips, reactjs

---

I have been working professionally with React for the past **+5 years**.

In this article, I share the 101 best tips & tricks I learned over the years.

Ready? Let's dive in 💪!

> ℹ️ *Notes:*
> 
> * This guide assumes a basic familiarity with React and an understanding of the terms `props`, `state`, `context`, etc.
>     
> * I tried to use Vanilla JS for most of the examples to keep things simple. If you're using TypeScript, you can easily adapt the code.
>     
> * The code is not production-ready. Use at your own discretion.
>     

---

## Category #1: Components organization 🧹

---

### 1\. Use self-closing tags to keep your code compact

```jsx
// ❌ Bad: too verbose
<MyComponent></MyComponent>

// ✅ Good
<MyComponent/>
```

---

### 2\. Prefer `fragments` over DOM nodes (e.g., div, span, etc.) to group elements

In React, each component must return a single element. Instead of wrapping multiple elements in a `<div>` or `<span>`, use `<Fragment>` to keep your [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) neat and tidy.

**❌ Bad:** Using `div` clutters your DOM and may require more CSS code.

```jsx
function Dashboard() {
  return (
    <div>
      <Header />
      <Main />
    </div>
  );
}
```

**✅ Good:**`<Fragment>` wraps elements without affecting the DOM structure.

```jsx
function Dashboard() {
  return (
    <Fragment>
      <Header />
      <Main />
    </Fragment>
  );
}
```

---

### 3\. Use React fragment shorthand `<></>` (except if you need to set a key)

**❌ Bad:** The code below is unnecessarily verbose.

```jsx
<Fragment>
   <FirstChild />
   <SecondChild />
</Fragment>
```

**✅ Good:** Unless, you need a `key`, `<></>` is more concise.

```jsx
<>
   <FirstChild />
   <SecondChild />
</>

// Using a `Fragment` here is required because of the key.
function List({ users }) {
  return (
    <div>
      {users.map((user) => (
        <Fragment key={user.id}>
          <span>{user.name}</span>
          <span>{user.occupation}</span>
        </Fragment>
      ))}
    </div>
  );
}
```

---

### 4\. Prefer spreading props over accessing each one individually

**❌ Bad:** The code below is harder to read (especially at scale).

```jsx
// We do `props…` all over the code.
function TodoList(props) {
  return (
    <div>
      {props.todos.map((todo) => (
        <div key={todo}>
          <button
            onClick={() => props.onSelectTodo(todo)}
            style={{
              backgroundColor: todo === props.selectedTodo ? "gold" : undefined,
            }}
          >
            <span>{todo}</span>
          </button>
        </div>
      ))}
    </div>
  );
}
```

**✅ Good:** The code below is more concise.

```jsx
function TodoList({ todos, selectedTodo, onSelectTodo }) {
  return (
    <div>
      {todos.map((todo) => (
        <div key={todo}>
          <button
            onClick={() => onSelectTodo(todo)}
            style={{
              backgroundColor: todo === selectedTodo ? "gold" : undefined,
            }}
          >
            <span>{todo}</span>
          </button>
        </div>
      ))}
    </div>
  );
}
```

---

### 5\. When setting default values for props, do it while destructuring them

**❌ Bad**: You may need to define the defaults in multiple places and introduce new variables.

```jsx
function Button({ onClick, text, small, colorScheme }) {
  let scheme = colorScheme || "light";
  let isSmall = small || false;
  return (
    <button
      onClick={onClick}
      style={{
        color: scheme === "dark" ? "white" : "black",
        fontSize: isSmall ? "12px" : "16px",
      }}
    >
      {text ?? "Click here"}
    </button>
  );
}
```

**✅ Good**: You can set all your defaults in one place at the top. This makes it easy for someone to locate them.

```jsx
function Button({
  onClick,
  text = "Click here",
  small = false,
  colorScheme = "light",
}) {
  return (
    <button
      onClick={onClick}
      style={{
        color: colorScheme === "dark" ? "white" : "black",
        fontSize: small ? "12px" : "16px",
      }}
    >
      {text}
    </button>
  );
}
```

---

### 6\. Drop curly braces when passing `string` type props.

```jsx
// ❌ Bad: curly braces are not needed
<Button text={"Click me"} colorScheme={"dark"} />

// ✅ Good
<Button text="Click me" colorScheme="dark" />
```

---

### 7\. Ensure that `value` is a boolean before using `value && <Component {...props}/>` to prevent displaying unexpected values on the screen.

**❌ Bad:** When the list is empty, `0` will be printed on the screen.

```jsx
export function ListWrapper({ items, selectedItem, setSelectedItem }) {
  return (
    <div className="list">
      {items.length && ( // `0` if the list is empty
        <List
          items={items}
          onSelectItem={setSelectedItem}
          selectedItem={selectedItem}
        />
      )}
    </div>
  );
}
```

✅ **Good**: Nothing will be printed on the screen when there are no items.

```jsx
export function ListWrapper({ items, selectedItem, setSelectedItem }) {
  return (
    <div className="list">
      {items.length > 0 && (
        <List
          items={items}
          onSelectItem={setSelectedItem}
          selectedItem={selectedItem}
        />
      )}
    </div>
  );
}
```

---

### 8\. Use IIFE (Immediately Invoked Function Expression) to keep your code clean and avoid lingering variables

❌ **Bad**: The variables `gradeSum` and `gradeCount` are cluttering the component's scope.

```jsx
function Grade() {
  let gradeSum = 0;
  let gradeCount = 0;
  grades.forEach((grade) => {
    gradeCount++;
    gradeSum += grade;
  });
  const averageGrade = gradeSum / gradeCount;
  return <>{averageGrade}</>;
}
```

✅ **Good**: The variables `gradeSum` and `gradeCount`are scoped within the IIFE.

```jsx
function Grade() {
  const averageGrade = (() => {
    let gradeSum = 0;
    let gradeCount = 0;
    grades.forEach((grade) => {
      gradeCount++;
      gradeSum += grade;
    });
    return gradeSum / gradeCount;
  })();
  return <>{averageGrade}</>;
}
```

> 💡 Note: you can also define a function `computeAverageGrade` outside the component and call it inside it.

---

### 9\. Use curried functions to reuse logic (and properly memoize callback functions)

**❌ Bad:** The logic for updating a field is very repetitive.

```jsx
function Form() {
  const [{ name, email }, setFormState] = useState({
    name: "",
    email: "",
  });

  return (
    <>
      <h1>Class Registration Form</h1>
      <form>
        <label>
          Name:{" "}
          <input
            type="text"
            value={name}
            onChange={(evt) =>
              setFormState((formState) => ({
                ...formState,
                name: evt.target.value,
              }))
            }
          />
        </label>
        <label>
          Email:{" "}
          <input
            type="email"
            value={email}
            onChange={(evt) =>
              setFormState((formState) => ({
                ...formState,
                email: evt.target.value,
              }))
            }
          />
        </label>
      </form>
    </>
  );
}
```

**✅ Good:** Introduce `createFormValueChangeHandler` that returns the correct handler for each field.

> Note: This trick is especially nice if you have the ESLint rule [jsx-no-bind](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md) turned on. You can just wrap the curried function inside `useCallback` and "Voilà!".

```jsx
function Form() {
  const [{ name, email }, setFormState] = useState({
    name: "",
    email: "",
  });

  const createFormValueChangeHandler = (field) => {
    return (event) => {
      setFormState((formState) => ({
        ...formState,
        [field]: event.target.value,
      }));
    };
  };

  return (
    <>
      <h1>Class Registration Form</h1>
      <form>
        <label>
          Name:{" "}
          <input
            type="text"
            value={name}
            onChange={createFormValueChangeHandler("name")}
          />
        </label>
        <label>
          Email:{" "}
          <input
            type="email"
            value={email}
            onChange={createFormValueChangeHandler("email")}
          />
        </label>
      </form>
    </>
  );
}
```

---

### 10\. Move data that doesn't rely on the component props/state outside of it for cleaner (and more efficient) code

**❌ Bad:**`OPTIONS` and `renderOption` don't need to be inside the component because they don't depend on any props or state.

Also, keeping them inside means we get new object references every time the component renders. If we were to pass `renderOption` to a child component wrapped in `memo`, it would break the memoization.

```jsx
function CoursesSelector() {
  const OPTIONS = ["Maths", "Literature", "History"];
  const renderOption = (option: string) => {
    return <option>{option}</option>;
  };

  return (
    <select>
      {OPTIONS.map((opt) => (
        <Fragment key={opt}>{renderOption(opt)}</Fragment>
      ))}
    </select>
  );
}
```

**✅ Good:** Move them out of the component to keep the component clean and references stable.

```jsx
const OPTIONS = ["Maths", "Literature", "History"];
const renderOption = (option: string) => {
  return <option>{option}</option>;
};

function CoursesSelector() {
  return (
    <select>
      {OPTIONS.map((opt) => (
        <Fragment key={opt}>{renderOption(opt)}</Fragment>
      ))}
    </select>
  );
}
```

> 💡 Note: In this example, you can simplify further by using the option element inline.

```jsx
const OPTIONS = ["Maths", "Literature", "History"];

function CoursesSelector() {
  return (
    <select>
      {OPTIONS.map((opt) => (
        <option key={opt}>{opt}</option>
      ))}
    </select>
  );
}
```

---

### 11\. When storing the selected item from a list, store the item ID rather than the entire item

**❌ Bad:** If an item is selected but then it changes (i.e., we receive a completely new object reference for the same ID), or if the item is no longer present in the list, `selectedItem` will either retain an outdated value or become incorrect.

```jsx
function ListWrapper({ items }) {
  // We are referencing the entire item
  const [selectedItem, setSelectedItem] = useState<Item | undefined>();

  return (
    <>
      {selectedItem != null && <div>{selectedItem.name}</div>}
      <List
        items={items}
        selectedItem={selectedItem}
        onSelectItem={setSelectedItem}
      />
    </>
  );
}
```

**✅ Good:** We store the selected item by its ID (which should be stable). This ensures that even if the item is removed from the list or one of its properties changed, the UI should be correct.

```jsx
function ListWrapper({ items }) {
  const [selectedItemId, setSelectedItemId] = useState<number | undefined>();
  // We derive the selected item from the list
  const selectedItem = items.find((item) => item.id === selectedItemId);

  return (
    <>
      {selectedItem != null && <div>{selectedItem.name}</div>}
      <List
        items={items}
        selectedItemId={selectedItemId}
        onSelectItem={setSelectedItemId}
      />
    </>
  );
}
```

---

### 12\. If you're frequently checking a prop's value before doing something, introduce a new component

**❌ Bad:** The code is cluttered because of all the `user == null` checks.

> Here, we can't return early because of the [rules of hooks](https://react.dev/warnings/invalid-hook-call-warning).

```jsx
function Posts({ user }) {
  // Due to the rules of hooks, `posts` and `handlePostSelect` must be declared before the `if` statement.
  const posts = useMemo(() => {
    if (user == null) {
      return [];
    }
    return getUserPosts(user.id);
  }, [user]);

  const handlePostSelect = useCallback(
    (postId) => {
      if (user == null) {
        return;
      }
      // TODO: Do something
    },
    [user]
  );

  if (user == null) {
    return null;
  }

  return (
    <div>
      {posts.map((post) => (
        <button key={post.id} onClick={() => handlePostSelect(post.id)}>
          {post.title}
        </button>
      ))}
    </div>
  );
}
```

**✅ Good:** We introduce a new component, `UserPosts`, that takes a defined user and is much cleaner.

```jsx
function Posts({ user }) {
  if (user == null) {
    return null;
  }

  return <UserPosts user={user} />;
}

function UserPosts({ user }) {
  const posts = useMemo(() => getUserPosts(user.id), [user.id]);

  const handlePostSelect = useCallback(
    (postId) => {
      // TODO: Do something
    },
    [user]
  );

  return (
    <div>
      {posts.map((post) => (
        <button key={post.id} onClick={() => handlePostSelect(post.id)}>
          {post.title}
        </button>
      ))}
    </div>
  );
}
```

---

### 13\. Use the CSS `:empty` pseudo-class to hide elements with no children

In the example below 👇, a wrapper takes children and adds a red border around them.

```jsx
function PostWrapper({ children }) {
  return <div className="posts-wrapper">{children}</div>;
}
```

```css
.posts-wrapper {
  border: solid 1px red;
}
```

**❌ Problem:** The border remains visible on the screen even if the children are empty (i.e., equal to `null`, `undefined`, etc.).

![Red border](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yq53ai7xzx200bxreer2.png align="left")

**✅ Solution:** Use the `:empty` CSS pseudo-class to ensure the wrapper is not displayed when it's empty.

```css
.posts-wrapper:empty {
  display: none;
}
```

---

### 14\. Group all the state and context at the top of the component

When all the state and context are located at the top, it is easy to spot what can trigger a component re-render.

**❌ Bad:** State and context are scattered, making it hard to track.

```jsx
function App() {
  const [email, setEmail] = useState("");
  const onEmailChange = (event) => {
    setEmail(event.target.value);
  };
  const [password, setPassword] = useState("");
  const onPasswordChange = (event) => {
    setPassword(event.target.value);
  };
  const theme = useContext(ThemeContext);

  return (
    <div className={`App ${theme}`}>
      <h1>Welcome</h1>
      <p>
        Email: <input type="email" value={email} onChange={onEmailChange} />
      </p>
      <p>
        Password:{" "}
        <input type="password" value={password} onChange={onPasswordChange} />
      </p>
    </div>
  );
}
```

**✅ Good:** All state and context are grouped at the top, making it easy to spot.

```jsx
function App() {
  const theme = useContext(ThemeContext);
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const onEmailChange = (event) => {
    setEmail(event.target.value);
  };
  const onPasswordChange = (event) => {
    setPassword(event.target.value);
  };

  return (
    <div className={`App ${theme}`}>
      <h1>Welcome</h1>
      <p>
        Email: <input type="email" value={email} onChange={onEmailChange} />
      </p>
      <p>
        Password:{" "}
        <input type="password" value={password} onChange={onPasswordChange} />
      </p>
    </div>
  );
}
```

---

## Category #2: Effective Design Patterns & Techniques 🛠 ️

---

### 15\. Leverage the `children` props for cleaner code (and performance benefits)

Using the `children` props has several benefits:

* **Benefit #1:** You can avoid prop drilling by passing props directly to children components instead of routing them through the parent.
    
* **Benefit #2:** Your code is more extensible since you can easily modify children without changing the parent component.
    
* **Benefit #3:** You can use this trick to avoid re-rendering "slow" components (see in the example below 👇).
    

**❌ Bad:**`MyVerySlowComponent` renders whenever `Dashboard` renders, which happens every time the current time updates. You can see it in the next image, where I use [React Developer Tool's profiler](https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi).

```jsx
function App() {
  // Some other logic…
  return (
    <Dashboard />
  );
}

function Dashboard() {
  const [currentTime, setCurrentTime] = useState(new Date());
  useEffect(() => {
    const intervalId = setInterval(() => {
      setCurrentTime(new Date());
    }, 1_000);
    return () => clearInterval(intervalId);
  }, []);

  return (
    <>
      <h1>{currentTime.toTimeString()}</h1>
      <MyVerySlowComponent />  {/* Renders whenever `Dashboard` renders */}
    </>
  );
}
```

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ptrvtuiuhptr2adq9ica.gif align="left")

**MyVerySlowComponent** renders whenever **Dashboard** renders

**✅ Good:**`MyVerySlowComponent` doesn't render when `Dashboard` renders.

```jsx
function App() {
  return (
    <Dashboard >
      <MyVerySlowComponent />
    </Dashboard>
  );
}

function Dashboard({ children }) {
  const [currentTime, setCurrentTime] = useState(new Date());
  useEffect(() => {
    const intervalId = setInterval(() => {
      setCurrentTime(new Date());
    }, 1_000);
    return () => clearInterval(intervalId);
  }, []);

  return (
    <>
      <h1>{currentTime.toTimeString()}</h1>
      {children}
    </>
  );
}
```

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/480znnhi87gws2u9qmuf.gif align="left")

**MyVerySlowComponent** doesn't render anymore

---

### 16\. Build composable code with `compound components`

Think of compound components as **Lego** blocks.

You piece them together to create a customized UI. These components work exceptionally well when creating libraries, resulting in both expressive and highly extendable code.

> You can explore this pattern further here 👉 [Compound Pattern](https://www.patterns.dev/react/compound-pattern/)

**Example from** [**reach.ui**](https://reach.tech/menu-button) (*Menu, MenuButton, MenuList, MenuLink* are compound components)

```jsx
<Menu>
  <MenuButton>
    Actions <span aria-hidden>▾</span>
  </MenuButton>
  <MenuList>
    <MenuItem onSelect={() => alert("Download")}>Download</MenuItem>
    <MenuItem onSelect={() => alert("Copy")}>Create a Copy</MenuItem>
    <MenuLink as="a" href="https://reacttraining.com/workshops/">
    Attend a Workshop
    </MenuLink>
  </MenuList>
</Menu>
```

---

### 17\. Make your code more extensible with `render functions` or `component functions` props

Let's say we want to display various lists, such as messages, profiles, or posts, and each one should be sortable.

To achieve this, we introduce a `List` component for reuse. There are two ways we can go around this:

**❌ Bad: Option 1**`List` handles rendering each item and how they are sorted. This is problematic as it violates the [Open Closed Principle](https://www.freecodecamp.org/news/open-closed-principle-solid-architecture-concept-explained/#whatistheopenclosedprinciple). Whenever a new item type is added, this code will be modified.

**✅ Good: Option 2**`List` takes render functions or component functions, invoking them only when needed.

You can find an example in this sandbox below 👇:

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/nice-feistel-7yd768?from-embed=)

---

### 18\. When dealing with different cases, use `value === case && <Component />` to avoid holding onto old state

**❌ Problem:** In the sandbox below, the counter doesn't reset when switching between `Posts` and `Snippets`. This happens because when rendering the same component, its state persists across type changes.

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/counter-zrl9pf?from-embed=)

**✅ Solution:** Render a component based on the `selectedType` or use a key to force a reset when the type changes.

```jsx
function App() {
  const [selectedType, setSelectedType] = useState<ResourceType>("posts");
  return (
    <>
      <Navbar selectedType={selectedType} onSelectType={setSelectedType} />
      {selectedType === "posts" && <Resource type="posts" />}
      {selectedType === "snippets" && <Resource type="snippets" />}
    </>
  );
}

// We use the `selectedType` as a key
function App() {
  const [selectedType, setSelectedType] = useState<ResourceType>("posts");
  return (
    <>
      <Navbar selectedType={selectedType} onSelectType={setSelectedType} />
      <Resource type={selectedType} key={selectedType} />
    </>
  );
}
```

---

### 19\. Always use error boundaries

By default, if your application encounters an error during rendering, the entire UI crashes 💥.

To prevent this, use [error boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary) to:

* Keep parts of your app functional even if an error occurs.
    
* Display user-friendly error messages and optionally track errors.
    

> 💡 Tip: you can use the [react-error-boundary](https://www.npmjs.com/package/react-error-boundary) library.

---

## Category #3: Keys & Refs 🗝️

---

### 20\. Use `crypto.randomUUID` or `Math.random` to generate keys

JSX elements inside a `map()` call always need keys.

Suppose your elements don't already have keys. In that case, you can generate unique IDs using `crypto.randomUUID`, `Math.random`, or the [uuid](https://www.npmjs.com/package/uuid) library.

> Note: Beware that `crypto.randomUUID` is not defined in older browsers.

---

### 21\. Make sure your list items IDs are stable (i.e., they don't change between renders)

As much as possible, keys/IDs should be stable.

Otherwise, React may re-render some components uselessly, or selections will no longer be valid, like in the example below.

**❌ Bad:**`selectedQuoteId` changes whenever `App` renders, so there is never a valid selection.

```jsx
function App() {
  const [quotes, setQuotes] = useState([]);
  const [selectedQuoteId, setSelectedQuoteId] = useState(undefined);

  // Fetch quotes
  useEffect(() => {
    const loadQuotes = () =>
      fetchQuotes().then((result) => {
        setQuotes(result);
      });
    loadQuotes();
  }, []);

  // Add ids: this is bad!!!
  const quotesWithIds = quotes.map((quote) => ({
    value: quote,
    id: crypto.randomUUID(),
  }));

  return (
    <List
      items={quotesWithIds}
      selectedItemId={selectedQuoteId}
      onSelectItem={setSelectedQuoteId}
    />
  );
}
```

**✅ Good:** The `IDs` are added when we get the quotes.

```jsx
function App() {
  const [quotes, setQuotes] = useState([]);
  const [selectedQuoteId, setSelectedQuoteId] = useState(undefined);

  // Fetch quotes and save with ID
  useEffect(() => {
    const loadQuotes = () =>
      fetchQuotes().then((result) => {
        // We add the `ids` as soon as we get the results
        setQuotes(
          result.map((quote) => ({
            value: quote,
            id: crypto.randomUUID(),
          }))
        );
      });
    loadQuotes();
  }, []);

  return (
    <List
      items={quotes}
      selectedItemId={selectedQuoteId}
      onSelectItem={setSelectedQuoteId}
    />
  );
}
```

---

### 22\. Strategically use the `key` attribute to trigger component re-renders

Want to force a component to re-render from scratch? Just change its `key`.

In the example below, we use this trick to reset the error boundary when switching to a new tab.

> [**🏖 Sandbox**](https://stackblitz.com/edit/using-key-react?file=src%2FApp.tsx)

---

### 23\. Use a `ref callback function` for tasks such as monitoring size changes and managing multiple node elements.

Did you know you can pass a function to the `ref` attribute instead of a ref object?

Here's how it works:

* When the DOM node is added to the screen, React calls the function with the DOM node as the argument.
    
* When the DOM node is removed, React calls the function with `null`.
    

In the example below, we use this tip to skip the `useEffect`

**❌ Before:** Using `useEffect` to focus the input

```jsx
function App() {
  const ref = useRef();

  useEffect(() => {
    ref.current?.focus();
  }, []);

  return <input ref={ref} type="text" />;
}
```

**✅ After:** We focus on the input as soon as it is available.

```jsx
function App() {
  const ref = useCallback((inputNode) => {
    inputNode?.focus();
  }, []);

  return <input ref={ref} type="text" />;
}
```

---

## Category #4: Organizing React code 🧩

---

### 24\. Colocate React components with their assets (e.g., styles, images, etc.)

Always keep each React component with related assets, like styles and images.

* This makes it easier to remove them when the component is no longer needed.
    
* It also simplifies code navigation, as everything you need is in one place.
    

![Components folder structure](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/aqi13ygqu2hw2myf0u1h.png align="left")

---

### 25\. Limit your component file size

Big files with tons of components and exports can be confusing.

Plus, they tend to grow even bigger as more stuff gets added.

So, aim for a reasonable file size and split components into separate files when it makes sense.

---

### 26\. Limit the number of return statements in your functional component file

Multiple `return` statements in a functional component make it hard to see what the component is returning.

This was not a problem for class components where we could search for the `render` term.

A handy trick is to use arrow functions without braces when possible (VSCode has an action for this 😀).

**❌ Bad:** It is harder to spot the component return statement

```jsx
function Dashboard({ posts, searchTerm, onPostSelect }) {
  const filteredPosts = posts.filter((post) => {
    return post.title.includes(searchTerm);
  });
  const createPostSelectHandler = (post) => {
    return () => {
      onPostSelect(post.id);
    };
  };
  return (
    <>
      <h1>Posts</h1>
      <ul>
        {filteredPosts.map((post) => {
          return (
            <li key={post.id} onClick={createPostSelectHandler(post)}>
              {post.title}
            </li>
          );
        })}
      </ul>
    </>
  );
}
```

**✅ Good:** There is one return statement for the component

```jsx
function Dashboard({ posts, searchTerm, onPostSelect, selectedPostId }) {
  const filteredPosts = posts.filter((post) => post.title.includes(searchTerm));
  const createPostSelectHandler = (post) => () => {
    onPostSelect(post.id);
  };
  return (
    <>
      <h1>Posts</h1>
      <ul>
        {filteredPosts.map((post) => (
          <li
            key={post.id}
            onClick={createPostSelectHandler(post)}
            style={{ color: post.id === selectedPostId ? "red" : "black" }}
          >
            {post.title}
          </li>
        ))}
      </ul>
    </>
  );
}
```

---

### 27\. Prefer named exports over default exports

I see default exports everywhere, and it makes me sad 🥲.

Let's compare the two approaches:

```jsx
/// `Dashboard` is exported as the default component
export function Dashboard(props) {
 /// TODO
}

/// `Dashboard` export is named
export function Dashboard(props) {
 /// TODO
}
```

We now import the component like this:

```jsx
/// Default export
import Dashboard from "/path/to/Dashboard"


/// Named export
import { Dashboard } from "/path/to/Dashboard"
```

**These are the problems with default exports:**

* If the component is renamed, the IDE won't automatically rename the export.
    

For example, if `Dashboard` is renamed into `Console`, we will have this:

```jsx
/// In the default export case, the name is not changed
import Dashboard from "/path/to/Console"


/// In the named export case, the name is changed
import { Console } from "/path/to/Console"
```

* It's harder to see what is exported from a file with default exports.
    

For example, in the case of named imports, once I type `import { } from "/path/to/file"`, I get autocompletion when I set my cursor inside the brackets.

* Default exports are harder to re-export.
    

For example, if I wanted to re-export the `Dashboard` component from let's say an `index` file, I would have to do this:

```jsx
export { default as Dashboard } from "/path/to/Dashboard"
```

The solution is more straightforward with named exports.

```jsx
export { Dashboard } from "/path/to/Dashboard"
```

So, please default to named exports 🙏.

> 💡 Note: Even if you're using React [lazy](https://react.dev/reference/react/lazy) you can still use named exports. See an example [here](https://dev.to/iamandrewluca/react-lazy-without-default-export-4b65).

---

## Category #5: Efficient State Management 🚦

---

### 28\. Never create a state for a value that can be derived from other state or props

More state = more trouble.

Every piece of state can trigger a re-render and make resetting the state a hassle.

So, if a value can be derived from state or props, skip adding a new state.

**❌ Bad:**`filteredPosts` doesn't need to be in the state.

```jsx
function App({ posts }) {
  const [filters, setFilters] = useState();
  const [filteredPosts, setFilteredPosts] = useState([]);

  useEffect(
    () => {
      setFilteredPosts(filterPosts(posts, filters));
    },
    [posts, filters]
  );

  return (
    <Dashboard>
      <Filters filters={filters} onFiltersChange={setFilters} />
      {filteredPosts.length > 0 && <Posts posts={filteredPosts} />}
    </Dashboard>
  );
}
```

**✅ Good:**`filteredPosts` is derived from `posts` and `filters.`

```jsx
function App({ posts }) {
  const [filters, setFilters] = useState({});
  const filteredPosts = filterPosts(posts, filters)

  return (
    <Dashboard>
      <Filters filters={filters} onFiltersChange={setFilters} />
      {filteredPosts.length > 0 && <Posts posts={filteredPosts} />}
    </Dashboard>
  );
}
```

---

### 29\. Keep the state at the lowest level necessary to minimize re-renders

Whenever the state changes inside a component, React re-renders the component and all its children (there is an exception with children wrapped in [memo](https://react.dev/reference/react/memo)).

This happens even if those children don't use the changed state. To minimize re-renders, move the state down the component tree as far as possible.

**❌ Bad:** When `sortOrder` changes, both `LeftSidebar` and `RightSidebar` re-render.

```jsx
function App() {
  const [sortOrder, setSortOrder] = useState("popular");
  return (
    <div className="App">
      <LeftSidebar />
      <Main sortOrder={sortOrder} setSortOrder={setSortOrder} />
      <RightSidebar />
    </div>
  );
}

function Main({ sortOrder, setSortOrder }) {
  return (
    <div>
      <Button
        onClick={() => setSortOrder("popular")}
        active={sortOrder === "popular"}
      >
        Popular
      </Button>
      <Button
        onClick={() => setSortOrder("latest")}
        active={sortOrder === "latest"}
      >
        Latest
      </Button>
    </div>
  );
}
```

**✅ Good:**`sortOrder` change will only affect `Main`.

```jsx
function App() {
  return (
    <div className="App">
      <LeftSidebar />
      <Main />
      <RightSidebar />
    </div>
  );
}

function Main() {
  const [sortOrder, setSortOrder] = useState("popular");
  return (
    <div>
      <Button
        onClick={() => setSortOrder("popular")}
        active={sortOrder === "popular"}
      >
        Popular
      </Button>
      <Button
        onClick={() => setSortOrder("latest")}
        active={sortOrder === "latest"}
      >
        Latest
      </Button>
    </div>
  );
}
```

---

### 30\. Clarify the distinction between the initial state and the current state

**❌ Bad:** It's unclear that sortOrder is just the initial value, which may lead to confusion or errors in state management.

```jsx
function Main({ sortOrder }) {
  const [internalSortOrder, setInternalSortOrder] = useState(sortOrder);
  return (
    <div>
      <Button
        onClick={() => setInternalSortOrder("popular")}
        active={internalSortOrder === "popular"}
      >
        Popular
      </Button>
      <Button
        onClick={() => setInternalSortOrder("latest")}
        active={internalSortOrder === "latest"}
      >
        Latest
      </Button>
    </div>
  );
}
```

**✅ Good:** Naming is clear about what is the initial state and what is current.

```jsx
function Main({ initialSortOrder }) {
  const [sortOrder, setSortOrder] = useState(initialSortOrder);
  return (
    <div>
      <Button
        onClick={() => setSortOrder("popular")}
        active={sortOrder === "popular"}
      >
        Popular
      </Button>
      <Button
        onClick={() => setSortOrder("latest")}
        active={sortOrder === "latest"}
      >
        Latest
      </Button>
    </div>
  );
}
```

---

### 31\. Update state based on the previous state, especially when memoizing with `useCallback`

React allows you to pass an updater function to the `set` function from `useState`.

This updater function uses the current state to calculate the next state.

I use this behavior whenever I need to update the state based on the previous state, especially inside functions wrapped with `useCallback.` In fact, this approach prevents the need to have the state as one of the hook dependencies.

**❌ Bad:**`handleAddTodo` and `handleRemoveTodo` change whenever `todos` changes.

```jsx
function App() {
  const [todos, setToDos] = useState([]);
  const handleAddTodo = useCallback(
    (todo) => {
      setToDos([...todos, todo]);
    },
    [todos]
  );

  const handleRemoveTodo = useCallback(
    (id) => {
      setToDos(todos.filter((todo) => todo.id !== id));
    },
    [todos]
  );

  return (
    <div className="App">
      <TodoInput onAddTodo={handleAddTodo} />
      <TodoList todos={todos} onRemoveTodo={handleRemoveTodo} />
    </div>
  );
}
```

**✅ Good:**`handleAddTodo` and `handleRemoveTodo` remain the same even when `todos` changes.

```jsx
function App() {
  const [todos, setToDos] = useState([]);
  const handleAddTodo = useCallback((todo) => {
    setToDos((prevTodos) => [...prevTodos, todo]);
  }, []);

  const handleRemoveTodo = useCallback((id) => {
    setToDos((prevTodos) => prevTodos.filter((todo) => todo.id !== id));
  }, []);

  return (
    <div className="App">
      <TodoInput onAddTodo={handleAddTodo} />
      <TodoList todos={todos} onRemoveTodo={handleRemoveTodo} />
    </div>
  );
}
```

---

### 32\. Use functions in `useState` for lazy initialization and performance gains, as they are invoked only once.

Using a function in useState ensures the initial state is computed only once.

This can improve performance, especially when the initial state is derived from an "expensive" operation like reading from local storage.

**❌ Bad:** We read the theme from local storage every time the component renders

```jsx
const THEME_LOCAL_STORAGE_KEY = "101-react-tips-theme";

function PageWrapper({ children }) {
  const [theme, setTheme] = useState(
    localStorage.getItem(THEME_LOCAL_STORAGE_KEY) || "dark"
  );

  const handleThemeChange = (theme) => {
    setTheme(theme);
    localStorage.setItem(THEME_LOCAL_STORAGE_KEY, theme);
  };

  return (
    <div
      className="page-wrapper"
      style={{ background: theme === "dark" ? "black" : "white" }}
    >
      <div className="header">
        <button onClick={() => handleThemeChange("dark")}>Dark</button>
        <button onClick={() => handleThemeChange("light")}>Light</button>
      </div>
      <div>{children}</div>
    </div>
  );
}
```

**✅ Good:** We only read from the local storage when the component mounts.

```jsx
function PageWrapper({ children }) {
  const [theme, setTheme] = useState(
    () => localStorage.getItem(THEME_LOCAL_STORAGE_KEY) || "dark"
  );

  const handleThemeChange = (theme) => {
    setTheme(theme);
    localStorage.setItem(THEME_LOCAL_STORAGE_KEY, theme);
  };

  return (
    <div
      className="page-wrapper"
      style={{ background: theme === "dark" ? "black" : "white" }}
    >
      <div className="header">
        <button onClick={() => handleThemeChange("dark")}>Dark</button>
        <button onClick={() => handleThemeChange("light")}>Light</button>
      </div>
      <div>{children}</div>
    </div>
  );
}
```

---

### 33\. Use react context for broadly needed, static state to prevent prop drilling

I will use React context whenever I have some data that:

* Is needed in multiple places (e.g., theme, current user, etc.)
    
* Is mostly static or read-only (i.e., the user can't/doesn't change the data often)
    

This approach helps avoid prop drilling (i.e., passing down data or state through multiple layers of the component hierarchy).

See an example in the sandbox below 👇.

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/sad-farrell-t4cqqp?from-embed=)

---

### 34\. React Context: Split your context into parts that change frequently and those that change infrequently to enhance app performance

One challenge with React context is that all components consuming the context re-render whenever the context data changes, even if they don't use the part of the context that changed 🤦‍♀️.

**A solution?** Use separate contexts.

In the example below, we've created two contexts: one for actions (which are constant) and another for state (which can change).

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/context-split-vdny3w?from-embed=)

---

### 35\. React Context: Introduce a `Provider` component when the value computation is not straightforward

**❌ Bad:** There is too much logic inside `App` to manage the theme.

```jsx
export function App() {
  const [theme, setTheme] = useState(() =>
    window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light"
  );
  useEffect(() => {
    const darkThemeMq = window.matchMedia("(prefers-color-scheme: dark)");
    const listener = (event) => {
      setTheme(event.matches ? "dark" : "light");
    };
    darkThemeMq.addEventListener("change", listener);
    return () => darkThemeMq.removeEventListener("change", listener);
  }, []);
  const [selectedPostId, setSelectedPostId] = useState(undefined);
  const onPostSelect = (postId) => {
    // TODO: some logging
    setSelectedPostId(postId);
  };

  return (
    <div className="App">
      <ThemeContext.Provider value={theme}>
        <Dashboard
          posts={posts}
          onPostSelect={onPostSelect}
          selectedPostId={selectedPostId}
        />
      </ThemeContext.Provider>
    </div>
  );
}
```

**✅ Good:** The theme logic is encapsulated in `ThemeProvider`

```jsx
export function App() {
  const [selectedPostId, setSelectedPostId] = useState(undefined);
  const onPostSelect = (postId) => {
    // TODO: some logging
    setSelectedPostId(postId);
  };

  return (
    <div className="App">
      <ThemeProvider>
        <Dashboard
          posts={posts}
          onPostSelect={onPostSelect}
          selectedPostId={selectedPostId}
        />
      </ThemeProvider>
    </div>
  );
}

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState(() =>
    window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light"
  );
  useEffect(() => {
    const darkThemeMq = window.matchMedia("(prefers-color-scheme: dark)");
    const listener = (event) => {
      setTheme(event.matches ? "dark" : "light");
    };
    darkThemeMq.addEventListener("change", listener);
    return () => darkThemeMq.removeEventListener("change", listener);
  }, []);

  return (
    <ThemeContext.Provider value={theme}>{children}</ThemeContext.Provider>
  );
}
```

---

### 36\. Consider using the `useReducer` hook as a lightweight state management solution

Whenever I have too many values in my state or a complex state and don't want to rely on external libraries, I will reach for `useReducer`.

It's especially effective when combined with context for broader state management needs.

**Example:** See [#Tip 34](#34-react-context-split-your-context-into-parts-that-change-frequently-and-those-that-change-infrequently-to-enhance-app-performance).

---

### 37\. Simplify state updates with `useImmer` or `useImmerReducer`

With hooks like `useState` and `useReducer`, the state must be immutable (i.e., all changes need to create a new state vs. modifying the current one).

This is often cumbersome to achieve.

This is where [useImmer](https://github.com/immerjs/use-immer?tab=readme-ov-file#useimmer) and [useImmerReducer](https://github.com/immerjs/use-immer?tab=readme-ov-file#useimmerreducer) offer a simpler alternative. They allow you to write "mutable" code automatically converted to immutable updates.

**❌ Bad:** We must carefully ensure we're creating a new state object.

```jsx
export function App() {
  const [{ email, password }, setState] = useState({
    email: "",
    password: "",
  });
  const onEmailChange = (event) => {
    setState((prevState) => ({ ...prevState, email: event.target.value }));
  };
  const onPasswordChange = (event) => {
    setState((prevState) => ({ ...prevState, password: event.target.value }));
  };

  return (
    <div className="App">
      <h1>Welcome</h1>
      <p>
        Email: <input type="email" value={email} onChange={onEmailChange} />
      </p>
      <p>
        Password:{" "}
        <input type="password" value={password} onChange={onPasswordChange} />
      </p>
    </div>
  );
}
```

**✅ Good:** We can directly modify `draftState`.

```jsx
import { useImmer } from "use-immer";

export function App() {
  const [{ email, password }, setState] = useImmer({
    email: "",
    password: "",
  });
  const onEmailChange = (event) => {
    setState((draftState) => {
      draftState.email = event.target.value;
    });
  };
  const onPasswordChange = (event) => {
    setState((draftState) => {
      draftState.password = event.target.value;
    });
  };

  /// Rest of logic
}
```

---

### 38\. Use Redux (or another state management solution) for complex client-side state accessed across multiple components

I turn to [Redux](https://redux.js.org/) whenever:

* I have a complex FE app with a lot of shared client-side state (for example, dashboard apps)
    
* I want the user to be able to go back in time and revert changes
    
* I don't want my components to re-render unnecessarily like they can with React context
    
* I have too many contexts that start getting out of control
    

For a streamlined experience, I recommend using [redux-tooltkit](https://redux-toolkit.js.org/introduction/getting-started).

> 💡 Note: You can also consider other alternatives to Redux, such as [Zustand](https://github.com/pmndrs/zustand) or [Recoil](https://recoiljs.org/).

---

### 39\. Redux: Use Redux DevTools to debug your state

The **Redux DevTools browser extension** is an useful tool for debugging your Redux projects.

It allows you to visualize your state and actions in real-time, maintain state persistence across refreshes, and much more.

To understand its use, watch this great [YouTube video](https://youtu.be/BYpuigD01Ew?si=ai3UcZ4B1aFZVpwL).

---

## Category #6: React Code Optimization 🚀

---

### 40\. Prevent unnecessary re-renders with `memo`

When dealing with components that are costly to render and their parent components frequently update, wrapping them in [memo](https://react.dev/reference/react/memo) can be a game changer.

`memo` ensures that a component only re-renders when its props have changed, not simply because its parent re-rendered.

In the example below, I get some data from the server through `useGetDashboardData`. If the `posts` didn't change, wrapping `ExpensiveList` in `memo` will prevent it from re-rendering when other parts of the data update.

```jsx
export function App() {
  const { profileInfo, posts } = useGetDashboardData();
  return (
    <div className="App">
      <h1>Dashboard</h1>
      <Profile data={profileInfo} />
      <ExpensiveList posts={posts} />
    </div>
  );
}

const ExpensiveList = memo(
  ({ posts }) => {
    /// Rest of implementation
  }
);
```

> 💡: This tip may be irrelevant once [React compiler](https://react.dev/learn/react-compiler) gets stable 😅.

---

### 41\. Specify an equality function with `memo` to instruct React on how to compare the props.

By default, `memo`uses [Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) to compare each prop with its previous value.

However, specifying a custom equality function can be more efficient than default comparisons or re-rendering for more complex or specific scenarios.

**Example 👇**

```jsx
const ExpensiveList = memo(
  ({ posts }) => {
    return <div>{JSON.stringify(posts)}</div>;
  },
  (prevProps, nextProps) => {
    // Only re-render if the last post or the list size changes
    const prevLastPost = prevProps.posts[prevProps.posts.length - 1];
    const nextLastPost = nextProps.posts[nextProps.posts.length - 1];
    return (
      prevLastPost.id === nextLastPost.id &&
      prevProps.posts.length === nextProps.posts.length
    );
  }
)
```

---

### 42\. Prefer named functions over arrow functions when declaring a memoized component

When defining memoized components, using named functions instead of arrow functions can improve clarity in React DevTools.

Arrow functions often result in generic names like `_c2`, making debugging and profiling more difficult.

**❌ Bad:** Using arrow functions for memoized components results in less informative names in React DevTools.

```jsx
const ExpensiveList = memo(
  ({ posts }) => {
    /// Rest of implementation
  }
);
```

![](https://i.imgur.com/DhNujjR.png align="left")

**ExpensiveList** name is not visible

**✅ Good:** The component's name will be visible in DevTools.

```jsx
const ExpensiveList = memo(
  function ExpensiveListFn({ posts }) {
    /// Rest of implementation
  }
);
```

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x95zmmg2z5cua6oj4hxq.png align="left")

You can see **ExpensiveListFn** in DevTools

---

### 43\. Cache expensive computations or preserve references with `useMemo`

I will generally `useMemo`:

* When I have expensive computations that should not be repeated on each render.
    
* If the computed value is a **non-primitive value** that is used as a dependency in hooks like `useEffect`.
    
* The computed **non-primitive** value will be passed as a prop to a component wrapped in `memo`; otherwise, this will break the memoization since React uses [Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) to detect whether props changed.
    

**❌ Bad:** The `memo` for `ExpensiveList` does not prevent re-renders because styles are recreated on every render.

```jsx
export function App() {
  const { profileInfo, posts, baseStyles } = useGetDashboardData();
  // We get a new `styles` object on every render
  const styles = { ...baseStyles, padding: "10px" };
  return (
    <div className="App">
      <h1>Dashboard</h1>
      <Profile data={profileInfo} />
      <ExpensiveList posts={posts} styles={styles} />
    </div>
  );
}

const ExpensiveList = memo(
  function ExpensiveListFn({ posts, styles }) {
    /// Rest of implementation
  }
);
```

**✅ Good:** The use of `useMemo` ensures `styles` only changes when `baseStyles` changes, allowing `memo` to effectively prevent unnecessary re-renders.

```jsx
export function App() {
  const { profileInfo, posts, baseStyles } = useGetDashboardData();
  // We get a new `styles` object only if `baseStyles` changes
  const styles = useMemo(
    () => ({ ...baseStyles, padding: "10px" }),
    [baseStyles]
  );
  return (
    <div className="App">
      <h1>Dashboard</h1>
      <Profile data={profileInfo} />
      <ExpensiveList posts={posts} styles={styles} />
    </div>
  );
}
```

---

### 44\. Use `useCallback` to memoize functions

`useCallback` is similar to `useMemo` but is designed explicitly for memoizing functions.

**❌ Bad:** Whenever the theme changes, `handleThemeChange` will be called twice, and we will push logs to the server twice.

```jsx
function useTheme() {
  const [theme, setTheme] = useState("light");

  // `handleThemeChange` changes on every render
  // As a result, the effect will be triggered after each render
  const handleThemeChange = (newTheme) => {
    pushLog(["Theme changed"], {
      context: {
        theme: newTheme,
      },
    });
    setTheme(newTheme);
  };

  useEffect(() => {
    const dqMediaQuery = window.matchMedia("(prefers-color-scheme: dark)");
    handleThemeChange(dqMediaQuery.matches ? "dark" : "light");
    const listener = (event) => {
      handleThemeChange(event.matches ? "dark" : "light");
    };
    dqMediaQuery.addEventListener("change", listener);
    return () => {
      dqMediaQuery.removeEventListener("change", listener);
    };
  }, [handleThemeChange]);

  return theme;
}
```

**✅ Good:** Wrapping `handleThemeChange` in useCallback ensures that it is recreated only when necessary, reducing unnecessary executions.

```jsx
const handleThemeChange = useCallback((newTheme) => {
    pushLog(["Theme changed"], {
      context: {
        theme: newTheme,
      },
    });
    setTheme(newTheme);
  }, []);
```

---

### 45\. Memoize callbacks or values returned from utility hooks to avoid performance issues

When you create a custom hook to share with others, memoizing the returned values and functions is crucial.

This practice makes your hook more efficient and prevents unnecessary performance issues for anyone using it.

**❌ Bad:**`loadData` is not memoized and creates performance issues.

```jsx
function useLoadData(fetchData) {
  const [result, setResult] = useState({
    type: "notStarted",
  });

  async function loadData() {
    setResult({ type: "loading" });
    try {
      const data = await fetchData();
      setResult({ type: "loaded", data });
    } catch (err) {
      setResult({ type: "error", error: err });
    }
  }

  return { result, loadData };
}
```

**✅ Good:** We memoize everything so there are no unexpected performance issues.

```jsx
function useLoadData(fetchData) {
  const [result, setResult] = useState({
    type: "notStarted",
  });

  // Wrap in `useRef` and use the `ref` value so the function never changes
  const fetchDataRef = useRef(fetchData);
  useEffect(() => {
    fetchDataRef.current = fetchData;
  }, [fetchData]);

  // Wrap in `useCallback` and use the `ref` value so the function never changes
  const loadData = useCallback(async () => {
    setResult({ type: "loading" });
    try {
      const data = await fetchDataRef.current();
      setResult({ type: "loaded", data });
    } catch (err) {
      setResult({ type: "error", error: err });
    }
  }, []);

  return useMemo(() => ({ result, loadData }), [result, loadData])
}
```

---

### 46\. Leverage lazy loading and `Suspense` to make your apps load faster

When you're building your app, consider using lazy loading and `Suspense` for code that is:

* Expensive to load.
    
* Only relevant to some users (like premium features).
    
* Not immediately necessary for the initial user interaction.
    

In the **sandbox** below 👇, the Slider assets (JS + CSS) only load after you click on a card.

> [**🏖 Sandbox**](https://stackblitz.com/edit/vitejs-vite-czefdj?file=src%2FApp.jsx)

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5t1d58u6qq6dxl2lu1wu.png align="left")

Slider assets only load when needed

---

### 47\. Throttle your network to simulate a slow network

Did you know you can simulate slow internet connections directly in Chrome?

This is especially useful when:

* Customers report slow loading times that you can't replicate on your faster network.
    
* You're implementing lazy loading and want to observe how files load under slower conditions to ensure appropriate loading states.
    

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b5hrtnmrvkuxd63tbitf.gif align="left")

Throttling network requests to observe lazy loading

---

### 48\. Use `react-window` or `react-virtuoso` to efficiently render lists

Never render a long list of items all at once—such as chat messages, logs, or infinite lists.

Doing so can cause the browser to freeze.

Instead, virtualize the list. This means rendering only the subset of items likely to be visible to the user.

Libraries like [react-window](https://github.com/bvaughn/react-window), [react-virtuoso](https://virtuoso.dev/) or [@tanstack/react-virtual](https://tanstack.com/virtual/latest/docs/introduction) are designed for this purpose.

**❌ Bad:**`NonVirtualList` renders all 50,000 log lines simultaneously, even if they aren't visible.

```jsx
function NonVirtualList({ items }) {
  return (
    <div style={{ height: "100%" }}>
      {items.map((log, index) => (
        <div
          key={log.id}
          style={{
            padding: "5px",
            borderBottom:
              index === items.length - 1 ? "none" : "1px solid #ccc",
          }}
        >
          <LogLine log={log} index={index} />
        </div>
      ))}
    </div>
  );
}
```

**✅ Good:**`VirtualList` renders only the items likely to be visible.

```jsx
function VirtualList({ items }) {
  return (
    <Virtuoso
      style={{ height: "100%" }}
      data={items}
      itemContent={(index, log) => (
        <div
          key={log.id}
          style={{
            padding: "5px",
            borderBottom:
              index === items.length - 1 ? "none" : "1px solid #ccc",
          }}
        >
          <LogLine log={log} index={index} />
        </div>
      )}
    />
  );
}
```

You can switch between the two options in the sandbox below and notice how bad the app performs when `NonVirtualList` is used 👇.

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/virtual-list-9qfh5y?from-embed=)

---

## Category #7: Debugging React code 🐞

---

### 49\. Use `StrictMode` to catch bugs in your components before deploying them to production

Using [StrictMode](https://react.dev/reference/react/StrictMode) is a proactive way to detect potential issues in your application during development.

It helps identify problems such as:

* Incomplete cleanup in effects, like forgetting to release resources.
    
* Impurities in React components, ensuring they return consistent JSX given the same inputs (props, state, and context).
    

The example below shows a bug because `clearInterval` is never called. `StrictMode` helps catch this by running the effect twice, which creates two intervals.

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/strict-mode-example-pyrcg8?from-embed=)

---

### 50\. Install the React Developer Tools browser extension to view/edit your components and detect performance issues

React Developer Tools is a must extension ([Chrome](https://chromewebstore.google.com/detail/fmkadmapgofadopljbjfkapdkoienihi), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)).

This extension lets you:

* Visualize and delve into the details of your React components, examining everything from props to state.
    
* Directly modify a component's state or props to see how changes affect behavior and rendering.
    
* Profile your application to identify when and why components are re-rendering, helping you spot performance issues.
    
* Etc.
    

> 💡 Learn how to use it in this great [guide](https://www.freecodecamp.org/news/how-to-use-react-dev-tools/).

---

### 51\. React DevTools Components: Highlight components that render to identify potential issues

I will use this trick whenever I suspect that my app has performance issues. You can highlight the components that render to detect potential problems (e.g., too many renders).

The gif below shows that the `FollowersListFn` component re-renders whenever the time changes, which is wrong.

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/upp8xgmrmzdc79mfz1q5.gif align="left")

Highlight updates when components render.

\---

### 52\. Leverage `useDebugValue` in your custom hooks for better visibility in React DevTools

[useDebugValue](https://react.dev/reference/react/useDebugValue) can be a handy tool for adding descriptive labels to your custom hooks in React DevTools.

This makes it easier to monitor their states directly from the DevTools interface.

For instance, consider this custom hook I use to fetch and display the current time, updated every second:

```jsx
function useCurrentTime(){
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const intervalId = setInterval(() => {
      setTime(new Date());
    }, 1_000);
    return () => clearInterval(intervalId);
  }, [setTime]);

  return time;
}
```

**❌ Bad:** Without `useDebugValue`, the actual time value isn't immediately visible; you'd need to expand the CurrentTime hook:

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kangnk8gzzpvzynhcesl.png align="left")

Current time not quickly visible on the devtools

**✅ Good:** With `useDebugValue`, the current time is readily visible:

```jsx
useDebugValue(time)
```

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r1ilx97d01ownw9933zb.png align="left")

Current time quickly visible on the devtools

> Note: Use `useDebugValue` judiciously. It's best reserved for complex hooks in shared libraries where understanding the internal state is crucial.

---

### 53\. Use the `why-did-you-render` library to track component rendering and identify potential performance bottlenecks

Sometimes, a component re-renders, and it's not immediately clear why 🤦‍♀️.

While React DevTools is helpful, in large apps, it might only provide vague explanations like "hook #1 rendered," which can be useless.

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/me1hs1f232w3bpvwfhu4.png align="left")

App rendering because of hook 1 change

In such cases, you can turn to the [why-did-you-render](https://github.com/welldone-software/why-did-you-render) library. It offers more detailed insights into why components re-render, helping to pinpoint performance issues more effectively.

I made an example in the sandbox below 👇. Thanks to this library, we can find the issue with the `FollowersList` component.

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/why-did-you-render-sandbox-forked-nc4fnk?from-embed=)

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g889xopi0k3qdjz7tab6.png align="left")

*why-did-you-render* console logs

---

### 54\. Hide logs during the second render in Strict Mode

[StrictMode](https://react.dev/reference/react/StrictMode) helps catch bugs early in your application's development.

However, since it causes components to render twice, this can result in duplicated logs, which might clutter your console.

You can hide logs during the second render in Strict Mode to address this.

Check out how to do it in the gif below 👇:

![Hide logs during the second render in Strict Mode](https://i.imgur.com/ozccbE2.gif align="left")

---

## Category #8: Testing React code 🧪

---

### 55\. Use `React Testing Library` to test your React components effectively

Want to test your React apps?

Make sure to use [@testing-library/react](https://testing-library.com/docs/react-testing-library/intro/).

You can find a minimal example [here](https://testing-library.com/docs/react-testing-library/example-intro).

---

### 56\. React Testing Library: Use testing playground to effortlessly create queries

Struggling to decide which queries to use in your tests?

Consider using [testing playground](https://testing-playground.com/) to quickly generate them from your component's HTML.

Here are two ways to leverage it:

**Option #1:** Use `screen.logTestingPlaygroundURL()` in your test. This function generates a URL that opens the Testing Playground tool with the HTML of your component already loaded.

![Using screen.logTestingPlaygroundURL](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8qevn3ro4jr5ykvy4ns3.png align="left")

**Option #2:** Install [Testing Playground Chrome extension](https://chromewebstore.google.com/detail/testing-playground/hejbmebodbijjdhflfknehhcgaklhano). This extension allows you to hover over elements in your app directly in the browser to find the best queries for testing them.

![Using testing playground extension](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y7ul73mmlq600e26gvrr.png align="left")

---

### 57\. Conduct end-to-end tests with `Cypress` or `Playwright`

Need to conduct end-to-end tests?

Make sure to check out [Cypress](https://docs.cypress.io/guides/component-testing/react/overview) or [Playwright](https://playwright.dev/docs/test-components).

> *Note:* Playwright's support for components is experimental at the time of writing.

---

### 58\. Use `MSW` to mock network requests in your tests

Sometimes, your tests need to make network requests.

Rather than implementing your own mocks (or, God forbid, making actual network requests 😅), consider using [MSW (Mock Service Worker)](https://mswjs.io/) to handle your API responses.

MSW allows you to intercept and manipulate network interactions directly in your tests, providing a robust and straightforward solution for simulating server responses **without affecting live servers**.

This approach helps maintain a controlled and predictable testing environment, enhancing the reliability of your tests.

---

## Category #9: React hooks 🎣

---

### 59\. Make sure you perform any required cleanup in your `useEffect` hooks

Always return a cleanup function in your `useEffect` hooks if you're setting up anything that needs to be cleaned up later.

This could be anything from ending a chat session to closing a database connection.

Neglecting this step can lead to poor resource usage and potential memory leaks.

**❌ Bad:** This example sets an interval. But we never clear it, which means it keeps running even after the component is unmounted.

```jsx
function Timer() {
  const [time, setTime] = useState(new Date());
  useEffect(() => {
    setInterval(() => {
      setTime(new Date());
    }, 1_000);
  }, []);

  return <>Current time {time.toLocaleTimeString()}</>;
}
```

**✅ Good:** The interval is properly cleared when the component unmounts.

```jsx
function Timer() {
  const [time, setTime] = useState(new Date());
  useEffect(() => {
    const intervalId = setInterval(() => {
      setTime(new Date());
    }, 1_000);
    // We clear the interval
    return () => clearInterval(intervalId);
  }, []);

  return <>Current time {time.toLocaleTimeString()}</>;
}
```

---

### 60\. Use `refs` for accessing DOM elements

You should never manipulate the DOM directly with React.

Methods like `document.getElementById` and `document.getElementsByClassName` are forbidden since React should access/manipulate the DOM.

So, what should you do when you need access to DOM elements?

You can use the [useRef](https://react.dev/reference/react/useRef) hook, like in the example below, where we need access to the canvas element.

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/chart-js-example-xdrnqv?from-embed=)

> *Note:* We could have added an ID to the canvas and used `document.getElementById`, but this is not recommended.

---

### 61\. Use `refs` to preserve values across re-renders

If you have mutable values in your React component that aren't stored in the state, you'll notice that changes to these values don't persist through re-renders.

This happens unless you save them globally.

You might consider putting these values in the state. However, if they are irrelevant to the rendering, doing so can cause unnecessary re-renders, which wastes performance.

This is where [useRef](https://react.dev/reference/react/useRef) also shines.

In the example below, I want to stop the timer when the user clicks on some button. For that, I need to store the intervalId somewhere.

**❌ Bad:** The example below won't work as intended because `intervalId` gets reset with every component re-render.

```jsx
function Timer() {
  const [time, setTime] = useState(new Date());
  let intervalId;

  useEffect(() => {
    intervalId = setInterval(() => {
      setTime(new Date());
    }, 1_000);
    return () => clearInterval(intervalId);
  }, []);

  const stopTimer = () => {
    intervalId && clearInterval(intervalId);
  };

  return (
    <>
      <>Current time: {time.toLocaleTimeString()} </>
      <button onClick={stopTimer}>Stop timer</button>
    </>
  );
}
```

**✅ Good:** By using `useRef`, we ensure that the interval ID is preserved between renders.

```jsx
function Timer() {
  const [time, setTime] = useState(new Date());
  const intervalIdRef = useRef();
  const intervalId = intervalIdRef.current;

  useEffect(() => {
    const interval = setInterval(() => {
      setTime(new Date());
    }, 1_000);
    intervalIdRef.current = interval;
    return () => clearInterval(interval);
  }, []);

  const stopTimer = () => {
    intervalId && clearInterval(intervalId);
  };

  return (
    <>
      <>Current time: {time.toLocaleTimeString()} </>
      <button onClick={stopTimer}>Stop timer</button>
    </>
  );
}
```

---

### 62\. Prefer named functions over arrow functions within hooks such as `useEffect` to easily find them in React Dev Tools

If you have many hooks, finding them in React DevTools can be challenging.

One trick is to use named functions so you can quickly spot them.

**❌ Bad:** It's hard to find the specific effect among many hooks.

```jsx
function HelloWorld() {
  useEffect(() => {
    console.log("🚀 ~ Hello, I just got mounted")
  }, []);

  return <>Hello World</>;
}
```

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hnrzeovfn1uw1opy0ca0.png align="left")

Effect has no associated name

**✅ Good:** You can quickly spot the effect.

```jsx
function HelloWorld() {
  useEffect(function logOnMount() {
    console.log("🚀 ~ Hello, I just got mounted");
  }, []);

  return <>Hello World</>;
}
```

![](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7u76jr3v823lcam3nf98.png align="left")

Effect with associated name

---

### 63\. Encapsulate logic with custom hooks

Let's say I have a component that gets the theme from the user's dark mode preferences and uses it inside the app.

It's better to extract the logic that returns the theme into a custom hook (to reuse it and keep the component clean).

**❌ Bad:**`App` is overcrowded

```jsx
function App() {
  const [theme, setTheme] = useState("light");

  useEffect(() => {
    const dqMediaQuery = window.matchMedia("(prefers-color-scheme: dark)");
    setTheme(dqMediaQuery.matches ? "dark" : "light");
    const listener = (event) => {
      setTheme(event.matches ? "dark" : "light");
    };
    dqMediaQuery.addEventListener("change", listener);
    return () => {
      dqMediaQuery.removeEventListener("change", listener);
    };
  }, []);

  return (
    <div className={`App ${theme === "dark" ? "dark" : ""}`}>Hello Word</div>
  );
}
```

**✅ Good:**`App` is much simpler, and we can reuse the logic

```jsx
function App() {
  const theme = useTheme();

  return (
    <div className={`App ${theme === "dark" ? "dark" : ""}`}>Hello Word</div>
  );
}

// Custom hook that can be reused
function useTheme() {
  const [theme, setTheme] = useState("light");

  useEffect(() => {
    const dqMediaQuery = window.matchMedia("(prefers-color-scheme: dark)");
    setTheme(dqMediaQuery.matches ? "dark" : "light");
    const listener = (event) => {
      setTheme(event.matches ? "dark" : "light");
    };
    dqMediaQuery.addEventListener("change", listener);
    return () => {
      dqMediaQuery.removeEventListener("change", listener);
    };
  }, []);

  return theme;
}
```

---

### 64\. Prefer functions over custom hooks

Never put logic inside a hook when a function can be used 🛑.

In effect:

* Hooks can only be used inside other hooks or components, whereas functions can be used everywhere.
    
* Functions are simpler than hooks.
    
* Functions are easier to test.
    
* Etc.
    

**❌ Bad:** The `useLocale` hook is unnecessary since it doesn't need to be a hook. It doesn't use other hooks like `useEffect`, `useState`, etc.

```jsx
function App() {
  const locale = useLocale();
  return (
    <div className="App">
      <IntlProvider locale={locale}>
        <BlogPost post={EXAMPLE_POST} />
      </IntlProvider>
    </div>
  );
}

function useLocale() {
  return window.navigator.languages?.[0] ?? window.navigator.language;
}
```

**✅ Good:** Create a function `getLocale` instead

```jsx
function App() {
  const locale = getLocale();
  return (
    <div className="App">
      <IntlProvider locale={locale}>
        <BlogPost post={EXAMPLE_POST} />
      </IntlProvider>
    </div>
  );
}

function getLocale() {
  return window.navigator.languages?.[0] ?? window.navigator.language;
}
```

---

### 65\. Prevent visual UI glitches by using the `useLayoutEffect` hook

When an effect isn't caused by a user interaction, the user will see the UI before the effect runs (often briefly).

As a result, if the effect modifies the UI, the user will see the initial UI version very quickly before seeing the updated one, creating a visual glitch.

Using `useLayoutEffect` ensures the effect runs synchronously after all DOM mutations, preventing the initial render glitch.

In the **sandbox** below, we want the width to be equally distributed between the columns (I know this can be done in CSS, but I need an example 😅).

With `useEffect`, you can see briefly at the beginning that the table is changing. The columns are rendered with their default size before being adjusted to their correct size.

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/use-layout-effect-hqhnld?from-embed=)

If you're looking for another great usage, check out this [post](https://www.developerway.com/posts/no-more-flickering-ui).

---

### 66\. Generate unique IDs for accessibility attributes with the `useId` hook

Tired of coming up with IDs or having them clash?

You can use the [useId](https://react.dev/reference/react/useId) hook to generate a unique ID inside your React component and ensure your app is accessible.

**Example**

```jsx
function Form() {
  const id = useId();
  return (
    <div className="App">
      <div>
        <label htmlFor={id}>Name</label>
        <input type="text" id={id} aria-describedby={id} />
      </div>
      <span id={id}>Make sure to include full name</span>
    </div>
  );
}
```

---

### 67\. Use the `useSyncExternalStore`to subscribe to an external store

This is a rarely needed but super powerful hook 💪.

Use this hook if:

* You have some state not accessible in the React tree (i.e., not present in the state or context)
    
* The state can change, and you need your component to be notified of changes
    

In the example below, I want a `Logger`[singleton](https://www.patterns.dev/vanilla/singleton-pattern/) to log errors, warnings, info, etc., in my entire app.

These are the **requirements**:

* I need to be able to call this everywhere in my React app (even inside non-React components), so I won't put it inside a state/context.
    
* I want to display all the logs to the user inside a `Logs` component
    

👉 I can use `useSyncExternalStore` inside my `Logs` component to access the logs and listen to changes.

```js
function createLogger() {
  let logs = [];
  let listeners = [];

  const pushLog = (log) => {
    logs = [...logs, log];
    listeners.forEach((listener) => listener());
  };

  return {
    getLogs: () => Object.freeze(logs),
    subscribe: (listener) => {
      listeners.push(listener);
      return () => {
        listeners = listeners.filter((l) => l !== listener);
      };
    },
    info: (message) => {
      pushLog({ level: "info", message });
      console.info(message);
    },
    error: (message) => {
      pushLog({ level: "error", message });
      console.error(message);
    },
    warn: (message) => {
      pushLog({ level: "warn", message });
      console.warn(message);
    },
  };
}

export const Logger = createLogger();
```

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/sync-external-storage-rkq2zq?from-embed=)

---

### 68\. Use the `useDeferredValue` hook to display the previous query results until the new results become available

Imagine you're building an app that represents countries on a map.

Users can filter to see countries up to a specific population size.

Every time `maxPopulationSize` updates, the map is re-rendered (see sandbox below).

> [**🏖 Sandbox**](https://codesandbox.io/p/sandbox/countries-app-use-deferred-value-4nh73p?from-embed=)

As a result, notice how janky the slider is **when you move it too fast**. This is because the map is re-rendered every time the slider moves.

To solve this, we can use the `useDeferredValue` hook so that the slider updates smoothly.

```jsx
<Map
    maxPopulationSize={deferredMaxPopulationSize}
    // …
/>
```

If you're looking for another great usage, check out this [post](https://www.joshwcomeau.com/react/use-deferred-value/).

---

## Category #10: Must-known React Libraries/Tools 🧰

---

### 69\. Incorporate routing into your app with `react-router`

If you need your app to support multiple pages, check out [react-router](https://reactrouter.com/).

You can find a minimal example [here](https://reactrouter.com/en/main/start/tutorial).

---

### 70\. Implement first-class data fetching in your app with `swr` or `React Query`

Data fetching can be notoriously tricky.

However, libraries like [swr](https://swr.vercel.app/) or [React Query](https://react-query.tanstack.com/) make it much easier.

I recommend [swr](https://swr.vercel.app/) for simple use cases and [React Query](https://react-query.tanstack.com/) for more complex ones.

---

### 71\. Simplify form state management with libraries like `formik`, `React Hook Form`, or `TanStack Form`

I used to hate form management in React 🥲.

That was until I discovered libraries like:

* [formik](https://formik.org/)
    
* [React Hook Form](https://react-hook-form.com/), or
    
* or [TanStack Form](https://tanstack.com/form)
    

So make sure to check these out if you're struggling with forms.

---

### 72\. Internationalize your app using `Format.js,Lingui,` or `react-i18next.`

If your app needs to support multiple languages, it should be internationalized.

You can achieve this with libraries like:

* [Format.js](https://formatjs.io/)
    
* [Lingui](https://lingui.js.org/)
    
* [react-i18next](https://react.i18next.com/)
    

---

### 73\. Effortlessly create impressive animations with `framer-motion`

Animations can make your app stand out 🔥.

You can create them easily with [framer-motion](https://www.framer.com/motion/).

---

### 74\. Tired of re-inventing the wheel with custom hooks? Check out https://usehooks.com/

If you're like me, you've written the same hooks over and over again.

So check [usehooks.com](https://usehooks.com/) first to see if someone has already done the work for you.

---

### 75\. Streamline app development by leveraging UI libraries like Shadcdn or Headless UI

It's hard to build UI at scale that is accessible, responsive, and beautiful.

Libraries like [Shadcdn](https://ui.shadcn.com/) or [Headless UI](https://headlessui.com/) make it easier.

* Shadcdn provides a set of accessible, reusable, and composable React components that you can copy and paste into your apps. At the time of writing, it requires Tailwind CSS.
    
* Headless UI provides unstyled, fully accessible UI components that you can use to build your own UI components.
    

---

### 76\. Check your website's accessibility with the `axe-core-npm` library

Websites should be accessible to everyone.

However, it's easy to miss accessibility issues.

[axe-core-npm](https://www.npmjs.com/package/axe-core) is a fast, secure, and reliable way to check your website's accessibility while developing it.

> 💡 Tip: If you're a VSCode user, you can install the associated extension: [axe Accessibility Linter](https://marketplace.visualstudio.com/items?itemName=deque-systems.vscode-axe-linter).

---

### 77\. Refactor React code effortlessly with `react-codemod`

Codemods are transformations that run on your codebase programmatically 💻.

They make it easy to refactor your codebase.

For example, [React codemods](https://github.com/reactjs/react-codemod) can help you remove all React imports from your codebase, update your code to use the latest React features, and more.

So, make sure to check those out before manually refactoring your code.

---

### 78\. Transform your app into a Progressive Web Application (PWA) using vite-pwa

Progressive Web Applications (PWAs) load like regular web pages but offer functionality such as working offline, push notifications, and device hardware access.

You can easily create a PWA in React using [vite-pwa](https://vite-pwa-org.netlify.app/).

---

## Category #11: React & Visual Studio Code 🛠️

---

### 79\. Enhance your productivity with the Simple React Snippets snippets extension

Bootstrapping a new React component can be tedious 😩.

Snippets from the [Simple React Snippets](https://marketplace.visualstudio.com/items?itemName=burkeholland.simple-react-snippets) extension make it easier.

---

### 80\. Set `editor.stickyScroll.enabled` to `true` to quickly locate the current component

I love this feature ❤️.

If you have a big file, it can be hard to locate the current component.

By setting `editor.stickyScroll.enabled` to `true`, the current component will always be at the top of the screen.

* **❌ Without sticky scroll**
    

![Without Sticky Scroll](https://i.imgur.com/nCdd3uL.gif align="left")

* **✅ With sticky scroll**
    

![With Sticky Scroll](https://i.imgur.com/NdBGSir.gif align="left")

---

### 81\. Simplify refactoring with extensions like VSCode Glean or VSCode React Refactor

If you need to refactor your code frequently (for example, extract JSX into a new component), make sure to check out extensions like [VSCode Glean](https://marketplace.visualstudio.com/items?itemName=wix.glean) or [VSCode React Refactor](https://marketplace.visualstudio.com/items?itemName=planbcoding.vscode-react-refactor).

---

## Category #12: React & TypeScript 🚀

---

### 82\. Use `ReactNode` instead of `JSX.Element | null | undefined | ...` to keep your code more compact

I see this mistake a lot.

Instead of typing your children's prop like this:

```jsx
const MyComponent = ({ children }: {
  children:
    | JSX.Element
    | null
    | undefined
    // | ...
}) => {
  //   …
};
```

You can use `ReactNode` to keep your code more compact.

```jsx
const MyComponent = ({ children }: { children: ReactNode }) => {
  //   …
};
```

---

### 83\. Simplify the typing of components expecting children props with `PropsWithChildren`

You don't have to type the `children` prop manually.

In fact, you can use `PropsWithChildren` to simplify the typings.

```jsx
// 🟠 Ok
const HeaderPage = ({ children,...pageProps }: { children: ReactNode } & PageProps) => {
  //   …
};

// ✅ Better
const HeaderPage = ({ children, ...pageProps } : PropsWithChildren<PageProps>) => {
//   …
};
```

---

### 84\. Access element props efficiently with `ComponentProps`, `ComponentPropsWithoutRef`,…

There are cases where you need to figure out a component's props.

For example, let's say you want a button that will log to the console when clicked.

You can use `ComponentProps` to access the props of the `button` element then override the `click` prop.

```jsx
const ButtonWithLogging = (props: ComponentProps<"button">) => {
  const handleClick: MouseEventHandler<HTMLButtonElement> = (e) => {
    console.log("Button clicked"); //TODO: Better logging
    props.onClick?.(e);
  };
  return <button {...props} onClick={handleClick} />;
};
```

This trick also works with custom components.

```jsx
const MyComponent = (props: { name: string }) => {
  //   …
};

const MyComponentWithLogging = (props: ComponentProps<typeof MyComponent>) => {
  //   …
};
```

---

### 85\. Leverage types like `MouseEventHandler`, `FocusEventHandler` and others for concise typings

Rather than typing the event handlers manually, you can use types like `MouseEventHandler` to keep the code more concise and readable.

```jsx
// 🟠 Ok
const MyComponent = ({ onClick, onFocus, onChange }: {
  onClick: (e: MouseEvent<HTMLButtonElement>) => void;
  onFocus: (e: FocusEvent<HTMLButtonElement>) => void;
  onChange: (e: ChangeEvent<HTMLInputElement>) => void;
}) => {
  //   …
};

// ✅ Better
const MyComponent = ({ onClick, onFocus, onChange }: {
  onClick: MouseEventHandler<HTMLButtonElement>;
  onFocus: FocusEventHandler<HTMLButtonElement>;
  onChange: ChangeEventHandler<HTMLInputElement>;
}) => {
  //   …
};
```

---

### 86\. Specify types explicitly in useState, useRef, etc., when the type can't be or shouldn't be inferred from the initial value

Don't forget to specify the type when it can't be inferred from the initial value.

For example, in the example below, there is a `selectedItemId` stored in the state. It should be a `string` or `undefined`.

since the type is not specified, TypeScript will infer the type as `undefined`, which is not what we want.

```jsx
// ❌ Bad: `selectedItemId` will be inferred as `undefined`
const [selectedItemId, setSelectedItemId] = useState(undefined);

// ✅ Good
const [selectedItemId, setSelectedItemId] = useState<string | undefined>(undefined);
```

> 💡 Note: The opposite of this is that you don't need to specify the type when TypeScript can infer it for you.

---

### 87\. Leverage the `Record` type for cleaner and more extensible code

I love this helper type.

Let's say I have a type that represents log levels.

```ts
type LogLevel = "info" | "warn" | "error";
```

We have a corresponding function for each log level that logs the message.

```ts
const logFunctions = {
  info: (message: string) => console.info(message),
  warn: (message: string) => console.warn(message),
  error: (message: string) => console.error(message),
};
```

Instead of typing the logFunctions manually, you can use the Record type.

```ts
const logFunctions: Record<LogLevel, (message: string) => void> = {
  info: (message) => console.info(message),
  warn: (message) => console.warn(message),
  error: (message) => console.error(message),
};
```

Using the `Record` type makes the code more concise and readable.

Additionally, it helps capture any error if a new log level is added or removed.

For example, if I decided to add a `debug` log level, TypeScript would throw an error.

![Error with debug item](https://i.imgur.com/m0WzKT3.png align="left")

---

### 88\. Use the `as const` trick to accurately type your hook return values

Let's say we have a hook `useIsHovered` to detect whether a div element is hovered.

The hook returns a `ref` to use with the div element and a boolean indicating whether the div is hovered.

```jsx
const useIsHovered = () => {
  const ref = useRef<HTMLDivElement>(null);
  const [isHovered, setIsHovered] = useState(false);
  // TODO : Rest of implementation
  return [ref, isHovered]
};
```

Currently, TypeScript will not correctly infer the function return type.

![Incorrect return type for function](https://i.imgur.com/pjVi1t9.png align="left")

You can either fix this by explicitly typing the return type like this:

```jsx
const useIsHovered = (): [RefObject<HTMLDivElement>, boolean] => {
  // TODO : Rest of implementation
  return [ref, isHovered]
};
```

Or you can use the `as const` trick to accurately type the return values:

```jsx
const useIsHovered = () => {
  // TODO : Rest of implementation
  return [ref, isHovered] as const;
};
```

---

### 89\. Redux: Ensure proper typing by referring to https://react-redux.js.org/using-react-redux/usage-with-typescript to correctly type your Redux state and helpers

I love using Redux to manage heavy client-side state.

It also works well with TypeScript.

You can find a great guide on how to use Redux with TypeScript [here](https://react-redux.js.org/using-react-redux/usage-with-typescript).

---

### 90\. Simplify your types with `ComponentType`

Let's say you're designing an app like Figma (I know, you're ambitious 😅).

The app is made of widgets, each accepting a `size`.

To reuse logic, we can define a shared `WidgetWrapper` component that takes a widget of type `Widget` defined as follows:

```jsx
interface Size {
  width: number;
  height: number
};

interface Widget {
  title: string;
  Component: ComponentType<{ size: Size }>;
}
```

The `WidgetWrapper` component will render the widget and pass the relevant size to it.

```jsx
const WidgetWrapper = ({ widget }: { widget: Widget }) => {
  const { Component, title } = widget;
  const { onClose, size, onResize } = useGetProps(); //TODO: better name but you get the idea 😅
  return (
    <Wrapper onClose={onClose} onResize={onResize}>
      <Title>{title}</Title>
      {/* We can render the component below with the size */}
      <Component size={size} />
    </Wrapper>
  );
};
```

---

### 91\. Make your code more reusable with TypeScript generics

If you're not using TypeScript generics, only two things could be happening:

* You are either writing very simple code or,
    
* You are missing out 😅
    

TypeScript generics make your code more reusable and flexible.

For example, let's say I have different items on a blog (e.g., `Post`, `Follower`, etc.), and I want a generic list component to display them.

```ts
export interface Post {
  id: string;
  title: string;
  contents: string;
  publicationDate: Date;
}

export interface User {
  username: string;
}

export interface Follower extends User {
  followingDate: Date;
}
```

Each list should be sortable.

There is a bad and a good way to do this.

**❌ Bad:** I create a single list component that accepts a union of items.

This is bad because:

* Every time a new item is added, the functions/types must be updated.
    
* The function is not entirely type-safe (see `This shouldn't happen` comment).
    
* This code depends on other files (e.g.: `FollowerItem`, `PostItem`).
    
* Etc.
    

```jsx
import { FollowerItem } from "./FollowerItem";
import { PostItem } from "./PostItem";
import { Follower, Post } from "./types";

type ListItem = { type: "follower"; follower: Follower } | { type: "post"; post: Post };

function ListBad({
  items,
  title,
  vertical = true,
  ascending = true,
}: {
  title: string;
  items: ListItem[];
  vertical?: boolean;
  ascending?: boolean;
}) {
  const sortedItems = [...items].sort((a, b) => {
    const sign = ascending ? 1 : -1;
    return sign * compareItems(a, b);
  });

  return (
    <div>
      <h3 className="title">{title}</h3>
      <div className={`list ${vertical ? "vertical" : ""}`}>
        {sortedItems.map((item) => (
          <div key={getItemKey(item)}>{renderItem(item)}</div>
        ))}
      </div>
    </div>
  );
}

function compareItems(a: ListItem, b: ListItem) {
  if (a.type === "follower" && b.type === "follower") {
    return (
      a.follower.followingDate.getTime() - b.follower.followingDate.getTime()
    );
  } else if (a.type == "post" && b.type === "post") {
    return a.post.publicationDate.getTime() - b.post.publicationDate.getTime();
  } else {
    // This shouldn't happen
    return 0;
  }
}

function getItemKey(item: ListItem) {
  switch (item.type) {
    case "follower":
      return item.follower.username;
    case "post":
      return item.post.id;
  }
}

function renderItem(item: ListItem) {
  switch (item.type) {
    case "follower":
      return <FollowerItem follower={item.follower} />;
    case "post":
      return <PostItem post={item.post} />;
  }
}
```

**Instead, we can use TypeScript generics to create a more reusable and type-safe list component.**

I made an example in the sandbox below 👇.

> [**🏖 Sandbox**](https://codesandbox.io/p/devbox/generics-ts-zvfp6n?file=%2Fsrc%2Fcomponents%2FList.tsx&embed=1&runonclick=1)

---

### 92\. Ensure precise typing with the `NoInfer` utility type

Imagine you're developing a video game 🎮.

The game has multiple locations (e.g., `LeynTir`, `Forin`, `Karin`, etc.).

You want to create a function that teleports the player to a new location.

```ts
function teleportPlayer<L extends string>(
  position: Position,
  locations: L[],
  defaultLocation: L,
) : L {
  // Teleport the player and return the location
}
```

The function will be called this way:

```ts
const position = { x: 1, y: 2, z: 3 };
teleportPlayer(position, ["LeynTir", "Forin", "Karin"], "Forin");
teleportPlayer(position, ["LeynTir", "Karin"], "anythingCanGoHere"); // ❌ This will work, but it is wrong since "anythingCanGoHere" shouldn't be a valid location
```

The second example is invalid because `anythingCanGoHere` is not a valid location.

However, TypeScript will not throw an error since it inferred the type of `L` from the list and the default location.

**To fix this**, use the `NoInfer` utility type.

```ts
function teleportPlayer<L extends string>(
  position: Position,
  locations: L[],
  defaultLocation: NoInfer<L>,
) : NoInfer<L> {
  // Teleport the player and return the location
}
```

Now TypeScript will throw an error:

```ts
teleportPlayer(position, ["LeynTir", "Karin"], "anythingCanGoHere"); // ❌ Error: Argument of type '"anythingCanGoHere"' is not assignable to parameter of type '"LeynTir" | "Karin"
```

Using the `NoInfer` utility type ensures that the default location must be one of the valid locations provided in the list, preventing invalid inputs.

---

### 93\. Effortlessly type refs with the `ElementRef` type helper

There is a hard and easy way to type refs.

The hard way is to remember the element's type name and use it directly 🤣.

```ts
const ref = useRef<HTMLDivElement>(null);
```

The easy way is to use the `ElementRef` type helper. This method is more straightforward since you should already know the element's name.

```ts
const ref = useRef<ElementRef<"div">>(null);
```

---

## Category #13: Miscellaneous Tips 🎉

---

### 94\. Boost your code's quality and safety with `eslint-plugin-react` and Prettier.

You can't be serious about React if you're not using [eslint-plugin-react](https://www.npmjs.com/package/eslint-plugin-react)😅.

It helps you catch potential bugs and enforce best practices.

So, make sure to install and configure it for your project.

You can also use [Prettier](https://prettier.io/) to format your code automatically and ensure your codebase is consistent.

---

### 95\. Log and monitor your app with tools like Sentry or Grafana Cloud Frontend Observability.

You can't improve what you don't measure 📏.

If you're looking for a monitoring tool for your production apps, check out [Sentry](https://sentry.io/) or [Grafana Cloud Frontend Observability](https://grafana.com/products/cloud/frontend-observability-for-real-user-monitoring/).

---

### 96\. Start coding quickly with online IDEs like **Code Sandbox** or **Stackblitz**

Local development environments can be a pain to set up.

Especially as a beginner 🐣.

So start with online IDEs like [Code Sandbox](https://codesandbox.io/) or [Stackblitz](https://stackblitz.com/).

These tools allow you to start coding quickly without worrying about setting up your environment.

---

### 97\. Looking for advanced react skills? Check out these books 👇

If you're looking for advanced React books 📚, I would recommend:

* [Advanced React](https://www.advanced-react.com/) by @adevnadia
    
* [Fluent React](https://www.oreilly.com/library/view/fluent-react/9781098138707/) by [@TejasKumar\_](https://x.com/TejasKumar_)
    
* [Building Large Scale Web Apps](https://largeapps.dev/) by [@addyosmani](https://x.com/addyosmani) and [@djirdehh](https://x.com/djirdehh)
    

---

### 98\. Prepping React interviews? Check reactjs-interview-questions

React interviews ⚛️ can be tricky.

Luckily, you can prep for them by checking [this repo](https://github.com/sudheerj/reactjs-interview-questions).

---

### 99\. Learn React best practices from experts like Nadia, Dan, Josh, Kent, etc.

If you want to stay up-to-date with the best practices and learn tips, make sure to follow experts like:

* @adevnadia: https://x.com/adevnadia for advanced react tips
    
* @joshwcomeau: https://x.com/joshwcomeau
    
* @kentcdodds: https://x.com/kentcdodds
    
* @mattpocockuk: https://x.com/mattpocockuk for TypeScript tips
    
* @TejasKumar\_: https://x.com/TejasKumar\_
    
* @housecor: https://x.com/housecor
    
* or me 😅: https://x.com/\_ndeyefatoudiop
    

---

### 100\. Stay updated with the React ecosystem by subscribing to newsletters like This Week In React or ui.dev

React is a fast-moving ecosystem.

There are many tools, libraries, and best practices to keep up with.

To stay updated, make sure to subscribe to newsletters 💌 like:

* [This Week In React](https://thisweekinreact.com/) by @sebastienlorber
    
* [ui.dev](https://bytes.dev/)
    
* Etc.
    

---

### 101\. Engage with the React community on platforms like r/reactjs

The React community is fantastic.

You can learn a lot from other developers and share your knowledge.

So engage with the community on platforms like [r/reactjs](https://www.reddit.com/r/reactjs/).

---

That's a wrap 🎉.

Leave a comment 📩 to share your favorite tip (or add one).

And don't forget to drop a "💖🦄🔥".

If you like articles like this, join my **FREE** newsletter, [**FrontendJoy**](https://frontendjoy.substack.com/).

If you want daily tips, find me on [X/Twitter](https://twitter.com/_ndeyefatoudiop).