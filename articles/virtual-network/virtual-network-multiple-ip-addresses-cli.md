---
title: "hello Azure CLI 2.0 használatával több IP-címmel aaaVM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gép használt hello Azure CLI 2.0 |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 15efd853cc7c31bacb64ed052dabedd3fe4d3079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-cli-20"></a><span data-ttu-id="df829-103">Több IP-cím hozzárendelése toovirtual gépek hello Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="df829-103">Assign multiple IP addresses toovirtual machines using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="df829-104">Ez a cikk azt ismerteti, hogyan toocreate keresztül hello Azure Resource Manager deployment model használatával egy virtuális gép (VM) hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="df829-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using hello Azure CLI 2.0.</span></span> <span data-ttu-id="df829-105">Több IP-cím hello klasszikus telepítési modell használatával létrehozott tooresources nem lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="df829-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="df829-106">További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="df829-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="df829-107"><a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="df829-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="df829-108">Hajthatja végre ezt a feladatot az Azure CLI 2.0 (Ez a cikk) hello vagy hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="df829-108">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md).</span></span> <span data-ttu-id="df829-109">Hello értékek, a környezetének megfelelő módosítására.</span><span class="sxs-lookup"><span data-stu-id="df829-109">Change hello values, as appropriate, for your environment.</span></span> <span data-ttu-id="df829-110">hello következő lépések azt ismertetik, hogyan toocreate például VM több IP-címek, hello forgatókönyvben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="df829-110">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="df829-111">A változók értékeinek módosítása "" és az IP-cím típusok pedig ez szükséges lenne a megvalósítás.</span><span class="sxs-lookup"><span data-stu-id="df829-111">Change variable values in "" and IP address types as required for your implementation.</span></span> 

