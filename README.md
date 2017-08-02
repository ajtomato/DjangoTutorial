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
