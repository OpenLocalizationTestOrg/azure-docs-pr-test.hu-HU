---
title: "Virtuális gép létrehozása egy statikus nyilvános IP-cím - Azure CLI 2.0 |} Microsoft Docs"
description: "Útmutató: virtuális gép létrehozása az Azure parancssori felület (CLI) 2.0 használatával statikus nyilvános IP-cím."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a4c32694949880037f01bb2b6b9779d2cbb9809c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli-20"></a><span data-ttu-id="49d8d-103">Virtuális gép létrehozása az Azure CLI 2.0 használatával statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="49d8d-103">Create a VM with a static public IP address using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="49d8d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="49d8d-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="49d8d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="49d8d-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="49d8d-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="49d8d-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="49d8d-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="49d8d-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="49d8d-108">Sablon</span><span class="sxs-lookup"><span data-stu-id="49d8d-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="49d8d-109">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="49d8d-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

<span data-ttu-id="49d8d-110">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49d8d-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="49d8d-111">Ez a cikk a Microsoft azt javasolja, hogy a klasszikus üzembe helyezési modellel helyett az új telepítések esetén a Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="49d8d-111">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <span data-ttu-id="49d8d-112"><a name = "create"></a>A virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="49d8d-112"><a name = "create"></a>Create the VM</span></span>

