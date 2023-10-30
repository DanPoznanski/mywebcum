---
title: "Terraform"
discription: Terraform 
date: 2023-04-03T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","terraform","Go"]
showTableOfContents: true
--- 






![terraform1](images/terraform1.svg)


structura of terraform:

main.tf

outputs.tf

terraform.tfstate

terraform.tfstate.backup

variable.tf

 

 used for create new project `terraform init` command in Terraform initializes a new or existing Terraform configuration, setting up the environment, downloading necessary plugins, modules, and configuring the backend to prepare for managing infrastructure as code.
 ```bash
cd /path/to/your/terraform/project
terraform init
 ```

`terraform plan` command in Terraform creates an execution plan that outlines the actions Terraform will take to modify the infrastructure according to the configuration files. It shows the changes, additions, modifications, or deletions of resources without actually applying these changes, allowing users to review and validate the proposed modifications before execution.

```
 terraform plan
```

`terraform apply`  command in Terraform applies the changes specified in your configuration files to the infrastructure, creating, modifying, or destroying resources as necessary to achieve the desired state.
```
terraform apply 
```

```
`terraform destroy`  command in Terraform destroys all the resources created by your Terraform configuration, effectively tearing down the entire infrastructure.
```
terraform destroy
```


terraform used syntex `HCL configuration syntax` 

```hcl
# main.tf

provider "ilo" {
  host        = "<ILO_IP>"
  username    = "<ILO_USERNAME>"
  password    = "<ILO_PASSWORD>"
  server_certificate_verification = "false"
}

resource "ilo_server_profile" "example_server_profile" {
  name                   = "ExampleServerProfile"
  server_hardware_type   = "SY 480 Gen10"
  enclosure_group_uri    = "/rest/enclosure-groups/<ENCLOSURE_GROUP_URI>"
  server_hardware_uri    = "/rest/server-hardware/<SERVER_HARDWARE_URI>"
  enclosure_index        = 1

  bios {
    manage_bios = "true"
  }

  boot {
    manage_boot = "true"
    order       = ["Cd", "Hdd", "Usb"]
  }

  local_storage {
    manage_local_storage = "true"
  }
}

resource "ilo_virtual_media" "example_virtual_media" {
  name       = "ExampleVirtualMedia"
  boot       = "Once"
  image_uri  = "http://path/to/your/iso/image.iso"
  server_uri = ilo_server_profile.example_server_profile.uri
}

resource "ilo_power" "example_power" {
  name      = "ExamplePower"
  server_uri = ilo_server_profile.example_server_profile.uri
}

resource "ilo_user" "example_user" {
  name          = "ExampleUser"
  login_name    = "exampleuser"
  password      = "examplepassword"
  server_uri    = ilo_server_profile.example_server_profile.uri
  privileges    = ["Login"]
}

```