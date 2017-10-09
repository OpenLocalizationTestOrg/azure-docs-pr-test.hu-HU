---
title: "virtuális gép több hálózati adapter - Azure CLI 2.0 és aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan használja több hálózati adapterrel rendelkező virtuális gép toocreate hello Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac0291a978e2c8682c69104915196cc6c4fcf8dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-20"></a><span data-ttu-id="c1d82-103">Azure CLI 2.0 hello segítségével több hálózati adapterrel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1d82-103">Create a VM with multiple NICs using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="c1d82-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c1d82-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="c1d82-105">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](virtual-network-deploy-multinic-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c1d82-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-cli.md).</span></span>
>

## <span data-ttu-id="c1d82-106"><a name="create"></a>Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1d82-106"><a name="create"></a>Create hello VM</span></span>

<span data-ttu-id="c1d82-107">Hajthatja végre ezt a feladatot az Azure CLI 2.0 (Ez a cikk) hello vagy hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c1d82-107">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md).</span></span> <span data-ttu-id="c1d82-108">az értékek hello "" hello lépések hello változók erőforrások létre hello forgatókönyvet beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="c1d82-108">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="c1d82-109">Hello értékek, a környezetének megfelelő módosítására.</span><span class="sxs-lookup"><span data-stu-id="c1d82-109">Change hello values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="c1d82-110">Telepítse a hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="c1d82-110">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="c1d82-111">Az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek hello lépések végrehajtásával a hello [az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c1d82-111">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="c1d82-112">A bejelentkezési hello paranccsal parancshéj `az login`.</span><span class="sxs-lookup"><span data-stu-id="c1d82-112">From a command shell, login with hello command `az login`.</span></span>
4. <span data-ttu-id="c1d82-113">Hozzon létre hello VM hello parancsfájl, amely a következő Linux vagy a Mac számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c1d82-113">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="c1d82-114">hello parancsfájlt hoz létre egy erőforráscsoportot, egy virtuális hálózatot (VNet) két alhálózat, a két hálózati adapter és a hello két hálózati adapterrel rendelkező virtuális csatolni tooit.</span><span class="sxs-lookup"><span data-stu-id="c1d82-114">hello script creates a resource group, one virtual network (VNet) with two subnets, two NICs, and a VM with hello two NICs attached tooit.</span></span> <span data-ttu-id="c1d82-115">Hálózati adapter hello egyik csatlakoztatott tooone alhálózat, és hozzá van rendelve egy statikus nyilvános és magánhálózati IP-címet.</span><span class="sxs-lookup"><span data-stu-id="c1d82-115">One of hello NICs is connected tooone subnet and is assigned a static public and private IP address.</span></span> <span data-ttu-id="c1d82-116">hello más hálózati csatlakoztatott toohello más alhálózathoz, és hozzá statikus magánhálózati IP-cím és a nyilvános IP-cím van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="c1d82-116">hello other NIC is connected toohello other subnet and is assigned a static private IP address and no public IP address.</span></span> <span data-ttu-id="c1d82-117">hálózati adapter, a nyilvános IP-cím, a virtuális hálózati hello, és a Virtuálisgép-erőforrások kell lenniük a hello azonos helyen és az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c1d82-117">hello NIC, public IP address, virtual network, and VM resources must all exist in hello same location and subscription.</span></span> <span data-ttu-id="c1d82-118">Bár a hello erőforrások összes nincs tooexist hello ugyanabban az erőforráscsoportban, a parancsfájl tehetik a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c1d82-118">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

