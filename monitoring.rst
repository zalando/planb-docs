==========
Monitoring
==========

Cassandra
=========

The Cassandra cluster should be closely monitored using the usual Cassandra JMX beans, e.g.:

* Number of UP/DOWN nodes
* Number of Read/Write requests per second
* Read/Write latency

Provider
========

The Provider exposes metrics on its management port 7979 with path /metrics.

``planb.provider.access_token.{realm}.success``
    DropWizard timer for successful access token requests by realm.
``planb.provider.access_token.{realm}.error.{errortype}``
    DropWizard timer for failed access token requests by realm and error type.

Token Info
==========

The Token Info exposes metrics on its metrics port 9020 with path /metrics.

``planb.openidprovider.numkeys``
    Number of public keys in memory.
``planb.tokeninfo.jwt.{realm}.requests``
    Timer for the JWT handler (one timer per realm).
``planb.tokeninfo.proxy``
    Timer for the proxy handler (includes cached results and upstream calls).
``planb.tokeninfo.proxy.cache.hits``
    Number of upstream cache hits.
``planb.tokeninfo.proxy.cache.misses``
    Number of upstream cache misses.
``planb.tokeninfo.proxy.cache.expirations``
    Number of upstream cache misses because of expiration.
``planb.tokeninfo.proxy.upstream``
    Timer for calls to the upstream tokeninfo. Cached responses are not measured here.
