==============
OpenID Connect
==============

The Plan B Provider does not yet fully implement all `OpenID Provider`_ endpoints.

Authorization Endpoint
======================

The Plan B Provider :ref:`authorization-endpoint` does not yet support the optional OpenID Connect request parameters.

Token Endpoint
==============

The Plan B Provider :ref:`token-endpoint` returns an ID token (``id_token``).

OpenID Provider Configuration Information Endpoint
==================================================

The Plan B Provider supports the `OpenID Connect Discovery`_ endpoint ``/.well-known/openid-configuration``.

Only the ``jwks_uri`` has a meaningful value in the OpenID Provider Configuration Response.



.. _OpenID Provider: http://openid.net/specs/openid-connect-core-1_0.html
.. _OpenID Connect Discovery: http://openid.net/specs/openid-connect-discovery-1_0.html
