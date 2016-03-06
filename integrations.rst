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


Resource Servers
----------------

* The Python Flask REST framework Connexion_ supports token validation against Plan B Token Info
* The Clojure REST framework Friboo_ supports token validation against Plan B Token Info





.. _STUPS infrastructure: https://stups.io
.. _mint worker: http://docs.stups.io/en/latest/components/mint.html
.. _Kio: http://docs.stups.io/en/latest/components/kio.html
.. _STUPS Java tokens library: https://github.com/zalando-stups/tokens
.. _STUPS Python tokens library: https://github.com/zalando-stups/python-tokens
.. _Connexion: https://github.com/zalando/connexion
.. _Friboo: https://github.com/zalando-stups/friboo

