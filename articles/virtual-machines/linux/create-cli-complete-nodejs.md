---
title: "Hozzon létre egy teljes körű Linux környezetet az Azure CLI 1.0 |} Microsoft Docs"
description: "Tárolási, Linux virtuális gép, egy virtuális hálózati és alhálózati, egy adott terheléselosztóhoz, egy olyan hálózati adapter, egy nyilvános IP-cím és a hálózati biztonsági csoport, az Azure CLI 1.0 használatával alapoktól összes létrehozása."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 201ccd523e49d638ace50fbc0ffdceb705b35473
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-complete-linux-environment-with-the-azure-cli-10"></a><span data-ttu-id="3d35e-103">Hozzon létre egy teljes körű Linux környezetet az Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="3d35e-103">Create a complete Linux environment with the Azure CLI 1.0</span></span>
<span data-ttu-id="3d35e-104">Ebben a cikkben azt a terheléselosztó és a virtuális gépek, amelyek hasznosak a fejlesztési és egyszerű számítástechnikai két egyszerű hálózat felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3d35e-104">In this article, we build a simple network with a load balancer and a pair of VMs that are useful for development and simple computing.</span></span> <span data-ttu-id="3d35e-105">Azt ismerteti a folyamatot, a parancs által parancs, amíg a két üzemel, biztonságos Linux virtuális gép, amelyre lehet csatlakoztatni bárhol az interneten.</span><span class="sxs-lookup"><span data-stu-id="3d35e-105">We walk through the process command by command, until you have two working, secure Linux VMs to which you can connect from anywhere on the Internet.</span></span> <span data-ttu-id="3d35e-106">Majd továbbléphet összetettebb hálózatokhoz és a környezetben.</span><span class="sxs-lookup"><span data-stu-id="3d35e-106">Then you can move on to more complex networks and environments.</span></span>

