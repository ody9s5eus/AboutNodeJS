# 10. Working with Mongoose

## **What is Mongoose?**
Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js, providing structure and schema validation for NoSQL databases. It simplifies interactions with MongoDB by offering features like middleware, schema definitions, and model relationships.

---

## **Connecting to the MongoDB Server with Mongoose**
To use Mongoose, first install it:
```sh
npm install mongoose
```

Then, establish a connection:
```js
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/shop', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
  .then(() => console.log('Mongoose connected'))
  .catch(err => console.log(err));
```

---

## **Creating the Product Schema**
Mongoose schemas define the structure of documents in a collection.
```js
const productSchema = new mongoose.Schema({
  title: { type: String, required: true },
  price: { type: Number, required: true }
});

const Product = mongoose.model('Product', productSchema);
```

---

## **Saving Data Through Mongoose**
```js
const product = new Product({ title: 'Laptop', price: 1200 });
product.save()
  .then(() => console.log('Product saved'))
  .catch(err => console.log(err));
```

---

## **Fetching All Products**
```js
Product.find()
  .then(products => console.log(products))
  .catch(err => console.log(err));
```

---

## **Fetching a Single Product**
```js
Product.findById('someId')
  .then(product => console.log(product))
  .catch(err => console.log(err));
```

---

## **Updating Products**
```js
Product.findByIdAndUpdate('someId', { price: 1400 }, { new: true })
  .then(updatedProduct => console.log(updatedProduct))
  .catch(err => console.log(err));
```

---

## **Deleting Products**
```js
Product.findByIdAndDelete('someId')
  .then(() => console.log('Product deleted'))
  .catch(err => console.log(err));
```

---

## **Adding and Using a User Model**
```js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true }
});
const User = mongoose.model('User', userSchema);
```

---

## **Using Relations in Mongoose**
```js
const orderSchema = new mongoose.Schema({
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  products: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Product' }]
});
const Order = mongoose.model('Order', orderSchema);
```

---

## **One Important Thing About Fetching Relations**
Mongoose allows population of referenced documents.
```js
Order.find().populate('user').populate('products')
  .then(order => console.log(order))
  .catch(err => console.log(err));
```

---

## **Working on the Shopping Cart**
```js
const cartSchema = new mongoose.Schema({
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  products: [{
    product: { type: mongoose.Schema.Types.ObjectId, ref: 'Product' },
    quantity: Number
  }]
});
const Cart = mongoose.model('Cart', cartSchema);
```

---

## **Loading the Cart**
```js
Cart.findOne({ user: 'userId' }).populate('products.product')
  .then(cart => console.log(cart))
  .catch(err => console.log(err));
```

---

## **Deleting Cart Items**
```js
Cart.updateOne({ user: 'userId' }, { $pull: { products: { product: 'productId' } } })
  .then(() => console.log('Product removed from cart'))
  .catch(err => console.log(err));
```

---

## **Creating & Getting Orders**
```js
const order = new Order({ user: 'userId', products: ['productId1', 'productId2'] });
order.save()
  .then(() => console.log('Order created'))
  .catch(err => console.log(err));
```

```js
Order.find({ user: 'userId' })
  .then(orders => console.log(orders))
  .catch(err => console.log(err));
```

---

## **Storing All Order Related Data**
Instead of just storing product references, you can store complete order details to maintain historical data.
```js
const orderSchema = new mongoose.Schema({
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  products: [{
    title: String,
    price: Number,
    quantity: Number
  }]
});
```

---

## **Clearing the Cart After Storing an Order**
```js
Order.create(orderData)
  .then(() => Cart.deleteOne({ user: 'userId' }))
  .then(() => console.log('Cart cleared after order'))
  .catch(err => console.log(err));
```

---

## **Getting & Displaying the Orders**
```js
Order.find({ user: 'userId' })
  .then(orders => console.log(orders))
  .catch(err => console.log(err));
```

---

This section provides a comprehensive guide on using Mongoose for MongoDB interactions in a structured and efficient way.