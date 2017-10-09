---
title: "a virtuális gép statikus nyilvános IP-címmel – Azure CLI 2.0 aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate virtuális gép és a statikus nyilvános IP cím használatával hello Azure parancssori felület (CLI) 2.0-s."
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
ms.openlocfilehash: 486060463486462dd8336734a7ad23c4a2cba452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-cli-20"></a><span data-ttu-id="c5da9-103">Virtuális gép létrehozása az Azure CLI 2.0 hello segítségével statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="c5da9-103">Create a VM with a static public IP address using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c5da9-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c5da9-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="c5da9-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5da9-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="c5da9-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c5da9-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="c5da9-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c5da9-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="c5da9-108">Sablon</span><span class="sxs-lookup"><span data-stu-id="c5da9-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="c5da9-109">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="c5da9-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

<span data-ttu-id="c5da9-110">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c5da9-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="c5da9-111">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="c5da9-111">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <span data-ttu-id="c5da9-112"><a name = "create"></a>Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5da9-112"><a name = "create"></a>Create hello VM</span></span>

<span data-ttu-id="c5da9-113">Hajthatja végre ezt a feladatot az Azure CLI 2.0 (Ez a cikk) hello vagy hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c5da9-113">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span></span> <span data-ttu-id="c5da9-114">az értékek hello "" hello lépések hello változók erőforrások létre hello forgatókönyvet beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="c5da9-114">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="c5da9-115">Hello értékek, a környezetének megfelelő módosítására.</span><span class="sxs-lookup"><span data-stu-id="c5da9-115">Change hello values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="c5da9-116">Telepítse a hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="c5da9-116">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="c5da9-117">Az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek hello lépések végrehajtásával a hello [az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c5da9-117">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="c5da9-118">A bejelentkezési hello paranccsal parancshéj `az login`.</span><span class="sxs-lookup"><span data-stu-id="c5da9-118">From a command shell, login with hello command `az login`.</span></span>
4. <span data-ttu-id="c5da9-119">Hozzon létre hello VM hello parancsfájl, amely a következő Linux vagy a Mac számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c5da9-119">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="c5da9-120">az Azure nyilvános IP-cím, a virtuális hálózat, a hálózati illesztő hello, és a Virtuálisgép-erőforrások kell lenniük a hello azonos helyen.</span><span class="sxs-lookup"><span data-stu-id="c5da9-120">hello Azure public IP address, virtual network, network interface, and VM resources must all exist in hello same location.</span></span> <span data-ttu-id="c5da9-121">Bár a hello erőforrások összes nincs tooexist hello ugyanabban az erőforráscsoportban, a parancsfájl tehetik a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c5da9-121">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using hello --allocation-method Static option.
# If you do not specify this option, hello address is allocated dynamically. hello address is assigned toothe
# resource from a pool of IP adresses unique tooeach Azure region. hello DnsName must be unique within the
# Azure location it's created in. Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists hello ranges for each region.

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

# Create a network interface connected toohello VNet with a static private IP address and associate hello public IP address
# resource toohello NIC.

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

# Create a new VM with hello NIC

VmName="WEB1"

# Replace hello value for hello VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable with a value for *urn* from hello output returned by entering
# hello `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace hello following value with hello path tooyour public key file.
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
# If creating a Windows VM, remove hello previous line and you'll be prompted for hello password you want tooconfigure for hello VM.
```

<span data-ttu-id="c5da9-122">Továbbá a virtuális gépek toocreating, hello parancsfájl hoz létre:</span><span class="sxs-lookup"><span data-stu-id="c5da9-122">In addition toocreating a VM, hello script creates:</span></span>
- <span data-ttu-id="c5da9-123">Egyszeri támogatás lemez felügyelt alapértelmezés szerint, de más beállítások érhetők el hello lemeztípus hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="c5da9-123">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="c5da9-124">Olvasási hello [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="c5da9-124">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="c5da9-125">Virtuális hálózati alhálózat, a hálózati adapter és nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c5da9-125">Virtual network, subnet, NIC, and public IP address resources.</span></span> <span data-ttu-id="c5da9-126">Másik megoldásként használhatja *meglévő* virtuális hálózat, alhálózat, a hálózati adapter vagy nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c5da9-126">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="c5da9-127">Hogyan toouse meglévő hálózati erőforrások helyett adjon meg további erőforrások létrehozása toolearn `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="c5da9-127">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="c5da9-128"><a name = "validate"></a>Virtuális gép létrehozása és a nyilvános IP-cím ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c5da9-128"><a name = "validate"></a>Validate VM creation and public IP address</span></span>