1. <span data-ttu-id="df829-112">Telepítse a hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="df829-112">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="df829-113">Az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek hello lépések végrehajtásával a hello [az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="df829-113">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="df829-114">A bejelentkezési hello paranccsal parancshéj `az login` válassza ki a hello előfizetés használata.</span><span class="sxs-lookup"><span data-stu-id="df829-114">From a command shell, login with hello command `az login` and select hello subscription you're using.</span></span>
4. <span data-ttu-id="df829-115">Hozzon létre hello VM hello parancsfájl, amely a következő Linux vagy a Mac számítógépen.</span><span class="sxs-lookup"><span data-stu-id="df829-115">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="df829-116">hello parancsfájlt hoz létre egy erőforráscsoportot, egy virtuális hálózathoz (VNet), egyetlen hálózati Adapterrel rendelkező három IP-konfigurációk és a virtuális gépek hello két hálózati adapter csatlakoztatva tooit.</span><span class="sxs-lookup"><span data-stu-id="df829-116">hello script creates a resource group, one virtual network (VNet), one NIC with three IP configurations, and a VM with hello two NICs attached tooit.</span></span> <span data-ttu-id="df829-117">hálózati adapter, a nyilvános IP-cím, a virtuális hálózati hello, és a Virtuálisgép-erőforrások kell lenniük a hello azonos helyen és az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="df829-117">hello NIC, public IP address, virtual network, and VM resources must all exist in hello same location and subscription.</span></span> <span data-ttu-id="df829-118">Bár a hello erőforrások összes nincs tooexist hello ugyanabban az erőforráscsoportban, a parancsfájl tehetik a következő hello.</span><span class="sxs-lookup"><span data-stu-id="df829-118">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using hello `--allocation-method Static` option. If you
# do not specify this option, hello address is allocated dynamically. hello address is assigned toohello resource from a pool
# of IP adresses unique tooeach Azure region. Download and view hello file from
# https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists hello ranges for each region.

PipName="myPublicIP"

# This name must be unique within an Azure location.
DnsName="myDNSName"

az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--dns-name $DnsName\
--allocation-method Static

# Create a virtual network with one subnet

VnetName="myVnet"
VnetPrefix="10.0.0.0/16"
VnetSubnetName="mySubnet"
VnetSubnetPrefix="10.0.0.0/24"

az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnetName \
--subnet-prefix $VnetSubnetPrefix

# Create a network interface connected toohello subnet and associate hello public IP address tooit. Azure will create the
# first IP configuration with a static private IP address and will associate hello public IP address resource tooit.

NicName="MyNic1"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--private-ip-address 10.0.0.4
--vnet-name $VnetName \
--public-ip-address $PipName
    
# Create a second public IP address, a second IP configuration, and associate it toohello NIC. This configuration has a
# static public IP address and a static private IP address.

az network public-ip create \
--resource-group $RgName \
--location $Location \
--name myPublicIP2 \
--dns-name mypublicdns2 \
--allocation-method Static

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--name IPConfig-2 \
--private-ip-address 10.0.0.5 \
--public-ip-name myPublicIP2

# Create a third IP configuration, and associate it toohello NIC. This configuration has  static private IP address and # no public IP address.

azure network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--private-ip-address 10.0.0.6 \
--name IPConfig-3

# Note: Though this article assigns all IP configurations tooa single NIC, you can also assign multiple IP configurations
# tooany NIC in a VM. toolearn how toocreate a VM with multiple NICs, read hello Create a VM with multiple NICs 
# article: https://docs.microsoft.com/azure/virtual-network/virtual-network-deploy-multinic-arm-cli.

# Create a VM and attach hello NIC.

VmName="myVm"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes rticle. hello script fails if hello VM size
# is not supported in hello location you select. Run hello `azure vm sizes --location estcentralus` command tooget a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello utput returned by entering the
# `az vm image list` command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file. If you're creating a Windows VM, remove hello following
# line and you'll be prompted for hello password you want tooconfigure for hello VM.

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
```

<span data-ttu-id="df829-119">Ezenkívül toocreating virtuális gép és egy hálózati Adaptert a 3 IP-konfigurációk, hello parancsfájlt hoz létre:</span><span class="sxs-lookup"><span data-stu-id="df829-119">In addition toocreating a VM with a NIC with 3 IP configurations, hello script creates:</span></span>

- <span data-ttu-id="df829-120">Egyszeri támogatás lemez felügyelt alapértelmezés szerint, de más beállítások érhetők el hello lemeztípus hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="df829-120">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="df829-121">Olvasási hello [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="df829-121">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="df829-122">Egy virtuális hálózatot egyetlen alhálózattal és két nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="df829-122">A virtual network with one subnet and two public IP addresses.</span></span> <span data-ttu-id="df829-123">Másik megoldásként használhatja *meglévő* virtuális hálózat, alhálózat, a hálózati adapter vagy nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="df829-123">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="df829-124">Hogyan toouse meglévő hálózati erőforrások helyett adjon meg további erőforrások létrehozása toolearn `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="df829-124">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

<span data-ttu-id="df829-125">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="df829-125">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="df829-126">tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="df829-126">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="df829-127">Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="df829-127">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="df829-128">További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="df829-128">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

<span data-ttu-id="df829-129">Hello virtuális gép létrehozása után adja meg a hello `az network nic show --name MyNic1 --resource-group myResourceGroup` parancs tooview hello hálózati Adapteres konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="df829-129">After hello VM is created, enter hello `az network nic show --name MyNic1 --resource-group myResourceGroup` command tooview hello NIC configuration.</span></span> <span data-ttu-id="df829-130">Adja meg a hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview hello IP-konfigurációk listáját társított toohello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="df829-130">Enter hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview a list of hello IP configurations associated toohello NIC.</span></span>

<span data-ttu-id="df829-131">Hozzáadás hello privát IP-címek toohello virtuális gép operációs rendszer hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="df829-131">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="df829-132"><a name="add"></a>Adja hozzá az IP-címek tooa méretű VM</span><span class="sxs-lookup"><span data-stu-id="df829-132"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="df829-133">Hozzáadhat további magán- és IP címeket tooan meglévő hálózati hello következő lépések végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="df829-133">You can add additional private and public IP addresses tooan existing NIC by completing hello steps that follow.</span></span> <span data-ttu-id="df829-134">hello példák épül hello [forgatókönyv](#Scenario) a cikkben.</span><span class="sxs-lookup"><span data-stu-id="df829-134">hello examples build upon hello [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="df829-135">Nyisson meg egy parancssori rendszerhéj és a teljes hello hátralévő lépéseket ebben a szakaszban egy munkameneten belül.</span><span class="sxs-lookup"><span data-stu-id="df829-135">Open a command shell and complete hello remaining steps in this section within a single session.</span></span> <span data-ttu-id="df829-136">Ha még nem rendelkezik Azure CLI telepítése és konfigurálása, teljes hello hello szükséges lépések [Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben és a bejelentkezési tooyour hello Azure fiók `az-login` parancs.</span><span class="sxs-lookup"><span data-stu-id="df829-136">If you don't already have Azure CLI installed and configured, complete hello steps in hello [Azure CLI 2.0 installation](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) article and login tooyour Azure account with hello `az-login` command.</span></span>

2. <span data-ttu-id="df829-137">Végezze el a következő szakaszok a követelmények alapján hello egyikében hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="df829-137">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    <span data-ttu-id="df829-138">**Adja hozzá a magánhálózati IP-cím**</span><span class="sxs-lookup"><span data-stu-id="df829-138">**Add a private IP address**</span></span>
    
    <span data-ttu-id="df829-139">tooadd egy privát IP-cím tooa hálózati adapter, létre kell hoznia egy IP-konfiguráció, amely a következő hello parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="df829-139">tooadd a private IP address tooa NIC, you must create an IP configuration using hello command that follows.</span></span> <span data-ttu-id="df829-140">hello statikus IP-cím hello alhálózat nem használt címnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="df829-140">hello static IP address must be an unused address for hello subnet.</span></span>

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    <span data-ttu-id="df829-141">Tetszőleges számú konfigurációk összes kívánt beállítást, egyedi konfigurációs nevek és privát IP-címek használata (a statikus IP-címekkel rendelkező konfigurációk) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="df829-141">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    <span data-ttu-id="df829-142">**A nyilvános IP-cím hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="df829-142">**Add a public IP address**</span></span>
    
    <span data-ttu-id="df829-143">A nyilvános IP-cím való társítás tooeither által hozzáadott új IP-konfiguráció vagy a meglévő IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="df829-143">A public IP address is added by associating it tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="df829-144">Hajtsa végre hello egyik hello szakaszokban szereplő, az összes kívánt beállítást.</span><span class="sxs-lookup"><span data-stu-id="df829-144">Complete hello steps in one of hello sections that follow, as you require.</span></span>

    <span data-ttu-id="df829-145">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="df829-145">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="df829-146">tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="df829-146">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="df829-147">Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="df829-147">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="df829-148">További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="df829-148">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    - <span data-ttu-id="df829-149">**Hello erőforrás tooa új IP-konfiguráció hozzárendelése**</span><span class="sxs-lookup"><span data-stu-id="df829-149">**Associate hello resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="df829-150">Egy nyilvános IP-címet ad hozzá egy új IP-konfiguráció, amikor is hozzá kell magánhálózati IP-cím, mert az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="df829-150">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="df829-151">Egy meglévő nyilvános IP-cím erőforrás hozzáadása, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="df829-151">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="df829-152">toocreate egy új, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="df829-152">toocreate a new one, enter hello following command:</span></span>
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        <span data-ttu-id="df829-153">egy új IP-beállítását egy statikus magánhálózati IP-cím és a kapcsolódó hello toocreate *myPublicIP3* nyilvános IP-cím erőforrás címet, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="df829-153">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIP3* public IP address resource, enter hello following command:</span></span>

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - <span data-ttu-id="df829-154">**Rendeljen hozzá hello erőforrás tooan meglévő IP-konfiguráció** egy nyilvános IP-cím erőforrás csak lehet társított tooan IP-konfigurációja, amely még nincs társítva.</span><span class="sxs-lookup"><span data-stu-id="df829-154">**Associate hello resource tooan existing IP configuration** A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="df829-155">Segítségével meghatározhatja, hogy rendelkezik-e IP-konfigurációt tartozó nyilvános IP-cím hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="df829-155">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        <span data-ttu-id="df829-156">Kimeneti adott vissza:</span><span class="sxs-lookup"><span data-stu-id="df829-156">Returned output:</span></span>
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        <span data-ttu-id="df829-157">Hello óta **PublicIpAddressId** oszlopában *IpConfig-3* van a hello üres kimeneti, nem nyilvános IP-cím erőforrás jelenleg társított tooit.</span><span class="sxs-lookup"><span data-stu-id="df829-157">Since hello **PublicIpAddressId** column for *IpConfig-3* is blank in hello output, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="df829-158">Adja hozzá egy meglévő nyilvános IP cím erőforrás tooIpConfig-3, vagy adja meg a következő parancs toocreate egy hello:</span><span class="sxs-lookup"><span data-stu-id="df829-158">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        <span data-ttu-id="df829-159">Adja meg a következő parancs tooassociate hello nyilvános IP-cím erőforrás toohello meglévő IP-konfiguráció hello *IPConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="df829-159">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IPConfig-3*:</span></span>
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. <span data-ttu-id="df829-160">Nézet hello magánhálózati IP-címek és hello nyilvános IP-cím erőforrás-azonosítók hozzárendelt toohello megadásával NIC hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="df829-160">View hello private IP addresses and hello public IP address resource Ids assigned toohello NIC by entering hello following command:</span></span>

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    <span data-ttu-id="df829-161">Kimeneti adott vissza:</span><span class="sxs-lookup"><span data-stu-id="df829-161">Returned output:</span></span> <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. <span data-ttu-id="df829-162">Hello toohello NIC toohello virtuális gép operációs rendszer hello hello utasításait követve hozzáadott magánhálózati IP-címek hozzáadása [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="df829-162">Add hello private IP addresses you added toohello NIC toohello VM operating system by following hello instructions in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="df829-163">Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="df829-163">Do not add hello public IP addresses toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
