---
title: "hello Azure CLI 1.0 használatával több IP-címmel aaaVM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gép használt hello Azure CLI 1.0 |} Erőforrás-kezelő."
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
ms.openlocfilehash: 83ad48e67309fb21d5aca967d4f2c01afdc0b5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-azure-cli-10"></a><span data-ttu-id="6248c-103">Több IP-cím használata az Azure CLI 1.0 toovirtual gépek hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6248c-103">Assign multiple IP addresses toovirtual machines using Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="6248c-104">Ez a cikk azt ismerteti, hogyan toocreate keresztül hello Azure Resource Manager deployment model használatával egy virtuális gép (VM) hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="6248c-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using hello Azure CLI 1.0.</span></span> <span data-ttu-id="6248c-105">Több IP-cím hello klasszikus telepítési modell használatával létrehozott tooresources nem lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="6248c-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="6248c-106">További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6248c-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="6248c-107"><a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6248c-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="6248c-108">Hajthatja végre ezt a feladatot az Azure CLI 1.0 (Ez a cikk) hello vagy hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span><span class="sxs-lookup"><span data-stu-id="6248c-108">You can complete this task using hello Azure CLI 1.0 (this article) or hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span></span> <span data-ttu-id="6248c-109">hello következő lépések azt ismertetik, hogyan toocreate például VM több IP-címek, hello forgatókönyvben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6248c-109">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="6248c-110">Módosítsa a változó nevét és IP cím típusát a végrehajtásához szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="6248c-110">Change variable names and IP address types as required for your implementation.</span></span>

