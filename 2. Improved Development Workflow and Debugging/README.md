# 2. Improved Development Workflow and Debugging

tags: #NodeJS #Backend #Debugging #NPM

---

## **Understanding NPM Scripts**
NPM scripts allow you to automate tasks like starting a server, running tests, or building your project.

Define scripts in `package.json`:
```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  }
}
```
Run a script:
```sh
npm run dev
```

---

## **Installing 3rd Party Packages**
Install dependencies globally or locally:
```sh
npm install express
npm install -g nodemon
```
Local packages go into `node_modules/`, while global packages can be used across projects.

---

## **Global Features vs Core Modules vs 3rd-party Modules**
- **Core Modules**: Built into Node.js (`fs`, `http`, `path`)
- **3rd-party Modules**: Installed via npm (`express`, `lodash`)
- **Global Features**: Available without requiring (`console`, `setTimeout`)

---

## **Using Nodemon for Auto Restarts**
Nodemon automatically restarts the server when file changes occur.

Install and use it:
```sh
npm install -g nodemon
nodemon server.js
```
Or define it in `package.json`:
```json
"scripts": {
  "dev": "nodemon server.js"
}
```

---

## **Global & Local npm Packages**
- **Global**: Installed with `-g`, accessible anywhere.
- **Local**: Installed without `-g`, used only in the project.

Example:
```sh
npm install -g nodemon  # Global
npm install axios       # Local
```

---

## **Understanding Different Error Types**
### **1. Syntax Errors**
Occurs when code is written incorrectly.
```js
console.log("Hello World" // Missing closing parenthesis
```
Fix: Ensure proper syntax.

### **2. Runtime Errors**
Occurs when executing code.
```js
const obj = null;
console.log(obj.name); // TypeError
```
Fix: Handle potential issues.

### **3. Logical Errors**
Code runs but produces incorrect results.
```js
function add(a, b) {
  return a - b; // Mistake in logic
}
```
Fix: Debug the logic.

---

## **Finding & Fixing Syntax Errors**
Use ESLint to catch syntax errors early:
```sh
npm install eslint --save-dev
npx eslint yourfile.js
```

---

## **Dealing with Runtime Errors**
Use `try...catch` to handle runtime errors gracefully:
```js
try {
  let result = riskyFunction();
} catch (error) {
  console.error("An error occurred:", error.message);
}
```

---

## **Using the Debugger**
### **1. Debugging with `debugger` Statement**
Insert `debugger;` in your code and run it with Node.js Inspector:
```js
function test() {
  let x = 10;
  debugger;
  console.log(x);
}
test();
```
Run:
```sh
node --inspect-brk app.js
```

### **2. Debugging in VS Code**
1. Open VS Code.
2. Add a breakpoint in your code.
3. Run `Debug` from the sidebar.

---

## **Restarting the Debugger Automatically**
Use Nodemon with `--inspect`:
```sh
nodemon --inspect server.js
```
This will restart debugging on file changes.

---

## **Changing Variables in the Debug Console**
While debugging in VS Code:
1. Pause execution at a breakpoint.
2. Open the debug console.
3. Modify variable values dynamically.
```sh
> myVar = 42
```

---

## **Summary**
- Use NPM scripts for automation.
- Install packages globally or locally.
- Understand different error types and debugging tools.
- Use Nodemon for automatic server restarts.
- Debug with `debugger` and VS Code for efficient development.
