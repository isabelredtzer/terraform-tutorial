# Terraform Tutorial
This is a Terraform Tutorial for DevOps course DD2482 at KTH.

## Installation 

You can follow the complete installation guide [here](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/docker-get-started). 

### Terraform for Mac OS X
Start with installing the HashiCorp tap, which is a repository with all of their Homebrew packages. 

    brew tap hashicorp/tap

Then move on to installing Terraform with the following command: 

    brew install hashicorp/tap/terraform

### Docker for Mac OS X 
Start with downloading Docker from [this](https://docs.docker.com/desktop/mac/install/) link. 

Now it's time to start the Docker Desktop:

    open -a Docker

Then you create a directory with the name of your choice:

    mkdir name-of-the-dir

And move on to navigating to it: 

    cd name-of-the-dir

### Terraform and Docker

Now it's time to create a file named **main.tf**. In this file you will add the configuration you want to use for Docker and Terraform. This is an example file which will work on Mac OS X and Linux. 

    terraform {
        required_providers {
            docker = {
                source  = "kreuzwerker/docker"
                version = "~> 2.13.0"
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

This is the outline of the file:

* **terraform{}** - this block contains the information about the Terraform settings. You will have to add a source to each provider.
* **provider** - this block states which provider is being used, which in this case is docker. 
* **resource** - this block defines the components. In this case it contains a Docker image but it can also contain logical resources such as a Heroku application. 

Now you can initialize the project in the command line with the following command:

    terraform init

You can now move on to initializing the NGIX server with the following command:

    terraform apply

When using this command Terraform will ask you to confirm the action, so type **yes** and press ENTER. 

You should be able to verify this by visiting [localhost:8000](localhost:8000) or by running the command docker ps to see that the container is being used in action. 

To stop the container you can just run:

    terraform destroy 

If you found the presentation interesting and want to develop this further you can keep working by creating real infrastructure in the cloud you choose. 


## Build & Change Infrastructure 

The following command will update all configurations for readability and consistency:

    terraform fmt

If you want to make sure that your configuration is correct and up to date, all you have to do is run the following command:

    terraform validate

To see what changes will be made by the edits in the file, you can use the following command:

    terraform plan

The terraform plan command is similar to git diff and will show you the changes made in your files and what will need to happen in order to create the end result. 

To apply changes, use the following command:

    terraform apply

You will have to type in yes and press ENTER to be able to actually apply the changes. 

If you want to see the current state you can use the following command:

    terraform show


## Destroy Infrastructure 
If you have finished with the project, you can destroy what you have created with the command:

    terraform destroy