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

### Playing with the API

To invoke the Python shell, use this command:

    $ python manage.py shell

The following django database API can be used:

    >>> from polls.models import Question, Choice
    >>> Question.objects.all()
    >>> from django.utils import timezone
    >>> q = Question(question_text="What's new?", pub_date=timezone.now())
    >>> q.save()
    >>> q.id
    >>> q.question_text
    >>> q.pub_date
    >>> q.question_text = "What's up?"
    >>> q.save()

    # Django provides a rich database lookup API that's entirely driven by
    # keyword arguments.
    >>> Question.objects.filter(id=1)
    >>> Question.objects.filter(question_text__startswith='What')
    >>> current_year = timezone.now().year
    >>> Question.objects.get(pub_date__year=current_year)

    # Request an ID that doesn't exist, this will raise an exception.
    >>> Question.objects.get(id=2)

    # Lookup by a primary key is the most common case, so Django provides a
    # shortcut for primary-key exact lookups.
    # The following is identical to Question.objects.get(id=1).

    # Give the Question a couple of Choices. The create call constructs a new
    # Choice object, does the INSERT statement, adds the choice to the set
    # of available choices and returns the new Choice object. Django creates
    # a set to hold the "other side" of a ForeignKey relation
    # (e.g. a question's choice) which can be accessed via the API.
    >>> q = Question.objects.get(pk=1)
    >>> q.was_published_recently()
    >>> q.choice_set.create(choice_text='Not much', votes=0)
    >>> q.choice_set.create(choice_text='The sky', votes=0)
    >>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

    # Choice objects have API access to their related Question objects.
    >>> c.question
    >>> q.choice_set.all()
    >>> q.choice_set.count()

    # The API automatically follows relationships as far as you need.
    # Use double underscores to separate relationships.
    # This works as many levels deep as you want; there's no limit.
    # Find all Choices for any question whose pub_date is in this year
    # (reusing the 'current_year' variable we created above).
    >>> Choice.objects.filter(question__pub_date__year=current_year)
    >>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
    >>> c.delete()

It’s important to add _\_\_str\_\_()_ methods to your models, not only for your own convenience when dealing with the interactive prompt, but also because objects’ representations are used throughout Django’s automatically-generated admin.

### Introducing the Django Admin

#### Creating an admin user¶

    $ python manage.py createsuperuser

Now, open a Web browser and go to “/admin/” on your local domain – e.g., http://127.0.0.1:8000/admin/. You should see the admin’s login screen.

#### Make the poll app modifiable in the admin

## Writing your first Django app, part 3

### Overview

A view is a “type” of Web page in your Django application that generally serves a specific function and has a specific template.

In Django, web pages and other content are delivered by views. Each view is represented by a simple Python function (or method, in the case of class-based views). Django will choose a view by examining the URL that’s requested (to be precise, the part of the URL after the domain name). To get from a URL to a view, Django uses what are known as ‘URLconfs’. A URLconf maps URL patterns (described as regular expressions) to views.

### Writing more views

When somebody requests a page from your website – say, “/polls/34/”, Django will load the _mysite.urls_ Python module because it’s pointed to by the _ROOT\_URLCONF_ _setting_. It finds the variable named urlpatterns and traverses the regular expressions in order. After finding the match at '_^polls/_', it **strips off** the matching text ("_polls/_") and sends the remaining text – "34/" – to the ‘_polls.urls_’ URLconf for further processing. There it matches *r'^(?P<question_id>[0-9]+)/$'*, resulting in a call to the *detail()* view like so:

    detail(request=<HttpRequest object>, question_id='34')

The *question_id='34'* part comes from *(?P<question_id>[0-9]+)*. Using *parentheses* around a **pattern** “captures” the text matched by that **pattern** and sends it as an argument to the view function; *?P<question_id>* defines the **name** that will be used to identify the matched **pattern**; and [0-9]+ is a regular expression to match a sequence of digits (i.e., a number).

### Write views that actually do something

Each view is responsible for doing one of two things: returning an _HttpResponse_ object containing the content for the requested page, or raising an exception such as _Http404_.

Your project’s *TEMPLATES* *setting* describes how Django will load and render templates. The default settings file configures a *DjangoTemplates* backend whose *APP_DIRS* option is set to *True*. By convention *DjangoTemplates* looks for a “*templates*” subdirectory in each of the *INSTALLED_APPS*.

#### A shortcut: render()

The _render()_ function takes the request object as its first argument, a template name as its second argument and a dictionary as its optional third argument. It returns an _HttpResponse_ object of the given template rendered with the given context.

### Raising a 404 error

#### A shortcut: get_object_or_404()

The *get_object_or_404()* function takes a Django model as its first argument and an arbitrary number of keyword arguments, which it passes to the *get()* function of the model’s manager. It raises *Http404* if the object doesn’t exist.

There’s also a *get_list_or_404()* function, which works just as *get_object_or_404()* – except using *filter()* instead of *get()*. It raises *Http404* if the list is empty.

### Use the template system

The template system uses dot-lookup syntax to access variable attributes. In the example of *{{ question.question_text }}*, first Django does a *dictionary lookup* on the object *question*. Failing that, it tries an *attribute lookup* – which works, in this case. If attribute lookup had failed, it would’ve tried a *list-index lookup*.

Method-calling happens in the *{% for %}* loop: *question.choice_set.all* is interpreted as the Python code *question.choice_set.all()*, which returns an iterable of *Choice* objects and is suitable for use in the *{% for %}* tag.

### Removing hardcoded URLs in templates

Since you defined the _name_ argument in the _url()_ functions in the _polls.urls_ module, you can remove a reliance on specific URL paths defined in your url configurations by using the _{% url %}_ template tag.

### Namespacing URL names

## Writing your first Django app, part 4

### Write a simple form

Since we’re creating a POST form (which can have the effect of modifying data), we need to worry about Cross Site Request Forgeries. All POST forms that are targeted at internal URLs should use the {% csrf_token %} template tag.

_request.POST_ is a dictionary-like object that lets you access submitted data by key name. _request.POST_ values are always strings. Note that Django also provides request.GET for accessing GET data in the same way.

We are using the _reverse()_ function in the _HttpResponseRedirect_ constructor in this example. This function helps avoid having to hardcode a URL in the view function. It is given the name of the view that we want to pass control to and the variable portion of the URL pattern that points to that view.

### Use generic views: Less code is better

Generic views abstract common patterns to the point where you don’t even need to write Python code to write an app.
