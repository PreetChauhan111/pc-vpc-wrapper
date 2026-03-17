# pc-vpc-wrapper

Wrapper module for AWS VPC and networking resources.

## Source

This wrapper uses the upstream module from [`terraform-aws-modules/vpc/aws`](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest) and adds extensive local naming (vpc, subnets, groups as pc-<env>-<region>-vpc/public-1 etc.), common tags, region/environment defaults.

## Usage

```hcl
module "example_vpc" {
  source = "./pc-vpc-wrapper"

  region = "ap-south-1"
  environment = "dev"
  
  cidr = "10.0.0.0/16"
  azs  = ["ap-south-1a", "ap-south-1b"]
  
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnets = ["10.0.101.0/24", "10.0.102.0/24"]
}
```

## Inputs

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `region` | AWS Region | `string` | `"ap-south-1"` |
| `environment` | Deployment environment | `string` | `"Dev"` |
| `name` | VPC name. Auto pc-<env>-<region>-vpc if empty | `string` | `""` |
| `cidr` | VPC CIDR | `string` | n/a |
| `azs` | Availability zones | `list(string)` | `[]` |
| `public_subnets` | List of public subnet CIDRs | `list(string)` | `[]` |
| `private_subnets` | List of private subnet CIDRs | `list(string)` | `[]` |
| `enable_nat_gateway` | Enable NAT GW | `bool` | `false` |
<!-- Abbrev; 100+ vars for subnets (db, elasticache), NACLs, flow logs etc. --> |

Full inputs: [variables.tf](variables.tf).

## Outputs

| Name | Description |
|------|-------------|
| `vpc_id` | The ID of the VPC |
| `public_subnets` | List of IDs of public subnets |
| `private_subnets` | List of IDs of private subnets |
| `igw_id` | The ID of the Internet Gateway |
| `default_security_group_id` | Default SG ID |

## Notes

- Auto-generates names/tags extensively: common_tags `{Environment, GitHub=pc-vpc-wrapper, Owner=pc}`.
- Subnet names: public-1, private-1 etc.
- Supports NAT, VPN, IPv6, IPAM, flow logs, DB/ElastiCache/Redshift subnets/groups.
- Default region `ap-south-1`, env `Dev`.

