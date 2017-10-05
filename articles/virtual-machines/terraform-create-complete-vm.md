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
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="5e629-103">Alapvető infrastruktúra létrehozása az Azure-ban Terraform használatával</span><span class="sxs-lookup"><span data-stu-id="5e629-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="5e629-104">Ez a cikk ismerteti a lépéseket kell tennie az Azure virtuális gép, és az alapjául szolgáló infrastruktúra kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5e629-104">This article describes the steps you need to take to provision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="5e629-105">Megtudhatja, hogyan Terraform parancsfájlokat írhat és hogyan megjelenítéséhez a módosításokat, mielőtt azokat a felhőalapú infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="5e629-105">You will learn how to write Terraform scripts and how to visualize the changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="5e629-106">Azt is megtudhatja, hogyan hozhat létre az infrastruktúra az Azure-ban Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="5e629-106">You also will learn how to create infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="5e629-107">Első lépésként hozzon létre egy \terraform_azure101.tf nevű választott (Visual Studio Code/Sublime/Vim/stb.) a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="5e629-107">To get started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="5e629-108">A fájl a pontos név nem fontos, mert Terraform fogad paraméterként a mappa neve: a mappában található összes parancsfájl azonnal végrehajtásra kerülhetnek.</span><span class="sxs-lookup"><span data-stu-id="5e629-108">The exact name of the file isn't important because Terraform accepts the folder name as a parameter: all scripts in the folder get executed.</span></span> <span data-ttu-id="5e629-109">Illessze be a következő kódot az új fájlban:</span><span class="sxs-lookup"><span data-stu-id="5e629-109">Paste the following code in the new file:</span></span>

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
<span data-ttu-id="5e629-110">Az a `provider` szakasz parancsfájl, a parancsfájl egy Azure szolgáltató rendelkezés erőforrások használatának Terraform biztosítják.</span><span class="sxs-lookup"><span data-stu-id="5e629-110">In the `provider` section of the script, you tell Terraform to use an Azure provider to provision resources in the script.</span></span> <span data-ttu-id="5e629-111">Értékek ELŐFIZETÉS_AZONOSÍTÓJA, appId, jelszó és tenant_id, megtekinteni a [telepítése és konfigurálása Terraform](terraform-install-configure.md) útmutató.</span><span class="sxs-lookup"><span data-stu-id="5e629-111">To get values for subscription_id, appId, password, and tenant_id, see the [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="5e629-112">Ha a blokk környezeti változók értékek hozott létre, nem kell is.</span><span class="sxs-lookup"><span data-stu-id="5e629-112">If you have created environment variables for the values in this block, you don't need to include it.</span></span> 

<span data-ttu-id="5e629-113">A `azurerm_resource_group` erőforrás utasítja Terraform egy új erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5e629-113">The `azurerm_resource_group` resource instructs Terraform to create a new resource group.</span></span> <span data-ttu-id="5e629-114">A cikk későbbi részében Terraform elérhető további erőforrástípusok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="5e629-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-the-script"></a><span data-ttu-id="5e629-115">Futtassa a parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="5e629-115">Execute the script</span></span>
<span data-ttu-id="5e629-116">A parancsfájl mentése után Kilépés a konzol/parancssorba, és írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="5e629-116">After you save the script, exit to the console/command line, and type the following:</span></span>
```
terraform init
```
<span data-ttu-id="5e629-117">az Azure-Terraform szolgáltató inicializálása.</span><span class="sxs-lookup"><span data-stu-id="5e629-117">to initialize Terraform provider for Azure.</span></span> <span data-ttu-id="5e629-118">Írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="5e629-118">Then type the following:</span></span>
```
terraform plan terraformscripts
```
<span data-ttu-id="5e629-119">Feltételezzük, hogy `terraformscripts` az a mappa, ahol a parancsfájl mentve van.</span><span class="sxs-lookup"><span data-stu-id="5e629-119">We assume that `terraformscripts` is the folder where the script was saved.</span></span> <span data-ttu-id="5e629-120">Használhatja a `plan` Terraform parancsot, amely ellenőrzi, hogy az erőforrásokat, a parancsfájlok definiált.</span><span class="sxs-lookup"><span data-stu-id="5e629-120">We used the `plan` Terraform command, which looks at the resources defined in the scripts.</span></span> <span data-ttu-id="5e629-121">Összehasonlítja azokat az állapotadatokat Terraform mentette, és majd exportálja a tervezett végrehajtási _nélkül_ ténylegesen létrehozása az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="5e629-121">It compares them to the state information saved by Terraform and then outputs the planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="5e629-122">Az előző parancs végrehajtása után kell megjelennie a következő képernyő hasonlót:</span><span class="sxs-lookup"><span data-stu-id="5e629-122">After you execute the previous command, you should see something like the following screen:</span></span>

![Terraform terv](linux/media/terraform/tf_plan2.png)

<span data-ttu-id="5e629-124">Ha minden helyes, kiépítése az új erőforráscsoportot az Azure-ban a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="5e629-124">If everything looks correct, provision this new resource group in Azure by executing the following:</span></span> 
```
terraform apply terraformscripts
```
<span data-ttu-id="5e629-125">Az Azure portálon kell megjelennie a nevű új üres erőforráscsoport `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="5e629-125">In the Azure portal, you should see the new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="5e629-126">A következő szakaszban hozzáadhat egy virtuális gép és a támogató infrastruktúra, a virtuális gép az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5e629-126">In the following section, you add a virtual machine and all the supporting infrastructure for that virtual machine to the resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="5e629-127">A Terraform Ubuntu virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="5e629-127">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="5e629-128">Most kiterjesztése a Terraform parancsfájlt futtató Ubuntu virtuális gép kiépítéséhez szükséges a adatokkal létrehoztunk Önnek.</span><span class="sxs-lookup"><span data-stu-id="5e629-128">Let's extend the Terraform script we've created with the details that are necessary to provision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="5e629-129">Az erőforrásokat, amelyek a következő szakaszokban kiépítése a következők:</span><span class="sxs-lookup"><span data-stu-id="5e629-129">The resources that you provision in the following sections are:</span></span>

* <span data-ttu-id="5e629-130">Egyetlen alhálózattal rendelkező hálózat</span><span class="sxs-lookup"><span data-stu-id="5e629-130">A network with a single subnet</span></span>
* <span data-ttu-id="5e629-131">A hálózati kártya</span><span class="sxs-lookup"><span data-stu-id="5e629-131">A network interface card</span></span> 
* <span data-ttu-id="5e629-132">A tárfiók egy tároló</span><span class="sxs-lookup"><span data-stu-id="5e629-132">A storage account with a storage container</span></span>
* <span data-ttu-id="5e629-133">Egy nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="5e629-133">A public IP</span></span>
* <span data-ttu-id="5e629-134">A virtuális gép, amely használja az előző erőforrások</span><span class="sxs-lookup"><span data-stu-id="5e629-134">A virtual machine that utilizes all the previous resources</span></span> 

<span data-ttu-id="5e629-135">Az egyes Azure Terraform erőforrások alapos dokumentációjáért lásd: a [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="5e629-135">For thorough documentation for each of the Azure Terraform resources, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="5e629-136">A teljes verzióját a [telepítési parancsfájlban](#complete-terraform-script) is biztosított kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="5e629-136">The full version of the [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-the-terraform-script"></a><span data-ttu-id="5e629-137">A Terraform parancsfájlt kiterjeszteni</span><span class="sxs-lookup"><span data-stu-id="5e629-137">Extend the Terraform script</span></span>
<span data-ttu-id="5e629-138">A következő erőforrások hoztak létre a parancsfájl kiterjesztése:</span><span class="sxs-lookup"><span data-stu-id="5e629-138">Extend the script that was created with the following resources:</span></span> 
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
<span data-ttu-id="5e629-139">A korábbi parancsfájlja létrehozza a virtuális hálózat és egy alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="5e629-139">The previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="5e629-140">Jegyezze fel az "${azurerm_resource_group.helloterraform.name}" a virtuális hálózat és az alhálózati definíció keresztül már létrehozott erőforráscsoport mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="5e629-140">Note the reference to the resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both the virtual network and the subnet definition.</span></span>

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
<span data-ttu-id="5e629-141">Az előző parancsfájl kódtöredékek hozzon létre egy nyilvános IP-cím, és a hálózati adaptert, amely a nyilvános IP-címet használja létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5e629-141">The previous script snippets create a public IP and a network interface that makes use of the public IP created.</span></span> <span data-ttu-id="5e629-142">Vegye figyelembe a subnet_id és public_ip_address_id mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="5e629-142">Note the references to subnet_id and public_ip_address_id.</span></span> <span data-ttu-id="5e629-143">Terraform megérteni, hogy rendelkezik-e a hálózati adapter egy függőséget az erőforrásokat, amelyek a hálózati illesztő létrehozása előtt kell létrehozni a beépített eszközintelligencia rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5e629-143">Terraform has built-in intelligence to understand that the network interface has a dependency on the resources that need to be created before the creation of the network interface.</span></span>

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
<span data-ttu-id="5e629-144">A tárfiók létrehozott itt, és meghatározott egy tárolót a tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="5e629-144">Here, you created a storage account and defined a storage container within that storage account.</span></span> <span data-ttu-id="5e629-145">Ez a tárfiók a virtuális merevlemezek (VHD) tárolására kell készíteni a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="5e629-145">This storage account is where you store virtual hard disks (VHDs) for the virtual machine about to be created.</span></span>

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
<span data-ttu-id="5e629-146">Végül az előző részlet hoz létre egy virtuális gép, amely már kiépített összes erőforrást használja.</span><span class="sxs-lookup"><span data-stu-id="5e629-146">Finally, the previous snippet creates a virtual machine that utilizes all the resources provisioned already.</span></span> <span data-ttu-id="5e629-147">Egy tárfiók és tároló virtuális merevlemezek, a nyilvános IP- és, a megadott alhálózat egy hálózati adapter, és az erőforráscsoport hozott létre.</span><span class="sxs-lookup"><span data-stu-id="5e629-147">They are a storage account and container for a VHD, a network interface with public IP and subnet specified, and the resource group you already created.</span></span> <span data-ttu-id="5e629-148">Vegye figyelembe a vm_size tulajdonságot, ha a parancsfájl meghatározza egy Azure A0 Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="5e629-148">Note the vm_size property, where the script specifies an Azure A0 SKU.</span></span>

### <a name="execute-the-script"></a><span data-ttu-id="5e629-149">Futtassa a parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="5e629-149">Execute the script</span></span>
<span data-ttu-id="5e629-150">A teljes parancsfájl mentése, a kilépéshez a konzol/parancssorba, és írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="5e629-150">With the full script saved, exit to the console/command line and type the following:</span></span>
```
terraform apply terraformscripts
```
<span data-ttu-id="5e629-151">Némi várakozás után az erőforrások, például egy virtuális gép megjelennek a `terraformtest` erőforráscsoportot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="5e629-151">After some time, the resources, including a virtual machine, appear in the `terraformtest` resource group in the Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="5e629-152">Teljes Terraform parancsfájl</span><span class="sxs-lookup"><span data-stu-id="5e629-152">Complete Terraform script</span></span>

<span data-ttu-id="5e629-153">Az Ön kényelme érdekében alatt van a teljes Terraform parancsfájlt, hogy az infrastruktúra minden cikkben említett rendelkezések.</span><span class="sxs-lookup"><span data-stu-id="5e629-153">For your convenience, below is the complete Terraform script that provisions all of the infrastructure discussed in this article.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5e629-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e629-154">Next steps</span></span>
<span data-ttu-id="5e629-155">Alapvető infrastruktúra az Azure-ban hozott létre a Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="5e629-155">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="5e629-156">Összetettebb forgatókönyveket, beleértve a példák, amelyek használatát egy terheléselosztó és a virtuális gép skálázása beállítása, lásd: számos [Terraform példák az Azure-](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="5e629-156">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="5e629-157">Támogatott Azure szolgáltatók naprakész listáját, tekintse meg a [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="5e629-157">For an up-to-date list of supported Azure providers, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
