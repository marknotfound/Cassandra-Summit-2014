Successful Software Development with Cassandra
---


####Data Modeling
* #1 Performance Problem
* Don't port your SQL schema
* Do a lot of research

####Productivity
* Use CQL.
* Thrift is deprecated.
* Use the Java driver. If you can. :(

####CQL
* Tools support
* Easy tracing and trace discovery
* Documentation

####Java Driver
* Reference implementation
* Well written, extensive coverage
* Spring integration

####Java Driver Configuration
* Three policy types
  * load balancing
  * connectivity
  * retry
* Connection options:
  * protocol
  * pooling
  * socket

####Using User Defined Types
* Serialized as blobs
* Refactor/new version being discussed
* Will be a painful migration path in the future

####Tools
* Datastax DevCenter
* Metrics API

####Trace Frequently
* Trace through `cqlsh`
* Will help to understand why your query does or does not suck.
* Similar to SQL explain
* Enable traces in the driver
  * `nodetool settraceprobability`
    * Probability that a trace runs for a query
* Do it sparingly

####Logging verbosity
* Can be changed dynamically

####CCM
* Supports multiple versions
* Clusters and datacenters
* Up/down individual nodes

####Vagrant
* isolated, controlled environment
* Configuration management integration
* Same CM for production
