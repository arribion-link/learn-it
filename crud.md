# **CRUD operations** (Create, Read, Update, Delete) in an Express.js application:

---

## 🆕 1. Create

- Receive input data from the client (e.g., via a form or API call).
- Validate the input to ensure required fields are present and correctly formatted.
- Insert the new record into the database.
- Return a success response with the created item or confirmation message.

---

## 📖 2. Read

- Accept query parameters or identifiers (e.g., ID) from the client.
- Fetch data from the database:
  - Single item by ID
  - Multiple items with filters or pagination
- Return the requested data in the response.

---

## ✏️ 3. Update

- Receive the item ID and updated data from the client.
- Validate the input and check if the item exists.
- Apply changes to the database record.
- Return the updated item or a success message.

---

## 🗑️ 4. Delete

- Receive the item ID from the client.
- Check if the item exists in the database.
- Remove the item from the database.
- Return a confirmation message or status code.

---

- **Jeff Arribion - codnify**
