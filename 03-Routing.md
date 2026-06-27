# Chapter 3 — Routing

In the previous chapter, we built our first Flask application and understood how every line of code works. Our application responded to a single URL (`/`) and displayed a simple message in the browser.

However, real-world web applications contain multiple pages, each serving a different purpose. For example, a website may have a Home page, About page, Contact page, Login page, Profile page, and many more.

This raises an important question:

> **How does Flask know which Python function should execute when a user visits a particular URL?**

The answer is **Routing**.

Routing is one of the most fundamental concepts in Flask. It connects the URLs requested by a client (browser) to the Python functions that generate the response.

By the end of this chapter, you'll understand how Flask maps URLs to Python functions, create multiple routes, build dynamic routes, work with URL variables, use different HTTP methods, and learn how to build and redirect URLs.

---

# Understanding URLs

Before learning routing, let's first understand what a **URL** is.

A **URL (Uniform Resource Locator)** is the address used to locate a resource on the web.

For example:

```text
http://127.0.0.1:5000/about
```

Let's break it down.

| Part | Description |
|------|-------------|
| `http://` | Communication protocol between the client and the server |
| `127.0.0.1` | Server address (your own computer) |
| `5000` | Port number |
| `/about` | Path requested by the client |

The first three parts identify **where** the request should be sent.

The last part (`/about`) identifies **what** the user wants to access.

Some common URLs are:

```text
/
```

Home Page

```text
/about
```

About Page

```text
/contact
```

Contact Page

```text
/login
```

Login Page

```text
/profile
```

Profile Page

Every page in a web application has its own unique URL.

---

# What Is Routing?

Routing is the process of connecting a **URL** to a **Python function**.

Whenever a request reaches a Flask application, Flask performs the following steps:

1. Receives the incoming request.
2. Reads the requested URL.
3. Searches for a matching route.
4. Executes the corresponding Python function.
5. Returns the response to the client.

You can think of routing as a mapping table.

```text
URL                     Python Function

/                 ─────►  home()

/about            ─────►  about()

/contact          ─────►  contact()

/profile          ─────►  profile()
```

Without routing, Flask would have no way of determining which function should execute for a particular request.

Routing acts as the bridge between the browser and your Python code.

---

# Multiple Routes

So far, our application has responded to only one URL.

```python
@app.route("/")
def home():
    return "Welcome to the Home Page"
```

Let's add another route.

```python
@app.route("/about")
def about():
    return "Welcome to the About Page"
```

Now let's add one more.

```python
@app.route("/contact")
def contact():
    return "Welcome to the Contact Page"
```

Our application now looks like this:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Welcome to the Home Page"

@app.route("/about")
def about():
    return "Welcome to the About Page"

@app.route("/contact")
def contact():
    return "Welcome to the Contact Page"

if __name__ == "__main__":
    app.run(debug=True)
```

Run the application and visit the following URLs.

### Home Page

```text
http://127.0.0.1:5000/
```

Output:

```text
Welcome to the Home Page
```

### About Page

```text
http://127.0.0.1:5000/about
```

Output:

```text
Welcome to the About Page
```

### Contact Page

```text
http://127.0.0.1:5000/contact
```

Output:

```text
Welcome to the Contact Page
```

Each URL is connected to a different Python function.

When a request arrives, Flask compares the requested URL with all registered routes. Once it finds a matching route, it executes the corresponding function and returns its response.

This is the foundation of routing in Flask.

---

## Key Takeaways


- What a URL is.
- How a URL is structured.
- What routing is.
- How Flask maps URLs to Python functions.
- How to create multiple routes.
- How different URLs can display different content.

# Dynamic Routes

So far, our routes have been **static**, meaning each route responds to one fixed URL.

For example:

```python
@app.route("/about")
def about():
    return "About Page"
```

This route responds only when the user visits:

```text
/about
```

However, real-world applications often need to display different data using the same route.

For example:

```text
/product/101
/product/102
/product/205
/product/999
```

Creating a separate route for every product is neither practical nor scalable.

Flask solves this problem using **Dynamic Routes**.

A dynamic route allows a portion of the URL to change while keeping the remaining route unchanged.

---

## Creating a Dynamic Route

```python
@app.route("/product/<id>")
def product(id):
    return "Product ID: " + id
