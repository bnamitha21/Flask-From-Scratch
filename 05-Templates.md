# Chapter 5 — Templates

In the previous chapters, our Flask applications returned responses directly from Python.

For example:

```python
@app.route("/")
def home():
    return "<h1>Welcome to Flask</h1>"
```

Although this works, it is not a practical approach for building real-world web applications.

Imagine creating an e-commerce website or a student portal where each page contains hundreds of lines of HTML. Writing all of that HTML inside Python functions would make the code difficult to read, maintain, and update.

A better approach is to keep the application's logic and its user interface separate.

- **Python** handles the application's logic and data.
- **HTML** is responsible for displaying that data to the user.

Flask achieves this separation using **Templates**.

---

# Why Templates?

Before understanding templates, let's first understand the problem they solve.

Consider the following HTML page.

```html
<h1>Welcome</h1>
```

Every user visiting this page will see exactly the same content.

However, real-world applications display different information to different users.

For example:

User 1:

```text
Welcome Namitha
```

User 2:

```text
Welcome Rahul
```

Creating a separate HTML page for every user is neither practical nor scalable.

Instead, we create **one HTML page** and let Flask insert the appropriate data before sending it to the browser.

This is the purpose of a **Template**.

---

# What is an HTML Template?

An **HTML Template** is a normal HTML file that can display dynamic data.

Unlike a regular HTML page, a template can contain **placeholders**. These placeholders are replaced with actual values before the page is sent to the browser.

For example:

```html
<h1>Welcome {{ username }}</h1>
```

Here,

```jinja
{{ username }}
```

is a **placeholder**.

It tells Flask:

> "Replace this placeholder with the value of `username`."

Suppose Flask provides:

```python
username = "Namitha"
```

Before sending the page to the browser, Flask generates the following HTML:

```html
<h1>Welcome Namitha</h1>
```

Notice that the browser never receives:

```jinja
{{ username }}
```

It only receives the final HTML page after the placeholder has been replaced.

This process of replacing placeholders with actual data is called **Rendering**, which we'll understand shortly.

---

# The `templates` Directory

Now that we know what a template is, the next question is:

**Where should these template files be stored?**

Flask follows a simple convention.

Every HTML template should be placed inside a directory named:

```text
templates/
```

Example project structure:

```text
FlaskProject/

│── app.py
│
└── templates/
    │── index.html
    │── about.html
    └── contact.html
```

Whenever Flask needs to render a template, it automatically searches inside the `templates` directory.

For this reason, the folder should always be named `templates`.

As projects become larger, templates can also be organized into subdirectories.

Example:

```text
templates/

│── home/
│     └── index.html
│
│── users/
│     ├── login.html
│     └── profile.html
│
└── admin/
      └── dashboard.html
```

Organizing templates this way makes large projects easier to navigate and maintain.

---

# `render_template()`

Creating an HTML file inside the `templates` directory does not automatically display it.

Flask must be instructed to load that template and return it as the response.

Simply writing:

```python
return "index.html"
```

returns the text `"index.html"` to the browser.

It does **not** open the HTML file.

To render an HTML template, Flask provides the `render_template()` function.

```python
from flask import render_template

@app.route("/")
def home():
    return render_template("index.html")
```

The `render_template()` function performs several tasks automatically:

1. Searches for the specified template inside the `templates` directory.
2. Loads the HTML template.
3. Passes the template to Flask's template engine.
4. Replaces any placeholders with actual data.
5. Generates the final HTML page.
6. Returns the generated HTML to the browser.

This entire process is called **Rendering**.

In simple terms,

> **Rendering means generating the final HTML page by combining an HTML template with data provided by the Flask application.**

The complete flow is shown below.

```text
Browser

↓

HTTP Request

↓

Flask Route Function

↓

render_template()

↓

Load HTML Template

↓

Replace Placeholders

↓

Generate Final HTML

↓

Send Response

↓

Browser Displays the Page
```

Instead of mixing Python and HTML in the same file, Flask encourages us to keep HTML inside templates and use `render_template()` whenever a webpage needs to be returned.

This approach makes applications cleaner, easier to maintain, and much more scalable.

---

## Key Takeaways

- Templates separate application logic from the user interface.
- An HTML template is an HTML file that can display dynamic data.
- Templates use placeholders that Flask replaces with actual values.
- All templates should be stored inside the `templates` directory.
- `render_template()` loads, renders, and returns an HTML template.
- Rendering is the process of generating the final HTML page before it is sent to the browser.

# Introduction to Jinja

In the previous section, we learned that templates contain placeholders that are replaced with actual data before the page is sent to the browser.

The next question is:

**Who replaces those placeholders?**

The answer is **Jinja**.

Jinja is the template engine used by Flask.

Its primary responsibility is to combine the HTML template with the data provided by the Flask application and generate the final HTML page.

