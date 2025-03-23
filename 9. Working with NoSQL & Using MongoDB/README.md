# 9. Working with NoSQL & Using MongoDB

## **What is MongoDB?**
MongoDB is a NoSQL database that stores data in a flexible, JSON-like format. Unlike relational databases, it does not require a fixed schema and allows for dynamic data storage.

---

## **Why Use MongoDB?**
- **Scalability**: Supports horizontal scaling.
- **Flexibility**: Schema-less design allows easy modifications.
- **High Performance**: Optimized for read and write operations.
- **Ease of Use**: JSON-like documents make data handling intuitive.

---

## **Relations in NoSQL**
Unlike SQL databases, NoSQL databases like MongoDB do not use rigid table structures and instead rely on document-based relationships. There are different ways to model relationships in MongoDB:

### **1. Embedding Documents**
Storing related data within a single document.
```json
{
  "name": "John",
  "orders": [
    { "product": "Laptop", "price": 1200 },
    { "product": "Phone", "price": 800 }
  ]
}
```

### **2. Referencing Documents**
Using references between collections.
```json
{
  "name": "John",
  "orders": ["orderId1", "orderId2"]
}
```

---

## **Setting up MongoDB**
Install MongoDB and MongoDB Driver:
```sh
npm install mongodb
```

---

## **Creating the Database Connection**
```js
const { MongoClient } = require('mongodb');
const client = new MongoClient('mongodb://localhost:27017');
client.connect()
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.log(err));
```

---

## **Using the Database Connection**
```js
const db = client.db('shop');
```

---

## **Creating Products**
```js
db.collection('products').insertOne({ title: 'Laptop', price: 1200 })
  .then(result => console.log(result))
  .catch(err => console.log(err));
```

---

## **Understanding the MongoDB Compass**
MongoDB Compass is a GUI tool for visualizing and managing MongoDB data.

---

## **Fetching All Products**
```js
db.collection('products').find().toArray()
  .then(products => console.log(products))
  .catch(err => console.log(err));
```

---

## **Fetching a Single Product**
```js
db.collection('products').findOne({ _id: new ObjectId('someId') })
  .then(product => console.log(product))
  .catch(err => console.log(err));
```

---

## **Updating & Deleting Products**

### **Updating a Product**
```js
db.collection('products').updateOne({ _id: new ObjectId('someId') }, { $set: { price: 1400 } })
  .then(() => console.log('Product updated'))
  .catch(err => console.log(err));
```

### **Deleting a Product**
```js
db.collection('products').deleteOne({ _id: new ObjectId('someId') })
  .then(() => console.log('Product deleted'))
  .catch(err => console.log(err));
```

---

## **Creating & Managing Users**

### **Creating New Users**
```js
db.collection('users').insertOne({ name: 'John Doe' })
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

### **Fetching Users**
```js
db.collection('users').find().toArray()
  .then(users => console.log(users))
  .catch(err => console.log(err));
```

---

## **Working on Cart Items & Orders**

### **Adding the "Add to Cart" Functionality**
```js
db.collection('carts').insertOne({ userId: new ObjectId('someUserId'), products: [] })
  .then(cart => console.log(cart))
  .catch(err => console.log(err));
```

### **Storing Multiple Products in the Cart**
```js
db.collection('carts').updateOne(
  { userId: new ObjectId('someUserId') },
  { $push: { products: { productId: new ObjectId('someProductId'), quantity: 1 } } }
);
```

### **Displaying the Cart Items**
```js
db.collection('carts').findOne({ userId: new ObjectId('someUserId') })
  .then(cart => console.log(cart))
  .catch(err => console.log(err));
```

### **Deleting Cart Items**
```js
db.collection('carts').updateOne(
  { userId: new ObjectId('someUserId') },
  { $pull: { products: { productId: new ObjectId('someProductId') } } }
);
```

---

### **Placing an Order**
```js
db.collection('orders').insertOne({ userId: new ObjectId('someUserId'), products: [/* cart items */] })
  .then(order => console.log(order))
  .catch(err => console.log(err));
```

### **Fetching Orders**
```js
db.collection('orders').find({ userId: new ObjectId('someUserId') }).toArray()
  .then(orders => console.log(orders))
  .catch(err => console.log(err));
```

### **Removing Deleted Items From the Cart**
```js
db.collection('carts').updateOne(
  { userId: new ObjectId('someUserId') },
  { $pull: { products: { productId: new ObjectId('deletedProductId') } } }
);
```

---

## **Best Practices for MongoDB**
- **Use Indexing**: Improves query performance.
- **Avoid Large Documents**: Keep document sizes manageable.
- **Use Proper Data Modeling**: Choose between embedding and referencing wisely.
- **Optimize Queries**: Use projections to limit retrieved fields.
- **Implement Data Validation**: Use schemas to enforce data integrity.

---