```

Here,

```text
<id>
```

is called a **URL Variable**.

Anything provided after `/product/` is captured and stored inside the variable `id`.

For example,

```text
/product/101
```

Output:

```text
Product ID: 101
```

Another request:

```text
/product/205
```

Output:

```text
Product ID: 205
```

Instead of creating hundreds of routes, a single dynamic route can handle them all.

---

## How Dynamic Routes Work

When a user visits:

```text
/product/101
```

Flask performs the following steps:

1. Receives the request.
2. Matches `/product/<id>`.
3. Extracts `101`.
4. Stores it inside the variable `id`.
5. Executes the `product()` function.
6. Returns the response.

Internally, it behaves as if Flask calls:

```python
product("101")
```

---

## Naming URL Variables

The variable name inside `< >` can be anything.

```python
@app.route("/product/<id>")
```

```python
@app.route("/product/<product_id>")
```

```python
@app.route("/product/<item>")
```

All are valid.

However, always choose meaningful names.

Good:

```text
product_id
username
order_id
```

Avoid vague names such as:

```text
x
a
temp
```

---

## Important Rule

The variable name in the route must match the function parameter.

Correct:

```python
@app.route("/product/<id>")
def product(id):
    return id
```

Incorrect:

```python
@app.route("/product/<id>")
def product(product_id):
    return product_id
```

Flask won't know where to pass the value because the names don't match.

---

# URL Variables & Converters

By default, Flask treats every URL variable as a **string**.

Example:

```python
@app.route("/product/<id>")
def product(id):
    return id
```

Visiting:

```text
/product/101
```

stores:

```python
id = "101"
```

Notice that `101` is stored as a string, not an integer.

---

## Why Do We Need URL Converters?

Suppose a product ID should always be a number.

The following URL should work:

```text
/product/101
```

But this should not:

```text
/product/apple
```

To enforce this, Flask provides **URL Converters**.

---

## Integer Converter

```python
@app.route("/product/<int:id>")
def product(id):
    return str(id)
```

Now:

```text
/product/101
```

works successfully.

But:

```text
/product/apple
```

returns a **404 Not Found** because `"apple"` is not an integer.

---

## Common URL Converters

### String (Default)

```python
<string:name>
```

Example:

```text
/user/Namitha
```

---

### Integer

```python
<int:id>
```

Example:

```text
/product/101
```

---

### Float

```python
<float:price>
```

Example:

```text
/price/99.99
```

---

### Path

Used when the variable may contain forward slashes.

```python
<path:file_path>
```

Example:

```text
/files/python/flask/app.py
```

Unlike the default string converter, the `path` converter captures the entire path, including `/`.

---

## When Should You Use URL Converters?

Use converters whenever your route expects a specific type of data.

Examples:

```text
<int:id>           Product IDs
<string:name>      Usernames
<float:price>      Prices
<path:file_path>   File paths
```

They improve validation and prevent invalid URLs from reaching your application.

---

## Key Takeaways

- Dynamic routes allow one route to handle multiple URLs.
- Values inside `< >` are called URL Variables.
- Flask passes URL variables as function arguments.
- URL variables are strings by default.
- URL Converters validate and convert URL values into specific Python data types.
- Using converters makes your application more reliable and easier to maintain.

# HTTP Methods

Every time a client (such as a browser, mobile application, or API client) sends a request to a server, it also specifies **what action it wants to perform**.

This action is represented by an **HTTP Method**.

For example, a client may want to:

- Retrieve information
- Submit new data
- Update existing data
- Delete data

Instead of using different URLs for each action, HTTP uses different methods to describe the client's intention.

---

## Common HTTP Methods

The most commonly used HTTP methods are:

- GET
- POST
- PUT
- PATCH
- DELETE

Although Flask supports many HTTP methods, **GET** and **POST** are the ones you'll use most frequently while building web applications.

---

# GET Method

The **GET** method is used to retrieve data from the server.

It does **not** modify or create any data.

Examples include:

- Opening the Home page
- Viewing products
- Reading blog posts
- Opening a user's profile

A simple Flask route:

```python
@app.route("/")
def home():
    return "Welcome to Flask"
```

When you visit:

```text
http://127.0.0.1:5000/
```

the browser automatically sends:

```text
GET /
```

Flask matches the route and executes:

```python
home()
```

---

# POST Method

The **POST** method is used to send new data to the server.

Examples include:

- Registering a new account
- Logging in
- Submitting a contact form
- Adding a new product

A route that accepts only POST requests:

```python
@app.route("/login", methods=["POST"])
def login():
    return "Login Successful"
```

When a user submits a login form, the browser sends:

```text
POST /login
```

Flask matches both the URL and the HTTP method before executing the function.

---

# Accepting Multiple HTTP Methods

Sometimes the same route needs to perform different actions.

Consider a Login page.

When a user first visits the page:

```text
GET /login
```

The application should display the login form.

After entering the username and password and clicking **Login**, the browser sends:

```text
POST /login
```

Now the application should process the submitted data.

Flask allows a route to accept multiple HTTP methods.

```python
@app.route("/login", methods=["GET", "POST"])
def login():
    pass
