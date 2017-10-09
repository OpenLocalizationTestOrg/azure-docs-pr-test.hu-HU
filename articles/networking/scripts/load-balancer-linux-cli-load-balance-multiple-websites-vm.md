---
title: "aaaAzure CLI parancsfájl minta - terhelésének elosztása több webhely, az Azure CLI hello |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - terhelésének elosztása több webhelyek toohello ugyanahhoz a virtuális géphez"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 136da5d1783fb9f9dc87f1ffad8eec7b95c6bd7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a><span data-ttu-id="ab2c8-103">Terhelésének elosztása több webhely</span><span class="sxs-lookup"><span data-stu-id="ab2c8-103">Load balance multiple websites</span></span>

<span data-ttu-id="ab2c8-104">Parancsfájl a következő példában két virtuális gépekkel (VM), amelyek egy rendelkezésre állási csoport tagjai hoz létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-104">This script sample creates a virtual network with two virtual machines (VM) that are members of an availability set.</span></span> <span data-ttu-id="ab2c8-105">A terheléselosztó irányítja a forgalmat a két külön IP-címek toohello két virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-105">A load balancer directs traffic for two separate IP addresses toohello two VMs.</span></span> <span data-ttu-id="ab2c8-106">Hello a parancsfájl futtatását, miután sikerült web server szoftver toohello virtuális gépek telepítése, és mindegyiket a saját IP-cím több webhely üzemeltetésére.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-106">After running hello script, you could deploy web server software toohello VMs and host multiple web sites, each with its own IP address.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ab2c8-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ab2c8-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a><span data-ttu-id="ab2c8-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-108">Clean up deployment</span></span> 

<span data-ttu-id="ab2c8-109">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="ab2c8-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ab2c8-110">Script explanation</span></span>

<span data-ttu-id="ab2c8-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, virtuális hálózati terheléselosztó, és minden kapcsolódó erőforrások betöltése hello.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-111">This script uses hello following commands toocreate a resource group, virtual network, load balancer, and all related resources.</span></span> <span data-ttu-id="ab2c8-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ab2c8-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="ab2c8-113">Command</span></span> | <span data-ttu-id="ab2c8-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ab2c8-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ab2c8-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ab2c8-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ab2c8-117">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="ab2c8-118">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="ab2c8-119">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="ab2c8-120">Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="ab2c8-121">az hálózati terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-121">az network lb create</span></span>](https://docs.microsoft.com/cli/azure/network/lb#create) | <span data-ttu-id="ab2c8-122">Egy Azure terheléselosztót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-122">Creates an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="ab2c8-123">az hálózati lb mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-123">az network lb probe create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | <span data-ttu-id="ab2c8-124">Terheléselosztói mintavétel létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-124">Creates a load balancer probe.</span></span> <span data-ttu-id="ab2c8-125">Terheléselosztói mintavétel használt toomonitor hello load balancer készlet minden egyes virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-125">A load balancer probe is used toomonitor each VM in hello load balancer set.</span></span> <span data-ttu-id="ab2c8-126">Ha minden virtuális gép elérhetetlenné válik, akkor kimenő forgalomról nem útválasztásos toohello VM.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-126">If any VM becomes inaccessible, traffic is not routed toohello VM.</span></span> |
| [<span data-ttu-id="ab2c8-127">az hálózati terheléselosztó szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-127">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="ab2c8-128">Létrehoz egy terheléselosztó szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-128">Creates a load balancer rule.</span></span> <span data-ttu-id="ab2c8-129">Ez a példa létrejön egy szabály 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-129">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="ab2c8-130">HTTP-forgalom érkezik hello terheléselosztót, mert már irányított tooport 80 hello load balancer set hello virtuális gépeinek egyikét.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-130">As HTTP traffic arrives at hello load balancer, it is routed tooport 80 one of hello VMs in hello load balancer set.</span></span> |
| [<span data-ttu-id="ab2c8-131">az előtér-IP-hálózati terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-131">az network lb frontend-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | <span data-ttu-id="ab2c8-132">Hozzon létre egy hello terheléselosztó előtérbeli IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-132">Create a frontend IP address for hello Load Balancer.</span></span> |
| [<span data-ttu-id="ab2c8-133">az hálózati lb-címkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-133">az network lb address-pool create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | <span data-ttu-id="ab2c8-134">Létrehoz egy háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-134">Creates a backend address pool.</span></span> |
| [<span data-ttu-id="ab2c8-135">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-135">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="ab2c8-136">A virtuális hálózati kártya létrehozza, és csatolja toohello virtuális hálózati és alhálózati.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-136">Creates a virtual network card and attaches it toohello virtual network, and subnet.</span></span> |
| [<span data-ttu-id="ab2c8-137">az virtuális gép rendelkezésre állási-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-137">az vm availability-set create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="ab2c8-138">Létrehoz egy rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-138">Creates an availability set.</span></span> <span data-ttu-id="ab2c8-139">Rendelkezésre állási készletek elosztásával hello virtuális gépek a fizikai erőforrások között, úgy, hogy hiba esetén nem történik teljes készletét megtisztítja hello alkalmazás hasznos üzemidő biztosítása.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-139">Availability sets ensure application uptime by spreading hello virtual machines across physical resources such that if failure occurs, hello entire set is not effected.</span></span> |
| [<span data-ttu-id="ab2c8-140">az hálózati hálózati adapter ip-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-140">az network nic ip-config create</span></span>](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | <span data-ttu-id="ab2c8-141">Létrehoz egy IP-confiuration.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-141">Creates an IP confiuration.</span></span> <span data-ttu-id="ab2c8-142">Hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic szolgáltatás engedélyezve van, az előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-142">You must have hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic feature enabled for your subscription.</span></span> <span data-ttu-id="ab2c8-143">Csak egy konfigurációs hello elsődleges IP-konfiguráció / NIC hello--ellenőrizze elsődleges jelző használatával ki lehet.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-143">Only one configuration may be designated as hello primary IP configuration per NIC, using hello --make-primary flag.</span></span> |
| [<span data-ttu-id="ab2c8-144">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2c8-144">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="ab2c8-145">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-145">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="ab2c8-146">Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-146">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="ab2c8-147">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="ab2c8-147">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="ab2c8-148">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="ab2c8-148">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ab2c8-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab2c8-149">Next steps</span></span>

<span data-ttu-id="ab2c8-150">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ab2c8-150">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ab2c8-151">További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ab2c8-151">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
