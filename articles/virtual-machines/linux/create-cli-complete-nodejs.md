---
title: "teljes körű Linux-környezetet hello Azure CLI 1.0 aaaCreate |} Microsoft Docs"
description: "Tárolási, Linux virtuális gép, egy virtuális hálózati és alhálózati, egy terhelés-kiegyenlítő, egy hálózati Adapterre, egy nyilvános IP-cím és a hálózati biztonsági csoport létrehozása minden a hello Azure CLI 1.0 használatával szabad hello."
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
ms.openlocfilehash: 7fe00e138704fe9c9a1c9b87a7dd1afd6174e527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-environment-with-hello-azure-cli-10"></a><span data-ttu-id="f8660-103">Hozzon létre egy teljes körű Linux környezetet hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f8660-103">Create a complete Linux environment with hello Azure CLI 1.0</span></span>
<span data-ttu-id="f8660-104">Ebben a cikkben azt a terheléselosztó és a virtuális gépek, amelyek hasznosak a fejlesztési és egyszerű számítástechnikai két egyszerű hálózat felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f8660-104">In this article, we build a simple network with a load balancer and a pair of VMs that are useful for development and simple computing.</span></span> <span data-ttu-id="f8660-105">Hello folyamat parancs által parancs azt ismerteti, két működő beszerzéséig biztonságos Linux virtuális gépek toowhich lehet csatlakoztatni bárhol az interneten hello.</span><span class="sxs-lookup"><span data-stu-id="f8660-105">We walk through hello process command by command, until you have two working, secure Linux VMs toowhich you can connect from anywhere on hello Internet.</span></span> <span data-ttu-id="f8660-106">Majd továbbléphet toomore bonyolult hálózatok és a környezetben.</span><span class="sxs-lookup"><span data-stu-id="f8660-106">Then you can move on toomore complex networks and environments.</span></span>

