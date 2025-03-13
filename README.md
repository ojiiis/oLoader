# oLoader

**oLoader** is a lightweight JavaScript framework for dynamically loading HTML, CSS, and JavaScript files into a webpage. It allows developers to insert content into the `<head>` or `<body>` with control over placement while ensuring scripts execute correctly.

## ✨ Features

- **Load HTML files dynamically** into the head or body.
- **Insert content at the beginning or end** of a target element.
- **Load external and inline scripts** while maintaining execution order.
- **Chaining support** for cleaner and more readable code.
- **Error handling mechanism** (to be added in future updates).

## 🚀 Installation
Simply include `oLoader.js` in your project:

```html
<script src="oLoader.js"></script>
```

Or use it as a module:

```js
import oLoader from './oLoader.js';
```

## 📌 Usage

### Load Content into `<head>`

```js
oLoader().head('headContent.html').load();
```
This inserts the content from `headContent.html` into the `<head>`.

### Load Content into `<body>`

```js
oLoader().body('pageContent.html').load();
```
This loads `pageContent.html` into the `<body>` at the end (default behavior).

### Insert Content at the Beginning or End

```js
// Insert content at the beginning of body
oLoader().body('header.html', 'b').load();

// Insert content at the end of body (default behavior)
oLoader().body('footer.html', 'e').load();
```

### Load JavaScript Files Dynamically

```js
oLoader().script('script.js').load();
```
This loads `script.js` and ensures it executes properly.

### Chain Multiple Load Requests

```js
oLoader()
    .head('metaTags.html')
    .body('navbar.html', 'b')
    .body('mainContent.html')
    .script('app.js')
    .load();
```
This ensures all assets are loaded in sequence.

### Handling Errors (Upcoming Feature)

```js
oLoader().onError((file, error) => {
    console.log(`Failed to load ${file}:`, error);
}).body('content.html').load();
```

## 🎯 Why Use oLoader?

- **Lightweight & Fast** – No dependencies, just pure JavaScript.
- **Control Content Placement** – Load HTML anywhere you need it.
- **Automatic Script Execution** – Inline and external scripts are handled properly.
- **Chaining Support** – Cleaner syntax for multiple load operations.

## 🛠 Future Improvements

- Advanced error handling with custom callbacks.
- Support for CSS file loading.
- Improved script execution for better performance.

## 📜 License

MIT License.

---

💡 **Contributions are welcome!** Feel free to open issues or submit pull requests.

