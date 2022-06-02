# vault-consul-failover
HashiCorp Vault failover using HashiCorp Consul

## Setup

### Add a properly permissioned Role Arn to Assume for AWS Provider

Your AWS provider will need to assume role (which is generally considered good practice and whoever is providing your creds may require this)

In the top-level tf.main file you will see the following.

```
provider "aws" {
  region = local.vpc_region_primary
  assume_role {
    role_arn = "<add arn of assume role>"
  }
}

provider "aws" {
  alias = "us_east_1"
  region = local.vpc_region_secondary
  assume_role {
    role_arn = "<add arn of assume role>"
  }
}
```

Please create an appropriate role and add the ARN so that Terraform will have the permissions to create the needed resources.
 
### First set your HCP and AWS Credentials

You will be provisioning infrastruct for both HashiCorp Cloud Platform (Consul) and AWS (EC2 instances to host Consul agent and Vault). As such you will need to provide adequately permissioned credentials for both these platforms.

You can do the following to a POSIX environment
```
export HCP_CLIENT_ID=<Insert HCP Client ID>
export HCP_CLIENT_SECRET=<Insert HCP Secret ID>

export AWS_ACCESS_KEY_ID=<Insert AWS ID>
export AWS_SECRET_ACCESS_KEY=<Inset AWS KEY>
export AWS_SESSION_TOKEN=<Insert Session Token>
```

Try a Terraform Plan and look over what will be created.

```
terraform plan
```

Try a Terraform Apply

```
terraform apply
```

Remember to Cleanup after yourself when you are done.

```
terraform destroy
```
