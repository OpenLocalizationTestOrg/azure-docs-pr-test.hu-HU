---
title: "Betöltése kiegyenlítheti a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Az Azure load balancer használata magas rendelkezésre állású és biztonságos-alkalmazás létrehozásának három Linux virtuális gépek között"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 7b3a089d2f6386afcc46cbc4377594be0d758fc6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a><span data-ttu-id="c21f0-103">Betöltése kiegyenlítheti a Linux virtuális gépek magas rendelkezésre állású alkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c21f0-103">How to load balance Linux virtual machines in Azure to create a highly available application</span></span>
<span data-ttu-id="c21f0-104">Terheléselosztás biztosít a rendelkezésre állási magasabb szintű bejövő kérelmek elosztásával el több virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="c21f0-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="c21f0-105">Ebben az oktatóanyagban elsajátíthatja az Azure load balancer különböző összetevőit, ossza el a forgalmat, és magas rendelkezésre állás biztosításához.</span><span class="sxs-lookup"><span data-stu-id="c21f0-105">In this tutorial, you learn about the different components of the Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="c21f0-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="c21f0-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c21f0-107">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="c21f0-108">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="c21f0-109">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="c21f0-110">Egy alapszintű Node.js-alkalmazás létrehozásához használja a felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="c21f0-110">Use cloud-init to create a basic Node.js app</span></span>
> * <span data-ttu-id="c21f0-111">Virtuális gépek létrehozása és csatolása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-111">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="c21f0-112">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="c21f0-112">View a load balancer in action</span></span>
> * <span data-ttu-id="c21f0-113">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-113">Add and remove VMs from a load balancer</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c21f0-114">Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c21f0-114">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="c21f0-115">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="c21f0-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="c21f0-116">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c21f0-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="azure-load-balancer-overview"></a><span data-ttu-id="c21f0-117">Az Azure load balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="c21f0-117">Azure load balancer overview</span></span>
<span data-ttu-id="c21f0-118">Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="c21f0-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="c21f0-119">Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat egy operatív virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="c21f0-119">A load balancer health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

<span data-ttu-id="c21f0-120">Egy előtér-IP-konfigurációja, amely tartalmaz egy vagy több nyilvános IP-címeket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c21f0-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="c21f0-121">Az előtér-IP-konfiguráció lehetővé teszi, hogy a terheléselosztó és az alkalmazások elérhetők lesznek az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="c21f0-121">This front-end IP configuration allows your load balancer and applications to be accessible over the Internet.</span></span> 

<span data-ttu-id="c21f0-122">Virtuális gépek csatlakozni a virtuális hálózati kártya (NIC) használatával egy terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="c21f0-122">Virtual machines connect to a load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="c21f0-123">A virtuális gépek felé irányuló forgalom terjesztéséhez, egy háttér címkészletet tartalmaz az IP-cím, a virtuális (NIC) címét csatlakozik a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="c21f0-123">To distribute traffic to the VMs, a back-end address pool contains the IP addresses of the virtual (NICs) connected to the load balancer.</span></span>

<span data-ttu-id="c21f0-124">A forgalmat szabályozásához adhat meg terhelés terheléselosztó szabályt adott portok és protokollok, amelyek a virtuális gépekre van leképezve.</span><span class="sxs-lookup"><span data-stu-id="c21f0-124">To control the flow of traffic, you define load balancer rules for specific ports and protocols that map to your VMs.</span></span>

