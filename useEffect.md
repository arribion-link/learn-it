# useEffect

is a React Hook that allows functional components to perform side effects. Side effects are operations that interact with the outside world or have an impact beyond the component's render output, such as data fetching, directly manipulating the DOM, setting up subscriptions, or timers.

## Key aspects of useEffect:
 
**Purpose:** It enables developers to synchronize a component with an external system or perform actions in response to changes within the component's state or props.

**Syntax:**

```js
JavaScript

    useEffect(() => {
      // Effect function: code to be executed
      // This runs after every render by default

      return () => {
        // Cleanup function: optional, runs before the effect re-runs or component unmounts
      };
    }, [dependencies]); // Optional dependency array
```

**Execution Timing:**
- The effect function runs after every render by default, including the initial mount.
- If a dependency array is provided, the effect will only re-run if any of the values in the array have changed since the last render.
- An empty dependency array ([]) means the effect will run only once after the initial mount and the cleanup function will run when the component unmounts.

**Cleanup Function:**
- The optional return function within useEffect is the cleanup function.
- It executes before the effect runs again (if dependencies change) and when the component unmounts.
- This is crucial for preventing memory leaks and managing resources, such as unsubscribing from event listeners, clearing timers, or canceling network requests.

### **Analogy to Class Component Lifecycle Methods:**

The main body of useEffect can be thought of as a combination of componentDidMount and componentDidUpdate (when dependencies change).
The cleanup function (return () => {}) is similar to componentWillUnmount.

### Common Use Cases:

- **Data Fetching:** Initiating API calls when a component mounts or when specific data dependencies change.
- **DOM Manipulation:** Directly interacting with the browser's DOM (e.g., setting document title).
- **Event Listeners:** Adding and removing event listeners to global objects like window or document.
- **Subscriptions:** Setting up and tearing down subscriptions to external services.
- **Timers:** Managing setTimeout or setInterval operations.