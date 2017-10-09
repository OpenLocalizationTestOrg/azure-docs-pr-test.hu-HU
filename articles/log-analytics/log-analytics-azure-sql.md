---
title: "a Naplóelemzési SQL elemzési megoldások aaaAzure |} Microsoft Docs"
description: "hello Azure SQL elemzési megoldások kezelheti az Azure SQL Database adatbázisok."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: fe228bb3cb3f9d578a84707c3917f02fbeb8a627
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Azure SQL adatbázis Azure SQL elemzés (előzetes verzió) Naplóelemzési figyelése

![Az Azure SQL elemzés szimbólum](./media/log-analytics-azure-sql/azure-sql-symbol.png)

hello Azure SQL elemzési megoldás az Azure Naplóelemzés gyűjt, és fontos SQL Azure teljesítménymutatók visualizes. Hello metrikák gyűjtött hello megoldás segítségével létrehozhat egyéni ellenőrzési szabályok és értesítések. Figyelheti, hogy az Azure SQL Database és a rugalmas készlet metrikák több Azure-előfizetések és rugalmas készletek, és jelenítheti meg őket. hello megoldás is segítséget nyújt az alkalmazáscsoportokat az egyes rétegekben tooidentify problémákat.  Használja [Azure diagnosztikai metrikák](log-analytics-azure-storage.md) együtt Naplóelemzési megtekinti egyetlen Naplóelemzési munkaterület az Azure SQL-adatbázisok és a rugalmas készletek toopresent adatait.

Jelenleg ez a minta megoldás legfeljebb too150, 000 Azure SQL-adatbázisok és 5000 SQL rugalmas készletek / munkaterület támogat.

Azure SQL elemzési megoldás, mások számára elérhető Naplóelemzési, például hello segítségével megfigyelheti és hello állapotát az Azure-erőforrások értesítések fogadása – ebben az esetben az Azure SQL Database. A Microsoft Azure SQL Database az ismerős SQL-kiszolgáló-szerű képességeket tooapplications hello Azure felhőben futó biztosító méretezhető relációs adatbázis-szolgáltatás. A Naplóelemzési toocollect nyújt segítséget, összefüggéseket és strukturált és strukturálatlan adatok megjelenítése.

## <a name="connected-sources"></a>Összekapcsolt források

hello Azure SQL elemzési megoldások ügynökök tooconnect toohello Naplóelemzés szolgáltatás nem használja.

a következő táblázat hello hello csatlakoztatott adatforrások, ez a megoldás által támogatott ismerteti.

| Összekapcsolt forrás | Támogatás | Leírás |
| --- | --- | --- |
| [Windows-ügynökök](log-analytics-windows-agents.md) | Nem | A közvetlen Windows-ügynökök nem hello megoldás által használt. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem | Közvetlen Linux-ügynökök nem hello megoldás által használt. |
| [SCOM felügyeleti csoport](log-analytics-om-agents.md) | Nem | Hello SCOM ügynök tooLog Analytics a közvetlen kapcsolat hello megoldás nem használja. |
| [Azure Storage-fiók](log-analytics-azure-storage.md) | Nem | A Naplóelemzési nem hello adatokat olvasni egy tárfiókot. |
| [Az Azure diagnostics](log-analytics-azure-storage.md) | Igen | Az Azure metrika adatküldést tooLog Analytics közvetlenül az Azure-ban. |

## <a name="prerequisites"></a>Előfeltételek

