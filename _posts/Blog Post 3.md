---
layout: post
title: Blog Post 3
---

In this blog post, I'm going to create a simple webapp using Flask. The app is a simple message bank. It can do two things:

1. Allow the user to **submit** messages to the bank.

2. Allow the user to **view** a sample of the messages currently stored in the bank.

Here's a link to the repository containing the code for the app:

**https://github.com/JiaaiShen/blog-post-3/**

Let's begin by importing all the libraries that we'll need in order to create the app.


```python
from flask import Flask, g, current_app, render_template, request
import sqlite3
```

Run the following cell to create a `Flask` object.


```python
app = Flask(__name__)
```

## Our First Page

First, we'll create a template called `base.html`.

```
<!doctype html>
<link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
<title>Blog Post 3</title>
<nav>
  <h1>A Simple Message Bank</h1>
  <ul>
    <li><a href="{{ url_for('submit') }}">Submit a message</a></li>
    <li><a href="{{ url_for('view') }}">View messages</a></li>
  </ul>
</nav>
<section class="content">
    <header>
       {% block header %}{% endblock %}
    </header>
    {% block content %}{% endblock %}
</section>
```

We put navigation links (the top two links at the top of the screen, one for submitting messages, the other for viewing messages) inside this template. Then the other two templates we'll write (`submit.html` and `view.html`) can extend the `base.html` template.  

Let's write a function to render the `base.html` template.


```python
# root directory of our webapp
@app.route("/")
def base():
    # render the base.html template
    return render_template("base.html")
```

Notice that `app.route` controls what URL the interface we are developing is going to have.

## Enabling Submissions

Next, we'll create a template called `submit.html` with three user interface elements:

1. A text box for submitting a message

2. A text box for submitting the name of the user

3. A "Submit message" button

We have this template extend the `base.html` template. 

```
{% extends 'base.html' %}

{% block header %}
  <h1>{% block title %}Submit{% endblock %}</h1>
{% endblock %}

{% block content %}
  <form method="post">
      <label for="message">Your message:</label>
      <br>
      <input type="text" name="message" id="message">
      <br>
      <label for="handle">Your name or handle:</label>
      <br>
      <input type="text" name="handle" id="handle">
      <br>
      <input type="submit" value="Submit message">
  </form>

  {% if message %}
    <br>
    <b>Hello {{handle}}! Thanks for submitting a message!</b>
  {% endif %}

{% endblock %}
```
Here's what we should note:

1. The title of our submission page is `Submit`. 

2. We add a note thanking the user for submitting a message.

Here's an example of our submission page:

![submission-page.png](/images/submission-page.png)

Now, we'll write two Python functions for database management.

The `get_message_db()` function should handle creating the database of messages.


```python
def get_message_db():
    # check whether there is a database called message_db
    # in the g attribute of the app
    if 'message_db' not in g:
        # connect to the database
        g.message_db = sqlite3.connect("messages_db.sqlite")

    # execute the SQL command in the init.sql file
    with current_app.open_resource('init.sql') as f:
        g.message_db.executescript(f.read().decode('utf8'))

    # return the connection g.message_db
    return g.message_db
```

This function checks whether there is a database called `message_db` in the `g` attribute of the app. If not, then this function will connect to that database if not. This function also checks whether a table called `messages` exists in `message_db`. This function will create the table if not. We give the table an `id` column (integer), a `handle` column (text), and a `message` column (text). We write the SQL command to check and create such a table in a separate `init.sql` file:

```
CREATE TABLE IF NOT EXISTS messages (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    handle TEXT NOT NULL,
    message TEXT NOT NUll
);
```

The `get_message_db()` function should return the connection `g.message_db`.

The `insert_message()` function should handle inserting a user's message into the database of messages.


```python
def insert_message(request):
    # extract the message and the handle from request
    message = request.form["message"]
    handle = request.form["handle"]

    # connect to the database
    db = get_message_db()

    # use a cursor to insert the message into the database
    cursor = db.cursor()
    cursor.execute(
        'INSERT INTO messages (handle, message) VALUES (?, ?)',
        (handle, message)
    )
    # save the insertion
    db.commit()

    # close the database connection
    db.close()

    return(message, handle)
```

This function extracts `message` and `handle` fom `request`. Using a cursor, this function will then insert the `message` into the database of messages. We run `db.commit()` after the insertion to ensure that our row insertion has been saved. Don't forget to close the database connection within this function!

In the next cell, we'll write a function to render the `submit.html` template. Since our submission page will both transmit and receive data, we should ensure that the page suppots both `POST` and `GET` methods. In the `GET` method, this function will render the `submit.html` template with no other parameters. In the `POST` method, this function will first call the `insert_message()` function and then render the `submit.html` template with `message` and `handle` as parameters.


```python
# ensure the submission page supports both POST and GET methods
@app.route("/submit/", methods = ["POST", "GET"])
def submit():
    # render the submit.html template in the GET case
    if request.method == "GET":
        return render_template("submit.html")
    # call the insert_message() function 
    # and render the submit.html with parameters
    # in the POST case
    else:
        message, handle = insert_message(request)
        return render_template("submit.html", message=message, handle=handle)
```

## Viewing Random Messages

Next, we'll create a template called `view.html` to display a random collection of messages. We have this template extend the `base.html` template.

```
{% extends 'base.html' %}

{% block header %}
  <h1>{% block title %}Some Cool Messages{% endblock %}</h1>
{% endblock %}

{% block content %}
{% if messages %}
  {% for message in messages %}
   {{message[2]}}
   <br>
   <i>- {{message[1]}}</i>
   <br>
   <br>
  {% endfor %}
{% endif %}
{% endblock %}
```

Here's what we should note:

1. The title of our viewing page is `Some Cool Messages`.

2. We take advantage of looping and indexing of objects.

Here's an example of our viewing page:

![viewing-page.png](/images/viewing-page.png)

Now, we'll write a function to extract a collection of random messages from the database. Don't forget to close the database connection within the function!


```python
def random_messages(n):
    # connect to the database
    db = get_message_db()

    # use a cursor to select a collection of n random messages
    # from the database
    cursor = db.cursor()
    cursor.execute(f'SELECT * FROM messages ORDER BY RANDOM() LIMIT {n}')
    result = cursor.fetchall()

    # close the database connection
    db.close()

    return(result)
```

In the next cell, we'll write a function to render the `view.html` template. This function should first call the `random_messages()` function to collect 5 random messages and then pass these messages as an argument to the `render_template()` function.


```python
@app.route("/view/")
def view():
    # grab 5 random messages
    messages = random_messages(5)
    # render the view.html template with the messages as an argument
    return render_template("view.html", messages=messages)
```

## Customize Our App

Lastly, we'll use CSS to make our webapp look attractive and interesting. We change the fonts and colors of our webapp in a file called `style.css`:

```
html {
    font-family: Monaco, monospace;
    background: PowderBlue;
    padding: 1rem;
}

h1 {
    color: rgb(240, 156, 30);
    font-family: Papyrus, fantasy;
    font-size: 30px;
    margin: 1rem 0;
}
```
