-----


# 🟢 oLoader: Dynamic Content Assembler

![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)
![GitHub stars](https://imgs.shields.io/github/stars/YourUsername/oLoader.svg?style=social)
![GitHub issues](https://img.shields.io/github/issues/YourUsername/oLoader.svg)

**oLoader** is a lightweight, zero-dependency JavaScript module designed to manage client-side content assembly, dependency ordering, and data hydration in a static, runtime environment.

It delivers **modern application structure without modern application overhead**, making it perfect for adding dynamic, component-based power to simple HTML files without needing a complex build step.

---

## ✨ Features at a Glance

* **Zero Overhead:** No Virtual DOM, no complex lifecycle. Pure imperative JavaScript.
* **Sequential Assembly:** Guarantees HTML, CSS, and Scripts load in the correct order.
* **Clean DOM Selection:** The global **`oG()`** utility simplifies element querying (`oG('#id')`).
* **Scoped Templating:** Element methods like **`elem.paintObj()`** provide fast, scoped data hydration directly on the element, preventing data collisions.
* **Dynamic View Swapping:** The **`elem.put()`** method allows seamless asynchronous loading and insertion of new HTML components.
* **Simple Forms:** **`elem.ifsubmit()`** handles form prevention and data parsing automatically.

---

## 🚀 Getting Started

### 1. Installation

Download the `oloader.js` file and include it in your main HTML document, followed by your application logic (`app.js`).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script src="path/to/oloader.js"></script>
    <script src="path/to/app.js"></script>
</head>
<body>
    <div id="app-container">Loading...</div>
</body>
</html>
````

### 2\. Core Assembly (`app.js`)

Use the `oLoader()` constructor to define your assets, and call `.load()` to execute the full assembly process.

```javascript
// app.js

const app = oLoader();

// 1. Queue all files
app.head('/assets/styles.css', 'e');
app.body('/components/main-layout.html', 'e');
app.script('/app/router.js');

// 2. Execute assembly and initialize runtime logic
app.load(() => {
    console.log('Application assembled and ready!');
    
    // Use the oG() selector to attach runtime logic
    oG('#main-form', (form) => {
        form.ifsubmit(({ data, location }) => {
            console.log(`Submitting form data to ${location}`, data);
        });
    });
});
```

-----

## 🎨 Scoped Data Hydration

Instead of using a global object, `oLoader` extends `HTMLElement` with scoped methods for data painting, ensuring data only lands where it's supposed to.

Elements to be hydrated must have the class **`.use-painter`** and the attribute **`painter-data`**.

### Component HTML

```html
<div id="user-profile-card">
    <h3 class="use-painter" painter-data="userName"></h3>
    
    <img class="use-painter" painter-data="avatarURL" src="" alt="Avatar">
</div>
```

### Application Logic

The search for `.use-painter` elements is limited to the element the method is called on (`#user-profile-card`).

```javascript
// Assume this runs after the component has been loaded via .put()
const profileData = { 
    userName: 'AlphaTrader', 
    avatarURL: '/img/alpha.jpg' 
};

// Select the element and paint data DIRECTLY on it.
oG('#user-profile-card').paintObj(profileData); 
```

### List Rendering Example

```javascript
// The list container must contain one element used as the template.
oG('#transaction-list').paintArray([
    { id: 1, amount: '$100' }, 
    { id: 2, amount: '$500' }
]);
```

-----

## 📚 Core API Reference

### Initial Assembly Methods (The `app` Instance)

| Method | Description |
| :--- | :--- |
| **`oLoader()`** | Initializes and returns the assembly instance. |
| **`app.head(file, pos)`** | Queues content for insertion into `<head>`. |
| **`app.body(file, pos)`** | Queues content for insertion into `<body>`. |
| **`app.script(file)`** | Queues a JS file for **sequential execution**. |
| **`app.load(callback)`** | Triggers fetching and runs the `callback` when **all** assets are ready. |

### Dynamic Element Methods

These methods are available on **any HTML Element** (e.g., elements returned by `oG()`).

| Method | Description |
| :--- | :--- |
| **`oG(selector, [cb])`** | **The Element Selector.** Returns an `HTMLElement` (for IDs) or an `Array` (for classes/tags). |
| **`elem.put(file, [pos], [cb])`** | Asynchronously fetches content and inserts it into the element. |
| **`elem.ifsubmit(callback)`** | Attaches a form handler, prevents default submission, and parses data into a clean object. |
| **`elem.paintObj(data)`** | **Scoped Templating.** Hydrates the DOM using an object. Search is limited to this element. |
| **`elem.paintArray(array)`** | **Scoped List Rendering.** Clones this element's first child for each item in the array. |

-----

## 🤝 Contributing

We welcome contributions\! Please read our [CONTRIBUTING.md](https://www.google.com/search?q=https://github.com/YourUsername/oLoader/blob/main/CONTRIBUTING.md) for guidelines.

-----

## 📜 License

oLoader is [Licensed under the MIT License](https://www.google.com/search?q=https://github.com/YourUsername/oLoader/blob/main/LICENSE).

```
```