<span data-ttu-id="3d35e-107">Menet elsajátíthatja függőségi hierarchiája, hogy a Resource Manager üzembe helyezési modellel lehetővé teszi, és mekkora kapcsolatos kapcsolja be biztosít.</span><span class="sxs-lookup"><span data-stu-id="3d35e-107">Along the way, you learn about the dependency hierarchy that the Resource Manager deployment model gives you, and about how much power it provides.</span></span> <span data-ttu-id="3d35e-108">Miután meggyőződött arról, hogyan épül fel a rendszer, újjáépíthető azt sokkal gyorsabban használatával [Azure Resource Manager-sablonok](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d35e-108">After you see how the system is built, you can rebuild it much more quickly by using [Azure Resource Manager templates](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="3d35e-109">Emellett után megtudhatja, hogyan működnek a környezetet részei együtt, automatizálhatják is azokat-sablonok létrehozása kezdve egyszerűbbé válik.</span><span class="sxs-lookup"><span data-stu-id="3d35e-109">Also, after you learn how the parts of your environment fit together, creating templates to automate them becomes easier.</span></span>

<span data-ttu-id="3d35e-110">A környezet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="3d35e-110">The environment contains:</span></span>

* <span data-ttu-id="3d35e-111">Két virtuális gépek rendelkezésre állási csoportok belül.</span><span class="sxs-lookup"><span data-stu-id="3d35e-111">Two VMs inside an availability set.</span></span>
* <span data-ttu-id="3d35e-112">Terheléselosztó terheléselosztási szabállyal 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="3d35e-112">A load balancer with a load-balancing rule on port 80.</span></span>
* <span data-ttu-id="3d35e-113">Hálózati biztonsági csoport (NSG) szabályok a nemkívánatos forgalom a virtuális gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="3d35e-113">Network security group (NSG) rules to protect your VM from unwanted traffic.</span></span>

<span data-ttu-id="3d35e-114">Az egyéni környezet létrehozása a legújabb kell [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erőforrás-kezelő módban (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="3d35e-114">To create this custom environment, you need the latest [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Resource Manager mode (`azure config mode arm`).</span></span> <span data-ttu-id="3d35e-115">Egy eszköz elemzése JSON is kell.</span><span class="sxs-lookup"><span data-stu-id="3d35e-115">You also need a JSON parsing tool.</span></span> <span data-ttu-id="3d35e-116">Ez a példa [jq](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="3d35e-116">This example uses [jq](https://stedolan.github.io/jq/).</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="3d35e-117">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="3d35e-117">CLI versions to complete the task</span></span>
<span data-ttu-id="3d35e-118">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="3d35e-118">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="3d35e-119">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="3d35e-119">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="3d35e-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="3d35e-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="3d35e-121">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="3d35e-121">Quick commands</span></span>
<span data-ttu-id="3d35e-122">Ha szeretné gyorsan a feladatnak a, a következő szakasz részleteit a következő parancsokat egy virtuális Gépet az Azure-bA feltölteni.</span><span class="sxs-lookup"><span data-stu-id="3d35e-122">If you need to quickly accomplish the task, the following section details the base commands to upload a VM to Azure.</span></span> <span data-ttu-id="3d35e-123">Részletes információkat és a környezetben az egyes lépéseit itt található: a dokumentum többi része a, kezdési [Itt](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="3d35e-123">More detailed information and context for each step can be found in the rest of the document, starting [here](#detailed-walkthrough).</span></span>

<span data-ttu-id="3d35e-124">Győződjön meg arról, hogy rendelkezik [az Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="3d35e-124">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="3d35e-125">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="3d35e-125">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="3d35e-126">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-126">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

<span data-ttu-id="3d35e-127">Az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3d35e-127">Create the resource group.</span></span> <span data-ttu-id="3d35e-128">Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `westeurope` helye:</span><span class="sxs-lookup"><span data-stu-id="3d35e-128">The following example creates a resource group named `myResourceGroup` in the `westeurope` location:</span></span>

```azurecli
azure group create -n myResourceGroup -l westeurope
```

<span data-ttu-id="3d35e-129">Az erőforráscsoport ellenőrizze a JSON-elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="3d35e-129">Verify the resource group by using the JSON parser:</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="3d35e-130">A storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-130">Create the storage account.</span></span> <span data-ttu-id="3d35e-131">Az alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-131">The following example creates a storage account named `mystorageaccount`.</span></span> <span data-ttu-id="3d35e-132">(A tárfiók nevét kell egyedinek lennie, ezért adja meg a saját egyedi nevét.)</span><span class="sxs-lookup"><span data-stu-id="3d35e-132">(The storage account name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

<span data-ttu-id="3d35e-133">Ellenőrizze a tárfiók a JSON-elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="3d35e-133">Verify the storage account by using the JSON parser:</span></span>

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

<span data-ttu-id="3d35e-134">Hozza létre a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="3d35e-134">Create the virtual network.</span></span> <span data-ttu-id="3d35e-135">Az alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-135">The following example creates a virtual network named `myVnet`:</span></span>

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

<span data-ttu-id="3d35e-136">Hozzon létre egy alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="3d35e-136">Create a subnet.</span></span> <span data-ttu-id="3d35e-137">Az alábbi példakód létrehozza nevű alhálózat `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-137">The following example creates a subnet named `mySubnet`:</span></span>

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

<span data-ttu-id="3d35e-138">Ellenőrizze a virtuális hálózati és alhálózati a JSON-elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="3d35e-138">Verify the virtual network and subnet by using the JSON parser:</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="3d35e-139">Hozzon létre egy nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="3d35e-139">Create a public IP.</span></span> <span data-ttu-id="3d35e-140">Az alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP` a DNS-beli nevű `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-140">The following example creates a public IP named `myPublicIP` with the DNS name of `mypublicdns`.</span></span> <span data-ttu-id="3d35e-141">(A DNS-nevét kell egyedinek lennie, így rendelkeznek a saját egyedi nevet.)</span><span class="sxs-lookup"><span data-stu-id="3d35e-141">(The DNS name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

<span data-ttu-id="3d35e-142">A terheléselosztó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-142">Create the load balancer.</span></span> <span data-ttu-id="3d35e-143">Az alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-143">The following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

<span data-ttu-id="3d35e-144">Hozzon létre egy előtér-IP-címkészletet a terheléselosztóhoz, és társítsa a nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="3d35e-144">Create a front-end IP pool for the load balancer, and associate the public IP.</span></span> <span data-ttu-id="3d35e-145">Az alábbi példa létrehoz egy előtér-IP-címkészletet nevű `mySubnetPool`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-145">The following example creates a front-end IP pool named `mySubnetPool`:</span></span>

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

<span data-ttu-id="3d35e-146">Hozzon létre a háttér IP-címkészlet a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="3d35e-146">Create the back-end IP pool for the load balancer.</span></span> <span data-ttu-id="3d35e-147">Az alábbi példakód létrehozza a háttér IP-címkészlet nevű `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-147">The following example creates a back-end IP pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

<span data-ttu-id="3d35e-148">Hozzon létre SSH bejövő hálózati cím címfordítási (NAT) szabályait a terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="3d35e-148">Create SSH inbound network address translation (NAT) rules for the load balancer.</span></span> <span data-ttu-id="3d35e-149">Az alábbi példa létrehoz két terheléselosztási szabály, `myLoadBalancerRuleSSH1` és `myLoadBalancerRuleSSH2`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-149">The following example creates two load balancer rules, `myLoadBalancerRuleSSH1` and `myLoadBalancerRuleSSH2`:</span></span>

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

<span data-ttu-id="3d35e-150">A webalkalmazás létrehozása bejövő NAT-szabályok a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="3d35e-150">Create the web inbound NAT rules for the load balancer.</span></span> <span data-ttu-id="3d35e-151">Az alábbi példa létrehoz egy terheléselosztási szabály nevű `myLoadBalancerRuleWeb`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-151">The following example creates a load balancer rule named `myLoadBalancerRuleWeb`:</span></span>

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

<span data-ttu-id="3d35e-152">Az állapotfigyelő terheléselosztói mintavétel létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-152">Create the load balancer health probe.</span></span> <span data-ttu-id="3d35e-153">Az alábbi példa létrehoz egy TCP-Hálózatfigyelővel nevű `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-153">The following example creates a TCP probe named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

<span data-ttu-id="3d35e-154">Ellenőrizze a terheléselosztó IP-készletek és NAT-szabályok a JSON-elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="3d35e-154">Verify the load balancer, IP pools, and NAT rules by using the JSON parser:</span></span>

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

<span data-ttu-id="3d35e-155">Hozzon létre az első hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="3d35e-155">Create the first network interface card (NIC).</span></span> <span data-ttu-id="3d35e-156">Cserélje le a `#####-###-###` szakaszok a saját Azure-előfizetés-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="3d35e-156">Replace the `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="3d35e-157">Az előfizetés-azonosító jelenik meg a kimeneti **jq** az erőforrások létrehozásakor vizsgálata során.</span><span class="sxs-lookup"><span data-stu-id="3d35e-157">Your subscription ID is noted in the output of **jq** when you examine the resources you are creating.</span></span> <span data-ttu-id="3d35e-158">Megtekintheti az előfizetés-Azonosítóval rendelkező `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-158">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="3d35e-159">Az alábbi példa létrehoz egy hálózati adapter nevű `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-159">The following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

<span data-ttu-id="3d35e-160">Hozzon létre a második hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="3d35e-160">Create the second NIC.</span></span> <span data-ttu-id="3d35e-161">Az alábbi példa létrehoz egy hálózati adapter nevű `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-161">The following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

<span data-ttu-id="3d35e-162">Ellenőrizze a két hálózati adaptert a JSON-elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="3d35e-162">Verify the two NICs by using the JSON parser:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

<span data-ttu-id="3d35e-163">A hálózati biztonsági csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3d35e-163">Create the network security group.</span></span> <span data-ttu-id="3d35e-164">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-164">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

<span data-ttu-id="3d35e-165">Két bejövő szabály a hálózati biztonsági csoport hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-165">Add two inbound rules for the network security group.</span></span> <span data-ttu-id="3d35e-166">Az alábbi példa létrehoz két szabály `myNetworkSecurityGroupRuleSSH` és `myNetworkSecurityGroupRuleHTTP`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-166">The following example creates two rules, `myNetworkSecurityGroupRuleSSH` and `myNetworkSecurityGroupRuleHTTP`:</span></span>

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

<span data-ttu-id="3d35e-167">Ellenőrizze a hálózati biztonsági csoport és a bejövő szabályok a JSON-elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="3d35e-167">Verify the network security group and inbound rules by using the JSON parser:</span></span>

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

<span data-ttu-id="3d35e-168">A két hálózati adapter hálózati biztonsági csoport kötést létrehozni:</span><span class="sxs-lookup"><span data-stu-id="3d35e-168">Bind the network security group to the two NICs:</span></span>

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

<span data-ttu-id="3d35e-169">A rendelkezésre állási csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-169">Create the availability set.</span></span> <span data-ttu-id="3d35e-170">Az alábbi példakód létrehozza a rendelkezésre állási készlet elnevezett `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-170">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

<span data-ttu-id="3d35e-171">Az első Linux virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-171">Create the first Linux VM.</span></span> <span data-ttu-id="3d35e-172">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM1`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-172">The following example creates a VM named `myVM1`:</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

<span data-ttu-id="3d35e-173">A második Linux virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-173">Create the second Linux VM.</span></span> <span data-ttu-id="3d35e-174">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM2`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-174">The following example creates a VM named `myVM2`:</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

<span data-ttu-id="3d35e-175">Használja a JSON-elemző ellenőrizze, hogy minden, ami készült:</span><span class="sxs-lookup"><span data-stu-id="3d35e-175">Use the JSON parser to verify that everything that was built:</span></span>

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

<span data-ttu-id="3d35e-176">Az új környezet gyorsan újból létrehozni új példányok sablon exportálása:</span><span class="sxs-lookup"><span data-stu-id="3d35e-176">Export your new environment to a template to quickly re-create new instances:</span></span>

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="3d35e-177">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="3d35e-177">Detailed walkthrough</span></span>
<span data-ttu-id="3d35e-178">A részletes következő lépések azt ismertetik, minden parancs tevékenységétől ki a környezet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-178">The detailed steps that follow explain what each command is doing as you build out your environment.</span></span> <span data-ttu-id="3d35e-179">Ezek a következők hasznos a saját egyéni fejlesztési és éles környezeteket összeállításakor.</span><span class="sxs-lookup"><span data-stu-id="3d35e-179">These concepts are helpful when you build your own custom environments for development or production.</span></span>

<span data-ttu-id="3d35e-180">Győződjön meg arról, hogy rendelkezik [az Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="3d35e-180">Make sure that you have [the Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="3d35e-181">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="3d35e-181">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="3d35e-182">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-182">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

## <a name="create-resource-groups-and-choose-deployment-locations"></a><span data-ttu-id="3d35e-183">Erőforráscsoport-sablonok létrehozása és központi telepítési helyek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="3d35e-183">Create resource groups and choose deployment locations</span></span>
<span data-ttu-id="3d35e-184">Azure erőforráscsoport-sablonok, amelyek tartalmazzák a beállítási adatokat és a metaadatok a logikai központi erőforrás-kezelés engedélyezése a logikai telepítési entitások.</span><span class="sxs-lookup"><span data-stu-id="3d35e-184">Azure resource groups are logical deployment entities that contain configuration information and metadata to enable the logical management of resource deployments.</span></span> <span data-ttu-id="3d35e-185">Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `westeurope` helye:</span><span class="sxs-lookup"><span data-stu-id="3d35e-185">The following example creates a resource group named `myResourceGroup` in the `westeurope` location:</span></span>

```azurecli
azure group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="3d35e-186">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-186">Output:</span></span>

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a><span data-ttu-id="3d35e-187">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="3d35e-187">Create a storage account</span></span>
<span data-ttu-id="3d35e-188">A virtuális gép lemezeit, majd bármelyik adatlemeznek, amelyeket meg szeretne adni a storage-fiókok szükség van.</span><span class="sxs-lookup"><span data-stu-id="3d35e-188">You need storage accounts for your VM disks and for any additional data disks that you want to add.</span></span> <span data-ttu-id="3d35e-189">Szinte azonnal után létrehozhat erőforráscsoportok storage-fiókokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="3d35e-189">You create storage accounts almost immediately after you create resource groups.</span></span>

<span data-ttu-id="3d35e-190">Jelen példában használjuk a `azure storage account create` parancsot, és a kívánt tárolási támogatás típusa átadja a fiók, az erőforráscsoport helye, amely szabályozza.</span><span class="sxs-lookup"><span data-stu-id="3d35e-190">Here we use the `azure storage account create` command, passing the location of the account, the resource group that controls it, and the type of storage support you want.</span></span> <span data-ttu-id="3d35e-191">Az alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-191">The following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

<span data-ttu-id="3d35e-192">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-192">Output:</span></span>

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

<span data-ttu-id="3d35e-193">Az erőforráscsoport segítségével megvizsgálhatja a `azure group show` paranccsal, most használja az [jq](https://stedolan.github.io/jq/) eszközzel együtt a `--json` Azure CLI lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3d35e-193">To examine our resource group by using the `azure group show` command, let's use the [jq](https://stedolan.github.io/jq/) tool along with the `--json` Azure CLI option.</span></span> <span data-ttu-id="3d35e-194">(Használható **jsawk** vagy a JSON elemezni szeretné nyelvi függvénytárat.)</span><span class="sxs-lookup"><span data-stu-id="3d35e-194">(You can use **jsawk** or any language library you prefer to parse the JSON.)</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="3d35e-195">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-195">Output:</span></span>

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

<span data-ttu-id="3d35e-196">A parancssori felület használatával vizsgálja meg a tárfiók, először a fióknevek és a kulcsok beállításához.</span><span class="sxs-lookup"><span data-stu-id="3d35e-196">To investigate the storage account by using the CLI, you first need to set the account names and keys.</span></span> <span data-ttu-id="3d35e-197">Cserélje le egy nevet az Ön által a következő példa a tárfiók nevét:</span><span class="sxs-lookup"><span data-stu-id="3d35e-197">Replace the name of the storage account in the following example with a name that you choose:</span></span>

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

<span data-ttu-id="3d35e-198">Ezután egyszerűen megtekintheti a tárolással kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="3d35e-198">Then you can view your storage information easily:</span></span>

```azurecli
azure storage container list
```

<span data-ttu-id="3d35e-199">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-199">Output:</span></span>

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="3d35e-200">Hozzon létre egy virtuális hálózat és alhálózat</span><span class="sxs-lookup"><span data-stu-id="3d35e-200">Create a virtual network and subnet</span></span>
<span data-ttu-id="3d35e-201">Ezután fog létre kell hoznia az Azure-ban és a virtuális gépeket hozhat létre alhálózat futtató virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="3d35e-201">Next you're going to need to create a virtual network running in Azure and a subnet in which you can create your VMs.</span></span> <span data-ttu-id="3d35e-202">Az alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` rendelkező a `192.168.0.0/16` címelőtag:</span><span class="sxs-lookup"><span data-stu-id="3d35e-202">The following example creates a virtual network named `myVnet` with the `192.168.0.0/16` address prefix:</span></span>

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="3d35e-203">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-203">Output:</span></span>

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

<span data-ttu-id="3d35e-204">Ebben az esetben használja most--json teszi a `azure group show` és `jq` hogyan dolgozunk az erőforrások megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3d35e-204">Again, let's use the --json option of `azure group show` and `jq` to see how we're building our resources.</span></span> <span data-ttu-id="3d35e-205">Most már rendelkezik egy `storageAccounts` erőforrás és a `virtualNetworks` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3d35e-205">We now have a `storageAccounts` resource and a `virtualNetworks` resource.</span></span>  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="3d35e-206">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-206">Output:</span></span>

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

<span data-ttu-id="3d35e-207">Most hozzuk létre az alhálózatot a `myVnet` virtuális hálózat, amelybe a virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="3d35e-207">Now let's create a subnet in the `myVnet` virtual network into which the VMs are deployed.</span></span> <span data-ttu-id="3d35e-208">Használjuk a `azure network vnet subnet create` parancsot, és az erőforrások már létrehoztunk: a `myResourceGroup` erőforráscsoport és a `myVnet` virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="3d35e-208">We use the `azure network vnet subnet create` command, along with the resources we've already created: the `myResourceGroup` resource group and the `myVnet` virtual network.</span></span> <span data-ttu-id="3d35e-209">A következő példában azt nevű alhálózat hozzáadása `mySubnet` az alhálózati cím előtaggal `192.168.1.0/24`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-209">In the following example, we add the subnet named `mySubnet` with the subnet address prefix of `192.168.1.0/24`:</span></span>

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

<span data-ttu-id="3d35e-210">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-210">Output:</span></span>

```azurecli
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

<span data-ttu-id="3d35e-211">Mivel az alhálózat logikailag a virtuális hálózaton belül, azt keresse meg az alhálózati adatokat egy némileg eltérő parancs.</span><span class="sxs-lookup"><span data-stu-id="3d35e-211">Because the subnet is logically inside the virtual network, we look for the subnet information with a slightly different command.</span></span> <span data-ttu-id="3d35e-212">A parancs az használjuk `azure network vnet show`, de továbbra is, hogy ellenőrizze a JSON-kimenetét a `jq`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-212">The command we use is `azure network vnet show`, but we continue to examine the JSON output by using `jq`.</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="3d35e-213">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-213">Output:</span></span>

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a><span data-ttu-id="3d35e-214">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="3d35e-214">Create a public IP address</span></span>
<span data-ttu-id="3d35e-215">Most hozzuk létre az a nyilvános IP-cím (PIP), azt hozzárendeli a terheléselosztó hasonló adataival.</span><span class="sxs-lookup"><span data-stu-id="3d35e-215">Now let's create the public IP address (PIP) that we assign to your load balancer.</span></span> <span data-ttu-id="3d35e-216">Ez lehetővé teszi, hogy a virtuális gépek az internetről használatával kapcsolódik a `azure network public-ip create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3d35e-216">It enables you to connect to your VMs from the Internet by using the `azure network public-ip create` command.</span></span> <span data-ttu-id="3d35e-217">Mivel az alapértelmezett cím dinamikus, létrehozhatunk egy elnevezett DNS-bejegyzést a **cloudapp.azure.com** tartomány használatával a `--domain-name-label` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3d35e-217">Because the default address is dynamic, we create a named DNS entry in the **cloudapp.azure.com** domain by using the `--domain-name-label` option.</span></span> <span data-ttu-id="3d35e-218">Az alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP` a DNS-beli nevű `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-218">The following example creates a public IP named `myPublicIP` with the DNS name of `mypublicdns`.</span></span> <span data-ttu-id="3d35e-219">A DNS-nevének egyedinek kell lennie, mert a saját egyedi DNS-nevet adja meg:</span><span class="sxs-lookup"><span data-stu-id="3d35e-219">Because the DNS name must be unique, you provide your own unique DNS name:</span></span>

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

<span data-ttu-id="3d35e-220">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-220">Output:</span></span>

```azurecli
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

<span data-ttu-id="3d35e-221">A nyilvános IP-cím egyben legfelső szintű erőforrás, így tekintheti meg a `azure group show`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-221">The public IP address is also a top-level resource, so you can see it with `azure group show`.</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="3d35e-222">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-222">Output:</span></span>

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

<span data-ttu-id="3d35e-223">Használatával megvizsgálhatja a több erőforrás adatait, többek között a altartomány teljesen minősített tartománynevét (FQDN) használatával a teljes `azure network public-ip show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3d35e-223">You can investigate more resource details, including the fully qualified domain name (FQDN) of the subdomain, by using the complete `azure network public-ip show` command.</span></span> <span data-ttu-id="3d35e-224">A nyilvános IP-cím erőforrás logikailag van rendelve, de egy adott cím még nincs hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="3d35e-224">The public IP address resource has been allocated logically, but a specific address has not yet been assigned.</span></span> <span data-ttu-id="3d35e-225">Az IP-címének megszerzéséhez fog kell egy adott terheléselosztóhoz, amely jelenleg még nem hozott létre.</span><span class="sxs-lookup"><span data-stu-id="3d35e-225">To obtain an IP address, you're going to need a load balancer, which we have not yet created.</span></span>

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

<span data-ttu-id="3d35e-226">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-226">Output:</span></span>

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a><span data-ttu-id="3d35e-227">Hozzon létre egy terheléselosztó és IP-készletek</span><span class="sxs-lookup"><span data-stu-id="3d35e-227">Create a load balancer and IP pools</span></span>
<span data-ttu-id="3d35e-228">Amikor létrehoz egy terhelés-kiegyenlítő, lehetővé teszi forgalom szét több virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="3d35e-228">When you create a load balancer, it enables you to distribute traffic across multiple VMs.</span></span> <span data-ttu-id="3d35e-229">Az alkalmazás redundanciájának több virtuális gép karbantartás vagy túl nagy terhelés esetén a felhasználói kérésekre válaszolnak futtatásával is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3d35e-229">It also provides redundancy to your application by running multiple VMs that respond to user requests in the event of maintenance or heavy loads.</span></span> <span data-ttu-id="3d35e-230">Az alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-230">The following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

<span data-ttu-id="3d35e-231">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-231">Output:</span></span>

```azurecli
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

<span data-ttu-id="3d35e-232">A terheléselosztó viszonylag üres, ezért az egyes IP-címkészletek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-232">Our load balancer is fairly empty, so let's create some IP pools.</span></span> <span data-ttu-id="3d35e-233">Azt szeretnénk, a terheléselosztó, egyet az előtér és a hátteret két IP-címkészletek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-233">We want to create two IP pools for our load balancer, one for the front end and one for the back end.</span></span> <span data-ttu-id="3d35e-234">Az előtér-IP-címkészlet nyilvánosan láthatóvá.</span><span class="sxs-lookup"><span data-stu-id="3d35e-234">The front-end IP pool is publicly visible.</span></span> <span data-ttu-id="3d35e-235">A helyet, amelyhez azt rendelje hozzá a korábban létrehozott PIP egyben.</span><span class="sxs-lookup"><span data-stu-id="3d35e-235">It's also the location to which we assign the PIP that we created earlier.</span></span> <span data-ttu-id="3d35e-236">Majd használjuk a háttér-készlet olyan hely, a virtuális gépekhez való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="3d35e-236">Then we use the back-end pool as a location for our VMs to connect to.</span></span> <span data-ttu-id="3d35e-237">Ily módon a áramolhasson az adatforgalom a terheléselosztó a virtuális gépek keresztül.</span><span class="sxs-lookup"><span data-stu-id="3d35e-237">That way, the traffic can flow through the load balancer to the VMs.</span></span>

<span data-ttu-id="3d35e-238">Először hozzuk létre az előtér-IP-címkészletet.</span><span class="sxs-lookup"><span data-stu-id="3d35e-238">First, let's create our front-end IP pool.</span></span> <span data-ttu-id="3d35e-239">Az alábbi példa létrehoz egy előtér-készlet nevű `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-239">The following example creates a front-end pool named `myFrontEndPool`:</span></span>

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

<span data-ttu-id="3d35e-240">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-240">Output:</span></span>

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

<span data-ttu-id="3d35e-241">Vegye figyelembe, hogyan használtuk az `--public-ip-name` felelt meg a kapcsoló a `myPublicIP` , amely a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3d35e-241">Note how we used the `--public-ip-name` switch to pass in the `myPublicIP` that we created earlier.</span></span> <span data-ttu-id="3d35e-242">A terheléselosztó a nyilvános IP-cím hozzárendelése lehetővé teszi a virtuális gépek eléréséhez az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="3d35e-242">Assigning the public IP address to the load balancer allows you to reach your VMs across the Internet.</span></span>

<span data-ttu-id="3d35e-243">Következő lépésként hozzon létre a második IP-címkészlet, ezúttal a háttér-forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="3d35e-243">Next, let's create our second IP pool, this time for our back-end traffic.</span></span> <span data-ttu-id="3d35e-244">Az alábbi példa létrehoz egy háttér címkészletet nevű `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-244">The following example creates a back-end pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

<span data-ttu-id="3d35e-245">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-245">Output:</span></span>

```azurecli
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

<span data-ttu-id="3d35e-246">A képen látható, hogyan ennek során a terheléselosztó azáltal, hogy megtekinti a `azure network lb show` és a JSON-kimenetét vizsgálata:</span><span class="sxs-lookup"><span data-stu-id="3d35e-246">We can see how our load balancer is doing by looking with `azure network lb show` and examining the JSON output:</span></span>

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

<span data-ttu-id="3d35e-247">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-247">Output:</span></span>

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a><span data-ttu-id="3d35e-248">Terheléselosztó NAT-szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d35e-248">Create load balancer NAT rules</span></span>
<span data-ttu-id="3d35e-249">Ahhoz, hogy a terheléselosztó átfutó forgalom, kell hálózati címet adja meg a bejövő vagy kimenő műveletek címfordítási (NAT) szabályok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3d35e-249">To get traffic flowing through our load balancer, we need to create network address translation (NAT) rules that specify either inbound or outbound actions.</span></span> <span data-ttu-id="3d35e-250">Adja meg a használni kívánt protokollt, majd leképezik a külső tetszés szerint belső portjaihoz.</span><span class="sxs-lookup"><span data-stu-id="3d35e-250">You can specify the protocol to use, then map external ports to internal ports as desired.</span></span> <span data-ttu-id="3d35e-251">A környezet hozzuk létre egyes szabályokat, amelyek lehetővé teszik az SSH a terheléselosztó a virtuális gépek keresztül.</span><span class="sxs-lookup"><span data-stu-id="3d35e-251">For our environment, let's create some rules that allow SSH through our load balancer to our VMs.</span></span> <span data-ttu-id="3d35e-252">Beállítjuk 4222 és 4223 TCP-port a 22-es TCP-portot a virtuális gépeken (amely később létrehozhatunk) irányítani.</span><span class="sxs-lookup"><span data-stu-id="3d35e-252">We set up TCP ports 4222 and 4223 to direct to TCP port 22 on our VMs (which we create later).</span></span> <span data-ttu-id="3d35e-253">Az alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleSSH1` TCP-port 4222 hozzárendelése 22-es portot:</span><span class="sxs-lookup"><span data-stu-id="3d35e-253">The following example creates a rule named `myLoadBalancerRuleSSH1` to map TCP port 4222 to port 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

<span data-ttu-id="3d35e-254">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-254">Output:</span></span>

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

<span data-ttu-id="3d35e-255">Ismételje meg az eljárást a második NAT-szabály az SSH.</span><span class="sxs-lookup"><span data-stu-id="3d35e-255">Repeat the procedure for your second NAT rule for SSH.</span></span> <span data-ttu-id="3d35e-256">Az alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleSSH2` TCP-port 4223 hozzárendelése 22-es portot:</span><span class="sxs-lookup"><span data-stu-id="3d35e-256">The following example creates a rule named `myLoadBalancerRuleSSH2` to map TCP port 4223 to port 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

<span data-ttu-id="3d35e-257">Most is használatától és a webes forgalomban, a szabály csatlakoztatás az IP-készletek legfeljebb 80-as TCP-port NAT-szabályt.</span><span class="sxs-lookup"><span data-stu-id="3d35e-257">Let's also go ahead and create a NAT rule for TCP port 80 for web traffic, hooking the rule up to our IP pools.</span></span> <span data-ttu-id="3d35e-258">Ha azt a számítógéphez a szabály, amely egy IP-készletet, helyett a szabályt, hogy a virtuális gépek egyenkénti csatlakoztatása a Microsoft adja hozzá, vagy távolítsa el a virtuális gépek IP-készletből.</span><span class="sxs-lookup"><span data-stu-id="3d35e-258">If we hook up the rule to an IP pool, instead of hooking up the rule to our VMs individually, we can add or remove VMs from the IP pool.</span></span> <span data-ttu-id="3d35e-259">A terheléselosztó automatikusan módosítja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="3d35e-259">The load balancer automatically adjusts the flow of traffic.</span></span> <span data-ttu-id="3d35e-260">Az alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleWeb` 80-as TCP-port hozzárendelését a 80-as port:</span><span class="sxs-lookup"><span data-stu-id="3d35e-260">The following example creates a rule named `myLoadBalancerRuleWeb` to map TCP port 80 to port 80:</span></span>

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

<span data-ttu-id="3d35e-261">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-261">Output:</span></span>

```azurecli
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a><span data-ttu-id="3d35e-262">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d35e-262">Create a load balancer health probe</span></span>
<span data-ttu-id="3d35e-263">A health mintavételi rendszeresen ellenőrzi a ellenőrizze, hogy most működik és válaszol a kérelmekre, meghatározott a terheléselosztó mögötti virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="3d35e-263">A health probe periodically checks on the VMs that are behind our load balancer to make sure they're operating and responding to requests as defined.</span></span> <span data-ttu-id="3d35e-264">Ha nem, most eltávolítja a biztonsági művelet, győződjön meg arról, hogy felhasználók nem irányítja őket.</span><span class="sxs-lookup"><span data-stu-id="3d35e-264">If not, they're removed from operation to ensure that users aren't being directed to them.</span></span> <span data-ttu-id="3d35e-265">Megadhatja a állapotmintáihoz, valamint időközök és az időkorlát-értékei egyéni keres.</span><span class="sxs-lookup"><span data-stu-id="3d35e-265">You can define custom checks for the health probe, along with intervals and timeout values.</span></span> <span data-ttu-id="3d35e-266">Állapotteljesítmény kapcsolatos további információkért lásd: [terheléselosztó vizsgálat](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d35e-266">For more information about health probes, see [Load Balancer probes](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="3d35e-267">Az alábbi példa létrehoz egy TCP állapotfigyelő megadott elnevezett `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-267">The following example creates a TCP health probed named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

<span data-ttu-id="3d35e-268">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-268">Output:</span></span>

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

<span data-ttu-id="3d35e-269">Itt azt az állapot-ellenőrzési eredményeire a 15 másodperces időközönként megadott.</span><span class="sxs-lookup"><span data-stu-id="3d35e-269">Here, we specified an interval of 15 seconds for our health checks.</span></span> <span data-ttu-id="3d35e-270">Azt is, amelyeken legfeljebb négy mintavételt (egy perces) a terheléselosztó úgy véli, hogy a gazdagép már nem működik.</span><span class="sxs-lookup"><span data-stu-id="3d35e-270">We can miss a maximum of four probes (one minute) before the load balancer considers that the host is no longer functioning.</span></span>

## <a name="verify-the-load-balancer"></a><span data-ttu-id="3d35e-271">Ellenőrizze a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="3d35e-271">Verify the load balancer</span></span>
<span data-ttu-id="3d35e-272">Most már a terheléselosztó-konfiguráció történik.</span><span class="sxs-lookup"><span data-stu-id="3d35e-272">Now the load balancer configuration is done.</span></span> <span data-ttu-id="3d35e-273">A lépéseket, a következők:</span><span class="sxs-lookup"><span data-stu-id="3d35e-273">Here are the steps you took:</span></span>

1. <span data-ttu-id="3d35e-274">Létrehozott egy terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="3d35e-274">You created a load balancer.</span></span>
2. <span data-ttu-id="3d35e-275">Létrehozott egy előtér-IP-címkészletet, és egy nyilvános IP-címet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="3d35e-275">You created a front-end IP pool and assigned a public IP to it.</span></span>
3. <span data-ttu-id="3d35e-276">Létrehozott egy háttér-IP-címkészlet, amely a virtuális gépek csatlakozni tud-e.</span><span class="sxs-lookup"><span data-stu-id="3d35e-276">You created a back-end IP pool that VMs can connect to.</span></span>
4. <span data-ttu-id="3d35e-277">NAT-szabályok, amelyek lehetővé teszik az SSH-kapcsolatot a virtuális gépek kezelésére, egy szabályt, amely lehetővé teszi, hogy a 80-as TCP-port a webalkalmazás együtt létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3d35e-277">You created NAT rules that allow SSH to the VMs for management, along with a rule that allows TCP port 80 for our web app.</span></span>
5. <span data-ttu-id="3d35e-278">Hozzáadta a rendszeres időnként ellenőrzik a virtuális gép állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="3d35e-278">You added a health probe to periodically check the VMs.</span></span> <span data-ttu-id="3d35e-279">A állapotmintáihoz biztosítja, hogy felhasználók ne próbáljon meg hozzáférni egy virtuális Gépet, amely már nem működik, vagy tartalom.</span><span class="sxs-lookup"><span data-stu-id="3d35e-279">This health probe ensures that users don't try to access a VM that is no longer functioning or serving content.</span></span>

<span data-ttu-id="3d35e-280">A terheléselosztó néz most tekintsük át:</span><span class="sxs-lookup"><span data-stu-id="3d35e-280">Let's review what your load balancer looks like now:</span></span>

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

<span data-ttu-id="3d35e-281">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-281">Output:</span></span>

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a><span data-ttu-id="3d35e-282">Egy olyan hálózati adapter használata a Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d35e-282">Create an NIC to use with the Linux VM</span></span>
<span data-ttu-id="3d35e-283">Hálózati adapter, ezért programozott módon való használatukat alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="3d35e-283">NICs are programmatically available because you can apply rules to their use.</span></span> <span data-ttu-id="3d35e-284">Egynél több is lehet.</span><span class="sxs-lookup"><span data-stu-id="3d35e-284">You can also have more than one.</span></span> <span data-ttu-id="3d35e-285">Az alábbi `azure network nic create` parancs, a hálózati adapter a terhelés háttér IP-készlethez a számítógéphez, és társítsa azt az SSH-forgalom engedélyezése a NAT-szabályt.</span><span class="sxs-lookup"><span data-stu-id="3d35e-285">In the following `azure network nic create` command, you hook up the NIC to the load back-end IP pool and associate it with the NAT rule to permit SSH traffic.</span></span>

<span data-ttu-id="3d35e-286">Cserélje le a `#####-###-###` szakaszok a saját Azure-előfizetés-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="3d35e-286">Replace the `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="3d35e-287">Az előfizetés-azonosító jelenik meg a kimeneti `jq` az erőforrások létrehozásakor vizsgálata során.</span><span class="sxs-lookup"><span data-stu-id="3d35e-287">Your subscription ID is noted in the output of `jq` when you examine the resources you are creating.</span></span> <span data-ttu-id="3d35e-288">Megtekintheti az előfizetés-Azonosítóval rendelkező `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-288">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="3d35e-289">Az alábbi példa létrehoz egy hálózati adapter nevű `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-289">The following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

<span data-ttu-id="3d35e-290">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-290">Output:</span></span>

```azurecli
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

<span data-ttu-id="3d35e-291">A részletek a erőforrás közvetlenül megvizsgálásával.</span><span class="sxs-lookup"><span data-stu-id="3d35e-291">You can see the details by examining the resource directly.</span></span> <span data-ttu-id="3d35e-292">Az erőforrás használatával vizsgálja meg a `azure network nic show` parancs:</span><span class="sxs-lookup"><span data-stu-id="3d35e-292">You examine the resource by using the `azure network nic show` command:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

<span data-ttu-id="3d35e-293">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-293">Output:</span></span>

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

<span data-ttu-id="3d35e-294">Most már tudjuk létrehozni a második hálózati adapter, csatlakoztatás a ismét a a háttér IP-címkészletet.</span><span class="sxs-lookup"><span data-stu-id="3d35e-294">Now we create the second NIC, hooking in to our back-end IP pool again.</span></span> <span data-ttu-id="3d35e-295">Ez a második alkalommal NAT-szabály lehetővé teszi az SSH-forgalom.</span><span class="sxs-lookup"><span data-stu-id="3d35e-295">This time the second NAT rule permits SSH traffic.</span></span> <span data-ttu-id="3d35e-296">Az alábbi példa létrehoz egy hálózati adapter nevű `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-296">The following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a><span data-ttu-id="3d35e-297">Hálózati biztonsági csoport és a szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d35e-297">Create a network security group and rules</span></span>
<span data-ttu-id="3d35e-298">Most azt hozzon létre egy hálózati biztonsági csoport és a hálózati hozzáférés szabályozásához, a bejövő szabályok</span><span class="sxs-lookup"><span data-stu-id="3d35e-298">Now we create a network security group and the inbound rules that govern access to the NIC.</span></span> <span data-ttu-id="3d35e-299">Hálózati biztonsági csoport egy hálózati adapter vagy az alhálózat alkalmazhatók.</span><span class="sxs-lookup"><span data-stu-id="3d35e-299">A network security group can be applied to a NIC or subnet.</span></span> <span data-ttu-id="3d35e-300">Szabályok, hogy a forgalmat a virtuális gépek mindkét definiálni.</span><span class="sxs-lookup"><span data-stu-id="3d35e-300">You define rules to control the flow of traffic in and out of your VMs.</span></span> <span data-ttu-id="3d35e-301">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-301">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

<span data-ttu-id="3d35e-302">A bejövő szabályt az NSG-t (SSH támogatásához) 22-es portot a bejövő kapcsolatok engedélyezésére az adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="3d35e-302">Let's add the inbound rule for the NSG to allow inbound connections on port 22 (to support SSH).</span></span> <span data-ttu-id="3d35e-303">Az alábbi példa létrehoz egy nevű szabályt `myNetworkSecurityGroupRuleSSH` TCP-port a 22-es engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="3d35e-303">The following example creates a rule named `myNetworkSecurityGroupRuleSSH` to allow TCP on port 22:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

<span data-ttu-id="3d35e-304">Most adjuk hozzá (a webes forgalom támogatásához) 80-as portot a bejövő kapcsolatok engedélyezésére az NSG bejövő szabályt.</span><span class="sxs-lookup"><span data-stu-id="3d35e-304">Now let's add the inbound rule for the NSG to allow inbound connections on port 80 (to support web traffic).</span></span> <span data-ttu-id="3d35e-305">Az alábbi példa létrehoz egy nevű szabályt `myNetworkSecurityGroupRuleHTTP` a TCP engedélyezéséhez a 80-as port:</span><span class="sxs-lookup"><span data-stu-id="3d35e-305">The following example creates a rule named `myNetworkSecurityGroupRuleHTTP` to allow TCP on port 80:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> <span data-ttu-id="3d35e-306">A bejövő forgalomra vonatkozó szabály egy szűrő a bejövő hálózati kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="3d35e-306">The inbound rule is a filter for inbound network connections.</span></span> <span data-ttu-id="3d35e-307">Ebben a példában a virtuális gépek virtuális hálózati adapter, ami azt jelenti, hogy minden kérést 22-es portot áthalad a hálózati adapterre a virtuális gépen az NSG kötni azt.</span><span class="sxs-lookup"><span data-stu-id="3d35e-307">In this example, we bind the NSG to the VMs virtual NIC, which means that any request to port 22 is passed through to the NIC on our VM.</span></span> <span data-ttu-id="3d35e-308">A bejövő szabály a hálózati kapcsolat, és nem kapcsolatos, a végpont melyik tárolókezelésének kapcsolatos a klasszikus üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="3d35e-308">This inbound rule is about a network connection, and not about an endpoint, which is what it would be about in classic deployments.</span></span> <span data-ttu-id="3d35e-309">Port megnyitásához, meg kell hagyni a `--source-port-range` beállítása "\*" (az alapértelmezett érték) a bejövő kéréseket fogad **bármely** a kért porton.</span><span class="sxs-lookup"><span data-stu-id="3d35e-309">To open a port, you must leave the `--source-port-range` set to '\*' (the default value) to accept inbound requests from **any** requesting port.</span></span> <span data-ttu-id="3d35e-310">Ezek általában dinamikus.</span><span class="sxs-lookup"><span data-stu-id="3d35e-310">Ports are typically dynamic.</span></span>
>
>

## <a name="bind-to-the-nic"></a><span data-ttu-id="3d35e-311">Kötést létrehozni a hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="3d35e-311">Bind to the NIC</span></span>
<span data-ttu-id="3d35e-312">Az NSG-t a hálózati adapterek kötni.</span><span class="sxs-lookup"><span data-stu-id="3d35e-312">Bind the NSG to the NICs.</span></span> <span data-ttu-id="3d35e-313">Igazolnia kell a hálózati adaptert megoszthatja a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="3d35e-313">We need to connect our NICs with our network security group.</span></span> <span data-ttu-id="3d35e-314">Mindkét parancsok, mind a hálózati adapterek bekötése futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3d35e-314">Run both commands, to hook up both of our NICs:</span></span>

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a><span data-ttu-id="3d35e-315">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d35e-315">Create an availability set</span></span>
<span data-ttu-id="3d35e-316">Rendelkezésre állási a virtuális gépek készletek súgó terjedésének tartalék tartományok és a frissítési tartományok között.</span><span class="sxs-lookup"><span data-stu-id="3d35e-316">Availability sets help spread your VMs across fault domains and upgrade domains.</span></span> <span data-ttu-id="3d35e-317">Hozzon létre egy rendelkezésre állási készletét, a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3d35e-317">Let's create an availability set for your VMs.</span></span> <span data-ttu-id="3d35e-318">Az alábbi példakód létrehozza a rendelkezésre állási készlet elnevezett `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="3d35e-318">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

<span data-ttu-id="3d35e-319">Tartalék tartományok definiálása, amelyek egy közös forrás- és hálózati kikapcsolás virtuális gépek csoportja.</span><span class="sxs-lookup"><span data-stu-id="3d35e-319">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="3d35e-320">Alapértelmezés szerint a rendelkezésre állási csoport belül konfigurált virtuális gépek egymástól legfeljebb három tartalék tartományokban.</span><span class="sxs-lookup"><span data-stu-id="3d35e-320">By default, the virtual machines that are configured within your availability set are separated across up to three fault domains.</span></span> <span data-ttu-id="3d35e-321">A lényege, hogy a hardver probléma a tartalék tartományok valamelyikében nem befolyásolja a minden virtuális gép, amelyen fut. az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3d35e-321">The idea is that a hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span> <span data-ttu-id="3d35e-322">Azure virtuális gépek elhelyezésekor azokat a rendelkezésre állási csoport automatikusan elosztja a tartalék tartományok.</span><span class="sxs-lookup"><span data-stu-id="3d35e-322">Azure automatically distributes VMs across the fault domains when placing them in an availability set.</span></span>

<span data-ttu-id="3d35e-323">Frissítési tartományok adja meg a virtuális gépek és a mögöttes fizikai hardver, amely egy időben újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="3d35e-323">Upgrade domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="3d35e-324">A sorrendet, amelyben a frissítési tartományok újraindítása után előfordulhat, hogy nem szekvenciális tervezett karbantartás alatt, de egyszerre csak egy frissítés újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="3d35e-324">The order in which upgrade domains are rebooted might not be sequential during planned maintenance, but only one upgrade is rebooted at a time.</span></span> <span data-ttu-id="3d35e-325">Ebben az esetben Azure automatikusan elosztása a virtuális gépek frissítési tartományok amikor egy rendelkezésre állási helyen helyezi el őket.</span><span class="sxs-lookup"><span data-stu-id="3d35e-325">Again, Azure automatically distributes your VMs across upgrade domains when placing them in an availability site.</span></span>

<span data-ttu-id="3d35e-326">Tudjon meg többet az [virtuális gépek rendelkezésre állásának kezelése](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d35e-326">Read more about [managing the availability of VMs](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="create-the-linux-vms"></a><span data-ttu-id="3d35e-327">A Linux virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d35e-327">Create the Linux VMs</span></span>
<span data-ttu-id="3d35e-328">A tárolási és hálózati erőforrásokat, internetről elérhető virtuális gépek támogatásához létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3d35e-328">You've created the storage and network resources to support Internet-accessible VMs.</span></span> <span data-ttu-id="3d35e-329">Most tegyük létrehozása a virtuális gépek és biztonságos őket egy SSH-kulcsban, amely nem tartozik jelszó.</span><span class="sxs-lookup"><span data-stu-id="3d35e-329">Now let's create those VMs and secure them with an SSH key that doesn't have a password.</span></span> <span data-ttu-id="3d35e-330">Ebben az esetben egy Ubuntu virtuális gép a legutóbbi LTS alapján hozzon létre programot fogjuk.</span><span class="sxs-lookup"><span data-stu-id="3d35e-330">In this case, we're going to create an Ubuntu VM based on the most recent LTS.</span></span> <span data-ttu-id="3d35e-331">Azt az kép információk keresse meg a `azure vm image list`leírtak szerint [Azure Virtuálisgép-rendszerképekről keresése](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d35e-331">We locate that image information by using `azure vm image list`, as described in [finding Azure VM images](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="3d35e-332">A parancs segítségével lemezkép jelenleg kijelölt `azure vm image list westeurope canonical | grep LTS`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-332">We selected an image by using the command `azure vm image list westeurope canonical | grep LTS`.</span></span> <span data-ttu-id="3d35e-333">Ebben az esetben használjuk `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span><span class="sxs-lookup"><span data-stu-id="3d35e-333">In this case, we use `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span></span> <span data-ttu-id="3d35e-334">Az utolsó mező azt át `latest` így a jövőben azt mindig a legfrissebb build.</span><span class="sxs-lookup"><span data-stu-id="3d35e-334">For the last field, we pass `latest` so that in the future we always get the most recent build.</span></span> <span data-ttu-id="3d35e-335">(A karakterlánc használjuk `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span><span class="sxs-lookup"><span data-stu-id="3d35e-335">(The string we use is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span></span>

<span data-ttu-id="3d35e-336">Ez a következő lépés nem ismerős bárki, aki már létrehozta a egy ssh rsa nyilvános és titkos kulcs párosítsa a Linux- vagy Mac használatával **ssh-keygen - t rsa -b 2048**.</span><span class="sxs-lookup"><span data-stu-id="3d35e-336">This next step is familiar to anyone who has already created an ssh rsa public and private key pair on Linux or Mac by using **ssh-keygen -t rsa -b 2048**.</span></span> <span data-ttu-id="3d35e-337">Ha nincs a tanúsítvány kulcspárok a `~/.ssh` könyvtárában, akkor létrehozhat őket:</span><span class="sxs-lookup"><span data-stu-id="3d35e-337">If you do not have any certificate key pairs in your `~/.ssh` directory, you can create them:</span></span>

* <span data-ttu-id="3d35e-338">Automatikus, segítségével a `azure vm create --generate-ssh-keys` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3d35e-338">Automatically, by using the `azure vm create --generate-ssh-keys` option.</span></span>
* <span data-ttu-id="3d35e-339">Manuálisan [útmutatást követve létrehozhatja a saját kezűleg](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d35e-339">Manually, by using [the instructions to create them yourself](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="3d35e-340">Másik lehetőségként használhatja a `--admin-password` módszert az SSH-kapcsolatok a virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="3d35e-340">Alternatively, you can use the `--admin-password` method to authenticate your SSH connections after the VM is created.</span></span> <span data-ttu-id="3d35e-341">Ez a módszer általában kevésbé biztonságos.</span><span class="sxs-lookup"><span data-stu-id="3d35e-341">This method is typically less secure.</span></span>

<span data-ttu-id="3d35e-342">Azt a virtuális gép létrehozása az erőforrások és információk együtt hozásával a `azure vm create` parancs:</span><span class="sxs-lookup"><span data-stu-id="3d35e-342">We create the VM by bringing all our resources and information together with the `azure vm create` command:</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

<span data-ttu-id="3d35e-343">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-343">Output:</span></span>

```azurecli
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

<span data-ttu-id="3d35e-344">Csatlakozhat a virtuális gép azonnal az alapértelmezett SSH-kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="3d35e-344">You can connect to your VM immediately by using your default SSH keys.</span></span> <span data-ttu-id="3d35e-345">Győződjön meg arról, hogy a megfelelő portot adja meg, mivel azt még áthaladnának a terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="3d35e-345">Make sure that you specify the appropriate port since we're passing through the load balancer.</span></span> <span data-ttu-id="3d35e-346">(Az első virtuális gép, beállítjuk a NAT-szabály port 4222 továbbítsa a virtuális géphez.)</span><span class="sxs-lookup"><span data-stu-id="3d35e-346">(For our first VM, we set up the NAT rule to forward port 4222 to our VM.)</span></span>

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

<span data-ttu-id="3d35e-347">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-347">Output:</span></span>

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

<span data-ttu-id="3d35e-348">Lépjen tovább, és a második virtuális gép létrehozása az azonos módon:</span><span class="sxs-lookup"><span data-stu-id="3d35e-348">Go ahead and create your second VM in the same manner:</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

<span data-ttu-id="3d35e-349">És ezután már használhatja a `azure vm show myResourceGroup myVM1` parancs futtatásával ellenőrizze, hogy mi létrehozta.</span><span class="sxs-lookup"><span data-stu-id="3d35e-349">And you can now use the `azure vm show myResourceGroup myVM1` command to examine what you've created.</span></span> <span data-ttu-id="3d35e-350">Ezen a ponton a Ubuntu virtuális gép egy terheléselosztó mögött futtatja, amely jelentkezhet be az SSH-kulcspárral csak a (mert a jelszavak le vannak tiltva) Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3d35e-350">At this point, you're running your Ubuntu VMs behind a load balancer in Azure that you can sign into only with your SSH key pair (because passwords are disabled).</span></span> <span data-ttu-id="3d35e-351">Telepítse a nginx vagy httpd, webalkalmazás üzembe helyezése, és a hálózati forgalom mindkét a virtuális gépek számára a terheléselosztó keresztül.</span><span class="sxs-lookup"><span data-stu-id="3d35e-351">You can install nginx or httpd, deploy a web app, and see the traffic flow through the load balancer to both of the VMs.</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

<span data-ttu-id="3d35e-352">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="3d35e-352">Output:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a><span data-ttu-id="3d35e-353">A környezet sablon exportálása</span><span class="sxs-lookup"><span data-stu-id="3d35e-353">Export the environment as a template</span></span>
<span data-ttu-id="3d35e-354">Most, hogy már létrehozta a buildet ki ebben a környezetben, mi történik, ha meg szeretné további fejlesztési környezet létrehozása, és ugyanazokkal a paraméterekkel, amely megfelel az éles környezetben?</span><span class="sxs-lookup"><span data-stu-id="3d35e-354">Now that you have built out this environment, what if you want to create an additional development environment with the same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="3d35e-355">Erőforrás-kezelő a környezet összes paramétereit meghatározó JSON-sablonokat használ.</span><span class="sxs-lookup"><span data-stu-id="3d35e-355">Resource Manager uses JSON templates that define all the parameters for your environment.</span></span> <span data-ttu-id="3d35e-356">A JSON-sablon Vezérlőpultjának kimenő teljes környezetek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-356">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="3d35e-357">Is [JSON sablonok létrehozása manuálisan](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy egy meglévő környezet hozza létre a JSON-sablon exportálása:</span><span class="sxs-lookup"><span data-stu-id="3d35e-357">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment to create the JSON template for you:</span></span>

```azurecli
azure group export --name myResourceGroup
```

<span data-ttu-id="3d35e-358">Ezzel a paranccsal létrejön az `myResourceGroup.json` az aktuális munkakönyvtárban fájlban.</span><span class="sxs-lookup"><span data-stu-id="3d35e-358">This command creates the `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="3d35e-359">A sablon alapján hoz létre egy olyan környezetben, amikor az összes erőforrás neveit, beleértve a terheléselosztóhoz, hálózati adapterek vagy virtuális gépek nevek kéri.</span><span class="sxs-lookup"><span data-stu-id="3d35e-359">When you create an environment from this template, you are prompted for all the resource names, including the names for the load balancer, network interfaces, or VMs.</span></span> <span data-ttu-id="3d35e-360">Töltheti fel ezeket a neveket a sablon fájlban adja hozzá a `-p` vagy `--includeParameterDefaultValue` paramétert a `azure group export` korábban bemutatott parancsot.</span><span class="sxs-lookup"><span data-stu-id="3d35e-360">You can populate these names in your template file by adding the `-p` or `--includeParameterDefaultValue` parameter to the `azure group export` command that was shown earlier.</span></span> <span data-ttu-id="3d35e-361">Az erőforrás nevének megadása a JSON-sablon szerkesztése vagy [parameters.json fájl létrehozása](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , amely meghatározza, hogy az erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="3d35e-361">Edit your JSON template to specify the resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies the resource names.</span></span>

<span data-ttu-id="3d35e-362">Olyan környezetet hozhat létre a sablonból:</span><span class="sxs-lookup"><span data-stu-id="3d35e-362">To create an environment from your template:</span></span>

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

<span data-ttu-id="3d35e-363">Előfordulhat, hogy szeretné olvasni [felügyeleticsomag-sablonok telepítésével kapcsolatos további](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d35e-363">You might want to read [more about how to deploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="3d35e-364">További tudnivalók Növekményesen környezetek frissítése, használja a paraméterek fájlját, és egyetlen tárolási helyen sablonok elérésére.</span><span class="sxs-lookup"><span data-stu-id="3d35e-364">Learn about how to incrementally update environments, use the parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d35e-365">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d35e-365">Next steps</span></span>
<span data-ttu-id="3d35e-366">Most már készen áll a több hálózati összetevőkkel és virtuális gépek használatának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="3d35e-366">Now you're ready to begin working with multiple networking components and VMs.</span></span> <span data-ttu-id="3d35e-367">Ez a minta-környezet segítségével bevezetett alapösszetevőket itt használatával, az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d35e-367">You can use this sample environment to build out your application by using the core components introduced here.</span></span>
