## This reposistory is created with learning purposes for Terraform, focusing on Terraform graphs and interpolation.

## Purpose :

- The repository provides simple introduction to Terraform internal system.

## How to install terraform : 

- The information about installing terraform can be found on the HashiCorp website 
[here](https://learn.hashicorp.com/terraform/getting-started/install.html)

## Terraform internals : 

- Terraform is a Infrastructure as Code tool, it is used to provision infrastructure, either in the public or private cloud. Infrastructure usually has dependencies between different resources, for example, to run a web application that requires database server, this DB server should be deployed before the web application. Terraform takes care of all those dependencies using graphs and interpolation, for example :

```
provider "aws" {
  region = "us-west-2"
}

data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-trusty-14.04-amd64-server-*"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  owners = ["099720109477"] # Canonical
}

resource "aws_instance" "web" {
  ami           = "${data.aws_ami.ubuntu.id}"
  instance_type = "t2.micro"

}
```
- In this example the `data source` named `ubuntu` of type `aws_ami` is going to look for specific AMI. But as you can see the output of this data source is used in creating the resource `web` of type `aws_instance` to point the specific image to create the instance from, this is called `interpolation`. Terraform build internal graph dependencies between the resources. 
- The dependencies between the resources can be reviewed by using `terraform graph`, its output is suitable for drawing tools like `http://www.webgraphviz.com`. 