- Azure-előfizetéssel. Ha még nincs fiókja, létrehozhat egy a [szabad](https://azure.microsoft.com/free/).
- A Naplóelemzési munkaterület. Egy meglévő is használhatja, vagy beállíthatja a [hozzon létre egy újat](log-analytics-get-started.md) Ez a megoldás használata előtt.
- Azure Diagnostics engedélyezése az Azure SQL-adatbázisok és rugalmas készletek és [toosend konfigurálja őket az adatok tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfiguráció

Hajtsa végre a következő lépéseket tooadd hello Azure SQL elemzés megoldás tooyour munkaterület hello.

1. Hello Azure SQL elemzés megoldás tooyour munkaterület hozzáadása [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. Hello Azure-portálon, kattintson **új** (hello + szimbólumra), majd hello az erőforrások listájához, jelölje ki **figyelés + felügyeleti**.  
    ![Felügyelet és kezelés](./media/log-analytics-azure-sql/monitoring-management.png)
3. A hello **figyelés + felügyeleti** listában kattintson **láthatja az összes**.
4. A hello **ajánlott** listában, kattintson **további** , majd a hello új listában keresse meg **Azure SQL elemzés (előzetes verzió)** meg és jelölje ki.  
    ![Az Azure SQL elemzési megoldások](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. A hello **Azure SQL elemzés (előzetes verzió)** panelen kattintson a **létrehozása**.  
    ![Létrehozás](./media/log-analytics-azure-sql/portal-create.png)
6. A hello **hozzon létre új megoldás** panelen, jelölje be hello munkaterület tooadd hello megoldás tooand szeretné, majd kattintson a **létrehozása**.  
    ![tooworkspace hozzáadása](./media/log-analytics-azure-sql/add-to-workspace.png)


### <a name="tooconfigure-multiple-azure-subscriptions"></a>tooconfigure több Azure-előfizetéssel

toosupport több előfizetéssel hello PowerShell parancsfájl használatát [engedélyezése Azure erőforrás metrikáit naplózás PowerShell-lel](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Adja meg a hello munkaterület erőforrás-azonosító paramétert hello parancsfájl toosend diagnosztikai adatokat egy Azure-előfizetéssel tooa munkaterületén más Azure-előfizetések erőforrásokból végrehajtásakor.

**Példa**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-hello-solution"></a>Hello megoldással

Hello megoldás tooyour munkaterület hozzáadásakor hello Azure SQL elemzés csempe kerüljön tooyour munkaterület, és úgy tűnik, az Áttekintés. hello csempe az Azure SQL-adatbázisok és az Azure SQL rugalmas készletek hello megoldás kapcsolódó hello számát jeleníti meg.

![Az Azure SQL elemzés csempe](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL analitikai adatok megtekintése

Kattintson a hello **Azure SQL elemzés** csempe tooopen hello Azure SQL-elemzések irányítópultján. hello irányítópult tartalmazza az alábbiakban meghatározott hello paneleken. Minden egyes panel too15 erőforrások (előfizetés, kiszolgáló, a rugalmas készlet és adatbázis) sorolja fel. Kattintson bármelyik hello erőforrások tooopen hello irányítópult, hogy az adott erőforráshoz. A rugalmas készlet vagy adatbázis tartalmazza a hello diagramok a kiválasztott erőforrás metrikáit. Kattintson a diagram tooopen hello napló keresése párbeszédpanel.

| Panel | Leírás |
|---|---|
| Előfizetések | Előfizetések számának csatlakoztatott kiszolgálók, a készletek és az adatbázisok listája. |
| Kiszolgálók | Kiszolgálók számának csatlakoztatott készletek és az adatbázisok listája. |
| Rugalmas készletek | A maximális GB és a megfigyelt hello eDTU csatlakoztatott rugalmas készletek listája időszak. |
|Adatbázisok | A megfigyelt hello maximális GB és DTU csatlakoztatott adatbázisok listája időszak.|


### <a name="analyze-data-and-create-alerts"></a>Adatok elemzése és értesítések

Az Azure SQL adatbázis-erőforrások érkező hello adatokkal könnyen riasztásokat hozhat létre. Íme néhány hasznos [naplófájl-keresési](log-analytics-log-searches.md) riasztások használható lekérdezéseket:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Az Azure SQL Database magas DTU*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

*Magas, az Azure SQL Database rugalmas készlet DTU*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

A megadott küszöbértékek a riasztás-alapú lekérdezések tooalert használhatja az Azure SQL Database és a rugalmas készletek. tooconfigure egy riasztást az OMS-munkaterület:

#### <a name="tooconfigure-an-alert-for-your-workspace"></a>a munkaterület riasztást tooconfigure

1. Nyissa meg toohello [OMS-portálon](http://mms.microsoft.com/) , jelentkezzen be.
2. Nyissa meg a hello munkaterület hello megoldás konfigurált.
3. Hello áttekintése lapon kattintson a hello **Azure SQL elemzés (előzetes verzió)** csempére.
4. Hello mintalekérdezéseket egyikét futtatja.
5. Kattintson a napló keresési **riasztási**.  
![riasztás létrehozása, a keresés](./media/log-analytics-azure-sql/create-alert01.png)
6. A hello **riasztási szabály hozzáadása** lapon adja meg a megfelelő tulajdonságok hello és hello megadott küszöbértékek, majd kattintson **mentése**.  
![riasztási szabály hozzáadása](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-analytics-data"></a>Azure SQL analitikai adatok intézkedjen

Tegyük fel hello leghasznosabb lekérdezések hajthat végre egyik toocompare hello DTU-felhasználásban összes Azure SQL rugalmas készlet az összes Azure-előfizetések között. Adatbázis átviteli egységek (DTU) tartalmaz egy módja toodescribe hello a Basic, Standard és Premium adatbázisok és a készletek teljesítményszintjének relatív kapacitását. Dtu-inak száma CPU egy kevert mérésén alapulnak, memória, olvas és ír. Mivel Dtu növelje, hello szintű teljesítménynövekedést által kínált hello power. Például egy 5 dtu-val rendelkező teljesítményszintet áramellátása ötször több mint egy teljesítményszintet az 1 DTU. A maximális DTU-kvótáról tooeach kiszolgáló és a rugalmas készlet vonatkozik.

A következő naplófájl-keresési lekérdezés hello futtatásával könnyen megállapítható, ha vannak kihasználatlanul hagy vagy az SQL Azure rugalmas készletek több mint használatával.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés hello megváltozna toohello következő.
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

A következő példa hello, láthatja, hogy rendelkezik-e egy magas kihasználtsága 100 %-os közelében egy rugalmas készlet DTU, míg mások csak nagyon kis használati rendelkeznek. További tootroubleshoot lehetséges legutóbbi módosításainak használatával megvizsgálhatja a Azure tevékenységi naplóit a környezetben.

![Naplófájl-keresési eredmények – magas kihasználtsága](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Lásd még:

- Használjon [napló keresések](log-analytics-log-searches.md) Naplóelemzési tooview részletes Azure SQL data.
- [Hozzon létre egy saját irányítópultok](log-analytics-dashboards.md) Azure SQL-adatok jelennek meg.
- [Hozzon létre a riasztások](log-analytics-alerts.md) Ha meghatározott Azure SQL-esemény történik.