```

Here:

- `GET` displays the login page.
- `POST` processes the submitted login details.

The URL remains the same.

Only the HTTP method changes.

---

# PUT Method

The **PUT** method is used to replace existing data.

Example:

Updating an entire user profile.

```text
PUT /profile
```

Although PUT is more common in REST APIs than traditional Flask applications, understanding it is important.

---

# PATCH Method

The **PATCH** method is used to update only part of an existing resource.

For example, changing only a user's email address without replacing the entire profile.

```text
PATCH /profile
```

PATCH is commonly used in REST APIs.

---

# DELETE Method

The **DELETE** method removes an existing resource.

Example:

```text
DELETE /product/101
```

The server processes the request and removes Product 101.

---

# How Flask Matches HTTP Methods

When Flask receives a request, it performs two checks.

First, it checks whether the requested URL exists.

Then, it checks whether the HTTP method is allowed for that route.

For example,

```python
@app.route("/login", methods=["GET", "POST"])
def login():
    pass
```

If the browser sends:

```text
GET /login
```

Flask executes:

```python
login()
```

If the browser sends:

```text
POST /login
```

Flask again executes:

```python
login()
```

However, if the route only accepts POST requests:

```python
@app.route("/login", methods=["POST"])
def login():
    pass
```

and the browser sends:

```text
GET /login
```

Flask returns:

```text
405 Method Not Allowed
```

because the URL exists, but the HTTP method is not permitted.

---

# Common Project Examples

| URL | HTTP Method | Purpose |
|------|------------|---------|
| `/` | GET | Display Home Page |
| `/products` | GET | Display Products |
| `/product/101` | GET | Display Product Details |
| `/login` | GET | Display Login Page |
| `/login` | POST | Process Login |
| `/register` | POST | Create a New Account |
| `/profile` | PUT | Replace Profile Information |
| `/profile` | PATCH | Update Profile Information |
| `/product/101` | DELETE | Delete Product |

---

# Key Takeaways

- HTTP Methods describe the action a client wants to perform.
- `GET` retrieves data.
- `POST` sends new data to the server.
- `PUT` replaces existing data.
- `PATCH` updates part of existing data.
- `DELETE` removes existing data.
- Flask matches both the URL and the HTTP method before executing a route.
- If the URL exists but the method isn't allowed, Flask returns **405 Method Not Allowed**.

# Building URLs with `url_for()`

So far, we've been writing URLs directly in our application.

For example:

```python
@app.route("/about")
def about():
    return "About Page"
```

Suppose we want to navigate to the About page.

A beginner might simply write:

```python
redirect("/about")
```

While this works, it isn't the recommended approach.

Flask provides a better way to generate URLs using the `url_for()` function.

---

## Why Do We Need `url_for()`?

Imagine your application contains multiple pages.

```text
/
/about
/contact
/login
/profile
```

Now suppose you decide to rename the About page.

Old route:

```python
@app.route("/about")
def about():
```

New route:

```python
@app.route("/about-us")
def about():
```

If you had written:

```python
redirect("/about")
```

throughout your application, every occurrence would need to be updated manually.

Instead, Flask recommends generating URLs dynamically using the function name rather than hardcoding the URL.

---

## What Does `url_for()` Do?

The `url_for()` function generates the URL associated with a particular route.

Instead of writing the URL directly, we provide the **function name**.

Example:

```python
@app.route("/about")
def about():
    return "About Page"
```

Generate its URL:

```python
url_for("about")
```

Output:

```text
/about
```

Notice that we passed the function name:

```python
about
```

not the URL:

```text
/about
```

---

## How `url_for()` Works

When a Flask application starts, it internally creates a mapping between route functions and their URLs.

For example:

| Function | URL |
|----------|------|
| `home()` | `/` |
| `about()` | `/about` |
| `contact()` | `/contact` |

Now when Flask encounters:

```python
url_for("about")
```

it searches its routing table, finds the function named `about`, and returns its corresponding URL.

Generated URL:

```text
/about
```

---

## Using `url_for()` with Dynamic Routes

Suppose we have the following route.

```python
@app.route("/product/<int:id>")
def product(id):
    return "Product ID: " + str(id)
