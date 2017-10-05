---
title: "Azure hálózati figyelőt topológia - PowerShell megtekintése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan lehet a hálózati topológia lekérdezése a PowerShell használatával."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: bd0e882d-8011-45e8-a7ce-de231a69fb85
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 40e01a7a6a2ea6127ab725f04649cec47b9d9422
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a><span data-ttu-id="863f4-103">A PowerShell-lel hálózati figyelőt topológia megtekintése</span><span class="sxs-lookup"><span data-stu-id="863f4-103">View Network Watcher topology with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="863f4-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="863f4-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="863f4-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="863f4-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="863f4-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="863f4-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="863f4-107">REST API</span><span class="sxs-lookup"><span data-stu-id="863f4-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="863f4-108">A hálózati figyelőt topológia szolgáltatása a hálózati erőforrások, az előfizetés vizuális ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="863f4-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="863f4-109">A portál Ez a képi megjelenítés áll rendelkezésre, automatikusan.</span><span class="sxs-lookup"><span data-stu-id="863f4-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="863f4-110">Az adatokat a topológia e nézetében a portálon mögött PowerShell kérhető.</span><span class="sxs-lookup"><span data-stu-id="863f4-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="863f4-111">Ez a funkció lehetővé teszi a topológiainfomációja rugalmasabb, az adatok a képi megjelenítés felé más eszközökkel feldolgozottként.</span><span class="sxs-lookup"><span data-stu-id="863f4-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="863f4-112">Az összekapcsolási két kapcsolatok alapján van modellezve.</span><span class="sxs-lookup"><span data-stu-id="863f4-112">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="863f4-113">**Befoglaltsági** -példa: virtuális hálózat tartalmaz egy alhálózat egy hálózati Adaptert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="863f4-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="863f4-114">**Kapcsolódó** -példa: a hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="863f4-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="863f4-115">Az alábbi lista a topológia REST API lekérdezésekor visszaadott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="863f4-115">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="863f4-116">**név** – az erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="863f4-116">**name** - The name of the resource</span></span>
* <span data-ttu-id="863f4-117">**azonosító** -erőforrás uri.</span><span class="sxs-lookup"><span data-stu-id="863f4-117">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="863f4-118">**hely** – a hely, ahol az erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="863f4-118">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="863f4-119">**társítások** -társítást a hivatkozott objektum listáját.</span><span class="sxs-lookup"><span data-stu-id="863f4-119">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="863f4-120">**név** -a hivatkozott erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="863f4-120">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="863f4-121">**resourceId** – az erőforrás-azonosítója a található a hivatkozott erőforrás uri.</span><span class="sxs-lookup"><span data-stu-id="863f4-121">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="863f4-122">**associationType** -ezt az értéket a gyermekobjektum és a szülő közötti kapcsolat hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="863f4-122">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="863f4-123">Érvényes értékek a következők **Contains** vagy **társított**.</span><span class="sxs-lookup"><span data-stu-id="863f4-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="863f4-124">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="863f4-124">Before you begin</span></span>

<span data-ttu-id="863f4-125">Ebben az esetben használhatja a `Get-AzureRmNetworkWatcherTopology` parancsmag használatával beolvassa a topológia információkat.</span><span class="sxs-lookup"><span data-stu-id="863f4-125">In this scenario, you use the `Get-AzureRmNetworkWatcherTopology` cmdlet to retrieve the topology information.</span></span> <span data-ttu-id="863f4-126">Van még egy cikk való [beolvasni a REST API-t a hálózati topológia](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="863f4-126">There is also an article on how to [retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="863f4-127">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="863f4-127">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="863f4-128">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="863f4-128">Scenario</span></span>

<span data-ttu-id="863f4-129">A forgatókönyv a cikkben szereplő lekéri a topológia választ adott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="863f4-129">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="863f4-130">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="863f4-130">Retrieve Network Watcher</span></span>

<span data-ttu-id="863f4-131">Az első lépés a hálózati figyelőt példányának lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="863f4-131">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="863f4-132">A `$networkWatcher` változó átadott a `Get-AzureRmNetworkWatcherTopology` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="863f4-132">The `$networkWatcher` variable is passed to the `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a><span data-ttu-id="863f4-133">Topológia beolvasása</span><span class="sxs-lookup"><span data-stu-id="863f4-133">Retrieve topology</span></span>

<span data-ttu-id="863f4-134">A `Get-AzureRmNetworkWatcherTopology` parancsmag beolvassa a megadott erőforráscsoport-topológiát.</span><span class="sxs-lookup"><span data-stu-id="863f4-134">The `Get-AzureRmNetworkWatcherTopology` cmdlet retrieves the topology for a given resource group.</span></span>

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a><span data-ttu-id="863f4-135">Results (Eredmények)</span><span class="sxs-lookup"><span data-stu-id="863f4-135">Results</span></span>

<span data-ttu-id="863f4-136">A visszaadott eredmények rendelkezik tulajdonsággal "Resources" nevét, amely tartalmazza a json válasz törzse a `Get-AzureRmNetworkWatcherTopology` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="863f4-136">The results returned have a property name "Resources", which contains the json response body for the `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>  <span data-ttu-id="863f4-137">A válasz tartalmazza a hálózati biztonsági csoport és a társításokat (Ez azt jelenti, hogy a tartalmazza, a társított) erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="863f4-137">The response contains the resources in the Network Security Group and their associations (that is, Contains, Associated).</span></span>

```json
Id              : 00000000-0000-0000-0000-000000000000
CreatedDateTime : 2/1/2017 7:52:21 PM
LastModified    : 2/1/2017 7:46:18 PM
Resources       : [
                    {
                      "Name": "testrg-vnet",
                      "Id":
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet/subnets/default"
                        }
                      ]
                    },
                    {
                      "Name": "default",
                      "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testr
                  g-vnet/subnets/default",
                      "Location": "westcentralus",
                      "Associations": []
                    },
                    {
                      "Name": "testrg-vnet2",
                      "Id": 
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet2",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/default"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "GatewaySubnet",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/GatewaySubnet"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "gateway2",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworkGateways/gateway2"
                        }
                      ]
                    },
                    ...
                  ]
```

## <a name="next-steps"></a><span data-ttu-id="863f4-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="863f4-138">Next steps</span></span>

<span data-ttu-id="863f4-139">Megtudhatja, hogyan jelenítheti meg az NSG folyamata naplók a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="863f4-139">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


