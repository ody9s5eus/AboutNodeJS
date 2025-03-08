# 3. Working with Express.js

tags: #NodeJS #Backend #ExpressJS

---

## **What is Express.js?**
Express.js is a minimal and flexible Node.js web application framework that provides robust features for building web and mobile applications.

---

## **Installing Express.js**
To install Express.js, run:
```sh
npm install express
```
Then, create a basic Express server:
```js
const express = require('express');
const app = express();

app.listen(3000, () => console.log('Server running on port 3000'));
```

---

## **Adding Middleware**
Middleware functions are executed in the request-response cycle.
```js
app.use((req, res, next) => {
  console.log('Middleware executed');
  next();
});
```

---

## **How Middleware Works**
Middleware functions execute sequentially and can modify `req` and `res`.
```js
app.use((req, res, next) => {
  req.currentTime = new Date().toISOString();
  next();
});
app.get('/', (req, res) => {
  res.send(`Current Time: ${req.currentTime}`);
});
```

---

## **Express.js - Looking behind the Scenes**
Express.js simplifies Node.js’s built-in `http` module.
```js
const http = require('http');
const server = http.createServer((req, res) => {
  res.end('Hello World');
});
server.listen(3000);
```
Express abstracts this complexity into simpler methods.

---

## **Handling Different Routes**
Define routes for handling different HTTP methods.
```js
app.get('/', (req, res) => res.send('Home Page'));
app.post('/submit', (req, res) => res.send('Form Submitted'));
```

---

## **Parsing Incoming Requests**
To handle JSON data:
```js
app.use(express.json());
app.post('/data', (req, res) => {
  res.send(req.body);
});
```

---

## **Limiting Middleware Execution to POST Requests**
Use method-specific middleware:
```js
app.post('/log', (req, res, next) => {
  console.log('Logging POST request');
  next();
}, (req, res) => {
  res.send('Logged');
});
```

---

## **Using Express Router**
Use `express.Router()` to modularize routes.
```js
const router = express.Router();
router.get('/user', (req, res) => res.send('User Page'));
app.use(router);
```

---

## **Adding a 404 Error Page**
Handle undefined routes.
```js
app.use((req, res) => {
  res.status(404).send('Page Not Found');
});
```

---

## **Filtering Paths**
Match specific patterns.
```js
app.get('/user/:id', (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});
```

---

## **Creating & Serving HTML Pages**
Serve static HTML files.
```js
const path = require('path');
app.use(express.static(path.join(__dirname, 'public')));
```

---

## **Returning a 404 Page**
Serve a custom error page.
```js
app.use((req, res) => {
  res.status(404).sendFile(path.join(__dirname, 'public', '404.html'));
});
```

---

## **Using a Helper Function for Navigation**
Centralize navigation logic.
```js
function navigateTo(route) {
  return `<a href="${route}">${route}</a>`;
}
```

---

## **Styling our Pages**
Use CSS files for styling.
```css
body { font-family: Arial, sans-serif; }
```
Place styles in `/public/styles.css` and reference in HTML.

---

## **Serving Files Statically**
Host images, CSS, and JavaScript files.
```js
app.use(express.static('public'));
```
Now you can access `http://localhost:3000/styles.css` directly.

---

## **Summary**
- Express.js simplifies backend development.
- Middleware functions process requests.
- Routing, request parsing, and error handling are essential features.
- Static files and templating improve frontend integration.
