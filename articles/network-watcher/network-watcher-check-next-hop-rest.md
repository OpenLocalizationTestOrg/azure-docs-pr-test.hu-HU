---
title: "Következő Ugrás az Azure hálózati figyelő következő ugrásaként - REST aaaFind |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan milyen hello a következő ugrás típusa, és IP-cím használatával a következő ugrás használatával hello Azure REST API-n található"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a2b61b355aae8ae513ebd44837184fbc6cfd668c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="d27c6-103">Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használ, amely a következőkre hálózati figyelőt Azure REST API használatával</span><span class="sxs-lookup"><span data-stu-id="d27c6-103">Find out what hello next hop type is using hello Next Hop capability in Aure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="d27c6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d27c6-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="d27c6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d27c6-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="d27c6-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d27c6-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="d27c6-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d27c6-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="d27c6-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="d27c6-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="d27c6-109">Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d27c6-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="d27c6-110">Ez a funkció akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.</span><span class="sxs-lookup"><span data-stu-id="d27c6-110">This capability is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d27c6-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d27c6-111">Before you begin</span></span>

<span data-ttu-id="d27c6-112">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d27c6-112">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="d27c6-113">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="d27c6-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="d27c6-114">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="d27c6-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="d27c6-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d27c6-115">Scenario</span></span>

<span data-ttu-id="d27c6-116">a cikkben szereplő hello forgatókönyv használja a következő ugrás, egyik funkciója, amely megkeresi hello következő ugrás típusa és IP-cím erőforrás hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="d27c6-116">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="d27c6-117">toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d27c6-117">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="d27c6-118">Ebben a forgatókönyvben a tartalma:</span><span class="sxs-lookup"><span data-stu-id="d27c6-118">In this scenario, you will:</span></span>

* <span data-ttu-id="d27c6-119">Hello a következő ugrás a virtuális gép beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d27c6-119">Retrieve hello next hop for a virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="d27c6-120">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="d27c6-120">Log in with ARMClient</span></span>

<span data-ttu-id="d27c6-121">Jelentkezzen be a Azure hitelesítő adataival tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="d27c6-121">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="d27c6-122">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="d27c6-122">Retrieve a virtual machine</span></span>

<span data-ttu-id="d27c6-123">A következő parancsfájl tooreturn hello egy virtuális gép futtatásához.</span><span class="sxs-lookup"><span data-stu-id="d27c6-123">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="d27c6-124">Ezek az információk szükségesek a következő ugrás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="d27c6-124">This information is needed for running next hop.</span></span>

<span data-ttu-id="d27c6-125">a következő kód hello értékeket a következő változók hello szüksége van:</span><span class="sxs-lookup"><span data-stu-id="d27c6-125">hello following code needs values for hello following variables:</span></span>

- <span data-ttu-id="d27c6-126">**a subscriptionId** -előfizetési azonosító toouse hello.</span><span class="sxs-lookup"><span data-stu-id="d27c6-126">**subscriptionId** - hello subscription Id toouse.</span></span>
- <span data-ttu-id="d27c6-127">**resourceGroupName** – hello virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="d27c6-127">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="d27c6-128">Hello következő kimeneti, hello virtuális gép hello azonosítója szerepel a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="d27c6-128">From hello following output, hello id of hello virtual machine is used in hello following example:</span></span>

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a><span data-ttu-id="d27c6-129">Következő ugrás beolvasása</span><span class="sxs-lookup"><span data-stu-id="d27c6-129">Get Next Hop</span></span>

<span data-ttu-id="d27c6-130">Hello authorization fejlécet létrehozása után lekérhető hello a következő ugrás a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="d27c6-130">Once hello authorization header is created, hello next hop from a virtual machine can be retrieved.</span></span> <span data-ttu-id="d27c6-131">hello következő értékek cserélni hello kód példa toowork.</span><span class="sxs-lookup"><span data-stu-id="d27c6-131">hello following values must be replaced for hello code example toowork.</span></span>

> [!Important]
> <span data-ttu-id="d27c6-132">Hálózati figyelő REST API-hívások hello hello kérelem URI hello hálózati figyelőt nem hello olyan erőforrásokat tartalmaz, a hello diagnosztikai műveleteket hajt végre hello erőforráscsoport erőforráscsoport-név.</span><span class="sxs-lookup"><span data-stu-id="d27c6-132">For Network Watcher REST API calls hello resource group name in hello request URI is hello resource group that contains hello Network Watcher, not hello resources you are performing hello diagnostic actions on.</span></span>

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> <span data-ttu-id="d27c6-133">Következő ugrás megköveteli, hogy hello VM erőforrás toorun van lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="d27c6-133">Next hop requires that hello VM resource is allocated toorun.</span></span>

## <a name="results"></a><span data-ttu-id="d27c6-134">Results (Eredmények)</span><span class="sxs-lookup"><span data-stu-id="d27c6-134">Results</span></span>

<span data-ttu-id="d27c6-135">hello következő kódrészlettel példája hello kimeneti kapott.</span><span class="sxs-lookup"><span data-stu-id="d27c6-135">hello following snippet is an example of hello output received.</span></span> <span data-ttu-id="d27c6-136">hello eredmények tartalmazzák a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="d27c6-136">hello results contain hello following values:</span></span>

* <span data-ttu-id="d27c6-137">**nextHopType** -ezt az értéket hello a következő értékek egyike: Internet, VirtualAppliance, pedig, VnetLocal, HyperNetGateway vagy None.</span><span class="sxs-lookup"><span data-stu-id="d27c6-137">**nextHopType** - This value is one of hello following values: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway, or None.</span></span>
* <span data-ttu-id="d27c6-138">**nextHopIpAddress** -hello hello következő ugrás IP-címét.</span><span class="sxs-lookup"><span data-stu-id="d27c6-138">**nextHopIpAddress** - hello IP address of hello next hop.</span></span>
* <span data-ttu-id="d27c6-139">**routeTableId** – hello értéke, vagy egy URI-jának hello útvonal társított hello útvonaltábla, vagy ha a felhasználó által definiált nem útvonal meghatározott hello érték *Rendszerútvonal* adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d27c6-139">**routeTableId** - hello value is either a uri for hello route table associated with hello route, or if no user-defined route is defined hello value of *System Route* is returned.</span></span>

<span data-ttu-id="d27c6-140">Az alábbiakban hello hello eredmények json formátumban.</span><span class="sxs-lookup"><span data-stu-id="d27c6-140">hello following are hello results in json format.</span></span>

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a><span data-ttu-id="d27c6-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d27c6-141">Next steps</span></span>

<span data-ttu-id="d27c6-142">Kimenő hello a következő ugrás a virtuális gépek képesek toofind követően megtekintheti látogasson el a hálózati erőforrások biztonsági hello [biztonsági nézettel áttekintése](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d27c6-142">Once you have been able toofind out hello next hop for a virtual machine, you can view hello security of your network resources by visiting [Security View overview](network-watcher-security-group-view-overview.md)</span></span>














