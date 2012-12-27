Introduction
============

The ``any_urlfield`` module allows users to enter URLs to internal models and external URLs.

This addresses is a common challenge in CMS interfaces;
where providing a ``URLField`` makes it hard to enter internal URLs,
while providing a ``ModelChoiceField`` makes it too inflexible.

Relevant public classes:

* Model fields:

 * ``AnyUrlField``: allow users to choose either a model or external link as URL value

* Form widget rendering:

 * ``HorizonatalRadioFieldRenderer``
 * ``SimpleRawIdWidget``


Screenshot
==========

.. figure:: https://github.com/edoburu/django-any-urlfield/raw/master/docs/images/anyurlfield1.png
   :width: 363px
   :height: 74px
   :alt: AnyUrlField, with external URL input.

.. figure:: https://github.com/edoburu/django-any-urlfield/raw/master/docs/images/anyurlfield2.png
   :width: 290px
   :height: 76px
   :alt: AnyUrlField, with internal page input.


Installation
============

First install the module, preferably in a virtual environment. It can be installed from PyPI::

    pip install django-any-urlfield

Or the current folder can be installed::

    pip install .

Configuration
-------------

Add the module to the installed apps::

    INSTALLED_APPS += (
        'any_urlfield',
    )

Usage
-----

In a Django model, the field can be included::

    from django.db import models
    from django.utils.translation import ugettext_lazy as _
    from any_urlfield.models.fields import AnyUrlField

    class MyModel(models.Model):
        title = models.CharField(_("title"), max_length=200)
        url = AnyUrlField(_("URL"))

By default, the ``AnyUrlField`` only supports linking to external pages.
To add support for your own models (e.g. an ``Article`` model),
include the following code in ``models.py``::

    from any_urlfield.models import AnyUrlField
    AnyUrlField.register_model(Article)

Now, the ``AnyUrlField`` offers users a dropdown field to directly select an article.
By default, it uses a ``django.forms.models.ModelChoiceField`` field with a ``django.forms.widgets.Select`` widget
to render the field.  This can be customized using the ``form_field`` and ``widget`` parameters::

    from any_urlfield.models import AnyUrlField
    from any_urlfield.forms.widgets import SimpleRawIdWidget

    AnyUrlField.register_model(Article, widget=SimpleRawIdWidget(Article))

Now, the ``Article`` model will be displayed as raw input field with a browse button.
