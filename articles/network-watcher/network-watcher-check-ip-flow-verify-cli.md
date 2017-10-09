---
title: "az Azure hálózati figyelő IP Flow ellenőrizze - Azure CLI aaaVerify forgalom |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott Azure parancssori felület használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 128a00b4296994551e7e17838a51e6d9de180e21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Ha a forgalom engedélyezett vagy megtagadott a tooor IP Flow ellenőrizze a virtuális gép alapján Azure hálózati figyelőt összetevője egy ellenőrzése

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Az Azure REST API-n](network-watcher-check-ip-flow-verify-rest.md)


IP-adatfolyam ellenőrzése egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt. Ebben a forgatókönyvben hasznos tooget e virtuális gép működik tooan külső erőforrás- vagy háttéradatbázis aktuális állapotának. IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása. Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.

Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt, vagy hálózati figyelőt meglévő példányát. hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.

## <a name="scenario"></a>Forgatókönyv

Ez a forgatókönyv használ tooverify IP Flow győződjön meg arról, ha egy virtuális gép működik tooa ismert Bing IP-címet. Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza. toolearn IP Flow ellenőrzéséhez kapcsolatos további információkért látogasson el [IP-adatfolyam ellenőrizze áttekintése](network-watcher-ip-flow-verify-overview.md)

## <a name="get-a-vm"></a>A virtuális gép beolvasása

IP-adatfolyam tesztek forgalom tooor tartozó IP-címet a virtuális gép tooor a távoli célhelyről a ellenőrzése. A virtuális gép azonosítóját hello parancsmag szükség. Ha már ismeri hello virtuális gép toouse hello azonosítója, kihagyhatja ezt a lépést.

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-hello-nics"></a>Hálózati adapter hello beolvasása

hello hello virtuális gépen egy hálózati adapter IP-címe szükséges, ebben a példában beolvassuk hello hálózati adaptert egy virtuális gépen. Ha már ismeri a hello IP-címet, amelyet szeretne tootest hello virtuális gépen ezt a lépést kihagyhatja.

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a>Futtatási IP-adatfolyam ellenőrzése

Most, hogy hello információ szükséges toorun hello parancsmag, hello Futtatás `az network watcher test-ip-flow` parancsmag tootest hello forgalmat. A jelen példában használjuk hello első IP-cím hello első adapteren zajlik.

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> Megköveteli, hogy hello VM erőforrás toorun lefoglalt IP-adatfolyam ellenőrzése.

## <a name="review-results"></a>Tekintse át az eredményeket

Futtatása után `az network watcher test-ip-flow` hello eredményeinek, hello alábbi példa az előző lépésben hello hello eredményének.

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a>Következő lépések

Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello hálózati biztonsági csoport és a biztonsági szabályok definiált.

Látogasson el a NSG beállítások tooaudit további [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
