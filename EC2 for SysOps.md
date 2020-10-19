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
### Cluster
Cluster instances into low -latency group in a single Availability Zone
* EC2 instances are on the same hardware in the same rack
* Pros
  * Great network performance
* Cons
  * Increased risk of service failure as all instances fail if the rack fails
* Use case
  * Big data job that need fast completion
  * Application that needs extremely low latency and high network throughput
### Spread
**Spreads instances across underlying hardware** (max 7 instances per group per AZ) - critical applications
* EC2 Instances are hosted on different hardware
* Pros
  * **Can span across Availability Zones**
  * **Reduced risk of simultaneous failure**
* Cons
  * Limited to 7 instances per AZ per placement group
* Use Case
  * Application that requires **maximum high availability**
  * **Critical applications** where each instance must be isolated from failure from each other
### Partition
Spreads instances across many different partitions which rely on different sets of racks of hardware within an AZ. Scales to 100s of EC2 instances per group - Hadoop, Cassandra, Kafka)
* Partitions are separate racks
* Up to 7 partitions per AZ
* 100s of EC2 instances
* The instances in a partition **do not share racks** with other instances in another partition
* A partition failure will effect the EC2 instances in that partition only
* EC2 instances get access to partition metadata information they belong to
* Use case
  * *HDFS* - Hadoop Distributed File System - a distributed file system that handles large data sets running on commodity hardware.
  * *HBase* - A column-oriented database management system that runs on top of HDFS, a main component of Apache Hadoop.
  * *Cassandra* - Apache Cassandra is a free and open-source, distributed, wide column store, NoSQL database management system designed to handle large amounts of data
  * *Kafka* - Apache Kafka is a community distributed event streaming platform capable of handling trillions of events a day. Kafka is primarily used to build real-time streaming data pipelines and applications that adapt to the data streams.

### Creating a Placement Group
#### Console UI
`EC2 => Network & Security => Placement Groups`

#### AWS CLI
https://docs.aws.amazon.com/cli/latest/reference/ec2/create-placement-group.html
```
aws ec2 create-placement-group --group-name high-performance-group --strategy cluster

aws ec2 create-placement-group --group-name critical-app-group --strategy spread

aws ec2 create-placement-group --group-name distributed-app-group --strategy partition
```

#### CloudFormation 
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-placementgroup.html
https://github.com/awsdocs/aws-cloudformation-user-guide/blob/main/doc_source/aws-resource-ec2-placementgroup.md

```
PlacementGroup:
  Type: AWS::EC2::PlacementGroup
    Properties:
      Strategy: cluster

PlacementGroup:
  Type: AWS::EC2::PlacementGroup
    Properties:
      Strategy: spread
      
PlacementGroup:
  Type: AWS::EC2::PlacementGroup
    Properties:
      Strategy: partition
```

### Using Placement Groups
EC2 instances can be assigned to an exisiting placement group at the point of creation

## EC2 Shutdown Behaviour & Termination
### Shutdown behaviour
Influencing shutdown behaviour of an instance when the instance is shutdown via the OS i.e. `# shutdown now`
Stopped is the Default behaviour
An instance can be set to Terminate instead
#### Stopped vs Terminated
***Stopped***
* When you an EC2 instance, the instance will be shutdown and the virtual machine that was provisioned for you will be permanently taken away and you will no longer be charged for instance usage.
* The key difference between stopping and terminating an instance is that the **attached bootable EBS volume will not be deleted**.
* The ability to stop an instance is only supported on instances that were launched using an EBS-based AMI where the root device data is stored on an attached EBS volume as an EBS boot partition instead of being stored on the local instance itself. 
***Terminated***
* When you terminate an EC2 instance, the instance will be shutdown and the virtual machine that was provisioned for you will be permanently taken away and you will no longer be charged for instance usage. 
* **Any data that was stored locally on the instance will be lost.** 
* **Any attached EBS volumes will be detached and deleted.** However, if you attach an EBS Snapshot to an instance at boot time, the default option in the Dashboard is to delete the attached EBS volume upon termination.

### Termination protection
Enabing termination protection on an EC2 instance to prevent accidental termination via the AWS Console.

