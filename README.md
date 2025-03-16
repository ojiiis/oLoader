# oLoader

**oLoader** is a lightweight JavaScript framework for dynamically loading external content into your web pages. It enables you to queue up global content (such as headers, footers, and scripts) that are only added to the DOM when you call the final `load()` method. In addition, it provides immediate content loading for individual elements using the `put()` method.

> **Note:**  
> - Use `instance.head()` to insert content directly into the `<head>` tag (for meta tags, styles, etc.).  
> - Use `instance.body(file, 'b')` to insert content at the beginning of the `<body>` (ideal for a global header or navigation bar).  
> - Use `instance.body(file, 'e')` to insert content at the end of the `<body>` (suitable for a global footer).  
> - All scripts must be loaded via `instance.script()` or the element’s `put()` method to avoid unwanted side effects.

## Features

- **Deferred Content Loading:**  
  Queue global content (HTML and scripts) and update the DOM only when `load()` is called.
  
- **Dynamic DOM Insertion:**  
  Insert content into `<head>` or `<body>` (with options to prepend or append).

- **Script Extraction & Execution:**  
  Automatically extract external and inline `<script>` tags from loaded content and execute them in the correct order.

- **Custom Preloader Support:**  
  Use `beforeLoad()` to display a customizable loading element until all queued content is processed.

- **Element-Level Immediate Loading:**  
  After calling `load()`, every DOM element gains a `put()` method to immediately load content into that element without waiting for queued operations.

## Installation

Include the `oLoader` script in your project. For example, if it’s hosted on GitHub Pages, add the following to your HTML:

```html
<script src="https://ojiiis.github.io/oLoader/"></script>
```

Then initialize oLoader in your code:

```js
const loader = oLoader();
```

## Usage

### 1. Global Content Loading (Deferred)

oLoader queues content (HTML and scripts) and only inserts it into the DOM when you call `load()`. For example:

```js
oLoader()
  .beforeLoad("<div>Loading, please wait...</div>")  // Displays a preloader
  .head("meta.html")     // Loads meta tags, styles, etc. into the <head>
  .body("navbar.html", "b")  // Loads a navigation bar at the beginning of the <body>
  .body("mainContent.html")  // Loads main content (appended by default)
  .body("footer.html", "e")  // Loads a footer at the end of the <body>
  .script("analytics.js")    // Loads an external script
  .load(() => {
      console.log("All global content loaded!");
  });
```

In this example:
- `head("meta.html")` inserts content into the `<head>` tag.
- `body("navbar.html", "b")` inserts the navbar at the beginning of the `<body>`, serving as a global header.
- `body("footer.html", "e")` appends the footer at the end of the `<body>`.
- The preloader set by `beforeLoad()` is removed automatically when `load()` finishes.

### 2. Element-Level Immediate Loading with `put()`

After the global content has been loaded (i.e. after calling `load()`), any DOM element gains a `put()` method for immediate content insertion. For example:

```js
// Immediately load content into a specific element:
document.getElementById("sidebar").put("sidebar.html");
```

The `put()` method works as follows:
- It temporarily removes all `<script>` tags from the document body.
- Loads and inserts new content into the target element.
- Re-adds the previously removed scripts to ensure they still execute.

### 3. Using the Instance Methods

All instance methods (`head()`, `body()`, `script()`) queue up content to be loaded globally, then the final `load()` method processes that queue. For example:

```js
const loader = oLoader();

loader
  .beforeLoad("<div>Loading...</div>")
  .head("headerContent.html")
  .body("globalNavbar.html", "b")  // Global header loaded at beginning
  .body("pageContent.html")        // Main page content appended
  .body("globalFooter.html", "e")  // Global footer appended
  .script("appLogic.js")
  .load(() => {
      console.log("Page loaded successfully!");
  });
```

## Example Multi-Page App

Below is an example of a multi-page app that leverages oLoader to dynamically load global header/footer and page-specific content:

### Folder Structure
```
/my-app
  ├── index.html
  ├── header.html        // Global header (e.g., navigation)
  ├── footer.html        // Global footer
  ├── home.html          // Home page content
  ├── about.html         // About page content
  ├── contact.html       // Contact page content
  ├── oLoader.js         // Your oLoader script
  ├── app.js             // Your app's initialization script
  └── styles.css         // App styles
```

### index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Multi-Page App</title>
    <link rel="stylesheet" href="styles.css">
    <script src="oLoader.js"></script>
</head>
<body>
    <div id="content">
        <!-- Dynamic content will be loaded here -->
    </div>
    <script src="app.js"></script>
</body>
</html>
```

### app.js
```js
const loader = oLoader();

// Load global header and footer on initial load
loader
  .head("header.html")         // You could also use .body("header.html", "b") for a global header
  .body("footer.html", "e")      // Loads footer at the end of the body
  .load(() => {
      console.log("Global content loaded.");
  });

// Navigate between pages by updating the #content element immediately:
function navigate(page) {
    document.getElementById("content").put(page);
}

// Example: navigate when buttons in your header (from header.html) are clicked
document.getElementById("navHome").addEventListener("click", () => {
    navigate("home.html");
});
document.getElementById("navAbout").addEventListener("click", () => {
    navigate("about.html");
});
document.getElementById("navContact").addEventListener("click", () => {
    navigate("contact.html");
});
```

### styles.css
```css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}
#content {
    padding: 20px;
}
```

## Contributing

Contributions are welcome! Please open issues or pull requests on [GitHub](https://github.com/ojiiis/oLoader).

## License

This project is licensed under the MIT License.

---

For more details, documentation, and updates, please refer to the [oLoader GitHub repository](https://github.com/ojiiis/oLoader).

Happy coding! 🚀

