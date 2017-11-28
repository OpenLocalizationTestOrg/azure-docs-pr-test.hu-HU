---
title: "az Azure hálózati figyelő következő ugrás - Azure CLI 1.0 aaaFind a következő ugrás |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan található milyen hello a következő ugrás típusa, és ip-cím használatával a következő Ugrás az Azure parancssori felület használatával."
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
ms.openlocfilehash: 54124c051021413695d70ba93c370605abc6ebbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="7e2da-103">Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használata az Azure CLI 1.0 Azure hálózati figyelőt használ</span><span class="sxs-lookup"><span data-stu-id="7e2da-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="7e2da-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7e2da-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="7e2da-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e2da-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="7e2da-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7e2da-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="7e2da-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7e2da-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="7e2da-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="7e2da-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="7e2da-109">Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet.</span><span class="sxs-lookup"><span data-stu-id="7e2da-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="7e2da-110">A szolgáltatás akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.</span><span class="sxs-lookup"><span data-stu-id="7e2da-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

<span data-ttu-id="7e2da-111">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="7e2da-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7e2da-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="7e2da-112">Before you begin</span></span>

<span data-ttu-id="7e2da-113">Ebben a forgatókönyvben hello Azure CLI toofind hello következő ugrás típusa és IP-címet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="7e2da-113">In this scenario, you will use hello Azure CLI toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="7e2da-114">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="7e2da-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="7e2da-115">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="7e2da-115">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="7e2da-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="7e2da-116">Scenario</span></span>

<span data-ttu-id="7e2da-117">a cikkben szereplő hello forgatókönyv használja a következő ugrás, egyik funkciója, amely megkeresi hello következő ugrás típusa és IP-cím erőforrás hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="7e2da-117">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="7e2da-118">toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e2da-118">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="7e2da-119">Következő ugrás beolvasása</span><span class="sxs-lookup"><span data-stu-id="7e2da-119">Get Next Hop</span></span>

<span data-ttu-id="7e2da-120">tooget hello következő ugrás hello nevezzük `azure netowrk watcher next-hop` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7e2da-120">tooget hello next hop we call hello `azure netowrk watcher next-hop` cmdlet.</span></span> <span data-ttu-id="7e2da-121">Azt adja át a hello parancsmag hello hálózati figyelőt erőforráscsoport, a hello NetworkWatcher, a virtuális gép azonosítója, a forrás IP-cím és a cél IP-cím.</span><span class="sxs-lookup"><span data-stu-id="7e2da-121">We pass hello cmdlet hello Network Watcher resource group, hello NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="7e2da-122">Ebben a példában a hello cél IP-cím tooa virtuális gép egy másik virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="7e2da-122">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="7e2da-123">Nincs a virtuális hálózati átjáró hello virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="7e2da-123">There is a virtual network gateway between hello two virtual networks.</span></span> 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
<span data-ttu-id="7e2da-124">Ha IP-továbbítás engedélyezve van a hálózati adapterek hello hello virtuális gép több hálózati adapterrel rendelkezik, majd hello NIC paramétert (-i nic-id) meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="7e2da-124">If hello VM has multiple NICs and IP forwarding is enabled on any of hello NICs, then hello NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="7e2da-125">Ellenkező esetben nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="7e2da-125">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="7e2da-126">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="7e2da-126">Review results</span></span>

<span data-ttu-id="7e2da-127">Amikor végzett, hello eredményei.</span><span class="sxs-lookup"><span data-stu-id="7e2da-127">When complete, hello results are provided.</span></span> <span data-ttu-id="7e2da-128">továbbá az erőforrás típusát hello hello következő ugrás IP-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7e2da-128">hello next hop IP address is returned as well as hello type of resource it is.</span></span>

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
```

<span data-ttu-id="7e2da-129">hello alábbi lista mutatja azokat hello jelenleg rendelkezésre álló NextHopType értékeket:</span><span class="sxs-lookup"><span data-stu-id="7e2da-129">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="7e2da-130">**Következő ugrás típusa**</span><span class="sxs-lookup"><span data-stu-id="7e2da-130">**Next Hop Type**</span></span>

* <span data-ttu-id="7e2da-131">Internet</span><span class="sxs-lookup"><span data-stu-id="7e2da-131">Internet</span></span>
* <span data-ttu-id="7e2da-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="7e2da-132">VirtualAppliance</span></span>
* <span data-ttu-id="7e2da-133">Pedig</span><span class="sxs-lookup"><span data-stu-id="7e2da-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="7e2da-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="7e2da-134">VnetLocal</span></span>
* <span data-ttu-id="7e2da-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="7e2da-135">HyperNetGateway</span></span>
* <span data-ttu-id="7e2da-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="7e2da-136">VnetPeering</span></span>
* <span data-ttu-id="7e2da-137">None</span><span class="sxs-lookup"><span data-stu-id="7e2da-137">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e2da-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e2da-138">Next steps</span></span>

<span data-ttu-id="7e2da-139">Megtudhatja, hogyan tooreview programozott módon látogasson el a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="7e2da-139">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
