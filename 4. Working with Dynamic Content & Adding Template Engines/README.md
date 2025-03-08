# 4. Working with Dynamic Content & Adding Template Engines

tags: #NodeJS #Backend #Templating

---

## **Sharing Data Across Requests & Users**
In a stateless system like HTTP, data persistence is managed using sessions, cookies, or databases.
```js
const session = require('express-session');
app.use(session({ secret: 'secret', resave: false, saveUninitialized: true }));
```

---

## **Templating Engines - An Overview**
Templating engines allow dynamic HTML rendering using placeholders and logic within templates.
Common engines include **Pug, Handlebars, and EJS**.

---

## **Installing & Implementing Pug**
Install Pug:
```sh
npm install pug
```
Set it as the view engine:
```js
app.set('view engine', 'pug');
app.set('views', './views');
```

---

## **Outputting Dynamic Content**
Use variables in Pug:
```pug
html
  head
    title= title
  body
    h1 Welcome #{user}
```
Render the template:
```js
app.get('/', (req, res) => {
  res.render('index', { title: 'Home', user: 'Alice' });
});
```

---

## **Converting HTML Files to Pug**
Transform standard HTML to Pug syntax:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Home</title>
</head>
<body>
  <h1>Welcome</h1>
</body>
</html>
```
Becomes:
```pug
doctype html
html
  head
    title Home
  body
    h1 Welcome
```

---

## **Adding a Layout**
Define a base layout:
```pug
html
  head
    title= title
  body
    block content
```
Extend it in a template:
```pug
extends layout
block content
  h1 Welcome, #{user}
```

---

## **Finishing the Pug Template**
Complete the Pug setup by structuring pages and reusing layouts effectively.

---

## **Working with Handlebars**
Install Handlebars:
```sh
npm install express-handlebars
```
Set it up:
```js
const exphbs = require('express-handlebars');
app.engine('handlebars', exphbs.engine());
app.set('view engine', 'handlebars');
```
Create a template:
```handlebars
<h1>Welcome, {{user}}</h1>
```
Render it:
```js
app.get('/', (req, res) => {
  res.render('home', { user: 'Alice' });
});
```

---

## **Converting Our Project to Handlebars**
Modify views to use Handlebars syntax and partials.

---

## **Adding the Layout to Handlebars**
Define a layout:
```handlebars
<!DOCTYPE html>
<html>
<head>
  <title>{{title}}</title>
</head>
<body>
  {{{body}}}
</body>
</html>
```

---

## **Working with EJS**
Install EJS:
```sh
npm install ejs
```
Set up EJS:
```js
app.set('view engine', 'ejs');
```
Create a view:
```html
<h1>Welcome, <%= user %></h1>
```
Render the template:
```js
app.get('/', (req, res) => {
  res.render('home', { user: 'Alice' });
});
```

---

## **Working on the Layout with Partials**
Extract reusable components using EJS partials:
```html
<!-- header.ejs -->
<header>
  <h1>My Website</h1>
</header>
```
Include it in pages:
```html
<%- include('header') %>
<h1>Welcome, <%= user %></h1>
```

---

## **Summary**
- Templating engines simplify dynamic content rendering.
- Pug, Handlebars, and EJS each have unique syntax and benefits.
- Layouts and partials improve maintainability.
