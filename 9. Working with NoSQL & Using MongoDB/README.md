# 9. Working with NoSQL & Using MongoDB

## **What is MongoDB?**
MongoDB is a NoSQL database that stores data in a flexible, JSON-like format.

---

## **Relations in NoSQL**
Unlike SQL databases, NoSQL databases like MongoDB do not use rigid table structures and instead rely on document-based relationships.

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

## **Making the "Edit" & "Delete" Buttons Work Again**
```js
db.collection('products').updateOne({ _id: new ObjectId('someId') }, { $set: { price: 1400 } })
  .then(() => console.log('Product updated'))
  .catch(err => console.log(err));

db.collection('products').deleteOne({ _id: new ObjectId('someId') })
  .then(() => console.log('Product deleted'))
  .catch(err => console.log(err));
```

---

## **Working on the Product Models to Edit Our Product**
Updating the schema to include edit functionality.

---

## **Finishing the "Update Product" Code**
Finalizing update logic.

---

## **One Note About Updating Products**
Ensure updates do not overwrite unintended fields.

---

## **Deleting Products**
```js
db.collection('products').deleteOne({ _id: new ObjectId('someId') })
  .then(() => console.log('Product deleted'))
  .catch(err => console.log(err));
```

---

## **Fixing the "Add Product" Functionality**
Ensuring product addition follows correct schema.

---

## **Creating New Users**
```js
db.collection('users').insertOne({ name: 'John Doe' })
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

---

## **Storing the User in Our Database**
Users can be stored as individual documents or embedded within product data.

---

## **Working on Cart Items & Orders**

### **Adding the "Add to Cart" Functionality**
```js
db.collection('carts').insertOne({ userId: new ObjectId('someUserId'), products: [] })
  .then(cart => console.log(cart))
  .catch(err => console.log(err));
```

---

### **Storing Multiple Products in the Cart**
```js
db.collection('carts').updateOne(
  { userId: new ObjectId('someUserId') },
  { $push: { products: { productId: new ObjectId('someProductId'), quantity: 1 } } }
);
```

---

### **Displaying the Cart Items**
```js
db.collection('carts').findOne({ userId: new ObjectId('someUserId') })
  .then(cart => console.log(cart))
  .catch(err => console.log(err));
```

---

### **Deleting Cart Items**
```js
db.collection('carts').updateOne(
  { userId: new ObjectId('someUserId') },
  { $pull: { products: { productId: new ObjectId('someProductId') } } }
);
```

---

### **Adding an Order**
```js
db.collection('orders').insertOne({ userId: new ObjectId('someUserId'), products: [/* cart items */] })
  .then(order => console.log(order))
  .catch(err => console.log(err));
```

---

### **Adding Relational Order Data**
Orders store a snapshot of the purchased products and their quantities at the time of checkout.

---

### **Getting Orders**
```js
db.collection('orders').find({ userId: new ObjectId('someUserId') }).toArray()
  .then(orders => console.log(orders))
  .catch(err => console.log(err));
```

---

### **Removing Deleted Items From the Cart**
```js
db.collection('carts').updateOne(
  { userId: new ObjectId('someUserId') },
  { $pull: { products: { productId: new ObjectId('deletedProductId') } } }
);
```

---
