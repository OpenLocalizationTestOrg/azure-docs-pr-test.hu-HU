---
title: "Azure hálózati figyelőt topológia - REST API megtekintése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan REST API használatával a hálózati topológia lekérdezése."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 568f3060da372f4a08cec342e04359172522cb69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a><span data-ttu-id="fef0d-103">REST API-t a hálózati figyelőt topológia megtekintése</span><span class="sxs-lookup"><span data-stu-id="fef0d-103">View Network Watcher topology with REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="fef0d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fef0d-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="fef0d-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fef0d-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="fef0d-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fef0d-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="fef0d-107">REST API</span><span class="sxs-lookup"><span data-stu-id="fef0d-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="fef0d-108">A hálózati figyelőt topológia szolgáltatása a hálózati erőforrások, az előfizetés vizuális ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fef0d-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="fef0d-109">A portál Ez a képi megjelenítés áll rendelkezésre, automatikusan.</span><span class="sxs-lookup"><span data-stu-id="fef0d-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="fef0d-110">Az adatokat a topológia e nézetében a portálon mögött PowerShell kérhető.</span><span class="sxs-lookup"><span data-stu-id="fef0d-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="fef0d-111">Ez a funkció lehetővé teszi a topológiainfomációja rugalmasabb, az adatok a képi megjelenítés felé más eszközökkel feldolgozottként.</span><span class="sxs-lookup"><span data-stu-id="fef0d-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="fef0d-112">Az összekapcsolási két kapcsolatok alapján van modellezve.</span><span class="sxs-lookup"><span data-stu-id="fef0d-112">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="fef0d-113">**Befoglaltsági** -példa: virtuális hálózat tartalmaz egy alhálózat egy hálózati Adaptert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="fef0d-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="fef0d-114">**Kapcsolódó** -példa: a hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="fef0d-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="fef0d-115">Az alábbi lista a topológia REST API lekérdezésekor visszaadott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="fef0d-115">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="fef0d-116">**név** – az erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="fef0d-116">**name** - The name of the resource</span></span>
* <span data-ttu-id="fef0d-117">**azonosító** -erőforrás uri.</span><span class="sxs-lookup"><span data-stu-id="fef0d-117">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="fef0d-118">**hely** – a hely, ahol az erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="fef0d-118">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="fef0d-119">**társítások** -társítást a hivatkozott objektum listáját.</span><span class="sxs-lookup"><span data-stu-id="fef0d-119">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="fef0d-120">**név** -a hivatkozott erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="fef0d-120">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="fef0d-121">**resourceId** – az erőforrás-azonosítója a található a hivatkozott erőforrás uri.</span><span class="sxs-lookup"><span data-stu-id="fef0d-121">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="fef0d-122">**associationType** -ezt az értéket a gyermekobjektum és a szülő közötti kapcsolat hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="fef0d-122">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="fef0d-123">Érvényes értékek a következők **Contains** vagy **társított**.</span><span class="sxs-lookup"><span data-stu-id="fef0d-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fef0d-124">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="fef0d-124">Before you begin</span></span>

<span data-ttu-id="fef0d-125">Ebben a forgatókönyvben a topológia adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="fef0d-125">In this scenario, you retrieve the topology information.</span></span> <span data-ttu-id="fef0d-126">A PowerShell használatával REST API hívása ARMclient szolgál.</span><span class="sxs-lookup"><span data-stu-id="fef0d-126">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="fef0d-127">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="fef0d-127">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="fef0d-128">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="fef0d-128">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="fef0d-129">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="fef0d-129">Scenario</span></span>

<span data-ttu-id="fef0d-130">A forgatókönyv a cikkben szereplő lekéri a topológia választ adott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="fef0d-130">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="fef0d-131">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="fef0d-131">Log in with ARMClient</span></span>

<span data-ttu-id="fef0d-132">Jelentkezzen be a Azure hitelesítő adataival armclient.</span><span class="sxs-lookup"><span data-stu-id="fef0d-132">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a><span data-ttu-id="fef0d-133">Topológia beolvasása</span><span class="sxs-lookup"><span data-stu-id="fef0d-133">Retrieve topology</span></span>

<span data-ttu-id="fef0d-134">A következő példa a topológia kéri le a REST API-t.</span><span class="sxs-lookup"><span data-stu-id="fef0d-134">The following example requests the topology from the REST API.</span></span>  <span data-ttu-id="fef0d-135">A példa van paraméteres rugalmasság például létrehozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="fef0d-135">The example is parameterized to allow for flexibility in creating an example.</span></span>  <span data-ttu-id="fef0d-136">Cserélje le az összes érték \< \> körülvevő őket.</span><span class="sxs-lookup"><span data-stu-id="fef0d-136">Replace all values with \< \> surrounding them.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name to run topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

<span data-ttu-id="fef0d-137">A rendszer a következő választ a rövidített válasz eredményül, ha például egy erőforráscsoport-topológiát beolvasása:</span><span class="sxs-lookup"><span data-stu-id="fef0d-137">The following response is an example of a shortened response that is returned when retrieve topology for a resourcegroup:</span></span>

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="fef0d-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fef0d-138">Next steps</span></span>

<span data-ttu-id="fef0d-139">Megtudhatja, hogyan jelenítheti meg az NSG folyamata naplók a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="fef0d-139">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

