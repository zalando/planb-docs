===================
Credential Rotation
===================

It's good practice to rotate all credentials regularly:

* OAuth service user passwords should be rotated frequently, e.g. every two hours.
* OAuth client credentials might be rotated every month.

The Provider allows rotating both user and client credentials via its ``/raw-sync/`` REST API.

To rotate an application's user credentials, do:

* Add a new password by sending a POST request to ``/raw-sync/users/{realm}/{id}/password``.
* Distribute the new password (e.g. via a S3 Mint Bucket).
* Wait at least 10 minutes to give the application time to pick up the new password. Both old and new password will be active during this grace period.
* Commit the new password by overwriting the user's passwords via a PATCH request to ``/raw-sync/users/{realm}/{id}``.
