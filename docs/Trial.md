# Free Trial

STOIC will provide a free trial option for PuffinDB to users who have an AWS account with limited privileges (access to S3 bucket only).

Users who have an AWS accout with proper privileges can use PuffinDB on their own virtual private cloud (VPC), without any limitations.

## Outline
- 30 days
- $100 AWS credits
- [CloudFormation](https://aws.amazon.com/cloudformation/) template deployed on PuffinDB's virtual private cloud (VPC) within user's availability zone

## Sessions
Once the CloudFormation template has been deployed, trial users can start and stop evaluation sessions at any time during the 30-days trial period. The [Monostore](Monostore.md) is started at the beginning of every session (this usually takes 2 to 3 minutes), and automatically stopped after 15 minutes of inactivity. When starting a session, the user selects one of four options for the Monostore:

| Size | Instance | Dataset Size | vCPUs | CPU RAM | GPUs | GPU RAM | Network BW | EBS BW | Hours |
| ---- | -------- | ------------ | ----- | ------- | ---- | ------- | ---------_ | ------ | ----- |
| XS | [`g5.2xlarge`](https://aws.amazon.com/ec2/instance-types/g5/)| Up to 25 GB | 8 | 32 GB | 1 | 16 GB | Up to 10 Gbps | Up to 3.5 Gbps | 200 |
| S | [`g5.8xlarge`](https://aws.amazon.com/ec2/instance-types/g5/)| Up to 100 GB | 32 | 128 GB | 1 | 32 GB | 25 Gbps | 16 Gbps | 75 |
| M | [`g5.24xlarge`](https://aws.amazon.com/ec2/instance-types/g5/)| Up to 250 GB | 96 | 384 GB | 4 | 96 GB | 50 Gbps | 19 Gbps | 25 |
| L | [`p4d.24xlarge`](https://aws.amazon.com/ec2/instance-types/p4/)| Up to 1 TB | 96 | 1,152 GB | 8 | 320 GB | 400 Gbps | 19 Gbps | 5 |

Assuming 25 days of actual evaluation, this translates into:
- 8 hours a day for 25 GB datasets
- 3 hours a day for 100 GB datasets
- 1 hour a day for 250 GB datasets
- 1 hour a week for 1 TB datasets

## STOIC
The free trial for PuffinDB will include unlimited seats for [STOIC](https://github.com/stoic-doc/Community/discussions/1).
