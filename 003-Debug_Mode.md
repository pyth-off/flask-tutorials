# Debug Mode in Flask

Flask applications can be executed in debug mode, which enables the reloader and debugger modules by default.

- **Reloader**: Automatically reloads the web server whenever a file changes.
- **Debugger**: Provides inspection tools to analyze the code.

## 1. Enable Debug Mode

To enable debug mode, set the `FLASK_DEBUG` flag to 1 before running the application.

```sh
export FLASK_APP=hello.py
```

```sh
export FLASK_DEBUG=1
```

```sh
flask run
```

When you run the application, you should see output similar to this:

```console
(venv) ***@***:~/Workspace/flasky$ flask run
 * Serving Flask app 'hello.py'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: ***-***-***
```

With debug mode enabled, your Flask application will automatically reload upon detecting changes, and you'll have access to helpful debugging tools.