1. <span data-ttu-id="6248c-111">Telepítse és konfigurálja a következő hello által az Azure CLI 1.0 lépések hello a hello [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a következő cikket, és jelentkezzen be az Azure-fiókjával hello `azure-login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6248c-111">Install and configure hello Azure CLI 1.0 by following hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account with hello `azure-login` command.</span></span>

2. <span data-ttu-id="6248c-112">Hozzon létre egy erőforráscsoportot:</span><span class="sxs-lookup"><span data-stu-id="6248c-112">Create a resource group:</span></span>
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. <span data-ttu-id="6248c-113">Virtuális hálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="6248c-113">Create a virtual network:</span></span>

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. <span data-ttu-id="6248c-114">A hello virtuális hálózati alhálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="6248c-114">Create a subnet in hello virtual network:</span></span>

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. <span data-ttu-id="6248c-115">Hozzon létre egy tárfiókot a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="6248c-115">Create  a storage account for hello VM.</span></span> <span data-ttu-id="6248c-116">Hello futtatása előtt a következő parancsban cserélje le *mystorageaccount* egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="6248c-116">Before running hello following command, replace *mystorageaccount* with a unique name.</span></span> <span data-ttu-id="6248c-117">hello nevének egyedinek kell lennie a Azure között:</span><span class="sxs-lookup"><span data-stu-id="6248c-117">hello name must be unique across Azure:</span></span>

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. <span data-ttu-id="6248c-118">Hozzon létre hello IP-konfigurációk, egy hálózati Adaptert, és rendelje a hello IP konfigurációk toohello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="6248c-118">Create hello IP configurations, a NIC, and assign hello IP configurations toohello NIC.</span></span> <span data-ttu-id="6248c-119">Hozzáadása, eltávolítása vagy szükség szerint hello konfigurációjának módosításához.</span><span class="sxs-lookup"><span data-stu-id="6248c-119">You can add, remove, or change hello configurations as necessary.</span></span> <span data-ttu-id="6248c-120">a következő konfigurációk hello hello forgatókönyvet ismerteti:</span><span class="sxs-lookup"><span data-stu-id="6248c-120">hello following configurations are described in hello scenario:</span></span>

    <span data-ttu-id="6248c-121">**IPConfig-1**</span><span class="sxs-lookup"><span data-stu-id="6248c-121">**IPConfig-1**</span></span>

    <span data-ttu-id="6248c-122">Adja meg a hello további parancsoknál, amelyek toocreate:</span><span class="sxs-lookup"><span data-stu-id="6248c-122">Enter hello commands that follow toocreate:</span></span>

    - <span data-ttu-id="6248c-123">A nyilvános IP-cím erőforrás egy statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="6248c-123">A public IP address resource with a static public IP address</span></span>
    - <span data-ttu-id="6248c-124">Egy hálózati Adaptert, hello nyilvános IP-cím és a statikus magánhálózati IP-cím tooit hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="6248c-124">A NIC, assigning hello public IP address and a static private IP address tooit.</span></span>
    
    <span data-ttu-id="6248c-125">Cserélje le *mypublicdns* hello Azure-beli hely egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="6248c-125">Replace *mypublicdns* with a name that is unique within hello Azure location.</span></span>

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > <span data-ttu-id="6248c-126">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="6248c-126">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="6248c-127">tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="6248c-127">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="6248c-128">Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="6248c-128">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="6248c-129">További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="6248c-129">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    <span data-ttu-id="6248c-130">**IPConfig-2**</span><span class="sxs-lookup"><span data-stu-id="6248c-130">**IPConfig-2**</span></span>

     <span data-ttu-id="6248c-131">Adja meg a következő parancsok toocreate hello, egy új nyilvános IP-cím erőforrás és egy új IP-konfiguráció egy statikus nyilvános IP-címet és egy statikus magánhálózati IP-cím:</span><span class="sxs-lookup"><span data-stu-id="6248c-131">Enter hello following commands toocreate a new public IP address resource and a new IP configuration with a static public IP address and a static private IP address:</span></span>
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    <span data-ttu-id="6248c-132">**IPConfig-3**</span><span class="sxs-lookup"><span data-stu-id="6248c-132">**IPConfig-3**</span></span>

    <span data-ttu-id="6248c-133">Adja meg a következő parancsok toocreate hozzá statikus magánhálózati IP-cím és a nyilvános IP-cím az IP-konfigurációt hello:</span><span class="sxs-lookup"><span data-stu-id="6248c-133">Enter hello following commands toocreate an IP configuration with a static private IP address and no public IP address:</span></span>

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    ><span data-ttu-id="6248c-134">Ez a cikk összes IP-konfigurációk tooa rendeli hozzá, ha egyetlen hálózati adapter, több IP-konfigurációk tooany egy virtuális hálózati adapter is hozzárendelhetők.</span><span class="sxs-lookup"><span data-stu-id="6248c-134">Though this article assigns all IP configurations tooa single NIC, you can also assign multiple IP configurations tooany NIC in a VM.</span></span> <span data-ttu-id="6248c-135">toolearn hogyan toocreate virtuális gép több hálózati adaptert, és olvasási hello létrehozása a cikk több hálózati adapterrel rendelkező virtuális gépeknél.</span><span class="sxs-lookup"><span data-stu-id="6248c-135">toolearn how toocreate a VM with multiple NICs, read hello Create a VM with multiple NICs article.</span></span>

7. <span data-ttu-id="6248c-136">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6248c-136">Create a Linux VM</span></span> 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    <span data-ttu-id="6248c-137">toochange hello mérete tooStandard DS2 v2 virtuális gép, például egyszerűen adja hozzá a következő tulajdonság hello `--vm-size Standard_DS3_v2` toohello `azure vm create` parancsot a 6.</span><span class="sxs-lookup"><span data-stu-id="6248c-137">toochange hello VM size tooStandard DS2 v2, for example, simply add hello following property `--vm-size Standard_DS3_v2` toohello `azure vm create` command in step 6.</span></span>

8. <span data-ttu-id="6248c-138">Adja meg a következő parancs tooview hello NIC hello és hello társított IP-konfigurációk:</span><span class="sxs-lookup"><span data-stu-id="6248c-138">Enter hello following command tooview hello NIC and hello associated IP configurations:</span></span>

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. <span data-ttu-id="6248c-139">Hozzáadás hello privát IP-címek toohello virtuális gép operációs rendszer hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="6248c-139">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="6248c-140"><a name="add"></a>Adja hozzá az IP-címek tooa méretű VM</span><span class="sxs-lookup"><span data-stu-id="6248c-140"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="6248c-141">Hozzáadhat további magán- és IP címeket tooan meglévő hálózati hello következő lépések végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="6248c-141">You can add additional private and public IP addresses tooan existing NIC by completing hello steps that follow.</span></span> <span data-ttu-id="6248c-142">hello példák épül hello [forgatókönyv](#Scenario) a cikkben.</span><span class="sxs-lookup"><span data-stu-id="6248c-142">hello examples build upon hello [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="6248c-143">Nyissa meg az Azure parancssori felület és a teljes hello hátralévő lépéseket ebben a szakaszban egy CLI-munkameneten belül.</span><span class="sxs-lookup"><span data-stu-id="6248c-143">Open Azure CLI and complete hello remaining steps in this section within a single CLI session.</span></span> <span data-ttu-id="6248c-144">Ha még nem rendelkezik Azure CLI telepítése és konfigurálása, teljes hello hello szükséges lépések [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a következő cikket, és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="6248c-144">If you don't already have Azure CLI installed and configured, complete hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account.</span></span>

2. <span data-ttu-id="6248c-145">Végezze el a következő szakaszok a követelmények alapján hello egyikében hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6248c-145">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    - <span data-ttu-id="6248c-146">**Adja hozzá a magánhálózati IP-cím**</span><span class="sxs-lookup"><span data-stu-id="6248c-146">**Add a private IP address**</span></span>
    
        <span data-ttu-id="6248c-147">egy privát IP-cím tooa NIC tooadd, létre kell hoznia hello parancsot az alábbi IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="6248c-147">tooadd a private IP address tooa NIC, you must create an IP configuration using hello command below.</span></span> <span data-ttu-id="6248c-148">hello statikus cím hello alhálózat nem használt címnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6248c-148">hello static address must be an unused address for hello subnet.</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        <span data-ttu-id="6248c-149">Tetszőleges számú konfigurációk összes kívánt beállítást, egyedi konfigurációs nevek és privát IP-címek használata (a statikus IP-címekkel rendelkező konfigurációk) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6248c-149">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    - <span data-ttu-id="6248c-150">**A nyilvános IP-cím hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="6248c-150">**Add a public IP address**</span></span>
    
        <span data-ttu-id="6248c-151">A nyilvános IP-cím való társítás tooeither által hozzáadott új IP-konfiguráció vagy a meglévő IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="6248c-151">A public IP address is added by associating it tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="6248c-152">Hajtsa végre hello egyik hello szakaszokban szereplő, az összes kívánt beállítást.</span><span class="sxs-lookup"><span data-stu-id="6248c-152">Complete hello steps in one of hello sections that follow, as you require.</span></span>

        > [!NOTE]
        > <span data-ttu-id="6248c-153">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="6248c-153">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="6248c-154">tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="6248c-154">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="6248c-155">Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="6248c-155">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="6248c-156">További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="6248c-156">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
        >

        <span data-ttu-id="6248c-157">**Hello erőforrás tooa új IP-konfiguráció hozzárendelése**</span><span class="sxs-lookup"><span data-stu-id="6248c-157">**Associate hello resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="6248c-158">Egy nyilvános IP-címet ad hozzá egy új IP-konfiguráció, amikor is hozzá kell magánhálózati IP-cím, mert az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="6248c-158">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="6248c-159">Egy meglévő nyilvános IP-cím erőforrás hozzáadása, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="6248c-159">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="6248c-160">toocreate egy új, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6248c-160">toocreate a new one, enter hello following command:</span></span>

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        <span data-ttu-id="6248c-161">egy új IP-beállítását egy statikus magánhálózati IP-cím és a kapcsolódó hello toocreate *myPublicIP3* nyilvános IP-cím erőforrás címet, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6248c-161">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIP3* public IP address resource, enter hello following command:</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        <span data-ttu-id="6248c-162">**Hello erőforrás tooan meglévő IP-konfiguráció hozzárendelése**</span><span class="sxs-lookup"><span data-stu-id="6248c-162">**Associate hello resource tooan existing IP configuration**</span></span>

        <span data-ttu-id="6248c-163">A nyilvános IP-cím erőforrás csak lehet társított tooan IP-konfigurációja, amely még nincs társítva.</span><span class="sxs-lookup"><span data-stu-id="6248c-163">A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="6248c-164">Segítségével meghatározhatja, hogy rendelkezik-e IP-konfigurációt tartozó nyilvános IP-cím hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="6248c-164">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        <span data-ttu-id="6248c-165">Egy sor hasonló toohello egy adott vissza a kimeneti hello az IPConfig-3 következő keresése:</span><span class="sxs-lookup"><span data-stu-id="6248c-165">Look for a line similar toohello one that follows for IPConfig-3 in hello returned output:</span></span>

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        <span data-ttu-id="6248c-166">Hello óta **nyilvános IP-cím** oszlopában *IpConfig-3* van üres, nem nyilvános IP-cím erőforrás jelenleg társított tooit.</span><span class="sxs-lookup"><span data-stu-id="6248c-166">Since hello **Public IP** column for *IpConfig-3* is blank, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="6248c-167">Adja hozzá egy meglévő nyilvános IP cím erőforrás tooIpConfig-3, vagy adja meg a következő parancs toocreate egy hello:</span><span class="sxs-lookup"><span data-stu-id="6248c-167">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        <span data-ttu-id="6248c-168">Adja meg a következő parancs tooassociate hello nyilvános IP-cím erőforrás toohello meglévő IP-konfiguráció hello *IPConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="6248c-168">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IPConfig-3*:</span></span>
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. <span data-ttu-id="6248c-169">Hello magánhálózati IP-címek megtekintése és hello nyilvános IP cím erőforrások hozzárendelt toohello megadásával NIC hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6248c-169">View hello private IP addresses and hello public IP address resources assigned toohello NIC by entering hello following command:</span></span>

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      <span data-ttu-id="6248c-170">hello visszaadott kimeneti hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="6248c-170">hello returned output is similar toohello following:</span></span>
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. <span data-ttu-id="6248c-171">Hello toohello NIC toohello virtuális gép operációs rendszer hello hello utasításait követve hozzáadott magánhálózati IP-címek hozzáadása [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="6248c-171">Add hello private IP addresses you added toohello NIC toohello VM operating system by following hello instructions in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="6248c-172">Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="6248c-172">Do not add hello public IP addresses toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
