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
ms.openlocfilehash: 916a838c118f28b3fbd373188e0acb2afc655081
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a><span data-ttu-id="37180-103">Alapvető infrastruktúra létrehozása az Azure-ban Terraform használatával</span><span class="sxs-lookup"><span data-stu-id="37180-103">Create basic infrastructure in Azure by using Terraform</span></span>
<span data-ttu-id="37180-104">Ez a cikk az Azure virtuális gép, alapul szolgáló infrastruktúra együtt tootake tooprovision kell hello lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="37180-104">This article describes hello steps you need tootake tooprovision a virtual machine, together with underlying infrastructure, into Azure.</span></span> <span data-ttu-id="37180-105">Megtudhatja, hogyan toowrite Terraform parancsfájlok és hogyan toovisualize hello előtt változik, hogy azokat a felhőalapú infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="37180-105">You will learn how toowrite Terraform scripts and how toovisualize hello changes before you make them in your cloud infrastructure.</span></span> <span data-ttu-id="37180-106">Azt is megtudhatja, hogyan toocreate infrastruktúra az Azure-ban Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="37180-106">You also will learn how toocreate infrastructure in Azure by using Terraform.</span></span>

<span data-ttu-id="37180-107">tooget elindult, választott (Visual Studio Code/Sublime/Vim/stb.) a szövegszerkesztőben \terraform_azure101.tf nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37180-107">tooget started, create a file called \terraform_azure101.tf in your text editor of choice (Visual Studio Code/Sublime/Vim/etc.).</span></span> <span data-ttu-id="37180-108">hello pontos hello fájl neve nem fontos, mert Terraform fogad paraméterként hello mappanév: hello mappában található összes parancsfájl azonnal végrehajtásra kerülhetnek.</span><span class="sxs-lookup"><span data-stu-id="37180-108">hello exact name of hello file isn't important because Terraform accepts hello folder name as a parameter: all scripts in hello folder get executed.</span></span> <span data-ttu-id="37180-109">Illessze be a következő hello új fájl hello:</span><span class="sxs-lookup"><span data-stu-id="37180-109">Paste hello following code in hello new file:</span></span>

~~~~
# Configure hello Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have tooinclude this block
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
<span data-ttu-id="37180-110">A hello `provider` hello szakasza parancsfájl közli Terraform toouse egy Azure szolgáltató tooprovision erőforrások hello parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="37180-110">In hello `provider` section of hello script, you tell Terraform toouse an Azure provider tooprovision resources in hello script.</span></span> <span data-ttu-id="37180-111">tooget értékek ELŐFIZETÉS_AZONOSÍTÓJA, appId, jelszó és tenant_id, lásd: hello [telepítse és konfigurálja a Terraform](terraform-install-configure.md) útmutató.</span><span class="sxs-lookup"><span data-stu-id="37180-111">tooget values for subscription_id, appId, password, and tenant_id, see hello [Install and configure Terraform](terraform-install-configure.md) guide.</span></span> <span data-ttu-id="37180-112">Ha a blokk környezeti változók hello értékek hozott létre, nem kell tooinclude azt.</span><span class="sxs-lookup"><span data-stu-id="37180-112">If you have created environment variables for hello values in this block, you don't need tooinclude it.</span></span> 

<span data-ttu-id="37180-113">Hello `azurerm_resource_group` erőforrás utasítja Terraform toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="37180-113">hello `azurerm_resource_group` resource instructs Terraform toocreate a new resource group.</span></span> <span data-ttu-id="37180-114">A cikk későbbi részében Terraform elérhető további erőforrástípusok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="37180-114">You can see more resource types that are available in Terraform later in this article.</span></span>

## <a name="execute-hello-script"></a><span data-ttu-id="37180-115">Hello parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="37180-115">Execute hello script</span></span>
<span data-ttu-id="37180-116">Hello parancsfájl mentése után toohello konzol/parancssorból való kilépéshez, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="37180-116">After you save hello script, exit toohello console/command line, and type hello following:</span></span>
```
terraform init
```
<span data-ttu-id="37180-117">tooinitialize Terraform szolgáltató az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="37180-117">tooinitialize Terraform provider for Azure.</span></span> <span data-ttu-id="37180-118">Írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="37180-118">Then type hello following:</span></span>
```
terraform plan terraformscripts
```
<span data-ttu-id="37180-119">Feltételezzük, hogy `terraformscripts` hello mappa hello parancsfájl menteni.</span><span class="sxs-lookup"><span data-stu-id="37180-119">We assume that `terraformscripts` is hello folder where hello script was saved.</span></span> <span data-ttu-id="37180-120">Hello használtuk `plan` Terraform parancsot, amely ellenőrzi, hogy az hello parancsfájlok definiált hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="37180-120">We used hello `plan` Terraform command, which looks at hello resources defined in hello scripts.</span></span> <span data-ttu-id="37180-121">Terraform által mentett toohello állapotadatokat összehasonlítja őket, és majd kimenetek hello tervezett végrehajtási _nélkül_ ténylegesen létrehozása az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="37180-121">It compares them toohello state information saved by Terraform and then outputs hello planned execution _without_ actually creating resources in Azure.</span></span> 

