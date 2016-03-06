.. _service-to-service-auth:

=================================
Service To Service Authentication
=================================

The main driver for Plan B's development was enabling "Service to Service" authentication with OAuth Bearer tokens.

Application A (client) needs to authenticate itself, so that application B (resource server) can grant access:

* Service user and OAuth client for application A are created in the Plan B Provider
* A's user and client credentials are distributed (e.g. via S3 buckets), so that A can read them
* Application A gets a new OAuth access token (JWT) from the Plan B Provider
* A calls the desired HTTP endpoint of application B, passing the JWT token in the ``Authorization`` header ("Bearer {token}")
* B validates the token by calling the Token Info endpoint
* Token Info will verify the JWT signature and returns a JSON token info structure with A's service user ID in the ``uid`` property

For this to work, both applications need to know about the authentication infrastructure:

* A needs to know its service user (username and password) and client credentials (client ID and secret)
* A needs to know the OAuth2 access token URL (``OAUTH2_ACCESS_TOKEN_URL`` environment variable) to create JWT tokens, e.g. https://planb-provider.example.org/oauth2/access_token
* B needs to know the OAuth2 token info URL (``TOKENINFO_URL`` environment variable), e.g. https://planb-tokeninfo.example.org/oauth2/tokeninfo






