---
title: "aaaCreate értesítések az Azure-szolgáltatások - platformfüggetlen parancssori Felülettel |} Microsoft Docs"
description: "Eseményindító e-mailek, értesítések, a webhely URL-címek (webhookok), vagy az automation megadott hello feltételek teljesülése esetén hívható."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: e53701e5377a415038a69fbd32f1e5fc5fe99be9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a>Hozzon létre metrika riasztások Azure figyelése az Azure-szolgáltatások - platformfüggetlen parancssori Felülettel
> [!div class="op_single_selector"]
> * [Portál](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [Parancssori felület](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan használja az Azure metrika figyelmeztetéseket tooset hello platformfüggetlen parancssori felület (CLI).

> [!NOTE]
> Az Azure a figyelő az hello új neve "Azure Insights" nevezett csak 2016. Szeptembertől 25. Azonban hello névterek, ezért az alábbi parancsok hello továbbra is tartalmazhat hello "insights".
>
>

A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.

* **Metrika értékek** – hello eseményindítók riasztást, ha a megadott metrika értékét hello mindkét irányban rendel a küszöbérték keverve használ. Ez azt jelenti, hogy elindítja a mindkét amikor először hello feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.    
* **Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha egy bizonyos események következik be. További információk a napló tevékenységriasztásokat toolearn [kattintson ide](monitoring-activity-log-alerts.md)

A metrika riasztási toodo hello követően amikor elindítja a konfigurálhatja:

* e-mail értesítések toohello szolgáltatás-rendszergazda és a társadminisztrátorok küldése
* e-mail küldése a megadott tooadditional e-maileket.
* A webhook hívása
* egy Azure-runbook (csak az Azure-portál jelenleg hello) végrehajtásának elindítása

Konfigurálhatja és metrika riasztási szabályok adatainak beolvasása

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [parancssori felület (CLI)](insights-alerts-command-line-interface.md)
* [Az Azure figyelő REST API-n](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Mindig fogadhat parancsokhoz tartozó súgók parancs beírásával és üzembe - súgó hello végén. Példa:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-hello-cli"></a>Hello CLI riasztási szabályok létrehozására
1. Hajtsa végre a hello Előfeltételek és a bejelentkezési tooAzure. Lásd: [Azure figyelő CLI minták](insights-cli-samples.md). Röviden hello parancssori felület telepítése, és futtassa az alábbi parancsokat. Ezt első jelentkezett be, milyen előfizetésre használ, és felkészülni toorun Azure figyelő parancsok megjelenítése.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. meglévő szabályok toolist egy erőforráscsoport, használja a következő képernyő hello **riasztások szabályához lista azure insights** *[kapcsolók] &lt;resourceGroup&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. a szabály toocreate kell toohave több fontos adatra először.
  * Hello **erőforrás-azonosító** hello erőforrás keresi tooset riasztást
  * Hello **metrikai meghatározásainak** az adott erőforrás érhető el

     Egyirányú tooget hello erőforrás-azonosító toouse hello Azure-portálon. Ha hello erőforrás létrehozása már be van állítva, válassza ki azt a hello portálon. Hello következő panelen válassza ki *tulajdonságok* alatt hello *beállítások* szakasz. Hello *erőforrás-azonosító* mező kitöltése hello következő panelen. Egy másik módja toouse hello [Azure erőforrás-kezelő](https://resources.azure.com/).

     Egy példa egy webalkalmazást az erőforrás-azonosító

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     tooget hello elérhető metrikák és ezek metrikák hello előző erőforrás például a következő parancsot a CLI használata hello mértékegységét listáját:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* hello granularitási hello elérhető mérési (1 perces időközönként) van. Különböző Granularitás van használatával különböző metrika lehetőséget biztosít.
4. toocreate metrika-alapú riasztási szabály, a hello képernyőn a következő paranccsal:

    **Azure-riasztások metrika szabálykészlet insights** *[kapcsolók] &lt;ruleName&gt; &lt;hely&gt; &lt;resourceGroup&gt; &lt;ablakméret&gt; &lt;operátor&gt; &lt;küszöbérték&gt; &lt;targetresourceid azonosítója&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*

    Példa állítja be a riasztást követő webhely erőforráson hello. hello riasztási eseményindítók Ha 5 percig, majd újra amikor megkapja sincs forgalom 5 percig következetesen kap minden forgalom.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. toocreate webhook vagy küldése e-mailt egy metrika riasztás aktiválódásakor először létre kell hoznia hello e-mailek és/vagy webhook. Ezután hozzon létre azonnal hello szabályt ezt követően. Nem társítható webhook vagy e-mailek már létrehozott szabályok hello parancssori felület használatával.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Ellenőrizheti, hogy a riasztások elkészültek megfelelően az egyes szabályok alapján.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. toodelete szabályok hello űrlap paranccsal:

    **riasztások szabály törlése insights** [kapcsolók] &lt;resourceGroup&gt; &lt;szabálynév&gt;

    Ezek a parancsok a cikkben korábban létrehozott hello szabályok törlése.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Következő lépések
* [Az Azure Figyelés áttekintése](monitoring-overview.md) például hello típusú információkat gyűjt, és figyelheti.
* További információ [konfigurálása webhookokkal a riasztások](insights-webhooks-alerts.md).
* További információ [riasztások konfigurálása a naplózási eseményeket](monitoring-activity-log-alerts.md).
* További információ [Azure Automation-forgatókönyveket](../automation/automation-starting-a-runbook.md).
* Első egy [diagnosztikai naplók gyűjtésére áttekintése](monitoring-overview-of-diagnostic-logs.md) toocollect részletes nagyon gyakori metrikákat a szolgáltatásban.
* Első egy [metrikák gyűjtemény áttekintése](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
