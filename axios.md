# Axios

 is a popular, promise-based HTTP client for JavaScript, used for making HTTP requests from both the browser and Node.js environments. It simplifies the process of sending asynchronous requests to REST endpoints and performing CRUD (Create, Read, Update, Delete) operations.

## Key Features and Usage:

**Promise-based:** 
- Axios leverages JavaScript Promises, allowing for straightforward handling of asynchronous operations using `.then()` for successful responses and `.catch()` for error handling. 
- It also supports async/await syntax for cleaner asynchronous code.

**HTTP Methods:** Axios provides methods for all common HTTP request types, including:

```
axios.get(url): For retrieving data.
axios.post(url, data): For sending new data.
axios.put(url, data): For updating existing data.
axios.delete(url): For deleting data.
```


**Request and Response Interceptors:**
- Interceptors allow you to modify requests before they are sent or responses before they are handled by `.then()` or `.catch()`. 
- This is useful for tasks like adding authentication headers to requests or transforming response data.
Error Handling: Axios provides robust error handling, distinguishing between network errors and HTTP status code errors, which can be handled in the `.catch()` block.
- Automatic JSON Transformation: Axios automatically transforms JSON data in requests and responses, simplifying data handling.

### Installation:

Axios can be installed using npm or yarn:
Code

```bash
npm install axios
# or
yarn add axios
```
Alternatively, it can be included via a CDN link in an HTML file.
Basic Usage Example (GET Request):
JavaScript
```js
axios.get('https://api.example.com/users/1')
  .then(response => {
    console.log(response.data); // Access the data payload
  })
  .catch(error => {
    console.error('Error fetching user:', error);
  });
```
Basic Usage Example (POST Request with async/await):
JavaScript
```js
async function createUser(userData) {
  try {
    const response = await axios.post('https://api.example.com/users', userData);
    console.log('User created:', response.data);
  } catch (error) {
    console.error('Error creating user:', error);
  }
}

createUser({ name: 'John Doe', email: 'john.doe@example.com' });
```

---

# Axios  configuration options

 that you can use when making HTTP requests, whether using the generic `axios(config)` syntax or shorthand methods like `axios.get()`. Below, I’ll list **all the commonly used Axios configuration options**, explain their purpose, and provide example parameters for each in both syntaxes (`axios()` and `axios.get()`). This is tailored for beginners to understand how to use these options effectively.

### Axios Configuration Options
These options can be passed in the configuration object to customize your HTTP request. Some are more commonly used, while others are for specific use cases.

1. **`url`** (Required)
   - The API endpoint to send the request to.
   - Type: `string`
   - Example: `"https://jsonplaceholder.typicode.com/posts/1"`

2. **`method`**
   - The HTTP method for the request.
   - Type: `string`
   - Common values: `"GET"`, `"POST"`, `"PUT"`, `"DELETE"`, `"PATCH"`, `"HEAD"`, `"OPTIONS"`
   - Default: `"GET"` (if using shorthand like `axios.get()`)
   - Example: `"GET"`

3. **`baseURL`**
   - A base URL to prepend to the `url` if the `url` is relative.
   - Type: `string`
   - Example: `"https://api.example.com"`

4. **`headers`**
   - Custom HTTP headers to include in the request.
   - Type: `object`
   - Example: `{ "Content-Type": "application/json", "Authorization": "Bearer abc123" }`

5. **`params`**
   - Query parameters to append to the URL (e.g., `?key=value`).
   - Type: `object`
   - Example: `{ userId: 1, sort: "asc" }` (becomes `?userId=1&sort=asc`)

6. **`data`**
   - The request body data, used for POST, PUT, PATCH, etc.
   - Type: `object`, `string`, `ArrayBuffer`, etc.
   - Example: `{ title: "New Post", body: "Content", userId: 1 }`

7. **`timeout`**
   - Time in milliseconds before the request times out.
   - Type: `number`
   - Default: `0` (no timeout)
   - Example: `5000` (5 seconds)

