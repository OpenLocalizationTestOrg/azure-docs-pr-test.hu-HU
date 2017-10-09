---
title: "a Microsoft Azure-ban mérőszámok aaaOverview |} Microsoft Docs"
description: "Metrikák és a használatukat a Microsoft Azure-ban – áttekintés"
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 405ec51c-0946-4ec9-b535-60f65c4a5bd1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: johnkem
ms.openlocfilehash: 2b97f51e0554dae95a929241ae1f0e25e5c215ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>A Microsoft Azure-ban mérőszámok áttekintése
Ez a cikk ismerteti, hogy milyen adatok gyűjtése le van a Microsoft Azure, az előnyöket, és hogyan toostart használja őket.  

## <a name="what-are-metrics"></a>Mik azok a metrikák?
Az Azure a figyelő lehetővé teszi tooconsume telemetriai toogain láthatósága hello teljesítmény- és a munkaterhelések Azure állapot. hello Azure "telemetrikus" adatok legfontosabb típusú hello metrikák (más néven teljesítményszámlálók) az Azure erőforrások által kibocsátott. A figyelő az Azure számos módon tooconfigure biztosít, és figyeléshez és hibaelhárításhoz metrikákat felhasználását.

## <a name="what-can-you-do-with-metrics"></a>Mire szolgál a metrikák?
Metrikák egy értékes telemetriai adatok forrását, és lehetővé teszik a következő feladatok toodo hello:

* **Hello teljesítményének nyomon** az erőforrás (például egy virtuális gép, a webhely vagy a logikai alkalmazást) a metrikák a portál diagramon ábrázolásához és diagram tooa irányítópult rögzítését.
* **Probléma értesítés** , hogy a hatások metrika ebbe a meghatározott küszöbérték hello az erőforrás teljesítményét.
* **Konfigurálja az automatikus műveleteket**, például az automatikus skálázás egy erőforrás vagy runbook kiváltó metrika ebbe a meghatározott küszöbérték.
* **Hajtsa végre a speciális elemzés** vagy a teljesítmény vagy a használati trendeket a erőforrás jelentés.
* **Archív** az erőforrás teljesítmény- vagy állapotának előzménye hello **megfelelőség és naplózás** céljából.

## <a name="what-are-hello-characteristics-of-metrics"></a>Mik azok a metrikák hello jellemzői?
Metrikák hello a következő jellemzőkkel rendelkezik:

* Összes metrikát rendelkezik **egy perces gyakoriságot**. Kapott a metrika értékét percenként az erőforrás felkínálva közel valós idejű információkat hello állapot és az erőforrás állapotát.
* Adatok gyűjtése le van **érhető el azonnal**. Nem kell a tooopt, vagy állítsa be a további diagnosztikához.
* Van-e hozzáférési **előzmények 30 napnyi** minden mérőszám. Gyorsan megtalálhatja hello legutóbbi és havi folyamatainak hello teljesítmény- vagy az erőforrás állapotát.

További lehetőségek:

* Metrika konfigurálása **szabályt, amely értesítést küld vagy fogad automatikus művelet riasztás** amikor hello metrika áthalad hello küszöbérték beállítását. Automatikus skálázás, amely lehetővé teszi, hogy az erőforrás-toomeet bejövő kéréseket tooscale vagy a webhely vagy a számítási erőforrások betölti további automatizált műveletet. Az automatikus skálázási beállítás szabály tooscale bejövő vagy kimenő küszöbértéket meghaladó metrika alapján konfigurálhatja.

* **Útvonal** összes metrikákat az Application Insights vagy Naplóelemzés (OMS) tooenable azonnali elemzés és a metrikai adatok az erőforrások közül egyéni riasztást küld. Is adatfolyam formájában metrikák tooan Eseményközpont, amely lehetővé teszi, toothen irányíthatja őket tooAzure Stream Analytics vagy a közel valós idejű elemzési toocustom-alkalmazásokat. Beállíthatja az Event Hubs streaming a diagnosztikai beállítások használatával.

