---
title: "aaaView Azure hálózati figyelőt topológia - PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse PowerShell tooquery a hálózati topológia."
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
ms.openlocfilehash: 2bc0ecf5baa81a68be53f55c74f362a7bc97116f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a><span data-ttu-id="21945-103">A PowerShell-lel hálózati figyelőt topológia megtekintése</span><span class="sxs-lookup"><span data-stu-id="21945-103">View Network Watcher topology with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="21945-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="21945-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="21945-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="21945-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="21945-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="21945-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="21945-107">REST API</span><span class="sxs-lookup"><span data-stu-id="21945-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="21945-108">hello topológia szolgáltatása hálózati figyelőt hello hálózati erőforrások egy előfizetésben vizuális ábrázolását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="21945-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="21945-109">Hello portal Ez a képi megjelenítés áll rendelkezésre tooyou automatikusan.</span><span class="sxs-lookup"><span data-stu-id="21945-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="21945-110">hello információk mögött hello topológia e nézetében hello portálon PowerShell kérhető.</span><span class="sxs-lookup"><span data-stu-id="21945-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="21945-111">E képesség révén hello topológiainfomációja rugalmasabb hello adatok más eszközök toobuild kimenő hello képi megjelenítés által feldolgozottként.</span><span class="sxs-lookup"><span data-stu-id="21945-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="21945-112">hello összekapcsolása a két van modellezve.</span><span class="sxs-lookup"><span data-stu-id="21945-112">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="21945-113">**Befoglaltsági** -példa: virtuális hálózat tartalmaz egy alhálózat egy hálózati Adaptert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="21945-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="21945-114">**Kapcsolódó** -példa: a hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="21945-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="21945-115">hello következő lista rendszer hello topológia REST API lekérdezésekor visszaadott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="21945-115">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="21945-116">**név** – hello hello erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="21945-116">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="21945-117">**azonosító** -hello hello erőforrás URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21945-117">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="21945-118">**hely** -hello helyre, ahol hello erőforrás létezik-e.</span><span class="sxs-lookup"><span data-stu-id="21945-118">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="21945-119">**társítások** -társítások toohello listája hivatkozott objektum.</span><span class="sxs-lookup"><span data-stu-id="21945-119">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="21945-120">**név** -hello hello neve hivatkozott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="21945-120">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="21945-121">**resourceId** -hello resourceId hello hello társítás hivatkozott hello erőforrás URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="21945-121">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="21945-122">**associationType** -Ez az érték hivatkozik hello gyermekobjektum és hello szülő hello kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="21945-122">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="21945-123">Érvényes értékek a következők **Contains** vagy **társított**.</span><span class="sxs-lookup"><span data-stu-id="21945-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="21945-124">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="21945-124">Before you begin</span></span>

<span data-ttu-id="21945-125">Ebben az esetben használhatja az hello `Get-AzureRmNetworkWatcherTopology` parancsmag tooretrieve hello topológiainfomációja.</span><span class="sxs-lookup"><span data-stu-id="21945-125">In this scenario, you use hello `Get-AzureRmNetworkWatcherTopology` cmdlet tooretrieve hello topology information.</span></span> <span data-ttu-id="21945-126">Van még egy cikk hogyan túl[beolvasni a REST API-t a hálózati topológia](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="21945-126">There is also an article on how too[retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="21945-127">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="21945-127">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="21945-128">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="21945-128">Scenario</span></span>

<span data-ttu-id="21945-129">a cikkben szereplő hello forgatókönyv hello topológia választ adott erőforráscsoport kéri le.</span><span class="sxs-lookup"><span data-stu-id="21945-129">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="21945-130">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="21945-130">Retrieve Network Watcher</span></span>

<span data-ttu-id="21945-131">hello első lépéseként tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="21945-131">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="21945-132">Hello `$networkWatcher` változó átadása toohello `Get-AzureRmNetworkWatcherTopology` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="21945-132">hello `$networkWatcher` variable is passed toohello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a><span data-ttu-id="21945-133">Topológia beolvasása</span><span class="sxs-lookup"><span data-stu-id="21945-133">Retrieve topology</span></span>

<span data-ttu-id="21945-134">Hello `Get-AzureRmNetworkWatcherTopology` parancsmag beolvassa a megadott erőforráscsoport hello topológia.</span><span class="sxs-lookup"><span data-stu-id="21945-134">hello `Get-AzureRmNetworkWatcherTopology` cmdlet retrieves hello topology for a given resource group.</span></span>

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a><span data-ttu-id="21945-135">Results (Eredmények)</span><span class="sxs-lookup"><span data-stu-id="21945-135">Results</span></span>

<span data-ttu-id="21945-136">hello eredményének rendelkezik tulajdonsággal "Források" név, hello json adott válasz törzsének hello tartalmazó `Get-AzureRmNetworkWatcherTopology` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="21945-136">hello results returned have a property name "Resources", which contains hello json response body for hello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>  <span data-ttu-id="21945-137">hello válasz hello erőforrások hello hálózati biztonsági csoport és a társításokat (Ez azt jelenti, hogy a tartalmazza, a társított) tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21945-137">hello response contains hello resources in hello Network Security Group and their associations (that is, Contains, Associated).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="21945-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21945-138">Next steps</span></span>

<span data-ttu-id="21945-139">Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="21945-139">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