<span data-ttu-id="37180-122">Hello előző parancs végrehajtása után kell megjelennie a következő képernyő hello hasonlót:</span><span class="sxs-lookup"><span data-stu-id="37180-122">After you execute hello previous command, you should see something like hello following screen:</span></span>

![Terraform terv](linux/media/terraform/tf_plan2.png)

<span data-ttu-id="37180-124">Ha minden megfelelő, az új erőforráscsoportot az Azure-ban kiépítése a hello alábbiak végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="37180-124">If everything looks correct, provision this new resource group in Azure by executing hello following:</span></span> 
```
terraform apply terraformscripts
```
<span data-ttu-id="37180-125">Hello Azure-portálon, a kell megjelennie hello üres nevű új erőforráscsoport `terraformtest`.</span><span class="sxs-lookup"><span data-stu-id="37180-125">In hello Azure portal, you should see hello new empty resource group called `terraformtest`.</span></span> <span data-ttu-id="37180-126">A következő szakasz hello hozzáadhat egy virtuális gépet és annak minden virtuális gép toohello erőforráscsoporthoz támogató infrastruktúra hello.</span><span class="sxs-lookup"><span data-stu-id="37180-126">In hello following section, you add a virtual machine and all hello supporting infrastructure for that virtual machine toohello resource group.</span></span>

## <a name="provision-an-ubuntu-vm-with-terraform"></a><span data-ttu-id="37180-127">A Terraform Ubuntu virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="37180-127">Provision an Ubuntu VM with Terraform</span></span>
<span data-ttu-id="37180-128">Létrehoztunk Önnek hello adatokkal, amelyek a szükséges tooprovision hello Terraform parancsfájl most kiterjesztése futtató Ubuntu virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="37180-128">Let's extend hello Terraform script we've created with hello details that are necessary tooprovision a virtual machine running Ubuntu.</span></span> <span data-ttu-id="37180-129">a következő részekben hello kiépítése hello erőforrásokat a következők:</span><span class="sxs-lookup"><span data-stu-id="37180-129">hello resources that you provision in hello following sections are:</span></span>

* <span data-ttu-id="37180-130">Egyetlen alhálózattal rendelkező hálózat</span><span class="sxs-lookup"><span data-stu-id="37180-130">A network with a single subnet</span></span>
* <span data-ttu-id="37180-131">A hálózati kártya</span><span class="sxs-lookup"><span data-stu-id="37180-131">A network interface card</span></span> 
* <span data-ttu-id="37180-132">A tárfiók egy tároló</span><span class="sxs-lookup"><span data-stu-id="37180-132">A storage account with a storage container</span></span>
* <span data-ttu-id="37180-133">Egy nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="37180-133">A public IP</span></span>
* <span data-ttu-id="37180-134">A virtuális gép, amely használja az összes hello előző erőforrások</span><span class="sxs-lookup"><span data-stu-id="37180-134">A virtual machine that utilizes all hello previous resources</span></span> 

