.. image:: https://raw.githubusercontent.com/ClearcodeHQ/pytest-mysql/master/logo.png
    :width: 100px
    :height: 100px
    
pytest-mysql
============

.. image:: https://img.shields.io/pypi/v/pytest-mysql.svg
    :target: https://pypi.python.org/pypi/pytest-mysql/
    :alt: Latest PyPI version

.. image:: https://img.shields.io/pypi/wheel/pytest-mysql.svg
    :target: https://pypi.python.org/pypi/pytest-mysql/
    :alt: Wheel Status

.. image:: https://img.shields.io/pypi/pyversions/pytest-mysql.svg
    :target: https://pypi.python.org/pypi/pytest-mysql/
    :alt: Supported Python Versions

.. image:: https://img.shields.io/pypi/l/pytest-mysql.svg
    :target: https://pypi.python.org/pypi/pytest-mysql/
    :alt: License

Package status
--------------

.. image:: https://travis-ci.org/ClearcodeHQ/pytest-mysql.svg?branch=v2.0.2
    :target: https://travis-ci.org/ClearcodeHQ/pytest-mysql
    :alt: Tests

.. image:: https://coveralls.io/repos/ClearcodeHQ/pytest-mysql/badge.png?branch=v2.0.2
    :target: https://coveralls.io/r/ClearcodeHQ/pytest-mysql?branch=v2.0.2
    :alt: Coverage Status

.. image:: https://requires.io/github/ClearcodeHQ/pytest-mysql/requirements.svg?tag=v2.0.2
     :target: https://requires.io/github/ClearcodeHQ/pytest-mysql/requirements/?tag=v2.0.2
     :alt: Requirements Status

What is this?
=============

This is a pytest plugin, that enables you to test your code that relies on a running MySQL Database.
It allows you to specify fixtures for MySQL process and client.

.. warning::

    Only MySQL 5.7.6 and up are supported. For older versions, please use pytest-mysql 2.0.2
    Although Pull Request to add back support for older MySQL versions are welcome.

How to use
==========

Plugin contains two fixtures

* **mysql** - it's a client fixture that has functional scope. After each test drops test database from MySQL ensuring repeatability.
* **mysql_proc** - session scoped fixture, that starts MySQL instance at it's first use and stops at the end of the tests.

Simply include one of these fixtures into your tests fixture list.

You can also create additional mysql client and process fixtures if you'd need to:


.. code-block:: python

    from pytest_mysql import factories

    mysql_my_proc = factories.mysql_proc(
        port=None, logsdir='/tmp')
    mysql_my = factories.mysql('mysql_my_proc')

.. note::

    Each MySQL process fixture can be configured in a different way than the others through the fixture factory arguments.

Configuration
=============

You can define your settings in three ways, it's fixture factory argument, command line option and pytest.ini configuration option.
You can pick which you prefer, but remember that these settings are handled in the following order:

    * ``Fixture factory argument``
    * ``Command line option``
    * ``Configuration option in your pytest.ini file``

+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| MySQL option             | Fixture factory argument | Command line option | pytest.ini option | Default                   |
+==========================+==========================+=====================+===================+===========================+
| Path to executable       | mysqld_exec              | --mysql-mysqld      | mysql_mysqld      | mysqld                    |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| Path to safe executable  | mysqld_safe              | --mysql-mysqld-safe | mysql_mysqld_safe | /usr/bin/mysqld_safe      |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| Path to mysql_install_db | install_db               | --mysql-install-db  | mysql_install_db  | /usr/bin/mysql_install_db |
| for legacy installations |                          |                     |                   |                           |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| Path to Admin executable | admin_executable         | --mysql-admin       | mysql_admin       | /usr/bin/mysqladmin       |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| host                     | host                     | --mysql-host        | mysql_host        | localhost                 |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| port                     | port                     | --mysql-port        | mysql_port        | random                    |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| MySQL user to work with  | user                     | --mysql-user        | mysql_user        | root                      |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| User's password          | passwd                   | --mysql-passwd      | mysql_passwd      |                           |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| Test database name       | dbname                   | --mysql-dbname      | mysqldbname       | test                      |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| Starting parameters      | params                   | --mysql-params      | mysql_params      |                           |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+
| Log directory location   | logsdir                  | --mysql-logsdir     | mysql_logsdir     | $TMPDIR                   |
+--------------------------+--------------------------+---------------------+-------------------+---------------------------+

Example usage:

* pass it as an argument in your own fixture

    .. code-block:: python

        mysql_proc = factories.mysql_proc(
            port=8888)

* use ``--mysql-port`` command line option when you run your tests

    .. code-block::

        py.test tests --mysql-port=8888


* specify your port as ``mysql_port`` in your ``pytest.ini`` file.

    To do so, put a line like the following under the ``[pytest]`` section of your ``pytest.ini``:

    .. code-block:: ini

        [pytest]
        mysql_port = 8888

Running on Docker/as root
=========================

Unfortunately, running MySQL as root (thus by default on docker) is not possible.
MySQL (and MariaDB as well) will not allow it.

.. code-block::

    USER nobody

This line should switch your docker process to run on user nobody. See `this comment for example <https://github.com/ClearcodeHQ/pytest-mysql/issues/62#issuecomment-367975723>`_

Package resources
-----------------

* Bug tracker: https://github.com/ClearcodeHQ/pytest-mysql/issues
