---
title: "A Naplóelemzési Azure SQL elemzési megoldások |} Microsoft Docs"
description: "Az Azure SQL elemzési megoldások kezelheti az Azure SQL Database adatbázisok."
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
ms.openlocfilehash: cab45cc6dd621eb4a95ef5f1842ec38c25e980b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Azure SQL adatbázis Azure SQL elemzés (előzetes verzió) Naplóelemzési figyelése

![Az Azure SQL elemzés szimbólum](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Az Azure SQL elemzési megoldás az Azure Naplóelemzés gyűjt, és fontos SQL Azure teljesítménymutatók visualizes. A metrikák gyűjtött megoldás segítségével létrehozhat egyéni ellenőrzési szabályok és értesítések. Figyelheti, hogy az Azure SQL Database és a rugalmas készlet metrikák több Azure-előfizetések és rugalmas készletek, és jelenítheti meg őket. A megoldás is segítséget az alkalmazás-készlet minden egyes rétegben problémák azonosításához.  Használja [Azure diagnosztikai metrikák](log-analytics-azure-storage.md) Log Analytics-nézetet megjeleníteni az adatokat az Azure SQL-adatbázisok és rugalmas készletek egyetlen Naplóelemzési munkaterület együtt.

Jelenleg ez preview megoldás legfeljebb 150 000 Azure SQL-adatbázisok és 5000 SQL rugalmas készletek / munkaterület támogatja.

Az Azure SQL elemzési megoldás, mások számára elérhető Naplóelemzési, például segítségével megfigyelheti és az Azure-erőforrások értesítéseket állapotával kapcsolatos – ebben az esetben az Azure SQL Database. A Microsoft Azure SQL Database egy méretezhető relációs adatbázis-szolgáltatás, amely az Azure felhőben futó alkalmazások ismerős SQL-kiszolgáló-szerű képességeket biztosít. A Naplóelemzési segítségével gyűjtése, összefüggéseket és strukturált és strukturálatlan adatok megjelenítése.

## <a name="connected-sources"></a>Összekapcsolt források

Az Azure SQL elemzési megoldások ügynökök nem használ, a Naplóelemzés szolgáltatáshoz való kapcsolódáshoz.

Az alábbi táblázat áttekintést nyújt az ebben a megoldásban támogatott összekapcsolt forrásokról.

| Összekapcsolt forrás | Támogatás | Leírás |
| --- | --- | --- |
| [Windows-ügynökök](log-analytics-windows-agents.md) | Nem | A közvetlen Windows-ügynökök nem használják a megoldás. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem | Közvetlen Linux-ügynökök nem használják a megoldás. |
| [SCOM felügyeleti csoport](log-analytics-om-agents.md) | Nem | Közvetlen kapcsolat az SCOM-ügynököt a szolgáltatáshoz a megoldás nem használja. |
| [Azure Storage-fiók](log-analytics-azure-storage.md) | Nem | A Naplóelemzési nem beolvasni az adatokat egy tárfiókot. |
| [Az Azure diagnostics](log-analytics-azure-storage.md) | Igen | Azure metrikaadatokat közvetlenül az Azure Log Analytics érkezik. |

## <a name="prerequisites"></a>Előfeltételek

- Azure-előfizetéssel. Ha még nincs fiókja, létrehozhat egy a [szabad](https://azure.microsoft.com/free/).
- A Naplóelemzési munkaterület. Egy meglévő is használhatja, vagy beállíthatja a [hozzon létre egy újat](log-analytics-get-started.md) Ez a megoldás használata előtt.
- Azure Diagnostics engedélyezése az Azure SQL-adatbázisok és rugalmas készletek és [úgy konfigurálja azokat az adatokat küldeni a Naplóelemzési](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfiguráció

A következő lépésekkel adja hozzá az Azure SQL elemzési megoldások a munkaterületre.

1. Az Azure SQL elemzési megoldások hozzáadása a munkaterület [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot követve [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).
2. Az Azure portálon kattintson **új** (a + szimbólumra), majd válassza az erőforrások listájához, **figyelés + felügyeleti**.  
    ![Felügyelet és kezelés](./media/log-analytics-azure-sql/monitoring-management.png)
3. Az a **figyelés + felügyeleti** listában kattintson **láthatja az összes**.
4. Az a **ajánlott** listában, kattintson **további** , majd új listájában keresse meg **Azure SQL elemzés (előzetes verzió)** meg és jelölje ki.  
    ![Az Azure SQL elemzési megoldások](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. Az a **Azure SQL elemzés (előzetes verzió)** panelen kattintson a **létrehozása**.  
    ![Létrehozás](./media/log-analytics-azure-sql/portal-create.png)
6. Az a **hozzon létre új megoldás** panelen, válassza ki, amely hozzá szeretné adni, hogy a munkaterület, és kattintson **létrehozása**.  
    ![munkaterület felvétele](./media/log-analytics-azure-sql/add-to-workspace.png)


### <a name="to-configure-multiple-azure-subscriptions"></a>Több Azure-előfizetések beállítása

Több előfizetések támogatásához a PowerShell parancsfájl használatát [engedélyezése Azure erőforrás metrikáit naplózás PowerShell-lel](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Adja meg a munkaterület-azonosító paramétert, a diagnosztikai adatokat küldeni a források egy Azure-előfizetésben más Azure-előfizetések munkaterületeinek a parancsfájl végrehajtása közben.

**Példa**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>A megoldás használata

A megoldás a munkaterülethez való hozzáadásakor az Azure SQL elemzés csempe hozzáadódik a munkaterület, és megjeleníti a áttekintése. A csempe az Azure SQL-adatbázisok és az Azure SQL rugalmas készletek a megoldás csatlakozik a számát mutatja.

![Az Azure SQL elemzés csempe](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL analitikai adatok megtekintése

Kattintson a **Azure SQL elemzés** csempére kattintva nyissa meg az Azure SQL-elemzések irányítópultján. Az irányítópult az alább megadott paneleken tartalmazza. Minden egyes panel legfeljebb 15 források (előfizetés, kiszolgáló, a rugalmas készlet és adatbázis) tartalmazza. Kattintson az erőforrásokat, hogy az adott erőforráshoz az irányítópult megnyitásához. A rugalmas készlet vagy adatbázis tartalmazza a kiválasztott erőforrás metrikáit rendelkező diagramok. Kattintson a diagram a naplófájl-keresési párbeszédpanel megnyitásához.

| Panel | Leírás |
|---|---|
| Előfizetések | Előfizetések számának csatlakoztatott kiszolgálók, a készletek és az adatbázisok listája. |
| Kiszolgálók | Kiszolgálók számának csatlakoztatott készletek és az adatbázisok listája. |
| Rugalmas készletek | A maximális GB és edtu-ra a vizsgált időszakban csatlakoztatott rugalmas készletek listája. |
|Adatbázisok | A vizsgált időszakban maximális GB és DTU csatlakoztatott adatbázisok listája.|


### <a name="analyze-data-and-create-alerts"></a>Adatok elemzése és értesítések

Az Azure SQL Database-forrásokból származó adatokat tartalmazó könnyen riasztásokat hozhat létre. Íme néhány hasznos [naplófájl-keresési](log-analytics-log-searches.md) riasztások használható lekérdezéseket:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Az Azure SQL Database magas DTU*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

*Magas, az Azure SQL Database rugalmas készlet DTU*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

A riasztás-alapú lekérdezések segítségével az Azure SQL Database és a rugalmas készletek megadott küszöbértékek riasztás. Az OMS-munkaterület a riasztás konfigurálása:

#### <a name="to-configure-an-alert-for-your-workspace"></a>A munkaterület riasztás konfigurálása

1. Lépjen a [OMS-portálon](http://mms.microsoft.com/) , jelentkezzen be.
2. Nyissa meg a munkaterületet, a megoldás konfigurált.
3. A áttekintése lapon kattintson a **Azure SQL elemzés (előzetes verzió)** csempére.
4. A példa lekérdezések egyikét futtatja.
5. Kattintson a napló keresési **riasztási**.  
![riasztás létrehozása, a keresés](./media/log-analytics-azure-sql/create-alert01.png)
6. Az a **riasztási szabály hozzáadása** lapján konfigurálja a megfelelő tulajdonságokat és a meghatározott küszöbértékeket, majd kattintson **mentése**.  
![riasztási szabály hozzáadása](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-analytics-data"></a>Azure SQL analitikai adatok intézkedjen

Például végezheti el a leghasznosabb lekérdezések egyik hasonlítsa össze az összes Azure SQL rugalmas készlet DTU-felhasználásban minden Azure-előfizetés között. Adatbázis átviteli egységek (DTU) a Basic, Standard és Premium adatbázisok és a készletek teljesítményszintjének relatív kapacitását lehetőséget biztosít. Dtu-inak száma CPU egy kevert mérésén alapulnak, memória, olvas és ír. Dtu növelje, a teljesítmény a teljesítményszintet által kínált növekszik. Például egy 5 dtu-val rendelkező teljesítményszintet áramellátása ötször több mint egy teljesítményszintet az 1 DTU. A maximális DTU-kvótát minden egyes kiszolgáló és a rugalmas készlet vonatkozik.

A következő naplófájl-keresési lekérdezés futtatásával könnyen megállapítható, ha a kihasználatlanul vannak hagy, vagy az SQL Azure rugalmas készletek okhoz keresztül.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> Ha a munkaterületet lett frissítve a [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés megváltozna a következők.
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

A következő példában láthatja, hogy rendelkezik-e egy magas kihasználtsága 100 %-os közelében egy rugalmas készlet DTU, míg mások csak nagyon kis használati rendelkeznek. Használatával megvizsgálhatja a további lehetséges legutóbbi módosítások Azure tevékenység-naplók segítségével környezetében elhárítása.

![Naplófájl-keresési eredmények – magas kihasználtsága](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Lásd még:

- Használjon [napló keresések](log-analytics-log-searches.md) a Log Analyticshez Azure SQL részletes adatainak megtekintéséhez.
- [Hozzon létre egy saját irányítópultok](log-analytics-dashboards.md) Azure SQL-adatok jelennek meg.
- [Hozzon létre a riasztások](log-analytics-alerts.md) Ha meghatározott Azure SQL-esemény történik.
