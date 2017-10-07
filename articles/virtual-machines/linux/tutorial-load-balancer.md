---
title: "aaaHow tooload egyenleg Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure betölteni a terheléselosztó toocreate egy magas rendelkezésre állású és biztonságos alkalmazás három Linux virtuális gépek között"
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
ms.openlocfilehash: f01752c3caec3489ee13e63000775769f3236e11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-linux-virtual-machines-in-azure-toocreate-a-highly-available-application"></a><span data-ttu-id="423bc-103">Hogyan tooload egyenleg Linux virtuális gépek Azure toocreate a magas rendelkezésre állású alkalmazások</span><span class="sxs-lookup"><span data-stu-id="423bc-103">How tooload balance Linux virtual machines in Azure toocreate a highly available application</span></span>
<span data-ttu-id="423bc-104">Terheléselosztás biztosít a rendelkezésre állási magasabb szintű bejövő kérelmek elosztásával el több virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="423bc-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="423bc-105">Ebben az oktatóanyagban elsajátíthatja hello különböző összetevőkről hello Azure terheléselosztó ossza el a forgalmat, és magas rendelkezésre állás biztosításához.</span><span class="sxs-lookup"><span data-stu-id="423bc-105">In this tutorial, you learn about hello different components of hello Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="423bc-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="423bc-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="423bc-107">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="423bc-108">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="423bc-109">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="423bc-110">Használja a felhő inicializálás toocreate egy alapszintű Node.js alkalmazást</span><span class="sxs-lookup"><span data-stu-id="423bc-110">Use cloud-init toocreate a basic Node.js app</span></span>
> * <span data-ttu-id="423bc-111">Virtuális gépek létrehozása és csatolása tooa terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-111">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="423bc-112">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="423bc-112">View a load balancer in action</span></span>
> * <span data-ttu-id="423bc-113">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-113">Add and remove VMs from a load balancer</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="423bc-114">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="423bc-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="423bc-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="423bc-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="423bc-116">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="423bc-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="azure-load-balancer-overview"></a><span data-ttu-id="423bc-117">Az Azure load balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="423bc-117">Azure load balancer overview</span></span>
<span data-ttu-id="423bc-118">Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="423bc-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="423bc-119">Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="423bc-119">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

<span data-ttu-id="423bc-120">Egy előtér-IP-konfigurációja, amely tartalmaz egy vagy több nyilvános IP-címeket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="423bc-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="423bc-121">Az előtér-IP-konfiguráció lehetővé teszi, hogy a load balancer és az alkalmazások toobe elérhető hello interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="423bc-121">This front-end IP configuration allows your load balancer and applications toobe accessible over hello Internet.</span></span> 

<span data-ttu-id="423bc-122">Virtuális gépek csatlakozni a virtuális hálózati kártya (NIC) használatával tooa terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="423bc-122">Virtual machines connect tooa load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="423bc-123">toodistribute forgalom toohello virtuális gépeket, egy háttér címkészletet hello IP-címek hello virtuális (NIC) csatlakoztatott toohello terheléselosztó tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="423bc-123">toodistribute traffic toohello VMs, a back-end address pool contains hello IP addresses of hello virtual (NICs) connected toohello load balancer.</span></span>

<span data-ttu-id="423bc-124">forgalom toocontrol hello áramló, megadhatja a terheléselosztási szabály az adott portok és protokollok, amelyek kapcsolódnak tooyour virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="423bc-124">toocontrol hello flow of traffic, you define load balancer rules for specific ports and protocols that map tooyour VMs.</span></span>

