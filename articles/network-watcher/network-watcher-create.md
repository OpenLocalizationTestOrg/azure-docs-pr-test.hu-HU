---
title: "Hozzon létre egy Azure hálózati figyelőt példányt |} Microsoft Docs"
description: "Ezen a lapon ismerteti a hálózati figyelőt példányának létrehozása a portál és az Azure REST API használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2aeaffdd5ab552e18677cbd1a24a748dd14bf172
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-network-watcher-instance"></a><span data-ttu-id="47d2e-103">Hozzon létre egy Azure hálózati figyelőt példányt</span><span class="sxs-lookup"><span data-stu-id="47d2e-103">Create an Azure Network Watcher instance</span></span>

<span data-ttu-id="47d2e-104">Hálózati figyelőt olyan regionális szolgáltatás, amely lehetővé teszi, hogy figyelése és diagnosztizálása szintjén feltételek egy hálózati forgatókönyv, hogy, és az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="47d2e-104">Network Watcher is a regional service that enables you to monitor and diagnose conditions at a network scenario level in, to, and from Azure.</span></span> <span data-ttu-id="47d2e-105">Forgatókönyv szintű figyelését teszi lehetővé egy végpontok közötti hálózati szintű áttekintés a problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="47d2e-105">Scenario level monitoring enables you to diagnose problems at an end to end network level view.</span></span> <span data-ttu-id="47d2e-106">Hálózati diagnosztika és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és információt kaphat a hálózathoz, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="47d2e-106">Network diagnostic and visualization tools available with Network Watcher help you understand, diagnose, and gain insights to your network in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="47d2e-107">Hálózati figyelőt jelenleg csak támogatja CLI 1.0, CLI 1.0 biztosított új hálózati figyelőt példány létrehozásához nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="47d2e-107">As Network Watcher currently only supports CLI 1.0, the instructions to create a new Network Watcher instance is provided for CLI 1.0.</span></span>

## <a name="create-a-network-watcher-in-the-portal"></a><span data-ttu-id="47d2e-108">Hozzon létre egy hálózati figyelőt a portálon</span><span class="sxs-lookup"><span data-stu-id="47d2e-108">Create a Network Watcher in the portal</span></span>

<span data-ttu-id="47d2e-109">Navigáljon a **további szolgáltatások** > **hálózati** > **hálózati figyelő**.</span><span class="sxs-lookup"><span data-stu-id="47d2e-109">Navigate to **More Services** > **Networking** > **Network Watcher**.</span></span> <span data-ttu-id="47d2e-110">Kiválaszthatja, hogy a hálózati figyelőt engedélyezni szeretné az előfizetéseket.</span><span class="sxs-lookup"><span data-stu-id="47d2e-110">You can select all the subscriptions you want to enable Network Watcher for.</span></span> <span data-ttu-id="47d2e-111">Ez a művelet létrehoz egy hálózati figyelőt minden régióban elérhető.</span><span class="sxs-lookup"><span data-stu-id="47d2e-111">This action creates a Network Watcher in every region that is available.</span></span>

![Hozzon létre egy hálózati figyelőt][1]

<span data-ttu-id="47d2e-113">Ha engedélyezi a hálózati figyelőt a portál használatával, a hálózati figyelőt példányának neve automatikusan állítja be NetworkWatcher_region_name, ahol region_name felel meg az Azure-régióban, ahol a példány volt engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="47d2e-113">When you enable Network Watcher using the Portal, the name of the Network Watcher instance will automatically be set to NetworkWatcher_region_name where region_name corresponds to the Azure Region where the instance was enabled.</span></span>  <span data-ttu-id="47d2e-114">Például egy hálózati figyelőt nyugati középső Régiójában régióban engedélyezve lesznek elnevezve NetworkWatcher_westcentralus</span><span class="sxs-lookup"><span data-stu-id="47d2e-114">For example, a Network Watcher enabled in West Central US region will be named NetworkWatcher_westcentralus</span></span>

<span data-ttu-id="47d2e-115">Emellett a hálózati figyelőt példány automatikusan hozzáadja a NetworkWatcherRG nevű erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="47d2e-115">Additionally, the Network Watcher instance will automatically be added into a Resource Group called NetworkWatcherRG.</span></span>  <span data-ttu-id="47d2e-116">Ez az erőforráscsoport létrejön, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="47d2e-116">This Resource Group will be created if it does not already exist.</span></span>

