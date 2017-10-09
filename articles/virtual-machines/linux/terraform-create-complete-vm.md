---
title: "aaaCreate Alapinfrastruktúra az Azure-ban Terraform használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate Azure erőforrásokat a Terraform"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: 52591009ee7cb906402b8bca2ce63794ac7afcc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a>Alapvető infrastruktúra létrehozása az Azure-ban Terraform használatával
Ez a cikk az Azure virtuális gép, alapul szolgáló infrastruktúra együtt tootake tooprovision kell hello lépéseit ismerteti. Megtudhatja, hogyan toowrite Terraform parancsfájlok és hogyan toovisualize hello előtt változik, hogy azokat a felhőalapú infrastruktúrában. Azt is megtudhatja, hogyan toocreate infrastruktúra az Azure-ban Terraform használatával.

tooget elindult, választott (Visual Studio Code/Sublime/Vim/stb.) a szövegszerkesztőben \terraform_azure101.tf nevű fájl létrehozása. hello pontos hello fájl neve nem fontos, mert Terraform fogad paraméterként hello mappanév: hello mappában található összes parancsfájl azonnal végrehajtásra kerülhetnek. Illessze be a következő hello új fájl hello:

~~~~
# Configure hello Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have tooinclude this block
provider "azurerm" {
  subscription_id = "your_subscription_id_from_script_execution"
  client_id       = "your_appId_from_script_execution"
  client_secret   = "your_password_from_script_execution"
  tenant_id       = "your_tenant_id_from_script_execution"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}
~~~~
A hello `provider` hello szakasza parancsfájl közli Terraform toouse egy Azure szolgáltató tooprovision erőforrások hello parancsfájlban. tooget értékek ELŐFIZETÉS_AZONOSÍTÓJA, appId, jelszó és tenant_id, lásd: hello [telepítse és konfigurálja a Terraform](terraform-install-configure.md) útmutató. Ha a blokk környezeti változók hello értékek hozott létre, nem kell tooinclude azt. 

Hello `azurerm_resource_group` erőforrás utasítja Terraform toocreate egy új erőforráscsoportot. A cikk későbbi részében Terraform elérhető további erőforrástípusok tekintheti meg.

## <a name="execute-hello-script"></a>Hello parancsfájlt
Hello parancsfájl mentése után toohello konzol/parancssorból való kilépéshez, és írja be a következő hello:
```
terraform init
```
 tooinitialize hello Terraform szolgáltató az Azure-bA. Majd módosítsa a hello túl**terraformscripts**, és probléma hello a következő parancsot:
```
terraform plan
```
Hello használtuk `plan` Terraform parancsot, amely ellenőrzi, hogy az hello parancsfájlok definiált hello erőforrásokat. Terraform által mentett toohello állapotadatokat összehasonlítja őket, és majd kimenetek hello tervezett végrehajtási _nélkül_ ténylegesen létrehozása az Azure-erőforrások. 

Hello előző parancs végrehajtása után kell megjelennie a következő képernyő hello hasonlót:

![Terraform terv](./media/terraform/tf_plan2.png)

Ha minden megfelelő, az új erőforráscsoportot az Azure-ban kiépítése a hello alábbiak végrehajtásával: 
```
terraform apply
```
Hello Azure-portálon, a kell megjelennie hello üres nevű új erőforráscsoport `terraformtest`. A következő szakasz hello hozzáadhat egy virtuális gépet és annak minden virtuális gép toohello erőforráscsoporthoz támogató infrastruktúra hello.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>A Terraform Ubuntu virtuális gép kiépítése
Létrehoztunk Önnek hello adatokkal, amelyek a szükséges tooprovision hello Terraform parancsfájl most kiterjesztése futtató Ubuntu virtuális gép. a következő részekben hello kiépítése hello erőforrásokat a következők:

* Egyetlen alhálózattal rendelkező hálózat
* A hálózati kártya 
* Hello virtuális gép diagnosztikai tárfiók
* Egy nyilvános IP-cím
* A virtuális gép, amely használja az összes hello előző erőforrások 

Az egyes hello Azure Terraform erőforrások alapos dokumentációjáért lásd: hello [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).

hello teljes verzióját hello [telepítési parancsfájlban](#complete-terraform-script) is biztosított kényelmét szolgálja.

### <a name="extend-hello-terraform-script"></a>Hello Terraform parancsfájl kiterjesztése
A következő erőforrások hello hoztak létre hello parancsfájl kiterjesztése: 
~~~~
# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}
~~~~
hello előző parancsfájlja létrehozza a virtuális hálózat és egy alhálózatot a virtuális hálózaton belül. Vegye figyelembe a hello hivatkozás toohello erőforráscsoport már hozott létre "${azurerm_resource_group.helloterraform.name}" hello virtuális hálózati és alhálózati definíció hello keresztül.

~~~~
# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
hello előző parancsfájl kódtöredékek hozzon létre egy nyilvános IP-cím és a hálózati adaptert, amely használja a hello nyilvános IP-cím létrehozása. Megjegyzés: hello toosubnet_id és public_ip_address_id hivatkozik. Terraform rendelkezik beépített funkciói toounderstand adott hello hálózati illesztőnek egy függőséget hello erőforrástól függ, hogy hello hello hálózati illesztő létrehozása előtt létre kell toobe.

~~~~
# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Itt létrehozott egy tárfiókot. Ez a tárfiók, ahol hello virtuális gép fogja tárolni a diagnosztikai részletek.

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Végezetül hello előző részlet hoz létre egy virtuális gép, amely már kiosztott összes hello erőforrásokat használja. A tárfiók hello virtuálisgép-diagnosztika, a hálózati illesztő – nyilvános IP- és a megadott alhálózat, és hello erőforráscsoportban már létrehozott. Vegye figyelembe a hello vm_size tulajdonság, ahol hello parancsfájl határozza meg az Azure Standard DS1v2 Termékváltozat.

Nyilvános SSH-kulcs szükséges toosupply áll. Nyilvános kulcs értéke hello helyezzen hello szakasz **... HELYEZZE BE A PROTOKOLL OPENSSH ITT A NYILVÁNOS KULCSOT...**  felett. Használhat egy meglévő ssh kulcs pár, vagy hajtsa végre a hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) vagy [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) dokumentáció toogenerate hello kulcsból álló kulcspárt.

### <a name="execute-hello-script"></a>Hello parancsfájlt
Hello teljes mentett, parancsfájl toohello konzol/parancssorból való kilépéshez, és írja be a következő hello:
```
terraform apply
```
Némi várakozás után hello erőforrások, például egy virtuális gép jelennek meg hello `terraformtest` erőforráscsoportja hello Azure-portálon.

## <a name="complete-terraform-script"></a>Teljes Terraform parancsfájl

Az Ön kényelme érdekében alatt van hello teljes Terraform parancsfájlt, hogy az összes hello infrastruktúra cikkben említett rendelkezések.

```
# Configure hello Microsoft Azure Provider
provider "azurerm" {
  subscription_id = "XXX"
  client_id       = "XXX"
  client_secret   = "XXX"
  tenant_id       = "XXX"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}

# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}

# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```

## <a name="next-steps"></a>Következő lépések
Alapvető infrastruktúra az Azure-ban hozott létre a Terraform használatával. Összetettebb forgatókönyveket, beleértve a példák, amelyek használatát egy terheléselosztó és a virtuális gép skálázása beállítása, lásd: számos [Terraform példák az Azure-](https://github.com/hashicorp/terraform/tree/master/examples). Támogatott Azure szolgáltatók naprakész listáját, lásd: hello [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).
