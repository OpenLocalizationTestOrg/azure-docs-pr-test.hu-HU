---
title: "Alapvető infrastruktúra létrehozása az Azure-ban Terraform használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre Azure-erőforrások Terraform használatával"
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
ms.openlocfilehash: 9660a95b440c2e4311829979e270d9f10099f624
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a>Alapvető infrastruktúra létrehozása az Azure-ban Terraform használatával
Ez a cikk ismerteti a lépéseket kell tennie az Azure virtuális gép, és az alapjául szolgáló infrastruktúra kiépítéséhez. Megtudhatja, hogyan Terraform parancsfájlokat írhat és hogyan megjelenítéséhez a módosításokat, mielőtt azokat a felhőalapú infrastruktúrában. Azt is megtudhatja, hogyan hozhat létre az infrastruktúra az Azure-ban Terraform használatával.

Első lépésként hozzon létre egy \terraform_azure101.tf nevű választott (Visual Studio Code/Sublime/Vim/stb.) a szövegszerkesztőben. A fájl a pontos név nem fontos, mert Terraform fogad paraméterként a mappa neve: a mappában található összes parancsfájl azonnal végrehajtásra kerülhetnek. Illessze be a következő kódot az új fájlban:

~~~~
# Configure the Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have to include this block
provider "azurerm" {
  subscription_id = "your_subscription_id_from_script_execution"
  client_id       = "your_appId_from_script_execution"
  client_secret   = "your_password_from_script_execution"
  tenant_id       = "your_tenant_id_from_script_execution"
}

# create a resource group 
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}
~~~~
Az a `provider` szakasz parancsfájl, a parancsfájl egy Azure szolgáltató rendelkezés erőforrások használatának Terraform biztosítják. Értékek ELŐFIZETÉS_AZONOSÍTÓJA, appId, jelszó és tenant_id, megtekinteni a [telepítése és konfigurálása Terraform](terraform-install-configure.md) útmutató. Ha a blokk környezeti változók értékek hozott létre, nem kell is. 

A `azurerm_resource_group` erőforrás utasítja Terraform egy új erőforráscsoport létrehozásához. A cikk későbbi részében Terraform elérhető további erőforrástípusok tekintheti meg.

## <a name="execute-the-script"></a>Futtassa a parancsfájlt
A parancsfájl mentése után Kilépés a konzol/parancssorba, és írja be a következőt:
```
terraform init
```
az Azure-Terraform szolgáltató inicializálása. Írja be a következőt:
```
terraform plan terraformscripts
```
Feltételezzük, hogy `terraformscripts` az a mappa, ahol a parancsfájl mentve van. Használhatja a `plan` Terraform parancsot, amely ellenőrzi, hogy az erőforrásokat, a parancsfájlok definiált. Összehasonlítja azokat az állapotadatokat Terraform mentette, és majd exportálja a tervezett végrehajtási _nélkül_ ténylegesen létrehozása az Azure-erőforrások. 

Az előző parancs végrehajtása után kell megjelennie a következő képernyő hasonlót:

![Terraform terv](linux/media/terraform/tf_plan2.png)

Ha minden helyes, kiépítése az új erőforráscsoportot az Azure-ban a következő lépések végrehajtásával: 
```
terraform apply terraformscripts
```
Az Azure portálon kell megjelennie a nevű új üres erőforráscsoport `terraformtest`. A következő szakaszban hozzáadhat egy virtuális gép és a támogató infrastruktúra, a virtuális gép az erőforráscsoportot.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>A Terraform Ubuntu virtuális gép kiépítése
Most kiterjesztése a Terraform parancsfájlt futtató Ubuntu virtuális gép kiépítéséhez szükséges a adatokkal létrehoztunk Önnek. Az erőforrásokat, amelyek a következő szakaszokban kiépítése a következők:

* Egyetlen alhálózattal rendelkező hálózat
* A hálózati kártya 
* A tárfiók egy tároló
* Egy nyilvános IP-cím
* A virtuális gép, amely használja az előző erőforrások 

Az egyes Azure Terraform erőforrások alapos dokumentációjáért lásd: a [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).

A teljes verzióját a [telepítési parancsfájlban](#complete-terraform-script) is biztosított kényelmét szolgálja.

### <a name="extend-the-terraform-script"></a>A Terraform parancsfájlt kiterjeszteni
A következő erőforrások hoztak létre a parancsfájl kiterjesztése: 
~~~~
# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}
~~~~
A korábbi parancsfájlja létrehozza a virtuális hálózat és egy alhálózatot a virtuális hálózaton belül. Jegyezze fel az "${azurerm_resource_group.helloterraform.name}" a virtuális hálózat és az alhálózati definíció keresztül már létrehozott erőforráscsoport mutató hivatkozást.

~~~~
# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "TerraformDemo"
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
}
~~~~
Az előző parancsfájl kódtöredékek hozzon létre egy nyilvános IP-cím, és a hálózati adaptert, amely a nyilvános IP-címet használja létrehozni. Vegye figyelembe a subnet_id és public_ip_address_id mutató hivatkozásokat. Terraform megérteni, hogy rendelkezik-e a hálózati adapter egy függőséget az erőforrásokat, amelyek a hálózati illesztő létrehozása előtt kell létrehozni a beépített eszközintelligencia rendelkezik.

~~~~
# create a random id
resource "random_id" "randomId" {
  byte_length = 4
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "tfstorage${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}
~~~~
A tárfiók létrehozott itt, és meghatározott egy tárolót a tárfiókon belül. Ez a tárfiók a virtuális merevlemezek (VHD) tárolására kell készíteni a virtuális géphez.

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
~~~~
Végül az előző részlet hoz létre egy virtuális gép, amely már kiépített összes erőforrást használja. Egy tárfiók és tároló virtuális merevlemezek, a nyilvános IP- és, a megadott alhálózat egy hálózati adapter, és az erőforráscsoport hozott létre. Vegye figyelembe a vm_size tulajdonságot, ha a parancsfájl meghatározza egy Azure A0 Termékváltozat.

### <a name="execute-the-script"></a>Futtassa a parancsfájlt
A teljes parancsfájl mentése, a kilépéshez a konzol/parancssorba, és írja be a következőt:
```
terraform apply terraformscripts
```
Némi várakozás után az erőforrások, például egy virtuális gép megjelennek a `terraformtest` erőforráscsoportot az Azure portálon.

## <a name="complete-terraform-script"></a>Teljes Terraform parancsfájl

Az Ön kényelme érdekében alatt van a teljes Terraform parancsfájlt, hogy az infrastruktúra minden cikkben említett rendelkezések.

```
variable "resourcesname" {
  default = "helloterraform"
}

# Configure the Microsoft Azure Provider
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

# create virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "tfvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "tfsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}


# create public IPs
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "TerraformDemo"
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
}

# create a random id
resource "random_id" "randomId" {
  byte_length = 4
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "tfstorage${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
```

## <a name="next-steps"></a>Következő lépések
Alapvető infrastruktúra az Azure-ban hozott létre a Terraform használatával. Összetettebb forgatókönyveket, beleértve a példák, amelyek használatát egy terheléselosztó és a virtuális gép skálázása beállítása, lásd: számos [Terraform példák az Azure-](https://github.com/hashicorp/terraform/tree/master/examples). Támogatott Azure szolgáltatók naprakész listáját, tekintse meg a [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).
