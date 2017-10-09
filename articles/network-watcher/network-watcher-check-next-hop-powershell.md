---
title: "az Azure hálózati figyelő következő ugrás - PowerShell aaaFind a következő ugrás |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan található milyen hello a következő ugrás típusa, és ip-cím használatával a következő ugrás PowerShell használatával."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fdb0b4a02d95fc45c103fe952fc1afa095414c18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-powershell"></a><span data-ttu-id="b9fca-103">Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használ az Azure hálózati figyelőt PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b9fca-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="b9fca-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b9fca-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="b9fca-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b9fca-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="b9fca-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b9fca-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="b9fca-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b9fca-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="b9fca-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="b9fca-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="b9fca-109">Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b9fca-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="b9fca-110">A szolgáltatás akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.</span><span class="sxs-lookup"><span data-stu-id="b9fca-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b9fca-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b9fca-111">Before you begin</span></span>

<span data-ttu-id="b9fca-112">Ebben a forgatókönyvben hello Azure portál toofind hello következő ugrás típusa és IP-címet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b9fca-112">In this scenario, you will use hello Azure portal toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="b9fca-113">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="b9fca-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="b9fca-114">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="b9fca-114">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="b9fca-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="b9fca-115">Scenario</span></span>

<span data-ttu-id="b9fca-116">a cikkben szereplő hello forgatókönyv használja a következő ugrás, egyik funkciója, amely megkeresi hello következő ugrás típusa és IP-cím erőforrás hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="b9fca-116">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="b9fca-117">toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b9fca-117">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="b9fca-118">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9fca-118">Retrieve Network Watcher</span></span>

<span data-ttu-id="b9fca-119">hello első lépéseként tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="b9fca-119">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="b9fca-120">Hello `$networkWatcher` változó átadása toohello következő ugrás parancsmag ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="b9fca-120">hello `$networkWatcher` variable is passed toohello next hop verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a><span data-ttu-id="b9fca-121">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9fca-121">Get a virtual machine</span></span>

<span data-ttu-id="b9fca-122">Következő ugrás a virtuális gép hello következő ugrás és hello hello következő ugrás IP-címét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b9fca-122">Next hop returns hello next hop and hello IP address of hello next hop from a virtual machine.</span></span> <span data-ttu-id="b9fca-123">A virtuális gép azonosítóját hello parancsmag szükség.</span><span class="sxs-lookup"><span data-stu-id="b9fca-123">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="b9fca-124">Ha már ismeri hello virtuális gép toouse hello azonosítója, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="b9fca-124">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> <span data-ttu-id="b9fca-125">Következő ugrás megköveteli, hogy hello VM erőforrás toorun van lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="b9fca-125">Next hop requires that hello VM resource is allocated toorun.</span></span>

## <a name="get-hello-network-interfaces"></a><span data-ttu-id="b9fca-126">Hello hálózati illesztők beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9fca-126">Get hello network interfaces</span></span>

<span data-ttu-id="b9fca-127">hello hello virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk hello hálózati adaptert egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="b9fca-127">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="b9fca-128">Ha már ismeri a hello IP-címet, amelyet szeretne tootest hello virtuális gépen ezt a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="b9fca-128">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a><span data-ttu-id="b9fca-129">Következő ugrás beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9fca-129">Get Next Hop</span></span>

<span data-ttu-id="b9fca-130">Most közvetlen telepítésnek hello `Get-AzureRmNetworkWatcherNextHop` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b9fca-130">Now we call hello `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span></span> <span data-ttu-id="b9fca-131">Azt adja át a hello parancsmag hello hálózati figyelőt, virtuális gép azonosítója, forrás IP-címet, és a cél IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b9fca-131">We pass hello cmdlet hello Network Watcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="b9fca-132">Ebben a példában a hello cél IP-cím tooa virtuális gép egy másik virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="b9fca-132">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="b9fca-133">Nincs a virtuális hálózati átjáró hello virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="b9fca-133">There is a virtual network gateway between hello two virtual networks.</span></span>

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a><span data-ttu-id="b9fca-134">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="b9fca-134">Review results</span></span>

<span data-ttu-id="b9fca-135">Amikor végzett, hello eredményei.</span><span class="sxs-lookup"><span data-stu-id="b9fca-135">When complete, hello results are provided.</span></span> <span data-ttu-id="b9fca-136">továbbá az erőforrás típusát hello hello következő ugrás IP-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b9fca-136">hello next hop IP address is returned as well as hello type of resource it is.</span></span> <span data-ttu-id="b9fca-137">Az ebben az esetben is hello virtuális hálózati átjáró hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b9fca-137">In this scenario, it is hello public IP address of hello virtual network gateway.</span></span>

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

<span data-ttu-id="b9fca-138">hello alábbi lista mutatja azokat hello jelenleg rendelkezésre álló NextHopType értékeket:</span><span class="sxs-lookup"><span data-stu-id="b9fca-138">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="b9fca-139">**Következő ugrás típusa**</span><span class="sxs-lookup"><span data-stu-id="b9fca-139">**Next Hop Type**</span></span>

* <span data-ttu-id="b9fca-140">Internet</span><span class="sxs-lookup"><span data-stu-id="b9fca-140">Internet</span></span>
* <span data-ttu-id="b9fca-141">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="b9fca-141">VirtualAppliance</span></span>
* <span data-ttu-id="b9fca-142">Pedig</span><span class="sxs-lookup"><span data-stu-id="b9fca-142">VirtualNetworkGateway</span></span>
* <span data-ttu-id="b9fca-143">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="b9fca-143">VnetLocal</span></span>
* <span data-ttu-id="b9fca-144">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="b9fca-144">HyperNetGateway</span></span>
* <span data-ttu-id="b9fca-145">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="b9fca-145">VnetPeering</span></span>
* <span data-ttu-id="b9fca-146">None</span><span class="sxs-lookup"><span data-stu-id="b9fca-146">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9fca-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9fca-147">Next steps</span></span>

<span data-ttu-id="b9fca-148">Megtudhatja, hogyan tooreview programozott módon látogasson el a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="b9fca-148">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

















