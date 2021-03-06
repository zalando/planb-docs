.. _revocations:

===========
Revocations
===========

Plan B allows revoking JWT tokens via three different revocation types:

``TOKEN``
    Revoke single JWT tokens.
``CLAIM``
    Revoke all JWTs having a specific claim value.
``GLOBAL``
    Revoke all JWTs issued before a certain date.

Revocations are stored in Cassandra and the Token Info component regularly polls for deltas.

Revoking a Single Token
=======================

.. code-block:: bash

    $ tok=... # some valid token accepted by the configured TOKENINFO_URL
    $ curl -X POST \
         -H "Authorization: Bearer $tok" \
         -H 'Content-Type: application/json' \
         -d '{"type": "TOKEN", "data": {"token": "..."}}' \
         "https://planb-revocation.example.org/revocations"


Revoking Tokens by Claims
=========================

Revoking all tokens issued up to now with subject (username) "jdoe":

.. code-block:: bash

    $ tok=... # some valid token accepted by the configured TOKENINFO_URL
    $ curl -X POST \
         -H "Authorization: Bearer $tok" \
         -H 'Content-Type: application/json' \
         -d '{"type": "CLAIM", "data": {"claims": {"sub": "jdoe"}}}' \
         "https://planb-revocation.example.org/revocations"

Forcing Token Info to refresh from certain Timestamp
====================================================

.. code-block:: bash

    $ tok=... # some valid token accepted by the configured TOKENINFO_URL
    $ curl -X POST \
        -H "Authorization: Bearer $tok" \
        https://planb-revocation.example.org/notifications/REFRESH_FROM?value=123
