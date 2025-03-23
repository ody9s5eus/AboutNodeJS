# 12. Adding Authentication

## **What is Authentication?**
Authentication is the process of verifying the identity of a user before granting access to protected resources.

```js
// Example of user authentication flow
if (user.username === inputUsername && user.password === inputPassword) {
  console.log('User authenticated');
}
```

---

## **How is Authentication Implemented?**
Authentication typically involves:
1. User registration
2. Password encryption
3. Storing user credentials securely
4. User login with credential verification
5. Session or token-based authentication

---

## **Implementing an Authentication Flow**
```js
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  User.findOne({ username }).then(user => {
    if (!user) {
      return res.status(401).send('Invalid username or password');
    }
    res.redirect('/dashboard');
  });
});
```

---

## **Encrypting Passwords**
Use bcrypt to hash passwords:
```sh
npm install bcryptjs
```
```js
const bcrypt = require('bcryptjs');
const saltRounds = 10;

bcrypt.hash('myPassword', saltRounds, (err, hash) => {
  console.log('Hashed Password:', hash);
});
```

---

## **Adding the Signin Functionality**
```js
app.post('/signin', async (req, res) => {
  const { username, password } = req.body;
  const user = await User.findOne({ username });
  if (!user || !(await bcrypt.compare(password, user.password))) {
    return res.status(401).send('Invalid credentials');
  }
  req.session.user = user;
  res.redirect('/dashboard');
});
```

---

## **Working on Route Protection**
```js
const isAuth = (req, res, next) => {
  if (!req.session.user) {
    return res.redirect('/login');
  }
  next();
};
```

---

## **Using Middleware to Protect Routes**
```js
app.get('/dashboard', isAuth, (req, res) => {
  res.send('Welcome to your dashboard');
});
```

---

## **Understanding CSRF Attacks**
Cross-Site Request Forgery (CSRF) is an attack where unauthorized commands are sent from a trusted user.

---

## **Using a CSRF Token**
```sh
npm install csurf
```
```js
const csrf = require('csurf');
app.use(csrf());
```

---

## **Adding CSRF Protection**
```js
app.post('/update-profile', (req, res) => {
  res.send(`Form submitted with CSRF token: ${req.body._csrf}`);
});
```

---

## **csurf() Alternatives**
- JWT-based authentication
- SameSite cookies
- Custom middleware for validating requests

---

## **Providing User Feedback**
Use flash messages to provide feedback on authentication:
```sh
npm install connect-flash
```
```js
const flash = require('connect-flash');
app.use(flash());
```

---

## **Finishing the Flash Messages**
```js
app.post('/login', (req, res) => {
  req.flash('success', 'Login successful!');
  res.redirect('/dashboard');
});
```
