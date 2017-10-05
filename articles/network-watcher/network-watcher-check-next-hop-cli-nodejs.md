---
title: "Azure hálózati figyelő következő ugrás - Azure CLI 1.0 a következő ugrás található |} Microsoft Docs"
description: "Ez a cikk leírja, hogyan megtalálhatja a következő ugrás típusa van, és használó Azure parancssori felület használatával, a következő ugrás IP-cím."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: ff88e945060ae033717ceb29db1352e112f05a3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="68cde-103">Megtudhatja, milyen a következő ugrás típusa a következő ugrás funkció használata az Azure CLI 1.0 Azure hálózati figyelőt használ</span><span class="sxs-lookup"><span data-stu-id="68cde-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="68cde-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="68cde-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="68cde-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="68cde-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="68cde-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="68cde-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="68cde-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="68cde-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="68cde-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="68cde-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="68cde-109">Következő ugrás csak a hálózati figyelő, amely a képességét get biztosít, a következő ugrás típusa és az IP-cím a megadott virtuális gép alapján.</span><span class="sxs-lookup"><span data-stu-id="68cde-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="68cde-110">Ez a szolgáltatás akkor hasznos, meghatározni, hogy ha egy virtuális gép elhagyó forgalomra halad át egy átjárót, az interneten vagy a virtuális hálózatok a cél eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="68cde-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

<span data-ttu-id="68cde-111">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="68cde-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="68cde-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="68cde-112">Before you begin</span></span>

<span data-ttu-id="68cde-113">Ebben a forgatókönyvben az Azure parancssori felület használandó keresse meg a következő ugrás típusa és az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="68cde-113">In this scenario, you will use the Azure CLI to find the next hop type and IP address.</span></span>

<span data-ttu-id="68cde-114">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="68cde-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="68cde-115">A forgatókönyv feltételezi, hogy létezik-e egy erőforráscsoportot, egy érvényes virtuális géppel használandó.</span><span class="sxs-lookup"><span data-stu-id="68cde-115">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="68cde-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="68cde-116">Scenario</span></span>

<span data-ttu-id="68cde-117">A forgatókönyv, a cikkben szereplő használja a következő ugrás, egyik funkciója, amely a következő ugrás típusa és az IP-cím erőforrás megkeresése hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="68cde-117">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="68cde-118">Következő ugrás kapcsolatos további információkért látogasson el a [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68cde-118">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="68cde-119">Következő ugrás beolvasása</span><span class="sxs-lookup"><span data-stu-id="68cde-119">Get Next Hop</span></span>

<span data-ttu-id="68cde-120">A következő ugrás nevezzük eléréséhez a `azure netowrk watcher next-hop` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="68cde-120">To get the next hop we call the `azure netowrk watcher next-hop` cmdlet.</span></span> <span data-ttu-id="68cde-121">Azt adja át a parancsmag a hálózati figyelőt erőforráscsoport NetworkWatcher, virtuális gép azonosítója, IP-forráscím, és az IP-címre.</span><span class="sxs-lookup"><span data-stu-id="68cde-121">We pass the cmdlet the Network Watcher resource group, the NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="68cde-122">Ebben a példában a cél IP-címét, hogy egy virtuális Gépet egy másik virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="68cde-122">In this example, the destination IP address is to a VM in another virtual network.</span></span> <span data-ttu-id="68cde-123">Nincs a virtuális hálózati átjáró a két virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="68cde-123">There is a virtual network gateway between the two virtual networks.</span></span> 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
<span data-ttu-id="68cde-124">Ha a virtuális Gépnek több hálózati adapter legyen, és IP-továbbítás engedélyezve van, bármely, a hálózati adapterek, akkor a hálózati adapter paramétert (-i nic-id) meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="68cde-124">If the VM has multiple NICs and IP forwarding is enabled on any of the NICs, then the NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="68cde-125">Ellenkező esetben nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="68cde-125">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="68cde-126">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="68cde-126">Review results</span></span>

<span data-ttu-id="68cde-127">Amikor végzett, a eredményei.</span><span class="sxs-lookup"><span data-stu-id="68cde-127">When complete, the results are provided.</span></span> <span data-ttu-id="68cde-128">Továbbá az erőforrás típusa a következő ugrás IP-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="68cde-128">The next hop IP address is returned as well as the type of resource it is.</span></span>

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
```

<span data-ttu-id="68cde-129">Az alábbi listában a jelenleg rendelkezésre álló NextHopType értékeket mutatja:</span><span class="sxs-lookup"><span data-stu-id="68cde-129">The following list shows the currently available NextHopType values:</span></span>

<span data-ttu-id="68cde-130">**Következő ugrás típusa**</span><span class="sxs-lookup"><span data-stu-id="68cde-130">**Next Hop Type**</span></span>

* <span data-ttu-id="68cde-131">Internet</span><span class="sxs-lookup"><span data-stu-id="68cde-131">Internet</span></span>
* <span data-ttu-id="68cde-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="68cde-132">VirtualAppliance</span></span>
* <span data-ttu-id="68cde-133">Pedig</span><span class="sxs-lookup"><span data-stu-id="68cde-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="68cde-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="68cde-134">VnetLocal</span></span>
* <span data-ttu-id="68cde-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="68cde-135">HyperNetGateway</span></span>
* <span data-ttu-id="68cde-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="68cde-136">VnetPeering</span></span>
* <span data-ttu-id="68cde-137">None</span><span class="sxs-lookup"><span data-stu-id="68cde-137">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="68cde-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="68cde-138">Next steps</span></span>

<span data-ttu-id="68cde-139">Megtudhatja, hogyan ellátogatva programozott módon tekintse át a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="68cde-139">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
