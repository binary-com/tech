---
layout: post
title: "Automating Cloud Infrastructure Provisioning With Terraform"
modified:
categories: 
description:
tags: [cloud, terraform, orchestration, aws]
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2015-11-27T14:29:56+08:00
---
# Intro
This will be a short introduction to terraform, tool for automating
cloud infrastructure management.

## Infrastructure on a cloud

* Let's talk about IT infrastructure on a cloud
* After your infrastructure size reach some level, let's say about 30
  instances, you need to think about automated solution for
  provisioning them and configure your cloud.
* Also you need to have some change management
* And of course if you have more than one people changing your cloud
  infrastrucutre you need to share changes you made.

## Terraform

* So we want to automate our cloud infrastructure configuration.
* For middle sized infrastructure on a cloud (like 30-1000 hosts)
[Terraform](https://terraform.io) could be a good choice.
* Discussing other orchestration solution is a topic for separate
  article, so I just limit this section with describing terraform main
  features.

So what's good about it?

<!-- more -->

* It's cloud agnostic. You can use [Terraform](https://terraform.io)
to automate AWS, Rackspace, Google cloud, etc.
* It supports sharing your infrastructure saved state over git. So you
can collaborate in a team on developming your infrastructure.
* It's simple. It can be a good thing or a bad thing, depending on a
  task that you want to accomplish. But usually - if something is
  simple - it feels/works great.
* it shows you what does it plan to do, before actually do it.
* For golang lovers - it's written in golang.

# Terraform setup
incredibly simple.

* You copy files for your architecture from [Terraform: downloads](https://terraform.io/downloads.html)
* unzip
* copy to /usr/local/bin

done. it's ready for use.

# Terraform workflow

* Terraform read all *.tf files in the folder where you run it.
* After applying configuration to the cloud it save you cloud state
representation at **terraform.tfstate** file.
* That file should be commited and pushed to common repository along
with your changes.
* To review changes that terraform plan to do you should use `terraform plan`
* To apply changes you should use `terraform apply`

# Usage example
Let's configure something. For example: AWS security groups.

~~~
mkdir orchestration
cd orchestration
~~~

## Authentication on AWS
Let's export AWS access and secret key environment variables:

~~~
export TF_VAR_aws_access_key=_my_aws_access_key_
export TF_VAR_aws_secret_key=_my_aws_secret_key_
~~~

Environment variables exported in this format will be available inside
terraform as:

~~~
aws_access_key
aws_secret_key
~~~

variables.

## Configure AWS provider
Let's presume that we have 2 AWS regions.

**aws.tf**

~~~
variable "aws_access_key" {}
variable "aws_secret_key" {}

provider "aws" {
    access_key = "${var.aws_access_key}"
    secret_key = "${var.aws_secret_key}"
    region = "us-east-1"
    alias = "us"
}

provider "aws" {
    access_key = "${var.aws_access_key}"
    secret_key = "${var.aws_secret_key}"
    region = "eu-central-1"
    alias = "eu"
}
~~~

Okay, we defined two regions, let's create some security groups there.

**security_groups.tf**

~~~
resource "aws_security_group" "common_security_group_eu" {

  provider = "aws.eu"
  vpc_id = "vpc-xxxxxxxxx"
  description = "common_security_group"

  ingress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 443
    to_port = 443
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 22
    to_port = 22
    protocol = "tcp"
    cidr_blocks = ["xxx.xxx.xxx.xxx/32", "yyy.yyy.yyy.yyy/32"]
  }
	
  ingress {
    from_port = -1
    to_port = -1
    protocol = "icmp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port = 0
    to_port = 0
    protocol = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags {
      Name = "common_security_group"
  }
}

resource "aws_security_group" "common_security_group_us" {

  provider = "aws.us"
  vpc_id = "vpc-xxxxxxxxx"
  description = "qa_security_group"

  ingress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 443
    to_port = 443
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 22
    to_port = 22
    protocol = "tcp"
    cidr_blocks = ["xxx.xxx.xxx.xxx/32", "yyy.yyy.yyy.yyy/32"]
  }

  ingress {
    from_port = 8110
    to_port = 8110
    protocol = "tcp"
    security_groups = ["sg-fffffff"]
  }

  ingress {
    from_port = -1
    to_port = -1
    protocol = "icmp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port = 0
    to_port = 0
    protocol = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags {
      Name = "common_security_group"
  }
}
~~~

## Print security group names in the end of terraform execution

This will print computed security group ids in the end of terraform execution:


**output.tf**

~~~
output "security_group_id_us_common" {
    value = "${aws_security_group.common_security_group_us.id}"
}

output "security_group_id_eu_common" {
    value = "${aws_security_group.common_security_group_eu.id}"
}
~~~

## Test and apply configuration

~~~
terraform plan
terraform apply
~~~

done, your security groups are ready for use. Just assign them to your
hosts and enjoy AWS security groups managed with code.

# Summary and afterthoughs

* This article is a just short overview, that allow you to see what's
terraform
* Terraform has much more features for cloud configuration than described in this article. You can read about them at [Terraform: documentation](https://terraform.io/docs)
* Security group configuration it's only beginning. You can describe all your cloud network infrastructure and instances etc.
* And even further Terraform allow you to do initial provisioning of your hosts with configuration management software, like Chef.
* In additional to presented config files format, terraform suports JSON based configs, that allow you to generate terraform configs with your own software if necessary.

# References

* [Terraform](https://terraform.io)
* [Terraform: downloads](https://terraform.io/downloads.html)
* [Terraform: documentation](https://terraform.io/docs)
