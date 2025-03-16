# oLoader

**oLoader** is a lightweight JavaScript framework for dynamically loading external content into your web pages. It allows you to queue up global content (such as headers, footers, and scripts) that are only added to the DOM when you call the final `load()` method. Additionally, after the global content is loaded, every DOM element gains a `put()` method that lets you immediately load content into that element.

> **Important:**  
> - **All file paths must be specified as either relative (`./file.html`) or absolute (`https://example.com/file.html`).**  
>   For example, instead of `"index.html"`, use `"./index.html"` or `"https://example.com/index.html"`.  
> - **`instance.head()` inserts content into the `<head>` tag**—use it for meta tags, styles, and scripts.  
> - **`instance.body()` inserts content into the `<body>` tag**—use it for visible elements like headers, footers, and main content.  
> - **Scripts inside loaded content are automatically extracted and executed.**  
> - **Once `load()` is called, all queued content is injected into the DOM and the preloader (if any) is removed.**  

## Features

- **Deferred Global Content Loading:**  
  Queue global content (HTML and scripts) and inject it into the `<head>` or `<body>` only when `load()` is called.
  
- **Flexible DOM Insertion:**  
  - Use `instance.head()` for head content (meta tags, styles, etc.).  
  - Use `instance.body(file, option)` for body content. Use `"b"` (beginning) for content that should appear at the top (such as a navigation bar) and `"e"` (end, the default) for content that should be appended (like a footer).

- **Dynamic Script Extraction & Execution:**  
  Automatically extracts external and inline `<script>` tags from loaded content and executes them in the correct order.

- **Custom Preloader Support:**  
  Display a custom loading element via `beforeLoad()`. This element is removed automatically when loading is complete.

- **Immediate Element-Level Loading:**  
  After `load()` is called, every DOM element gains a `put()` method for immediately inserting content into that element without waiting for queued operations.

## Installation

Include the oLoader script in your project. For example, if it’s hosted on GitHub Pages:

```html
<script src="https://ojiiis.github.io/oLoader/"></script>
```

Then initialize oLoader in your code:

```js
const loader = oLoader();
```

## Usage

### 1. Global Content Loading (Deferred)

Queue content to be inserted into the global document structure. Use the following methods:

- **`instance.head(file, option)`**  
  Loads content into the `<head>` tag.  
  _Example:_  
  ```js
  loader.head("./meta.html");
  ```

- **`instance.body(file, option)`**  
  Loads content into the `<body>` tag. Use `"b"` to insert content at the beginning (e.g. a global header) and `"e"` (default) to append content (e.g. a global footer).  
  _Example:_  
  ```js
  loader.body("./navbar.html", "b"); // Loads navbar at the beginning
  loader.body("./footer.html", "e"); // Appends footer at the end
  ```

- **`instance.script(file)`**  
  Loads an external or inline script to be executed.  
  _Example:_  
  ```js
  loader.script("./analytics.js");
  ```

- **`instance.beforeLoad(elem, callback)`**  
  Displays a custom loading element until the queued content is processed.  
  _Example:_  
  ```js
  loader.beforeLoad("<div>Loading, please wait...</div>");
  ```

- **`instance.load(callback)`**  
  Processes all queued content, replaces the `<head>` and `<body>` content, and then removes the loading element.  
  _Example:_  
  ```js
  loader.load(() => {
      console.log("Global content loaded!");
  });
  ```

### 2. Immediate Content Loading (`put()`)

After `load()` is called, every DOM element gains a `put()` method that allows content to be loaded dynamically.

_Example:_  
```js
document.getElementById("content-area").put("./page-content.html");
```

This will insert the content of `./page-content.html` inside the element with ID `"content-area"`.

## Best Practices

1. **Always use relative (`./file.html`) or absolute (`https://example.com/file.html`) URLs.**
2. **Load only necessary scripts globally.** If a script is only needed in a specific section, load it dynamically using `.put()`.
3. **Use `"b"` for top content and `"e"` for bottom content.** This ensures proper page structure.
4. **Use `beforeLoad()` to provide a better user experience.** A loading screen helps prevent a flash of unstyled content.

## Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My oLoader App</title>
    <script src="https://ojiiis.github.io/oLoader/"></script>
    <script>
        const loader = oLoader();
        loader.beforeLoad("<div>Loading...</div>");
        loader.head("./head-content.html");
        loader.body("./navbar.html", "b");
        loader.body("./footer.html", "e");
        loader.script("./global-scripts.js");
        loader.load();
    </script>
</head>
<body>
    <div id="content-area">
        <script>
            // Load dynamic content into this div
            document.getElementById("content-area").put("./page-content.html");
        </script>
    </div>
</body>
</html>
```

## License

oLoader is open-source and licensed under the MIT License.

---
