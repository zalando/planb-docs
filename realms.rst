======
Realms
======

Plan B supports the "realms" concept to configure different properties and backends for different use cases.
Usernames (``sub`` claim or ``uid`` Token Info response property) are **only unique within one realm**,
i.e. Plan B allows to have two different users with the same username "jdoe" in two different realms (e.g. "jdoe" customer and "jdoe" employee).

By default, the following realms are defined:

``/services``
    Service users (applications) which are authenticated using the Cassandra storage.
    See the section on :ref:`service-to-service-auth`.
``/employees``
    Human users which are authenticated against an upstream (OAuth) service.
``/customers``
    Special realm for "customers" which are authenticated against a specific, proprietary `Customer Service web service`_.

Client credentials are always checked against Plan B's Cassandra storage, but user authentication might be delegated
to upstream services (done by default for realms "/employees" and "/customers").

Different realms can be configured via the `Provider's Spring configuration files`_ or environment variables:

.. code-block:: bash

    $ cd planb-provider
    $ ./mvnw verify
    $ export REALM_NAMES=/myrealm,/otherrealm
    $ java -jar target/planb-provider-1.0-SNAPSHOT.jar

The realm is always included in the JWT payload as the ``realm`` claim.
Plan B's Token Info will return the token's realm in the "realm" property;
this can be used for authorization rules in resource servers, e.g.:

* allow all tokens with the "/employees" realm to read data
* disallow any access for tokens with the "/customers" realm

.. _Provider's Spring configuration files: https://github.com/zalando/planb-provider/blob/master/src/main/resources/config/application.yml
.. _Customer Service web service: https://github.com/zalando/planb-provider/tree/master/mocks/customer-service
