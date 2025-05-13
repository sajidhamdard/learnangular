## 📄 **What is `angular.json`?**

It is the **main configuration file** for your Angular project.
Angular CLI **reads this file** to know **how to build, serve, test, and configure your project**.

It is like the **"control panel"** for your whole project setup.

---

## 🚀 **What does `angular.json` control?**

(Real things it configures 👇)

| Task                   | Example                                                |
| ---------------------- | ------------------------------------------------------ |
| **Build config**       | Which files are included/excluded in build             |
| **Styles/scripts**     | Global CSS/SCSS/JS files                               |
| **Assets**             | Images/fonts etc.                                      |
| **File replacements**  | Use `environment.prod.ts` when building for production |
| **Output paths**       | Where final build files go                             |
| **Development server** | Configure host, port, SSL etc.                         |
| **Linting**            | Set up lint rules                                      |
| **Testing**            | Configure Karma/Jasmine etc.                           |
| **Default schematics** | CLI behavior (e.g., default style for new components)  |

---

## 🛠️ **Real Example parts of `angular.json`**

### 1️⃣ **Global styles** (SCSS, CSS)

```json
"styles": [
  "src/styles.scss"
]
```

This tells Angular to include `styles.scss` globally in all components (without import).

---

### 2️⃣ **Assets (Images, fonts)**

```json
"assets": [
  "src/favicon.ico",
  "src/assets"
]
```

This copies `src/assets` folder and favicon to the build output.

---

### 3️⃣ **File replacements (for environments)**

```json
"fileReplacements": [
  {
    "replace": "src/environments/environment.ts",
    "with": "src/environments/environment.prod.ts"
  }
]
```

When you run:

```
ng build --configuration production
```

👉 It swaps **environment.ts → environment.prod.ts** automatically.

---

### 4️⃣ **Output Path**

```json
"outputPath": "dist/task-manager"
```

Your built files (after `ng build`) go into **`dist/task-manager/`**

---

## ✅ **Why is `angular.json` important?**

Because **without it**:
❌ Your project won’t know
– how to build
– where to build
– what files to include
– how to run tests
– what global styles/assets to load
– how routing, environments, ports, etc. behave

It is **the backbone of Angular CLI commands**.

---

## 🎯 **Simple summary**

> `angular.json` = **Project configuration file** for build, serve, test, styles, assets, environments etc.

---

## ⚡ **Pro tip (useful in real projects)**

If you want to **add a global library** (like Bootstrap),
you just add in `angular.json` styles/scripts:

```json
"styles": [
  "src/styles.scss",
  "node_modules/bootstrap/dist/css/bootstrap.min.css"
],
"scripts": [
  "node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"
]
```

👉 No need to manually import in every component!
