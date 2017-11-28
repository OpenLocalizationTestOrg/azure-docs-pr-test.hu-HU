---
title: "Azure hálózati figyelőt topológia – az Azure parancssori felület megtekintése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan Azure CLI használata a hálózati topológia lekérdezni."
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
ms.openlocfilehash: 5be8e103f9a1f32117a4ed3be73bff021db1186d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-azure-cli"></a><span data-ttu-id="a57ad-103">Az Azure parancssori felület hálózati figyelőt topológia megtekintése</span><span class="sxs-lookup"><span data-stu-id="a57ad-103">View Network Watcher topology with Azure CLI</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a57ad-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a57ad-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="a57ad-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a57ad-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="a57ad-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a57ad-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="a57ad-107">REST API</span><span class="sxs-lookup"><span data-stu-id="a57ad-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="a57ad-108">A hálózati figyelőt topológia szolgáltatása a hálózati erőforrások, az előfizetés vizuális ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a57ad-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="a57ad-109">A portál Ez a képi megjelenítés áll rendelkezésre, automatikusan.</span><span class="sxs-lookup"><span data-stu-id="a57ad-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="a57ad-110">Az adatokat a topológia e nézetében a portálon mögött PowerShell kérhető.</span><span class="sxs-lookup"><span data-stu-id="a57ad-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="a57ad-111">Ez a funkció lehetővé teszi a topológiainfomációja rugalmasabb, az adatok a képi megjelenítés felé más eszközökkel feldolgozottként.</span><span class="sxs-lookup"><span data-stu-id="a57ad-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="a57ad-112">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="a57ad-112">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="a57ad-113">Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.</span><span class="sxs-lookup"><span data-stu-id="a57ad-113">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

<span data-ttu-id="a57ad-114">Az összekapcsolási két kapcsolatok alapján van modellezve.</span><span class="sxs-lookup"><span data-stu-id="a57ad-114">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="a57ad-115">**Befoglaltsági** -példa: virtuális hálózat tartalmaz egy alhálózat egy hálózati Adaptert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="a57ad-115">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="a57ad-116">**Kapcsolódó** -példa: a hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="a57ad-116">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="a57ad-117">Az alábbi lista a topológia REST API lekérdezésekor visszaadott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a57ad-117">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="a57ad-118">**név** – az erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="a57ad-118">**name** - The name of the resource</span></span>
* <span data-ttu-id="a57ad-119">**azonosító** -erőforrás uri.</span><span class="sxs-lookup"><span data-stu-id="a57ad-119">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="a57ad-120">**hely** – a hely, ahol az erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="a57ad-120">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="a57ad-121">**társítások** -társítást a hivatkozott objektum listáját.</span><span class="sxs-lookup"><span data-stu-id="a57ad-121">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="a57ad-122">**név** -a hivatkozott erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="a57ad-122">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="a57ad-123">**resourceId** – az erőforrás-azonosítója a található a hivatkozott erőforrás uri.</span><span class="sxs-lookup"><span data-stu-id="a57ad-123">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="a57ad-124">**associationType** -ezt az értéket a gyermekobjektum és a szülő közötti kapcsolat hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a57ad-124">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="a57ad-125">Érvényes értékek a következők **Contains** vagy **társított**.</span><span class="sxs-lookup"><span data-stu-id="a57ad-125">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a57ad-126">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a57ad-126">Before you begin</span></span>

<span data-ttu-id="a57ad-127">Ebben az esetben használhatja a `network watcher topology` parancsmag használatával beolvassa a topológia információkat.</span><span class="sxs-lookup"><span data-stu-id="a57ad-127">In this scenario, you use the `network watcher topology` cmdlet to retrieve the topology information.</span></span> <span data-ttu-id="a57ad-128">Van még egy cikk való [beolvasni a REST API-t a hálózati topológia](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="a57ad-128">There is also an article on how to [retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="a57ad-129">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="a57ad-129">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="a57ad-130">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="a57ad-130">Scenario</span></span>

<span data-ttu-id="a57ad-131">A forgatókönyv a cikkben szereplő lekéri a topológia választ adott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="a57ad-131">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="retrieve-topology"></a><span data-ttu-id="a57ad-132">Topológia beolvasása</span><span class="sxs-lookup"><span data-stu-id="a57ad-132">Retrieve topology</span></span>

<span data-ttu-id="a57ad-133">A `network watcher topology` parancsmag beolvassa a megadott erőforráscsoport-topológiát.</span><span class="sxs-lookup"><span data-stu-id="a57ad-133">The `network watcher topology` cmdlet retrieves the topology for a given resource group.</span></span> <span data-ttu-id="a57ad-134">Adja hozzá a következő argumentum "--json" megtekintéséhez a oput json formátumban</span><span class="sxs-lookup"><span data-stu-id="a57ad-134">Add the argument "--json" to view the oput in json format</span></span>

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a><span data-ttu-id="a57ad-135">Results (Eredmények)</span><span class="sxs-lookup"><span data-stu-id="a57ad-135">Results</span></span>

<span data-ttu-id="a57ad-136">A visszaadott eredmények rendelkezik tulajdonsággal "Resources" nevét, amely tartalmazza a json válasz törzse a `network watcher topology` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a57ad-136">The results returned have a property name "Resources", which contains the json response body for the `network watcher topology` cmdlet.</span></span>  <span data-ttu-id="a57ad-137">A válasz tartalmazza a hálózati biztonsági csoport és a társításokat (Ez azt jelenti, hogy a tartalmazza, a társított) erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="a57ad-137">The response contains the resources in the Network Security Group and their associations (that is, Contains, Associated).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a57ad-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a57ad-138">Next steps</span></span>

<span data-ttu-id="a57ad-139">További információ a biztonsági szabályok látogasson el a hálózati erőforrások alkalmazott [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a57ad-139">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