Consider the following template:

```html
<h1>Welcome {{ username }}</h1>
```

If Flask passes:

```python
username = "Namitha"
```

Jinja replaces the placeholder and generates:

```html
<h1>Welcome Namitha</h1>
```

The browser never receives:

```jinja
{{ username }}
```

Instead, it receives only the final HTML generated by Jinja.

In simple terms,

> **Jinja acts as a bridge between Python and HTML.**

Python provides the data, Jinja inserts that data into the HTML template, and Flask sends the final HTML page to the browser.

---

# Jinja Syntax

Jinja provides three main types of syntax.

Each serves a different purpose while generating the final HTML page.

---

## Variables

Variables are written using double curly braces.

```jinja
{{ variable }}
```

Variables are used to display data inside a template.

Example:

```jinja
<h1>Welcome {{ username }}</h1>
```

If Flask passes:

```python
username = "Namitha"
```

Jinja generates:

```html
<h1>Welcome Namitha</h1>
```

Whenever you want to display a value inside an HTML page, use:

```jinja
{{ }}
```

---

## Statements

Statements are written using:

```jinja
{% ... %}
```

Unlike variables, statements do not display any value.

Instead, they control how the HTML page should be generated.

Statements are commonly used for:

- Conditions
- Loops
- Template Inheritance
- Including Templates

For example,

```jinja
{% if %}
```

checks a condition,

while

```jinja
{% for %}
```

repeats a block of HTML.

Whenever you want to perform logic inside a template, use:

```jinja
{% %}
```

---

## Comments

Comments are written using:

```jinja
{# This is a comment #}
```

Comments are ignored while rendering the template.

They are visible only to developers and never appear in the final HTML page.

---

# Variables

A template becomes dynamic only when Flask passes data to it.

Data is passed using the `render_template()` function.

Example:

```python
@app.route("/")
def home():
    return render_template(
        "index.html",
        username="Namitha",
        age=20
    )
```

Inside the template:

```html
<h1>{{ username }}</h1>

<p>Age: {{ age }}</p>
```

The browser receives:

```html
<h1>Namitha</h1>

<p>Age: 20</p>
```

You can pass any number of variables to a template.

These values may come from:

- User input
- A database
- An API
- Calculations
- Python variables

As long as the data is passed through `render_template()`, Jinja can display it.

---

# Loops

Real-world applications often need to display multiple items instead of a single value.

For example:

- Product listings
- Student records
- Blog posts
- Orders
- Database results

Writing HTML repeatedly for every item would create unnecessary duplication.

Instead, Jinja provides the `for` loop.

Suppose Flask passes:

```python
students = [
    "Namitha",
    "Rahul",
    "Anjali"
]
```

```python
return render_template(
    "index.html",
    students=students
)
```

Inside the template:

```jinja
{% for student in students %}

<h2>{{ student }}</h2>

{% endfor %}
```

Jinja generates:

```html
<h2>Namitha</h2>

<h2>Rahul</h2>

<h2>Anjali</h2>
```

Instead of writing the same HTML multiple times, Jinja automatically repeats the HTML for every item in the collection.

Loops also work with dictionaries and other Python collections.

---

# Conditional Rendering

Not every user should see the same content.

For example:

- A logged-in user should see a **Logout** button.
- A guest user should see a **Login** button.
- An administrator should see an **Admin Dashboard**.
- A customer should not.

Showing different content based on a condition is called **Conditional Rendering**.

Jinja provides conditional statements similar to Python.

Example:

```jinja
{% if age >= 18 %}

<h2>{{ username }}</h2>

{% endif %}
```

If the condition is true, Jinja includes the HTML inside the block.

If the condition is false, Jinja skips that HTML while generating the final page.

Jinja also supports `elif` and `else`.

Example:

```jinja
{% if marks >= 35 %}

<p>Pass</p>

{% else %}

<p>Fail</p>

{% endif %}
```

Conditional rendering allows a single template to display different content for different users without creating multiple HTML pages.

---

## Key Takeaways

- Jinja is Flask's template engine.
- Jinja combines HTML templates with Python data.
- `{{ }}` is used to display values.
- `{% %}` is used for template logic such as loops and conditions.
- `{# #}` is used for comments.
- Variables allow templates to display dynamic data.
- Loops generate repeated HTML from Python collections.
- Conditional rendering displays different HTML based on conditions.

# Template Inheritance

As web applications grow, many pages begin to share the same layout.

For example, consider the following pages:

- Home
- About
- Contact
- Profile

Although the content of each page is different, they usually share common elements such as:

- Navigation Bar
- Footer
- CSS files
- JavaScript files
- Company Logo

Writing the same HTML in every page creates unnecessary duplication. If the navigation bar needs to be updated later, every template would have to be modified individually.

