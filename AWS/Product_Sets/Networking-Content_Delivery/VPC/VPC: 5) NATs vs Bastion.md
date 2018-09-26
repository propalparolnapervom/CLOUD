# VPC: NATs vs Bastion 

## OVERALL

A NAT is used to provide internet traffic to EC2 instances in private subnets.

A Bastion is used to securely administer EC2 instances (using SSH or RDP) in private subnets. 

To make a Bastion highly available, you want to have atleast 2 subnets (AZ) with Bastion, where you can do things like Autscaling of 1 server (so if it fails, Autoscaling will launch another server).





































