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

Terracognita poderia ser um bom nome para um filme da Marvel ou para uma série do Netflix.

Infelizmente não é, mas ainda sim é legal, eu prometo.

Você tem um ambiente em cloud que foi criado manualmente ou usando outra ferramenta de infra como código?
Mas na verdade você queria usar o Terraform para gerenciar este ambiente?
Mas tem muitos recursos para codificar?

Então talvez o Terracognita seja exatamente o que você está procurando.

[Terracognita](https://github.com/cycloidio/terracognita) importa sua infraestrutura atual na nuvem para um código HCL do Terraform ou para um arquivo de state do Terraform, fazendo possível sua infra ser gerenciada pelo Terraform, ou seja, ela faz uma engenharia reversa do que você tem na sua nuvem e transforma no código para você.

Atualmente, a ferramenta só suporta AWS, Azure e GCP.

## 1. Instalando

Se você está usando ArchLinux/Manjaro:

```bash
$ yay -S terracognita
$ pamac install terracognita #Manjaro
```

Uma outra distro:

* Go 1.15+ é necessário

```bash
$ git clone https://github.com/cycloidio/terracognita
$ cd terracognita
$ make install
```

Para usuários MacOS:

```bash
$ brew install terracognita
```

## 2. Uso de exemplo

### Listando os recursos suportados de serem importados

```bash
$ terracognita aws resources #AWS
$ terracognita azurerm resources #Azure
$ terracognita google resources #GCP
```

### Importando recursos

#### AWS

{{< rawhtml >}}
    <script id="asciicast-404941" src="https://asciinema.org/a/404941.js" async></script>
{{< /rawhtml >}}

```bash
# Para importar todos os recursos da AWS da região us-east-2
terracognita aws --aws-shared-credentials-file ~/.aws/credentials --hcl resources.tf --tfstate terraform.tfstate --aws-default-region us-east-2

# Para importar recursos selecionados da mesma região
terracognita aws --aws-shared-credentials-file ~/.aws/credentials -i aws_instance -i aws_security_group --hcl resources.tf --tfstate terraform.tfstate --aws-default-region us-east-2

# Para importar como módulo
terracognita aws --aws-shared-credentials-file ~/.aws/credentials -i aws_instance -i aws_security_group --hcl resources.tf --module test --aws-default-region us-east-2
```

#### Azure

```bash
# Para importar todos os recursos da Azure para um resource group
terracognita azurerm --client-id 81fc3229-ad4a-4027-909b-a1d2350b7199 --client-secret EEH.8L5.Oj2bqtA55i9DWx4vx5-kb_9Mv1 --resource-group-name terracognita-test --subscription-id 09b31dc8-cc80-45fd-9584-6b67682db8cc --tenant-id 4b3f72e8-d03d-4db6-b328-7cb606f550be --hcl resources.tf --tfstate terraform.tfstate

# Para importar recursos selecionados deste resource group
terracognita azurerm --client-id 81fc3229-ad4a-4027-909b-a1d2350b7199 --client-secret EEH.8L5.Oj2bqtA55i9DWx4vx5-kb_9Mv1 --resource-group-name terracognita-test --subscription-id 09b32dc8-cc80-47fd-9584-6b68882db8cc --tenant-id 4b3f72e8-d03d-4db6-b328-7cb606f550be -i azurerm_virtual_machine -i azurerm_resource_group --hcl resources.tf --tfstate terraform.tfstate

# Para importar como módulo
terracognita azurerm --client-id 81fc3229-ad4a-4027-909b-a1d2350b7199 --client-secret EEH.8L5.Oj2bqtA55i9DWx4vx5-kb_9Mv1 --resource-group-name terracognita-test --subscription-id 09b32dc8-cc80-47fd-9584-6b68882db8cc --tenant-id 4b3f72e8-d03d-4db6-b328-7cb606f550be -i azurerm_virtual_machine -i azurerm_resource_group --module test --hcl resources.tf --tfstate terraform.tfstate
```

#### GCP

{{< rawhtml >}}
    <script id="asciicast-330055" src="https://asciinema.org/a/330055.js" async></script>
{{< /rawhtml >}}

```bash
# Para importar todos os recursos de um projeto e uma região
terracognita google --project cycloid-sandbox --region us-central1 --credentials cycloid-iam-9789b361a19b.json --tfstate resources.tfstate --hcl resources.tf

# Para importar recursos selecionados deste projeto
terracognita google --project cycloid-sandbox --region us-central1 --credentials cycloid-iam-9789b361a19b.json --tfstate resources.tfstate --hcl resources.tf -i google_compute_instance -i google_compute_network

# Para importar como módulo
terracognita google --project cycloid-sandbox --region us-central1 --credentials cycloid-iam-9789b361a19b.json --tfstate resources.tfstate --hcl resources.tf -i google_compute_instance -i google_compute_network --module test
```

A ferramenta tem várias outras opções que vale a pena experimentar.

**Para mais informações:**
- https://blog.cycloid.io/what-is-terracognita
- https://github.com/cycloidio/terracognita