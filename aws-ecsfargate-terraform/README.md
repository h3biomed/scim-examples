# Deploying the 1Password SCIM Bridge in AWS ECS with Terraform

This document describes deploying the 1Password SCIM bridge to your Amazon Web Service Elastic Container Service Fargate using Terraform. It's just a suggested starting point - you may be using different services for different things, this example uses only AWS products. Please familiarize yourself with [PREPARATION.md](/PREPARATION.md) before beginning.

Prerequisites
- [terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- AWS credentials configured (either through $HOME/.aws/credentials or environment variable)
  see: [Terraform AWS authentication](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication)
- scimsession file and bearer token ([follow step 1 here](https://support.1password.com/scim/))

1. Clone this repository and cd to it
2. Copy the `scimsession` file from 1Password directory
3. In `terraform.tf`, edit the following values as needed (`vpc_id` is specified in three places, subnets must be public and not in the same availability zone)

```
profile                 = "h3-infrastructure"
vpc_id = "vpc-3fe34a5a"
subnets = ["subnet-4343486b", "subnet-6f579918"]
```
 
6. Now run the following commands:
```
terraform init
terraform plan -out=./op-scim.plan
# Validate what this plan does by reading it
terraform apply ./op-scim.plan
```
After a few minutes, if you go to the SCIM Bridge URL you set, you should be able to enter your bearer token to verify that your scim bridge is up and running. Connect to your IdP using step 3 [here](https://support.1password.com/scim/) and go to your 1Password account and check that provisioning is on in Setting -> Provisioning and you should be good to go!

If you want to check out the logs for your scim bridge, in AWS go to Cloudwatch -> Log Groups and you should see the log group that was printed out at the end of your terraform apply. You can then see the scim-bridge and redis container logs. 
