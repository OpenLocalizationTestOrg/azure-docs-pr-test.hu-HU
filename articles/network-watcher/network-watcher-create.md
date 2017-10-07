---
title: "egy Azure hálózati figyelőt példány aaaCreate |} Microsoft Docs"
description: "Ezen a lapon hello lépéseket toocreate egy példányt biztosít a hálózati figyelőt hello portál és az Azure REST API használatával"
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
ms.openlocfilehash: 90d4f90c9709a80e4b27863e79e5b6e16de145c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-network-watcher-instance"></a><span data-ttu-id="077b1-103">Hozzon létre egy Azure hálózati figyelőt példányt</span><span class="sxs-lookup"><span data-stu-id="077b1-103">Create an Azure Network Watcher instance</span></span>

<span data-ttu-id="077b1-104">Hálózati figyelőt regionális szolgáltatás, amely lehetővé teszi, hogy Ön toomonitor és diagnosztizálásához szintjén feltételek egy hálózati forgatókönyv, hogy, és az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="077b1-104">Network Watcher is a regional service that enables you toomonitor and diagnose conditions at a network scenario level in, to, and from Azure.</span></span> <span data-ttu-id="077b1-105">Forgatókönyv szintű figyelési lehetővé teszi toodiagnose problémákat. az end tooend hálózati szint nézetében.</span><span class="sxs-lookup"><span data-stu-id="077b1-105">Scenario level monitoring enables you toodiagnose problems at an end tooend network level view.</span></span> <span data-ttu-id="077b1-106">Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="077b1-106">Network diagnostic and visualization tools available with Network Watcher help you understand, diagnose, and gain insights tooyour network in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="077b1-107">Hálózati figyelőt jelenleg csak támogatja CLI 1.0, hello utasításokat toocreate új hálózati figyelőt példány biztosított CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="077b1-107">As Network Watcher currently only supports CLI 1.0, hello instructions toocreate a new Network Watcher instance is provided for CLI 1.0.</span></span>

## <a name="create-a-network-watcher-in-hello-portal"></a><span data-ttu-id="077b1-108">Hozzon létre egy hálózati figyelőt hello portálon</span><span class="sxs-lookup"><span data-stu-id="077b1-108">Create a Network Watcher in hello portal</span></span>

<span data-ttu-id="077b1-109">Keresse meg a túl**több szolgáltatások** > **hálózati** > **hálózati figyelőt**.</span><span class="sxs-lookup"><span data-stu-id="077b1-109">Navigate too**More Services** > **Networking** > **Network Watcher**.</span></span> <span data-ttu-id="077b1-110">Kiválaszthatja a kívánt hálózati figyelőt tooenable minden hello előfizetés esetében.</span><span class="sxs-lookup"><span data-stu-id="077b1-110">You can select all hello subscriptions you want tooenable Network Watcher for.</span></span> <span data-ttu-id="077b1-111">Ez a művelet létrehoz egy hálózati figyelőt minden régióban elérhető.</span><span class="sxs-lookup"><span data-stu-id="077b1-111">This action creates a Network Watcher in every region that is available.</span></span>

![Hozzon létre egy hálózati figyelőt][1]

<span data-ttu-id="077b1-113">Ha engedélyezi a hálózati figyelőt hello portál használatával, hello hálózati figyelőt példány hello neve lesz automatikusan beállítva tooNetworkWatcher_region_name ahol region_name megfelel-e toohello Azure-régióban, ahol hello példány volt engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="077b1-113">When you enable Network Watcher using hello Portal, hello name of hello Network Watcher instance will automatically be set tooNetworkWatcher_region_name where region_name corresponds toohello Azure Region where hello instance was enabled.</span></span>  <span data-ttu-id="077b1-114">Például egy hálózati figyelőt nyugati középső Régiójában régióban engedélyezve lesznek elnevezve NetworkWatcher_westcentralus</span><span class="sxs-lookup"><span data-stu-id="077b1-114">For example, a Network Watcher enabled in West Central US region will be named NetworkWatcher_westcentralus</span></span>

