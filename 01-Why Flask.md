# Chapter 1 — Why Flask?

Before learning Flask, it's important to understand **why it exists**.

Every technology is built to solve a problem. If we understand the problem first, learning the solution becomes much easier.

In this chapter, we'll discover why Python alone isn't enough for web development, how websites actually work, and why developers use web frameworks like Flask.



## Why Python Alone Isn't Enough

If you've learned Python, you've probably written programs like this:

```python
print("Hello, World!")
```

When you run the program, Python displays:

```text
Hello, World!
```

The program works perfectly.

Now, imagine you want your friend to see this output.

Can they simply open their browser, enter your computer's address, and view your program?

**No.**

Your Python program runs only on **your computer**. It executes the code, displays the output, and finishes.

It doesn't wait for users.

It doesn't communicate with web browsers.

It doesn't create web pages.

This is the first problem we need to solve.



## Understanding the Web

Whenever you open a website, your browser communicates with another computer over the internet.

For example, when you visit:

```text
https://www.google.com
```

your browser sends a **request** asking Google's server for its homepage.

Google processes the request and sends back a **response** containing the webpage.

Your browser then displays that webpage on your screen.

Every website follows this same process.

```text
Browser
   │
   │ Request
   ▼
Server
   │
   │ Response
   ▼
Browser
```

This exchange happens in just a fraction of a second.


## Client and Server

To understand Flask, you only need to know two important terms.

### Client

A **client** is any application that requests information.

Examples include:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox
- Mobile Applications

Whenever you open a website, your browser acts as the client.



### Server

A **server** is a computer or application that receives requests, processes them, and sends responses back.

For example:

1. You open a website.
2. Your browser sends a request.
3. The server processes that request.
4. The server returns the webpage.

Without a server, websites cannot respond to users.



## Why Do We Need a Web Framework?

Suppose you wanted to build a website using only Python.

You would need to handle:

- Browser requests
- URLs
- HTTP communication
- Responses
- Error handling
- Application structure

Building all of this from scratch for every project would take a significant amount of time.

Instead, developers use **web frameworks**.

A web framework provides the tools and structure needed to build web applications efficiently, allowing developers to focus on the application's functionality rather than low-level implementation details.



## Introducing Flask

**Flask** is a lightweight web framework built for Python.

It acts as a bridge between your Python code and a web browser.

Instead of manually handling browser requests and responses, Flask manages that communication for you.

This allows you to focus on writing your application's logic while Flask takes care of the underlying web functionality.

Throughout this guide, you'll not only learn **how** to use Flask, but also **why** each feature exists and how it works behind the scenes.



## Chapter Summary

In this chapter, you learned that:

- A standard Python program runs only on your local computer.
- Websites work by exchanging requests and responses.
- Browsers act as clients.
- Servers process requests and return responses.
- Web frameworks simplify web development.
- Flask is a lightweight Python web framework designed for building web applications.



In the next chapter, we'll build our first Flask application and understand every line of code from the ground up.