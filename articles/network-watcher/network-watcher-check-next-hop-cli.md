---
title: "az Azure hálózati figyelő következő ugrás - Azure CLI 2.0 aaaFind a következő ugrás |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan található milyen hello a következő ugrás típusa, és ip-cím használatával a következő Ugrás az Azure parancssori felület használatával."
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
ms.openlocfilehash: 77c2bde51274bd5c64e7a2467f95139af620ca30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a>Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használ az Azure CLI 2.0 verziót használja Azure hálózati figyelőt

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Az Azure REST API-n](network-watcher-check-next-hop-rest.md)

Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet. A szolgáltatás akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.

Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Előkészületek

Ebben a forgatókönyvben hello Azure CLI toofind hello következő ugrás típusa és IP-címet fogja használni.

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt. hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv használja a következő ugrás, egyik funkciója, amely megkeresi hello következő ugrás típusa és IP-cím erőforrás hálózati figyelőt. toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).


## <a name="get-next-hop"></a>Következő ugrás beolvasása

tooget hello következő ugrás hello nevezzük `az network watcher show-next-hop` parancsmag. Azt adja át a hello parancsmag hello hálózati figyelőt erőforráscsoport, a hello NetworkWatcher, a virtuális gép azonosítója, a forrás IP-cím és a cél IP-cím. Ebben a példában a hello cél IP-cím tooa virtuális gép egy másik virtuális hálózaton. Nincs a virtuális hálózati átjáró hello virtuális hálózatok között.

Ha még nem még telepít, és hello konfigurálása legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login). Ezután futtassa a következő parancs hello:

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
Ha IP-továbbítás engedélyezve van a hálózati adapterek hello hello virtuális gép több hálózati adapterrel rendelkezik, majd hello NIC paramétert (-i nic-id) meg kell adni. Ellenkező esetben nem kötelező.

## <a name="review-results"></a>Tekintse át az eredményeket

Amikor végzett, hello eredményei. továbbá az erőforrás típusát hello hello következő ugrás IP-címet adja vissza.

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
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
