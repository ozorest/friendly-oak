+++
title = "Terracognita"
description = ""
tags = [
    "terraform",
    "terracognita",
]
date = "2021-04-03"
categories = [
    "Terraform",
]
menu = "main"
+++

Terracognita would be a great name for a movie from Marvel or for a series from Netflix.

Unfortunately it isn't, but it's still cool I promise.

Do you have an environment in the cloud created manually or created using other infra-as-code tool?
But what do you want for real is manage this environment using Terraform?
But are there a lot of resources to code?

So, maybe Terracognita is exactly what you are looking for.

[Terracognita](https://github.com/cycloidio/terracognita) imports your current cloud infrastructure to a HCL Terraform code or to Terraform state, making it manageable by Terraform, in other words, it makes a reverse engineering from what you have in your cloud infra and transform in the code.

For now, the tool supports only AWS, Azure and GCP.

## 1. Installing

If you are using ArchLinux/Manjaro:

```bash
$ yay -S terracognita
$ pamac install terracognita #Manjaro
```

For another distro:

* Go 1.15+ is required

```bash
$ git clone https://github.com/cycloidio/terracognita
$ cd terracognita
$ make install
```

For MacOS users:

```bash
$ brew install terracognita
```

## 2. Sample Usage

### Listing supported resources

```bash
$ terracognita aws resources #AWS
$ terracognita azurerm resources #Azure
$ terracognita google resources #GCP
```

### Importing resources

#### AWS

{{< rawhtml >}}
    <script id="asciicast-404941" src="https://asciinema.org/a/404941.js" async></script>
{{< /rawhtml >}}

```bash
# For import all resources from AWS of the us-east-2
terracognita aws --aws-shared-credentials-file ~/.aws/credentials --hcl resources.tf --tfstate terraform.tfstate --aws-default-region us-east-2

# For import only selected resources from the same region
terracognita aws --aws-shared-credentials-file ~/.aws/credentials -i aws_instance -i aws_security_group --hcl resources.tf --tfstate terraform.tfstate --aws-default-region us-east-2

# For import as a module
terracognita aws --aws-shared-credentials-file ~/.aws/credentials -i aws_instance -i aws_security_group --hcl resources.tf --module test --aws-default-region us-east-2
```

#### Azure

```bash
# For import all resources from Azure for a such resource group
terracognita azurerm --client-id 81fc3229-ad4a-4027-909b-a1d2350b7199 --client-secret EEH.8L5.Oj2bqtA55i9DWx4vx5-kb_9Mv1 --resource-group-name terracognita-test --subscription-id 09b31dc8-cc80-45fd-9584-6b67682db8cc --tenant-id 4b3f72e8-d03d-4db6-b328-7cb606f550be --hcl resources.tf --tfstate terraform.tfstate

# For import selected resources from the same resource group
terracognita azurerm --client-id 81fc3229-ad4a-4027-909b-a1d2350b7199 --client-secret EEH.8L5.Oj2bqtA55i9DWx4vx5-kb_9Mv1 --resource-group-name terracognita-test --subscription-id 09b32dc8-cc80-47fd-9584-6b68882db8cc --tenant-id 4b3f72e8-d03d-4db6-b328-7cb606f550be -i azurerm_virtual_machine -i azurerm_resource_group --hcl resources.tf --tfstate terraform.tfstate

# For import as a module
terracognita azurerm --client-id 81fc3229-ad4a-4027-909b-a1d2350b7199 --client-secret EEH.8L5.Oj2bqtA55i9DWx4vx5-kb_9Mv1 --resource-group-name terracognita-test --subscription-id 09b32dc8-cc80-47fd-9584-6b68882db8cc --tenant-id 4b3f72e8-d03d-4db6-b328-7cb606f550be -i azurerm_virtual_machine -i azurerm_resource_group --module test --hcl resources.tf --tfstate terraform.tfstate
```

#### GCP

{{< rawhtml >}}
    <script id="asciicast-330055" src="https://asciinema.org/a/330055.js" async></script>
{{< /rawhtml >}}

```bash
# For import all resources from Azure for a such resource group
terracognita google --project cycloid-sandbox --region us-central1 --credentials cycloid-iam-9789b361a19b.json --tfstate resources.tfstate --hcl resources.tf

# For import selected resources
terracognita google --project cycloid-sandbox --region us-central1 --credentials cycloid-iam-9789b361a19b.json --tfstate resources.tfstate --hcl resources.tf -i google_compute_instance -i google_compute_network

# For import as a module
terracognita google --project cycloid-sandbox --region us-central1 --credentials cycloid-iam-9789b361a19b.json --tfstate resources.tfstate --hcl resources.tf -i google_compute_instance -i google_compute_network --module test
```

The tool has a lot of other options that is worth to experiment.

**For more information:**
- https://blog.cycloid.io/what-is-terracognita
- https://github.com/cycloidio/terracognita