8. **`responseType`**
   - The expected format of the response data.
   - Type: `string`
   - Values: `"json"`, `"text"`, `"arraybuffer"`, `"blob"`, `"document"`, `"stream"`
   - Default: `"json"`
   - Example: `"json"`

9. **`responseEncoding`**
   - The encoding for the response (Node.js only).
   - Type: `string`
   - Default: `"utf8"`
   - Example: `"utf8"`

10. **`withCredentials`**
    - Indicates whether to send cookies and authentication headers in cross-origin requests.
    - Type: `boolean`
    - Default: `false`
    - Example: `true`

11. **`auth`**
    - Basic HTTP authentication credentials.
    - Type: `object`
    - Example: `{ username: "user", password: "pass" }`

12. **`xsrfCookieName`**
    - Name of the cookie used for CSRF protection.
    - Type: `string`
    - Default: `"XSRF-TOKEN"`
    - Example: `"my-csrf-token"`

13. **`xsrfHeaderName`**
    - Name of the HTTP header for CSRF token.
    - Type: `string`
    - Default: `"X-XSRF-TOKEN"`
    - Example: `"X-Custom-XSRF-Token"`

14. **`maxContentLength`**
    - Maximum size of the response body in bytes (Node.js only).
    - Type: `number`
    - Example: `2000` (2KB)
    - Default: `Infinity`

15. **`maxBodyLength`**
    - Maximum size of the request body in bytes (Node.js only).
    - Type: `number`
    - Example: `2000` (2KB)
    - Default: `Infinity`

16. **`maxRedirects`**
    - Maximum number of redirects to follow.
    - Type: `number`
    - Default: `5` (browser), `21` (Node.js)
    - Example: `3`

17. **`paramsSerializer`**
    - Custom function to serialize `params` into a query string.
    - Type: `function`
    - Example: `(params) => `qs.stringify(params, { arrayFormat: "brackets" })`` (requires `qs` library)

18. **`transformRequest`**
    - Function to modify the request data before sending.
    - Type: `function` or `array` of functions
    - Example: `[(data) => JSON.stringify(data)]`

19. **`transformResponse`**
    - Function to modify the response data before it’s passed to `.then()`.
    - Type: `function` or `array` of functions
    - Example: `[(data) => data.toUpperCase()]` (for text responses)

20. **`onUploadProgress`**
    - Callback to track upload progress (useful for file uploads).
    - Type: `function`
    - Example: `(progressEvent) => console.log(progressEvent.loaded / progressEvent.total)`

21. **`onDownloadProgress`**
    - Callback to track download progress.
    - Type: `function`
    - Example: `(progressEvent) => console.log(progressEvent.loaded / progressEvent.total)`

22. **`validateStatus`**
    - Function to determine if a status code is valid (if not, the request rejects).
    - Type: `function`
    - Default: `(status) => status >= 200 && status < 300`
    - Example: `(status) => status >= 200 && status <= 400`

23. **`cancelToken`**
    - Token to cancel the request using Axios’s `CancelToken`.
    - Type: `CancelToken`
    - Example: `axios.CancelToken.source().token`

### Example with All Options (Generic `axios()` Syntax)
Here’s a comprehensive example using the generic `axios()` syntax with realistic parameters for a GET request:

```javascript
import axios from "axios";
import qs from "qs"; // Optional: for paramsSerializer

axios({
  url: "/posts/1", // Relative URL (requires baseURL)
  method: "GET",
  baseURL: "https://jsonplaceholder.typicode.com",
  headers: {
    "Content-Type": "application/json",
    Authorization: "Bearer abc123",
  },
  params: {
    userId: 1,
    sort: "asc",
  },
  data: null, // Not used for GET, included for completeness
  timeout: 5000,
  responseType: "json",
  responseEncoding: "utf8", // Node.js only
  withCredentials: true,
  auth: {
    username: "user",
    password: "pass",
  },
  xsrfCookieName: "my-csrf-token",
  xsrfHeaderName: "X-Custom-XSRF-Token",
  maxContentLength: 2000, // Node.js only
  maxBodyLength: 2000, // Node.js only
  maxRedirects: 3,
  paramsSerializer: (params) => qs.stringify(params, { arrayFormat: "brackets" }),
  transformRequest: [(data, headers) => {
    console.log("Transforming request data");
    return data;
  }],
  transformResponse: [(data) => {
    console.log("Transforming response data");
    return data;
  }],
  onUploadProgress: (progressEvent) => {
    console.log(`Upload Progress: ${Math.round((progressEvent.loaded / progressEvent.total) * 100)}%`);
  },
  onDownloadProgress: (progressEvent) => {
    console.log(`Download Progress: ${Math.round((progressEvent.loaded / progressEvent.total) * 100)}%`);
  },
  validateStatus: (status) => status >= 200 && status <= 400,
  cancelToken: axios.CancelToken.source().token,
})
  .then((response) => {
    console.log("Data:", response.data);
  })
  .catch((error) => {
    console.error("Error:", error.message);
  });
```

