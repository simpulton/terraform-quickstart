# Getting Started with Terraform

Terraform is a code tool that allows organizations to quickly and reliably define and provision infrastructure. The ability to provision infrastructure as code (IaC) enables operators and teams to manage complex infrastructure through human-readable, automated deployments. We are going to walk through the necessary steps you need to get started with Terraform.

## Prerequisites
To follow this tutorial, you will need:

[Docker](https://docs.docker.com/get-docker/) installed

## Install Terraform

The first step to using Terraform is to install it. You can visit [Terraform.io](https://learn.hashicorp.com/tutorials/terraform/install-cli) to see the various install options. The easiest way to get up and running with Terraform is to use a package manager.

If you are on OS X, you can use [Homebrew](https://brew.sh/) to install Terraform by running this command:

```shell
$ brew install hashicorp/tap/terraform
```

If you are on Windows, you can use [Chocolatey](https://chocolatey.org/) to install Terraform by running this command:

```shell
$ choco install terraform
```

If you want to install Terraform manually, you can visit the [Terraform.io downloads page](https://www.terraform.io/downloads.html) and download the appropriate binary package as a zip archive. Once you have downloaded the archive, you can extract the **terraform** binary and run Terraform. You may need to make sure that **terraform** is in your **PATH**, depending on your local environment. 

You can verify that Terraform was successfully installed by listing the available subcommands from a terminal.

```shell
$ terraform -help
```

## Write Configuration

With Terraform installed, you are now ready to describe your infrastructure by writing your first Terraform configuration. Terraform configuration is the set of files that describes your infrastructure. You will create the configuration necessary to launch a Docker image with Nginx. 

Create a new directory on your local machine and then navigate inside of it.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Create a file for your configuration code.

```shell
$ touch main.tf
```

Paste the following lines into **main.tf** and save it.

```hcl
provider "docker" {
    host = "unix:///var/run/docker.sock"
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name = "training"
  ports {
    internal = 80
    external = 80
  }
}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

## Initialize the Directory

With your configuration complete, you need to initialize your directory so that Terraform has the necessary plugins to deploy your configuration. In this case, we need to install the **Docker** provider.

Initialize Terraform with the init command. 

```shell
$ terraform init
```

Upon completion, Terraform has downloaded the **Docker** plugin into your working directory, and you are ready to create your infrastructure.

## Create the Infrastructure

We recommend that you format and validate your configuration files before deploying your infrastructure. These extra steps ensure consistency when working within a large team or even multiple teams.

Format your configuration.

```shell
$ terraform fmt
```

Validate your configuration.

```shell
$ terraform validate
```

If your configuration is successfully validated, you are ready to provision the resource with the apply command. 

```shell
$ terraform apply
```

The command may take up to a few minutes to run and will display an execution plan describing the changes that Terraform will make to apply these changes to real infrastructure. 

If the execution plan is acceptable, type `yes`, and hit **ENTER** at the command prompt to proceed. 

## Destroy the Infrastructure

Terraform adheres to a principle of [immutable infrastructure](https://www.hashicorp.com/tao-of-hashicorp#immutability), which means that we never change existing infrastructure, but instead we replace it with new infrastructure. To accomplish this, we need the ability to destroy any existing infrastructure before we deploy new infrastructure.

The opposite of `terraform apply` is `terraform destroy`, which allows us to terminate any resources created by executing the current configuration.

```shell
$ terraform destroy
```

Confirm that you want to destroy the created resources at the prompt by answering `yes`, which will execute the plan to destroy the existing infrastructure.

## Next Steps

In this tutorial, you have learned:

- How to install Terraform
- How to create a Terraform configuration file
- How to initialize Terraform
- How to format and validate Terraform configuration files
- How to create infrastructure using Terraform
- How to destroy infrastructure using Terraform

To create infrastructure with dynamic input variables, continue to the [Define Input Variables tutorial](https://learn.hashicorp.com/tutorials/terraform/aws-variables?in=terraform/aws-get-started).

