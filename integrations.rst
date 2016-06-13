============
Integrations
============

STUPS
=====

Plan B can be seamlessly integrated into the `STUPS infrastructure`_:

* The `mint worker`_ can automatically create and rotate application OAuth users for all applications registered in Kio_
* The `mint worker`_ can distribute OAuth credentials via S3 buckets

Libraries
=========

The following libraries are compatible with Plan B:

Clients
-------

* The `STUPS Java tokens library`_ supports token creation using the Plan B Provider Token Endpoint
* The `STUPS Python tokens library`_ supports token creation using the Plan B Provider Token Endpoint
* The `Go tokens library`_ also supports token creation using the Plan B Provider Token Endpoint

There are many libraries for the OAuth2 authorization code grant flow (which is supported by Plan B), e.g. the `Flask-OAuth`_ library for Python. 

Resource Servers
----------------

* The Python Flask REST framework Connexion_ supports token validation against Plan B Token Info
* The Clojure REST framework Friboo_ supports token validation against Plan B Token Info
* The Java `Spring-OAuth2 STUPS Support`_ library supports token validation against Plan B Token Info when implementing resource servers with the Spring framework

.. _STUPS infrastructure: https://stups.io
.. _mint worker: http://docs.stups.io/en/latest/components/mint.html
.. _Kio: http://docs.stups.io/en/latest/components/kio.html
.. _STUPS Java tokens library: https://github.com/zalando-stups/tokens
.. _STUPS Python tokens library: https://github.com/zalando-stups/python-tokens
.. _Go tokens library: https://github.com/zalando/go-tokens
.. _Connexion: https://github.com/zalando/connexion
.. _Friboo: https://github.com/zalando-stups/friboo
.. _Spring-OAuth2 STUPS Support: https://github.com/zalando-stups/stups-spring-oauth2-support
.. _Flask-OAuth: https://pythonhosted.org/Flask-OAuth/

