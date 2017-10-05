---
title: "Azure hálózati figyelő következő ugrás - Azure CLI 1.0 a következő ugrás található |} Microsoft Docs"
description: "Ez a cikk leírja, hogyan megtalálhatja a következő ugrás típusa van, és használó Azure parancssori felület használatával, a következő ugrás IP-cím."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: ff88e945060ae033717ceb29db1352e112f05a3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a>Megtudhatja, milyen a következő ugrás típusa a következő ugrás funkció használata az Azure CLI 1.0 Azure hálózati figyelőt használ

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Az Azure REST API-n](network-watcher-check-next-hop-rest.md)

Következő ugrás csak a hálózati figyelő, amely a képességét get biztosít, a következő ugrás típusa és az IP-cím a megadott virtuális gép alapján. Ez a szolgáltatás akkor hasznos, meghatározni, hogy ha egy virtuális gép elhagyó forgalomra halad át egy átjárót, az interneten vagy a virtuális hálózatok a cél eléréséhez.

Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.

## <a name="before-you-begin"></a>Előkészületek

Ebben a forgatókönyvben az Azure parancssori felület használandó keresse meg a következő ugrás típusa és az IP-címet.

Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt. A forgatókönyv feltételezi, hogy létezik-e egy erőforráscsoportot, egy érvényes virtuális géppel használandó.

## <a name="scenario"></a>Forgatókönyv

A forgatókönyv, a cikkben szereplő használja a következő ugrás, egyik funkciója, amely a következő ugrás típusa és az IP-cím erőforrás megkeresése hálózati figyelőt. Következő ugrás kapcsolatos további információkért látogasson el a [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).


## <a name="get-next-hop"></a>Következő ugrás beolvasása

A következő ugrás nevezzük eléréséhez a `azure netowrk watcher next-hop` parancsmag. Azt adja át a parancsmag a hálózati figyelőt erőforráscsoport NetworkWatcher, virtuális gép azonosítója, IP-forráscím, és az IP-címre. Ebben a példában a cél IP-címét, hogy egy virtuális Gépet egy másik virtuális hálózaton. Nincs a virtuális hálózati átjáró a két virtuális hálózatok között. 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
Ha a virtuális Gépnek több hálózati adapter legyen, és IP-továbbítás engedélyezve van, bármely, a hálózati adapterek, akkor a hálózati adapter paramétert (-i nic-id) meg kell adni. Ellenkező esetben nem kötelező.

## <a name="review-results"></a>Tekintse át az eredményeket

Amikor végzett, a eredményei. Továbbá az erőforrás típusa a következő ugrás IP-címet adja vissza.

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
```

Az alábbi listában a jelenleg rendelkezésre álló NextHopType értékeket mutatja:

**Következő ugrás típusa**

* Internet
* VirtualAppliance
* Pedig
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan ellátogatva programozott módon tekintse át a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)