<span data-ttu-id="f8660-107">Mentén hello módon akkor további tudnivalók hello függőségi hierarchia hello Resource Manager telepítési modell lehetővé teszi, és arról, hogy mekkora teljesítményre biztosít.</span><span class="sxs-lookup"><span data-stu-id="f8660-107">Along hello way, you learn about hello dependency hierarchy that hello Resource Manager deployment model gives you, and about how much power it provides.</span></span> <span data-ttu-id="f8660-108">Miután meggyőződött arról, hogyan hello rendszer épül, újjáépíthető azt sokkal gyorsabban használatával [Azure Resource Manager-sablonok](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8660-108">After you see how hello system is built, you can rebuild it much more quickly by using [Azure Resource Manager templates](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f8660-109">Emellett után megtudhatja, hogyan működnek a környezet hello részei együtt, sablonok tooautomate létrehozása őket kezdve egyszerűbbé válik.</span><span class="sxs-lookup"><span data-stu-id="f8660-109">Also, after you learn how hello parts of your environment fit together, creating templates tooautomate them becomes easier.</span></span>

<span data-ttu-id="f8660-110">hello környezet tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f8660-110">hello environment contains:</span></span>

* <span data-ttu-id="f8660-111">Két virtuális gépek rendelkezésre állási csoportok belül.</span><span class="sxs-lookup"><span data-stu-id="f8660-111">Two VMs inside an availability set.</span></span>
* <span data-ttu-id="f8660-112">Terheléselosztó terheléselosztási szabállyal 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="f8660-112">A load balancer with a load-balancing rule on port 80.</span></span>
* <span data-ttu-id="f8660-113">Hálózati biztonsági csoport (NSG) szabályok tooprotect nem kívánt forgalmat a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f8660-113">Network security group (NSG) rules tooprotect your VM from unwanted traffic.</span></span>

<span data-ttu-id="f8660-114">toocreate egyéni ebben a környezetben, hello legújabb kell [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erőforrás-kezelő módban (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="f8660-114">toocreate this custom environment, you need hello latest [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Resource Manager mode (`azure config mode arm`).</span></span> <span data-ttu-id="f8660-115">Egy eszköz elemzése JSON is kell.</span><span class="sxs-lookup"><span data-stu-id="f8660-115">You also need a JSON parsing tool.</span></span> <span data-ttu-id="f8660-116">Ez a példa [jq](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="f8660-116">This example uses [jq](https://stedolan.github.io/jq/).</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="f8660-117">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="f8660-117">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="f8660-118">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="f8660-118">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="f8660-119">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="f8660-119">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="f8660-120">[Az Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="f8660-120">[Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="f8660-121">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="f8660-121">Quick commands</span></span>
<span data-ttu-id="f8660-122">Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello hello kiinduló parancsok tooupload egy virtuális gép tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f8660-122">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VM tooAzure.</span></span> <span data-ttu-id="f8660-123">Részletes információkat és a környezetben a hello többi hello dokumentum indítása találhatók az egyes lépések [Itt](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="f8660-123">More detailed information and context for each step can be found in hello rest of hello document, starting [here](#detailed-walkthrough).</span></span>

<span data-ttu-id="f8660-124">Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="f8660-124">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="f8660-125">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="f8660-125">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="f8660-126">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="f8660-126">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

<span data-ttu-id="f8660-127">Hello erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f8660-127">Create hello resource group.</span></span> <span data-ttu-id="f8660-128">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helye:</span><span class="sxs-lookup"><span data-stu-id="f8660-128">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location:</span></span>

```azurecli
azure group create -n myResourceGroup -l westeurope
```

<span data-ttu-id="f8660-129">Ellenőrizze a hello erőforráscsoport hello JSON elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="f8660-129">Verify hello resource group by using hello JSON parser:</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="f8660-130">Hello storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-130">Create hello storage account.</span></span> <span data-ttu-id="f8660-131">hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`.</span><span class="sxs-lookup"><span data-stu-id="f8660-131">hello following example creates a storage account named `mystorageaccount`.</span></span> <span data-ttu-id="f8660-132">(hello tárfióknév kell egyedinek lennie, így rendelkeznek a saját egyedi nevet.)</span><span class="sxs-lookup"><span data-stu-id="f8660-132">(hello storage account name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

<span data-ttu-id="f8660-133">Ellenőrizze a hello tárfiók hello JSON elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="f8660-133">Verify hello storage account by using hello JSON parser:</span></span>

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

<span data-ttu-id="f8660-134">Hello virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-134">Create hello virtual network.</span></span> <span data-ttu-id="f8660-135">hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet`:</span><span class="sxs-lookup"><span data-stu-id="f8660-135">hello following example creates a virtual network named `myVnet`:</span></span>

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

<span data-ttu-id="f8660-136">Hozzon létre egy alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="f8660-136">Create a subnet.</span></span> <span data-ttu-id="f8660-137">hello alábbi példa létrehoz egy nevű alhálózat `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="f8660-137">hello following example creates a subnet named `mySubnet`:</span></span>

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

<span data-ttu-id="f8660-138">Ellenőrizze a hello virtuális hálózati és alhálózati hello JSON elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="f8660-138">Verify hello virtual network and subnet by using hello JSON parser:</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="f8660-139">Hozzon létre egy nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="f8660-139">Create a public IP.</span></span> <span data-ttu-id="f8660-140">hello alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP` hello DNS-névvel, `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="f8660-140">hello following example creates a public IP named `myPublicIP` with hello DNS name of `mypublicdns`.</span></span> <span data-ttu-id="f8660-141">(hello DNS-nevét kell egyedinek lennie, így rendelkeznek a saját egyedi nevet.)</span><span class="sxs-lookup"><span data-stu-id="f8660-141">(hello DNS name must be unique, so provide your own unique name.)</span></span>

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

<span data-ttu-id="f8660-142">Hello terheléselosztó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-142">Create hello load balancer.</span></span> <span data-ttu-id="f8660-143">hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="f8660-143">hello following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

<span data-ttu-id="f8660-144">Hozzon létre egy előtér-IP-címkészletet hello terheléselosztóhoz, és társítsa hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="f8660-144">Create a front-end IP pool for hello load balancer, and associate hello public IP.</span></span> <span data-ttu-id="f8660-145">hello alábbi példa létrehoz egy előtér-IP-címkészletet nevű `mySubnetPool`:</span><span class="sxs-lookup"><span data-stu-id="f8660-145">hello following example creates a front-end IP pool named `mySubnetPool`:</span></span>

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

<span data-ttu-id="f8660-146">Hozzon létre a hello háttér IP-címkészletet hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="f8660-146">Create hello back-end IP pool for hello load balancer.</span></span> <span data-ttu-id="f8660-147">hello alábbi példa létrehoz egy háttér-IP-készlet nevű `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="f8660-147">hello following example creates a back-end IP pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

<span data-ttu-id="f8660-148">Hozzon létre SSH bejövő hálózati cím címfordítási (NAT) szabályait hello terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="f8660-148">Create SSH inbound network address translation (NAT) rules for hello load balancer.</span></span> <span data-ttu-id="f8660-149">hello alábbi példa létrehoz két terheléselosztási szabály, `myLoadBalancerRuleSSH1` és `myLoadBalancerRuleSSH2`:</span><span class="sxs-lookup"><span data-stu-id="f8660-149">hello following example creates two load balancer rules, `myLoadBalancerRuleSSH1` and `myLoadBalancerRuleSSH2`:</span></span>

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

<span data-ttu-id="f8660-150">Hello webalkalmazás létrehozása a terheléselosztó bejövő NAT-szabályokat hello.</span><span class="sxs-lookup"><span data-stu-id="f8660-150">Create hello web inbound NAT rules for hello load balancer.</span></span> <span data-ttu-id="f8660-151">hello alábbi példa létrehoz egy terheléselosztási szabály nevű `myLoadBalancerRuleWeb`:</span><span class="sxs-lookup"><span data-stu-id="f8660-151">hello following example creates a load balancer rule named `myLoadBalancerRuleWeb`:</span></span>

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

<span data-ttu-id="f8660-152">Hello állapotfigyelő terheléselosztói mintavétel létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-152">Create hello load balancer health probe.</span></span> <span data-ttu-id="f8660-153">hello alábbi példa létrehoz egy TCP-Hálózatfigyelővel nevű `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="f8660-153">hello following example creates a TCP probe named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

<span data-ttu-id="f8660-154">Hello JSON elemző hello terheléselosztóhoz, az IP-készletek és a NAT-szabályok ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="f8660-154">Verify hello load balancer, IP pools, and NAT rules by using hello JSON parser:</span></span>

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

<span data-ttu-id="f8660-155">Hozzon létre hello első hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="f8660-155">Create hello first network interface card (NIC).</span></span> <span data-ttu-id="f8660-156">Cserélje le a hello `#####-###-###` szakaszok a saját Azure-előfizetés-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="f8660-156">Replace hello `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="f8660-157">Az előfizetés-azonosító hello kimenete jelenik meg **jq** hello erőforrások létrehozásakor vizsgálata során.</span><span class="sxs-lookup"><span data-stu-id="f8660-157">Your subscription ID is noted in hello output of **jq** when you examine hello resources you are creating.</span></span> <span data-ttu-id="f8660-158">Megtekintheti az előfizetés-Azonosítóval rendelkező `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="f8660-158">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="f8660-159">hello alábbi példa létrehoz egy hálózati adapter nevű `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="f8660-159">hello following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

<span data-ttu-id="f8660-160">Hozzon létre hello második hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="f8660-160">Create hello second NIC.</span></span> <span data-ttu-id="f8660-161">hello alábbi példa létrehoz egy hálózati adapter nevű `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="f8660-161">hello following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

<span data-ttu-id="f8660-162">Ellenőrizze a hello két hálózati adapterrel hello JSON elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="f8660-162">Verify hello two NICs by using hello JSON parser:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

<span data-ttu-id="f8660-163">Hello hálózati biztonsági csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-163">Create hello network security group.</span></span> <span data-ttu-id="f8660-164">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="f8660-164">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

<span data-ttu-id="f8660-165">Két bejövő szabályok hello hálózati biztonsági csoport hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="f8660-165">Add two inbound rules for hello network security group.</span></span> <span data-ttu-id="f8660-166">hello alábbi példa létrehoz két szabály `myNetworkSecurityGroupRuleSSH` és `myNetworkSecurityGroupRuleHTTP`:</span><span class="sxs-lookup"><span data-stu-id="f8660-166">hello following example creates two rules, `myNetworkSecurityGroupRuleSSH` and `myNetworkSecurityGroupRuleHTTP`:</span></span>

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

<span data-ttu-id="f8660-167">Ellenőrizze a hello hálózati biztonsági csoport és a bejövő szabályok hello JSON elemző használatával:</span><span class="sxs-lookup"><span data-stu-id="f8660-167">Verify hello network security group and inbound rules by using hello JSON parser:</span></span>

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

<span data-ttu-id="f8660-168">Kötési hello hálózati biztonsági csoport toohello két hálózati adapter:</span><span class="sxs-lookup"><span data-stu-id="f8660-168">Bind hello network security group toohello two NICs:</span></span>

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

<span data-ttu-id="f8660-169">Hello rendelkezésre állási csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-169">Create hello availability set.</span></span> <span data-ttu-id="f8660-170">hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="f8660-170">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

<span data-ttu-id="f8660-171">Hozzon létre első Linux virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="f8660-171">Create hello first Linux VM.</span></span> <span data-ttu-id="f8660-172">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM1`:</span><span class="sxs-lookup"><span data-stu-id="f8660-172">hello following example creates a VM named `myVM1`:</span></span>

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

<span data-ttu-id="f8660-173">Hello létrehozása Linux virtuális gép második.</span><span class="sxs-lookup"><span data-stu-id="f8660-173">Create hello second Linux VM.</span></span> <span data-ttu-id="f8660-174">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM2`:</span><span class="sxs-lookup"><span data-stu-id="f8660-174">hello following example creates a VM named `myVM2`:</span></span>

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

<span data-ttu-id="f8660-175">Használjon hello JSON elemző tooverify, hogy minden épülő:</span><span class="sxs-lookup"><span data-stu-id="f8660-175">Use hello JSON parser tooverify that everything that was built:</span></span>

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

<span data-ttu-id="f8660-176">Az új környezet tooa sablon tooquickly hozza létre új példányok exportálása:</span><span class="sxs-lookup"><span data-stu-id="f8660-176">Export your new environment tooa template tooquickly re-create new instances:</span></span>

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="f8660-177">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="f8660-177">Detailed walkthrough</span></span>
<span data-ttu-id="f8660-178">hello részletes következő lépések azt ismertetik, milyen minden parancs módon, hogy ki a környezetben.</span><span class="sxs-lookup"><span data-stu-id="f8660-178">hello detailed steps that follow explain what each command is doing as you build out your environment.</span></span> <span data-ttu-id="f8660-179">Ezek a következők hasznos a saját egyéni fejlesztési és éles környezeteket összeállításakor.</span><span class="sxs-lookup"><span data-stu-id="f8660-179">These concepts are helpful when you build your own custom environments for development or production.</span></span>

<span data-ttu-id="f8660-180">Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="f8660-180">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="f8660-181">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="f8660-181">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="f8660-182">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="f8660-182">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>

## <a name="create-resource-groups-and-choose-deployment-locations"></a><span data-ttu-id="f8660-183">Erőforráscsoport-sablonok létrehozása és központi telepítési helyek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="f8660-183">Create resource groups and choose deployment locations</span></span>
<span data-ttu-id="f8660-184">Azure erőforráscsoport-sablonok konfigurációs információkat és a metaadatok tooenable hello logikai kezelése erőforrás központi telepítések tartalmazó logikai telepítési entitások.</span><span class="sxs-lookup"><span data-stu-id="f8660-184">Azure resource groups are logical deployment entities that contain configuration information and metadata tooenable hello logical management of resource deployments.</span></span> <span data-ttu-id="f8660-185">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helye:</span><span class="sxs-lookup"><span data-stu-id="f8660-185">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location:</span></span>

```azurecli
azure group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="f8660-186">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-186">Output:</span></span>

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

## <a name="create-a-storage-account"></a><span data-ttu-id="f8660-187">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="f8660-187">Create a storage account</span></span>
<span data-ttu-id="f8660-188">Storage-fiókok a virtuális gép lemezeit és bármelyik adatlemeznek a megjeleníteni kívánt tooadd van szüksége.</span><span class="sxs-lookup"><span data-stu-id="f8660-188">You need storage accounts for your VM disks and for any additional data disks that you want tooadd.</span></span> <span data-ttu-id="f8660-189">Szinte azonnal után létrehozhat erőforráscsoportok storage-fiókokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="f8660-189">You create storage accounts almost immediately after you create resource groups.</span></span>

<span data-ttu-id="f8660-190">Jelen példában használjuk hello `azure storage account create` parancsot, és hello típusú kívánt tárolási támogatást átadásakor hello fiók, hello erőforráscsoport hello helye, amely szabályozza.</span><span class="sxs-lookup"><span data-stu-id="f8660-190">Here we use hello `azure storage account create` command, passing hello location of hello account, hello resource group that controls it, and hello type of storage support you want.</span></span> <span data-ttu-id="f8660-191">hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="f8660-191">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

<span data-ttu-id="f8660-192">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-192">Output:</span></span>

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

<span data-ttu-id="f8660-193">az erőforráscsoport hello segítségével tooexamine `azure group show` paranccsal, most használja az hello [jq](https://stedolan.github.io/jq/) eszközzel együtt hello `--json` Azure CLI lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f8660-193">tooexamine our resource group by using hello `azure group show` command, let's use hello [jq](https://stedolan.github.io/jq/) tool along with hello `--json` Azure CLI option.</span></span> <span data-ttu-id="f8660-194">(Használható **jsawk** vagy tooparse inkább függvénytárat, nyelvi hello JSON-NÁ.)</span><span class="sxs-lookup"><span data-stu-id="f8660-194">(You can use **jsawk** or any language library you prefer tooparse hello JSON.)</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="f8660-195">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-195">Output:</span></span>

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

<span data-ttu-id="f8660-196">hello-tárfiók tooinvestigate hello parancssori felület használatával, először tooset hello számítógépfiók-nevét és a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="f8660-196">tooinvestigate hello storage account by using hello CLI, you first need tooset hello account names and keys.</span></span> <span data-ttu-id="f8660-197">Cserélje le a hello storage-fiókot a következő példa egy névvel, Ön által hello hello nevét:</span><span class="sxs-lookup"><span data-stu-id="f8660-197">Replace hello name of hello storage account in hello following example with a name that you choose:</span></span>

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

<span data-ttu-id="f8660-198">Ezután egyszerűen megtekintheti a tárolással kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="f8660-198">Then you can view your storage information easily:</span></span>

```azurecli
azure storage container list
```

<span data-ttu-id="f8660-199">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-199">Output:</span></span>

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="f8660-200">Hozzon létre egy virtuális hálózat és alhálózat</span><span class="sxs-lookup"><span data-stu-id="f8660-200">Create a virtual network and subnet</span></span>
<span data-ttu-id="f8660-201">Ezután fog tooneed toocreate Azure-ban és a virtuális gépeket hozhat létre alhálózat futtató virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="f8660-201">Next you're going tooneed toocreate a virtual network running in Azure and a subnet in which you can create your VMs.</span></span> <span data-ttu-id="f8660-202">hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` a hello `192.168.0.0/16` címelőtag:</span><span class="sxs-lookup"><span data-stu-id="f8660-202">hello following example creates a virtual network named `myVnet` with hello `192.168.0.0/16` address prefix:</span></span>

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="f8660-203">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-203">Output:</span></span>

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

<span data-ttu-id="f8660-204">Ebben az esetben használja most hello--json lehetőség a `azure group show` és `jq` toosee hogyan dolgozunk, hogy az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f8660-204">Again, let's use hello --json option of `azure group show` and `jq` toosee how we're building our resources.</span></span> <span data-ttu-id="f8660-205">Most már rendelkezik egy `storageAccounts` erőforrás és a `virtualNetworks` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="f8660-205">We now have a `storageAccounts` resource and a `virtualNetworks` resource.</span></span>  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="f8660-206">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-206">Output:</span></span>

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

<span data-ttu-id="f8660-207">Most hozzuk létre az alhálózatot a hello `myVnet` virtuális hálózatot, mely hello a virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="f8660-207">Now let's create a subnet in hello `myVnet` virtual network into which hello VMs are deployed.</span></span> <span data-ttu-id="f8660-208">Hello használjuk `azure network vnet subnet create` parancs már korábban létrehozott hello erőforrások együtt: hello `myResourceGroup` erőforráscsoport és hello `myVnet` virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="f8660-208">We use hello `azure network vnet subnet create` command, along with hello resources we've already created: hello `myResourceGroup` resource group and hello `myVnet` virtual network.</span></span> <span data-ttu-id="f8660-209">A következő példa hello, azt hozzá nevű hello alhálózati `mySubnet` hello alhálózati cím előtagját a `192.168.1.0/24`:</span><span class="sxs-lookup"><span data-stu-id="f8660-209">In hello following example, we add hello subnet named `mySubnet` with hello subnet address prefix of `192.168.1.0/24`:</span></span>

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

<span data-ttu-id="f8660-210">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-210">Output:</span></span>

```azurecli
info:    Executing command network vnet subnet create
+ Looking up hello subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up hello subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

<span data-ttu-id="f8660-211">Hello alhálózati logikailag hello virtuális hálózaton belül van, mert azt egy némileg eltérő parancs hello alhálózati információkért meg.</span><span class="sxs-lookup"><span data-stu-id="f8660-211">Because hello subnet is logically inside hello virtual network, we look for hello subnet information with a slightly different command.</span></span> <span data-ttu-id="f8660-212">hello parancs használjuk `azure network vnet show`, de tooexamine hello JSON-kimenetét használatával továbbra is `jq`.</span><span class="sxs-lookup"><span data-stu-id="f8660-212">hello command we use is `azure network vnet show`, but we continue tooexamine hello JSON output by using `jq`.</span></span>

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

<span data-ttu-id="f8660-213">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-213">Output:</span></span>

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

## <a name="create-a-public-ip-address"></a><span data-ttu-id="f8660-214">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="f8660-214">Create a public IP address</span></span>
<span data-ttu-id="f8660-215">Most hozzuk létre az hello nyilvános IP-cím (PIP), azt rendelje tooyour terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="f8660-215">Now let's create hello public IP address (PIP) that we assign tooyour load balancer.</span></span> <span data-ttu-id="f8660-216">Lehetővé teszi a tooconnect tooyour virtuális gépek a hello Internet hello segítségével `azure network public-ip create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="f8660-216">It enables you tooconnect tooyour VMs from hello Internet by using hello `azure network public-ip create` command.</span></span> <span data-ttu-id="f8660-217">Mivel hello alapértelmezett cím dinamikus, nem létrehozni egy elnevezett DNS-bejegyzés hello **cloudapp.azure.com** hello segítségével tartomány `--domain-name-label` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f8660-217">Because hello default address is dynamic, we create a named DNS entry in hello **cloudapp.azure.com** domain by using hello `--domain-name-label` option.</span></span> <span data-ttu-id="f8660-218">hello alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP` hello DNS-névvel, `mypublicdns`.</span><span class="sxs-lookup"><span data-stu-id="f8660-218">hello following example creates a public IP named `myPublicIP` with hello DNS name of `mypublicdns`.</span></span> <span data-ttu-id="f8660-219">Mivel a hello DNS-nevének egyedinek kell lennie, meg kell adni a saját egyedi DNS-név:</span><span class="sxs-lookup"><span data-stu-id="f8660-219">Because hello DNS name must be unique, you provide your own unique DNS name:</span></span>

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

<span data-ttu-id="f8660-220">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-220">Output:</span></span>

```azurecli
info:    Executing command network public-ip create
+ Looking up hello public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up hello public ip "myPublicIP"
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

<span data-ttu-id="f8660-221">hello nyilvános IP-cím egyben legfelső szintű erőforrás, így tekintheti meg a `azure group show`.</span><span class="sxs-lookup"><span data-stu-id="f8660-221">hello public IP address is also a top-level resource, so you can see it with `azure group show`.</span></span>

```azurecli
azure group show myResourceGroup --json | jq '.'
```

<span data-ttu-id="f8660-222">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-222">Output:</span></span>

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

<span data-ttu-id="f8660-223">További erőforrás adatait, többek között a hello teljesen minősített tartománynevét (FQDN) hello altartomány teljes hello segítségével kivizsgálhatja `azure network public-ip show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="f8660-223">You can investigate more resource details, including hello fully qualified domain name (FQDN) of hello subdomain, by using hello complete `azure network public-ip show` command.</span></span> <span data-ttu-id="f8660-224">hello nyilvános IP-cím erőforrás logikailag van rendelve, de egy adott cím még nincs hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="f8660-224">hello public IP address resource has been allocated logically, but a specific address has not yet been assigned.</span></span> <span data-ttu-id="f8660-225">tooobtain IP-cím, egy adott terheléselosztóhoz, amely jelenleg még nem hozott létre tooneed fog.</span><span class="sxs-lookup"><span data-stu-id="f8660-225">tooobtain an IP address, you're going tooneed a load balancer, which we have not yet created.</span></span>

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

<span data-ttu-id="f8660-226">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-226">Output:</span></span>

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

## <a name="create-a-load-balancer-and-ip-pools"></a><span data-ttu-id="f8660-227">Hozzon létre egy terheléselosztó és IP-készletek</span><span class="sxs-lookup"><span data-stu-id="f8660-227">Create a load balancer and IP pools</span></span>
<span data-ttu-id="f8660-228">Amikor létrehoz egy terhelés-kiegyenlítő, lehetővé teszi a toodistribute forgalmat több virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="f8660-228">When you create a load balancer, it enables you toodistribute traffic across multiple VMs.</span></span> <span data-ttu-id="f8660-229">Több virtuális gép toouser kérelmek hello eseményben karbantartás vagy nagy válaszolnak futtatásával redundancia tooyour alkalmazásokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f8660-229">It also provides redundancy tooyour application by running multiple VMs that respond toouser requests in hello event of maintenance or heavy loads.</span></span> <span data-ttu-id="f8660-230">hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer`:</span><span class="sxs-lookup"><span data-stu-id="f8660-230">hello following example creates a load balancer named `myLoadBalancer`:</span></span>

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

<span data-ttu-id="f8660-231">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-231">Output:</span></span>

```azurecli
info:    Executing command network lb create
+ Looking up hello load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

<span data-ttu-id="f8660-232">A terheléselosztó viszonylag üres, ezért az egyes IP-címkészletek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-232">Our load balancer is fairly empty, so let's create some IP pools.</span></span> <span data-ttu-id="f8660-233">A terheléselosztóhoz, hello előtér egyet, majd egy a hello háttér szeretnénk toocreate két IP-készletek.</span><span class="sxs-lookup"><span data-stu-id="f8660-233">We want toocreate two IP pools for our load balancer, one for hello front end and one for hello back end.</span></span> <span data-ttu-id="f8660-234">hello előtér-IP-címkészlet nyilvánosan láthatóvá.</span><span class="sxs-lookup"><span data-stu-id="f8660-234">hello front-end IP pool is publicly visible.</span></span> <span data-ttu-id="f8660-235">Akkor is hello hely toowhich azt rendelje hozzá a korábban létrehozott PIP hello.</span><span class="sxs-lookup"><span data-stu-id="f8660-235">It's also hello location toowhich we assign hello PIP that we created earlier.</span></span> <span data-ttu-id="f8660-236">Majd azt használja a hello háttér-olyan hely, hogy a virtuális gépek tooconnect.</span><span class="sxs-lookup"><span data-stu-id="f8660-236">Then we use hello back-end pool as a location for our VMs tooconnect to.</span></span> <span data-ttu-id="f8660-237">Ily módon hello áramolhasson az adatforgalom hello load balancer toohello virtuális gépek keresztül.</span><span class="sxs-lookup"><span data-stu-id="f8660-237">That way, hello traffic can flow through hello load balancer toohello VMs.</span></span>

<span data-ttu-id="f8660-238">Először hozzuk létre az előtér-IP-címkészletet.</span><span class="sxs-lookup"><span data-stu-id="f8660-238">First, let's create our front-end IP pool.</span></span> <span data-ttu-id="f8660-239">hello alábbi példa létrehoz egy előtér-készlet nevű `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="f8660-239">hello following example creates a front-end pool named `myFrontEndPool`:</span></span>

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

<span data-ttu-id="f8660-240">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-240">Output:</span></span>

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up hello load balancer "myLoadBalancer"
+ Looking up hello public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

<span data-ttu-id="f8660-241">Vegye figyelembe, hogyan használtuk hello `--public-ip-name` toopass kapcsoló hello `myPublicIP` , amely a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f8660-241">Note how we used hello `--public-ip-name` switch toopass in hello `myPublicIP` that we created earlier.</span></span> <span data-ttu-id="f8660-242">Hello nyilvános IP-cím hozzárendelése cím toohello terheléselosztó lehetővé teszi a tooreach a virtuális gépek hello interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="f8660-242">Assigning hello public IP address toohello load balancer allows you tooreach your VMs across hello Internet.</span></span>

<span data-ttu-id="f8660-243">Következő lépésként hozzon létre a második IP-címkészlet, ezúttal a háttér-forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="f8660-243">Next, let's create our second IP pool, this time for our back-end traffic.</span></span> <span data-ttu-id="f8660-244">hello alábbi példa létrehoz egy háttér címkészletet nevű `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="f8660-244">hello following example creates a back-end pool named `myBackEndPool`:</span></span>

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

<span data-ttu-id="f8660-245">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-245">Output:</span></span>

```azurecli
info:    Executing command network lb address-pool create
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

<span data-ttu-id="f8660-246">A képen látható, hogyan ennek során a terheléselosztó azáltal, hogy megtekinti a `azure network lb show` és hello JSON-kimenetét vizsgálata:</span><span class="sxs-lookup"><span data-stu-id="f8660-246">We can see how our load balancer is doing by looking with `azure network lb show` and examining hello JSON output:</span></span>

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

<span data-ttu-id="f8660-247">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-247">Output:</span></span>

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

## <a name="create-load-balancer-nat-rules"></a><span data-ttu-id="f8660-248">Terheléselosztó NAT-szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8660-248">Create load balancer NAT rules</span></span>
<span data-ttu-id="f8660-249">a terheléselosztó átfutó tooget forgalom toocreate hálózati cím címfordítási (NAT) meghatározott szabályok bejövő vagy kimenő műveletek szükséges.</span><span class="sxs-lookup"><span data-stu-id="f8660-249">tooget traffic flowing through our load balancer, we need toocreate network address translation (NAT) rules that specify either inbound or outbound actions.</span></span> <span data-ttu-id="f8660-250">Adja meg a hello protokoll toouse, majd leképezik a külső portok toointernal tetszés szerint.</span><span class="sxs-lookup"><span data-stu-id="f8660-250">You can specify hello protocol toouse, then map external ports toointernal ports as desired.</span></span> <span data-ttu-id="f8660-251">A környezet hozzuk létre egyes szabályokat, amelyek lehetővé teszik az SSH a load balancer tooour virtuális gépek keresztül.</span><span class="sxs-lookup"><span data-stu-id="f8660-251">For our environment, let's create some rules that allow SSH through our load balancer tooour VMs.</span></span> <span data-ttu-id="f8660-252">Beállítjuk TCP portok 4222 és 4223 toodirect tooTCP 22-es portot a virtuális gépeken (amely később létrehozhatunk).</span><span class="sxs-lookup"><span data-stu-id="f8660-252">We set up TCP ports 4222 and 4223 toodirect tooTCP port 22 on our VMs (which we create later).</span></span> <span data-ttu-id="f8660-253">hello alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleSSH1` toomap TCP port 4222 tooport 22:</span><span class="sxs-lookup"><span data-stu-id="f8660-253">hello following example creates a rule named `myLoadBalancerRuleSSH1` toomap TCP port 4222 tooport 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

<span data-ttu-id="f8660-254">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-254">Output:</span></span>

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up hello load balancer "myLoadBalancer"
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

<span data-ttu-id="f8660-255">Az SSH ismételje meg a második NAT-szabály hello eljárást.</span><span class="sxs-lookup"><span data-stu-id="f8660-255">Repeat hello procedure for your second NAT rule for SSH.</span></span> <span data-ttu-id="f8660-256">hello alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleSSH2` toomap TCP port 4223 tooport 22:</span><span class="sxs-lookup"><span data-stu-id="f8660-256">hello following example creates a rule named `myLoadBalancerRuleSSH2` toomap TCP port 4223 tooport 22:</span></span>

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

<span data-ttu-id="f8660-257">Most is habozzon, hogy webes forgalomban hello szabály csatlakoztatása tooour IP-készletek 80-as TCP-port NAT-szabályt.</span><span class="sxs-lookup"><span data-stu-id="f8660-257">Let's also go ahead and create a NAT rule for TCP port 80 for web traffic, hooking hello rule up tooour IP pools.</span></span> <span data-ttu-id="f8660-258">Ha azt a számítógéphez hello szabály tooan IP-készlet helyett hello szabály tooour virtuális gépek egyenkénti csatlakoztatása a Microsoft adja hozzá, vagy távolítsa el a virtuális gépek hello IP-készletből.</span><span class="sxs-lookup"><span data-stu-id="f8660-258">If we hook up hello rule tooan IP pool, instead of hooking up hello rule tooour VMs individually, we can add or remove VMs from hello IP pool.</span></span> <span data-ttu-id="f8660-259">hello terheléselosztó automatikusan beállítja a forgalom áramlását hello.</span><span class="sxs-lookup"><span data-stu-id="f8660-259">hello load balancer automatically adjusts hello flow of traffic.</span></span> <span data-ttu-id="f8660-260">hello alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleWeb` toomap TCP 80 tooport 80-as port:</span><span class="sxs-lookup"><span data-stu-id="f8660-260">hello following example creates a rule named `myLoadBalancerRuleWeb` toomap TCP port 80 tooport 80:</span></span>

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

<span data-ttu-id="f8660-261">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-261">Output:</span></span>

```azurecli
info:    Executing command network lb rule create
+ Looking up hello load balancer "myLoadBalancer"
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

## <a name="create-a-load-balancer-health-probe"></a><span data-ttu-id="f8660-262">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8660-262">Create a load balancer health probe</span></span>
<span data-ttu-id="f8660-263">Állapotfigyelő mintavételi rendszeresen ellenőrzi a virtuális gépek, a betöltés terheléselosztó toomake meg arról, hogy van működő és toorequests válaszol meghatározott mögötti hello.</span><span class="sxs-lookup"><span data-stu-id="f8660-263">A health probe periodically checks on hello VMs that are behind our load balancer toomake sure they're operating and responding toorequests as defined.</span></span> <span data-ttu-id="f8660-264">Ha nem, akkor most eltávolítja a művelet tooensure, hogy a felhasználók nem alatt irányítja a rendszer toothem.</span><span class="sxs-lookup"><span data-stu-id="f8660-264">If not, they're removed from operation tooensure that users aren't being directed toothem.</span></span> <span data-ttu-id="f8660-265">Megadhat egyéni ellenőrzések hello állapotmintáihoz, valamint időközök és az időkorlát-értékei.</span><span class="sxs-lookup"><span data-stu-id="f8660-265">You can define custom checks for hello health probe, along with intervals and timeout values.</span></span> <span data-ttu-id="f8660-266">Állapotteljesítmény kapcsolatos további információkért lásd: [terheléselosztó vizsgálat](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8660-266">For more information about health probes, see [Load Balancer probes](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f8660-267">hello alábbi példa létrehoz egy TCP állapotfigyelő megadott elnevezett `myHealthProbe`:</span><span class="sxs-lookup"><span data-stu-id="f8660-267">hello following example creates a TCP health probed named `myHealthProbe`:</span></span>

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

<span data-ttu-id="f8660-268">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-268">Output:</span></span>

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

<span data-ttu-id="f8660-269">Itt azt az állapot-ellenőrzési eredményeire a 15 másodperces időközönként megadott.</span><span class="sxs-lookup"><span data-stu-id="f8660-269">Here, we specified an interval of 15 seconds for our health checks.</span></span> <span data-ttu-id="f8660-270">Azt is, amelyeken legfeljebb négy mintavételt (egy perces) hello terheléselosztó úgy ítéli meg, hello üzemeltető már nem működik.</span><span class="sxs-lookup"><span data-stu-id="f8660-270">We can miss a maximum of four probes (one minute) before hello load balancer considers that hello host is no longer functioning.</span></span>

## <a name="verify-hello-load-balancer"></a><span data-ttu-id="f8660-271">Hello terheléselosztó ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f8660-271">Verify hello load balancer</span></span>
<span data-ttu-id="f8660-272">Most hello terheléselosztó-konfiguráció történik.</span><span class="sxs-lookup"><span data-stu-id="f8660-272">Now hello load balancer configuration is done.</span></span> <span data-ttu-id="f8660-273">Hello lépéseket, a következők:</span><span class="sxs-lookup"><span data-stu-id="f8660-273">Here are hello steps you took:</span></span>

1. <span data-ttu-id="f8660-274">Létrehozott egy terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="f8660-274">You created a load balancer.</span></span>
2. <span data-ttu-id="f8660-275">Létrehozott egy előtér-IP-címkészletet, és egy nyilvános IP-tooit rendelt.</span><span class="sxs-lookup"><span data-stu-id="f8660-275">You created a front-end IP pool and assigned a public IP tooit.</span></span>
3. <span data-ttu-id="f8660-276">Létrehozott egy háttér-IP-címkészlet, amely a virtuális gépek csatlakozni tud-e.</span><span class="sxs-lookup"><span data-stu-id="f8660-276">You created a back-end IP pool that VMs can connect to.</span></span>
4. <span data-ttu-id="f8660-277">NAT-szabályok, amelyek lehetővé teszik az SSH toohello virtuális gépek kezelésére, egy szabályt, amely lehetővé teszi, hogy a 80-as TCP-port a webalkalmazás együtt létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f8660-277">You created NAT rules that allow SSH toohello VMs for management, along with a rule that allows TCP port 80 for our web app.</span></span>
5. <span data-ttu-id="f8660-278">Hozzáadta a rendszerállapot mintavételi tooperiodically ellenőrzés hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="f8660-278">You added a health probe tooperiodically check hello VMs.</span></span> <span data-ttu-id="f8660-279">A állapotmintáihoz biztosítja, hogy a felhasználók ne próbáljon tooaccess, amely már nem működik, vagy tartalom.</span><span class="sxs-lookup"><span data-stu-id="f8660-279">This health probe ensures that users don't try tooaccess a VM that is no longer functioning or serving content.</span></span>

<span data-ttu-id="f8660-280">A terheléselosztó néz most tekintsük át:</span><span class="sxs-lookup"><span data-stu-id="f8660-280">Let's review what your load balancer looks like now:</span></span>

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

<span data-ttu-id="f8660-281">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-281">Output:</span></span>

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

## <a name="create-an-nic-toouse-with-hello-linux-vm"></a><span data-ttu-id="f8660-282">Hozzon létre egy hálózati adapter toouse hello Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="f8660-282">Create an NIC toouse with hello Linux VM</span></span>
<span data-ttu-id="f8660-283">Hálózati adapter, ezért programozott módon alkalmazhat szabályok tootheir használja.</span><span class="sxs-lookup"><span data-stu-id="f8660-283">NICs are programmatically available because you can apply rules tootheir use.</span></span> <span data-ttu-id="f8660-284">Egynél több is lehet.</span><span class="sxs-lookup"><span data-stu-id="f8660-284">You can also have more than one.</span></span> <span data-ttu-id="f8660-285">A következő hello `azure network nic create` hello NIC toohello terhelés háttér IP-címkészletet a számítógéphez, és hozzárendelheti a NAT-szabály toopermit SSH forgalom hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="f8660-285">In hello following `azure network nic create` command, you hook up hello NIC toohello load back-end IP pool and associate it with hello NAT rule toopermit SSH traffic.</span></span>

<span data-ttu-id="f8660-286">Cserélje le a hello `#####-###-###` szakaszok a saját Azure-előfizetés-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="f8660-286">Replace hello `#####-###-###` sections with your own Azure subscription ID.</span></span> <span data-ttu-id="f8660-287">Az előfizetés-azonosító hello kimenete jelenik meg `jq` hello erőforrások létrehozásakor vizsgálata során.</span><span class="sxs-lookup"><span data-stu-id="f8660-287">Your subscription ID is noted in hello output of `jq` when you examine hello resources you are creating.</span></span> <span data-ttu-id="f8660-288">Megtekintheti az előfizetés-Azonosítóval rendelkező `azure account list`.</span><span class="sxs-lookup"><span data-stu-id="f8660-288">You can also view your subscription ID with `azure account list`.</span></span>

<span data-ttu-id="f8660-289">hello alábbi példa létrehoz egy hálózati adapter nevű `myNic1`:</span><span class="sxs-lookup"><span data-stu-id="f8660-289">hello following example creates a NIC named `myNic1`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

<span data-ttu-id="f8660-290">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-290">Output:</span></span>

```azurecli
info:    Executing command network nic create
+ Looking up hello subnet "mySubnet"
+ Looking up hello network interface "myNic1"
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

<span data-ttu-id="f8660-291">Hello részletek hello erőforrás közvetlenül megvizsgálásával.</span><span class="sxs-lookup"><span data-stu-id="f8660-291">You can see hello details by examining hello resource directly.</span></span> <span data-ttu-id="f8660-292">Hello erőforrás hello segítségével vizsgálja meg `azure network nic show` parancs:</span><span class="sxs-lookup"><span data-stu-id="f8660-292">You examine hello resource by using hello `azure network nic show` command:</span></span>

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

<span data-ttu-id="f8660-293">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-293">Output:</span></span>

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

<span data-ttu-id="f8660-294">Most létrehozhatunk hello a második hálózati adapter, csatlakoztatás tooour háttér IP-címkészletben újra.</span><span class="sxs-lookup"><span data-stu-id="f8660-294">Now we create hello second NIC, hooking in tooour back-end IP pool again.</span></span> <span data-ttu-id="f8660-295">Ez idő hello második NAT-szabály lehetővé teszi a SSH-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="f8660-295">This time hello second NAT rule permits SSH traffic.</span></span> <span data-ttu-id="f8660-296">hello alábbi példa létrehoz egy hálózati adapter nevű `myNic2`:</span><span class="sxs-lookup"><span data-stu-id="f8660-296">hello following example creates a NIC named `myNic2`:</span></span>

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a><span data-ttu-id="f8660-297">Hálózati biztonsági csoport és a szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8660-297">Create a network security group and rules</span></span>
<span data-ttu-id="f8660-298">Hálózati biztonsági csoport létrehozása és bejövő hello meghatározó szabályok hozzáférési toohello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="f8660-298">Now we create a network security group and hello inbound rules that govern access toohello NIC.</span></span> <span data-ttu-id="f8660-299">Hálózati biztonsági csoport lehet alkalmazott tooa hálózati adapter vagy az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="f8660-299">A network security group can be applied tooa NIC or subnet.</span></span> <span data-ttu-id="f8660-300">Szabályok toocontrol hello áramló forgalmat a virtuális gépek mindkét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f8660-300">You define rules toocontrol hello flow of traffic in and out of your VMs.</span></span> <span data-ttu-id="f8660-301">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="f8660-301">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

<span data-ttu-id="f8660-302">Adjunk hello bejövő szabály hello NSG tooallow a bejövő kapcsolatok 22 (toosupport SSH) porton.</span><span class="sxs-lookup"><span data-stu-id="f8660-302">Let's add hello inbound rule for hello NSG tooallow inbound connections on port 22 (toosupport SSH).</span></span> <span data-ttu-id="f8660-303">hello alábbi példa létrehoz egy nevű szabályt `myNetworkSecurityGroupRuleSSH` tooallow TCP-port a 22:</span><span class="sxs-lookup"><span data-stu-id="f8660-303">hello following example creates a rule named `myNetworkSecurityGroupRuleSSH` tooallow TCP on port 22:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

<span data-ttu-id="f8660-304">Most adjuk hozzá hello bejövő szabály hello NSG tooallow a bejövő kapcsolatok (toosupport webes forgalom) 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="f8660-304">Now let's add hello inbound rule for hello NSG tooallow inbound connections on port 80 (toosupport web traffic).</span></span> <span data-ttu-id="f8660-305">hello alábbi példa létrehoz egy nevű szabályt `myNetworkSecurityGroupRuleHTTP` tooallow TCP 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="f8660-305">hello following example creates a rule named `myNetworkSecurityGroupRuleHTTP` tooallow TCP on port 80:</span></span>

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> <span data-ttu-id="f8660-306">hello bejövő forgalomra vonatkozó szabály egy szűrő a bejövő hálózati kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="f8660-306">hello inbound rule is a filter for inbound network connections.</span></span> <span data-ttu-id="f8660-307">Ebben a példában azt kötési hello NSG toohello virtuális gépek virtuális hálózati adapter, ami azt jelenti, hogy a kérelem tooport 22 áthalad toohello hálózati adapter a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="f8660-307">In this example, we bind hello NSG toohello VMs virtual NIC, which means that any request tooport 22 is passed through toohello NIC on our VM.</span></span> <span data-ttu-id="f8660-308">A bejövő szabály a hálózati kapcsolat, és nem kapcsolatos, a végpont melyik tárolókezelésének kapcsolatos a klasszikus üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="f8660-308">This inbound rule is about a network connection, and not about an endpoint, which is what it would be about in classic deployments.</span></span> <span data-ttu-id="f8660-309">tooopen port, meg kell hagyni hello `--source-port-range` túl beállítása "\*" (hello alapértelmezett érték) tooaccept kérelmeinek bejövő **bármely** a kért porton.</span><span class="sxs-lookup"><span data-stu-id="f8660-309">tooopen a port, you must leave hello `--source-port-range` set too'\*' (hello default value) tooaccept inbound requests from **any** requesting port.</span></span> <span data-ttu-id="f8660-310">Ezek általában dinamikus.</span><span class="sxs-lookup"><span data-stu-id="f8660-310">Ports are typically dynamic.</span></span>
>
>

## <a name="bind-toohello-nic"></a><span data-ttu-id="f8660-311">A hálózati adapter toohello kötése</span><span class="sxs-lookup"><span data-stu-id="f8660-311">Bind toohello NIC</span></span>
<span data-ttu-id="f8660-312">Kötési hello NSG toohello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="f8660-312">Bind hello NSG toohello NICs.</span></span> <span data-ttu-id="f8660-313">Igazolnia kell tooconnect a hálózati adapter a hálózati biztonsági csoporttal.</span><span class="sxs-lookup"><span data-stu-id="f8660-313">We need tooconnect our NICs with our network security group.</span></span> <span data-ttu-id="f8660-314">Mindkét parancsok, mind a hálózati adapterek toohook futtatása:</span><span class="sxs-lookup"><span data-stu-id="f8660-314">Run both commands, toohook up both of our NICs:</span></span>

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a><span data-ttu-id="f8660-315">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8660-315">Create an availability set</span></span>
<span data-ttu-id="f8660-316">Rendelkezésre állási a virtuális gépek készletek súgó terjedésének tartalék tartományok és a frissítési tartományok között.</span><span class="sxs-lookup"><span data-stu-id="f8660-316">Availability sets help spread your VMs across fault domains and upgrade domains.</span></span> <span data-ttu-id="f8660-317">Hozzon létre egy rendelkezésre állási készletét, a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="f8660-317">Let's create an availability set for your VMs.</span></span> <span data-ttu-id="f8660-318">hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="f8660-318">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

<span data-ttu-id="f8660-319">Tartalék tartományok definiálása, amelyek egy közös forrás- és hálózati kikapcsolás virtuális gépek csoportja.</span><span class="sxs-lookup"><span data-stu-id="f8660-319">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="f8660-320">Alapértelmezés szerint hello virtuális gépek a rendelkezésre állási csoport belül állítottak be toothree tartalék tartományok egymástól között.</span><span class="sxs-lookup"><span data-stu-id="f8660-320">By default, hello virtual machines that are configured within your availability set are separated across up toothree fault domains.</span></span> <span data-ttu-id="f8660-321">hello lényege, hogy a hardver probléma a tartalék tartományok valamelyikében nem befolyásolja a minden virtuális gép, amelyen fut. az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f8660-321">hello idea is that a hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span> <span data-ttu-id="f8660-322">Azure virtuális gépek elhelyezésekor azokat a rendelkezésre állási csoport automatikusan elosztja hello tartalék tartományok.</span><span class="sxs-lookup"><span data-stu-id="f8660-322">Azure automatically distributes VMs across hello fault domains when placing them in an availability set.</span></span>

<span data-ttu-id="f8660-323">Frissítési tartományok jelzi a virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportok ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f8660-323">Upgrade domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="f8660-324">hello sorrendben, ahol a frissítési tartományok újraindítása után előfordulhat, hogy nem szekvenciális tervezett karbantartás alatt, de egyszerre csak egy frissítés újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="f8660-324">hello order in which upgrade domains are rebooted might not be sequential during planned maintenance, but only one upgrade is rebooted at a time.</span></span> <span data-ttu-id="f8660-325">Ebben az esetben Azure automatikusan elosztása a virtuális gépek frissítési tartományok amikor egy rendelkezésre állási helyen helyezi el őket.</span><span class="sxs-lookup"><span data-stu-id="f8660-325">Again, Azure automatically distributes your VMs across upgrade domains when placing them in an availability site.</span></span>

<span data-ttu-id="f8660-326">Tudjon meg többet az [hello virtuális gépek rendelkezésre állásának kezelése](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8660-326">Read more about [managing hello availability of VMs](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="create-hello-linux-vms"></a><span data-ttu-id="f8660-327">Hello Linux virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8660-327">Create hello Linux VMs</span></span>
<span data-ttu-id="f8660-328">Internetről elérhető toosupport virtuális gépek létrehozott hello tárolási és hálózati erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f8660-328">You've created hello storage and network resources toosupport Internet-accessible VMs.</span></span> <span data-ttu-id="f8660-329">Most tegyük létrehozása a virtuális gépek és biztonságos őket egy SSH-kulcsban, amely nem tartozik jelszó.</span><span class="sxs-lookup"><span data-stu-id="f8660-329">Now let's create those VMs and secure them with an SSH key that doesn't have a password.</span></span> <span data-ttu-id="f8660-330">Ebben az esetben az oktatóanyagban módosítjuk az Ubuntu virtuális gép alapján hello toocreate legutóbbi LTS.</span><span class="sxs-lookup"><span data-stu-id="f8660-330">In this case, we're going toocreate an Ubuntu VM based on hello most recent LTS.</span></span> <span data-ttu-id="f8660-331">Azt az kép információk keresse meg a `azure vm image list`leírtak szerint [Azure Virtuálisgép-rendszerképekről keresése](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8660-331">We locate that image information by using `azure vm image list`, as described in [finding Azure VM images](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f8660-332">Azt a képfájl kijelölt hello paranccsal `azure vm image list westeurope canonical | grep LTS`.</span><span class="sxs-lookup"><span data-stu-id="f8660-332">We selected an image by using hello command `azure vm image list westeurope canonical | grep LTS`.</span></span> <span data-ttu-id="f8660-333">Ebben az esetben használjuk `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span><span class="sxs-lookup"><span data-stu-id="f8660-333">In this case, we use `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`.</span></span> <span data-ttu-id="f8660-334">Hello utolsó mezőben azt át `latest` , hogy a jövőben hello mindig kapunk hello legutóbbi build.</span><span class="sxs-lookup"><span data-stu-id="f8660-334">For hello last field, we pass `latest` so that in hello future we always get hello most recent build.</span></span> <span data-ttu-id="f8660-335">(használjuk hello karakterlánc `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span><span class="sxs-lookup"><span data-stu-id="f8660-335">(hello string we use is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).</span></span>

<span data-ttu-id="f8660-336">A következő lépés az ismerős tooanyone, akik már létre van hozva egy ssh rsa nyilvános és titkos kulcs párosítsa a Linux- vagy Mac használatával **ssh-keygen - t rsa -b 2048**.</span><span class="sxs-lookup"><span data-stu-id="f8660-336">This next step is familiar tooanyone who has already created an ssh rsa public and private key pair on Linux or Mac by using **ssh-keygen -t rsa -b 2048**.</span></span> <span data-ttu-id="f8660-337">Ha nincs a tanúsítvány kulcspárok a `~/.ssh` könyvtárában, akkor létrehozhat őket:</span><span class="sxs-lookup"><span data-stu-id="f8660-337">If you do not have any certificate key pairs in your `~/.ssh` directory, you can create them:</span></span>

* <span data-ttu-id="f8660-338">Hello segítségével automatikusan, `azure vm create --generate-ssh-keys` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f8660-338">Automatically, by using hello `azure vm create --generate-ssh-keys` option.</span></span>
* <span data-ttu-id="f8660-339">Manuálisan [hello utasításokat toocreate azokat saját kezűleg](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8660-339">Manually, by using [hello instructions toocreate them yourself](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f8660-340">Másik lehetőségként használhatja a hello `--admin-password` metódus tooauthenticate az SSH-kapcsolatok hello VM után jön létre.</span><span class="sxs-lookup"><span data-stu-id="f8660-340">Alternatively, you can use hello `--admin-password` method tooauthenticate your SSH connections after hello VM is created.</span></span> <span data-ttu-id="f8660-341">Ez a módszer általában kevésbé biztonságos.</span><span class="sxs-lookup"><span data-stu-id="f8660-341">This method is typically less secure.</span></span>

<span data-ttu-id="f8660-342">Az erőforrások és az adatokat a hello hozásával létrehozhatunk hello VM `azure vm create` parancs:</span><span class="sxs-lookup"><span data-stu-id="f8660-342">We create hello VM by bringing all our resources and information together with hello `azure vm create` command:</span></span>

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

<span data-ttu-id="f8660-343">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-343">Output:</span></span>

```azurecli
info:    Executing command vm create
+ Looking up hello VM "myVM1"
info:    Verifying hello public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using hello VM Size "Standard_DS1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Looking up hello storage account mystorageaccount
+ Looking up hello availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up hello NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in hello NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    hello storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by hello parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

<span data-ttu-id="f8660-344">Tooyour VM azonnal is elérheti az alapértelmezett SSH-kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="f8660-344">You can connect tooyour VM immediately by using your default SSH keys.</span></span> <span data-ttu-id="f8660-345">Ellenőrizze, hogy hello megfelelő portot adja meg, mivel azt még áthaladó hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="f8660-345">Make sure that you specify hello appropriate port since we're passing through hello load balancer.</span></span> <span data-ttu-id="f8660-346">(Az első virtuális gép, beállítjuk hello NAT szabály tooforward port 4222 tooour VM.)</span><span class="sxs-lookup"><span data-stu-id="f8660-346">(For our first VM, we set up hello NAT rule tooforward port 4222 tooour VM.)</span></span>

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

<span data-ttu-id="f8660-347">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-347">Output:</span></span>

```bash
hello authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

<span data-ttu-id="f8660-348">Lépjen tovább, és a második virtuális gép létrehozása a hello megegyező módon:</span><span class="sxs-lookup"><span data-stu-id="f8660-348">Go ahead and create your second VM in hello same manner:</span></span>

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

<span data-ttu-id="f8660-349">Ezután már használhatja a hello és `azure vm show myResourceGroup myVM1` mi létrehozott tooexamine parancsot.</span><span class="sxs-lookup"><span data-stu-id="f8660-349">And you can now use hello `azure vm show myResourceGroup myVM1` command tooexamine what you've created.</span></span> <span data-ttu-id="f8660-350">Ezen a ponton a Ubuntu virtuális gép egy terheléselosztó mögött futtatja, amely jelentkezhet be az SSH-kulcspárral csak a (mert a jelszavak le vannak tiltva) Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f8660-350">At this point, you're running your Ubuntu VMs behind a load balancer in Azure that you can sign into only with your SSH key pair (because passwords are disabled).</span></span> <span data-ttu-id="f8660-351">Telepítse a nginx vagy httpd, webalkalmazás üzembe helyezése és hello hálózati forgalom hello load balancer tooboth hello virtuális gépek keresztül.</span><span class="sxs-lookup"><span data-stu-id="f8660-351">You can install nginx or httpd, deploy a web app, and see hello traffic flow through hello load balancer tooboth of hello VMs.</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

<span data-ttu-id="f8660-352">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f8660-352">Output:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up hello VM "TestVM1"
+ Looking up hello NIC "myNic1"
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


## <a name="export-hello-environment-as-a-template"></a><span data-ttu-id="f8660-353">Sablonként exportálja a hello környezet</span><span class="sxs-lookup"><span data-stu-id="f8660-353">Export hello environment as a template</span></span>
<span data-ttu-id="f8660-354">Most, hogy tartalmazó hello további fejlesztési környezet létrehozása ebben a környezetben, mi történik, ha azt szeretné, hogy toocreate el paramétereket, vagy a megfelelő az éles környezetben?</span><span class="sxs-lookup"><span data-stu-id="f8660-354">Now that you have built out this environment, what if you want toocreate an additional development environment with hello same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="f8660-355">Erőforrás-kezelő a környezet összes hello paramétereit meghatározó JSON-sablonokat használ.</span><span class="sxs-lookup"><span data-stu-id="f8660-355">Resource Manager uses JSON templates that define all hello parameters for your environment.</span></span> <span data-ttu-id="f8660-356">A JSON-sablon Vezérlőpultjának kimenő teljes környezetek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8660-356">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="f8660-357">Is [JSON sablonok létrehozása manuálisan](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy a meglévő környezetben toocreate hello JSON-sablon exportálása meg:</span><span class="sxs-lookup"><span data-stu-id="f8660-357">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment toocreate hello JSON template for you:</span></span>

```azurecli
azure group export --name myResourceGroup
```

<span data-ttu-id="f8660-358">Ezzel a paranccsal létrejön hello `myResourceGroup.json` az aktuális munkakönyvtárban fájlban.</span><span class="sxs-lookup"><span data-stu-id="f8660-358">This command creates hello `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="f8660-359">Ha a sablon alapján hoz létre egy olyan környezetben, kéri összes hello erőforrás neveit, beleértve a hello hello terheléselosztóhoz, a hálózati illesztőkre vagy virtuális gépek neveit.</span><span class="sxs-lookup"><span data-stu-id="f8660-359">When you create an environment from this template, you are prompted for all hello resource names, including hello names for hello load balancer, network interfaces, or VMs.</span></span> <span data-ttu-id="f8660-360">Töltheti fel ezeket a neveket a sablon fájlban adja hozzá a hello `-p` vagy `--includeParameterDefaultValue` paraméter toohello `azure group export` korábban bemutatott parancsot.</span><span class="sxs-lookup"><span data-stu-id="f8660-360">You can populate these names in your template file by adding hello `-p` or `--includeParameterDefaultValue` parameter toohello `azure group export` command that was shown earlier.</span></span> <span data-ttu-id="f8660-361">A JSON sablonok toospecify hello erőforrás nevét, szerkesztése vagy [parameters.json fájl létrehozása](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , amely meghatározza a hello erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="f8660-361">Edit your JSON template toospecify hello resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies hello resource names.</span></span>

<span data-ttu-id="f8660-362">a sablon alapján környezet toocreate:</span><span class="sxs-lookup"><span data-stu-id="f8660-362">toocreate an environment from your template:</span></span>

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

<span data-ttu-id="f8660-363">Érdemes lehet tooread [kapcsolatos további sablonokból toodeploy](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8660-363">You might want tooread [more about how toodeploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f8660-364">További tudnivalók tooincrementally frissítés környezetekben, hogyan hello paraméterek fájlt használja, és sablonok érheti el egyetlen tárolási helyet.</span><span class="sxs-lookup"><span data-stu-id="f8660-364">Learn about how tooincrementally update environments, use hello parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8660-365">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8660-365">Next steps</span></span>
<span data-ttu-id="f8660-366">Most már készen áll a hálózati összetevők és a virtuális gépek használata toobegin most.</span><span class="sxs-lookup"><span data-stu-id="f8660-366">Now you're ready toobegin working with multiple networking components and VMs.</span></span> <span data-ttu-id="f8660-367">Ki az alkalmazást a minta környezet toobuild bevezetett hello alapösszetevőket itt segítségével is használhatók.</span><span class="sxs-lookup"><span data-stu-id="f8660-367">You can use this sample environment toobuild out your application by using hello core components introduced here.</span></span>
