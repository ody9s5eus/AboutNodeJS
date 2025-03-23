# 11. Sessions & Cookies

## **What is a Cookie?**
A cookie is a small piece of data stored on the user's browser by a website. It helps websites remember information like login status and user preferences.

```js
// Setting a simple cookie
res.cookie('username', 'JohnDoe');
```

---

## **Adding the Request Driven Login Solution**
Using cookies for authentication:
```js
app.post('/login', (req, res) => {
  const { username } = req.body;
  res.cookie('username', username, { httpOnly: true });
  res.redirect('/dashboard');
});
```

---

## **Setting a Cookie**
```js
res.cookie('theme', 'dark', { maxAge: 900000, httpOnly: true });
```

---

## **Manipulating Cookies**
Updating a cookie:
```js
res.cookie('theme', 'light');
```

---

## **Configuring Cookies**
```js
res.cookie('sessionId', '12345', { secure: true, sameSite: 'strict' });
```

---

## **What is a Session?**
A session is a server-side storage mechanism that maintains user state between HTTP requests.

---

## **Initializing the Session Middleware**
Install and use express-session:
```sh
npm install express-session
```
```js
const session = require('express-session');
app.use(session({
  secret: 'mySecret',
  resave: false,
  saveUninitialized: false
}));
```

---

## **Using the Session Middleware**
```js
app.post('/login', (req, res) => {
  req.session.user = req.body.username;
  res.redirect('/dashboard');
});
```

---

## **Using MongoDB to Store Sessions**
```sh
npm install connect-mongodb-session
```
```js
const MongoDBStore = require('connect-mongodb-session')(session);
const store = new MongoDBStore({
  uri: 'mongodb://localhost:27017/sessions',
  collection: 'sessions'
});
app.use(session({
  secret: 'mySecret',
  resave: false,
  saveUninitialized: false,
  store: store
}));
```

---

## **Sessions & Cookies - A Short Summary**
- Cookies are client-side storage mechanisms.
- Sessions store data on the server and use a cookie to track the session ID.
- Use express-session for handling sessions.
- MongoDB can be used to store sessions for persistence.

---

## **Deleting a Cookie**
```js
res.clearCookie('username');
res.send('Cookie deleted');
```
