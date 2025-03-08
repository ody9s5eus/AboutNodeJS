# 7. SQL Introduction

tags: #NodeJS #Backend #Database #SQL

---

## **Choosing a Database**
Selecting the right database depends on project requirements, scalability, and structure.
- **Relational (SQL):** Structured schema, ACID compliance.
- **NoSQL:** Flexible schema, high scalability.

---

## **NoSQL Introduction**
NoSQL databases store data in various formats:
- **Document-based (MongoDB)**
- **Key-Value stores (Redis, DynamoDB)**
- **Columnar (Cassandra)**
- **Graph databases (Neo4j)**

---

## **Comparing SQL and NoSQL**
| Feature | SQL | NoSQL |
|---------|-----|-------|
| Schema | Fixed | Flexible |
| Scalability | Vertical | Horizontal |
| Transactions | ACID | Eventual Consistency |

---

## **Setting Up MySQL**
Install MySQL and configure a database:
```sh
sudo apt update
sudo apt install mysql-server
mysql -u root -p
```

---

## **Connecting NodeJS App to the SQL Database**
Install MySQL driver for Node.js:
```sh
npm install mysql2
```
Establish a connection:
```js
const mysql = require('mysql2');
const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  database: 'shop',
  password: 'password'
});
module.exports = pool.promise();
```

---

## **Basic SQL & Creating a Table**
Define a `products` table:
```sql
CREATE TABLE products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  price DECIMAL(10,2) NOT NULL
);
```

---

## **Retrieving Data**
Fetch all products:
```js
const db = require('../util/database');
db.execute('SELECT * FROM products')
  .then(result => console.log(result))
  .catch(err => console.log(err));
```

---

## **Fetching Products**
Retrieve products dynamically:
```js
exports.getProducts = (req, res) => {
  db.execute('SELECT * FROM products')
    .then(([rows]) => res.render('shop', { products: rows }))
    .catch(err => console.log(err));
};
```

---

## **Inserting Data into the Database**
Insert a new product:
```js
exports.addProduct = (req, res) => {
  const { title, price } = req.body;
  db.execute('INSERT INTO products (title, price) VALUES (?, ?)', [title, price])
    .then(() => res.redirect('/'))
    .catch(err => console.log(err));
};
```

---

## **Fetching a Single Product with the "WHERE" Condition**
Get a product by ID:
```js
exports.getProductById = (req, res) => {
  const prodId = req.params.productId;
  db.execute('SELECT * FROM products WHERE id = ?', [prodId])
    .then(([rows]) => res.render('product-detail', { product: rows[0] }))
    .catch(err => console.log(err));
};
```

---

## **Summary**
- SQL databases ensure structured data handling.
- Node.js connects with MySQL using `mysql2`.
- Basic operations include inserting, retrieving, and filtering data with SQL queries.
