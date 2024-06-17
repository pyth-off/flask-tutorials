# Handling Forms with Flask-WTF

To manage forms in Flask, it's recommended to use Flask-WTF. You can install it using:

```sh
pip install flask-wtf
```

Flask-WTF doesn't require initialization, but you must set up a secret key. Add the following to your `hello.py` file:

```python
from flask import Flask, render_template
from flask_bootstrap import Bootstrap
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired

app = Flask(__name__)
bootstrap = Bootstrap(app)
app.config['SECRET_KEY'] = 'thisismysecretkey'
```

For this example, we'll create an HTML template to test the form's functionality. First, create a file named `templates/form.html`.

Next, add a route and create a form:

```python
# Define a simple form
class NameForm(FlaskForm):
    name = StringField('What is your name?', validators=[DataRequired()])
    submit = SubmitField('Submit')

# Define a route to display the form
@app.route('/form')
def form():
    form = NameForm()
    return render_template('form.html', form=form)
```

> Each form is a class that inherits from `FlaskForm`.

> Each form field can have one or more validators attached. For a comprehensive guide on fields and validators, visit [WTForms Documentation](https://wtforms.readthedocs.io/en/2.3.x/fields/).

In the `templates/form.html`, you have two options to display the form:

```html
{% extends "base.html" %}
{% block content %}
<h1>Form Test</h1>
<h3>Please fill in the form</h3>
<!-- Option 1: Classic rendering with more flexibility -->
<form method="post">
    {{ form.hidden_tag() }}
    {{ form.name.label }} {{ form.name(id='my_id') }}
    {{ form.submit(value='Send') }}
</form>
<!-- Option 2: WTF Bootstrap rendering with less code -->
{% import 'bootstrap/wtf.html' as wtf %}
{{ wtf.quick_form(form) }}
{% endblock %}
```

### Handling Form Submissions in the View

To handle form submissions, update the view function as follows:

```python
# Register the view function to handle GET and POST requests
@app.route('/form', methods=['GET', 'POST'])
def form():
    name = None
    form = NameForm()
    if form.validate_on_submit():
        name = form.name.data
    return render_template('form.html', form=form, name=name)
```

The template also needs some updates:

```html
{% extends "base.html" %}
{% block content %}
<h1>Form Test</h1>
{% if name %}
<h3>Hello {{ name }}!</h3>
{% else %}
<h3>Hello stranger!</h3>
{% endif %}
<!-- Classic form rendering -->
<form method="post">
    {{ form.hidden_tag() }}
    {{ form.name.label }} {{ form.name(id='my_id') }}
    {{ form.submit(value='Send') }}
</form>
{% endblock %}
```

### Flash Messages

Flash messages allow you to communicate with the user, often for errors, warnings, or success notifications. For example, "You have been successfully logged out." To add flash messages, update the view function:

```python
from flask import flash

@app.route('/form', methods=['GET', 'POST'])
def form():
    name = None
    form = NameForm()
    if form.validate_on_submit():
        name = form.name.data
        flash('Data received!')
    return render_template('form.html', form=form, name=name)
```

And update the template to display flash messages:

```html
<!-- Template code... -->
</form>
{% for msg in get_flashed_messages() %}
<div class="alert alert-warning">{{ msg }}</div>
{% endfor %}
{% endblock %}
```

This concludes the introduction to handling forms, validation, and flash messages in Flask. You can now proceed to working with databases.