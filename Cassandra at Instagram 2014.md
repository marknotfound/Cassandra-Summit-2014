Cassandra at Instagram 2014
---
Rick Branson (@rbranson)

Instagram

####What's good about Redis?
  * Fast
  * Stable, sets the bar for the NoSQL space
  * Easy to get started
  * Useful, rich data types

####What's wrong with Redis?
  * Hard to see what's inside
    * scan command not so intuitive or efficient
  * Snapshots (BGSAVE)
    * Race between how fast data is updated in memory and how quickly you can stream that to disk
    * Run out of memory
      * Sadness ensues
  * Single threaded
  * Memory allocator
    * Uses a heap allocator
    * Suffers from heap fragmentation
  * Lacks durability
  * Unsafe replication

####The Redis Philosophy
  * Is it a database or a cache?
    * Neither.

####What is Instagram using now?
  * Zookeeper
  * Cassandra
  -

####Zookeeper
  * They built Zozo!
    * *Very* few writes
    * Extremely high volumes
    * No-latency reads without hotspots
    * Stores arbitrary blob keys. a few megs at most.
    * Mostly stores json
    * Closed source
    * Agent is dead simple
      * Never needs updating. Resists both human and machine errors.
    * Incoming multi-region strategy

####"JSON sucks."
  * Wild wild west
  * No schema ties. No validation. Mistake prone.

####Redis -> Cassandra
  * FINALLY!
  * Moved a bunch of data
    * Ad serving metadata
    * Account suggestion metadata
    * Churn tracking
    * Follow requests

####Instagram Direct
  * One to many photo and video messaging
  * Comments, likes, etc
  * All data shared with all recipients

####Why Cassandra? (Instagram Direct)
  * Elasticity + Scalability
    * Scale up and down very easily/quickly
  * Write availability
  * Storage efficiency
  * Good data model fit

####Chose to use Thrift @ end of 2013
  * Made the implementation more difficult
  * Wanted to go with tried and true
  * Would choose CQL3 if doing it this year.

####Fan out on write
* Caching doesn't work
  * Too much data changing too frequently and randomly
  * #nocache
* Copy of full data for each recipient
  * Each recipient's inbox has a copy of the entire thread
* Atomic batches super useful

####Filter on Read
* When can a user receive a message?
  * Are they blocked?
  * Are they whitelisted?
  * Do they follow the sender?
  * All filtered on read instead of write.

####The Pending Queue
* Users who are trying to send you things that you do not follow.
* Each queue item is a post id aggregating into a user
* Everything is always written. Filtered on read.
* Trimmed when it grows to a certain size

####Deployment
* 60 x hi1.4xlarge EC2 instances
* 2x highest estimates
* Shrunk cluster after load was learned
* Peace of mind
* Data models held together

##Cassandra in Production

####Young GC is fun!
* Got reads up 20:1 to writes on one cluster
* Saw big P99 latency increases
* Reads = young gen garbage
* Double-collection bug in JDK 1.7+
  * Both collections would take the same amount of time
  * Causes pause
* Best practices were for nodes with more writes

####JVM Settings
* 10Gb Young Gen
* 20Gb Heap
* "<blink>DEAR GOD ALMIGHTY!!!</blink"
* This doesn't work out of the box.
* `-XX: + CMSScavangeBeforeRemark`
* `-XX:CMSMaAbortablePrecleanTime = 60000`
  * Default is 5 seconds (5000ms).
* `-XX:CMSWaitDuration = 30000`

####Stats
* 15k reads/sec
* 2k writes/sec

He then ran out of time and showed a picture of a dog as the music played him off.