<span data-ttu-id="37180-135">Az egyes hello Azure Terraform erőforrások alapos dokumentációjáért lásd: hello [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="37180-135">For thorough documentation for each of hello Azure Terraform resources, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>

<span data-ttu-id="37180-136">hello teljes verzióját hello [telepítési parancsfájlban](#complete-terraform-script) is biztosított kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="37180-136">hello full version of hello [provisioning script](#complete-terraform-script) is also provided for convenience.</span></span>

### <a name="extend-hello-terraform-script"></a><span data-ttu-id="37180-137">Hello Terraform parancsfájl kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="37180-137">Extend hello Terraform script</span></span>
<span data-ttu-id="37180-138">A következő erőforrások hello hoztak létre hello parancsfájl kiterjesztése:</span><span class="sxs-lookup"><span data-stu-id="37180-138">Extend hello script that was created with hello following resources:</span></span> 
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
<span data-ttu-id="37180-139">hello előző parancsfájlja létrehozza a virtuális hálózat és egy alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="37180-139">hello previous script creates a virtual network and a subnet within that virtual network.</span></span> <span data-ttu-id="37180-140">Vegye figyelembe a hello hivatkozás toohello erőforráscsoport már hozott létre "${azurerm_resource_group.helloterraform.name}" hello virtuális hálózati és alhálózati definíció hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="37180-140">Note hello reference toohello resource group you have created already via "${azurerm_resource_group.helloterraform.name}" in both hello virtual network and hello subnet definition.</span></span>

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
<span data-ttu-id="37180-141">hello előző parancsfájl kódtöredékek hozzon létre egy nyilvános IP-cím és a hálózati adaptert, amely használja a hello nyilvános IP-cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37180-141">hello previous script snippets create a public IP and a network interface that makes use of hello public IP created.</span></span> <span data-ttu-id="37180-142">Megjegyzés: hello toosubnet_id és public_ip_address_id hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="37180-142">Note hello references toosubnet_id and public_ip_address_id.</span></span> <span data-ttu-id="37180-143">Terraform rendelkezik beépített funkciói toounderstand adott hello hálózati illesztőnek egy függőséget hello erőforrástól függ, hogy hello hello hálózati illesztő létrehozása előtt létre kell toobe.</span><span class="sxs-lookup"><span data-stu-id="37180-143">Terraform has built-in intelligence toounderstand that hello network interface has a dependency on hello resources that need toobe created before hello creation of hello network interface.</span></span>

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
<span data-ttu-id="37180-144">A tárfiók létrehozott itt, és meghatározott egy tárolót a tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="37180-144">Here, you created a storage account and defined a storage container within that storage account.</span></span> <span data-ttu-id="37180-145">Ez a tárfiók a virtuális merevlemezek (VHD) tárolására hello virtuális géphez létre toobe kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="37180-145">This storage account is where you store virtual hard disks (VHDs) for hello virtual machine about toobe created.</span></span>

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
<span data-ttu-id="37180-146">Végezetül hello előző részlet hoz létre egy virtuális gép, amely már kiosztott összes hello erőforrásokat használja.</span><span class="sxs-lookup"><span data-stu-id="37180-146">Finally, hello previous snippet creates a virtual machine that utilizes all hello resources provisioned already.</span></span> <span data-ttu-id="37180-147">Egy tárfiók és tároló virtuális merevlemezek, hálózati illesztő – nyilvános IP- és, a megadott alhálózat és hello erőforráscsoport hozott létre.</span><span class="sxs-lookup"><span data-stu-id="37180-147">They are a storage account and container for a VHD, a network interface with public IP and subnet specified, and hello resource group you already created.</span></span> <span data-ttu-id="37180-148">Vegye figyelembe a hello vm_size tulajdonság, ahol hello parancsfájl határozza meg az Azure A0 Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="37180-148">Note hello vm_size property, where hello script specifies an Azure A0 SKU.</span></span>

### <a name="execute-hello-script"></a><span data-ttu-id="37180-149">Hello parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="37180-149">Execute hello script</span></span>
<span data-ttu-id="37180-150">Hello teljes mentett, parancsfájl toohello konzol/parancssorból való kilépéshez, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="37180-150">With hello full script saved, exit toohello console/command line and type hello following:</span></span>
```
terraform apply terraformscripts
```
<span data-ttu-id="37180-151">Némi várakozás után hello erőforrások, például egy virtuális gép jelennek meg hello `terraformtest` erőforráscsoportja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="37180-151">After some time, hello resources, including a virtual machine, appear in hello `terraformtest` resource group in hello Azure portal.</span></span>

## <a name="complete-terraform-script"></a><span data-ttu-id="37180-152">Teljes Terraform parancsfájl</span><span class="sxs-lookup"><span data-stu-id="37180-152">Complete Terraform script</span></span>

<span data-ttu-id="37180-153">Az Ön kényelme érdekében alatt van hello teljes Terraform parancsfájlt, hogy az összes hello infrastruktúra cikkben említett rendelkezések.</span><span class="sxs-lookup"><span data-stu-id="37180-153">For your convenience, below is hello complete Terraform script that provisions all of hello infrastructure discussed in this article.</span></span>

```
variable "resourcesname" {
  default = "helloterraform"
}

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

## <a name="next-steps"></a><span data-ttu-id="37180-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37180-154">Next steps</span></span>
<span data-ttu-id="37180-155">Alapvető infrastruktúra az Azure-ban hozott létre a Terraform használatával.</span><span class="sxs-lookup"><span data-stu-id="37180-155">You have created basic infrastructure in Azure by using Terraform.</span></span> <span data-ttu-id="37180-156">Összetettebb forgatókönyveket, beleértve a példák, amelyek használatát egy terheléselosztó és a virtuális gép skálázása beállítása, lásd: számos [Terraform példák az Azure-](https://github.com/hashicorp/terraform/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="37180-156">For more complex scenarios, including examples that use load balancers and virtual machine scale sets, see numerous [Terraform examples for Azure](https://github.com/hashicorp/terraform/tree/master/examples).</span></span> <span data-ttu-id="37180-157">Támogatott Azure szolgáltatók naprakész listáját, lásd: hello [Terraform dokumentáció](https://www.terraform.io/docs/providers/azurerm/index.html).</span><span class="sxs-lookup"><span data-stu-id="37180-157">For an up-to-date list of supported Azure providers, see hello [Terraform documentation](https://www.terraform.io/docs/providers/azurerm/index.html).</span></span>