```bash
#!/bin/sh

RgName="Multi-NIC-VM"
Location="westus"

# Create a resource group.
az group create \
--name $RgName \
--location $Location

# hello address is assigned toohello resource from a pool of IP adresses unique tooeach Azure region. 
# Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists
# hello ranges for each region.

PipName="PIP-WEB"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static

# Create a virtual network with one subnet

VnetName="VNet1"
VnetPrefix="10.0.0.0/16"
VnetSubnet1Name="Front-End"
VnetSubnet1Prefix="10.0.0.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnet1Name \
--subnet-prefix $VnetSubnet1Prefix

# Create a second subnet within hello VNet

VnetSubnet2Name="Back-end"
VnetSubnet2Prefix="10.0.1.0/24"
az network vnet subnet create \
--vnet-name $VnetName \
--resource-group $RgName \
--name $VnetSubnet2Name \
--address-prefix $VnetSubnet2Prefix

# Create a network interface connected tooone of hello subnets. hello NIC is assigned a single dynamic private and
# public IP address by default, but you can instead, assign static addresses, or no public IP address at all.
# You can also assign multiple private or public IP addresses tooeach NIC. toolearn more about IP addressing
# options for NICs, enter hello az network nic create -h command.

Nic1Name="NIC-FE"
PrivateIpAddress1="10.0.0.5"

az network nic create \
--name $Nic1Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress1 \
--public-ip-address $PipName

# Create a second network interface and connect it toohello other subnet. Though multiple NICs attached toohello same
# VM can be connected toodifferent subnets, hello subnets must all be within hello same VNet. Add additional NICs as necessary.

Nic2Name="NIC-BE"
PrivateIpAddress2="10.0.1.5"

az network nic create \
--name $Nic2Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet2Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress2

# Create a VM and attach hello two NICs.

VmName="WEB"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. Not all VM sizes support
# more than one NIC, so be sure tooselect a VM size that supports hello number of NICs you want tooattach toohello VM.
# You must create hello VM with at least two NICs if you want tooadd more after VM creation. If you create a VM with
# only one NIC, you can't add additional NICs toohello VM after VM creation, regardless of how many NICs hello VM supports.
# hello VM size specified in hello following variable supports two NICs.

VmSize="Standard_DS2"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello output returned by entering the
# az vm image list command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file.

SshKeyValue="~/.ssh/id_rsa.pub"

# Before executing hello following command, add variable names of additional NICs you may have added toohello script that
# you want tooattach toohello VM. If creating a Windows VM, remove hello **ssh-key-value** line and you'll be prompted for
# hello password you want tooconfigure for hello VM.

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $Nic1Name $Nic2Name \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

<span data-ttu-id="c1d82-119">Ezen kívül két hálózati adapterrel rendelkező virtuális toocreating, hello parancsfájl hoz létre:</span><span class="sxs-lookup"><span data-stu-id="c1d82-119">In addition toocreating a VM with two NICs, hello script creates:</span></span>
- <span data-ttu-id="c1d82-120">Egyszeri támogatás lemez felügyelt alapértelmezés szerint, de más beállítások érhetők el hello lemeztípus hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="c1d82-120">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="c1d82-121">Olvasási hello [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="c1d82-121">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="c1d82-122">Két alhálózatokat és egyetlen nyilvános IP-címmel rendelkező virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="c1d82-122">A virtual network with two subnets and a single public IP address.</span></span> <span data-ttu-id="c1d82-123">Másik megoldásként használhatja *meglévő* virtuális hálózat, alhálózat, a hálózati adapter vagy nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c1d82-123">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="c1d82-124">Hogyan toouse meglévő hálózati erőforrások helyett adjon meg további erőforrások létrehozása toolearn `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="c1d82-124">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="c1d82-125"><a name = "validate"></a>Ellenőrizze a virtuális gép létrehozása és a hálózati adapterek</span><span class="sxs-lookup"><span data-stu-id="c1d82-125"><a name = "validate"></a>Validate VM creation and NICs</span></span>

1. <span data-ttu-id="c1d82-126">Adja meg a hello parancs `az resource list --resouce-group Multi-NIC-VM --output table` toosee hello parancsfájl által létrehozott hello erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="c1d82-126">Enter hello command `az resource list --resouce-group Multi-NIC-VM --output table` toosee a list of hello resources created by hello script.</span></span> <span data-ttu-id="c1d82-127">Kell lennie hat erőforrások adott vissza a kimeneti hello: két hálózati adapterrel, egy lemezt, egy nyilvános IP-cím, egy virtuális hálózat és egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="c1d82-127">There should be six resources in hello returned output: Two NICs, one disk, one public IP address, one virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="c1d82-128">Adja meg a hello parancs `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`.</span><span class="sxs-lookup"><span data-stu-id="c1d82-128">Enter hello command `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`.</span></span> <span data-ttu-id="c1d82-129">Az adott vissza a kimeneti hello, vegye figyelembe a hello értékének **IP-cím** és, hogy hello értékének **PublicIpAllocationMethod** van *statikus*.</span><span class="sxs-lookup"><span data-stu-id="c1d82-129">In hello returned output, note hello value of **IpAddress** and that hello value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="c1d82-130">Hello a következő parancs futtatása, előtt távolítsa el a hello <>, cserélje le *felhasználónév* hello használt hello nevű **felhasználónév** hello parancsfájlt, és cserélje ki a változó *IP-cím* a hello **IP-cím** hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="c1d82-130">Before running hello following command, remove hello <>, replace *Username* with hello name you used for hello **Username** variable in hello script, and replace *ipAddress* with hello **ipAddress** from hello previous step.</span></span> <span data-ttu-id="c1d82-131">Futtatási hello következő parancsot a virtuális gép tooconnect toohello: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="c1d82-131">Run hello following command tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 
4. <span data-ttu-id="c1d82-132">Miután csatlakozott a virtuális gép toohello, futtassa a hello `sudo ifconfig` parancs toosee *eth0* és *eth1* felületek.</span><span class="sxs-lookup"><span data-stu-id="c1d82-132">Once connected toohello VM, run hello `sudo ifconfig` command toosee *eth0* and *eth1* interfaces.</span></span> <span data-ttu-id="c1d82-133">Egyes hálózati adapterek hello statikus magánhálózati IP-címek hello Azure DHCP-kiszolgálók hello parancsfájlban megadott van rendelve.</span><span class="sxs-lookup"><span data-stu-id="c1d82-133">Each NIC has been assigned hello static private IP addresses specified in hello script by hello Azure DHCP servers.</span></span> <span data-ttu-id="c1d82-134">hello hozzárendelt toohello hálózati adapter IP- és MAC-címek ne változtassa meg amíg hello virtuális gép törlődik.</span><span class="sxs-lookup"><span data-stu-id="c1d82-134">hello IP and MAC addresses assigned toohello NICs do not change until hello VM is deleted.</span></span> <span data-ttu-id="c1d82-135">Azt javasoljuk, hogy nem változtat IP-címzést, az operációs rendszerben, azt is le lehet tiltani kapcsolat toohello számítógép.</span><span class="sxs-lookup"><span data-stu-id="c1d82-135">We recommend that you do not change IP addressing within an operating system, as it can disable connectivity toohello computer.</span></span> <span data-ttu-id="c1d82-136">Nyilvános IP-címek nem jelennek meg hello operációs rendszerben, mert azok hálózati cím lefordított tooand hello privát IP-címről hello Azure infrastruktúra által.</span><span class="sxs-lookup"><span data-stu-id="c1d82-136">Public IP addresses do not appear within hello operating system, as they are network address translated tooand from hello private IP address by hello Azure infrastructure.</span></span>

