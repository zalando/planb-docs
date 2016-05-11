===============
Getting Started
===============

All three Plan B components can be easily built and started locally.
You will need:

* Java 8 JDK for Provider and Revocation Service
* Go 1.6 for Token Info
* Cassandra 2.1 for Provider and Revocation Service (can be started as Docker container)

Follow the README instructions in the respective repositories to build the components:

* https://github.com/zalando/planb-provider
* https://github.com/zalando/planb-tokeninfo
* https://github.com/zalando/planb-revocation

Start Cassandra and create the necessary keyspaces for Provider and Revocation (we assume source was checked out into ``planb-provider`` and ``planb-revocation``):

.. code-block:: bash

    $ docker run --name cassandra -d -p 9042:9042 cassandra:2.1
    $ docker run -i --link cassandra:cassandra --rm cassandra:2.1 cqlsh cassandra < planb-provider/schema.cql
    $ docker run -i --link cassandra:cassandra --rm cassandra:2.1 cqlsh cassandra < planb-revocation/schema.cql

You can use the ``generate-load-test-data.py`` script to generate a test key pair and test credentials:

.. code-block:: bash

    $ sudo pip3 install bcrypt # install required Python module to hash passwords
    $ ./planb-provider/scripts/generate-load-test-data.py > test-data.cql
    $ docker run -i --link cassandra:cassandra --rm cassandra:2.1 cqlsh cassandra < test-data.cql

This will create a bunch of test service users and clients, e.g.:

* username "test0" and password "test0"
* client ID "test0" and client secret "test0"

Now start the Provider (default port 8080) and Token Info (default port 9021).

.. code-block:: bash

    $ export OAUTH2_ACCESS_TOKENS=customerLogin=test             # fixed OAuth test token (unused)
    $ export TOKENINFO_URL=https://example.com/oauth2/tokeninfo  # required for /raw-sync REST API (unused here)
    $ java -jar planb-provider/target/planb-provider-1.0-SNAPSHOT.jar &

    $ export OPENID_PROVIDER_CONFIGURATION_URL=http://localhost:8080/.well-known/openid-configuration
    $ export UPSTREAM_TOKENINFO_URL=https://auth.example.org/oauth2/tokeninfo
    $ export REVOCATION_PROVIDER_URL=https://planb-revocation.example.org/revocations
    $ $GOPATH/bin/planb-tokeninfo

Let's first check that our OpenID Provider runs and contains at least one test key pair:

.. code-block:: bash

    $ curl http://localhost:8080/.well-known/openid-configuration # should contain jwks_uri
    $ curl http://localhost:8080/oauth2/connect/keys # should return at least one public key


We can create a new JWT token with cURL:

.. code-block:: bash

    $ curl -u test0:test0 \
      -d 'grant_type=password&username=test0&password=test0' \
      http://localhost:8080/oauth2/access_token?realm=/services

Afterwards we can validate the JWT using the Token Info:

.. code-block:: bash

    $ TOKEN=... # insert "access_token" value from above
    $ curl -H "Authorization: Bearer $TOKEN" http://localhost:9021/oauth2/tokeninfo



