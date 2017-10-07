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
# <a name="create-an-azure-network-watcher-instance"></a>Hozzon létre egy Azure hálózati figyelőt példányt

Hálózati figyelőt regionális szolgáltatás, amely lehetővé teszi, hogy Ön toomonitor és diagnosztizálásához szintjén feltételek egy hálózati forgatókönyv, hogy, és az Azure-ból. Forgatókönyv szintű figyelési lehetővé teszi toodiagnose problémákat. az end tooend hálózati szint nézetében. Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban.

> [!NOTE]
> Hálózati figyelőt jelenleg csak támogatja CLI 1.0, hello utasításokat toocreate új hálózati figyelőt példány biztosított CLI 1.0.

## <a name="create-a-network-watcher-in-hello-portal"></a>Hozzon létre egy hálózati figyelőt hello portálon

Keresse meg a túl**több szolgáltatások** > **hálózati** > **hálózati figyelőt**. Kiválaszthatja a kívánt hálózati figyelőt tooenable minden hello előfizetés esetében. Ez a művelet létrehoz egy hálózati figyelőt minden régióban elérhető.

![Hozzon létre egy hálózati figyelőt][1]

Ha engedélyezi a hálózati figyelőt hello portál használatával, hello hálózati figyelőt példány hello neve lesz automatikusan beállítva tooNetworkWatcher_region_name ahol region_name megfelel-e toohello Azure-régióban, ahol hello példány volt engedélyezve.  Például egy hálózati figyelőt nyugati középső Régiójában régióban engedélyezve lesznek elnevezve NetworkWatcher_westcentralus

Emellett hello hálózati figyelőt példány automatikusan hozzáadja a NetworkWatcherRG nevű erőforráscsoport.  Ez az erőforráscsoport létrejön, ha még nem létezik.

Ha egy hálózati figyelőt példány és hello toocustomize hello nevét erőforráscsoport kerül be, az alábbiakban Powershell, hello REST API vagy ARMClient módszerekkel.  Az egyes lehetőségek hello erőforráscsoportban már léteznie kell helyezi-e hálózati figyelőt hello bele.  

## <a name="create-a-network-watcher-with-powershell"></a>A hálózati figyelő létrehozása a PowerShell használatával

Hálózati figyelőt, futtassa a következő példa hello példányának toocreate:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-hello-rest-api"></a>Hozzon létre egy hálózati figyelőt hello REST API-n

ARMclient használt toocall hello REST API használatával PowerShell. ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Jelentkezzen be ARMClient

```powerShell
armclient login
```

### <a name="create-hello-network-watcher"></a>Hello hálózati figyelő létrehozása

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

## <a name="next-steps"></a>Következő lépések

Most, hogy egy hálózati figyelőt példányát, a rendelkezésre álló hello szolgáltatásainak megismerése:

* [Topológia](network-watcher-topology-overview.md)
* [Csomagrögzítéssel](network-watcher-packet-capture-overview.md)
* [IP-forgalom ellenőrzése](network-watcher-ip-flow-verify-overview.md)
* [Következő ugrás](network-watcher-next-hop-overview.md)
* [Biztonsági csoport nézet](network-watcher-security-group-view-overview.md)
* [NSG folyamata naplózás](network-watcher-nsg-flow-logging-overview.md)
* [Virtuális hálózati átjáró hibaelhárítása](network-watcher-troubleshoot-overview.md)

A hálózati figyelőt példány létrehozása után következő hello cikk csomag rögzítési konfigurálhatja: [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)

[1]: ./media/network-watcher-create/figure1.png











