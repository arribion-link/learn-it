When performing CRUD operations with MongoDB in an Express.js application, **Mongoose** is commonly used as an **Object Data Modeling (ODM)** library, simplifying interactions with the database. The methods for querying in Mongoose for CRUD operations are:

## Create (Insert):
Model.create(doc): Inserts a single document or an array of documents into the collection.
Model.insertMany(docs): Inserts multiple documents into the collection.

## Read (Retrieve):
Model.find(query, projection, options): Retrieves all documents matching the query. projection specifies which fields to include/exclude, and options can include sorting, limiting, skipping, etc.
Model.findOne(query, projection, options): Retrieves the first document matching the query.
Model.findById(id, projection, options): Retrieves a single document by its _id.

## Update:
Model.updateOne(filter, update, options): Updates the first document matching the filter with the specified update.
Model.updateMany(filter, update, options): Updates all documents matching the filter with the specified update.
Model.findByIdAndUpdate(id, update, options): Finds a document by _id and updates it.
Model.findOneAndUpdate(filter, update, options): Finds the first document matching the filter and updates it.

## Delete:
Model.deleteOne(filter): Deletes the first document matching the filter.
Model.deleteMany(filter): Deletes all documents matching the filter.
Model.findByIdAndDelete(id): Finds a document by _id and deletes it.
Model.findOneAndDelete(filter): Finds the first document matching the filter and deletes it.
Example Usage in Express.js Controller:
JavaScript

```js
const express = require('express');
const router = express.Router();
const Product = require('../models/Product'); // Assuming you have a Product Mongoose model

// Create a new product
router.post('/', async (req, res) => {
  try {
    const product = await Product.create(req.body);
    res.status(201).json(product);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

// Get all products
router.get('/', async (req, res) => {
  try {
    const products = await Product.find({});
    res.json(products);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// Get a single product by ID
router.get('/:id', async (req, res) => {
  try {
    const product = await Product.findById(req.params.id);
    if (!product) return res.status(404).json({ message: 'Product not found' });
    res.json(product);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// Update a product by ID
router.put('/:id', async (req, res) => {
  try {
    const product = await Product.findByIdAndUpdate(req.params.id, req.body, { new: true });
    if (!product) return res.status(404).json({ message: 'Product not found' });
    res.json(product);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

// Delete a product by ID
router.delete('/:id', async (req, res) => {
  try {
    const product = await Product.findByIdAndDelete(req.params.id);
    if (!product) return res.status(404).json({ message: 'Product not found' });
    res.json({ message: 'Product deleted' });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

module.exports = router;
```