1. <span data-ttu-id="c5da9-129">Adja meg a hello parancs `az resource list --resouce-group IaaSStory --output table` toosee hello parancsfájl által létrehozott hello erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="c5da9-129">Enter hello command `az resource list --resouce-group IaaSStory --output table` toosee a list of hello resources created by hello script.</span></span> <span data-ttu-id="c5da9-130">Kell lennie öt erőforrások adott vissza a kimeneti hello: hálózati kapcsolat, lemez, nyilvános IP-cím, virtuális hálózati és a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c5da9-130">There should be five resources in hello returned output: network interface, disk, public IP address, virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="c5da9-131">Adja meg a hello parancs `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span><span class="sxs-lookup"><span data-stu-id="c5da9-131">Enter hello command `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span></span> <span data-ttu-id="c5da9-132">Az adott vissza a kimeneti hello, vegye figyelembe a hello értékének **IP-cím** és, hogy hello értékének **PublicIpAllocationMethod** van *statikus*.</span><span class="sxs-lookup"><span data-stu-id="c5da9-132">In hello returned output, note hello value of **IpAddress** and that hello value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="c5da9-133">Hello a következő parancs végrehajtásakor, előtt távolítsa el a hello <>, cserélje le *felhasználónév* hello használt hello nevű **felhasználónév** hello parancsfájlt, és cserélje ki a változó *IP-cím* a hello **IP-cím** hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="c5da9-133">Before executing hello following command, remove hello <>, replace *Username* with hello name you used for hello **Username** variable in hello script, and replace *ipAddress* with hello **ipAddress** from hello previous step.</span></span> <span data-ttu-id="c5da9-134">Futtatási hello következő parancsot a virtuális gép tooconnect toohello: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="c5da9-134">Run hello following command tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 

## <span data-ttu-id="c5da9-135"><a name= "clean-up"></a>Távolítsa el a virtuális gép hello és a kapcsolódó erőforrások</span><span class="sxs-lookup"><span data-stu-id="c5da9-135"><a name= "clean-up"></a>Remove hello VM and associated resources</span></span>

<span data-ttu-id="c5da9-136">Javasoljuk, hogy törölje-e ebben a gyakorlatban létrejött, ha nem használja őket éles környezetben hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c5da9-136">It's recommended that you delete hello resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="c5da9-137">Virtuális gép, a nyilvános IP-cím és a lemezerőforrásokat függő díj terheli, mindaddig, amíg azok van üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="c5da9-137">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="c5da9-138">Ebben a gyakorlatban teljes hello lépések során létrejött tooremove hello erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="c5da9-138">tooremove hello resources created during this exercise, complete hello following steps:</span></span>

1. <span data-ttu-id="c5da9-139">tooview hello erőforrások hello erőforráscsoportban, futtassa a hello `az resource list --resource-group IaaSStory` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c5da9-139">tooview hello resources in hello resource group, run hello `az resource list --resource-group IaaSStory` command.</span></span>
2. <span data-ttu-id="c5da9-140">Ellenőrizze, hogy nincsenek erőforrások hello erőforráscsoport nem ebben a cikkben hello parancsfájl által létrehozott hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="c5da9-140">Confirm there are no resources in hello resource group, other than hello resources created by hello script in this article.</span></span> 
3. <span data-ttu-id="c5da9-141">Ebben a gyakorlatban, futtassa a hello létrehozott összes erőforrás toodelete `az group delete -n IaaSStory` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c5da9-141">toodelete all resources created in this exercise, run hello `az group delete -n IaaSStory` command.</span></span> <span data-ttu-id="c5da9-142">hello parancs hello erőforráscsoport és a benne található összes hello erőforrás törlése.</span><span class="sxs-lookup"><span data-stu-id="c5da9-142">hello command deletes hello resource group and all hello resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5da9-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5da9-143">Next steps</span></span>

<span data-ttu-id="c5da9-144">Hálózati forgalmat a virtuális gép létrehozása ebben a cikkben hello tooand áramolhasson.</span><span class="sxs-lookup"><span data-stu-id="c5da9-144">Any network traffic can flow tooand from hello VM created in this article.</span></span> <span data-ttu-id="c5da9-145">Bejövő és kimenő szabályok belül egy NSG áramolhasson hello hálózati adapter vagy hello alhálózati tooand hello forgalom korlátozó adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c5da9-145">You can define inbound and outbound rules within an NSG that limit hello traffic that can flow tooand from hello network interface, hello subnet, or both.</span></span> <span data-ttu-id="c5da9-146">További információk az NSG-k, olvassa el a hello toolearn [NSG áttekintése](virtual-networks-nsg.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c5da9-146">toolearn more about NSGs, read hello [NSG overview](virtual-networks-nsg.md) article.</span></span>
