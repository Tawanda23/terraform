# Terraform

## What is Terraform

Terraform is an infrastructure as code tool that is used for ochestration within the devops community. Orcherstration is defined as the automated configuration management of software.

It allows for versioning of cloud resources efficientky

## How dos Terraform work?

Terraform works by creating and managing resources on cloud platforms like Azure/AWS/GCP through the use of the cloud providers APIs. The providers allows terraform to to interact with any service that has an API that is accessible.

Providers can be found in the Terraform Registry. 

Terraform workflow has the following aspects:

* Write  - This stage, the resources are defined which can cross multiple cloud providers

* Plan - Creates a plan with the infrastructure that Terraform will create. This also updates, and or destroys any existing infrastructure

* Apply - Approves changes and applies them


## Why Use Terraform?

Terraform is widely used in the tech industry due to its versaitility. Terraform helps solve the following:

* Managing any Infrastructure

* Tracks Infrastructure

* Automate Changes

* Standardises Configurations

* Collaboration


## Core Concepts

### Providers

Terraform is heavily reliant on plugins  (Providers) to interact with cloud providers.

* Each provider has its own documentation which describes their resource types.
* They should be kept in the root directory of the project
* Multiple provider configurations are possible through the use of aliases as shown below:


```h
provider "aws" {
  region = "eu-west-2"
}

provider "aws" {
  alias  = "Ireland"
  region = "eu-east-2"
}
```

The providers are looked for by terraform and installed after the working directory has been initialised.

## Modules
These are used to define multiple resources that can be used together.

Allows to split out groups of terraform configuration into smaller reusable abstracts

Root Module - this is mandatory as it contains the resources mentioned/defined in the main.tf in root module 

Child Module - These are modules called on by other modules, usually the root module.

```h
module "networking" {
  source = "./networking-module"

  resource_group_name = "aks-networking-resource-group"
  location = "UK West"
  vnet_address_space = ["10.0.0.0/16"]
  secure_address       = var.secure_address
}
```
The example above calls the networking module which is located in the directory 'networking-module'



## Variables

Input Variables -  Allows user to customise parts of the terraform module's code. (inputs are similar to functions in other programming languages)

* When declared in the root module, values should be set using CLI.

* If declared in child modules, the module that is calling should pass values in the module block.

* Key type constructors are:

    * string
    * number
    * bool

* Complex constructors

    * list(<TYPE>).
    * set(<TYPE>).
    * map(<TYPE>).
    * object(.{<ATTR_NAME> = <TYPE>, ...}.).
    * tuple(.[<TYPE>], ....).


```h
resource "aws_vpc" {
    cidr_block = "10.0.0.0/16"

    tag = {
        Name = var.vpcname
    }
}
```
`export TF_VAR_vpcname=envvpc`

`terraform plan -var="vpcname=cliname"`

`terraform plan -var-file=prod.tfvars` -this is used for many environments

### Variables Order of Loading

1. Env Variables
2. .tfvars
3. .tfvars.json
4. any .auto.tfvars
5. any -var -r -var-file


## Terraform Workflow

### Individual

 * write - check latest code

* plan - Run plan and raise a pull request

* Create - creates infrastructure


### Team

* write - check latest code

* paln - Run plan and raise a pull request

* Create - creates infrastructure


### Cloud

* Write - Use .tf cloud as dev environment

* Plan - Pull Request is raised and .tf plan is run

* Create - creates infrastructure



## Workflow

`Terraform init` - initialises the directory

`Terraform Validate` - Checks the validity of the code

`Terraform Plan` - Creates a plan of what is to be created

`Terraform apply` - Runs terraform plan and asks to confirm

`Terraform Destroy` - tears down infrastructure



## Security

### Security Key

*Do not store access keys in the open as this is a security issue*

* Use CLI - `aws configure` for IAM configuration

* Run `terraform apply`

### Sentinel

This is policy as code that lets you control what users can do

* Ensures the infrastructure being created adheres to the policy or regulations required

### Secret Injection




## State Management

