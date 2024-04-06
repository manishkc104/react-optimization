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

### Using React.Memo()
