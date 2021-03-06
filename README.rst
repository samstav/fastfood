fastfood
========

| |latest pypi release|  |Build Status on Travis for 'master' branch|  |Requirements Status from requires.io|

|

| ========

| Helps build cookbooks faster by pre-templating parts and exposing
| options in a command line and config friendly way.

Installation
------------

The latest release of fastfood can be installed via pip:

::

    pip install fastfood

| An alternative install method would be manually installing it
  leveraging
| ``setup.py``:

::

    git clone https://github.com/rackerlabs/fastfood
    cd fastfood
    python setup.py install

Command Line Usage
------------------

list
~~~~

Shows a list of available stencils in your template pack.

Example:

::

    $ fastfood list
    Available Stencil Sets:
           varnish - Creates a recipe for installing Varnish
          ha-redis - Creates a highly available Redis and HAProxy recipe
              java - Installs Java JRE

show
~~~~

Shows more information about a stencil, including available options.

::

    $ fastfood show nginx
    Stencil Set nginx:
      Stencils:
        nginx
      Options:
        name - Name of the recipe to create
        example - Various premade Nginx examples

build
~~~~~

| Generates a new cookbook or updates an existing cookbook from a
  fastfood.json
| file.

Example Template:

.. code:: json

    {
      "name": "mycookbook",
      "stencils": [
        {
          "stencil_set": "base"
        },
        {
          "stencil_set": "rabbitmq",
          "openfor": "myapp"
        },
        {
          "stencil_set": "rails",
          "stencil": "nginx",
          "name": "myapp",
          "tag": "myapp"
        }
      ]
    }

Ex:

::

    fastfood build fastfood.json

Template Notes
--------------

| Fastfood uses the `Jinja2 <http://jinja.pocoo.org/>`__ templating
  engine with
| 2 modifications.

qstring()
~~~~~~~~~

| There is a helper method added to jinja2 for fastfood called qstring,
  it
| takes in an argument and if that argument does not match a Chef node
| attributes (node['mysomething'] \| node.chef\_environment) it will
  wrap that argument
| in a string otherwise it just returns the argument.

.. code:: ruby

    qstring("node['mysomething']")

    renders as

    node['mysomething']

and

::

    qstring("mynonchefstr")

    renders as

    "mynonchefstr"

jinja variable
~~~~~~~~~~~~~~

| Because the traditional jinja2 variable start and end strings can
  conflict
| with Ruby code fastfood uses '\|{' and '}\|' to represent a jinja2
  variable.

::

    |{ options['name'] }|

.. |latest pypi release| image:: https://img.shields.io/pypi/v/fastfood.svg
   :target: https://pypi.python.org/pypi/fastfood
.. |Build Status on Travis for 'master' branch| image:: https://travis-ci.org/rackerlabs/fastfood.svg?branch=master
   :target: https://travis-ci.org/checkmate/simpl
.. |Requirements Status from requires.io| image:: https://requires.io/github/rackerlabs/fastfood/requirements.svg?branch=master
   :target: https://requires.io/github/rackerlabs/fastfood/requirements/?branch=master
