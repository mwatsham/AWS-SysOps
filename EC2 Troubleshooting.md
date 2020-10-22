# EC2 Instance Launch Issues
## #InstanceLimitExceeded 
*Cause*: 
* Limit has been reached for the max number of instances in that region
* Default limit for instances in a region is 20

*Checks*:
* AWS Console: EC2 => Limits (Reserved Instances - Running instances)
* AWS CLI:
```
$ aws ec2 describe-account-attributes --attribute-names max-instances
AccountAttributes:
- AttributeName: max-instances
  AttributeValues:
  - AttributeValue: '20'
```

*Resolution*: 
* Launch the instance in a different region
* Submit request to AWS to increase the limit for the region
