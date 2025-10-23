# oLoader: The Minimalist Client-Side Component Assembler 🧩

**oLoader** is a high-performance, zero-dependency micro-framework for building modular web applications. It specializes in **structured component assembly** and **secure, declarative data-binding** without the need for a Virtual DOM or a complex runtime.

**Current Version:** 3.0 (Pure Assembler & Secure Templating)

-----

## ✨ Features at a Glance

  * **Micro-Sized & Zero Dependency:** Built entirely with native JavaScript for maximum performance.
  * **Modular Assembly:** Load external HTML files as reusable components with a single, clean API call.
  * **Secure Templating:** Use declarative HTML tags for data (`<var>`), conditionals (`<if>`), and loops (`<for>`).
  * **No `eval()`:** Template logic is parsed manually for enhanced security and predictability.
  * **Built-in App Control:** Easy access to URL parameters, local storage, and navigation utilities.

-----

## 🚀 Getting Started

### 1\. Setup

Include `oloader.js` in your HTML and initialize the application.

```html
<script src="oloader.js"></script>
<script>
    const app = oLoader();
    
    // Begin the main application logic here
    app.load(() => {
        // e.g., load the main dashboard component
        app.use('views/dashboard.html', { user: 'Guest' }).put(oG('#app-root'));
    });
</script>
```

### 2\. Define a Component (Example: `user-info.html`)

Components are regular HTML files using oLoader's template tags:

```html
<div class="profile-card">
    <h3>Hello, <var>user.name</var>!</h3>
    
    <if exp="$is_admin = 'true'">
        <span class="badge">Administrator</span>
    </if>
    
    <h4>Recent Activity:</h4>
    <ul>
        <for exp="$item of $activity_list">
            <li>Action: <var>item.action</var> on <var>item.date</var></li>
        </for>
    </ul>
</div>
```

-----

## 💻 Core API Reference

### 1\. `app.use(file, [variables])`

Loads the component file, processes all template tags, and returns an object ready for DOM insertion.

```javascript
const userData = {
    user: { name: "Alice", is_admin: 'true' },
    activity_list: [{ action: "Login", date: "Today" }, { action: "Post", date: "Yesterday" }]
};

// 1. Load the file and prepare the component data
const userComponent = await app.use('components/user-info.html', userData);
```

### 2\. `element.put(content, [pos], [variables], [callback])`

The main method for DOM insertion, available on any element retrieved via `oG()` or created via `app.use()`.

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `content` | Link/Text/Object | N/A | Component path, raw HTML string, or HTML Object. |
| `pos` | `string` | `null` | **Position:** `'b'` (beginning), `'e'` (end), `null` (replace inner HTML). |
| `variables` | `object` | `{}` | **Optional:** Variables to inject into the template during insertion. |
| `callback` | `function` | `undefined` | Executes after insertion, receiving the newly inserted element. |

```javascript
// 2. Insert the component into the DOM
userComponent.put(oG('#sidebar'), 'b', {}, (insertedElement) => {
    console.log("Component fully rendered and inserted.");
    // Add custom event listeners here
});
```

-----

## 🌐 Templating Syntax Details

### A. `<var>variable name</var>`

Replaced by the corresponding value from the merged `variables` object.

| Syntax | Description |
| :--- | :--- |
| `<var>user.name</var>` | Accesses nested properties (e.g., `variables.user.name`). |
| `<var>tag</var>` | Used inside a loop to access the current iteration item. |

### B. `<if exp="expression">...</if>`

Conditionally renders the content inside the tags.

| Syntax | Description |
| :--- | :--- |
| `<if exp="$status = 'admin'">` | **Equality:** Checks if the variable `$status` is equal to the literal string `'admin'`. |
| `<if exp="$count > 10">` | **Comparison:** Supports numerical comparisons (`>`, `<`, `>=`, `<=`). |

> **Security Note:** All expressions are processed via secure manual parsing. Only predefined operators are supported, and no arbitrary JavaScript code can be executed.

### C. `<for exp="$item in $array">...</for>`

Iterates over an array or object in the context variables.

| Syntax | Description |
| :--- | :--- |
| `<for exp="$item of $tags">` | `$item` is the loop variable; `$tags` is the array/object from the context. |
| `<var>item.name</var>` | Accessing properties of the current loop item. |

-----

## 🌍 Application Utilities (App Control)

The `app` instance provides direct helpers for common browser functions, streamlining application logic.

| Property/Method | Usage |
| :--- | :--- |
| `app.query` | `const id = app.query.user_id;` (URL query parameters as an object) |
| `app.location` | `console.log(app.location.href);` |
| `app.storage` | `app.storage.setItem('key', 'value');` (Alias for `window.localStorage`) |
| `app.path` | `if (app.path === '/home') { ... }` |
| `app.host` | `console.log(app.host);` |
| `app.navigate(link)` | `app.navigate('/new-page');` (Programmatic navigation) |
