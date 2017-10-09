---
title: "az Azure hálózati figyelő következő ugrás - PowerShell aaaFind a következő ugrás |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan található milyen hello a következő ugrás típusa, és ip-cím használatával a következő ugrás PowerShell használatával."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fdb0b4a02d95fc45c103fe952fc1afa095414c18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-powershell"></a>Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használ az Azure hálózati figyelőt PowerShell használatával

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Az Azure REST API-n](network-watcher-check-next-hop-rest.md)

Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet. A szolgáltatás akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.

## <a name="before-you-begin"></a>Előkészületek

Ebben a forgatókönyvben hello Azure portál toofind hello következő ugrás típusa és IP-címet fogja használni.

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt. hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv használja a következő ugrás, egyik funkciója, amely megkeresi hello következő ugrás típusa és IP-cím erőforrás hálózati figyelőt. toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).

## <a name="retrieve-network-watcher"></a>Hálózati figyelőt beolvasása

hello első lépéseként tooretrieve hello hálózati figyelőt példány. Hello `$networkWatcher` változó átadása toohello következő ugrás parancsmag ellenőrzése.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a>A virtuális gép beolvasása

Következő ugrás a virtuális gép hello következő ugrás és hello hello következő ugrás IP-címét adja vissza. A virtuális gép azonosítóját hello parancsmag szükség. Ha már ismeri hello virtuális gép toouse hello azonosítója, kihagyhatja ezt a lépést.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> Következő ugrás megköveteli, hogy hello VM erőforrás toorun van lefoglalva.

## <a name="get-hello-network-interfaces"></a>Hello hálózati illesztők beolvasása

hello hello virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk hello hálózati adaptert egy virtuális gépen. Ha már ismeri a hello IP-címet, amelyet szeretne tootest hello virtuális gépen ezt a lépést kihagyhatja.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a>Következő ugrás beolvasása

Most közvetlen telepítésnek hello `Get-AzureRmNetworkWatcherNextHop` parancsmag. Azt adja át a hello parancsmag hello hálózati figyelőt, virtuális gép azonosítója, forrás IP-címet, és a cél IP-címét. Ebben a példában a hello cél IP-cím tooa virtuális gép egy másik virtuális hálózaton. Nincs a virtuális hálózati átjáró hello virtuális hálózatok között.

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a>Tekintse át az eredményeket

Amikor végzett, hello eredményei. továbbá az erőforrás típusát hello hello következő ugrás IP-címet adja vissza. Az ebben az esetben is hello virtuális hálózati átjáró hello nyilvános IP-címét.

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

hello alábbi lista mutatja azokat hello jelenleg rendelkezésre álló NextHopType értékeket:

**Következő ugrás típusa**

* Internet
* VirtualAppliance
* Pedig
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooreview programozott módon látogasson el a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)

















