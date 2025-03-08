# About NodeJS

tags: #NodeJS #Backend #JavaScript #ServerSide

---

## **What is NodeJS?**
Node.js is a runtime environment that allows JavaScript to run outside the browser. It is built on the V8 JavaScript engine and is commonly used for building scalable network applications.

- **Single-threaded, non-blocking model**: Uses event-driven architecture for efficiency.
- **Uses V8 engine**: Same engine that powers Google Chrome.
- **Ideal for backend development**: APIs, servers, real-time applications, and more.

---

## **Installing NodeJS**
To install Node.js, visit the official website: [nodejs.org](https://nodejs.org/)

### **Installation on Windows/macOS**
1. Download and run the installer.
2. Follow the setup instructions.
3. Verify installation:

```sh
node -v # Check Node.js version
npm -v  # Check npm version
```

### **Installation on Linux (Ubuntu/Debian)**
```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

---

## **Understanding the Role & Usage of NodeJS**
Node.js is commonly used for:

- **Web servers & APIs** (Express.js, Fastify)
- **Real-time applications** (Chat apps, WebSockets)
- **Command-line tools** (CLI applications)
- **Microservices architecture** (Distributed systems)
- **Build tools** (Webpack, Babel, Gulp)

Example: Running a simple server with Node.js
```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, Node.js!');
});

server.listen(3000, () => console.log('Server running on port 3000'));
```

---

## **Working with REPL vs Using Files**
### **Using Node.js REPL**
REPL (Read-Eval-Print Loop) allows direct execution of JavaScript in the terminal.

To start REPL:
```sh
node
```

Example:
```sh
> console.log('Hello, Node.js!');
Hello, Node.js!
```

### **Writing and Running Node.js Files**
Instead of using REPL, we can write code in `.js` files and execute them.

Create `app.js`:
```js
console.log('Running Node.js from a file');
```

Run it:
```sh
node app.js
```

---

## **Summary**
- Node.js allows JavaScript to run outside the browser.
- Can be installed on Windows, macOS, and Linux.
- Used for backend development, APIs, real-time applications, and more.
- Supports interactive REPL and script execution via `.js` files.

This chapter lays the foundation for understanding and using Node.js effectively.
