# Templates in Flask

The task of a view function is to generate a response to a request. A template file contains the text of a response along with placeholders to be replaced by actual values. The process of replacing placeholders with actual values is called "rendering."

The default template engine in Flask is Jinja2. Template files are usually located in the `/templates` directory.

To link a function with a template, use the `render_template()` function:

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')  # Assuming the file exists in the /templates directory

@app.route('/user/<name>')
def user(name):
    return render_template('user.html', name=name)  # Assuming the file exists in the /templates directory
```

## 1. Dereferencing Variables in Templates with Jinja2

In the example above, the `name` variable can be accessed in the template like this:

```html
<h1>{{ name }}</h1>
```

Other variable types can be dereferenced as follows:

```html
<p>Dictionary value: {{ myDict['key'] }}</p>
<p>List value: {{ myList[2] }}</p>
<p>List value with a variable index: {{ myList[myIndexValue] }}</p>
<p>A value returned from an object's method: {{ myObject.method() }}</p>
```

## 2. Filtering Variables

Variables can be filtered, meaning they can be transformed by calling a function on the variable value. For example:

```html
<p>Good morning, {{ name|capitalize }}!</p>
```

A comprehensive list of Jinja2 filters can be found [here](https://jinja.palletsprojects.com/en/3.1.x/templates/#builtin-tests).

## 3. Control Structures

Here are some examples of the most used control structures. For a comprehensive list of available structures and special variables, refer to [Jinja2's documentation](https://jinja.palletsprojects.com/en/3.1.x/templates/#list-of-control-structures).

### For Loop

```html
<h1>Members</h1>
<ul>
{% for user in users %}
  <li>{{ user.username|e }}</li>
{% endfor %}
</ul>
```

### If Statement

```html
{% if users %}
<ul>
{% for user in users %}
    <li>{{ user.username|e }}</li>
{% endfor %}
</ul>
{% endif %}
```

## 4. Flask-Bootstrap

Bootstrap-Flask is a collection of Jinja macros for Bootstrap and Flask. It helps you render Flask-related data and objects to Bootstrap markup HTML more easily.

```python
from flask import Flask
from flask_bootstrap import Bootstrap

app = Flask(__name__)
bootstrap = Bootstrap(app)
```

For flexibility, Bootstrap-Flask doesn’t include built-in base templates (this may change in the future). For now, you have to create a base template yourself. Be sure to use an HTML5 doctype and include a viewport meta tag for proper responsive behaviors. Here’s an example base template:

```html
{% extends "bootstrap/base.html" %}
    <title>{% block title %}My Application{% endblock %}</title>
    {{ bootstrap.load_css() }}
    {{ bootstrap.load_js() }}
</head>
<body>
    <div class="container">
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```

Bootstrap-Flask provides a demo application that contains all the code snippets for the macros and the corresponding render output. See the [demo application](https://github.com/greyli/bootstrap-flask) for details.

## 5. Custom Error Pages

You can customize your error pages by adding the following to your code:

```python
@app.errorhandler(404)
def page_not_found(e):
    return render_template('404.html'), 404
```

An example 404.html looks like this

´´´html
{% extends "base.html" %}
{% block content %}
<h1 class="alert alert-danger text-center">Sorry, cannot find your page (404)</h1>
{% endblock %}
´´´

This concludes the introduction to templates with Jinja2 and Bootstrap. You are now ready to move on to web forms.