* **Archivált metrikák toostorage** a hosszabb megőrzési vagy offline jelentéskészítésre használja őket. Az erőforrás diagnosztikai beállításainak konfigurálásakor irányítani tudja a metrikák tooAzure Blob Storage tárolóban.

* Könnyen felderítése, fér hozzá, és **megtekintése összes metrikát** hello jelöljön ki egy erőforrást és tőzsdei diagram hello metrikák Azure portálon keresztül.

* **Felhasználását** hello metrikák keresztül hello új Azure figyelő REST API-k.

* **Lekérdezés** metrikák használatával hello PowerShell-parancsmagok vagy hello platformfüggetlen REST API-t.

  ![Az Azure a figyelő a metrikák útválasztását](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-hello-portal"></a>Hozzáférés metrikák hello portálon
Az alábbiakban látható egy gyors útmutató hogyan toocreate használatával metrika diagram hello Azure-portálon.

### <a name="tooview-metrics-after-creating-a-resource"></a>erőforrás létrehozása után tooview metrikák
1. Nyissa meg hello Azure-portálon.
2. Az Azure App Service-webhelyet hoz létre.
3. Miután létrehozott egy webhelyen, lépjen a toohello **áttekintése** hello webhely paneljén.
4. Új mérőszámok, megtekintheti a **figyelés** csempére. Módosíthatja a hello csempére, majd kiválaszthatja a további metrikákat.

   ![Az Azure-figyelő erőforrás metrikáit](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="tooaccess-all-metrics-in-a-single-place"></a>tooaccess összes metrikát egyetlen helyen
1. Nyissa meg hello Azure-portálon.
2. Keresse meg az új toohello **figyelő** fülre, és ezután és select hello **metrikák** alatta lehetőséget.
3. Válassza ki az előfizetést, erőforráscsoport és hello erőforrás neve hello hello legördülő listából.
4. Hello elérhető lista megtekintése. Ezután válassza ki a hello metrika kapcsolatban, és azt megrajzolásához.
5. Akkor is rögzítheti toohello irányítópult hello jobb felső sarkában hello PIN-kódjának kattintva.

   ![Azure-figyelőben egyetlen helyen összes metrikát eléréséhez](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> (Azure Resource Manager-alapú) virtuális gépek gazdaszintű metrikák érhető el, és a virtulálisgép-skálázási készletekben további diagnosztikai beállítások elvégzése nélkül. Új gazdagép-szintű metrikákat a Windows és Linux-példányok érhetők el. A metrikák nincsenek hello bekapcsolása a virtuális gépek vagy virtuálisgép-méretezési csoportok Azure Diagnostics hozzáférés toowhen rendelkező Vendég-operációsrendszer-szintű metrikák összetéveszthető toobe. toolearn bővebben diagnosztika, konfigurálásáról lásd: [Újdonságok a Microsoft Azure Diagnostics](../azure-diagnostics.md).
>
>

## <a name="access-metrics-via-hello-rest-api"></a>Hozzáférés metrikák hello REST API-n keresztül
Az Azure metrikák hello Azure figyelő API-k keresztül érhetők el. Két API-k, amelyek segítenek felderítése és metrikákat eléréséhez:

* Használjon hello [Azure figyelő metrika definíciók REST API](https://msdn.microsoft.com/library/mt743621.aspx) tooaccess hello szolgáltatás elérhető metrikák listája.
* Használjon hello [Azure figyelő metrikák REST API](https://msdn.microsoft.com/library/mt743622.aspx) tooaccess hello tényleges metrikai adatok.

> [!NOTE]
> Ez a cikk foglalkozik hello metrikák keresztül hello [új API-t a metrikák](https://msdn.microsoft.com/library/dn931930.aspx) az Azure-erőforrások. hello új metrikai meghatározásainak API hello API-verzió: 2016-03-01, és a metrikák API hello verzió: 2016-09-01. hello API verziója 2014-04-01 hello örökölt metrikai meghatározásainak és metrikák hozzáférhetők.
>
>

Hello Azure figyelő REST API-k használatával részletes útmutatást lásd: [Azure figyelő REST API-forgatókönyv](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Exportálás metrikák
Elvégezheti a toohello **diagnosztikai beállítások** részen hello **figyelő** lapon és a nézet hello exportálási beállítások metrikákat. Választhatja metrikák (és diagnosztikai naplók) toobe irányított tooBlob tárolási, tooAzure Event Hubs vagy tooOMS említett használatából adódó korábban ebben a cikkben.

 ![Exportálási beállítások a Azure figyelő metrikák](./media/monitoring-overview-metrics/MetricsOverview3.png)

A Resource Manager-sablonok, konfigurálható [PowerShell](insights-powershell-samples.md), [Azure CLI](insights-cli-samples.md), vagy [REST API-k](https://msdn.microsoft.com/library/dn931943.aspx).

## <a name="take-action-on-metrics"></a>Művelet végrehajtása a metrikák
tooreceive értesítések vagy metrikaadatokat automatikus hajtsa végre a megfelelő műveleteket, beállíthatja a riasztási szabályok vagy az automatikus skálázási beállításokat.

### <a name="configure-alert-rules"></a>A riasztási szabályok konfigurálása
Riasztási szabályok metrikák a konfigurálható. Ezek a riasztási szabályok ellenőrizheti, ha egy metrika elért egy bizonyos küszöböt. Azok majd e-mailben értesítse arról, vagy olyan webhook használt toorun használható egyéni parancsfájlok érvényesítést. Hello webhook tooconfigure harmadik féltől származó terméket Integrációk is használható.

 ![Metrikák és az Azure-figyelő riasztási szabályok](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale-your-azure-resources"></a>Automatikus skálázási az Azure erőforrások
Egyes Azure-erőforrások skálázás fel- vagy a több példány toohandle a munkaterhelések hello támogatja. Automatikus skálázási tooApp (Web Apps) szolgáltatás, a virtuálisgép-méretezési csoportok és a klasszikus Azure-Felhőszolgáltatások vonatkozik. Konfigurálhatja az automatikus skálázási szabályok tooscale fel- vagy bizonyos metrika, amely hatással van a számítási feladatok ebbe a megadott küszöbértéket. További információkért lásd: [automatikus skálázás áttekintése](monitoring-overview-autoscale.md).

 ![Metrikák és az Azure a figyelő automatikus skálázás](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Támogatott szolgáltatások és
Az Azure figyelő egy új metrikus infrastruktúrája. A következő Azure-szolgáltatásokat az Azure portál és hello új verziójának hello Azure figyelő API hello hello támogatja:

* Virtuális gépek (az Azure Resource Manager-alapú)
* Virtuálisgép-méretezési csoportok
* Batch
* Event Hubs névtér
* Service Bus-névtér (csak a Termékváltozat. prémium)
* SQL-adatbázis (12-es verzió)
* A rugalmas SQL-készlet
* Webhelyek
* Web server farms
* Logic Apps
* IoT-központok
* Redis Cache
* Hálózatkezelés: Alkalmazásátjárót
* Keresés

Megtekintheti az összes támogatott hello szolgáltatás és a metrikákat, részletes listáját [Azure figyelő metrikák--erőforrás típusonkénti támogatott metrikák](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Következő lépések
Ez a cikk toohello hivatkozásaiban hivatkozik. Ezenkívül további információk:  

* [Az automatikus skálázás közös metrikák](insights-autoscale-common-metrics.md)
* [Hogyan toocreate riasztási szabályok](insights-alerts-portal.md)
* [A Naplóelemzési az Azure storage naplóinak elemzése](../log-analytics/log-analytics-azure-storage.md)
