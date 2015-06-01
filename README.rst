=============
datastoredict
=============

.. image:: https://travis-ci.org/soofaloofa/datastoredict.svg?branch=master
    :target: https://travis-ci.org/soofaloofa/datastoredict

Provides a durabledict_ implementation for Google AppEngine -- an in-memory dictionary backed by the AppEngine
Datastore.

.. _durabledict: https://github.com/disqus/durabledict

Usage
=====

DatastoreDict requires an ndb model to store values. By default, the model
contains a property called `value`.

::

    from datastoredict import DatastoreDict

    class MyModel(ndb.Model):
        """
        A datastore model for storing durabledict data.
        """
        value = ndb.PickleProperty()

    DatastoreDict(model=MyModel)

You can provide a different property to store the value by instantiating
DatastoreDict with the `value_col` argument.

::

    from datastoredict import DatastoreDict

    class MyModel(ndb.Model):
        """
        A datastore model for storing durabledict data.
        """
        custom_value = ndb.JsonProperty()

    DatastoreDict(model=MyModel, value_col='custom_value')

Once you have a DatastoreDict instance, use it like a regular dictionary.

::

    from datastoredict import DatastoreDict

    class SettingsModel(ndb.Model):
        """
        A datastore model for storing durabledict data.
        """
        value = ndb.PickleProperty()

    settings = DatastoreDict(model=SettingsModel)

    # Assign and retrieve a value from the dict
    settings['foo'] = 'bar'
    settings['foo']
    >>> 'bar'

    # Assign and retrieve another value
    settings['buzz'] = 'foogle'
    settings['buzz']
    >>> 'foogle'

    # Delete a value and access receives a KeyError
    del settings['foo']
    settings['foo']
    >>> KeyError

Development
===========

Using virtualenv is recommended for development.

::

    virtualenv .
    source ./bin/activate

Development Dependencies
------------------------

We use flake8 for checking both PEP-8 and common Python errors and invoke for
continuous integration.

::

    pip install -U flake8
    pip install -U invoke

You can list available build tasks with :code:`inv -l`.

Due to a limitation with Google Appengine, dependencies must be installed
locally before running tests. The fetch_deps make command will install
dependencies to the vendor directory.

::

    inv fetch_deps


Publishing to PyPI
=================
You need pip, setuptools and wheel to publish to PyPI.

::

    inv publish
