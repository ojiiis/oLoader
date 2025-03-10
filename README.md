# oLoader

**oLoader** is a lightweight JavaScript framework for dynamically loading HTML content and scripts into a webpage. It provides a clean, chainable API for loading content into the `<head>` and `<body>` without reloading the page.

## 🚀 Features

- ✅ **Dynamic Content Loading** – Load HTML files into specific parts of the page.
- ✅ **Script Handling** – Supports inline and external scripts.
- ✅ **Non-Destructive DOM Manipulation** – Does not remove existing `<head>` and `<body>`.
- ✅ **Minimal & Fast** – Uses vanilla JavaScript with no dependencies.
- ✅ **Chainable API** – Simple method chaining for readability.

## 📦 Installation

You can use oLoader via a CDN:

```html
<script src="https://ojiiis.github.io/oLoader/"></script>
```
## 🔥 Usage

### Basic Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>oLoader Example</title>
    <script src="https://ojiiis.github.io/oLoader/"></script>
</head>
<body>
    <div id="app"></div>

    <script>
        App("app")
            .head("header.html")
            .body("content.html")
            .body("footer.html")
            .script("script.js")
            .run(() => console.log("All content loaded!"));
    </script>
</body>
</html>
```
