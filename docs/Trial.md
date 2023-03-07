# Free Trial

PuffinDB will provide a free trial option for users who have an AWS account with limited privileges (access to S3 bucket only).

## Outline
- 30 days
- $100 AWS credits
- [CloudFormation](https://aws.amazon.com/cloudformation/) template deployed on PuffinDB's virtual private cloud (VPC)

## Session
Once the CloudFormation template has been deployed, trial users can start and stop evaluation sessions at any time during the 30-days trial period. The [Monostore](Monostore.md) is instantiated at the beginning of every session (this takes 2 to 3 minutes). When starting a session, the user selects one of three options for the Monostore:

| Instance | Dataset Size |
| -------- | ------------ |
|[`g5.8xlarge`](https://aws.amazon.com/ec2/instance-types/g5/)| 100 GB |
|[`g5.24xlarge`](https://aws.amazon.com/ec2/instance-types/g5/)| 250 GB |
|[`p4d.24xlarge`](https://aws.amazon.com/ec2/instance-types/p4/)| 1 TB |
