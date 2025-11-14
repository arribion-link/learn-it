To pull data from a different file and map it on another page in ReactJS, you’ll typically follow these steps:

---

### 🗂️ Step 1: Create the Data File
Create a separate file (e.g., `data.js`) and export your data:

```js
// data.js
const users = [
  { id: 1, name: "Alice", age: 25 },
  { id: 2, name: "Bob", age: 30 },
  { id: 3, name: "Charlie", age: 22 },
];

export default users;
```

---

### 📄 Step 2: Import and Use the Data in Another Component/Page

In the page where you want to display the data (e.g., `UsersPage.js`), import the data and use `.map()` to render it:

```jsx
// UsersPage.js
import React from "react";
import users from "./data"; // adjust path as needed

const UsersPage = () => {
  return (
    <div>
      <h2>User List</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            {user.name} ({user.age} years old)
          </li>
        ))}
      </ul>
    </div>
  );
};

export default UsersPage;
```

---

### 🧭 Step 3: Route to the Page (if using React Router)

If you're using React Router, make sure the page is accessible:

```jsx
// App.js
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import UsersPage from "./UsersPage";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/users" element={<UsersPage />} />
      </Routes>
    </Router>
  );
}

export default App;
```

---

### ✅ Bonus Tips
- You can also fetch data from an API or JSON file using `fetch()` or `axios`.
- If the data is dynamic, consider using `useEffect()` and `useState()` to manage it.

