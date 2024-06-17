# Command Line Tools in Flask

Flask provides a command-line interface (CLI) that offers various utilities for managing your application. Here is an overview of how to use these tools.

## Basic Commands

To see the available commands, use:

```sh
flask --help
```

Example output:

```console
Usage: flask [OPTIONS] COMMAND [ARGS]...

  This shell command acts as a general utility script for Flask applications.

  It loads the application configured (through the FLASK_APP environment
  variable) and then provides commands either provided by the application or
  Flask itself.

  The most useful commands are the "run" and "shell" commands.

  Example usage:

    $ export FLASK_APP=hello.py
    $ export FLASK_DEBUG=1
    $ flask run

Options:
  --version  Show the Flask version
  --help     Show this message and exit.

Commands:
  db         Perform database migrations.
  deploy     Run deployment tasks.
  profile    Start the application under the code...
  run        Runs a development server.
  shell      Runs a shell in the app context.
  test       Run the unit tests.
```

To get further help and possible parameters for each command, type `flask <COMMAND> --help`. For example:

```sh
flask db --help
```

Example output:

```console
Usage: flask db [OPTIONS] COMMAND [ARGS]...

  Perform database migrations.

Options:
  --help  Show this message and exit.

Commands:
  branches   Show current branch points.
  current    Display the current revision for each...
  downgrade  Revert to a previous version.
  edit       Edit a revision file.
  heads      Show current available heads in the script...
  history    List changeset scripts in chronological...
  init       Creates a new migration repository.
  merge      Merge two revisions together, creating a new...
  migrate    Autogenerate a new revision file (Alias for...
  revision   Create a new revision file.
  show       Show the revision denoted by the given...
  stamp      'stamp' the revision table with the given...
  upgrade    Upgrade to a later version.
```

## Common Commands

### `run`

Runs a development server:

```sh
flask run
```

### `shell`

Opens a Python shell session in the application context:

```sh
flask shell
```

### `db`

Performs database migrations. For example, to initialize a new migration repository:

```sh
flask db init
```

You are now ready to add code and extend your application. The Flask CLI provides a powerful set of tools to streamline development and manage your Flask projects efficiently.
