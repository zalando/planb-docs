============
Introduction
============

Plan B provides an OAuth 2 infrastructure with these main features:

* Service to Service authentication using the `Resource Owner Password Credentials Grant`_ and `JWT tokens`_
* User to Service authentication supporting the `Authorization Code Grant`_ flow
* Token creation without any write operations (no token storage to avoid bottlenecks)
* Very fast, stateless OAuth access token validation (verifying JWT signature)
* Highly available Cassandra backend to store OAuth user and client credentials
* Revocation of JWT access tokens by claims (e.g. "revoke all tokens for user XY")
* Support for seamless rotation of OAuth user and client credentials

Plan B was developed with the following non-functional requirements in mind:

* Robustness & resilience -- all Plan B components must be highly available and resilient to error conditions
* Low latency for token validation -- the Token Info component is completely stateless and can be deployed "near" applications to avoid network latency
* Horizontal scalability -- all components (including Cassandra storage) should be able to scale out

Plan B provides three infrastructure components to do :ref:`service-to-service-auth` with JWT OAuth Bearer tokens:

**Provider**
    `OAuth2 Authorization Server`_ and `OpenID Connect Provider`_ issuing `JWT tokens`_.
**Token Info**
    OAuth2 tokeninfo validation endpoint for `JWT tokens`_.
**Revocation Service**
    REST service to manage token revocation lists.

.. image:: images/architecture.svg
   :alt: Plan B Architecture

Provider
========

The Plan B Provider issues signed JSON Web Tokens (JWT) as response to Access Token Requests
with a valid `Resource Owner Password Credentials Grant`_.

An example token creation response (``/oauth2/access_token`` endpoint):

.. code-block:: json

    {
        "access_token": "eyJraWQiOiJ0ZXN0a2V5LWVzMjU2IiwiYWxnIjoiRVMyNTYifQ.eyJzdWIiOiJ0ZXN0MiIsInNjb3BlIjpbImNuIl0sImlzcyI6IkIiLCJyZWFsbSI6Ii9zZXJ2aWNlcyIsImV4cCI6MTQ1NzMxOTgxNCwiaWF0IjoxNDU3MjkxMDE0fQ.KmDsVB09RAOYwT0Y6E9tdQpg0rAPd8SExYhcZ9tXEO6y9AWX4wBylnmNHVoetWu7MwoexWkaKdpKk09IodMVug",
        "id_token": "eyJraWQiOiJ0ZXN0a2V5LWVzMjU2IiwiYWxnIjoiRVMyNTYifQ.eyJzdWIiOiJ0ZXN0MiIsInNjb3BlIjpbImNuIl0sImlzcyI6IkIiLCJyZWFsbSI6Ii9zZXJ2aWNlcyIsImV4cCI6MTQ1NzMxOTgxNCwiaWF0IjoxNDU3MjkxMDE0fQ.KmDsVB09RAOYwT0Y6E9tdQpg0rAPd8SExYhcZ9tXEO6y9AWX4wBylnmNHVoetWu7MwoexWkaKdpKk09IodMVug",
        "token_type": "Bearer",
        "expires_in": 28800,
        "scope": "cn",
        "realm": "/services"
    }

The JWT header contains the ID of the signing key and the used algorithm:

.. code-block:: json

    {
        "kid": "testkey-es256",
        "alg": "ES256"
    }

The JWT payload contains the user ID (``sub`` claim), realm and granted scopes:

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

Token Info
==========

The Plan B Token Info validates signed JWT tokens using the right public key (exposed by the Provider) and
checks the token against any revocation lists.

The ``/oauth2/tokeninfo`` validation response contains details about the OAuth2 access token:

.. code-block:: json

    {
        "access_token": "eyJraWQiOiJ0ZXN0a2V5LWVzMjU2IiwiYWxnIjoiRVMyNTYifQ.eyJzdWIiOiJ0ZXN0MiIsInNjb3BlIjpbImNuIl0sImlzcyI6IkIiLCJyZWFsbSI6Ii9zZXJ2aWNlcyIsImV4cCI6MTQ1NzMxOTgxNCwiaWF0IjoxNDU3MjkxMDE0fQ.KmDsVB09RAOYwT0Y6E9tdQpg0rAPd8SExYhcZ9tXEO6y9AWX4wBylnmNHVoetWu7MwoexWkaKdpKk09IodMVug",
        "cn": true,
        "expires_in": 28292,
        "grant_type": "password",
        "open_id": "eyJraWQiOiJ0ZXN0a2V5LWVzMjU2IiwiYWxnIjoiRVMyNTYifQ.eyJzdWIiOiJ0ZXN0MiIsInNjb3BlIjpbImNuIl0sImlzcyI6IkIiLCJyZWFsbSI6Ii9zZXJ2aWNlcyIsImV4cCI6MTQ1NzMxOTgxNCwiaWF0IjoxNDU3MjkxMDE0fQ.KmDsVB09RAOYwT0Y6E9tdQpg0rAPd8SExYhcZ9tXEO6y9AWX4wBylnmNHVoetWu7MwoexWkaKdpKk09IodMVug",
        "realm": "/services",
        "scope": ["cn"],
        "token_type": "Bearer",
        "uid": "test2"
    }


Revocation Service
==================

The Plan B Revocation Service manages token revocation lists and provides them to Token Info.

See the :ref:`revocations` section for details.

.. _OAuth2 Authorization Server: http://tools.ietf.org/html/rfc6749#section-1.1
.. _OpenID Connect Provider: https://openid.net/specs/openid-connect-core-1_0.html
.. _JWT tokens: https://tools.ietf.org/html/rfc7519
.. _Resource Owner Password Credentials Grant: https://tools.ietf.org/html/rfc6749#section-4.3
.. _Authorization Code Grant: https://tools.ietf.org/html/rfc6749#section-4.1