<span data-ttu-id="423bc-125">Ha túl követte az oktatóanyag előző hello[hozzon létre egy virtuálisgép-méretezési csoport](tutorial-create-vmss.md), a terheléselosztó lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="423bc-125">If you followed hello previous tutorial too[create a virtual machine scale set](tutorial-create-vmss.md), a load balancer was created for you.</span></span> <span data-ttu-id="423bc-126">Ezeket az összetevőket az Ön hello méretezési részeként lettek konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="423bc-126">All these components were configured for you as part of hello scale set.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="423bc-127">Az Azure terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-127">Create Azure load balancer</span></span>
<span data-ttu-id="423bc-128">Ez a szakasz részletesen, hogyan hozhat létre, és minden egyes összetevő hello terheléselosztó konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="423bc-128">This section details how you can create and configure each component of hello load balancer.</span></span> <span data-ttu-id="423bc-129">A terheléselosztó létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-129">Before you can create your load balancer, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="423bc-130">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupLoadBalancer* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="423bc-130">hello following example creates a resource group named *myResourceGroupLoadBalancer* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="423bc-131">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="423bc-131">Create a public IP address</span></span>
<span data-ttu-id="423bc-132">tooaccess rendszeren hello Internet, egy nyilvános IP-címet kell hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="423bc-132">tooaccess your app on hello Internet, you need a public IP address for hello load balancer.</span></span> <span data-ttu-id="423bc-133">Hozzon létre egy nyilvános IP-cím [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-133">Create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="423bc-134">hello alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* a hello *myResourceGroupLoadBalancer* erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="423bc-134">hello following example creates a public IP address named *myPublicIP* in hello *myResourceGroupLoadBalancer* resource group:</span></span>

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="423bc-135">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-135">Create a load balancer</span></span>
<span data-ttu-id="423bc-136">Hozzon létre egy terheléselosztót, [az hálózati terheléselosztó létrehozása](/cli/azure/network/lb#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-136">Create a load balancer with [az network lb create](/cli/azure/network/lb#create).</span></span> <span data-ttu-id="423bc-137">hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű *myLoadBalancer* és rendel hello *myPublicIP* cím toohello előtér-IP-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="423bc-137">hello following example creates a load balancer named *myLoadBalancer* and assigns hello *myPublicIP* address toohello front-end IP configuration:</span></span>

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a><span data-ttu-id="423bc-138">Hozzon létre egy állapotmintáihoz</span><span class="sxs-lookup"><span data-stu-id="423bc-138">Create a health probe</span></span>
<span data-ttu-id="423bc-139">tooallow hello terheléselosztó toomonitor hello állapotához az alkalmazás, használhat egy állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="423bc-139">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="423bc-140">hello állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek hello terhelés terheléselosztó Elforgatás a válasz toohealth ellenőrzések alapján.</span><span class="sxs-lookup"><span data-stu-id="423bc-140">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="423bc-141">Alapértelmezés szerint a virtuális gépek terheléselosztó terheléselosztási hello 15 másodperces időközönként két egymást követő hibák után törlődik.</span><span class="sxs-lookup"><span data-stu-id="423bc-141">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="423bc-142">Létrehozhat egy állapotmintáihoz protokoll vagy egy meghatározott állapottal ellenőrzése lapon, az alkalmazás alapján.</span><span class="sxs-lookup"><span data-stu-id="423bc-142">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="423bc-143">a következő példa hello egy TCP-Hálózatfigyelővel hoz létre.</span><span class="sxs-lookup"><span data-stu-id="423bc-143">hello following example creates a TCP probe.</span></span> <span data-ttu-id="423bc-144">Egyéni HTTP mintavételt további részletes állapotának ellenőrzésére is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="423bc-144">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="423bc-145">Ha egy egyéni HTTP-vizsgálatot, létre kell hoznia hello állapotának ellenőrzése lapon, például a *healthcheck.js*.</span><span class="sxs-lookup"><span data-stu-id="423bc-145">When using a custom HTTP probe, you must create hello health check page, such as *healthcheck.js*.</span></span> <span data-ttu-id="423bc-146">hello mintavételi kell visszaadnia egy **HTTP 200 OK** hello terhelés terheléselosztó tookeep hello állomás Elforgatás válasz.</span><span class="sxs-lookup"><span data-stu-id="423bc-146">hello probe must return an **HTTP 200 OK** response for hello load balancer tookeep hello host in rotation.</span></span>

<span data-ttu-id="423bc-147">a TCP állapotmintáihoz toocreate, használhat [az hálózati lb mintavétel létrehozása](/cli/azure/network/lb/probe#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-147">toocreate a TCP health probe, you use [az network lb probe create](/cli/azure/network/lb/probe#create).</span></span> <span data-ttu-id="423bc-148">hello alábbi példa létrehoz egy nevű állapotmintáihoz *myHealthProbe*:</span><span class="sxs-lookup"><span data-stu-id="423bc-148">hello following example creates a health probe named *myHealthProbe*:</span></span>

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="423bc-149">Hozzon létre olyan terheléselosztó szabályhoz</span><span class="sxs-lookup"><span data-stu-id="423bc-149">Create a load balancer rule</span></span>
<span data-ttu-id="423bc-150">Olyan terheléselosztó szabályhoz használt toodefine forgalom Mitől elosztott toohello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="423bc-150">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span> <span data-ttu-id="423bc-151">Hello előtér-IP-konfiguráció hello bejövő forgalom és hello háttér-IP-készlet tooreceive hello forgalom, valamint hello szükséges forrás- és a célport adhat meg.</span><span class="sxs-lookup"><span data-stu-id="423bc-151">You define hello front-end IP configuration for hello incoming traffic and hello back-end IP pool tooreceive hello traffic, along with hello required source and destination port.</span></span> <span data-ttu-id="423bc-152">a toomake meg arról, hogy csak a megfelelő virtuális gépek forgalom fogadására, is definiálhat hello állapotfigyelő mintavételi toouse.</span><span class="sxs-lookup"><span data-stu-id="423bc-152">toomake sure only healthy VMs receive traffic, you also define hello health probe toouse.</span></span>

<span data-ttu-id="423bc-153">Hozzon létre olyan terheléselosztó szabályhoz rendelkező [az hálózati terheléselosztó szabály létrehozása](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-153">Create a load balancer rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="423bc-154">hello alábbi példa létrehoz egy nevű szabályt *myLoadBalancerRule*, használ hello *myHealthProbe* állapotmintáihoz és porton kiegyensúlyozza forgalom *80*:</span><span class="sxs-lookup"><span data-stu-id="423bc-154">hello following example creates a rule named *myLoadBalancerRule*, uses hello *myHealthProbe* health probe, and balances traffic on port *80*:</span></span>

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


## <a name="configure-virtual-network"></a><span data-ttu-id="423bc-155">Virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="423bc-155">Configure virtual network</span></span>
<span data-ttu-id="423bc-156">Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre virtuális hálózati erőforrások támogató hello.</span><span class="sxs-lookup"><span data-stu-id="423bc-156">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="423bc-157">Virtuális hálózatok kapcsolatos további információkért lásd: hello [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="423bc-157">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="423bc-158">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-158">Create network resources</span></span>
<span data-ttu-id="423bc-159">A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-159">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="423bc-160">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* nevű alhálózattal *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="423bc-160">hello following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

<span data-ttu-id="423bc-161">használja a hálózati biztonsági csoport tooadd, [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-161">tooadd a network security group, you use [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="423bc-162">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="423bc-162">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="423bc-163">A hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-163">Create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="423bc-164">hello alábbi példa létrehoz egy hálózati biztonsági szabály nevű *myNetworkSecurityGroupRule*:</span><span class="sxs-lookup"><span data-stu-id="423bc-164">hello following example creates a network security group rule named *myNetworkSecurityGroupRule*:</span></span>

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

<span data-ttu-id="423bc-165">Virtuális hálózati adapter jönnek létre [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-165">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="423bc-166">hello alábbi példakód létrehozza három virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="423bc-166">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="423bc-167">(Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a az alkalmazást a következő hello lépések).</span><span class="sxs-lookup"><span data-stu-id="423bc-167">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="423bc-168">További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és azok hozzáadása toohello terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="423bc-168">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

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

## <a name="create-virtual-machines"></a><span data-ttu-id="423bc-169">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-169">Create virtual machines</span></span>

### <a name="create-cloud-init-config"></a><span data-ttu-id="423bc-170">Felhő inicializálás konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-170">Create cloud-init config</span></span>
<span data-ttu-id="423bc-171">Az oktatóanyag előző [hogyan toocustomize egy Linux virtuális gép első indításakor](tutorial-automate-vm-deployment.md), akkor megtanulta, hogyan tooautomate virtuális gép testreszabása a felhő inicializálás.</span><span class="sxs-lookup"><span data-stu-id="423bc-171">In a previous tutorial on [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with cloud-init.</span></span> <span data-ttu-id="423bc-172">Használhatja ugyanazon felhő inicializálás konfigurációs fájl tooinstall NGINX hello és egy egyszerű "Hello World" Node.js-alkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="423bc-172">You can use hello same cloud-init configuration file tooinstall NGINX and run a simple 'Hello World' Node.js app.</span></span>

<span data-ttu-id="423bc-173">Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* és a Beillesztés hello a következő konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="423bc-173">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="423bc-174">A felhő rendszerhéj hello nem a helyi számítógépen hozzon létre például hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="423bc-174">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="423bc-175">Adja meg `sensible-editor cloud-init.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="423bc-175">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="423bc-176">Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:</span><span class="sxs-lookup"><span data-stu-id="423bc-176">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

### <a name="create-virtual-machines"></a><span data-ttu-id="423bc-177">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-177">Create virtual machines</span></span>
<span data-ttu-id="423bc-178">tooimprove hello magas rendelkezésre állás az alkalmazás a virtuális gépeket helyez egy rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="423bc-178">tooimprove hello high availability of your app, place your VMs in an availability set.</span></span> <span data-ttu-id="423bc-179">Rendelkezésre állási készletek kapcsolatos további információkért lásd az előző hello [hogyan toocreate magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="423bc-179">For more information about availability sets, see hello previous [How toocreate highly available virtual machines](tutorial-availability-sets.md) tutorial.</span></span>

<span data-ttu-id="423bc-180">Állítsa be a rendelkezésre állási létrehozása [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-180">Create an availability set with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="423bc-181">hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="423bc-181">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

<span data-ttu-id="423bc-182">Most a virtuális gépek hello hozhat létre [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="423bc-182">Now you can create hello VMs with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="423bc-183">hello alábbi példa létrehoz három virtuális gépek és SSH-kulcsokat generál, ha még nem léteznek:</span><span class="sxs-lookup"><span data-stu-id="423bc-183">hello following example creates three VMs and generates SSH keys if they do not already exist:</span></span>

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

<span data-ttu-id="423bc-184">Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés.</span><span class="sxs-lookup"><span data-stu-id="423bc-184">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="423bc-185">Hello `--no-wait` paraméter nem várja meg az összes hello feladatok toocomplete.</span><span class="sxs-lookup"><span data-stu-id="423bc-185">hello `--no-wait` parameter does not wait for all hello tasks toocomplete.</span></span> <span data-ttu-id="423bc-186">Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="423bc-186">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="423bc-187">hello terheléselosztó automatikusan észleli, ha egyes virtuális gépek hello az alkalmazás fut..</span><span class="sxs-lookup"><span data-stu-id="423bc-187">hello load balancer health probe automatically detects when hello app is running on each VM.</span></span> <span data-ttu-id="423bc-188">Miután hello alkalmazás fut, a hello terheléselosztási szabály elindít toodistribute forgalmat.</span><span class="sxs-lookup"><span data-stu-id="423bc-188">Once hello app is running, hello load balancer rule starts toodistribute traffic.</span></span>


## <a name="test-load-balancer"></a><span data-ttu-id="423bc-189">Teszt terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-189">Test load balancer</span></span>
<span data-ttu-id="423bc-190">Hello nyilvános IP-cím a terheléselosztó az beszerzése [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="423bc-190">Obtain hello public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="423bc-191">hello alábbi példa beszerzi hello IP-címet *myPublicIP* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="423bc-191">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="423bc-192">Majd tooa webböngészőben hello nyilvános IP-címet adhat meg.</span><span class="sxs-lookup"><span data-stu-id="423bc-192">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="423bc-193">Ne feledje, mert néhány perc hello hello virtuális gépek toobe készen áll a szükséges hello terheléselosztó toodistribute forgalom toothem megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="423bc-193">Remember - it takes a few minutes hello hello VMs toobe ready before hello load balancer starts toodistribute traffic toothem.</span></span> <span data-ttu-id="423bc-194">hello alkalmazásokról, beleértve a virtuális gép hello hello állomásnevét adott hello terheléselosztó forgalom tooas hello a következő példa az elosztott:</span><span class="sxs-lookup"><span data-stu-id="423bc-194">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Futó Node.js-alkalmazás](./media/tutorial-load-balancer/running-nodejs-app.png)

<span data-ttu-id="423bc-196">toosee hello terheléselosztó forgalom szét az alkalmazást futtató összes három virtuális gépet, a kényszerített-frissítési a webböngésző is.</span><span class="sxs-lookup"><span data-stu-id="423bc-196">toosee hello load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="423bc-197">Hozzá és távolíthat el a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="423bc-197">Add and remove VMs</span></span>
<span data-ttu-id="423bc-198">Szükség lehet az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket hello tooperform karbantartása.</span><span class="sxs-lookup"><span data-stu-id="423bc-198">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="423bc-199">toodeal megnövekedett forgalom tooyour alkalmazással, szükség lehet tooadd további virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="423bc-199">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="423bc-200">Ez a szakasz bemutatja, hogyan tooremove, vagy adja hozzá a virtuális gépek hello terheléselosztóról.</span><span class="sxs-lookup"><span data-stu-id="423bc-200">This section shows you how tooremove or add a VM from hello load balancer.</span></span>

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="423bc-201">Távolítsa el a virtuális gépek hello terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-201">Remove a VM from hello load balancer</span></span>
<span data-ttu-id="423bc-202">A virtuális gép eltávolítása hello háttér-címkészlet, amely [az hálózati hálózati adapter ip-config-címkészlet eltávolítása](/cli/azure/network/nic/ip-config/address-pool#remove).</span><span class="sxs-lookup"><span data-stu-id="423bc-202">You can remove a VM from hello backend address pool with [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove).</span></span> <span data-ttu-id="423bc-203">a következő példa eltávolítja hello hello virtuális hálózati Adapternek **myVM2** a *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="423bc-203">hello following example removes hello virtual NIC for **myVM2** from *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

<span data-ttu-id="423bc-204">toosee hello terheléselosztó forgalom szét a másik két hello az alkalmazást futtató virtuális gépek akkor is kényszerített frissítési a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="423bc-204">toosee hello load balancer distribute traffic across hello remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="423bc-205">Karbantartási is képes lemezvizsgálatok elvégzésére, hogy a virtuális gép, például az operációs rendszer frissítéseinek telepítése vagy a virtuális gép újraindítása végrehajtása hello.</span><span class="sxs-lookup"><span data-stu-id="423bc-205">You can now perform maintenance on hello VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="423bc-206">Virtuális gép toohello terheléselosztó felvétele</span><span class="sxs-lookup"><span data-stu-id="423bc-206">Add a VM toohello load balancer</span></span>
<span data-ttu-id="423bc-207">Virtuális gép karbantartásának végrehajtása után, vagy ha tooexpand kapacitás, hozzáadhat egy virtuális gép toohello háttér-címkészlet, amely [az hálózati hálózati adapter ip-config-címkészlet hozzáadása](/cli/azure/network/nic/ip-config/address-pool#add).</span><span class="sxs-lookup"><span data-stu-id="423bc-207">After performing VM maintenance, or if you need tooexpand capacity, you can add a VM toohello backend address pool with [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add).</span></span> <span data-ttu-id="423bc-208">hello következő példakóddal felveheti a hello virtuális hálózati Adapternek **myVM2** túl*myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="423bc-208">hello following example adds hello virtual NIC for **myVM2** too*myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a><span data-ttu-id="423bc-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="423bc-209">Next steps</span></span>
<span data-ttu-id="423bc-210">Ebben az oktatóanyagban egy terhelés-kiegyenlítő létrehozott, és csatolt virtuális gépek tooit.</span><span class="sxs-lookup"><span data-stu-id="423bc-210">In this tutorial, you created a load balancer and attached VMs tooit.</span></span> <span data-ttu-id="423bc-211">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="423bc-211">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="423bc-212">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-212">Create an Azure load balancer</span></span>
> * <span data-ttu-id="423bc-213">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-213">Create a load balancer health probe</span></span>
> * <span data-ttu-id="423bc-214">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="423bc-214">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="423bc-215">Használja a felhő inicializálás toocreate egy alapszintű Node.js alkalmazást</span><span class="sxs-lookup"><span data-stu-id="423bc-215">Use cloud-init toocreate a basic Node.js app</span></span>
> * <span data-ttu-id="423bc-216">Virtuális gépek létrehozása és csatolása tooa terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-216">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="423bc-217">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="423bc-217">View a load balancer in action</span></span>
> * <span data-ttu-id="423bc-218">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="423bc-218">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="423bc-219">Előzetes toohello következő útmutató toolearn további információk az Azure-beli virtuális hálózat összetevői.</span><span class="sxs-lookup"><span data-stu-id="423bc-219">Advance toohello next tutorial toolearn more about Azure virtual network components.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="423bc-220">Virtuális gépek és virtuális hálózatok kezelése</span><span class="sxs-lookup"><span data-stu-id="423bc-220">Manage VMs and virtual networks</span></span>](tutorial-virtual-network.md)