<span data-ttu-id="49d8d-113">Hajthatja végre ezt a feladatot az Azure CLI 2.0 (Ez a cikk) vagy a [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="49d8d-113">You can complete this task using the Azure CLI 2.0 (this article) or the [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span></span> <span data-ttu-id="49d8d-114">Az értékeket a "" a következő lépések a változók létre erőforrásokat tudja kihozni a forgatókönyvből beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="49d8d-114">The values in "" for the variables in the steps that follow create resources with settings from the scenario.</span></span> <span data-ttu-id="49d8d-115">Módosítsa az értékeket, a környezetének megfelelő.</span><span class="sxs-lookup"><span data-stu-id="49d8d-115">Change the values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="49d8d-116">Telepítse a [Azure CLI 2.0](/cli/azure/install-az-cli2) Ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="49d8d-116">Install the [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="49d8d-117">Az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek; Ehhez hajtsa végre a lépéseket a [az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49d8d-117">Create an SSH public and private key pair for Linux VMs by completing the steps in the [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="49d8d-118">Jelentkezzen be a parancsot a parancssorba a `az login`.</span><span class="sxs-lookup"><span data-stu-id="49d8d-118">From a command shell, login with the command `az login`.</span></span>
4. <span data-ttu-id="49d8d-119">A virtuális gép létrehozása az alábbi Linux vagy a Mac számítógépen parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="49d8d-119">Create the VM by executing the script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="49d8d-120">Az Azure nyilvános IP-cím, a virtuális hálózati, a hálózati adapter és a Virtuálisgép-erőforrások összes léteznie kell a helyre.</span><span class="sxs-lookup"><span data-stu-id="49d8d-120">The Azure public IP address, virtual network, network interface, and VM resources must all exist in the same location.</span></span> <span data-ttu-id="49d8d-121">Bár az erőforrásokat ugyanabban az erőforráscsoportban szerepel a nincs, a következő parancsfájlban tehetik.</span><span class="sxs-lookup"><span data-stu-id="49d8d-121">Though the resources don't all have to exist in the same resource group, in the following script they do.</span></span>

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using the --allocation-method Static option.
# If you do not specify this option, the address is allocated dynamically. The address is assigned to the
# resource from a pool of IP adresses unique to each Azure region. The DnsName must be unique within the
# Azure location it's created in. Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists the ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected to the VNet with a static private IP address and associate the public IP address
# resource to the NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with the NIC

VmName="WEB1"

# Replace the value for the VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace the value for the OsImage variable with a value for *urn* from the output returned by entering
# the `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace the following value with the path to your public key file.
SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
# If creating a Windows VM, remove the previous line and you'll be prompted for the password you want to configure for the VM.
```

<span data-ttu-id="49d8d-122">Virtuális gép létrehozása mellett a parancsfájl hoz létre:</span><span class="sxs-lookup"><span data-stu-id="49d8d-122">In addition to creating a VM, the script creates:</span></span>
- <span data-ttu-id="49d8d-123">Egyszeri támogatás lemez felügyelt alapértelmezés szerint, de más beállítások érhetők el a lemez típusát is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="49d8d-123">A single premium managed disk by default, but you have other options for the disk type you can create.</span></span> <span data-ttu-id="49d8d-124">Olvassa el a [Linux virtuális gép létrehozása az Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="49d8d-124">Read the [Create a Linux VM using the Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="49d8d-125">Virtuális hálózati alhálózat, a hálózati adapter és nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49d8d-125">Virtual network, subnet, NIC, and public IP address resources.</span></span> <span data-ttu-id="49d8d-126">Másik megoldásként használhatja *meglévő* virtuális hálózat, alhálózat, a hálózati adapter vagy nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49d8d-126">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="49d8d-127">Megtudhatja, hogyan használja a további erőforrások létrehozása helyett a meglévő hálózati erőforrások, írja be a következőt `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="49d8d-127">To learn how to use existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="49d8d-128"><a name = "validate"></a>Virtuális gép létrehozása és a nyilvános IP-cím ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="49d8d-128"><a name = "validate"></a>Validate VM creation and public IP address</span></span>

1. <span data-ttu-id="49d8d-129">Adja meg a parancsot `az resource list --resouce-group IaaSStory --output table` hozta létre a parancsfájl erőforrások listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="49d8d-129">Enter the command `az resource list --resouce-group IaaSStory --output table` to see a list of the resources created by the script.</span></span> <span data-ttu-id="49d8d-130">Nem kell öt erőforrások visszaadott kimenet: hálózati kapcsolat, lemez, nyilvános IP-cím, virtuális hálózati és a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="49d8d-130">There should be five resources in the returned output: network interface, disk, public IP address, virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="49d8d-131">Adja meg a parancsot `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span><span class="sxs-lookup"><span data-stu-id="49d8d-131">Enter the command `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span></span> <span data-ttu-id="49d8d-132">A visszaadott kimeneti jegyezze fel a értékének **IP-cím** , és hogy értékének **PublicIpAllocationMethod** van *statikus*.</span><span class="sxs-lookup"><span data-stu-id="49d8d-132">In the returned output, note the value of **IpAddress** and that the value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="49d8d-133">A következő parancs végrehajtása előtt távolítsa el a <>, cserélje le *felhasználónév* használt névvel a **felhasználónév** változó a parancsfájlt, és cserélje le a *IP-cím*rendelkező a **IP-cím** az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="49d8d-133">Before executing the following command, remove the <>, replace *Username* with the name you used for the **Username** variable in the script, and replace *ipAddress* with the **ipAddress** from the previous step.</span></span> <span data-ttu-id="49d8d-134">A következő parancsot a virtuális Géphez való kapcsolódáshoz: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="49d8d-134">Run the following command to connect to the VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 

## <span data-ttu-id="49d8d-135"><a name= "clean-up"></a>Távolítsa el a virtuális gép és a kapcsolódó erőforrások</span><span class="sxs-lookup"><span data-stu-id="49d8d-135"><a name= "clean-up"></a>Remove the VM and associated resources</span></span>

<span data-ttu-id="49d8d-136">Javasoljuk, hogy törölje-e az erőforrások létrehozott ebben a gyakorlatban, ha nem használja őket éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="49d8d-136">It's recommended that you delete the resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="49d8d-137">Virtuális gép, a nyilvános IP-cím és a lemezerőforrásokat függő díj terheli, mindaddig, amíg azok van üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="49d8d-137">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="49d8d-138">A gyakorlat során létrehozott erőforrások eltávolításához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="49d8d-138">To remove the resources created during this exercise, complete the following steps:</span></span>

1. <span data-ttu-id="49d8d-139">Az erőforrások az erőforráscsoportban megtekintéséhez futtassa a `az resource list --resource-group IaaSStory` parancsot.</span><span class="sxs-lookup"><span data-stu-id="49d8d-139">To view the resources in the resource group, run the `az resource list --resource-group IaaSStory` command.</span></span>
2. <span data-ttu-id="49d8d-140">Ellenőrizze, hogy nincsenek erőforrások az erőforráscsoportban, az ebben a cikkben a parancsfájl által létrehozott erőforrások eltérő.</span><span class="sxs-lookup"><span data-stu-id="49d8d-140">Confirm there are no resources in the resource group, other than the resources created by the script in this article.</span></span> 
3. <span data-ttu-id="49d8d-141">Ebben a gyakorlatban létrehozott összes erőforrást törli, futtassa a `az group delete -n IaaSStory` parancsot.</span><span class="sxs-lookup"><span data-stu-id="49d8d-141">To delete all resources created in this exercise, run the `az group delete -n IaaSStory` command.</span></span> <span data-ttu-id="49d8d-142">A parancs törli az erőforráscsoportot és a benne található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="49d8d-142">The command deletes the resource group and all the resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49d8d-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49d8d-143">Next steps</span></span>

<span data-ttu-id="49d8d-144">A hálózati forgalommal áramolhasson felé és felől a virtuális gép létrehozása az ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="49d8d-144">Any network traffic can flow to and from the VM created in this article.</span></span> <span data-ttu-id="49d8d-145">Megadhatja a bejövő és kimenő szabályok belül egy NSG korlátozó áramolhasson az a hálózati adapter vagy az alhálózat érkező vagy oda irányuló forgalmat.</span><span class="sxs-lookup"><span data-stu-id="49d8d-145">You can define inbound and outbound rules within an NSG that limit the traffic that can flow to and from the network interface, the subnet, or both.</span></span> <span data-ttu-id="49d8d-146">Ha többet szeretne megtudni az NSG-k, olvassa el a [NSG áttekintése](virtual-networks-nsg.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="49d8d-146">To learn more about NSGs, read the [NSG overview](virtual-networks-nsg.md) article.</span></span>