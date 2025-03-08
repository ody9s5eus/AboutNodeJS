# 8. Understanding Sequelize

tags: #NodeJS #Backend #Database #Sequelize

---

## **What is Sequelize?**
Sequelize is a promise-based Node.js ORM for SQL databases, supporting MySQL, PostgreSQL, MariaDB, SQLite, and MSSQL.

---

## **Connecting to the Database**
Install Sequelize and database driver:
```sh
npm install sequelize mysql2
```
Set up Sequelize connection:
```js
const { Sequelize } = require('sequelize');
const sequelize = new Sequelize('shop', 'root', 'password', {
  host: 'localhost',
  dialect: 'mysql'
});
module.exports = sequelize;
```

---

## **Defining a Model**
Create a `Product` model:
```js
const { DataTypes } = require('sequelize');
const sequelize = require('../util/database');

const Product = sequelize.define('Product', {
  id: { type: DataTypes.INTEGER, autoIncrement: true, primaryKey: true, allowNull: false },
  title: { type: DataTypes.STRING, allowNull: false },
  price: { type: DataTypes.FLOAT, allowNull: false }
});

module.exports = Product;
```

---

## **Syncing JS Definitions to the Database**
Synchronize models with the database:
```js
sequelize.sync()
  .then(() => console.log('Database synced'))
  .catch(err => console.log(err));
```

---

## **Inserting Data & Creating a Product**
```js
Product.create({ title: 'Laptop', price: 1200 })
  .then(product => console.log(product))
  .catch(err => console.log(err));
```

---

## **Retrieving Data & Finding Products**
```js
Product.findAll()
  .then(products => console.log(products))
  .catch(err => console.log(err));
```

---

## **Getting a Single Product with "Where" Condition**
```js
Product.findOne({ where: { id: 1 } })
  .then(product => console.log(product))
  .catch(err => console.log(err));
```

---

## **Fetching Admin Products**
```js
exports.getAdminProducts = (req, res) => {
  Product.findAll()
    .then(products => res.render('admin/products', { products }))
    .catch(err => console.log(err));
};
```

---

## **Updating Products**
```js
Product.update({ price: 1400 }, { where: { id: 1 } })
  .then(() => console.log('Product updated'))
  .catch(err => console.log(err));
```

---

## **Deleting Products**
```js
Product.destroy({ where: { id: 1 } })
  .then(() => console.log('Product deleted'))
  .catch(err => console.log(err));
```

---

## **Creating a User Model**
```js
const User = sequelize.define('User', {
  id: { type: DataTypes.INTEGER, autoIncrement: true, primaryKey: true, allowNull: false },
  name: { type: DataTypes.STRING, allowNull: false }
});
module.exports = User;
```

---

## **Adding a One-to-Many Relationship**
```js
User.hasMany(Product);
Product.belongsTo(User);
```

---

## **Creating & Managing a Dummy User**
```js
User.create({ name: 'John Doe' })
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

---

## **Using Magic Association Methods**
```js
User.findByPk(1)
  .then(user => user.createProduct({ title: 'Phone', price: 800 }))
  .then(product => console.log(product))
  .catch(err => console.log(err));
```

---

## **Fetching Related Products**
```js
User.findByPk(1, { include: Product })
  .then(user => console.log(user.Products))
  .catch(err => console.log(err));
```

---

## **One-to-Many and Many-to-Many Relations**
```js
const Order = sequelize.define('Order', { id: { type: DataTypes.INTEGER, autoIncrement: true, primaryKey: true } });
User.hasMany(Order);
Order.belongsTo(User);
```

---

## **Creating & Fetching a Cart**
```js
const Cart = sequelize.define('Cart', { id: { type: DataTypes.INTEGER, autoIncrement: true, primaryKey: true } });
User.hasOne(Cart);
Cart.belongsTo(User);
```

---

## **Adding New Products to the Cart**
```js
Cart.create({ userId: 1 })
  .then(cart => cart.createProduct({ title: 'Tablet', price: 600 }))
  .then(product => console.log(product))
  .catch(err => console.log(err));
```

---

## **Adding Existing Products & Retrieving Cart Items**
```js
Cart.findOne({ where: { userId: 1 } })
  .then(cart => cart.addProduct(1))
  .then(() => console.log('Product added to cart'))
  .catch(err => console.log(err));
```

---

## **Deleting Related Items & Deleting Cart Products**
```js
Cart.findOne({ where: { userId: 1 } })
  .then(cart => cart.removeProduct(1))
  .then(() => console.log('Product removed from cart'))
  .catch(err => console.log(err));
```

---

## **Adding an Order Model**
```js
const Order = sequelize.define('Order', { id: { type: DataTypes.INTEGER, autoIncrement: true, primaryKey: true } });
User.hasMany(Order);
Order.belongsTo(User);
```

---

## **Storing Cart Items as Order Items**
```js
Order.create({ userId: 1 })
  .then(order => Cart.findOne({ where: { userId: 1 } }))
  .then(cart => cart.getProducts())
  .then(products => order.addProducts(products))
  .then(() => console.log('Order placed'))
  .catch(err => console.log(err));
```

---

## **Resetting the Cart & Fetching and Outputting Orders**
```js
Cart.findOne({ where: { userId: 1 } })
  .then(cart => cart.setProducts([]))
  .then(() => console.log('Cart cleared'))
  .catch(err => console.log(err));
```
