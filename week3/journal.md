
I am following the hands-on lab from Cloud Academy, the learning path: https://cloudacademy.com/learning-paths/terraform-on-aws-1-2377/

Then, I Followed the lab 1, i.e: installing terraform which was reltively easy.

LAB 2: Deploying Amazon VPC with Terraform
the following code I used in Lab 2
> terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.9.0"
    }
  }
}
resource "aws_vpc" "lab_vpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "ca-lab-vpc"
  }
}
output "id" {
  description = "VPC ID"
  value       = aws_vpc.lab_vpc.id
}
output "route_table_id" {
  description = "Route Table ID associated with this VPC"
  value       = aws_vpc.lab_vpc.main_route_table_id
}
output "security_group_id" {
  description = "Default Security Group ID"
  value       = aws_vpc.lab_vpc.default_security_group_id
}

