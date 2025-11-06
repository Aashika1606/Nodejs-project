# Nodejs-project{
  "name": "Wireless Headphones",
  "description": "Noise-cancelling over-ear headphones",
  "category": "Electronics",
  "price": 99.99,
  "stock": 45,
  "images": [
    "https://example.com/images/headphones1.jpg",
    "https://example.com/images/headphones2.jpg"
const express = require('express');
const mongoose = require('mongoose');
const dotenv = require('dotenv');
const Product = require('./models/Product');

dotenv.config();

const app = express();
app.use(express.json());

// Connect to MongoDB
mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log('MongoDB connected!'))
  .catch(err => console.error(err));

// Routes

// ➕ Create Product
app.post('/api/products', async (req, res) => {
  try {
    const product = await Product.create(req.body);
    res.status(201).json(product);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

// 📋 Get All Products
app.get('/api/products', async (req, res) => {
  const products = await Product.find();
  res.json(products);
});

// 🔍 Get One Product
app.get('/api/products/:id', async (req, res) => {
  try {
    const product = await Product.findById(req.params.id);
    if (!product) return res.status(404).json({ error: 'Product not found' });
    res.json(product);
  } catch {
    res.status(400).json({ error: 'Invalid ID' });
  }
});

// ✏️ Update Product
app.put('/api/products/:id', async (req, res) => {
  try {
    const updated = await Product.findByIdAndUpdate(req.params.id, req.body, { new: true });
    res.json(updated);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

// ❌ Delete Product
app.delete('/api/products/:id', async (req, res) => {
  try {
    await Product.findByIdAndDelete(req.params.id);
    res.json({ message: 'Product deleted' });
  } catch {
    res.status(400).json({ error: 'Invalid ID' });
  }
});

app.listen(process.env.PORT, () => console.log(`Server running on port ${process.env.PORT}`));
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
  name: { type: String, required: true },
  description: String,
  category: { type: String, required: true },
  price: { type: Number, required: true },
  stock: { type: Number, default: 0 },
  images: [String],
}, { timestamps: true });

module.exports = mongoose.model('Product', productSchema);
mkdir product-catalog
cd product-catalog
npm init -y
npm install express mongoose dotenv

