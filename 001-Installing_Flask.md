# Introduction to Flask: Installation Guide

In this post, we'll walk through installing Flask and setting up a virtual environment.

Flask has three primary dependencies: routing, debugging, and the Web Server Gateway Interface (WSGI) from Werkzeug; the templating engine from Jinja2; and command-line integration from Click. Additional services are available via dependencies.

> This tutorial assumes you're using a Linux-like environment with Python 3.x installed.
> We'll name our application "flasky".

## 1. Create the Application Directory

Create an empty directory:
```sh
mkdir flasky && cd flasky
```

## 2. Create a Virtual Environment

The `venv` package required for the next step is not included by default in Linux installations.

To install the `venv` package:

```sh
sudo apt-get install python3-venv
```

To create a virtual environment, type:

```sh
python3 -m venv venv
```
> Our virtual environment is called "venv" and it is installed in our app root directory

If successfully created, you should see the "venv" directory inside your working directory.

## 3. Activate / Deactivate Your Virtual Environment

To activate the virtual environment:

```sh
source venv/bin/activate
```
From now on, you will now see the venv name in your prompt:

```console
(venv) ***@laptop:~/Workspace/flasky$
```

When a virtual environment is activated, its local Python interpreter location is added to the PATH variable. When you're done working with your virtual environment, type `deactivate` to restore the PATH value. Otherwise your venv will stop automatically when you close the console.

## 4. Install Flask

```sh
pip install flask
```

```console
Collecting flask
  Using cached flask-3.0.3-py3-none-any.whl (101 kB)
Collecting Jinja2>=3.1.2
  Using cached jinja2-3.1.4-py3-none-any.whl (133 kB)
Collecting click>=8.1.3
  Using cached click-8.1.7-py3-none-any.whl (97 kB)
Collecting Werkzeug>=3.0.0
  Using cached werkzeug-3.0.3-py3-none-any.whl (227 kB)
Collecting blinker>=1.6.2
  Using cached blinker-1.8.2-py3-none-any.whl (9.5 kB)
Collecting itsdangerous>=2.1.2
  Using cached itsdangerous-2.2.0-py3-none-any.whl (16 kB)
Collecting MarkupSafe>=2.0
  Using cached MarkupSafe-2.1.5-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Installing collected packages: MarkupSafe, itsdangerous, click, blinker, Werkzeug, Jinja2, flask
Successfully installed Jinja2-3.1.4 MarkupSafe-2.1.5 Werkzeug-3.0.3 blinker-1.8.2 click-8.1.7 flask-3.0.3 itsdangerous-2.2.0
```

## 5. Check Installation and Dependency Versions

```sh
pip freeze
```
You should see something like:

```console
blinker==1.8.2
click==8.1.7
Flask==3.0.3
itsdangerous==2.2.0
Jinja2==3.1.4
MarkupSafe==2.1.5
Werkzeug==3.0.3
```
That's it! You can now start working on your new Flask project.
