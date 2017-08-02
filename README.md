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