<span data-ttu-id="077b1-115">Emellett hello hálózati figyelőt példány automatikusan hozzáadja a NetworkWatcherRG nevű erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="077b1-115">Additionally, hello Network Watcher instance will automatically be added into a Resource Group called NetworkWatcherRG.</span></span>  <span data-ttu-id="077b1-116">Ez az erőforráscsoport létrejön, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="077b1-116">This Resource Group will be created if it does not already exist.</span></span>

<span data-ttu-id="077b1-117">Ha egy hálózati figyelőt példány és hello toocustomize hello nevét erőforráscsoport kerül be, az alábbiakban Powershell, hello REST API vagy ARMClient módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="077b1-117">If you wish toocustomize hello name of a Network Watcher instance and hello Resource Group it's placed into, you can use Powershell, hello REST API, or ARMClient methods described below.</span></span>  <span data-ttu-id="077b1-118">Az egyes lehetőségek hello erőforráscsoportban már léteznie kell helyezi-e hálózati figyelőt hello bele.</span><span class="sxs-lookup"><span data-stu-id="077b1-118">In each option, hello Resource Group must exist before you place hello Network Watcher into it.</span></span>  

## <a name="create-a-network-watcher-with-powershell"></a><span data-ttu-id="077b1-119">A hálózati figyelő létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="077b1-119">Create a Network Watcher with PowerShell</span></span>

<span data-ttu-id="077b1-120">Hálózati figyelőt, futtassa a következő példa hello példányának toocreate:</span><span class="sxs-lookup"><span data-stu-id="077b1-120">toocreate an instance of Network Watcher, run hello following example:</span></span>

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-hello-rest-api"></a><span data-ttu-id="077b1-121">Hozzon létre egy hálózati figyelőt hello REST API-n</span><span class="sxs-lookup"><span data-stu-id="077b1-121">Create a Network Watcher with hello REST API</span></span>

<span data-ttu-id="077b1-122">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="077b1-122">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="077b1-123">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="077b1-123">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

### <a name="log-in-with-armclient"></a><span data-ttu-id="077b1-124">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="077b1-124">Log in with ARMClient</span></span>

```powerShell
armclient login
```

### <a name="create-hello-network-watcher"></a><span data-ttu-id="077b1-125">Hello hálózati figyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="077b1-125">Create hello network watcher</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="077b1-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="077b1-126">Next steps</span></span>

<span data-ttu-id="077b1-127">Most, hogy egy hálózati figyelőt példányát, a rendelkezésre álló hello szolgáltatásainak megismerése:</span><span class="sxs-lookup"><span data-stu-id="077b1-127">Now that you have an instance of Network Watcher, learn about hello features available:</span></span>

* [<span data-ttu-id="077b1-128">Topológia</span><span class="sxs-lookup"><span data-stu-id="077b1-128">Topology</span></span>](network-watcher-topology-overview.md)
* [<span data-ttu-id="077b1-129">Csomagrögzítéssel</span><span class="sxs-lookup"><span data-stu-id="077b1-129">Packet capture</span></span>](network-watcher-packet-capture-overview.md)
* [<span data-ttu-id="077b1-130">IP-forgalom ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="077b1-130">IP flow verify</span></span>](network-watcher-ip-flow-verify-overview.md)
* [<span data-ttu-id="077b1-131">Következő ugrás</span><span class="sxs-lookup"><span data-stu-id="077b1-131">Next hop</span></span>](network-watcher-next-hop-overview.md)
* [<span data-ttu-id="077b1-132">Biztonsági csoport nézet</span><span class="sxs-lookup"><span data-stu-id="077b1-132">Security group view</span></span>](network-watcher-security-group-view-overview.md)
* [<span data-ttu-id="077b1-133">NSG folyamata naplózás</span><span class="sxs-lookup"><span data-stu-id="077b1-133">NSG flow logging</span></span>](network-watcher-nsg-flow-logging-overview.md)
* [<span data-ttu-id="077b1-134">Virtuális hálózati átjáró hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="077b1-134">Virtual Network Gateway troubleshooting</span></span>](network-watcher-troubleshoot-overview.md)

<span data-ttu-id="077b1-135">A hálózati figyelőt példány létrehozása után következő hello cikk csomag rögzítési konfigurálhatja: [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="077b1-135">Once a Network Watcher instance has been created, package capture can be configured by following hello article: [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

[1]: ./media/network-watcher-create/figure1.png











