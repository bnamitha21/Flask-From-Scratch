# Chapter 6 — Static Files

In the previous chapter, we learned how templates allow Flask to generate dynamic HTML pages.

However, a webpage is more than just HTML.

Modern websites contain styling, images, icons, animations, and interactive behavior. These resources are not generated dynamically. Instead, they are served directly to the browser without any modification.

Flask refers to these resources as **Static Files**.

---

# Why Static Files?

Consider the following HTML page.

```html
<h1>Welcome to Flask</h1>
```

Although this creates a webpage, it does not define how the page should look or behave.

For example:

- CSS controls the appearance of the page.
- JavaScript adds interactivity.
- Images display logos, banners, and icons.

Keeping these resources separate from HTML makes applications easier to organize and maintain.

---

# What are Static Files?

Static files are resources that Flask sends to the browser **without modifying their contents**.

Unlike templates, static files are **not processed by Jinja**.

Examples of static files include:

- CSS Files
- JavaScript Files
- Images
- Fonts
- Icons

For example, a CSS file may contain:

```css
h1{
    color: blue;
}
```

Flask sends this file exactly as it is written.

Similarly, JavaScript files and images are also sent directly to the browser without any processing.

This is why they are called **Static Files**.

---

# The `static` Directory

Flask follows a convention for organizing static files.

All static resources should be stored inside a directory named:

```text
static/
```

A typical project structure looks like:

```text
FlaskProject/

│── app.py
│
├── templates/
│     └── index.html
│
└── static/
      ├── css/
      │     └── style.css
      │
      ├── js/
      │     └── script.js
      │
      └── images/
            └── logo.png
```

Whenever a browser requests a static resource, Flask automatically searches inside the `static` directory.

For this reason, static files should always be placed inside this folder.

---

# Using Static Files

Static files are linked inside HTML templates.

For example, to include a CSS file:

```html
<link rel="stylesheet"
href="{{ url_for('static', filename='css/style.css') }}">
```

Here,

```python
url_for('static', filename='css/style.css')
```

generates the correct URL for the CSS file stored inside the `static` directory.

Similarly, JavaScript files are included as:

```html
<script
src="{{ url_for('static', filename='js/script.js') }}">
</script>
```

Images are included as:

```html
<img
src="{{ url_for('static', filename='images/logo.png') }}"
alt="Logo">
```

Notice that the `filename` always specifies the file path **relative to the `static` directory**.

---

# Why use `url_for()`?

Although writing the path directly is possible,

```html
/static/css/style.css
```

Flask recommends using:

```python
url_for('static', filename='css/style.css')
```

Using `url_for()` follows Flask's standard way of generating URLs, avoids hardcoding paths, and keeps templates consistent with the application's routing system.

> **Note:** If the file itself is renamed, the `filename` must also be updated. `url_for()` generates the correct URL based on the filename you provide—it does not automatically detect renamed files.

---

# Organizing Static Files

As applications grow, keeping all static files in one folder becomes difficult to manage.

A common project structure is:

```text
static/

├── css/
│     ├── style.css
│     └── login.css
│
├── js/
│     ├── app.js
│     └── validation.js
│
├── images/
│     ├── logo.png
│     └── banner.jpg
│
├── fonts/
│
└── icons/
```

Organizing files into separate folders improves readability and makes projects easier to maintain.

---

# Best Practices

- Store all static resources inside the `static` directory.
- Organize CSS, JavaScript, images, fonts, and icons into separate folders.
- Use meaningful file names.
- Use `url_for('static', filename=...)` when referencing static files.
- Keep HTML, CSS, JavaScript, and Python code separated for better maintainability.

---

## Key Takeaways

- Static files are resources that Flask serves directly to the browser.
- CSS, JavaScript, images, fonts, and icons are all static files.
- All static resources should be placed inside the `static` directory.
- Use `url_for('static', filename=...)` to generate URLs for static files.
- Organizing static files properly makes Flask applications cleaner and easier to maintain.