To solve this problem, Jinja provides **Template Inheritance**.

Template Inheritance allows multiple templates to share a common layout while changing only the page-specific content.

---

# The Parent Template

The common layout is placed inside a parent template, usually named:

```text
base.html
```

This template contains all the HTML that is common across the application.

Example:

```html
<!DOCTYPE html>
<html>

<head>
    <title>My Website</title>
</head>

<body>

    <nav>
        Navigation Bar
    </nav>

    {% block content %}
    {% endblock %}

    <footer>
        Footer
    </footer>

</body>
</html>
```

Notice the following statement:

```jinja
{% block content %}
{% endblock %}
```

This defines a **block**.

A block acts as a placeholder where child templates can insert their own content.

---

# The Child Template

Instead of creating the complete HTML page again, another template simply inherits the parent template.

Example:

```jinja
{% extends "base.html" %}

{% block content %}

<h1>Home Page</h1>

{% endblock %}
```

Here,

```jinja
{% extends "base.html" %}
```

tells Jinja to use `base.html` as the parent template.

The statement:

```jinja
{% block content %}
```

matches the block with the same name in `base.html`.

Everything written inside this block replaces the corresponding placeholder in the parent template.

---

# How Template Inheritance Works

When Flask renders the child template, Jinja performs the following steps:

1. Loads the parent template (`base.html`).
2. Finds the matching block.
3. Replaces that block with the content from the child template.
4. Generates the final HTML page.
5. Sends the generated page to the browser.

The final HTML becomes:

```html
<nav>
    Navigation Bar
</nav>

<h1>Home Page</h1>

<footer>
    Footer
</footer>
```

The parent template remains unchanged.

Only the content inside the matching block changes for each page.

---

# Why Use Template Inheritance?

Template Inheritance provides several advantages:

- Eliminates duplicate HTML.
- Makes templates easier to maintain.
- Keeps the project organized.
- Allows multiple pages to share the same layout.
- Changes to the common layout need to be made only once.

It is commonly used to share:

- Navigation Bars
- Footers
- Common CSS files
- JavaScript files
- Page Layouts

---

# Including Templates

Sometimes we do not want to reuse an entire page layout.

Instead, we may only want to reuse a small part of a page.

For example:

- Navigation Bar
- Footer
- Sidebar
- Alert Messages

Instead of copying these components into multiple templates, Jinja provides the `include` statement.

Example:

```jinja
{% include "navbar.html" %}
```

When Jinja encounters this statement, it loads the contents of `navbar.html` and inserts them into the current template.

Similarly,

```jinja
{% include "footer.html" %}
```

loads the footer template.

This keeps reusable components separate and makes the application easier to maintain.

---

# Template Inheritance vs Include

Although both help reduce duplicate code, they serve different purposes.

| Template Inheritance | Include |
|----------------------|---------|
| Reuses an entire page layout. | Reuses a small component of a page. |
| Uses `extends`. | Uses `include`. |
| Commonly used for `base.html`. | Commonly used for navbars, footers, sidebars, and reusable sections. |

Choose **Template Inheritance** when multiple pages share the same overall layout.

Choose **Include** when only a small HTML component needs to be reused.

---

# Jinja Filters

Sometimes the data received from Flask is correct, but we want to display it differently.

For example,

Python provides:

```python
username = "namitha"
```

The browser should display:

```text
NAMITHA
```

Instead of modifying the data in Python, Jinja provides **Filters**.

Filters transform data before it is displayed.

The original Python value remains unchanged.

Filters use the pipe (`|`) operator.

Syntax:

```jinja
{{ variable | filter }}
```

Example:

```jinja
{{ username | upper }}
```

Output:

```text
NAMITHA
```

Some commonly used filters are:

```jinja
{{ username | lower }}
```

Converts text to lowercase.

```jinja
{{ username | title }}
```

Converts text to title case.

Example:

```text
namitha bajjuri
```

becomes:

```text
Namitha Bajjuri
```

```jinja
{{ students | length }}
```

Returns the number of items in the collection.

Filters allow templates to control how data is displayed without modifying the original Python data.

---

# Best Practices

- Keep application logic inside Python, not inside templates.
- Use templates only for presenting data.
- Create a `base.html` for pages that share a common layout.
- Use `include` for reusable components such as navigation bars and footers.
- Organize templates into folders as the project grows.
- Use meaningful template names.
- Avoid duplicating HTML whenever possible.

---

## Key Takeaways

- Template Inheritance allows multiple pages to share a common layout.
- `base.html` usually acts as the parent template.
- `extends` inherits the parent template.
- `block` defines sections that child templates can replace.
- `include` inserts reusable HTML components into a template.
- Filters modify the displayed output without changing the original data.
- Templates become easier to maintain when layouts and reusable components are separated.