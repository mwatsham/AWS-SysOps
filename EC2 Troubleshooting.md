# EC2 Instance Launch Issues
## #InstanceLimitExceeded 
*Cause*: 
* Means the the limit has been reached for the max number of instances in that region
* Default limit for instances in a region is 20
*Checks*:
* AWS Console: EC2 => Limits (Reserved Instances - Running instances)
* 
*Resolution: 
* Launch the instance in a different region
* Submit request to AWS to increase the limit for the region
