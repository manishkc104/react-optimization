<div align="center">
<h2 align="center">Optimizing Performance in a React Application</h2>
</div>

<details open>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#1-introduction">Introduction</a>
    </li>
     <li>
      <a href="#2-why-performance-optimization">Why Performance Optimization</a>
    </li>
     <li>
      <a href="#3-react-performance-optimizaton-techniques">React Performance Optimizaton Techniques</a>
      <ul>
        <li><a href="#1-memoization">Memoization</a></li>
        <li><a href="#2-windowing-or-list-virtualization">Windowing or List Virtualization</a></li>
         <li><a href="#3-lazy-loading-images">Lazing Loading Images</a></li>
      </ul>
    </li>
  </ol>
</details>

## 1. Introduction

Optimizing application performance is a critical aspect of developing web applications. Users expect applications to load quickly and respond to their interactions smoothly.

Internally, React uses several techniques to minimize the number of costly DOM operations required to update the UI. We are guaranteed a very fast UI by default. However as the application grows there are ways where we can still speed up the application with various performance optimization techniques, significantly enhancing the user experience by reducing load times and improving responsivenes.

## 2. Why Performance Optimization

Optimizing the performance is important in React Application for several reasons:

### Improved User Experience :

Faster application provides better user experience. User expect the web application to be reponsiveness and snappy rather than slow-loading and laggy, which leads to a poor user experience.

### SEO Benefits :

Page load time is a significant factor in search engine ranking algorithms, Faster application tend to rank higher in search engine results pages.Hence a well-optimized application will rank higher in search results, making it more visible to potential users.

### Reduced Bounce Rate :

Slow-loading web applicaton often results in higher bounce rates, users will likely leave and never return. By optimizing react application we can retain user's attention by loading quickly and providing seamless browsing experience.

### Competitive Advantage :

According to research done by Portent, a website that loads within one second has a conversion rate five times higher than a site that takes ten seconds to load. Therefore a fast and efficient application sets you apart from competitors whose application may be slower or less optimized.

## 3. React Performance Optimizaton Techniques

Below are the optimmization techniques that be added to make the react application fast.

### 1. Memoization :

Memoization in React is a performance optimization technique within functional components. It involves storing the results of expensive computations or function calls in cache to avoid redundant calculation when the same values are encountered. This approach improves the efficiency of the application.

In React, there are three techniques for memoization: `React.memo()`, `useMemo()` and `useCallback()`.

### Using `React.memo()`

React.memo is a higher order component provided by React that is used to memoize the functional components.It prevent the re-rendering of the component if the recieved props remain unchanged.

Here is an example on how React.memo() is used

```js
import React from "react";

const Greeting = ({ name }) => {
  return <div>Hello, {name}!</div>;
};

export default React.memo(Greeting);
```

In the above code we have a simple functional component `Greeting` that takes a `name` prop and displays a greeting message. We implemented `React.memo()` to wrap the `Greeting`, by wrapping it react will only re-render the `Greeting` component if the `name` props changes.

```js
import React, { useState } from "react";

const ParentComponent = () => {
  const [count, setCount] = React.useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <Greeting name="John" />
      <p>Count: {count}</p>
    </div>
  );
};

export default ParentComponent;
```

We have a parent component `ParentComponent` that maintains a count state using `useState()` . It renders the `Greeting` component along with a button to increment the count.

When you run this code and click the "Increment Count" button, you'll notice that the `Greeting` component is only rendered once. This is because `React.memo()` memoizes the rendering of the component based on its props. Since the props passed to `Greeting` remain the same (name="John"), React skips re-rendering the component on subsequent updates triggered by the count increment.

However, if you remove `React.memo()` and use `Greeting` directly in `ParentComponent`, you'll see that `Greeting` is rendered on every count increment, regardless of whether its props have changed or not. This demonstrates the performance optimization achieved by memoization with `React.memo()`.

### Using `useMemo()`

The `useMemo()` is a hook provided by React that memoizes the result a function call of an expensive computation.

Below is an example on how to use the useMemo hook.

```js
import React, { useMemo } from "react";

const SumComponent = ({ numbers }) => {
  const sum = useMemo(() => {
    return numbers.reduce((acc, num) => acc + num, 0);
  }, [numbers]);

  return (
    <div>
      <h2>Sum of Numbers</h2>
      <p>{sum}</p>
    </div>
  );
};

export default SumComponent;
```

In the above example we have a `SumComponent` functional component that takes a list of numbers as a prop. Inside this component, we use `React.useMemo()` to memoize the calculation of the sum of the numbers.

Here the dependency array of `useMemo()` is `[numbers]`, which means that the memoized value `sum` will only be recalculated when the numbers array changes.

```js
import React, { useState } from "react";

const App = () => {
  const [numberList, setNumberList] = useState([1, 2, 3, 4, 5]);

  const addRandomNumber = () => {
    const randomNumber = Math.floor(Math.random() * 10) + 1;
    setNumberList([...numberList, randomNumber]);
  };

  return (
    <div>
      <button onClick={addRandomNumber}>Add Random Number</button>
      <ul>
        {numberList.map((number, index) => (
          <li key={index}>{number}</li>
        ))}
      </ul>
      <SumComponent numbers={numberList} />
    </div>
  );
};

export default App;
```

