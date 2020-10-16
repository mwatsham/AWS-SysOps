# EC2 for SysOps

## Launching an EC2 Instance

## Changing EC2 Instance Type
Changing the Instance type of an existing instance
* Only works for Elastic Block Store backed instances
* Will retain data
1. Need to stop the Instance
1. Access instance settings => Change Instance type
1. Start Instance
* Note the Public IP will change 

## Placement Groups
Controlling the placement strategy of an EC2 instance
* Placement groups have 3 strategies
1. Cluster - cluster instances into low -latency group in a single Availability Zone
* EC2 instances are on the same hardware in the same rack
* Pros
** Great network performance
* Cons
** Increased risk of service failure as all instances fail if the rack fails
* Use case
** Big data job that need fast completion
** Application that needs extremely low latency and high network throughput
1. Spread - spreads instances across underlying hardware (max 7 instances per group per AZ) - critical applications
* EC2 Instances are hosted on different hardware
* Pros
** Can span across Availability Zones
** Reduced risk of simultaneous failure
* Cons
** Limited to 7 instances per AZ per placement group
* Use Case
** Application that requires maximum high availability
** Critical applications where each instance must be isolated from failure from each other
1. Partition - spreads instances across many different partitions which rely on different sets of racks of hardware within an AZ. Scales to 100s of EC2 instances per group - Hadoop, Cassandra, Kafka)
* Partitions are separate racks
* Up to 7 partitions per AZ
* 100s of EC2 instances
* The instances in a partition do not share racks with other instances in another partition
* A partition failure will effect the EC2 instances in that partition only
* EC2 instances get access to partition metadata information they belong to
* Use case
** HDFS - Hadoop Distributed File System - a distributed file system that handles large data sets running on commodity hardware.
** HBase - A column-oriented database management system that runs on top of HDFS, a main component of Apache Hadoop.
** Cassandra - Apache Cassandra is a free and open-source, distributed, wide column store, NoSQL database management system designed to handle large amounts of data
** Kafka - Apache Kafka is a community distributed event streaming platform capable of handling trillions of events a day. Kafka is primarily used to build real-time streaming data pipelines and applications that adapt to the data streams.

# CloudWatch + EC2
