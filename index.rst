.. toctree::
   :maxdepth: 2
   :hidden:

   Data types and structures
   Service order api
   Cost proposals
   Messages
   Service provider API

API basics
==========

Naming convention
-----------------

Endpoints
~~~~~~~~~

All endpoints are named with following schema:

``https://serviceorderhub.com/api/version/action`` or
``https://serviceorderhub.com/api/version/entity/action``

Currently only ``v1`` is a valid ``version``. Words inside path string
must be separated with ``-`` sign.

Input
-----

Charset
~~~~~~~

Only ``utf-8`` charset is supported.

Encoding
~~~~~~~~

You can pass ``gzip`` or ``deflate`` inside ``Accept-Encoding`` header,
or omit it.

``Accept-Encoding: gzip``

Parameters
~~~~~~~~~~

All parameters inside query string are notation indifferent. You can use
either ``camelCase``, ``snake_case`` or even ``kebab-case``. Any of
these notations are valid. Next in this document only camelCase will be
used. Any unrecognized parameters (not described in this document) will
throw validation error and ``401`` status code.

For GET requests parameters should be passed with GET parameters inside
query string.

Example:

``/api/v1/accessory?accessToken=my_key&productType=1009``

For POST requests only ``accessToken`` is allowed inside GET parameters.
Anything other must be stored inside request body.

Languages
~~~~~~~~~

Language and locale are defined inside user settings. Typically one user
is assigned to single country and single locale (but this can be
improved in future).

You can pass ``Accept-Language`` header to determine in which locale
output data should be. For example:

``Accept-Language: sv-se, da-dk``

Currently only followed locales are supported:

-  sv-se
-  da-dk
-  nb-no
-  fi-fi

*This list may be continued*

**Note** This feature works only for "common" data like accessory, or
product types.

Accept data types
~~~~~~~~~~~~~~~~~

You can pass ``Accept`` HTTP header. Following types are valid:

-  ``text/html``
-  ``application/json``
-  ``text/xml`` - Currently not implemented

Response will contain the same ``Content-type`` as request's ``Accept``.
When ``text/html`` is selected, output will be wrapped inside simple
HTML page (This is useful for debug).

Output
------

Currently only JSON and HTML output is supported. *XML may be added in
future*

JSON
~~~~

Each response is wrapped with object contained ``code`` and ``data``
fields. Payload is stored inside ``data`` field. When everything is ok
response body will be like this:

.. code:: js

    {
      "data": [ // Payload
        { "id": 1, name: "Something" },
        { "id": 2, name: "Something other" }
      ]
    }

Also some extra fields may be added, for example offset/limit for
pagination

When an error is occurred response body will be like this:

.. code:: js

    {
      "error": {                // Exists only if there are any recognized reason of the error
        "message": "Validation error"
      },
      "deatils": { // Optional
      
      }
    }

HTML
~~~~

HTML output is just a "prettified" version of JSON

Authentication
--------------

Each request to API must be followed with special **authentication
token**. Requests without it will be rejected with ``401`` status code.

There are some different ways to add your access token to request:

X-Hub-Api-Token header
~~~~~~~~~~~~~~~~~~~~~~

You can pass your access key using **X-Hub-Api-Token** header.

``X-Hub-Api-Token: YOUR_TOKEN``

accessToken with GET parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Just add ``accessToken=YOUR_TOKEN`` to GET params. For example:

``/some-endpoint/?param1=X&param2=Y&accessToken=YOUR_TOKEN``

Other methods
~~~~~~~~~~~~~

Other auth methods will be added soon.
