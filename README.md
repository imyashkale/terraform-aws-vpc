# VPC Terraform Module

This Terraform module creates a Virtual Private Cloud (VPC) in AWS using the `terraform-aws-modules/vpc/aws` module.

## Usage

1. Create a new Terraform configuration file (e.g., `main.tf`) and define the VPC module:

    ```hcl
    module "vpc" {
      source  = "terraform-aws-modules/vpc/aws"
      version = "5.4.0"

      // Details
      name            = local.name
      cidr            = var.vpc_cidr_block
      azs             = var.vpc_availability_zones
      public_subnets  = var.vpc_public_subnets
      private_subnets = var.vpc_private_subnets

      // Database
      database_subnets                   = var.vpc_database_subnets
      create_database_subnet_group       = var.vpc_create_database_subnet_group
      create_database_subnet_route_table = var.vpc_create_database_subnet_route_table

      // NAT
      enable_nat_gateway = var.vpc_enable_nat_gateway
      single_nat_gateway = var.vpc_single_nat_gateway

      // VPC DNS
      enable_dns_hostnames = true
      enable_dns_support   = true

      // Tags
      tags     = local.tags
      vpc_tags = local.tags
    
      public_subnet_tags = {
        Type = "Public"
      }
      private_subnet_tags = {
        Type = "Private"
      }
      database_subnet_tags = {
        Type = "Private Datbase"
      }

      // For Public Subnet Launched Instances
      map_public_ip_on_launch = true
    }
    ```

2. Run `terraform init` and `terraform apply` to create the VPC.

3. Configure remote state in your Terraform configuration file for managing the state of your infrastructure.

## State Storage

The state of this VPC Terraform module is stored in an S3 bucket. This allows for easier management and collaboration when deploying an EKS cluster, as the state can be accessed remotely.

----
