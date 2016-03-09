=========
OAuth 2.0
=========

Plan B tries to be compliant with `RFC 6749 "The OAuth 2.0 Authorization Framework"`_;
the Plan B Provider implements an OAuth Authorization Server with the following features:

Authorization Grant
===================

The Plan B Provider currently supports the following grant types:

`Authorization Code Grant Type`_
    Client redirects to Plan B and user logs in with his credentials.
    This grant type is used for *User to Service* authentication.
`Resource Owner Password Credentials Grant Type`_
    User and client credentials are directly exchanged for an JWT access token.
    This grant type is mostly used for *Service to Service* authentication
    where user and client OAuth credentials are known to the service by some form of "secret distribution".

Access Token
============

The Plan B Provider issues access tokens in the `JWT format`_ which can be used as `Bearer Tokens`_ and validated against the Plan B Token Info.

The issued JWT tokens have the following properties:

* The access token is valid for 8 hours.
* The JWT is signed with a `JSON Web Signature (JWS)`_ and therefore has a corresponding JOSE Header and JWS signature
* The JOSE header always contains the signing key ID (``kid``) and signing algorithm (``alg``)
* The JWT payload always contains the following claims:

  * subject (``sub``)
  * realm (``realm``)
  * scopes (``scope``)
  * issuer (``iss``)
  * expiration date (``exp``)
  * issue date (``iat``)

An example JWT access token issued by Plan B Provider might look like (line breaks were added for readability):

.. code-block:: json

    eyJraWQiOiJ0ZXN0a2V5LWVzMjU2IiwiYWxnIjoiRVMyNTYifQ.
    eyJzdWIiOiJ0ZXN0MiIsInNjb3BlIjpbImNuIl0sImlzcyI6IkIiLCJyZWFsbSI6Ii9zZXJ2aWNlcyIsImV4cCI6MTQ1NzMxOTgxNCwiaWF0IjoxNDU3MjkxMDE0fQ.
    KmDsVB09RAOYwT0Y6E9tdQpg0rAPd8SExYhcZ9tXEO6y9AWX4wBylnmNHVoetWu7MwoexWkaKdpKk09IodMVug

The JOSE header contains:

.. code-block:: json

    {
        "kid": "testkey-es256",
        "alg": "ES256"
    }

The JWT payload contains:

.. code-block:: json

    {
        "sub": "test2",
        "scope": [
            "cn"
        ],
        "iss": "B",
        "realm": "/services",
        "exp": 1457319814,
        "iat": 1457291014
    }

Refresh Token
=============

The Plan B Provider does not support refresh tokens.


Protocol Endpoints
==================

.. _authorization-endpoint:

Authorization Endpoint
----------------------

Plan B Provider provides the `OAuth Authorization Endpoint`_ as ``/oauth2/authorize``:

* The client redirects the user to ``/oauth2/authorize?response_type=code&client_id={clientId}&realm={realm}``

  * ``response_type`` must be "code"
  * ``client_id`` must be set
  * ``realm`` must either be set or match hostname
  * ``redirect_uri`` is the optional callback URL
  * ``state`` is the optional client "state" (passed through)

* Plan B will display a login form
* Successful user authentication will trigger a redirect back to the client (``redirect_uri``) including a ``code`` query parameter
* The client can exchange the given authorization code for a valid JWT token at the :ref:`token-endpoint`.

See `RFC 6749 section 4.1.1. "Authorization Request"`_ for details.

.. _token-endpoint:

Token Endpoint
--------------

The Plan B Provider provides the `OAuth Token Endpoint`_ as ``/oauth2/access_token``:

* The client MUST use the HTTP "POST" method against ``/oauth2/access_token``
* Confidential clients MUST authenticate with their client ID and secret via HTTP Basic Auth (``Authorization`` header).
* The realm MUST be passed as either form or query parameter (e.g. ``/oauth2/access_token?realm=/services``)
* The ``grant_type`` parameter MUST have the value "password".

See `RFC 6749 section 4.3.2. "Access Token Request"`_ for details.

Introspection Endpoint
----------------------

The Plan B Token Info does not yet implement the `OAuth 2.0 Token Introspection Endpoint`_, but instead the endpoint ``/oauth2/tokeninfo`` is provided:

* The access token SHOULD be passed as a Bearer token in the ``Authorization`` header.
* The access token MAY be passed in the ``access_token`` query parameter.
* The response will only have HTTP status code 200 if:

  * the JWS signature is valid
  * the JWT is not expired (i.e. the ``exp`` value lies in the future)
  * the token was not revoked

* The JSON response will at least contain the following properties:

  * seconds till expiry (``expires_in``)
  * list of granted scopes (``scope``)
  * user ID (``uid``)
  * user realm (``realm``)




.. _RFC 6749 "The OAuth 2.0 Authorization Framework": http://tools.ietf.org/html/rfc6749
.. _Authorization Code Grant Type: http://tools.ietf.org/html/rfc6749#section-1.3.1
.. _Resource Owner Password Credentials Grant Type: http://tools.ietf.org/html/rfc6749#section-1.3.3
.. _JWT format: https://tools.ietf.org/html/rfc7519
.. _Bearer Tokens: http://tools.ietf.org/html/rfc6750
.. _JSON Web Signature (JWS): https://tools.ietf.org/html/rfc7515
.. _OAuth Token Endpoint: http://tools.ietf.org/html/rfc6749#section-3.2
.. _OAuth Authorization Endpoint: http://tools.ietf.org/html/rfc6749#section-3.1
.. _RFC 6749 section 4.1.1. "Authorization Request": https://tools.ietf.org/html/rfc6749#section-4.1.1
.. _RFC 6749 section 4.3.2. "Access Token Request": http://tools.ietf.org/html/rfc6749#section-4.3.2
.. _OAuth 2.0 Token Introspection Endpoint: https://tools.ietf.org/html/rfc7662
