---
title: "Virtuális gép több IP-címek az Azure CLI 1.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan több IP-címek hozzárendelése a virtuális gépek az Azure CLI 1.0 |} Erőforrás-kezelő."
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
ms.openlocfilehash: 9f085dfa1fe4db36d58cb976bb550a46bf241ac7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli-10"></a><span data-ttu-id="c613c-103">Több IP-címek hozzárendelése az Azure CLI 1.0 használata virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="c613c-103">Assign multiple IP addresses to virtual machines using Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="c613c-104">Ez a cikk ismerteti (VM) virtuális gép létrehozása az Azure Resource Manager deployment használatával az Azure CLI 1.0 használatával.</span><span class="sxs-lookup"><span data-stu-id="c613c-104">This article explains how to create a virtual machine (VM) through the Azure Resource Manager deployment model using the Azure CLI 1.0.</span></span> <span data-ttu-id="c613c-105">Több IP-cím nem lehet hozzárendelni a klasszikus üzembe helyezési modell használatával létrejött erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c613c-105">Multiple IP addresses cannot be assigned to resources created through the classic deployment model.</span></span> <span data-ttu-id="c613c-106">Az Azure üzembe helyezési modellel kapcsolatos további tudnivalókért olvassa el a [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c613c-106">To learn more about Azure deployment models, read the [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="c613c-107"><a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c613c-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="c613c-108">Hajthatja végre ezt a feladatot az Azure CLI 1.0 (Ez a cikk) vagy a [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c613c-108">You can complete this task using the Azure CLI 1.0 (this article) or the [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span></span> <span data-ttu-id="c613c-109">A következő lépések bemutatják, hogyan több IP-címmel, például virtuális gép létrehozásához a forgatókönyvben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="c613c-109">The steps that follow explain how to create an example VM with multiple IP addresses, as described in the scenario.</span></span> <span data-ttu-id="c613c-110">Módosítsa a változó nevét és IP cím típusát a végrehajtásához szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="c613c-110">Change variable names and IP address types as required for your implementation.</span></span>

1. <span data-ttu-id="c613c-111">Telepítheti és konfigurálhatja az Azure CLI 1.0 lépéseit a [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk, és jelentkezzen be a Azure fiók a `azure-login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c613c-111">Install and configure the Azure CLI 1.0 by following the steps in the [Install and Configure the Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account with the `azure-login` command.</span></span>

2. <span data-ttu-id="c613c-112">Hozzon létre egy erőforráscsoportot:</span><span class="sxs-lookup"><span data-stu-id="c613c-112">Create a resource group:</span></span>
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. <span data-ttu-id="c613c-113">Virtuális hálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="c613c-113">Create a virtual network:</span></span>

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. <span data-ttu-id="c613c-114">A virtuális hálózati alhálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="c613c-114">Create a subnet in the virtual network:</span></span>

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. <span data-ttu-id="c613c-115">Hozzon létre egy tárfiókot a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="c613c-115">Create  a storage account for the VM.</span></span> <span data-ttu-id="c613c-116">A következő parancs futtatása előtt cserélje le a *mystorageaccount* egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="c613c-116">Before running the following command, replace *mystorageaccount* with a unique name.</span></span> <span data-ttu-id="c613c-117">A nevének egyedinek kell lennie a Azure között:</span><span class="sxs-lookup"><span data-stu-id="c613c-117">The name must be unique across Azure:</span></span>

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. <span data-ttu-id="c613c-118">A hálózati adapter IP-konfigurációk létrehozása és az IP-konfiguráció hozzárendelése a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="c613c-118">Create the IP configurations, a NIC, and assign the IP configurations to the NIC.</span></span> <span data-ttu-id="c613c-119">Hozzáadása, eltávolítása vagy módosítsa a beállításokat, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="c613c-119">You can add, remove, or change the configurations as necessary.</span></span> <span data-ttu-id="c613c-120">A következő konfigurációk a forgatókönyv leírása olvasható:</span><span class="sxs-lookup"><span data-stu-id="c613c-120">The following configurations are described in the scenario:</span></span>

    <span data-ttu-id="c613c-121">**IPConfig-1**</span><span class="sxs-lookup"><span data-stu-id="c613c-121">**IPConfig-1**</span></span>

    <span data-ttu-id="c613c-122">Adja meg a további parancsoknál, amelyek létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="c613c-122">Enter the commands that follow to create:</span></span>

    - <span data-ttu-id="c613c-123">A nyilvános IP-cím erőforrás egy statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="c613c-123">A public IP address resource with a static public IP address</span></span>
    - <span data-ttu-id="c613c-124">Egy hálózati Adaptert, azt a nyilvános IP-cím és a hozzá statikus magánhálózati IP-cím hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="c613c-124">A NIC, assigning the public IP address and a static private IP address to it.</span></span>
    
    <span data-ttu-id="c613c-125">Cserélje le *mypublicdns* , amelynek az Azure-hely egyedi neve.</span><span class="sxs-lookup"><span data-stu-id="c613c-125">Replace *mypublicdns* with a name that is unique within the Azure location.</span></span>

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > <span data-ttu-id="c613c-126">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="c613c-126">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="c613c-127">IP-cím árazással kapcsolatos további tudnivalókért olvassa el a [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="c613c-127">To learn more about IP address pricing, read the [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="c613c-128">Nyilvános IP-címek egy előfizetésben használható száma korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="c613c-128">There is a limit to the number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="c613c-129">A korlátozásokkal kapcsolatos további információkért olvassa el az [Azure korlátairól](../azure-subscription-service-limits.md#networking-limits) szóló cikket.</span><span class="sxs-lookup"><span data-stu-id="c613c-129">To learn more about the limits, read the [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    <span data-ttu-id="c613c-130">**IPConfig-2**</span><span class="sxs-lookup"><span data-stu-id="c613c-130">**IPConfig-2**</span></span>

     <span data-ttu-id="c613c-131">Adja meg a következő parancsok futtatásával hozzon létre egy új nyilvános IP-cím erőforrás és egy új IP-konfiguráció egy statikus nyilvános IP-címet és egy statikus magánhálózati IP-cím:</span><span class="sxs-lookup"><span data-stu-id="c613c-131">Enter the following commands to create a new public IP address resource and a new IP configuration with a static public IP address and a static private IP address:</span></span>
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    <span data-ttu-id="c613c-132">**IPConfig-3**</span><span class="sxs-lookup"><span data-stu-id="c613c-132">**IPConfig-3**</span></span>

    <span data-ttu-id="c613c-133">Adja meg a következő parancsok futtatásával hozzon létre egy IP-konfiguráció hozzá statikus magánhálózati IP-cím és a nyilvános IP-cím:</span><span class="sxs-lookup"><span data-stu-id="c613c-133">Enter the following commands to create an IP configuration with a static private IP address and no public IP address:</span></span>

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    ><span data-ttu-id="c613c-134">Bár ez a cikk összes IP-konfigurációk rendel hozzá egyetlen hálózati adapter, is rendelhet több IP-konfigurációk a hálózati adapterek virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="c613c-134">Though this article assigns all IP configurations to a single NIC, you can also assign multiple IP configurations to any NIC in a VM.</span></span> <span data-ttu-id="c613c-135">Több hálózati adapterrel rendelkező virtuális gép létrehozása, olvassa el a hozza létre a cikk több hálózati adapterrel rendelkező virtuális gépeknél.</span><span class="sxs-lookup"><span data-stu-id="c613c-135">To learn how to create a VM with multiple NICs, read the Create a VM with multiple NICs article.</span></span>

7. <span data-ttu-id="c613c-136">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c613c-136">Create a Linux VM</span></span> 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    <span data-ttu-id="c613c-137">Módosítsa a Virtuálisgép-méretet szabványos DS2 v2, például egyszerűen adja hozzá a következő tulajdonság `--vm-size Standard_DS3_v2` számára a `azure vm create` parancsot a 6.</span><span class="sxs-lookup"><span data-stu-id="c613c-137">To change the VM size to Standard DS2 v2, for example, simply add the following property `--vm-size Standard_DS3_v2` to the `azure vm create` command in step 6.</span></span>

8. <span data-ttu-id="c613c-138">Adja meg a hálózati adapter és a társított IP-konfiguráció megtekintéséhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c613c-138">Enter the following command to view the NIC and the associated IP configurations:</span></span>

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. <span data-ttu-id="c613c-139">A privát IP-címek hozzáadása a virtuális gép operációs rendszer, az operációs rendszernek a lépések végrehajtásával a [hozzáadása IP-címek egy virtuális gép operációs rendszerre](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c613c-139">Add the private IP addresses to the VM operating system by completing the steps for your operating system in the [Add IP addresses to a VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="c613c-140"><a name="add"></a>IP-címek hozzáadása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="c613c-140"><a name="add"></a>Add IP addresses to a VM</span></span>

<span data-ttu-id="c613c-141">További privát és nyilvános IP-címeket adhat hozzá egy meglévő hálózati adapterre a következő lépések végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="c613c-141">You can add additional private and public IP addresses to an existing NIC by completing the steps that follow.</span></span> <span data-ttu-id="c613c-142">A példák épül a [forgatókönyv](#Scenario) a cikkben.</span><span class="sxs-lookup"><span data-stu-id="c613c-142">The examples build upon the [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="c613c-143">Nyissa meg az Azure parancssori felület, és hajtsa végre a fennmaradó lépéseit ebben a szakaszban egy CLI-munkameneten belül.</span><span class="sxs-lookup"><span data-stu-id="c613c-143">Open Azure CLI and complete the remaining steps in this section within a single CLI session.</span></span> <span data-ttu-id="c613c-144">Ha még nem rendelkezik Azure CLI telepítése és konfigurálása, hajtsa végre a a [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a következő cikket, és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c613c-144">If you don't already have Azure CLI installed and configured, complete the steps in the [Install and Configure the Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account.</span></span>

2. <span data-ttu-id="c613c-145">Hajtsa végre a következő részeiből, a követelmények alapján:</span><span class="sxs-lookup"><span data-stu-id="c613c-145">Complete the steps in one of the following sections, based on your requirements:</span></span>

    - <span data-ttu-id="c613c-146">**Adja hozzá a magánhálózati IP-cím**</span><span class="sxs-lookup"><span data-stu-id="c613c-146">**Add a private IP address**</span></span>
    
        <span data-ttu-id="c613c-147">A magánhálózati IP-cím hozzáadása a hálózati adapter, létre kell hoznia IP-konfigurációt az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="c613c-147">To add a private IP address to a NIC, you must create an IP configuration using the command below.</span></span> <span data-ttu-id="c613c-148">A statikus cím az alhálózat nem használt címnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c613c-148">The static address must be an unused address for the subnet.</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        <span data-ttu-id="c613c-149">Tetszőleges számú konfigurációk összes kívánt beállítást, egyedi konfigurációs nevek és privát IP-címek használata (a statikus IP-címekkel rendelkező konfigurációk) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c613c-149">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    - <span data-ttu-id="c613c-150">**A nyilvános IP-cím hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="c613c-150">**Add a public IP address**</span></span>
    
        <span data-ttu-id="c613c-151">A nyilvános IP-cím jelenik meg egy új IP-konfigurációt vagy egy meglévő IP-konfiguráció társításával.</span><span class="sxs-lookup"><span data-stu-id="c613c-151">A public IP address is added by associating it to either a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="c613c-152">Hajtsa végre a következő szakaszok, egyikében összes kívánt beállítást.</span><span class="sxs-lookup"><span data-stu-id="c613c-152">Complete the steps in one of the sections that follow, as you require.</span></span>

        > [!NOTE]
        > <span data-ttu-id="c613c-153">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="c613c-153">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="c613c-154">IP-cím árazással kapcsolatos további tudnivalókért olvassa el a [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="c613c-154">To learn more about IP address pricing, read the [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="c613c-155">Nyilvános IP-címek egy előfizetésben használható száma korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="c613c-155">There is a limit to the number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="c613c-156">A korlátozásokkal kapcsolatos további információkért olvassa el az [Azure korlátairól](../azure-subscription-service-limits.md#networking-limits) szóló cikket.</span><span class="sxs-lookup"><span data-stu-id="c613c-156">To learn more about the limits, read the [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
        >

        <span data-ttu-id="c613c-157">**Társítsa az erőforrást egy új IP-konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="c613c-157">**Associate the resource to a new IP configuration**</span></span>
    
        <span data-ttu-id="c613c-158">Egy nyilvános IP-címet ad hozzá egy új IP-konfiguráció, amikor is hozzá kell magánhálózati IP-cím, mert az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="c613c-158">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="c613c-159">Egy meglévő nyilvános IP-cím erőforrás hozzáadása, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="c613c-159">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="c613c-160">Hozzon létre egy újat, írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c613c-160">To create a new one, enter the following command:</span></span>

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        <span data-ttu-id="c613c-161">Új IP-konfiguráció létrehozása hozzá statikus magánhálózati IP-cím és a társított *myPublicIP3* nyilvános IP-cím erőforrás címet, adja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c613c-161">To create a new IP configuration with a static private IP address and the associated *myPublicIP3* public IP address resource, enter the following command:</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        <span data-ttu-id="c613c-162">**Társítsa az erőforrást egy meglévő IP-konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="c613c-162">**Associate the resource to an existing IP configuration**</span></span>

        <span data-ttu-id="c613c-163">A nyilvános IP-cím erőforrás csak lehet társítva, amely még nincs társított IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="c613c-163">A public IP address resource can only be associated to an IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="c613c-164">Segítségével meghatározhatja, hogy rendelkezik-e IP-konfigurációt tartozó nyilvános IP-cím a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="c613c-164">You can determine whether an IP configuration has an associated public IP address by entering the following command:</span></span>

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        <span data-ttu-id="c613c-165">Keresse meg az utánuk az IPConfig-3 visszaadott kimenet hasonló:</span><span class="sxs-lookup"><span data-stu-id="c613c-165">Look for a line similar to the one that follows for IPConfig-3 in the returned output:</span></span>

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        <span data-ttu-id="c613c-166">Mivel a **nyilvános IP-cím** oszlopában *IpConfig-3* van üres, nem nyilvános IP-cím erőforrás jelenleg társítva.</span><span class="sxs-lookup"><span data-stu-id="c613c-166">Since the **Public IP** column for *IpConfig-3* is blank, no public IP address resource is currently associated to it.</span></span> <span data-ttu-id="c613c-167">Egy meglévő nyilvános IP-cím erőforrás hozzáadása IpConfig-3, vagy hozzon létre egyet a következő parancsot írja be:</span><span class="sxs-lookup"><span data-stu-id="c613c-167">You can add an existing public IP address resource to IpConfig-3, or enter the following command to create one:</span></span>

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        <span data-ttu-id="c613c-168">Adja meg a következő parancsot a meglévő IP-konfiguráció a nyilvános IP-cím erőforrás hozzárendelni *IPConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="c613c-168">Enter the following command to associate the public IP address resource to the existing IP configuration named *IPConfig-3*:</span></span>
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. <span data-ttu-id="c613c-169">Tekintse meg a magánhálózati IP-címek és a nyilvános IP-cím erőforrás hozzárendelt hálózati adapter a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="c613c-169">View the private IP addresses and the public IP address resources assigned to the NIC by entering the following command:</span></span>

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      <span data-ttu-id="c613c-170">A visszaadott kimenete az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="c613c-170">The returned output is similar to the following:</span></span>
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. <span data-ttu-id="c613c-171">Adja hozzá a magánhálózati IP-címek utasításait követve hozzáadni a hálózati Adaptert a virtuális gép operációs rendszerre a [hozzáadása IP-címek egy virtuális gép operációs rendszerre](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c613c-171">Add the private IP addresses you added to the NIC to the VM operating system by following the instructions in the [Add IP addresses to a VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="c613c-172">Ne vegyen fel a nyilvános IP-címeket az operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="c613c-172">Do not add the public IP addresses to the operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