The `App` component maintains a state `numberList` to hold the list of numbers. When the `Add Random Number` button is clicked, a random number is added to the list and the `SumComponent` is rendered with the `numberList` as its prop.

Taking the above example when we run this code and click the `Add Random Number` button, you'll notice that the `sum` is recalculated only when the list of numbers changes. The calculation of the sum is memoized using `React.useMemo()`, which helps in optimizing performance by avoiding unnecessary recalculations.

### Using `useCallback()`

The `useCallback()` hook in React is used to memoize a function instead of memoizing the function result. The `useCallback()` memoizes the function, ensuring it remains the same across re-renders as long as the dependencies haven't changed.

Below is the example where the hook is being used :

```js
import React, { useState, useCallback } from "react";

const ChildComponent = ({ onClick }) => {
  return <button onClick={onClick}>Click Me</button>;
};

const ParentComponent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <h2>Parent Component</h2>
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
};

export default ParentComponent;
```

We have a `ChildComponent` functional component that receives a onClick callback function as a prop. This component thenrenders a button that triggers the onClick callback when clicked.

We have a ParentComponent which renders the ChildComponent and alsi maintains a `count` state using the `useState()` hook.

Inside the `ParentComponent`, we have used `React.useCallback()` to memoize the `handleClick` callback function. The dependency `[count]` specifies that the callback function should be recalculated only if the value of count state changes.

Taking the above implementation when we run the code and click the "Click Me", only when the button is clicked the count value is updated. The handleClick callback function is memoized using `React.useCallback()`, preventing unnecessary recreations of the function on each render of the parent component. This helps in optimizing performance by avoiding unnecessary re-renders of child components that receive the callback function as a prop.

### 2. Windowing or List Virtualization

Windowing or List Virtualization refers to a technique used particularly in user interface design, to efficiently manage dealing with large number of items in a list. Rendering all the items at once can lead to slow performance and consume a significant amount of memory.

Windowing or List Virtualization addresses this problem by only rendering the portion of the items that is currently visible to the user on the screen. As the user scrolls through the list or window, the content is dynamically loaded and unloaded, ensuring that only the visible portion is actively present in the user interface.

There are various approaches and ways to implement list visualization. Some of the ways are by using libraries such as `React Virtualized`, `React Window` and `React Infinite Scroller `

In below example we are gonna see how list virtualization technique work with the help of `React Virtualized`

First we need to install the `React Virtualized Package`

```
npm install react-virtualized
```

In this example we will used the library to render a large list efficiently.We'll render a list of items containing numbers from `1 to 1000`.

```js
import React from "react";
import { List } from "react-virtualized";
import "react-virtualized/styles.css";

const LargeList = () => {
  const list = Array.from({ length: 1000 }, (_, index) => index + 1);

  const rowRenderer = ({ index, key, style }) => {
    return (
      <div key={key} style={style}>
        {list[index]}
      </div>
    );
  };

  return (
    <div style={{ height: 400, width: 300 }}>
      <List
        width={300}
        height={400}
        rowCount={list.length}
        rowHeight={30}
        rowRenderer={rowRenderer}
      />
    </div>
  );
};

export default LargeList;
```

In the above example we imported the `List` component from `react-virtualized` along with the default styles.For haivng a large list we generated a list of items containing numbers from 1 to 1000 using `Array.from`. The `rowRenderer` function defines how each row should be rendered.

Then we pass the necessary props to the `List` component

- `width` and `height` define the dimensions of the list container.
- `rowCount` specifies the total number of items in the list.
- `rowHeight` defines the height of each row in pixels.
- `rowRenderer` is the function responsible for rendering each row.

Now when you render the `LargeList` component, React Virtualized will efficiently render only the visible portion of the list, improving performance by avoiding unnecessary rendering during off-screen items.

### 3. Lazy Loading Images :

In React applications, optimizing performance often involves managing the rendering of resources like images efficiently. When an application contains numerous images, rendering them all in the DOM nodes at once can significantly slow down the initial page load time.

To address this issue lazy loading is used to delay the loading of images until they are needed or visible to the user instead of loading all the images on page load.

Lazy loading images in React can be achieved in several ways and with the help of various libraries.

In below example we are gonna see how lazy load images technique work with the help of ` react-lazyload`

First we need to install the `react-lazyload`

```
npm install react-lazyload
```

```js
import React from "react";
import LazyLoad from "react-lazyload";

const LazyImage = ({ src, alt }) => {
  return (
    <LazyLoad height={200} once>
      <img src={src} alt={alt} />
    </LazyLoad>
  );
};

const LazyImageLoaded = () => {
  return (
    <div>
      <h1>Lazy Loaded Images</h1>
      <LazyImage src="path_to_image" alt="Lazy Loaded Image" />
      <LazyImage src="path_to_another_image" alt="Another Lazy Loaded Image" />
      {/* Add more LazyImage components as needed */}
    </div>
  );
};

export default LazyImageLoaded;
```

First we define a functional component called LazyImage. We import the `LazyLoad` component from `react-lazyload`. Inside the LazyLoad component, we wrap an <img> element. This element will be rendered lazily, meaning it will only load when it's about to enter the viewport.

By using this code, you can implement lazy loading of images in your React application, improving performance by only loading images when they're needed. Remember to replace "path_to_image" and "path_to_another_image" with the actual paths to your images.
