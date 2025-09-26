For any kind of resource you need to create using terraform, you have to mention the provider. The providers can be local providers `local` to create resources locally and it can be cloud providers like `aws`, `azure`, `gcp`.
Generally all the providers are mentioned in a separate `terraform.tf` file. 
```hcl
provider "aws" {
  region = "us-east-1" # Change region if needed
}
```

Provider is a `block_type` which does not have any parameters.

Note: Every time you mention a provider in you HCL file you have to do `terraform init` to install that provider. So simplify the process or have all the provider configuration in one file you can create a terraform.tf and add the providers and their configuration files in that.

Suppose you have certain configuration for your provider like all the resources needs to be created in `us-east-1` you can add those configurations in a separate file called `provider.tf`

Main.tf
```hcl
# Get Default VPC id
data "aws_vpc" "default" {
  default = true
}

# Get us-east-1a subnet-id
# data "aws_subnet" "test" {
#   vpc_id            = data.aws_vpc.default.id
#   availability_zone = "us-east-1a"
# 
# }

# Get the latest Ubuntu AMI
#data "aws_ami" "ubuntu" {
#  most_recent = true
#  owners      = ["099720109477"] # Canonical
#
#  filter {
#    name   = "name"
#    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
#  }
#
#  filter {
#    name   = "virtualization-type"
#    values = ["hvm"]
#  }
#}

# Create a security group allowing SSH, HTTPS, and Custom TCP Port
resource "aws_security_group" "terraform_sg" {
  name        = "terraform-sg"
  description = "Allow SSH, HTTP, and HTTPS"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "Allow port 22"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow port 80000"
    from_port   = 8000
    to_port     = 8000
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow port 80"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow port 443"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

#Creating an EC2 instance
resource "aws_instance" "terraform" {
  ami           = var.ubuntu_golden_ami
  instance_type = var.instance_type
  #   subnet_id              = data.aws_subnet.test.id
  vpc_security_group_ids = [aws_security_group.terraform_sg.id]
  key_name               = "PrivateKey"
  #user_data              = file("install_nginx.sh")

  root_block_device {
    volume_size = var.volume_size
    volume_type = "gp2"
  }

  tags = {
    Name = "Terraform Instance"
  }
}
```



Variables.tf
```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "volume_size" {
  description = "Size of the EBS volume in GB"
  type        = number
  default     = 10
}

variable "ubuntu_golden_ami" {
  description = "Latest ubuntu Golder AMI"
  type        = string
  default     = "ami-0dbde2e7b185ef946"
}
```


```hcl

output "Instance_Name" {
  description = "The name of the EC2 instance"
  value       = aws_instance.terraform.tags["Name"]
}

output "Public_IP" {
  description = "The public IP address of the EC2 instance"
  value       = aws_instance.terraform.public_ip
}


output "Private_IP" {
  description = "The private IP address of the EC2 instance"
  value       = aws_instance.terraform.private_ip
}
```