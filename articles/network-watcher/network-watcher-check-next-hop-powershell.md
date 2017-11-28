---
title: "Következő Ugrás az Azure hálózati figyelő következő ugrás - PowerShell található |} Microsoft Docs"
description: "Ez a cikk leírja, hogyan megtalálhatja a következő ugrás típusa van, és használja a PowerShell használatával, a következő ugrás IP-címet."
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
ms.openlocfilehash: 00161e7c6fb4becdb7d8eab266fa27128e50f8ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-powershell"></a><span data-ttu-id="35cd9-103">Megtudhatja, milyen a következő ugrás típusa a következő ugrás funkció használ az Azure hálózati figyelőt PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="35cd9-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="35cd9-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="35cd9-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="35cd9-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="35cd9-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="35cd9-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="35cd9-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="35cd9-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="35cd9-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="35cd9-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="35cd9-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="35cd9-109">Következő ugrás csak a hálózati figyelő, amely a képességét get biztosít, a következő ugrás típusa és az IP-cím a megadott virtuális gép alapján.</span><span class="sxs-lookup"><span data-stu-id="35cd9-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="35cd9-110">Ez a szolgáltatás akkor hasznos, meghatározni, hogy ha egy virtuális gép elhagyó forgalomra halad át egy átjárót, az interneten vagy a virtuális hálózatok a cél eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="35cd9-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="35cd9-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="35cd9-111">Before you begin</span></span>

<span data-ttu-id="35cd9-112">Ebben a forgatókönyvben szüksége lesz az Azure-portálon a következő ugrás típusa és az IP-cím kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="35cd9-112">In this scenario, you will use the Azure portal to find the next hop type and IP address.</span></span>

<span data-ttu-id="35cd9-113">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="35cd9-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="35cd9-114">A forgatókönyv feltételezi, hogy létezik-e egy erőforráscsoportot, egy érvényes virtuális géppel használandó.</span><span class="sxs-lookup"><span data-stu-id="35cd9-114">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="35cd9-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="35cd9-115">Scenario</span></span>

<span data-ttu-id="35cd9-116">A forgatókönyv, a cikkben szereplő használja a következő ugrás, egyik funkciója, amely a következő ugrás típusa és az IP-cím erőforrás megkeresése hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="35cd9-116">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="35cd9-117">Következő ugrás kapcsolatos további információkért látogasson el a [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="35cd9-117">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="35cd9-118">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="35cd9-118">Retrieve Network Watcher</span></span>

<span data-ttu-id="35cd9-119">Az első lépés a hálózati figyelőt példányának lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="35cd9-119">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="35cd9-120">A `$networkWatcher` változó átadása a következő ugrási parancsmag ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="35cd9-120">The `$networkWatcher` variable is passed to the next hop verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a><span data-ttu-id="35cd9-121">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="35cd9-121">Get a virtual machine</span></span>

<span data-ttu-id="35cd9-122">Következő ugrás a virtuális gép a következő ugrás és a következő ugrás IP-címét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="35cd9-122">Next hop returns the next hop and the IP address of the next hop from a virtual machine.</span></span> <span data-ttu-id="35cd9-123">A virtuális gép azonosítóját a parancsmag szükség.</span><span class="sxs-lookup"><span data-stu-id="35cd9-123">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="35cd9-124">Ha már ismeri az Azonosítót a virtuális gép használja, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="35cd9-124">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> <span data-ttu-id="35cd9-125">Következő ugrás megköveteli, hogy a virtuális gép erőforrásához lefoglalt futtatásához.</span><span class="sxs-lookup"><span data-stu-id="35cd9-125">Next hop requires that the VM resource is allocated to run.</span></span>

## <a name="get-the-network-interfaces"></a><span data-ttu-id="35cd9-126">A hálózati adapterek beolvasása</span><span class="sxs-lookup"><span data-stu-id="35cd9-126">Get the network interfaces</span></span>

<span data-ttu-id="35cd9-127">A virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk a hálózati adaptert egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="35cd9-127">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="35cd9-128">Ha már ismeri a virtuális gépen vizsgálni kívánt IP-cím, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="35cd9-128">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a><span data-ttu-id="35cd9-129">Következő ugrás beolvasása</span><span class="sxs-lookup"><span data-stu-id="35cd9-129">Get Next Hop</span></span>

<span data-ttu-id="35cd9-130">Most közvetlen telepítésnek a `Get-AzureRmNetworkWatcherNextHop` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="35cd9-130">Now we call the `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span></span> <span data-ttu-id="35cd9-131">Azt adja át a parancsmag a hálózati figyelőt, a virtuális gép azonosítója, a forrás IP-cím és a cél IP-címét.</span><span class="sxs-lookup"><span data-stu-id="35cd9-131">We pass the cmdlet the Network Watcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="35cd9-132">Ebben a példában a cél IP-címét, hogy egy virtuális Gépet egy másik virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="35cd9-132">In this example, the destination IP address is to a VM in another virtual network.</span></span> <span data-ttu-id="35cd9-133">Nincs a virtuális hálózati átjáró a két virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="35cd9-133">There is a virtual network gateway between the two virtual networks.</span></span>

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a><span data-ttu-id="35cd9-134">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="35cd9-134">Review results</span></span>

<span data-ttu-id="35cd9-135">Amikor végzett, a eredményei.</span><span class="sxs-lookup"><span data-stu-id="35cd9-135">When complete, the results are provided.</span></span> <span data-ttu-id="35cd9-136">Továbbá az erőforrás típusa a következő ugrás IP-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="35cd9-136">The next hop IP address is returned as well as the type of resource it is.</span></span> <span data-ttu-id="35cd9-137">Az ebben az esetben a virtuális hálózati átjáró nyilvános IP-címét is.</span><span class="sxs-lookup"><span data-stu-id="35cd9-137">In this scenario, it is the public IP address of the virtual network gateway.</span></span>

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

<span data-ttu-id="35cd9-138">Az alábbi listában a jelenleg rendelkezésre álló NextHopType értékeket mutatja:</span><span class="sxs-lookup"><span data-stu-id="35cd9-138">The following list shows the currently available NextHopType values:</span></span>

<span data-ttu-id="35cd9-139">**Következő ugrás típusa**</span><span class="sxs-lookup"><span data-stu-id="35cd9-139">**Next Hop Type**</span></span>

* <span data-ttu-id="35cd9-140">Internet</span><span class="sxs-lookup"><span data-stu-id="35cd9-140">Internet</span></span>
* <span data-ttu-id="35cd9-141">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="35cd9-141">VirtualAppliance</span></span>
* <span data-ttu-id="35cd9-142">Pedig</span><span class="sxs-lookup"><span data-stu-id="35cd9-142">VirtualNetworkGateway</span></span>
* <span data-ttu-id="35cd9-143">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="35cd9-143">VnetLocal</span></span>
* <span data-ttu-id="35cd9-144">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="35cd9-144">HyperNetGateway</span></span>
* <span data-ttu-id="35cd9-145">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="35cd9-145">VnetPeering</span></span>
* <span data-ttu-id="35cd9-146">None</span><span class="sxs-lookup"><span data-stu-id="35cd9-146">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="35cd9-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35cd9-147">Next steps</span></span>

<span data-ttu-id="35cd9-148">Megtudhatja, hogyan ellátogatva programozott módon tekintse át a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="35cd9-148">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

















