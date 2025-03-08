# 5. The Model View Controller (MVC)

tags: #NodeJS #Backend #MVC

---

## **What is the MVC?**
The Model-View-Controller (MVC) pattern is a software design pattern that separates an application into three interconnected components:
- **Model**: Handles data and business logic.
- **View**: Displays the user interface.
- **Controller**: Manages user input and interactions.

This separation improves code organization and maintainability.

---

## **Adding Controllers**
Controllers handle incoming requests and return appropriate responses. Example of a basic controller:
```js
exports.getProducts = (req, res) => {
  res.render('products', { title: 'Products' });
};
```

---

## **Finishing the Controllers**
Structure controllers into separate files for better maintainability:

**controllers/productController.js**
```js
exports.getProducts = (req, res) => {
  res.render('products', { title: 'Products', products: [] });
};
```

**routes/productRoutes.js**
```js
const express = require('express');
const productController = require('../controllers/productController');
const router = express.Router();

router.get('/products', productController.getProducts);

module.exports = router;
```

---

## **Adding a Product Model**
A model represents the data structure of an entity:

**models/product.js**
```js
class Product {
  constructor(title) {
    this.title = title;
  }

  save() {
    // Save product logic
  }

  static fetchAll() {
    return [];
  }
}

module.exports = Product;
```

---

## **Storing Data in Files via the Model**
Instead of using a database, data can be stored in JSON files:
```js
const fs = require('fs');
const path = require('path');

const filePath = path.join(__dirname, '..', 'data', 'products.json');

class Product {
  constructor(title) {
    this.title = title;
  }

  save() {
    fs.readFile(filePath, (err, data) => {
      let products = [];
      if (!err) {
        products = JSON.parse(data);
      }
      products.push(this);
      fs.writeFile(filePath, JSON.stringify(products), (err) => {
        console.log(err);
      });
    });
  }
}

module.exports = Product;
```

---

## **Fetching Data from Files via the Model**
Retrieving stored data from a file:
```js
static fetchAll(cb) {
  fs.readFile(filePath, (err, data) => {
    if (err) {
      cb([]);
    } else {
      cb(JSON.parse(data));
    }
  });
}
```

---

## **Refactoring the File Storage Code**
Refactoring improves code efficiency and readability by using helper functions:
```js
const getProductsFromFile = (cb) => {
  fs.readFile(filePath, (err, data) => {
    if (err) {
      cb([]);
    } else {
      cb(JSON.parse(data));
    }
  });
};

class Product {
  constructor(title) {
    this.title = title;
  }

  save() {
    getProductsFromFile((products) => {
      products.push(this);
      fs.writeFile(filePath, JSON.stringify(products), (err) => {
        console.log(err);
      });
    });
  }

  static fetchAll(cb) {
    getProductsFromFile(cb);
  }
}

module.exports = Product;
```

---

## **Summary**
- MVC separates concerns for better organization.
- Controllers handle routing logic.
- Models store and retrieve data.
- File-based storage provides a lightweight data handling approach.
- Refactoring enhances maintainability.