## <span data-ttu-id="c1d82-137"><a name= "clean-up"></a>Távolítsa el a virtuális gép hello és a kapcsolódó erőforrások</span><span class="sxs-lookup"><span data-stu-id="c1d82-137"><a name= "clean-up"></a>Remove hello VM and associated resources</span></span>

<span data-ttu-id="c1d82-138">Javasoljuk, hogy törölje-e ebben a gyakorlatban létrejött, ha nem használja őket éles környezetben hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c1d82-138">It's recommended that you delete hello resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="c1d82-139">Virtuális gép, a nyilvános IP-cím és a lemezerőforrásokat függő díj terheli, mindaddig, amíg azok van üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="c1d82-139">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="c1d82-140">Ebben a gyakorlatban teljes hello lépések során létrejött tooremove hello erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="c1d82-140">tooremove hello resources created during this exercise, complete hello following steps:</span></span>
1. <span data-ttu-id="c1d82-141">tooview hello erőforrások hello erőforráscsoportban, futtassa a hello `az resource list --resource-group Multi-NIC-VM` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1d82-141">tooview hello resources in hello resource group, run hello `az resource list --resource-group Multi-NIC-VM` command.</span></span>
2. <span data-ttu-id="c1d82-142">Ellenőrizze, hogy nincsenek erőforrások hello erőforráscsoport nem ebben a cikkben hello parancsfájl által létrehozott hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="c1d82-142">Confirm there are no resources in hello resource group, other than hello resources created by hello script in this article.</span></span> 
3. <span data-ttu-id="c1d82-143">Ebben a gyakorlatban, futtassa a hello létrehozott összes erőforrás toodelete `az group delete --name Multi-NIC-VM` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1d82-143">toodelete all resources created in this exercise, run hello `az group delete --name Multi-NIC-VM` command.</span></span> <span data-ttu-id="c1d82-144">hello parancs hello erőforráscsoport és a benne található összes hello erőforrás törlése.</span><span class="sxs-lookup"><span data-stu-id="c1d82-144">hello command deletes hello resource group and all hello resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1d82-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1d82-145">Next steps</span></span>

<span data-ttu-id="c1d82-146">Hálózati forgalmat a virtuális gép létrehozása ebben a cikkben hello tooand áramolhasson.</span><span class="sxs-lookup"><span data-stu-id="c1d82-146">Any network traffic can flow tooand from hello VM created in this article.</span></span> <span data-ttu-id="c1d82-147">Bejövő és kimenő szabályok belül egy NSG-t, amely korlátozza, hogy mindegyik hálózati interfész vagy az egyes alhálózatokon tooand áramolhasson hello forgalmat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c1d82-147">You can define inbound and outbound rules within an NSG that limit hello traffic that can flow tooand from each network interface, each subnet, or both.</span></span> <span data-ttu-id="c1d82-148">További információk az NSG-k, olvassa el a hello toolearn [NSG áttekintése](virtual-networks-nsg.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c1d82-148">toolearn more about NSGs, read hello [NSG overview](virtual-networks-nsg.md) article.</span></span>
