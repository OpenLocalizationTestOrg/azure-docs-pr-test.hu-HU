---
title: "aaaManage hálózati biztonsági csoport folyamata naplók Azure hálózati figyelőt |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan naplózza az toomanage hálózati biztonsági csoport folyamata az Azure hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fb250337ab9d1a0c0d0d3569c00bc221dd102a3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-group-flow-logs-in-hello-azure-portal"></a>Hálózati biztonsági csoport folyamata naplók az Azure-portálon hello kezelése

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Hálózati biztonsági csoport folyamata feldolgozásra egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt. A folyamat naplók JSON formátumban vannak megírva, és a fontos információkat, beleértve: 

- Kimenő és bejövő forgalom szabály alapon.
- Hálózati adapterre folyamata hello hello vonatkozik.
- hello folyamata (forrás vagy a cél IP, forrás vagy a cél port, protocol) 5 rekordos információt.
- Információ arról, hogy forgalom engedélyezett vagy megtagadott.

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt példányt](network-watcher-create.md). hello is feltételezzük, hogy egy meg van-e egy érvényes virtuális géppel erőforráscsoport.

## <a name="register-insights-provider"></a>Elemzések szolgáltató regisztrálása

A folyamat sikeres legyen, hello az naplózási toowork **Microsoft.Insights** szolgáltató regisztrálva kell lennie. tooregister hello szolgáltató, a következő lépéseket hajtsa végre a megfelelő hello: 

1. Nyissa meg túl**előfizetések**, majd válassza ki a kívánt tooenable folyamata naplók hello előfizetés. 
2. A hello **előfizetés** panelen válassza **erőforrás-szolgáltató**. 
3. Nézze meg hello szolgáltatók listáját, és győződjön meg arról, hogy hello **microsoft.insights** szolgáltató regisztrálva van. Ha nem, akkor válasszon **regisztrálása**.

![Nézet szolgáltatók][providers]

## <a name="enable-flow-logs"></a>Attribútumfolyam naplók engedélyezése

Ezen lépések hello folyamatot, amely a hálózati biztonsági csoport folyamata naplók engedélyezése.

### <a name="step-1"></a>1. lépés

Nyissa meg tooa hálózati figyelőt példányt, és válassza ki **NSG Flow naplózza**.

![Attribútumfolyam naplók – áttekintés][1]

### <a name="step-2"></a>2. lépés

Válassza ki a hálózati biztonsági csoport hello listából.

![Attribútumfolyam naplók – áttekintés][2]

### <a name="step-3"></a>3. lépés 

A hello **adatfolyam-naplói beállításainak** panelen hello állapot beállítása túl**a**, majd konfigurálja a storage-fiók.  Amikor elkészült, válassza ki a **OK**. Válassza ki **mentése**.

![Attribútumfolyam naplók – áttekintés][3]

## <a name="download-flow-logs"></a>Attribútumfolyam naplók letöltése

Attribútumfolyam naplók a storage-fiók mentése. A folyamat naplók tooview letölteni azokat.

### <a name="step-1"></a>1. lépés

toodownload folyamat naplókat, válassza ki **folyamata naplók letölthető konfigurált tárfiókok**. Ez a lépés viszi tooa tárolási fiók nézetet adja meg a mely naplók toodownload.

![Adatfolyam-naplói beállításainak][4]

### <a name="step-2"></a>2. lépés

Nyissa meg a megfelelő tárfiók toohello. Válassza ki **tárolók** > **insights-napló-networksecuritygroupflowevent**.

![Adatfolyam-naplói beállításainak][5]

### <a name="step-3"></a>3. lépés

Nyissa meg hello folyamata napló helye toohello, válassza ki azt, és válassza ki **letöltése**.

![Adatfolyam-naplói beállításainak][6]

Hello struktúra hello napló kapcsolatos információkért látogasson el a [hálózati biztonsági csoport folyamata napló áttekintése](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[jelenítheti meg az NSG folyamata naplók a powerbi-jal](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
