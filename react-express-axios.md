React applications fetch data from Express.js (or any other backend) using various HTTP methods, typically within functional components leveraging hooks like useEffect and useState. The fetch API or a library like Axios are commonly used for making these requests.

## 1. GET (Retrieve Data)

JavaScript

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Or use the native fetch API

function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get('/api/items'); // Replace with your Express endpoint
        setData(response.data);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };
    fetchData();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <h1>Fetched Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default DataFetcher;
```

---

## 2. POST (Create Data)

JavaScript

```jsx
import React, { useState } from 'react';
import axios from 'axios';

function CreateItemForm() {
  const [itemName, setItemName] = useState('');
  const [message, setMessage] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post('/api/items', { name: itemName });
      setMessage(response.data.message); // Assuming Express sends a message
      setItemName('');
    } catch (err) {
      setMessage(`Error: ${err.message}`);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={itemName}
        onChange={(e) => setItemName(e.target.value)}
        placeholder="Enter item name"
      />
      <button type="submit">Create Item</button>
      {message && <p>{message}</p>}
    </form>
  );
}

export default CreateItemForm;
```

## 3. PUT/PATCH (Update Data)

JavaScript

```jsx
import React, { useState } from 'react';
import axios from 'axios';

function UpdateItemForm({ itemId }) {
  const [newItemName, setNewItemName] = useState('');
  const [message, setMessage] = useState('');

  const handleUpdate = async () => {
    try {
      const response = await axios.put(`/api/items/${itemId}`, { name: newItemName }); // Or PATCH
      setMessage(response.data.message);
    } catch (err) {
      setMessage(`Error: ${err.message}`);
    }
  };

  return (
    <div>
      <input
        type="text"
        value={newItemName}
        onChange={(e) => setNewItemName(e.target.value)}
        placeholder="New item name"
      />
      <button onClick={handleUpdate}>Update Item</button>
      {message && <p>{message}</p>}
    </div>
  );
}

export default UpdateItemForm;
```

## 4. DELETE (Remove Data)

JavaScript

```jsx
import React, { useState } from 'react';
import axios from 'axios';

function DeleteItemButton({ itemId }) {
  const [message, setMessage] = useState('');

  const handleDelete = async () => {
    try {
      const response = await axios.delete(`/api/items/${itemId}`);
      setMessage(response.data.message);
    } catch (err) {
      setMessage(`Error: ${err.message}`);
    }
  };

  return (
    <div>
      <button onClick={handleDelete}>Delete Item</button>
      {message && <p>{message}</p>}
    </div>
  );
}

export default DeleteItemButton;
```