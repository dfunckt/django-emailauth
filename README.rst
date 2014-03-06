django-emailauth
^^^^^^^^^^^^^^^^

``emailauth`` provides seamless email-based authentication for Django. It
leverages Django's own ``contrib.auth`` package, extending where appropriate.


Requirements
============

``emailauth`` requires Django 1.5 or newer and Python 2.6/3.2 or newer.


How to install
==============

Using pip::

    $ pip install git+https://github.com/dfunckt/django-emailauth.git#egg=django-emailauth

Manually::

    $ git clone https://github.com/dfunckt/django-emailauth.git
    $ cd django-emailauth
    $ python setup.py install


How to use
==========

Make sure you have ``django.contrib.auth`` and ``django.contrib.contenttypes``
in your ``INSTALLED_APPS``. (See the Django documentation_ for more details).

Add ``emailauth`` to your ``INSTALLED_APPS``::

    INSTALLED_APPS = (
        # ...
        'django.contrib.contenttypes',
        'django.contrib.auth',
        
        'emailauth',
        # ...
    )

Order does not matter.

Set ``AUTH_USER_MODEL`` in your settings::

    AUTH_USER_MODEL = 'emailauth.User'

It's `important to note`_ that you must set ``AUTH_USER_MODEL`` *before*
creating any migrations or running ``manage.py migrate`` for the first time.

Run ``migrate`` to create the database tables::

    $ python manage.py migrate emailauth

You're done. Remember to always use `get_user_model()`_ when you need to
reference the user model.

.. _documentation: https://docs.djangoproject.com/en/1.7/topics/auth/
.. _important to note: https://docs.djangoproject.com/en/1.7/topics/auth/customizing/#substituting-a-custom-user-model
.. _get_user_model(): https://docs.djangoproject.com/en/1.7/topics/auth/customizing/#django.contrib.auth.get_user_model


Extending ``emailauth``
=======================

``emailauth`` can be used either *standalone* as a direct replacement of
``django.contrib.auth`` as described, or as a *base* to assist you with
boilerplate in order to implement your own email-based user model.

``emailauth`` provides both an abstract and a concrete user model. If you want
to create your own email-based user model you should subclass
``emailauth.models.AbstractUser``. For example, to add a ``phone`` field to
your user, you might do something like the following::

    from django.db import models
    from emailauth.models import AbstractUser
    
    class User(AbstractUser):
        phone = models.CharField(max_length=15, blank=True)

Remember to change ``AUTH_USER_MODEL`` to *your* user model in order to be
picked up by Django::

    AUTH_USER_MODEL = 'myapp.User'
