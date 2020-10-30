To install Terraform, find the appropriate package for your system and download it as a zip archive
Terraform runs as a single binary named terraform. Any other files in the package can be safely removed and Terraform will still function.
Finally, make sure that the terraform binary is available on your PATH. 
#### For Linux set path via:-

export PATH=/path/to/terraform:$PATH

#### Once setup is done get a hold on Terraform's sub-commands :-
terraform -help

### As a beginning task we shall use Docker service to start NGINX webserver

->Note :- Install Docker for the walkthrough

#### Make new directory and work on it
mkdir terraform-docker-demo && cd $_

#### Terraform neds a configuration file to interact with 

nano example.tf ( The extension should be compulsarily .tf)
```
terraform {
  required_providers {  
    docker = {    
      source = "terraform-providers/docker" 
    } 
  } 
}
provider "docker" {}
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false  
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

#### Initialize the project, which downloads a plugin that allows Terraform to interact with Docker.

terraform init

#### Provision the NGINX server container with apply

terraform apply

#### To stop the container, run terraform destroy

terraform destroy

### Now we can move with using AWS 

It is important to have :-

1. An AWS account

2. The AWS CLI installed

3. Your AWS credentials configured locally.

#### Make a new directory to work.

#### Craete a configuration file

nano example.tf 

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.70"
    }
  }
}

provider "aws" {
  profile = "default"
  region  = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"
}
```

#### Use init
terraform init

#### Validate your configuration. If your configuration is valid, Terraform will return a success message.

terraform validate

#### Then apply (After any change made to configuration file this vommand needs to be run)

terraform apply

#### Then destroy ( The terraform destroy command terminates resources defined in your Terraform configuration. )

terraform destroy
