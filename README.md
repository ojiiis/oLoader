# 🟢 oLoader: Dynamic Content Assembler

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Stars](https://img.shields.io/github/stars/YOUR_USERNAME/oloader?style=social)](https://github.com/YOUR_USERNAME/oloader)

**oLoader** is a lightweight, zero-dependency JavaScript module that brings modern **component management** and a **clean application lifecycle** to static HTML environments. It empowers developers to assemble content, manage dependencies, and hydrate data with minimal boilerplate and maximum performance.

---

## ✨ Features

* <strong style="color: #28a745;">Template Instantiation:</strong> The core **`app.use()`** method loads HTML files as reusable **template objects** for highly efficient list rendering and component cloning.
* **Scoped Data Hydration:** **`element.paintObj()`** provides fast, attribute-based data binding scoped directly to a single element, preventing global data collisions.
* **Sequential Assembly:** Guarantees all queued CSS, HTML, and Scripts load and execute in the correct order using the **`app.load()`** lifecycle.
* **Dynamic View Management:** The **`element.put()`** method handles view swapping (clear/replace) and content appending/prepending with clear, intuitive flags.
* **Imperative Form Handling:** **`element.ifsubmit()`** simplifies forms by preventing default submission and parsing data into a clean object.
* **Clean Element Selection:** The global **`oG()`** utility provides concise, readable DOM querying.

---

## 🚀 Getting Started

### 1. Installation and Script Placement

Include `oloader.js` in the `<head>`. For best practice and reliable DOM interaction, place your main application logic (`app.js`) **just before the closing `</body>` tag**.

```html
<!DOCTYPE html>
<html>
<head>
    <title>My oLoader App</title>
    <script src="path/to/oloader.js"></script>
    
    </head>
<body>
    <div class="product-listing">Loading products...</div>
    
    <script src="path/to/app.js"></script> 
</body>
</html>
````

### 2\. Component-Based List Rendering Example (`app.use`)

This pattern demonstrates the efficiency of **`app.use()`** for dynamic list generation.

```javascript
// app.js

const app = oLoader();

app.load(async () => {
    // 1. Load the component as a REUSABLE master template object ONCE
    const productCardTemplate = await app.use('components/product-card.html');
    
    // 2. Fetch the data
    let response = await fetch('[https://dummyjson.com/products](https://dummyjson.com/products)');
    let data = await response.json();
    
    const listingContainer = oG('.product-listing');

    // 3. Loop, clone, paint, and append
    for (const product of data.products) {
        // CRITICAL: Clone the template object for a unique instance
        const newCardInstance = productCardTemplate.cloneNode(true);
        
        // Hydrate the unique instance with product data
        newCardInstance.paintObj(product);
        
        // Append the new card using the 'e' flag (End/Append)
        newCardInstance.put(listingContainer, 'e'); 
    }
});
```

-----

## 📚 Core API Reference

### `app` Assembly Methods

| Method | Role | Usage |
| :--- | :--- | :--- |
| **`oLoader()`** | Initialization | `const app = oLoader();` |
| **`app.head()`/`app.body()`**| Structure Queueing | `app.body('file.html', 'e');` |
| **`app.script()`** | Sequential Execution | `app.script('logic.js');` |
| **`app.use(file)`**| **Template Instantiation**| `const t = await app.use('card.html');` |
| **`app.load(cb)`** | Lifecycle | `app.load(async () => { ... });` |

### Dynamic Element Methods

These methods are available on **any HTML Element** (e.g., the result of `oG('#id')`).

| Method | Purpose | `put()` Position Flags |
| :--- | :--- | :--- |
| **`element.put(file, [pos])`**| Dynamic Content Insertion | `null` or Omitted: **Replace** (Clear content)<br>`'e'`: **Append** (End)<br>`'b'`: **Prepend** (Beginning) |
| **`element.paintObj(data)`**| Scoped Data Hydration | *(N/A)* |
| **`element.ifsubmit(cb)`**| Form Handling | *(N/A)* |

-----

## 🎨 Data Hydration

Data binding is **scoped** to the element the method is called on and uses attributes for mapping:

  * **Class:** Target elements must have `class="use-painter"`.
  * **Attribute:** The mapping key is set by `painter-data="keyName"`.

<!-- end list -->

```html
<div class="product-card">
    <h3 class="use-painter" painter-data="title"></h3> 
    <p class="use-painter" painter-data="description"></p>
    <img class="use-painter" painter-data="thumbnail" src="">
</div>
```

-----

## 🤝 Contributing

We welcome contributions\! Please feel free to open issues for bug reports or feature suggestions, and submit pull requests to help improve **oLoader**.

-----

## 📜 License

This project is licensed under the **MIT License**.

```
```
