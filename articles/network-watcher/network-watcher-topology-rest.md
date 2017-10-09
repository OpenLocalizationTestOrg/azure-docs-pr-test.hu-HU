---
title: "aaaView Azure hálózati figyelőt topológia - REST API |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse REST API tooquery a hálózati topológia."
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
ms.openlocfilehash: 39292025bcd561f007c9e31271b1389be48ea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a><span data-ttu-id="49d47-103">REST API-t a hálózati figyelőt topológia megtekintése</span><span class="sxs-lookup"><span data-stu-id="49d47-103">View Network Watcher topology with REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="49d47-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="49d47-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="49d47-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="49d47-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="49d47-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="49d47-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="49d47-107">REST API</span><span class="sxs-lookup"><span data-stu-id="49d47-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="49d47-108">hello topológia szolgáltatása hálózati figyelőt hello hálózati erőforrások egy előfizetésben vizuális ábrázolását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="49d47-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="49d47-109">Hello portal Ez a képi megjelenítés áll rendelkezésre tooyou automatikusan.</span><span class="sxs-lookup"><span data-stu-id="49d47-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="49d47-110">hello információk mögött hello topológia e nézetében hello portálon PowerShell kérhető.</span><span class="sxs-lookup"><span data-stu-id="49d47-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="49d47-111">E képesség révén hello topológiainfomációja rugalmasabb hello adatok más eszközök toobuild kimenő hello képi megjelenítés által feldolgozottként.</span><span class="sxs-lookup"><span data-stu-id="49d47-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="49d47-112">hello összekapcsolása a két van modellezve.</span><span class="sxs-lookup"><span data-stu-id="49d47-112">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="49d47-113">**Befoglaltsági** -példa: virtuális hálózat tartalmaz egy alhálózat egy hálózati Adaptert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="49d47-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="49d47-114">**Kapcsolódó** -példa: a hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="49d47-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="49d47-115">hello következő lista rendszer hello topológia REST API lekérdezésekor visszaadott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="49d47-115">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="49d47-116">**név** – hello hello erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="49d47-116">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="49d47-117">**azonosító** -hello hello erőforrás URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="49d47-117">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="49d47-118">**hely** -hello helyre, ahol hello erőforrás létezik-e.</span><span class="sxs-lookup"><span data-stu-id="49d47-118">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="49d47-119">**társítások** -társítások toohello listája hivatkozott objektum.</span><span class="sxs-lookup"><span data-stu-id="49d47-119">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="49d47-120">**név** -hello hello neve hivatkozott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49d47-120">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="49d47-121">**resourceId** -hello resourceId hello hello társítás hivatkozott hello erőforrás URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="49d47-121">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="49d47-122">**associationType** -Ez az érték hivatkozik hello gyermekobjektum és hello szülő hello kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="49d47-122">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="49d47-123">Érvényes értékek a következők **Contains** vagy **társított**.</span><span class="sxs-lookup"><span data-stu-id="49d47-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="49d47-124">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="49d47-124">Before you begin</span></span>

<span data-ttu-id="49d47-125">Ebben a forgatókönyvben hello topológia adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="49d47-125">In this scenario, you retrieve hello topology information.</span></span> <span data-ttu-id="49d47-126">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49d47-126">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="49d47-127">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="49d47-127">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="49d47-128">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="49d47-128">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="49d47-129">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="49d47-129">Scenario</span></span>

<span data-ttu-id="49d47-130">a cikkben szereplő hello forgatókönyv hello topológia választ adott erőforráscsoport kéri le.</span><span class="sxs-lookup"><span data-stu-id="49d47-130">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="49d47-131">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="49d47-131">Log in with ARMClient</span></span>

<span data-ttu-id="49d47-132">Jelentkezzen be a Azure hitelesítő adataival tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="49d47-132">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a><span data-ttu-id="49d47-133">Topológia beolvasása</span><span class="sxs-lookup"><span data-stu-id="49d47-133">Retrieve topology</span></span>

<span data-ttu-id="49d47-134">a következő példa hello hello topológia kér hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="49d47-134">hello following example requests hello topology from hello REST API.</span></span>  <span data-ttu-id="49d47-135">hello példája paraméteres tooallow létrehozása, például a rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="49d47-135">hello example is parameterized tooallow for flexibility in creating an example.</span></span>  <span data-ttu-id="49d47-136">Cserélje le az összes érték \< \> körülvevő őket.</span><span class="sxs-lookup"><span data-stu-id="49d47-136">Replace all values with \< \> surrounding them.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name toorun topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

<span data-ttu-id="49d47-137">válasz a következő hello, a rövidített válasz eredményül, ha például egy erőforráscsoport-topológiát beolvasása:</span><span class="sxs-lookup"><span data-stu-id="49d47-137">hello following response is an example of a shortened response that is returned when retrieve topology for a resourcegroup:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="49d47-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49d47-138">Next steps</span></span>

<span data-ttu-id="49d47-139">Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="49d47-139">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