<span data-ttu-id="47d2e-117">Ha egy hálózati figyelőt példány és az erőforráscsoport nevét testreszabja az el van helyezve, az alábbiakban Powershell, a REST API vagy ARMClient módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="47d2e-117">If you wish to customize the name of a Network Watcher instance and the Resource Group it's placed into, you can use Powershell, the REST API, or ARMClient methods described below.</span></span>  <span data-ttu-id="47d2e-118">Az egyes lehetőségek az erőforráscsoport már léteznie kell a hálózati figyelőt helyezzen azt.</span><span class="sxs-lookup"><span data-stu-id="47d2e-118">In each option, the Resource Group must exist before you place the Network Watcher into it.</span></span>  

## <a name="create-a-network-watcher-with-powershell"></a><span data-ttu-id="47d2e-119">A hálózati figyelő létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="47d2e-119">Create a Network Watcher with PowerShell</span></span>

<span data-ttu-id="47d2e-120">Hálózati figyelőt példányának létrehozásához futtassa az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="47d2e-120">To create an instance of Network Watcher, run the following example:</span></span>

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a><span data-ttu-id="47d2e-121">Hozzon létre egy hálózati figyelőt REST API-val</span><span class="sxs-lookup"><span data-stu-id="47d2e-121">Create a Network Watcher with the REST API</span></span>

<span data-ttu-id="47d2e-122">A PowerShell használatával REST API hívása ARMclient szolgál.</span><span class="sxs-lookup"><span data-stu-id="47d2e-122">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="47d2e-123">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="47d2e-123">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

### <a name="log-in-with-armclient"></a><span data-ttu-id="47d2e-124">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="47d2e-124">Log in with ARMClient</span></span>

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a><span data-ttu-id="47d2e-125">A hálózati figyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="47d2e-125">Create the network watcher</span></span>

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a><span data-ttu-id="47d2e-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47d2e-126">Next steps</span></span>

<span data-ttu-id="47d2e-127">Most, hogy hálózati figyelőt példányának, megismerése funkciók érhetők el:</span><span class="sxs-lookup"><span data-stu-id="47d2e-127">Now that you have an instance of Network Watcher, learn about the features available:</span></span>

* [<span data-ttu-id="47d2e-128">Topológia</span><span class="sxs-lookup"><span data-stu-id="47d2e-128">Topology</span></span>](network-watcher-topology-overview.md)
* [<span data-ttu-id="47d2e-129">Csomagrögzítéssel</span><span class="sxs-lookup"><span data-stu-id="47d2e-129">Packet capture</span></span>](network-watcher-packet-capture-overview.md)
* [<span data-ttu-id="47d2e-130">IP-forgalom ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="47d2e-130">IP flow verify</span></span>](network-watcher-ip-flow-verify-overview.md)
* [<span data-ttu-id="47d2e-131">Következő ugrás</span><span class="sxs-lookup"><span data-stu-id="47d2e-131">Next hop</span></span>](network-watcher-next-hop-overview.md)
* [<span data-ttu-id="47d2e-132">Biztonsági csoport nézet</span><span class="sxs-lookup"><span data-stu-id="47d2e-132">Security group view</span></span>](network-watcher-security-group-view-overview.md)
* [<span data-ttu-id="47d2e-133">NSG folyamata naplózás</span><span class="sxs-lookup"><span data-stu-id="47d2e-133">NSG flow logging</span></span>](network-watcher-nsg-flow-logging-overview.md)
* [<span data-ttu-id="47d2e-134">Virtuális hálózati átjáró hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="47d2e-134">Virtual Network Gateway troubleshooting</span></span>](network-watcher-troubleshoot-overview.md)

<span data-ttu-id="47d2e-135">Ha a hálózati figyelőt példánya létrejött, csomag rögzítési konfigurálhatja a cikk a következő: [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="47d2e-135">Once a Network Watcher instance has been created, package capture can be configured by following the article: [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

[1]: ./media/network-watcher-create/figure1.png











