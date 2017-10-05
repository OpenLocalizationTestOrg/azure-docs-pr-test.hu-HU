---
title: "Az Azure Functions figyelése |} Microsoft Docs"
description: "Útmutató az Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2016
ms.author: wesmc
ms.openlocfilehash: b70214593b1417265387f42306a633bb0df2920e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-azure-functions"></a>Az Azure Functions figyelése

## <a name="overview"></a>Áttekintés 


A **figyelő** lapján minden funkció lehetővé teszi, hogy tekintse át a függvény minden egyes végrehajtása.

![Az Azure Functions figyelés lapján](./media/functions-monitoring/monitor-tab.png) 

Kattintson egy végrehajtását lehetővé teszi, hogy tekintse át az időtartam, a bemeneti adatok, a hibák és a kapcsolódó naplófájlok. Ez egy hasznos Hibakeresés és teljesítményének hangolása függvényeit.


> [!IMPORTANT]
> Használatakor a [üzemeltetési terv fogyasztás](functions-overview.md#pricing) az Azure Functions a **figyelés** az függvény áttekintése panelen a csempe nem jelenik meg az adatokat. Ennek az az oka a platform dinamikusan méretezi és számítási példányokért kezeli, úgy, hogy ezeket a mérési nem értelmezhető fogyasztás tervezze. A függvény alkalmazások megfigyeléséhez, Ehelyett használjon útmutatás ebben a cikkben.
> 
> Az alábbi képernyőfelvételen egy példát mutat be:
> 
> ![A fő erőforráspanelen figyelése](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a>Valós idejű figyelése

Valós idejű figyelés kattintva érhető **élő esemény adatfolyam** alább látható módon. 

![A figyelés lapján élő esemény adatfolyam beállítása](./media/functions-monitoring/monitor-tab-live-event-stream.png)

Az élő esemény adatfolyam fog graphed be egy új böngészőlapon, alább látható módon. 

![Az élő esemény adatfolyam – példa](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> Nincs egy ismert probléma, amelyek az adatok nem tölthetők fel okozhatnak. Ez tapasztal, esetleg zárja be az élő esemény folyamot tartalmazó böngészőlapon, és kattintson a **élő esemény adatfolyam** újra, hogy engedélyezi az esemény-adatfolyam adatok megfelelően feltöltéséhez. 

Az élő esemény adatfolyam fog diagramot a függvény a következő adatokat:

* A másodpercenként elindított végrehajtások
* Végrehajtások másodpercenként befejezett
* Végrehajtások másodpercenként sikertelen
* Átlagos végrehajtási idő, ezredmásodpercben megadva.

Ezeket a statisztikákat a valós idejű, de a tényleges megjelenítés a végrehajtási adatok esetleg késés körülbelül 10 másodperc.






## <a name="monitoring-log-files-from-a-command-line"></a>A parancssorból naplófájlok figyelése


Naplófájlokat, hogy egy parancssori munkamenetet egy helyi munkaállomáson az Azure parancssori felület (CLI) vagy a PowerShell használatával is adatfolyam.

### <a name="monitoring-function-app-log-files-with-the-azure-cli"></a>Figyelési funkció alkalmazások naplófájljainak az Azure parancssori felülettel

A kezdéshez [az Azure parancssori felület telepítése](../cli-install-nodejs.md)

Jelentkezzen be az Azure-fiókjával az alábbi parancsot, vagy az egyéb beállításokat tárgyalja, [jelentkezzen be az Azure-bA az Azure parancssori felületen](../xplat-cli-connect.md).

    azure login

Azure CLI szolgáltatás kezelési (ASM) mód engedélyezéséhez a következő paranccsal:.

    azure config mode asm

Ha több előfizetéssel rendelkezik, az alábbi parancsokkal az előfizetés, a függvény alkalmazást tartalmazó listában az előfizetések és az aktuális előfizetésben.

    azure account list
    azure account set <subscriptionNameOrId>

A következő parancsot fog adatfolyam a naplófájlok, a függvény alkalmazás, a parancssorba:

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a>Figyelési funkció alkalmazások naplófájljainak a PowerShell használatával

A kezdéshez [Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).

A következő parancs futtatásával adja hozzá az Azure-fiókjával:

    PS C:\> Add-AzureAccount

Ha több előfizetéssel rendelkezik, listázhatja azokat a következő paranccsal, hogy a megfelelő előfizetés a kijelölt alapján nevű `IsCurrent` tulajdonság:

    PS C:\> Get-AzureSubscription

Ha az aktív előfizetéssel beállítása a egy, a függvény alkalmazást tartalmazó van szüksége, használja a következő parancsot:

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

Adatfolyamként küldje el a naplókat a PowerShell-munkamenetet a következő parancsot:

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

További információt talál [hogyan: adatfolyam-naplókat web Apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Következő lépések
További információkért lásd a következőket:

* [A függvény tesztelése](functions-test-a-function.md)
* [Egy függvény méretezése](functions-scale.md)

