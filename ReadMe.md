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

### Using React.memo()

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

### Using useMemo()

useMemo() is a hook provided by React that memoizes the result a function call of an expensive computation.

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
