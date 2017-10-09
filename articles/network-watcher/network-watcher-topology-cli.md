---
title: "aaaView Azure hálózati figyelőt topológia - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse Azure CLI tooquery a hálózati topológia."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: afa7e7dd844ecb2ab4c616ba99fa0a433f1e4ade
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-azure-cli"></a><span data-ttu-id="30e67-103">Az Azure parancssori felület hálózati figyelőt topológia megtekintése</span><span class="sxs-lookup"><span data-stu-id="30e67-103">View Network Watcher topology with Azure CLI</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="30e67-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="30e67-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="30e67-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="30e67-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="30e67-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="30e67-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="30e67-107">REST API</span><span class="sxs-lookup"><span data-stu-id="30e67-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="30e67-108">hello topológia szolgáltatása hálózati figyelőt hello hálózati erőforrások egy előfizetésben vizuális ábrázolását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="30e67-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="30e67-109">Hello portal Ez a képi megjelenítés áll rendelkezésre tooyou automatikusan.</span><span class="sxs-lookup"><span data-stu-id="30e67-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="30e67-110">hello információk mögött hello topológia e nézetében hello portálon PowerShell kérhető.</span><span class="sxs-lookup"><span data-stu-id="30e67-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="30e67-111">E képesség révén hello topológiainfomációja rugalmasabb hello adatok más eszközök toobuild kimenő hello képi megjelenítés által feldolgozottként.</span><span class="sxs-lookup"><span data-stu-id="30e67-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="30e67-112">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="30e67-112">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="30e67-113">Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.</span><span class="sxs-lookup"><span data-stu-id="30e67-113">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

<span data-ttu-id="30e67-114">hello összekapcsolása a két van modellezve.</span><span class="sxs-lookup"><span data-stu-id="30e67-114">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="30e67-115">**Befoglaltsági** -példa: virtuális hálózat tartalmaz egy alhálózat egy hálózati Adaptert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="30e67-115">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="30e67-116">**Kapcsolódó** -példa: a hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="30e67-116">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="30e67-117">hello következő lista rendszer hello topológia REST API lekérdezésekor visszaadott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="30e67-117">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="30e67-118">**név** – hello hello erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="30e67-118">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="30e67-119">**azonosító** -hello hello erőforrás URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="30e67-119">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="30e67-120">**hely** -hello helyre, ahol hello erőforrás létezik-e.</span><span class="sxs-lookup"><span data-stu-id="30e67-120">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="30e67-121">**társítások** -társítások toohello listája hivatkozott objektum.</span><span class="sxs-lookup"><span data-stu-id="30e67-121">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="30e67-122">**név** -hello hello neve hivatkozott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="30e67-122">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="30e67-123">**resourceId** -hello resourceId hello hello társítás hivatkozott hello erőforrás URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="30e67-123">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="30e67-124">**associationType** -Ez az érték hivatkozik hello gyermekobjektum és hello szülő hello kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="30e67-124">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="30e67-125">Érvényes értékek a következők **Contains** vagy **társított**.</span><span class="sxs-lookup"><span data-stu-id="30e67-125">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="30e67-126">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="30e67-126">Before you begin</span></span>

<span data-ttu-id="30e67-127">Ebben az esetben használhatja az hello `network watcher topology` parancsmag tooretrieve hello topológiainfomációja.</span><span class="sxs-lookup"><span data-stu-id="30e67-127">In this scenario, you use hello `network watcher topology` cmdlet tooretrieve hello topology information.</span></span> <span data-ttu-id="30e67-128">Van még egy cikk hogyan túl[beolvasni a REST API-t a hálózati topológia](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="30e67-128">There is also an article on how too[retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="30e67-129">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="30e67-129">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="30e67-130">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="30e67-130">Scenario</span></span>

<span data-ttu-id="30e67-131">a cikkben szereplő hello forgatókönyv hello topológia választ adott erőforráscsoport kéri le.</span><span class="sxs-lookup"><span data-stu-id="30e67-131">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="retrieve-topology"></a><span data-ttu-id="30e67-132">Topológia beolvasása</span><span class="sxs-lookup"><span data-stu-id="30e67-132">Retrieve topology</span></span>

<span data-ttu-id="30e67-133">Hello `network watcher topology` parancsmag beolvassa a megadott erőforráscsoport hello topológia.</span><span class="sxs-lookup"><span data-stu-id="30e67-133">hello `network watcher topology` cmdlet retrieves hello topology for a given resource group.</span></span> <span data-ttu-id="30e67-134">Adja hozzá a hello argumentum "--json" tooview hello oput json formátumban</span><span class="sxs-lookup"><span data-stu-id="30e67-134">Add hello argument "--json" tooview hello oput in json format</span></span>

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a><span data-ttu-id="30e67-135">Results (Eredmények)</span><span class="sxs-lookup"><span data-stu-id="30e67-135">Results</span></span>

<span data-ttu-id="30e67-136">hello eredményének rendelkezik tulajdonsággal "Források" név, hello json adott válasz törzsének hello tartalmazó `network watcher topology` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="30e67-136">hello results returned have a property name "Resources", which contains hello json response body for hello `network watcher topology` cmdlet.</span></span>  <span data-ttu-id="30e67-137">hello válasz hello erőforrások hello hálózati biztonsági csoport és a társításokat (Ez azt jelenti, hogy a tartalmazza, a társított) tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="30e67-137">hello response contains hello resources in hello Network Security Group and their associations (that is, Contains, Associated).</span></span>

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="30e67-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30e67-138">Next steps</span></span>

<span data-ttu-id="30e67-139">További tudnivalók hello biztonsági szabályokat alkalmazott tooyour hálózati erőforrások ellátogatva [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="30e67-139">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
