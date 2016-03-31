===================
Credential Rotation
===================

It's `good practice to rotate all credentials regularly`_:

* OAuth service user passwords should be rotated frequently, e.g. every two hours.
* OAuth client credentials (client ID and secret for confidential clients) might be rotated every month.

The Provider allows rotating both user and client credentials via its ``/raw-sync/`` REST API.

Service Users
=============

To rotate an application's user credentials, do:

* Add a new password by sending a POST request to ``/raw-sync/users/{realm}/{id}/password``.
* Distribute the new password (e.g. via a S3 Mint Bucket).
* Wait at least 10 minutes to give the application time to pick up the new password. Both old and new password will be active during this grace period.
* Commit the new password by overwriting the user's passwords via a PATCH request to ``/raw-sync/users/{realm}/{id}``.

Clients
=======

OAuth client credentials can be rotated by creating a new client and deactivating or deleting the old one:

* Create a new client by sending a PUT request to ``/raw-sync/clients/{realm}/{id}`` (see below).
* Distribute the new client credentials (e.g. via a S3 Mint Bucket).
* Wait at least 10 minutes to give the application time to pick up the new client credentials. Both old and new client will be active during this grace period.
* Reset the old client's secret to some random string or delete the old client completely.

Example REST call to create a new client in the ``/services`` realm using HTTPie_:

.. code-block:: bash

    $ SECRET_HASH=$(python3 -c "import bcrypt; print(bcrypt.hashpw(b'test', bcrypt.gensalt(4)).decode('ascii'))")
    $ TOK='mytoken' # valid OAuth token
    $ http PUT https://planb-provider.example.org/raw-sync/clients/services/myclient-123 \
      is_confidential:=true \
      name='Example Client'\
      secret_hash=$SECRET_HASH \
      "Authorization:Bearer $TOK"



.. _good practice to rotate all credentials regularly: http://tools.ietf.org/html/rfc6749#section-3.2.1
.. _HTTPie: https://github.com/jkbrzt/httpie
