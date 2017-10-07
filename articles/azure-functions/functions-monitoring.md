---
title: az Azure Functions aaaMonitoring |} Microsoft Docs
description: Megtudhatja, hogyan toomonitor az Azure Functions.
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
ms.openlocfilehash: 254348d1cefce925654bd9532715b6def571e0ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-azure-functions"></a>Az Azure Functions figyelése

## <a name="overview"></a>Áttekintés 


Hello **figyelő** minden funkció lehetővé teszi tooreview lapján minden egyes egy függvény végrehajtása.

![Az Azure Functions figyelés lapján](./media/functions-monitoring/monitor-tab.png) 

Kattintson egy végrehajtását lehetővé teszi, hogy Ön tooreview hello időtartama, a bemeneti adatok, a hibák és a kapcsolódó naplófájlok. Ez egy hasznos Hibakeresés és teljesítményének hangolása függvényeit.


> [!IMPORTANT]
> Hello használatakor [üzemeltetési terv fogyasztás](functions-overview.md#pricing) Azure Functions hello **figyelés** hello függvény App áttekintése panelen csempe nem jelenik meg az adatokat. Ennek az az oka hello platform dinamikusan méretezi és számítási példányokért kezeli, úgy, hogy ezeket a mérési nem értelmezhető fogyasztás tervezze. Függvény alkalmazásai toomonitor hello használata, inkább használjon hello útmutatást ebben a cikkben.
> 
> a következő képernyőfelvétel hello példáját mutatja be:
> 
> ![Hello fő erőforráspanelen figyelése](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a>Valós idejű figyelése

Valós idejű figyelés kattintva érhető **élő esemény adatfolyam** alább látható módon. 

![Az élő esemény adatfolyam beállítást hello figyelő lap](./media/functions-monitoring/monitor-tab-live-event-stream.png)

élő hello eseményfelhasználó fog graphed be egy új böngészőlapon, alább látható módon. 

![Az élő esemény adatfolyam – példa](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> Nincs egy ismert probléma, amelyek az adatok toofail toobe feltöltve okozhatnak. Ha ez tapasztal, szükség lehet a tooclose hello böngésző lapon tartalmazó hello élő esemény adatfolyam, és kattintson **élő esemény adatfolyam** újra tooallow azt tooproperly az esemény-adatfolyam adatok feltöltése. 

élő hello eseményfelhasználó fog diagramot hello statisztikáit. a függvény a következő:

* A másodpercenként elindított végrehajtások
* Végrehajtások másodpercenként befejezett
* Végrehajtások másodpercenként sikertelen
* Átlagos végrehajtási idő, ezredmásodpercben megadva.

Ezeket a statisztikákat a valós idejű, de a tényleges hello végrehajtási adatok grafikonozás hello esetleg késés körülbelül 10 másodperc.






## <a name="monitoring-log-files-from-a-command-line"></a>A parancssorból naplófájlok figyelése


Napló fájlok tooa parancssori munkamenetet egy helyi munkaállomáson hello Azure parancssori felület (CLI) vagy a PowerShell használatával is adatfolyam.

### <a name="monitoring-function-app-log-files-with-hello-azure-cli"></a>Figyelési funkció alkalmazások naplófájljainak a hello Azure parancssori felület

elindult, tooget [hello Azure parancssori felület telepítése](../cli-install-nodejs.md)

Jelentkezzen be az Azure-fiókjával hello alábbi parancsot, vagy bármely más beállításokat tárgyalja, hello [jelentkezzen be az Azure CLI hello tooAzure](../xplat-cli-connect.md).

    azure login

Használjon hello következő parancsot a tooenable Azure CLI szolgáltatás kezelési (ASM) mód:.

    azure config mode asm

Ha több előfizetéssel rendelkezik, használja a következő parancsok toolist hello az előfizetések és -készlet hello aktuális előfizetés toohello előfizetést, amely tartalmazza az függvény alkalmazás.

    azure account list
    azure account set <subscriptionNameOrId>

hello következő parancs fog adatfolyam a függvény app toohello parancssor hello naplófájlokat:

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a>Figyelési funkció alkalmazások naplófájljainak a PowerShell használatával

elindult, tooget [Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).

Adja hozzá az Azure-fiókjával hello a következő parancs futtatásával:

    PS C:\> Add-AzureAccount

Ha több előfizetéssel rendelkezik, listázhatja azokat név szerint a következő parancs toosee, ha a megfelelő előfizetés kijelölve hello hello alapján hello `IsCurrent` tulajdonság:

    PS C:\> Get-AzureSubscription

Ha tooset hello aktív előfizetéssel toohello egyet a függvény alkalmazást tartalmazó van szüksége, használja a következő parancs hello:

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

Az adatfolyam hello naplók tooyour PowerShell-munkamenetet a következő parancs hello:

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

További információt talál a túl[hogyan: adatfolyam-naplókat web Apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello:

* [A függvény tesztelése](functions-test-a-function.md)
* [Egy függvény méretezése](functions-scale.md)

