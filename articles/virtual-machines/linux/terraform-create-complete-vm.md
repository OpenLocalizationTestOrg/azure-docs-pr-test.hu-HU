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
ms.openlocfilehash: aa0926de32dd85f4460bbfa84b7a6007e2199d51
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="abab9-103">Alapvető infrastruktúra létrehozása az Azure-ban Terraform használatával</span><span class="sxs-lookup"><span data-stu-id="abab9-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="abab9-104">Ez a cikk ismerteti a lépéseket kell tennie az Azure virtuális gép, és az alapjául szolgáló infrastruktúra kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="abab9-104">This article describes the steps you need to take to provision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="abab9-105">Megtudhatja, hogyan Terraform parancsfájlokat írhat és hogyan megjelenítéséhez a módosításokat, mielőtt azokat a felhőalapú infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="abab9-105">You will learn how to write Terraform scripts and how to visualize the changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="abab9-106">Azt is megtudhatja, hogyan hozhat létre az infrastruktúra az Azure-ban Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="abab9-106">You also will learn how to create infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="abab9-107">Első lépésként hozzon létre egy \terraform_azure101.tf nevű választott (Visual Studio Code/Sublime/Vim/stb.) a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="abab9-107">To get started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="abab9-108">A fájl a pontos név nem fontos, mert Terraform fogad paraméterként a mappa neve: a mappában található összes parancsfájl azonnal végrehajtásra kerülhetnek.</span><span class="sxs-lookup"><span data-stu-id="abab9-108">The exact name of the file isn't important because Terraform accepts the folder name as a parameter: all scripts in the folder get executed.</span></span> <span data-ttu-id="abab9-109">Illessze be a következő kódot az új fájlban:</span><span class="sxs-lookup"><span data-stu-id="abab9-109">Paste the following code in the new file:</span></span>

