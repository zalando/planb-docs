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

Token Info
==========

The Token Info exposes metrics on its metrics port 9020 with path /metrics.
