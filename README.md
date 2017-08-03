# DjangoTutorial

## Quick install guide

### Install Django

    pip install Django

### Verifying

    >>> import django
    >>> print(django.get_version())

## Writing your first Django app, part 1

### Creating a project

You’ll need to auto-generate some code that establishes a Django project – a collection of settings for an instance of Django, including database configuration, Django-specific options and application-specific settings.

From the command line, cd into a directory where you’d like to store your code, then run the following command:

    $ django-admin startproject mysite

You’ll need to avoid naming projects after built-in Python or Django components.

The outer _mysite/_ root directory is just a container for your project. Its name doesn’t matter to Django; you can rename it to anything you like.

The inner _mysite/_ directory is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. mysite.urls).

### The development server

Change into the outer mysite directory, if you haven’t already, and run the following commands:

    $ python manage.py runserver

By default, the runserver command starts the development server on the internal IP at port 8000. If you want to change the server’s port, pass it as a command-line argument.

    $ python manage.py runserver 8080

If you want to change the server’s IP, pass it along with the port. For example, to listen on all available public IPs (which is useful if you are running Vagrant or want to show off your work on other computers on the network), use:

    $ python manage.py runserver 0:8000

### Creating the Polls app

Now that your environment – a “project” – is set up, you’re set to start doing work. A **project** is a collection of configuration and apps for a particular website. A project can contain multiple apps. An app can be in multiple projects.

Each **application** you write in Django consists of a Python package that follows a certain convention. Django comes with a utility that automatically generates the basic directory structure of an app, so you can focus on writing code rather than creating directories.

Your apps can live anywhere on your Python path. In this tutorial, we’ll create our poll app right next to your manage.py file so that it can be imported as its own top-level module, rather than a submodule of mysite.

To create your app, make sure you’re in the same directory as manage.py and type this command:

    $ python manage.py startapp polls

### Write your first view

To call the view, we need to map it to a URL - and for this we need a URLconf. To create a URLconf in the _polls_ directory, create a file called _urls.py_. The next step is to point the root URLconf at the polls.urls module.

## Writing your first Django app, part 2

### Database setup

_mysite/settings.py_ is a normal Python module with module-level variables representing Django settings.

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        },
        'sid': {
            'ENGINE': 'django.db.backends.oracle',
            'NAME': 'sid',
            'USER': 'username',
            'PASSWORD': 'password',
            'HOST': 'ip',
            'PORT': 'port',
        },
        'server_name': {
            'ENGINE': 'django.db.backends.oracle',
            'NAME': 'ip:port/service_name',
            'USER': 'username',
            'PASSWORD': 'password',
        }
    }

The _migrate_ command looks at the _INSTALLED\_APPS_ setting and creates any necessary database tables according to the database settings in your mysite/settings.py file and the database migrations shipped with the app. The _migrate_ command will only run migrations for apps in INSTALLED_APPS.

    $ python manage.py migrate

### Creating models

A model is the single, definitive source of truth about your data. It contains the essential fields and behaviors of the data you’re storing.

You can use an optional first positional argument to a _Field_ to designate a human-readable name. That’s used in a couple of introspective parts of Django, and it doubles as documentation.

### Activating models

To include the app in our project, we need to add a reference to its configuration class in the _INSTALLED\_APPS_ setting. The _PollsConfig_ class is in the _polls/apps.py_ file, so its dotted path is 'polls.apps.PollsConfig'.

By running _makemigrations_, you’re telling Django that you’ve made some changes to your models (in this case, you’ve made new ones) and that you’d like the changes to be stored as a migration.

    $ python manage.py makemigrations polls

Migrations are how Django stores changes to your models (and thus your database schema) - they’re just files on disk.

The _sqlmigrate_ command takes migration names and returns their SQL. The sqlmigrate command doesn’t actually run the migration on your database - it just prints it to the screen so that you can see what SQL Django thinks is required.

    $ python manage.py sqlmigrate polls 0001

The following command checks for any problems in your project without making migrations or touching the database.

    $ python manage.py check

In short, the three-step guide to making model changes:

* Change your models (in _models.py_).
* Run _python manage.py makemigrations_ to create migrations for those changes
* Run _python manage.py migrate_ to apply those changes to the database.