```

Here, the route expects a dynamic value.

Simply writing:

```python
url_for("product")
```

is not enough because Flask doesn't know which product you want.

Instead, pass the required variable.

```python
url_for("product", id=101)
```

Generated URL:

```text
/product/101
```

Another example:

```python
url_for("product", id=500)
```

Generated URL:

```text
/product/500
```

The variable name passed to `url_for()` must match the variable name defined in the route.

---

## Real-World Examples

Generate the Home page URL.

```python
url_for("home")
```

Output:

```text
/
```

Generate the Contact page URL.

```python
url_for("contact")
```

Output:

```text
/contact
```

Generate a Product page URL.

```python
url_for("product", id=25)
```

Output:

```text
/product/25
```

---

## Why Use `url_for()`?

Using `url_for()` offers several advantages.

- Eliminates hardcoded URLs.
- Automatically generates the correct route.
- Makes applications easier to maintain.
- Reduces the chance of broken links.
- Works seamlessly with both static and dynamic routes.

For these reasons, `url_for()` is considered a best practice in Flask development.

---

## Key Takeaways

- `url_for()` generates a URL for a given route.
- It uses the **function name**, not the URL path.
- It works with both static and dynamic routes.
- Dynamic routes require passing the corresponding URL variables.
- Using `url_for()` makes Flask applications easier to maintain and less prone to broken links.


# Redirecting Requests

In many web applications, a user shouldn't always remain on the same page after performing an action.

For example:

- After logging in, the user should be taken to the Dashboard.
- After registering, the user should be redirected to the Login page.
- After logging out, the user should be redirected to the Home page.
- After deleting a record, the user should be redirected to the updated list of records.

Instead of displaying another page directly, Flask provides the `redirect()` function.

---

## What is `redirect()`?

The `redirect()` function tells the browser to navigate to another URL.

Unlike `url_for()`, which only generates a URL, `redirect()` actually sends the client to a different page.

Example:

```python
from flask import redirect

@app.route("/login")
def login():
    return redirect("/dashboard")
```

When a user visits:

```text
/login
```

they are automatically redirected to:

```text
/dashboard
```

---

## How `redirect()` Works

Consider the following application.

```python
from flask import Flask, redirect

app = Flask(__name__)

@app.route("/dashboard")
def dashboard():
    return "Welcome to Dashboard"

@app.route("/login")
def login():
    return redirect("/dashboard")
```

Let's understand what happens behind the scenes.

### Step 1

The user visits:

```text
/login
```

The browser sends:

```text
GET /login
```

---

### Step 2

Flask matches the route.

```python
@app.route("/login")
```

and executes:

```python
login()
```

---

### Step 3

The function returns:

```python
redirect("/dashboard")
```

Flask does **not** execute the `dashboard()` function directly.

Instead, it sends a redirect response back to the browser.

---

### Step 4

The browser receives the redirect response.

It automatically sends another request.

```text
GET /dashboard
```

---

### Step 5

Flask now matches:

```python
@app.route("/dashboard")
```

executes:

```python
dashboard()
```

and returns:

```text
Welcome to Dashboard
```

The user now sees the Dashboard page.

---

## Complete Flow

```text
User
   │
Visits /login
   │
Browser sends GET /login
   │
Flask executes login()
   │
login() returns redirect("/dashboard")
   │
Browser receives redirect response
   │
Browser sends GET /dashboard
   │
Flask executes dashboard()
   │
Dashboard page is displayed
```

Notice that **two separate HTTP requests** are involved.

---

## Using `redirect()` with `url_for()`

Although you can write:

```python
return redirect("/dashboard")
```

the recommended approach is:

```python
return redirect(url_for("dashboard"))
```

Here's what happens:

First,

```python
url_for("dashboard")
```

generates:

```text
/dashboard
```

Then,

```python
redirect("/dashboard")
```

redirects the browser to that URL.

Using both functions together avoids hardcoding URLs and makes the application easier to maintain.

---

## Real-World Examples

### Redirect after Login

```python
return redirect(url_for("dashboard"))
```

---

### Redirect after Registration

```python
return redirect(url_for("login"))
```

---

### Redirect after Logout

```python
return redirect(url_for("home"))
```

---

### Redirect after Deleting a Record

```python
return redirect(url_for("products"))
```

---

## `url_for()` vs `redirect()`

| `url_for()` | `redirect()` |
|-------------|--------------|
| Generates a URL | Redirects the browser to another URL |
| Returns a URL string | Returns a redirect response |
| Does not change the current page | Navigates the user to a different page |
| Commonly used with `redirect()` | Often accepts a URL generated by `url_for()` |

A simple way to remember the difference is:

- **`url_for()` answers:** *"What is the URL?"*
- **`redirect()` says:** *"Go to that URL."*

---

# Chapter Summary

In this chapter, you learned how Flask maps incoming requests to Python functions using **Routing**.

You also explored how to create multiple routes, build dynamic routes using URL variables, validate URL values with converters, and control which HTTP methods are accepted by a route.

Finally, you learned how to generate URLs using `url_for()` and redirect users to different pages using `redirect()`.

At this point, you have a solid understanding of how requests reach the correct function within a Flask application.

In the next chapter, we'll explore what happens **after** a route is matched by understanding the complete **Request–Response Lifecycle** in Flask.