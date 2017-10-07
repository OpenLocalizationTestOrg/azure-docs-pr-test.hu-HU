---
title: "következő ugrás aaaFind rendelkező Azure hálózati figyelő következő ugrás - Azure-portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan található milyen hello a következő ugrás típusa, és IP-cím használatával a következő ugrás használatával hello Azure-portálon"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b64a2a5275c15aa8bdb10601de4ae1504a9ab551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-hello-portal"></a>Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használ az Azure hálózati figyelőt hello portál használatával

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Az Azure REST API-n](network-watcher-check-next-hop-rest.md)

Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet. A szolgáltatás akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt. hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.

## <a name="scenario"></a>Forgatókönyv

a cikkben szereplő hello forgatókönyv használja a következő ugrás toofind hello következő ugrás típusa és IP-cím erőforrás. toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).

Ebben a forgatókönyvben a tartalma:

* Következő ugrás hello lekérése egy virtuális gépet.

## <a name="get-next-hop"></a>Következő ugrás beolvasása

### <a name="step-1"></a>1. lépés

Keresse meg a hálózati figyelőt erőforrás tooyour hello Azure-portálon.

### <a name="step-2"></a>2. lépés

Kattintson a **a következő Ugrás** hello navigációs ablaktábla, válassza hello virtuálisgép- és hálózati adapter, töltse ki a forrás és cél IP hello, majd kattintson a hello **a következő Ugrás** gombra.

> [!NOTE]
> Következő ugrás megköveteli, hogy hello VM erőforrás toorun van lefoglalva.

![következő ugrás áttekintése beolvasása][1]

### <a name="step-3"></a>3. lépés

Ha hello feladat befejeződött, hello eredményei. hello IP-cím és az eszköz hello következő ugrás típusa, jelenik meg. hello következő táblázatban hello elérhető visszaadott értékek hello portálon.

**Következő ugrás típusa**

* Internet
* VirtualAppliance
* Pedig
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

Ha egy egyéni útvonal lett használt tooroute a forgalmat, hello felhasználó által megadott útvonal (UDR) jelenik meg, valamint hello eredményekkel.

![következő ugrás eredményt ad][2]

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooreview programozott módon látogasson el a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














