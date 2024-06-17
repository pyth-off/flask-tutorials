# Database Management With Flask-SQLAlchemy

### Installing Flask-SQLAlchemy

First, install Flask-SQLAlchemy:

```sh
(venv) pip install flask-sqlalchemy
```

### Initializing SQLAlchemy

To handle circular dependencies, initialize the database in a separate `db.py` file:

```python
# db.py
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.ext.declarative import DeclarativeBase

class Base(DeclarativeBase):
    pass

db = SQLAlchemy(model_class=Base)
```

### Configuring and Initializing in Your Application

In your main application file (`index.py`), configure and initialize the database:

```python
# index.py
from db import db

app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
basedir = os.path.abspath(os.path.dirname(__file__))
path = 'sqlite:///{}/data.sqlite'.format(basedir)
app.config['SQLALCHEMY_DATABASE_URI'] = path

db.init_app(app)
```

### Creating Models

Define your models for a to-do list application:

#### List Model (`list/model.py`):

```python
import json
from db import db
from sqlalchemy.sql import func
from user.model import User

class List(db.Model):
    __tablename__ = 'list'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), nullable=False)
    active = db.Column(db.Boolean(), default=True)
    created = db.Column(db.DateTime(timezone=True), server_default=func.now())
    updated = db.Column(db.DateTime(timezone=True), onupdate=func.now())
    user_id = db.Column(db.Integer, db.ForeignKey(User.id), nullable=False)

    def __repr__(self):
        return json.dumps({k: str(v) for k, v in self.__dict__.items()})
```

#### List Item Model (`list_item/model.py`):

```python
import json
from db import db
from sqlalchemy.sql import func
from list.model import List

class ListItem(db.Model):
    __tablename__ = 'list_item'
    id = db.Column(db.Integer, primary_key=True)
    text = db.Column(db.String(128), nullable=False)
    done = db.Column(db.Boolean(), default=False)
    prio = db.Column(db.Integer)
    list_id = db.Column(db.Integer, db.ForeignKey(List.id))
    created = db.Column(db.DateTime(timezone=True), server_default=func.now())
    updated = db.Column(db.DateTime(timezone=True), onupdate=func.now())

    def __repr__(self):
        return json.dumps({k: str(v) for k, v in self.__dict__.items()})
```

#### User Model (`user/model.py`):

```python
import json
from db import db
from sqlalchemy.sql import func

class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True, nullable=False)
    email = db.Column(db.String(128), unique=True, nullable=False)
    password = db.Column(db.String(128), nullable=False)
    active = db.Column(db.Boolean(), default=True)
    created = db.Column(db.DateTime(timezone=True), server_default=func.now())
    updated = db.Column(db.DateTime(timezone=True), default=None, nullable=True, onupdate=func.now())

    def __repr__(self):
        return json.dumps({k: str(v) for k, v in self.__dict__.items()})
```

### List Routes and View Functions

Example of a route and view function for creating a list:

```python
# index.py
@app.route('/list_create', methods=['GET', 'POST'])
def list_create():
    form = ListForm()

    if form.validate_on_submit():
        list_id = ListModel.list_create(form.name.data, session['user']['id'])
        return redirect(url_for('list_display', list_id=list_id))

    return render_template('list_create.html', form=form, locale=session['locale'])
```

### List Form (`forms/list.py`)

Form definition using Flask-WTF:

```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired
from translator.translator import translate

class ListForm(FlaskForm):
    name = StringField(translate('Name'), validators=[DataRequired()])
    submit = SubmitField(translate('Save'))
```

### Managing Database Tables With Flask Shell

#### Creating and Dropping Tables

Use Flask shell to manage database tables:

```shell
(venv) $ flask shell
>>> from db import db
>>> db.create_all()
```

To drop all tables (development environment):

```shell
(venv) $ flask shell
>>> db.drop_all()
```

#### CRUD Operations

Basic operations using SQLAlchemy:

```shell
>>> list = List(id=1, name='Some New List', user_id=1)
>>> db.session.add(list)
>>> db.session.commit()

>>> lists = List.query.all()
>>> db.session.delete(list)
>>> db.session.commit()
>>> list.name = 'Some Other Name'
>>> db.session.add(list)
>>> db.session.commit()
```

### Conclusion

These steps demonstrate basic CRUD operations with Flask-SQLAlchemy, essential for managing relational databases in Flask applications. For advanced features, refer to the [SQLAlchemy documentation](https://docs.sqlalchemy.org/en/20/).