~~~~
# Configure the Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have to include this block
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
<span data-ttu-id="abab9-110">Az a `provider` szakasz parancsfájl, a parancsfájl egy Azure szolgáltató rendelkezés erőforrások használatának Terraform biztosítják.</span><span class="sxs-lookup"><span data-stu-id="abab9-110">In the `provider` section of the script, you tell Terraform to use an Azure provider to provision resources in the script.</span></span> <span data-ttu-id="abab9-111">Értékek ELŐFIZETÉS_AZONOSÍTÓJA, appId, jelszó és tenant_id, megtekinteni a [telepítése és konfigurálása Terraform](terraform-install-configure.md) útmutató.</span><span class="sxs-lookup"><span data-stu-id="abab9-111">To get values for subscription_id, appId, password, and tenant_id, see the [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="abab9-112">Ha a blokk környezeti változók értékek hozott létre, nem kell is.</span><span class="sxs-lookup"><span data-stu-id="abab9-112">If you have created environment variables for the values in this block, you don't need to include it.</span></span> 

<span data-ttu-id="abab9-113">A `azurerm_resource_group` erőforrás utasítja Terraform egy új erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="abab9-113">The `azurerm_resource_group` resource instructs Terraform to create a new resource group.</span></span> <span data-ttu-id="abab9-114">A cikk későbbi részében Terraform elérhető további erőforrástípusok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="abab9-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-the-script"></a><span data-ttu-id="abab9-115">Futtassa a parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="abab9-115">Execute the script</span></span>
<span data-ttu-id="abab9-116">A parancsfájl mentése után Kilépés a konzol/parancssorba, és írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="abab9-116">After you save the script, exit to the console/command line, and type the following:</span></span>
```
terraform init
```
 <span data-ttu-id="abab9-117">az Azure-Terraform szolgáltató inicializálása.</span><span class="sxs-lookup"><span data-stu-id="abab9-117">to initialize the Terraform provider for Azure.</span></span> <span data-ttu-id="abab9-118">Ezután lépjen be **terraformscripts**, és adja ki a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="abab9-118">Then change the directory to **terraformscripts**, and issue the following command:</span></span>
```
terraform plan
```
<span data-ttu-id="abab9-119">Használhatja a `plan` Terraform parancsot, amely ellenőrzi, hogy az erőforrásokat, a parancsfájlok definiált.</span><span class="sxs-lookup"><span data-stu-id="abab9-119">We used the `plan` Terraform command, which looks at the resources defined in the scripts.</span></span> <span data-ttu-id="abab9-120">Összehasonlítja azokat az állapotadatokat Terraform mentette, és majd exportálja a tervezett végrehajtási _nélkül_ ténylegesen létrehozása az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="abab9-120">It compares them to the state information saved by Terraform and then outputs the planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="abab9-121">Az előző parancs végrehajtása után kell megjelennie a következő képernyő hasonlót:</span><span class="sxs-lookup"><span data-stu-id="abab9-121">After you execute the previous command, you should see something like the following screen:</span></span>

![Terraform terv](./media/terraform/tf_plan2.png)

<span data-ttu-id="abab9-123">Ha minden helyes, kiépítése az új erőforráscsoportot az Azure-ban a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="abab9-123">If everything looks correct, provision this new resource group in Azure by executing the following:</span></span> 
```
terraform apply
```
<span data-ttu-id="abab9-124">Az Azure portálon kell megjelennie a nevű új üres erőforráscsoport `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="abab9-124">In the Azure portal, you should see the new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="abab9-125">A következő szakaszban hozzáadhat egy virtuális gép és a támogató infrastruktúra, a virtuális gép az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="abab9-125">In the following section, you add a virtual machine and all the supporting infrastructure for that virtual machine to the resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="abab9-126">A Terraform Ubuntu virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="abab9-126">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="abab9-127">Most kiterjesztése a Terraform parancsfájlt futtató Ubuntu virtuális gép kiépítéséhez szükséges a adatokkal létrehoztunk Önnek.</span><span class="sxs-lookup"><span data-stu-id="abab9-127">Let's extend the Terraform script we've created with the details that are necessary to provision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="abab9-128">Az erőforrásokat, amelyek a következő szakaszokban kiépítése a következők:</span><span class="sxs-lookup"><span data-stu-id="abab9-128">The resources that you provision in the following sections are:</span></span>

* <span data-ttu-id="abab9-129">Egyetlen alhálózattal rendelkező hálózat</span><span class="sxs-lookup"><span data-stu-id="abab9-129">A network with a single subnet</span></span>
* <span data-ttu-id="abab9-130">A hálózati kártya</span><span class="sxs-lookup"><span data-stu-id="abab9-130">A network interface card</span></span> 
* <span data-ttu-id="abab9-131">A virtuális gép diagnosztikai tárfiók</span><span class="sxs-lookup"><span data-stu-id="abab9-131">A storage account for the virtual machine diagnostics</span></span>
* <span data-ttu-id="abab9-132">Egy nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="abab9-132">A public IP</span></span>
* <span data-ttu-id="abab9-133">A virtuális gép, amely használja az előző erőforrások</span><span class="sxs-lookup"><span data-stu-id="abab9-133">A virtual machine that utilizes all the previous resources</span></span> 

<span data-ttu-id="abab9-134">Az egyes Azure Terraform erőforrások alapos dokumentációjáért lásd: a [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="abab9-134">For thorough documentation for each of the Azure Terraform resources, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="abab9-135">A teljes verzióját a [telepítési parancsfájlban](#complete-terraform-script) is biztosított kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="abab9-135">The full version of the [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-the-terraform-script"></a><span data-ttu-id="abab9-136">A Terraform parancsfájlt kiterjeszteni</span><span class="sxs-lookup"><span data-stu-id="abab9-136">Extend the Terraform script</span></span>
<span data-ttu-id="abab9-137">A következő erőforrások hoztak létre a parancsfájl kiterjesztése:</span><span class="sxs-lookup"><span data-stu-id="abab9-137">Extend the script that was created with the following resources:</span></span> 
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
<span data-ttu-id="abab9-138">A korábbi parancsfájlja létrehozza a virtuális hálózat és egy alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="abab9-138">The previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="abab9-139">Jegyezze fel az "${azurerm_resource_group.helloterraform.name}" a virtuális hálózat és az alhálózati definíció keresztül már létrehozott erőforráscsoport mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="abab9-139">Note the reference to the resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both the virtual network and the subnet definition.</span></span>

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
<span data-ttu-id="abab9-140">Az előző parancsfájl kódtöredékek hozzon létre egy nyilvános IP-cím, és a hálózati adaptert, amely a nyilvános IP-címet használja létrehozni.</span><span class="sxs-lookup"><span data-stu-id="abab9-140">The previous script snippets create a public IP and a network interface that makes use of the public IP created.</span></span> <span data-ttu-id="abab9-141">Vegye figyelembe a subnet_id és public_ip_address_id mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="abab9-141">Note the references to subnet_id and public_ip_address_id.</span></span> <span data-ttu-id="abab9-142">Terraform megérteni, hogy rendelkezik-e a hálózati adapter egy függőséget az erőforrásokat, amelyek a hálózati illesztő létrehozása előtt kell létrehozni a beépített eszközintelligencia rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="abab9-142">Terraform has built-in intelligence to understand that the network interface has a dependency on the resources that need to be created before the creation of the network interface.</span></span>

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
<span data-ttu-id="abab9-143">Itt létrehozott egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="abab9-143">Here, you created a storage account.</span></span> <span data-ttu-id="abab9-144">Ez a tárfiók, ahol a virtuális gép a diagnosztikai részletek fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="abab9-144">This storage account is where the virtual machine will store its diagnostics details.</span></span>

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
<span data-ttu-id="abab9-145">Végül az előző részlet hoz létre egy virtuális gép, amely már kiépített összes erőforrást használja.</span><span class="sxs-lookup"><span data-stu-id="abab9-145">Finally, the previous snippet creates a virtual machine that utilizes all the resources provisioned already.</span></span> <span data-ttu-id="abab9-146">-E a virtuálisgép-diagnosztika, a hálózati illesztő – nyilvános IP-cím és alhálózat a megadott tárfiók, és az erőforráscsoport hozott létre.</span><span class="sxs-lookup"><span data-stu-id="abab9-146">They are a storage account for the virtual machine diagnostics, a network interface with public IP and subnet specified, and the resource group you already created.</span></span> <span data-ttu-id="abab9-147">Vegye figyelembe a vm_size tulajdonságot, ha a parancsfájl meghatározza egy Azure Standard DS1v2 Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="abab9-147">Note the vm_size property, where the script specifies an Azure Standard DS1v2 SKU.</span></span>

<span data-ttu-id="abab9-148">Meg kell adnia a nyilvános SSH-kulcs.</span><span class="sxs-lookup"><span data-stu-id="abab9-148">You are required to supply an SSH public key.</span></span> <span data-ttu-id="abab9-149">Helyezze a nyilvános kulcs értékét a szakasz **... HELYEZZE BE A PROTOKOLL OPENSSH ITT A NYILVÁNOS KULCSOT...**  felett.</span><span class="sxs-lookup"><span data-stu-id="abab9-149">Place the public key value into the section **... INSERT OPENSSH PUBLIC KEY HERE ...** above.</span></span> <span data-ttu-id="abab9-150">Használhat egy meglévő ssh kulcs párba, vagy hajtsa végre a [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) vagy [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) létrehozni a kulcspárt dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="abab9-150">You can use an existing ssh key pair or follow the [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) or [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) documentation to generate the key pair.</span></span>

### <a name="execute-the-script"></a><span data-ttu-id="abab9-151">Futtassa a parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="abab9-151">Execute the script</span></span>
<span data-ttu-id="abab9-152">A teljes parancsfájl mentése, a kilépéshez a konzol/parancssorba, és írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="abab9-152">With the full script saved, exit to the console/command line and type the following:</span></span>
```
terraform apply
```
<span data-ttu-id="abab9-153">Némi várakozás után az erőforrások, például egy virtuális gép megjelennek a `terraformtest` erőforráscsoportot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="abab9-153">After some time, the resources, including a virtual machine, appear in the `terraformtest` resource group in the Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="abab9-154">Teljes Terraform parancsfájl</span><span class="sxs-lookup"><span data-stu-id="abab9-154">Complete Terraform script</span></span>

<span data-ttu-id="abab9-155">Az Ön kényelme érdekében alatt van a teljes Terraform parancsfájlt, hogy az infrastruktúra minden cikkben említett rendelkezések.</span><span class="sxs-lookup"><span data-stu-id="abab9-155">For your convenience, below is the complete Terraform script that provisions all of the infrastructure discussed in this article.</span></span>

```
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

## <a name="next-steps"></a><span data-ttu-id="abab9-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="abab9-156">Next steps</span></span>
<span data-ttu-id="abab9-157">Alapvető infrastruktúra az Azure-ban hozott létre a Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="abab9-157">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="abab9-158">Összetettebb forgatókönyveket, beleértve a példák, amelyek használatát egy terheléselosztó és a virtuális gép skálázása beállítása, lásd: számos [Terraform példák az Azure-](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="abab9-158">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="abab9-159">Támogatott Azure szolgáltatók naprakész listáját, tekintse meg a [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="abab9-159">For an up-to-date list of supported Azure providers, see the [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
