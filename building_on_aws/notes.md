# Building on AWS

## Infrsatructure

- **Availability Zone** - Logical Datacenter where you run your code e.g us-west-2b -end in letters
- **Region** - e.g us-west-2 region has AZs which are separated from each other in the event of a catastrophe.

AZ scope -EC2
Region Scope -S3

## Networking in AWS

**VPC** - Virtual Private Cloud
You can define subnets on the VPC. Resources inside a subnets can communicate with each other
A VPC has region scope, Subnets are AZ scope

## EC2 Metadata

    $ curl http://169.254.169.254/latest/dynamic/instance-identity/document
