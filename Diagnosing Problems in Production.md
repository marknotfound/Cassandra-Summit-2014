Diagnosing Problems in Production
---
Jon Haddad (@rustyrazorblade), and Blake Eggleston (@blakeeggleston)

Datastax

####Preventative Measures
  * Opscenter
  * Metrics integration
  * Datadog duh
  * Other third party monitoring services

####Narrow it down
  * Consistency issues?
    * Make sure NTP is set up and working
    * Last write wins
  * Ensure same version of Cassandra is used in each node
  * Cleanup when adding nodes (reclaim disk space)
    * This is not the same as nodetool repair
  * Slow queries
    * Look at compaction
    * Histograms
    * Tracing
  * Nodes flapping/failing
    * Check ops center
    * System metrics, JVM GC issues

####Compaction
  * Compaction merges SSTables
  * Opscenter provides insight cluster wide
  * nodetool
    * compactionhistory
    * getcompactionthroughput
  * Leveled vs Sized Tiered
    * Leveled on SSD + Read Heavy
    * Size tiered on spinning disks

####System Utilities
  * iostat
  * htop
  * iftop & netstat
  * dstat
    * all of the above in one tool
  * strace
    * for badasses like Ko

####Histograms
  * nodetool utilities
    * proxyhistograms
      * High level read + write times
      * Network latency
    * cfhistograms <keyspace> <table>
      * reports stats for single table on single node
      * identify tables with performance problems

####Query Tracing
  * `cqlsh> TRACING on;`
    * Then run a query and it will show info similar to MySQL explain

####Is Cassandra hitting JVM GC issues?
  * Opscenter GC stats
    * View correlations between GC and latency spikes
  * Cassandra GC logging
    * Activate with cassandra-env.sh
  * jstat
    * Outputs GC activity

####GC Profiling
  * What to look for:
    * Long multi-second pauses
    * Long minor GCs
      * pause time SHOULD be under 5ms
      * could be caused by objects being promoted prematurely
      * most commonly cuased by new gen being too big