### Example with All Options (Shorthand `axios.get()` Syntax)
The same options applied using the shorthand `axios.get()` syntax:

```javascript
import axios from "axios";
import qs from "qs"; // Optional: for paramsSerializer

axios.get("https://jsonplaceholder.typicode.com/posts/1", {
  headers: {
    "Content-Type": "application/json",
    Authorization: "Bearer abc123",
  },
  params: {
    userId: 1,
    sort: "asc",
  },
  timeout: 5000,
  responseType: "json",
  responseEncoding: "utf8", // Node.js only
  withCredentials: true,
  auth: {
    username: "user",
    password: "pass",
  },
  xsrfCookieName: "my-csrf-token",
  xsrfHeaderName: "X-Custom-XSRF-Token",
  maxContentLength: 2000, // Node.js only
  maxBodyLength: 2000, // Node.js only
  maxRedirects: 3,
  paramsSerializer: (params) => qs.stringify(params, { arrayFormat: "brackets" }),
  transformRequest: [(data, headers) => {
    console.log("Transforming request data");
    return data;
  }],
  transformResponse: [(data) => {
    console.log("Transforming response data");
    return data;
  }],
  onUploadProgress: (progressEvent) => {
    console.log(`Upload Progress: ${Math.round((progressEvent.loaded / progressEvent.total) * 100)}%`);
  },
  onDownloadProgress: (progressEvent) => {
    console.log(`Download Progress: ${Math.round((progressEvent.loaded / progressEvent.total) * 100)}%`);
  },
  validateStatus: (status) => status >= 200 && status <= 400,
  cancelToken: axios.CancelToken.source().token,
})
  .then((response) => {
    console.log("Data:", response.data);
  })
  .catch((error) => {
    console.error("Error:", error.message);
  });
```

### Notes for Beginners
- **Not all options are needed**: Most requests only use `url`, `method`, `headers`, `params`, or `data`. Others like `transformRequest` or `cancelToken` are for advanced use cases.
- **Environment differences**: Options like `responseEncoding`, `maxContentLength`, and `maxBodyLength` are Node.js-specific and ignored in browsers.
- **Testing**: Use a public API like `https://jsonplaceholder.typicode.com` to test these examples. Some options (e.g., `onUploadProgress`) are more relevant for file uploads or large data transfers.
- **CancelToken**: Requires creating a `CancelToken` source. Example:
  ```javascript
  const source = axios.CancelToken.source();
  // Use source.token in config
  // Cancel with: source.cancel("Request canceled");
  ```

### Practical Example (Minimal Configuration)
For a beginner, here’s a simpler GET request using both syntaxes with common options:
```javascript
// Generic axios()
axios({
  method: "GET",
  url: "https://jsonplaceholder.typicode.com/posts",
  params: { userId: 1 },
  headers: { "Content-Type": "application/json" },
})
  .then((response) => console.log(response.data))
  .catch((error) => console.error(error.message));

// Shorthand axios.get()
axios.get("https://jsonplaceholder.typicode.com/posts", {
  params: { userId: 1 },
  headers: { "Content-Type": "application/json" },
})
  .then((response) => console.log(response.data))
  .catch((error) => console.error(error.message));
```