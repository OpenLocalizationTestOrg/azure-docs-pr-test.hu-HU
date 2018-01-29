---
title: "Ellenőrizze a forgalmat az Azure hálózati figyelő IP Flow ellenőrizze - Azure CLI |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan ellenőrizhető, ha a bejövő és kimenő forgalmat a virtuális gépek engedélyezett vagy megtagadott Azure parancssori felület használatával"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 3f4a7d3f96a08b3296dd1abfec8abfbcb9759e9f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/21/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Ha a forgalom engedélyezett vagy megtagadott vagy a virtuális gép IP Flow ellenőrizze és Azure hálózati figyelőt összetevője ellenőrzése

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Az Azure REST API-n](network-watcher-check-ip-flow-verify-rest.md)


IP-adatfolyam ellenőrzése egy funkciója, amely lehetővé teszi, hogy ellenőrizze, hogy ha a forgalom engedélyezve van-e, vagy a virtuális gép hálózati figyelőt. Ebben a forgatókönyvben akkor hasznos, ha szeretné, hogy a virtuális gép kommunikálhat külső erőforrás- vagy háttéradatbázis aktuális állapotának. IP-adatfolyam győződjön meg arról is használható, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és hibáinak elhárítása az NSG-szabályok által blokkolt adatfolyamok ellenőrzése. Egy másik oka IP folyamat, ellenőrizze annak biztosítása, amelyet a letiltott forgalmat blokkol megfelelően az NSG.

Ez a cikk használja a következő generációs CLI a erőforrás management üzembe helyezési modellel, Azure CLI 2.0, elérhető a Windows, Mac és Linux.

Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) hozzon létre egy hálózati figyelőt, vagy egy meglévő példánya hálózati figyelőt. A forgatókönyv feltételezi, hogy létezik-e egy erőforráscsoportot, egy érvényes virtuális géppel használandó.

## <a name="scenario"></a>Forgatókönyv

Ez a forgatókönyv használ IP Flow ellenőrizze ellenőrizheti, ha egy virtuális gép működik egy ismert Bing IP-címre. Ha a forgalmat a rendszer megtagadja, a biztonsági szabály, amely megtagadja a forgalom adja vissza. IP Flow ellenőrizze kapcsolatos további információkért látogasson el a [IP-adatfolyam ellenőrizze áttekintése](network-watcher-ip-flow-verify-overview.md)

## <a name="get-a-vm"></a>A virtuális gép beolvasása

IP-adatfolyam tesztek bejövő és kimenő forgalmat a virtuális gép IP-címet vagy egy távoli céljához ellenőrzése. A virtuális gép azonosítóját a parancsmag szükség. Ha már ismeri az Azonosítót a virtuális gép használja, kihagyhatja ezt a lépést.

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-the-nics"></a>A hálózati adapterek beolvasása

A virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk a hálózati adaptert egy virtuális gépen. Ha már ismeri a virtuális gépen vizsgálni kívánt IP-cím, kihagyhatja ezt a lépést.

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a>Futtatási IP-adatfolyam ellenőrzése

Most, hogy a fordítás során futtassa a parancsmagot, és futtassa azt a `az network watcher test-ip-flow` parancsmag segítségével tesztelheti a forgalmat. A jelen példában használjuk az első IP-cím első hálózati adapteren

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> IP-adatfolyam győződjön meg arról, hogy a virtuális gép erőforrásához lefoglalt futtatásához szükséges.

## <a name="review-results"></a>Tekintse át az eredményeket

Futtatása után `az network watcher test-ip-flow` eredményeinek, az alábbi példa az előző lépésben adott eredmények.

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a>További lépések

Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) nyomon követheti a hálózati biztonsági csoport és a biztonsági meghatározott szabályokat.

Ismerje meg, látogasson el a NSG beállítások naplózandó [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
