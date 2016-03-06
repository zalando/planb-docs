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
