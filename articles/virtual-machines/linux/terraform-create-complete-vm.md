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
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="a3784-103">Alapvető infrastruktúra létrehozása az Azure-ban Terraform használatával</span><span class="sxs-lookup"><span data-stu-id="a3784-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="a3784-104">Ez a cikk az Azure virtuális gép, alapul szolgáló infrastruktúra együtt tootake tooprovision kell hello lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a3784-104">This article describes hello steps you need tootake tooprovision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="a3784-105">Megtudhatja, hogyan toowrite Terraform parancsfájlok és hogyan toovisualize hello előtt változik, hogy azokat a felhőalapú infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="a3784-105">You will learn how toowrite Terraform scripts and how toovisualize hello changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="a3784-106">Azt is megtudhatja, hogyan toocreate infrastruktúra az Azure-ban Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="a3784-106">You also will learn how toocreate infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="a3784-107">tooget elindult, választott (Visual Studio Code/Sublime/Vim/stb.) a szövegszerkesztőben \terraform_azure101.tf nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a3784-107">tooget started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="a3784-108">hello pontos hello fájl neve nem fontos, mert Terraform fogad paraméterként hello mappanév: hello mappában található összes parancsfájl azonnal végrehajtásra kerülhetnek.</span><span class="sxs-lookup"><span data-stu-id="a3784-108">hello exact name of hello file isn't important because Terraform accepts hello folder name as a parameter: all scripts in hello folder get executed.</span></span> <span data-ttu-id="a3784-109">Illessze be a következő hello új fájl hello:</span><span class="sxs-lookup"><span data-stu-id="a3784-109">Paste hello following code in hello new file:</span></span>

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
<span data-ttu-id="a3784-110">A hello `provider` hello szakasza parancsfájl közli Terraform toouse egy Azure szolgáltató tooprovision erőforrások hello parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="a3784-110">In hello `provider` section of hello script, you tell Terraform toouse an Azure provider tooprovision resources in hello script.</span></span> <span data-ttu-id="a3784-111">tooget értékek ELŐFIZETÉS_AZONOSÍTÓJA, appId, jelszó és tenant_id, lásd: hello [telepítse és konfigurálja a Terraform](terraform-install-configure.md) útmutató.</span><span class="sxs-lookup"><span data-stu-id="a3784-111">tooget values for subscription_id, appId, password, and tenant_id, see hello [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="a3784-112">Ha a blokk környezeti változók hello értékek hozott létre, nem kell tooinclude azt.</span><span class="sxs-lookup"><span data-stu-id="a3784-112">If you have created environment variables for hello values in this block, you don't need tooinclude it.</span></span> 

<span data-ttu-id="a3784-113">Hello `azurerm_resource_group` erőforrás utasítja Terraform toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a3784-113">hello `azurerm_resource_group` resource instructs Terraform toocreate a new resource group.</span></span> <span data-ttu-id="a3784-114">A cikk későbbi részében Terraform elérhető további erőforrástípusok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="a3784-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-hello-script"></a><span data-ttu-id="a3784-115">Hello parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="a3784-115">Execute hello script</span></span>
<span data-ttu-id="a3784-116">Hello parancsfájl mentése után toohello konzol/parancssorból való kilépéshez, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a3784-116">After you save hello script, exit toohello console/command line, and type hello following:</span></span>
```
terraform init
```
 <span data-ttu-id="a3784-117">tooinitialize hello Terraform szolgáltató az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="a3784-117">tooinitialize hello Terraform provider for Azure.</span></span> <span data-ttu-id="a3784-118">Majd módosítsa a hello túl**terraformscripts**, és probléma hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a3784-118">Then change hello directory too**terraformscripts**, and issue hello following command:</span></span>
```
terraform plan
```
<span data-ttu-id="a3784-119">Hello használtuk `plan` Terraform parancsot, amely ellenőrzi, hogy az hello parancsfájlok definiált hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a3784-119">We used hello `plan` Terraform command, which looks at hello resources defined in hello scripts.</span></span> <span data-ttu-id="a3784-120">Terraform által mentett toohello állapotadatokat összehasonlítja őket, és majd kimenetek hello tervezett végrehajtási _nélkül_ ténylegesen létrehozása az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="a3784-120">It compares them toohello state information saved by Terraform and then outputs hello planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="a3784-121">Hello előző parancs végrehajtása után kell megjelennie a következő képernyő hello hasonlót:</span><span class="sxs-lookup"><span data-stu-id="a3784-121">After you execute hello previous command, you should see something like hello following screen:</span></span>

![Terraform terv](./media/terraform/tf_plan2.png)

<span data-ttu-id="a3784-123">Ha minden megfelelő, az új erőforráscsoportot az Azure-ban kiépítése a hello alábbiak végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="a3784-123">If everything looks correct, provision this new resource group in Azure by executing hello following:</span></span> 
```
terraform apply
```
<span data-ttu-id="a3784-124">Hello Azure-portálon, a kell megjelennie hello üres nevű új erőforráscsoport `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="a3784-124">In hello Azure portal, you should see hello new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="a3784-125">A következő szakasz hello hozzáadhat egy virtuális gépet és annak minden virtuális gép toohello erőforráscsoporthoz támogató infrastruktúra hello.</span><span class="sxs-lookup"><span data-stu-id="a3784-125">In hello following section, you add a virtual machine and all hello supporting infrastructure for that virtual machine toohello resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="a3784-126">A Terraform Ubuntu virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="a3784-126">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="a3784-127">Létrehoztunk Önnek hello adatokkal, amelyek a szükséges tooprovision hello Terraform parancsfájl most kiterjesztése futtató Ubuntu virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a3784-127">Let's extend hello Terraform script we've created with hello details that are necessary tooprovision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="a3784-128">a következő részekben hello kiépítése hello erőforrásokat a következők:</span><span class="sxs-lookup"><span data-stu-id="a3784-128">hello resources that you provision in hello following sections are:</span></span>

* <span data-ttu-id="a3784-129">Egyetlen alhálózattal rendelkező hálózat</span><span class="sxs-lookup"><span data-stu-id="a3784-129">A network with a single subnet</span></span>
* <span data-ttu-id="a3784-130">A hálózati kártya</span><span class="sxs-lookup"><span data-stu-id="a3784-130">A network interface card</span></span> 
* <span data-ttu-id="a3784-131">Hello virtuális gép diagnosztikai tárfiók</span><span class="sxs-lookup"><span data-stu-id="a3784-131">A storage account for hello virtual machine diagnostics</span></span>
* <span data-ttu-id="a3784-132">Egy nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="a3784-132">A public IP</span></span>
* <span data-ttu-id="a3784-133">A virtuális gép, amely használja az összes hello előző erőforrások</span><span class="sxs-lookup"><span data-stu-id="a3784-133">A virtual machine that utilizes all hello previous resources</span></span> 

<span data-ttu-id="a3784-134">Az egyes hello Azure Terraform erőforrások alapos dokumentációjáért lásd: hello [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="a3784-134">For thorough documentation for each of hello Azure Terraform resources, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="a3784-135">hello teljes verzióját hello [telepítési parancsfájlban](#complete-terraform-script) is biztosított kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="a3784-135">hello full version of hello [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-hello-terraform-script"></a><span data-ttu-id="a3784-136">Hello Terraform parancsfájl kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="a3784-136">Extend hello Terraform script</span></span>
<span data-ttu-id="a3784-137">A következő erőforrások hello hoztak létre hello parancsfájl kiterjesztése:</span><span class="sxs-lookup"><span data-stu-id="a3784-137">Extend hello script that was created with hello following resources:</span></span> 
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
<span data-ttu-id="a3784-138">hello előző parancsfájlja létrehozza a virtuális hálózat és egy alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="a3784-138">hello previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="a3784-139">Vegye figyelembe a hello hivatkozás toohello erőforráscsoport már hozott létre "${azurerm_resource_group.helloterraform.name}" hello virtuális hálózati és alhálózati definíció hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="a3784-139">Note hello reference toohello resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both hello virtual network and hello subnet definition.</span></span>

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
<span data-ttu-id="a3784-140">hello előző parancsfájl kódtöredékek hozzon létre egy nyilvános IP-cím és a hálózati adaptert, amely használja a hello nyilvános IP-cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a3784-140">hello previous script snippets create a public IP and a network interface that makes use of hello public IP created.</span></span> <span data-ttu-id="a3784-141">Megjegyzés: hello toosubnet_id és public_ip_address_id hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a3784-141">Note hello references toosubnet_id and public_ip_address_id.</span></span> <span data-ttu-id="a3784-142">Terraform rendelkezik beépített funkciói toounderstand adott hello hálózati illesztőnek egy függőséget hello erőforrástól függ, hogy hello hello hálózati illesztő létrehozása előtt létre kell toobe.</span><span class="sxs-lookup"><span data-stu-id="a3784-142">Terraform has built-in intelligence toounderstand that hello network interface has a dependency on hello resources that need toobe created before hello creation of hello network interface.</span></span>

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
<span data-ttu-id="a3784-143">Itt létrehozott egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="a3784-143">Here, you created a storage account.</span></span> <span data-ttu-id="a3784-144">Ez a tárfiók, ahol hello virtuális gép fogja tárolni a diagnosztikai részletek.</span><span class="sxs-lookup"><span data-stu-id="a3784-144">This storage account is where hello virtual machine will store its diagnostics details.</span></span>

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
<span data-ttu-id="a3784-145">Végezetül hello előző részlet hoz létre egy virtuális gép, amely már kiosztott összes hello erőforrásokat használja.</span><span class="sxs-lookup"><span data-stu-id="a3784-145">Finally, hello previous snippet creates a virtual machine that utilizes all hello resources provisioned already.</span></span> <span data-ttu-id="a3784-146">A tárfiók hello virtuálisgép-diagnosztika, a hálózati illesztő – nyilvános IP- és a megadott alhálózat, és hello erőforráscsoportban már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a3784-146">They are a storage account for hello virtual machine diagnostics, a network interface with public IP and subnet specified, and hello resource group you already created.</span></span> <span data-ttu-id="a3784-147">Vegye figyelembe a hello vm_size tulajdonság, ahol hello parancsfájl határozza meg az Azure Standard DS1v2 Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="a3784-147">Note hello vm_size property, where hello script specifies an Azure Standard DS1v2 SKU.</span></span>

<span data-ttu-id="a3784-148">Nyilvános SSH-kulcs szükséges toosupply áll.</span><span class="sxs-lookup"><span data-stu-id="a3784-148">You are required toosupply an SSH public key.</span></span> <span data-ttu-id="a3784-149">Nyilvános kulcs értéke hello helyezzen hello szakasz **... HELYEZZE BE A PROTOKOLL OPENSSH ITT A NYILVÁNOS KULCSOT...**  felett.</span><span class="sxs-lookup"><span data-stu-id="a3784-149">Place hello public key value into hello section **... INSERT OPENSSH PUBLIC KEY HERE ...** above.</span></span> <span data-ttu-id="a3784-150">Használhat egy meglévő ssh kulcs pár, vagy hajtsa végre a hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) vagy [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) dokumentáció toogenerate hello kulcsból álló kulcspárt.</span><span class="sxs-lookup"><span data-stu-id="a3784-150">You can use an existing ssh key pair or follow hello [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) or [Linux/macOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) documentation toogenerate hello key pair.</span></span>

### <a name="execute-hello-script"></a><span data-ttu-id="a3784-151">Hello parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="a3784-151">Execute hello script</span></span>
<span data-ttu-id="a3784-152">Hello teljes mentett, parancsfájl toohello konzol/parancssorból való kilépéshez, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a3784-152">With hello full script saved, exit toohello console/command line and type hello following:</span></span>
```
terraform apply
```
<span data-ttu-id="a3784-153">Némi várakozás után hello erőforrások, például egy virtuális gép jelennek meg hello `terraformtest` erőforráscsoportja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a3784-153">After some time, hello resources, including a virtual machine, appear in hello `terraformtest` resource group in hello Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="a3784-154">Teljes Terraform parancsfájl</span><span class="sxs-lookup"><span data-stu-id="a3784-154">Complete Terraform script</span></span>

<span data-ttu-id="a3784-155">Az Ön kényelme érdekében alatt van hello teljes Terraform parancsfájlt, hogy az összes hello infrastruktúra cikkben említett rendelkezések.</span><span class="sxs-lookup"><span data-stu-id="a3784-155">For your convenience, below is hello complete Terraform script that provisions all of hello infrastructure discussed in this article.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a3784-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3784-156">Next steps</span></span>
<span data-ttu-id="a3784-157">Alapvető infrastruktúra az Azure-ban hozott létre a Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="a3784-157">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="a3784-158">Összetettebb forgatókönyveket, beleértve a példák, amelyek használatát egy terheléselosztó és a virtuális gép skálázása beállítása, lásd: számos [Terraform példák az Azure-](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="a3784-158">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="a3784-159">Támogatott Azure szolgáltatók naprakész listáját, lásd: hello [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="a3784-159">For an up-to-date list of supported Azure providers, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
