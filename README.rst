ASP.NET View State Decoder
==========================

A small Python 3.5+ library for decoding ASP.NET viewstate.

Viewstate is a method used in the ASP.NET framework to persist changes to a web form across postbacks. It is usually saved on a hidden form field:

.. code-block:: html

   <input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="/wEP...">

Decoding the view state can be useful in penetration testing on ASP.NET applications, as well as revealing more information that can be used to efficiently scrape web pages.

.. image:: https://github.com/yuvadm/viewstate/workflows/Build/badge.svg
    :target: https://github.com/yuvadm/viewstate/actions

.. image:: https://img.shields.io/pypi/v/viewstate
    :target: https://pypi.org/project/viewstate/

Install
-------

.. code-block:: shell

   $ pip install viewstate

Usage
-----

The Viewstate decoder accepts Base64 encoded .NET viewstate data and returns the decoded output in the form of plain Python objects.

There are two main ways to use this package. First, it can be used as an imported library with the following typical use case:

.. code-block:: python

  >>> from viewstate import ViewState
  >>> base64_encoded_viewstate = '/wEPBQVhYmNkZQ9nAgE='
  >>> vs = ViewState(base64_encoded_viewstate)
  >>> vs.decode()
  ('abcde', (True, 1))

It is also possible to feed the raw bytes directly:

.. code-block:: python

  >>> vs = ViewState(raw=b'\xff\x01....')

Alternatively, the library can be used via command line by directly executing the module:

.. code-block:: shell

  $ cat data.base64 | python -m viewstate

Which will pretty-print the decoded data structure.

The command line usage can also accept raw bytes with the ``-r`` flag:

.. code-block:: shell

  $ cat data.base64 | base64 -d | python -m viewstate -r

Viewstate HMAC signatures are also supported. In case there are any remaining bytes after parsing, they are assumed to be HMAC signatures, with the types estimated according to signature length.

.. code-block:: python

   >>> vs = ViewState(signed_view_state)
   >>> vs.decode()
   >>> vs.mac
   'hmac_sha256'
   >>> vs.signature
   b'....'

Development
-----------

.. code-block:: shell

  $ pytest

References
----------

The work on this library is based on publically available implementations from Microsoft:

- https://msdn.microsoft.com/en-us/library/ms972976.aspx
- https://referencesource.microsoft.com/#System.Web/UI/ObjectStateFormatter.cs,45

Previous versions were based on prior work:

- http://viewstatedecoder.azurewebsites.net/
- https://github.com/mutantzombie/JavaScript-ViewState-Parser

License
-------
MIT
