The existing documentation is well-structured and concise. To integrate the powerful, dynamic **`.put()`** method seamlessly, I will update the **"Core API Reference"** and insert a new dedicated section for **"Dynamic Element Methods"** right before **"Global Utilities."**

This final structure keeps the initial static assembly separate from the subsequent dynamic injection methods.

Here is the final, updated documentation:

-----

# 🟢 oLoader: Dynamic Content Assembler

**oLoader** is a lightweight, zero-dependency JavaScript module designed to manage client-side content assembly, dependency ordering, and data hydration in a static, runtime environment. It’s perfect for adding dynamic, component-based power to simple HTML files without needing a complex build step.

-----

## 💡 Features

  * **Runtime Assembly:** Asynchronously fetches and inserts HTML, CSS, and Scripts into the `<head>` or `<body>`.
  * **Guaranteed Script Order:** Scripts are executed sequentially, ensuring dependencies load correctly.
  * **Dynamic Component Management:** The **`.put()`** method enables **view swapping** and seamless loading state transitions *after* initial assembly.
  * **Global Data Hydration:** The integrated `window.paint` utility provides fast, attribute-based templating.
  * **Simple Lifecycle:** Uses a single `.load(callback)` method for clean initialization and cleanup.

-----

## 🚀 Getting Started

### 1\. Installation

Download the latest `oloader.js` and include it, followed by your application logic, in your main HTML file.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script src="path/to/oloader.js"></script>
    <script src="path/to/app.js"></script>
</head>
<body>
    </body>
</html>
```

### 2\. Basic Usage

Use the `oLoader()` constructor to create an assembly instance, queue your files, and call `.load()` to execute the process.

```javascript
// app.js

// 1. Initialize the assembly core
const app = oLoader();

// 2. Queue content and dependencies
app.head('/assets/styles.css', 'e');        // Load styles
app.body('/components/main-view.html', 'e'); // Load component structure
app.script('/app/router.js');               // Load logic (runs after assembly)

// 3. Execute assembly and run final initialization
app.load(() => {
    console.log('Application structure assembled and ready!');
    // Start router, hide loader, or initialize third-party libs here.
});
```

-----

## 🎨 Data Hydration (`window.paint`)

The global `paint` utility is used to inject data into loaded HTML templates. It searches for elements marked with the **`.use-painter`** class and the **`painter-data`** attribute.

### Templating

`paint` automatically targets attributes like `src` (for `<img>`) and `href` (for `<a>`). Otherwise, it updates `textContent`.

```html
<div class="card">
    <h3 class="use-painter" painter-data="userName"></h3>
    
    <img class="use-painter" painter-data="avatarURL" src="" alt="Avatar">
    
    <a class="use-painter" painter-data="profileID" href="/user/">View</a>
</div>
```

### Hydrating the Data

You typically call `paint.obj()` inside the `.load()` callback once data is available.

```javascript
// 1. Start fetching data concurrently
const userData = fetch('/api/user/current').then(res => res.json());

app.load(() => {
    userData.then(data => {
        // Hydrate the structure with data
        paint.obj({
            userName: data.name,
            avatarURL: data.image_url,
            profileID: data.id // The <a> href becomes /user/123
        });
    });
});
```

-----

## 📚 Core API Reference

The API is divided into methods for initial assembly and methods for dynamic interaction after the page is loaded.

### Initial Assembly Methods

| Method | Arguments | Description |
| :--- | :--- | :--- |
| **`oLoader()`** | *None* | Initializes and returns the assembly instance (`app`). |
| **`app.head(file, pos)`** | `string`, `string` ('b' or 'e') | Queues content for insertion into `<head>`. |
| **`app.body(file, pos)`** | `string`, `string` ('b' or 'e') | Queues content for insertion into `<body>`. |
| **`app.script(file)`** | `string` | Queues a JS file for **sequential execution**. |
| **`app.load(callback)`** | `function` | Triggers fetching and runs the `callback` when **all** assets are ready. |

\<hr\>

### Dynamic Element Methods

These methods become available on **any HTML Element** after `app.load()` has completed. They return a `Promise` and accept an optional callback.

| Method | Arguments | Description |
| :--- | :--- | :--- |
| **`element.put(file, [pos], [cb])`** | `string`, `string`, `function` | Asynchronously fetches content and inserts it into the element's DOM position. |

#### `[position]` Argument Details for `.put()`

| Value | Action | Clears Existing Content? | Use Case |
| :--- | :--- | :--- | :--- |
| **(Omitted or `null`)** | Replaces the element's **entire content**. | **YES** | **Seamless Loading Transition** (e.g., replacing a spinner with content). |
| **`e`** (End/Clear) | Appends content to the end. | **YES** | **View Swapping** (replaces old view with a new one). |
| **`E`** (End/No Clear) | Appends content to the end. | No | **Infinite Scroll/Pagination** (adding more items). |
| **`B`** (Begin/No Clear) | Prepends content to the beginning. | No | Inserting a new item at the top (e.g., a new chat message). |

```javascript
// Example: Swapping a loading state for a final component
const chartContainer = document.getElementById('chart-area');

// 1. Show the initial spinner
chartContainer.put('/components/spinner.html', 'e');

// 2. Load the final component, implicitly replacing the spinner
fetchChartData().then(data => {
    chartContainer.put('/components/final-chart.html', null, (newElement) => {
        // Use the new element to attach specific logic or data
        paint.obj(data, newElement); 
    });
});
```

\<hr\>

### Global Utilities

| Method | Arguments | Description |
| :--- | :--- | :--- |
| **`paint.obj(data, [element])`** | `Object`, `HTMLElement` | Hydrates the DOM using a single object. If `element` is provided, it scopes the search for `.use-painter` within that element. |
| **`paint.array(container, array)`** | `HTMLElement`, `Array` | Renders a list by cloning the container's first child for each item in the array. |

-----

## 🤝 Contributing

We welcome contributions\! Please read our [CONTRIBUTING.md](https://www.google.com/search?q=CONTRIBUTING.md) for guidelines on submitting pull requests, reporting bugs, and suggesting features.

-----

## 📜 License

oLoader is [Licensed under the MIT License](https://www.google.com/search?q=LICENSE).

-----