<span data-ttu-id="c21f0-125">Ha követte az előző oktatóanyag [hozzon létre egy virtuálisgép-méretezési csoport](tutorial-create-vmss.md), a terheléselosztó lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="c21f0-125">If you followed the previous tutorial to [create a virtual machine scale set](tutorial-create-vmss.md), a load balancer was created for you.</span></span> <span data-ttu-id="c21f0-126">Ezek az összetevők lettek konfigurálva, a méretezési csoport részeként.</span><span class="sxs-lookup"><span data-stu-id="c21f0-126">All these components were configured for you as part of the scale set.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="c21f0-127">Az Azure terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-127">Create Azure load balancer</span></span>
<span data-ttu-id="c21f0-128">Ez a szakasz részletesen, hogyan hozhat létre, és minden összetevője a terheléselosztó konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="c21f0-128">This section details how you can create and configure each component of the load balancer.</span></span> <span data-ttu-id="c21f0-129">A terheléselosztó létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-129">Before you can create your load balancer, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="c21f0-130">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupLoadBalancer* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="c21f0-130">The following example creates a resource group named *myResourceGroupLoadBalancer* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="c21f0-131">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="c21f0-131">Create a public IP address</span></span>
<span data-ttu-id="c21f0-132">Az alkalmazás az Internet eléréséhez a terheléselosztó szükség van egy nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="c21f0-132">To access your app on the Internet, you need a public IP address for the load balancer.</span></span> <span data-ttu-id="c21f0-133">Hozzon létre egy nyilvános IP-cím [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-133">Create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="c21f0-134">Az alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* a a *myResourceGroupLoadBalancer* erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="c21f0-134">The following example creates a public IP address named *myPublicIP* in the *myResourceGroupLoadBalancer* resource group:</span></span>

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="c21f0-135">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-135">Create a load balancer</span></span>
<span data-ttu-id="c21f0-136">Hozzon létre egy terheléselosztót, [az hálózati terheléselosztó létrehozása](/cli/azure/network/lb#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-136">Create a load balancer with [az network lb create](/cli/azure/network/lb#create).</span></span> <span data-ttu-id="c21f0-137">Az alábbi példa létrehoz egy terhelés-kiegyenlítő nevű *myLoadBalancer* és hozzárendeli a *myPublicIP* címét, hogy az előtér-IP-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="c21f0-137">The following example creates a load balancer named *myLoadBalancer* and assigns the *myPublicIP* address to the front-end IP configuration:</span></span>

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a><span data-ttu-id="c21f0-138">Hozzon létre egy állapotmintáihoz</span><span class="sxs-lookup"><span data-stu-id="c21f0-138">Create a health probe</span></span>
<span data-ttu-id="c21f0-139">Ahhoz, hogy a terheléselosztó a figyelheti az alkalmazás állapotát, használja a állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="c21f0-139">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="c21f0-140">A állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek állapotát ellenőrzi a válasz alapján terhelés terheléselosztó elforgatási.</span><span class="sxs-lookup"><span data-stu-id="c21f0-140">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="c21f0-141">Alapértelmezés szerint a virtuális gép törlődik a terheléselosztó terheléselosztási 15 másodperces időközönként két egymást követő hibák után.</span><span class="sxs-lookup"><span data-stu-id="c21f0-141">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="c21f0-142">Létrehozhat egy állapotmintáihoz protokoll vagy egy meghatározott állapottal ellenőrzése lapon, az alkalmazás alapján.</span><span class="sxs-lookup"><span data-stu-id="c21f0-142">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="c21f0-143">Az alábbi példa létrehoz egy TCP-Hálózatfigyelővel.</span><span class="sxs-lookup"><span data-stu-id="c21f0-143">The following example creates a TCP probe.</span></span> <span data-ttu-id="c21f0-144">Egyéni HTTP mintavételt további részletes állapotának ellenőrzésére is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="c21f0-144">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="c21f0-145">Ha egy egyéni HTTP-vizsgálatot, létre kell hoznia az állapot ellenőrzése lapon, például a *healthcheck.js*.</span><span class="sxs-lookup"><span data-stu-id="c21f0-145">When using a custom HTTP probe, you must create the health check page, such as *healthcheck.js*.</span></span> <span data-ttu-id="c21f0-146">A mintavétel kell visszaadnia egy **HTTP 200 OK** válasz megtartja a gazdagép Elforgatás a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="c21f0-146">The probe must return an **HTTP 200 OK** response for the load balancer to keep the host in rotation.</span></span>

<span data-ttu-id="c21f0-147">A TCP állapotmintáihoz létrehozásához használhatja [az hálózati lb mintavétel létrehozása](/cli/azure/network/lb/probe#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-147">To create a TCP health probe, you use [az network lb probe create](/cli/azure/network/lb/probe#create).</span></span> <span data-ttu-id="c21f0-148">Az alábbi példa létrehoz egy nevű állapotmintáihoz *myHealthProbe*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-148">The following example creates a health probe named *myHealthProbe*:</span></span>

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="c21f0-149">Hozzon létre olyan terheléselosztó szabályhoz</span><span class="sxs-lookup"><span data-stu-id="c21f0-149">Create a load balancer rule</span></span>
<span data-ttu-id="c21f0-150">Olyan terheléselosztó szabályhoz hogyan adatforgalom elosztása a virtuális gépek azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="c21f0-150">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span> <span data-ttu-id="c21f0-151">Megadhatja az előtér-IP-konfigurációjának megadása a bejövő forgalom és a háttér IP-címkészlet, valamint a megfelelő forrás és cél portot forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="c21f0-151">You define the front-end IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port.</span></span> <span data-ttu-id="c21f0-152">Győződjön meg arról, hogy csak a megfelelő virtuális gépek forgalom fogadására, hogy is megadhatja a használandó állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="c21f0-152">To make sure only healthy VMs receive traffic, you also define the health probe to use.</span></span>

<span data-ttu-id="c21f0-153">Hozzon létre olyan terheléselosztó szabályhoz rendelkező [az hálózati terheléselosztó szabály létrehozása](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-153">Create a load balancer rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="c21f0-154">Az alábbi példa létrehoz egy nevű szabályt *myLoadBalancerRule*, használja a *myHealthProbe* állapotmintáihoz és porton kiegyensúlyozza forgalom *80*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-154">The following example creates a rule named *myLoadBalancerRule*, uses the *myHealthProbe* health probe, and balances traffic on port *80*:</span></span>

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a><span data-ttu-id="c21f0-155">Virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c21f0-155">Configure virtual network</span></span>
<span data-ttu-id="c21f0-156">Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre a támogató virtuális hálózati erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c21f0-156">Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.</span></span> <span data-ttu-id="c21f0-157">Virtuális hálózatok kapcsolatos további információkért tekintse meg a [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c21f0-157">For more information about virtual networks, see the [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="c21f0-158">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-158">Create network resources</span></span>
<span data-ttu-id="c21f0-159">A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-159">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="c21f0-160">Az alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* nevű alhálózattal *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-160">The following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

<span data-ttu-id="c21f0-161">Hálózati biztonsági csoport hozzáadása, használhat [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-161">To add a network security group, you use [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="c21f0-162">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-162">The following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="c21f0-163">A hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-163">Create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="c21f0-164">Az alábbi példa létrehoz egy hálózati biztonsági szabály nevű *myNetworkSecurityGroupRule*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-164">The following example creates a network security group rule named *myNetworkSecurityGroupRule*:</span></span>

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

<span data-ttu-id="c21f0-165">Virtuális hálózati adapter jönnek létre [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-165">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="c21f0-166">Az alábbi példakód létrehozza a három virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="c21f0-166">The following example creates three virtual NICs.</span></span> <span data-ttu-id="c21f0-167">(Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre az alkalmazáshoz az alábbi lépéseket a).</span><span class="sxs-lookup"><span data-stu-id="c21f0-167">(One virtual NIC for each VM you create for your app in the following steps).</span></span> <span data-ttu-id="c21f0-168">További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és adja hozzá a terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="c21f0-168">You can create additional virtual NICs and VMs at any time and add them to the load balancer:</span></span>

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a><span data-ttu-id="c21f0-169">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-169">Create virtual machines</span></span>

### <a name="create-cloud-init-config"></a><span data-ttu-id="c21f0-170">Felhő inicializálás konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-170">Create cloud-init config</span></span>
<span data-ttu-id="c21f0-171">Az oktatóanyag előző [első indításakor Linux virtuális gépek testreszabása](tutorial-automate-vm-deployment.md), megtudta, hogyan automatizálható a felhő inicializálás a virtuális gép testreszabása.</span><span class="sxs-lookup"><span data-stu-id="c21f0-171">In a previous tutorial on [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how to automate VM customization with cloud-init.</span></span> <span data-ttu-id="c21f0-172">Az azonos felhő inicializálás konfigurációs fájl segítségével NGINX telepítheti és futtathatja egy egyszerű "Hello, World" Node.js alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c21f0-172">You can use the same cloud-init configuration file to install NGINX and run a simple 'Hello World' Node.js app.</span></span>

<span data-ttu-id="c21f0-173">Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* , majd illessze be a következő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="c21f0-173">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="c21f0-174">A felhő rendszerhéj nem a helyi számítógépen hozzon létre például a fájlt.</span><span class="sxs-lookup"><span data-stu-id="c21f0-174">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="c21f0-175">Adja meg `sensible-editor cloud-init.txt` hozza létre a fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="c21f0-175">Enter `sensible-editor cloud-init.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="c21f0-176">Győződjön meg arról, hogy az egész felhő inicializálás fájl megfelelően lett lemásolva különösen az első sor:</span><span class="sxs-lookup"><span data-stu-id="c21f0-176">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a><span data-ttu-id="c21f0-177">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-177">Create virtual machines</span></span>
<span data-ttu-id="c21f0-178">Az alkalmazás a magas rendelkezésre állás javítása érdekében helyezze a virtuális gépek rendelkezésre állási csoportba.</span><span class="sxs-lookup"><span data-stu-id="c21f0-178">To improve the high availability of your app, place your VMs in an availability set.</span></span> <span data-ttu-id="c21f0-179">További információ a rendelkezésre állási csoportok: a korábbi [magas rendelkezésre állású virtuális gépek létrehozása](tutorial-availability-sets.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c21f0-179">For more information about availability sets, see the previous [How to create highly available virtual machines](tutorial-availability-sets.md) tutorial.</span></span>

<span data-ttu-id="c21f0-180">Állítsa be a rendelkezésre állási létrehozása [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-180">Create an availability set with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="c21f0-181">Az alábbi példakód létrehozza a rendelkezésre állási készlet elnevezett *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-181">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

<span data-ttu-id="c21f0-182">Most a virtuális gépek is létrehozhat [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="c21f0-182">Now you can create the VMs with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c21f0-183">Az alábbi példában három virtuális gépeket hoz létre, és SSH-kulcsokat generál, ha még nem léteznek:</span><span class="sxs-lookup"><span data-stu-id="c21f0-183">The following example creates three VMs and generates SSH keys if they do not already exist:</span></span>

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

<span data-ttu-id="c21f0-184">Nincsenek háttérfeladatok, hogy végezze el az Azure parancssori felület visszatér a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="c21f0-184">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="c21f0-185">A `--no-wait` paraméter nem Várjon, amíg befejeződik a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="c21f0-185">The `--no-wait` parameter does not wait for all the tasks to complete.</span></span> <span data-ttu-id="c21f0-186">Elképzelhető, hogy egy másik néhány percet, mielőtt hozzáférhetne az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c21f0-186">It may be another couple of minutes before you can access the app.</span></span> <span data-ttu-id="c21f0-187">A load balancer állapotmintáihoz automatikusan észleli, ha az alkalmazás futtatása az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="c21f0-187">The load balancer health probe automatically detects when the app is running on each VM.</span></span> <span data-ttu-id="c21f0-188">A webalkalmazás működik, ha a terheléselosztó szabályhoz forgalom terjeszteni elindul.</span><span class="sxs-lookup"><span data-stu-id="c21f0-188">Once the app is running, the load balancer rule starts to distribute traffic.</span></span>


## <a name="test-load-balancer"></a><span data-ttu-id="c21f0-189">Teszt terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-189">Test load balancer</span></span>
<span data-ttu-id="c21f0-190">Szerezze be a terheléselosztó a nyilvános IP-címe [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="c21f0-190">Obtain the public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="c21f0-191">Az alábbi példa beolvassa az IP-címek *myPublicIP* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="c21f0-191">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="c21f0-192">Beírhatja a nyilvános IP-címet a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="c21f0-192">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="c21f0-193">Ne feledje - néhány percet vesz igénybe a készen álljon a terheléselosztó terjesztéséhez azokat felé irányuló forgalom megkezdése előtt a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="c21f0-193">Remember - it takes a few minutes the the VMs to be ready before the load balancer starts to distribute traffic to them.</span></span> <span data-ttu-id="c21f0-194">Az alkalmazás megjelenik, beleértve az állomásnevet, a virtuális gép, amelyek a terheléselosztó felé irányuló forgalom az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="c21f0-194">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Futó Node.js-alkalmazás](./media/tutorial-load-balancer/running-nodejs-app.png)

<span data-ttu-id="c21f0-196">Tekintse meg a terheléselosztó forgalom szét az alkalmazást futtató összes három virtuális gépet, akkor is kényszerített frissítési a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="c21f0-196">To see the load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="c21f0-197">Hozzá és távolíthat el a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="c21f0-197">Add and remove VMs</span></span>
<span data-ttu-id="c21f0-198">Szükség lehet a az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket karbantartásához.</span><span class="sxs-lookup"><span data-stu-id="c21f0-198">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="c21f0-199">Az alkalmazás megnövekedett forgalom kezelésére, szükség lehet további virtuális gépek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="c21f0-199">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="c21f0-200">Ez a szakasz bemutatja, hogyan távolítsa el, vagy adja hozzá a virtuális gépek a terheléselosztóról.</span><span class="sxs-lookup"><span data-stu-id="c21f0-200">This section shows you how to remove or add a VM from the load balancer.</span></span>

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="c21f0-201">A virtuális gép eltávolítása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-201">Remove a VM from the load balancer</span></span>
<span data-ttu-id="c21f0-202">A virtuális gép eltávolítása a háttér-címkészlet, amely [az hálózati hálózati adapter ip-config-címkészlet eltávolítása](/cli/azure/network/nic/ip-config/address-pool#remove).</span><span class="sxs-lookup"><span data-stu-id="c21f0-202">You can remove a VM from the backend address pool with [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove).</span></span> <span data-ttu-id="c21f0-203">A következő példában eltávolítjuk a virtuális hálózati Adapternek **myVM2** a *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-203">The following example removes the virtual NIC for **myVM2** from *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

<span data-ttu-id="c21f0-204">Tekintse meg a terheléselosztó elosztják a forgalom a fennmaradó két futó virtuális gépeket az alkalmazás akkor is kényszerített frissítési a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="c21f0-204">To see the load balancer distribute traffic across the remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="c21f0-205">Karbantartási is képes lemezvizsgálatok elvégzésére, hogy a virtuális Gépen, például az operációs rendszer frissítéseinek telepítése vagy a virtuális gép újraindítása végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="c21f0-205">You can now perform maintenance on the VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="c21f0-206">A virtuális gépek hozzáadása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-206">Add a VM to the load balancer</span></span>
<span data-ttu-id="c21f0-207">Virtuális gép karbantartásának végrehajtása után, vagy ha szüksége kapacitás bővítése céljából, adhat hozzá egy virtuális Gépet a háttér-címkészlet, amely [az hálózati hálózati adapter ip-config-címkészlet hozzáadása](/cli/azure/network/nic/ip-config/address-pool#add).</span><span class="sxs-lookup"><span data-stu-id="c21f0-207">After performing VM maintenance, or if you need to expand capacity, you can add a VM to the backend address pool with [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add).</span></span> <span data-ttu-id="c21f0-208">A következő példakóddal felveheti a virtuális hálózati Adapternek **myVM2** való *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="c21f0-208">The following example adds the virtual NIC for **myVM2** to *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a><span data-ttu-id="c21f0-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c21f0-209">Next steps</span></span>
<span data-ttu-id="c21f0-210">Ebben az oktatóanyagban egy terhelés-kiegyenlítő létrehozott és a virtuális gépek csatolva.</span><span class="sxs-lookup"><span data-stu-id="c21f0-210">In this tutorial, you created a load balancer and attached VMs to it.</span></span> <span data-ttu-id="c21f0-211">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="c21f0-211">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c21f0-212">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-212">Create an Azure load balancer</span></span>
> * <span data-ttu-id="c21f0-213">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-213">Create a load balancer health probe</span></span>
> * <span data-ttu-id="c21f0-214">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c21f0-214">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="c21f0-215">Egy alapszintű Node.js-alkalmazás létrehozásához használja a felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="c21f0-215">Use cloud-init to create a basic Node.js app</span></span>
> * <span data-ttu-id="c21f0-216">Virtuális gépek létrehozása és csatolása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-216">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="c21f0-217">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="c21f0-217">View a load balancer in action</span></span>
> * <span data-ttu-id="c21f0-218">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c21f0-218">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="c21f0-219">További információt az Azure virtuális hálózati összetevők a következő oktatóanyag továbblépés.</span><span class="sxs-lookup"><span data-stu-id="c21f0-219">Advance to the next tutorial to learn more about Azure virtual network components.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c21f0-220">Virtuális gépek és virtuális hálózatok kezelése</span><span class="sxs-lookup"><span data-stu-id="c21f0-220">Manage VMs and virtual networks</span></span>](tutorial-virtual-network.md)
