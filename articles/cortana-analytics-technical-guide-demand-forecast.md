---
title: "Igény-előrejelzés energia műszaki útmutató |} Microsoft Docs"
description: "Egy technikai útmutató a Microsoft a Cortana Intelligence megoldás-sablonnal a havi előrejelzési igény."
services: cortana-analytics
documentationcenter: 
author: yijichen
manager: ilanr9
editor: yijichen
ms.assetid: 7f1a866b-79b7-4b97-ae3e-bc6bebe8c756
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: inqiu;yijichen;ilanr9
ms.openlocfilehash: c3bbef8fee018dc54e7d3edb86e3f9434999bdae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Műszaki útmutató a Cortana Intelligence megoldás sablont a havi előrejelzési igény szerint
## <a name="overview"></a>**Áttekintés**
Megoldás sablonok készültek, annak érdekében, a folyamatot nevezzük, egy E2E bemutató Cortana Intelligence Suite felett. Egy telepített sablon kiépítéséhez szükséges Cortana Intelligence összetevő előfizetés, és a közötti kapcsolatok létrehozása. Azt is magok az adatok feldolgozási sor egy adatok szimuláció alkalmazás által létrehozott első mintaadatokkal. Az adatok szimulátor töltse le a megadott és a helyi számítógépre telepítse, a readme.txt fájlban további információk az a szimulátor használatával. A szimulátor előállított adatokat fog hydrate, az adatok feldolgozási sorban lévő és a machine learning előrejelzés amely megjeleníthetők a Power BI-irányítópult létrehozása start.

A megoldássablon található [Itt](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

A telepítési folyamat végigvezeti a megoldás hitelesítő adatok beállításához számos lépést. Ellenőrizze, hogy a hitelesítő adatokat, például a megoldás neve, a felhasználónevet és jelszót a telepítés során megadott rögzítése.

A jelen dokumentum célja a referencia-architektúrában és a különböző összetevőket az előfizetésében megoldás sablon részeként. A dokumentum is szól a mintaadatok lecserélése insights/előrejelzéseket az Ön nyert adatokat megtekintheti a saját adatok valóságosak módjáról. Emellett a dokumentum megbeszélések lehet módosítani, ha szeretné testre szabni a megoldás a saját adataival kell megoldás sablon részeivel kapcsolatos. A Power BI-irányítópultot megoldás sablon létrehozásával kapcsolatos útmutatást végén.

## <a name="big-picture"></a>**Nagy vonalakban tekinti**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architektúra ismertetése
Amikor a megoldást már telepítették, Cortana Analytics Suite belül különböző Azure-szolgáltatások rendszer aktiválja azokat (*azaz* Az Event Hubs, a Stream Analytics, HDInsight, adat-előállító gépi tanulás, *stb*). A fenti architektúrája látható, magas szinten, hogyan energia Megoldássablon az igény szerinti előrejelzés értékekből összeállított végpontok közötti. Vizsgálja meg ezeket a szolgáltatásokat a megoldás üzembe helyezése a létrehozott megoldás sablon diagram rájuk kattintva lehet. A következő szakaszok ismertetik a minden egyes adatra.

## <a name="data-source-and-ingestion"></a>**Az adatforrás és feldolgozási**
### <a name="synthetic-data-source"></a>Szintetikus adatforrás
Ehhez a sablonhoz használt adatforrás egy asztali alkalmazás, amely akkor letölti és helyileg futtassa a sikeres telepítést jönnek létre. Töltse le és telepítse az alkalmazást a Tulajdonságok sávon havi előrejelzési adatokat szimulátor nevű megoldás sablon diagram első csomópontjának kijelölésekor vonatkozó utasításokat talál. Ez az alkalmazás-hírcsatornák a [Azure Event Hubs](#azure-event-hub) szolgáltatás, vagy ugyanazt az eseményeket, a megoldás folyamatot a többi használandó.

Az esemény generációs alkalmazást feltölti az Azure Event Hubs csak, amíg a számítógép végrehajtása történik.

### <a name="azure-event-hub"></a>Az Azure Event Hubs
A [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) szolgáltatása címzettje a fent leírt szintetikus adatforrás által megadott bemenetet.

## <a name="data-preparation-and-analysis"></a>**Adatok előkészítése és elemzése**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
A [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatásával közel valós idejű elemzés a bemeneti adatfolyam nyújtanak a [Azure Event Hubs](#azure-event-hub) szolgáltatás, és tegye közzé eredményei közül a [Power BI](https://powerbi.microsoft.com)irányítópult, valamint az összes nyers bejövő események archiválása a [Azure Storage](https://azure.microsoft.com/services/storage/) szolgáltatás később feldolgozásra, amelyet a [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás.

### <a name="hd-insights-custom-aggregation"></a>HD Insights egyéni összesítési
Az Azure HD Insight-szolgáltatás futtatásához használt [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (Azure Data Factory által összehangolva) parancsfájlok összesítések ad az Azure Stream Analytics szolgáltatással lettek archiválva nyers eseményeket.

### <a name="azure-machine-learning"></a>Azure Machine Learning
A [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) szolgáltatás használható-e (Azure Data Factory által összehangolva.) egy adott régióban, a bemenetek kapott megadott jövőbeli energiafogyasztásának előrejelzési ellenőrzéséhez.

## <a name="data-publishing"></a>**Adatok közzététele**
### <a name="azure-sql-database-service"></a>Az Azure SQL adatbázis-szolgáltatás
A [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) szolgáltatás fogja tárolni (kezeli az Azure Data Factory) az Azure Machine Learning szolgáltatás, amely a fognak használni az előrejelzés a [Power BI](https://powerbi.microsoft.com) irányítópult.

## <a name="data-consumption"></a>**Adatok felhasználásához**
### <a name="power-bi"></a>Power BI
A [Power BI](https://powerbi.microsoft.com) szolgáltatásával által biztosított összesítések tartalmazó irányítópult megjelenítése a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás, valamint az igény szerinti előrejelzési tárolt eredmény [Azure SQL Adatbázis](https://azure.microsoft.com/services/sql-database/) használatával keletkezett a [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) szolgáltatás. A Power BI-irányítópultot megoldás sablon létrehozásával kapcsolatos útmutatásért tekintse meg a következő szakaszban.

## <a name="how-to-bring-in-your-own-data"></a>**Útmutató saját adatok**
Ez a szakasz ismerteti, hogyan kell a saját adatait az Azure-ba, és milyen területeken igényel módosításokat az adatok akkor vonja ebbe az architektúrába.

Nem valószínű, hogy a dataset kapcsolása megfelelő megoldás sablon használatos a dataset. Az adatok és a követelmények ismertetése a sablon a saját adatok módosítása a kulcsfontosságú lesz. Ha ez az első kapta az Azure Machine Learning szolgáltatáshoz, egy bemutatása példa használatával kaphat [az első kísérlet létrehozása](machine-learning/machine-learning-create-experiment.md).

Az alábbi szakaszok ismertetik a sablonból, amely a módosítások van szükség, amikor új adatkészlet megjelent szakaszok.

### <a name="azure-event-hub"></a>Az Azure Event Hubs
A [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) szolgáltatás nagyon általános, úgy, hogy az adatok is közzé lehet tenni a központi vagy a fürt megosztott kötetei szolgáltatás, vagy a JSON formátumban. Az Azure Event Hubs különleges feldolgozás nem történik, de fontos tisztában bele táplált adatok.

Ez a dokumentum nem ismerteti az adatok, de egy egyszerűen küldhet eseményeket vagy adatokat az Azure Event Hubs használatával a [Event Hub API](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
A [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás használható-adatfolyamok olvasása és írása források tetszőleges számú adatokat közel valós idejű elemzési nyújtanak.

Az igény szerinti előrejelzési energia megoldás sablon az Azure Stream Analytics-lekérdezés két segédlekérdezéseket, minden egyes fel az eseményeket az Azure Event Hubs szolgáltatásból bemeneteként és kimenetek kellene két különböző helyen áll. Ezek kimenetek áll egy Power BI adathalmazt és egy Azure tárolási helyét.

A [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) lekérdezés találhatók:

* Bejelentkezés a [Azure felügyeleti portálon](https://manage.windowsazure.com/)
* A stream analytics-feladatok keresése ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) , hogy a megoldás telepítésekor jött létre. Egyik tárházat adatokat a blob-tároló (pl. mytest1streaming432822asablob), a másik egy az adatok küldése a Power bi-ba (pl. mytest1streaming432822asapbi).
* Kiválasztása

  * ***BEMENETEK*** adjon meg a lekérdezés megtekintése
  * ***LEKÉRDEZÉS*** magát a lekérdezést megtekintése
  * ***KIMENETI*** a különböző kimenetének megtekintése

Azure Stream Analytics lekérdezési konstrukció kapcsolatos információk találhatók a [Stream Analytics lekérdezési hivatkozás](https://msdn.microsoft.com/library/azure/dn834998.aspx) az MSDN Webhelyén.

Ebben a megoldásban ez a megoldás sablon részeként a Azure Stream Analytics-feladat, amelyek közel valós idejű elemzési információ a Power BI-irányítópulthoz a bejövő adatfolyam adatkészlet kimenete valósul meg. Mivel a bejövő adatformátum implicit ismerete, ezeket a lekérdezéseket kell módosítani a adatformátum alapján.

Az egyéb Azure Stream Analytics-feladat kimenetében összes [Eseményközpont](https://azure.microsoft.com/services/event-hubs/) események [Azure Storage](https://azure.microsoft.com/services/storage/) és ezért van szükség az adatformátum függetlenül nincs változás a teljes eseményadatok Storage továbbítja adatfolyamként.

### <a name="azure-data-factory"></a>Azure Data Factory
A [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás koordinálja a mozgás és az adatok feldolgozása. Az igény szerinti előrejelzés energia megoldás sablon az adat-előállítóban áll tizenkettő [folyamatok](data-factory/data-factory-create-pipelines.md) , helyezze át, és feldolgozza a különféle technológiái.

  A data factory a Data Factory-csomópont a megoldás sablon diagram a megoldás üzembe helyezése a létrehozott alján megnyitásával érheti el. Ekkor megjelenik az adat-előállító az Azure felügyeleti portálon. Ha hibaüzenet jelenik meg a adatkészletek alatt, figyelmen kívül hagyhatja azokat, mivel ezek a adatgenerátor indítása előtt telepített adat-előállító miatt. Ezek a hibák nem akadályozzák a data factory működését.

Ez a szakasz ismerteti a szükséges [folyamatok](data-factory/data-factory-create-pipelines.md) és [tevékenységek](data-factory/data-factory-create-pipelines.md) szerepel a [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Az alábbiakban van a megoldás a diagram nézetben.

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Az öt, az adat-előállító adatcsatornák tartalmazhat [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok használt particionálása és összesíti az adatokat. Az áttelepítés előtt feljegyzett, ha a parancsfájlok találhatók a [Azure Storage](https://azure.microsoft.com/services/storage/) a telepítés során létrehozott fiók. A helyen lesz: demandforecasting\\\\parancsfájl\\\\hive\\ \\ (vagy https://[Your megoldás name].blob.core.windows.net/demandforecasting).

Hasonló a [Azure Stream Analytics](#azure-stream-analytics-1) lekérdezéseket, a [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok bejövő adatformátum implicit ismerete, ezeket a lekérdezéseket volna kell módosítani a Adatformátum és alapján[jellemzőkiemelés](machine-learning/machine-learning-feature-selection-and-engineering.md) követelményeinek.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*
Ez [csővezeték](data-factory/data-factory-create-pipelines.md) folyamat tartalmazza egy adott tevékenység - egy [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , amelyen fut a [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx)parancsfájl a 10 másodpercenként összesítésére óránkénti régió szintre állomás szinten igény szerinti adatok továbbítva, és be [Azure Storage](https://azure.microsoft.com/services/storage/) keresztül az Azure Stream Analytics-feladat.

A [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) particionálási feladat érték parancsfájl ***AggregateDemandRegion1Hr.hql***

#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*
Ez [csővezeték](data-factory/data-factory-create-pipelines.md) két tevékenységet tartalmaz:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , amely egy parancsprogramot futtathat Hive összesítéséhez az óránkénti igény szerinti előzményadatok állomás szinten óránkénti régió szintre, és az Azure Stream során az Azure Storage put Analytics-feladat
* [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, amely helyezi át az összesített adatokat az Azure Storage-blobból az Azure SQL Database, a megoldás sablon telepítésének részeként lett kiépítve.

A [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl esetében ez a feladat ***AggregateDemandHistoryRegion.hql***.

#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*
Ezek [folyamatok](data-factory/data-factory-create-pipelines.md) több tevékenységet tartalmaz, és amelynek végeredménynek az Azure Machine Learning kísérlet megoldás sablonhoz társított pontozott előrejelzéseket. Szinte teljesen azonosak lesznek kivételével azok csak kezeli a más régióban, amely minden egyes régió az ADF feldolgozási sorban lévő és a hive-parancsfájl átadott különböző RegionID történik.  
Ez szereplő tevékenységeket a következők:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , amely egy parancsprogramot futtathat Hive összesítéseket, és az Azure Machine Learning kísérlet szükséges jellemzőkiemelés. A Hive parancsfájlok a feladat olyan megfelelő ***PrepareMLInputRegionX.hql***.
* [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) helyezi át az eredményeket a tevékenység a [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység egy egyetlen Azure Storage-blobba, amelyek hozzáférését lehetnek a [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység, amely behívja az Azure Machine Learning kísérletezhet ennek eredményeképpen az eredményeket, mivel ez egy put egyetlen Azure Storage-blobba.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Ezek [folyamatok](data-factory/data-factory-create-pipelines.md) tartalmaz egy adott tevékenység - egy [másolási](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, mely az az Azure Machine Learning kísérlet eredményeit a megfelelő ***MLScoringRegionXPipeline*** az Azure SQL Database, a megoldás sablon telepítésének részeként lett kiépítve.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Ez [folyamatok](data-factory/data-factory-create-pipelines.md) egy adott tevékenység - tartalmaz egy [másolási](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység összesített folyamatos igény szerinti adatait helyezi át ***LoadHistoryDemandDataPipeline*** az Azure SQL Adatbázis, amely a megoldás sablon telepítésének részeként lett kiépítve.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Ezek [folyamatok](data-factory/data-factory-create-pipelines.md) tartalmaz egy adott tevékenység - egy [másolási](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, amely helyezi át a régió/állomás/Topologygeo a referenciaadatok, hogy a megoldás sablon részeként Azure Storage-blob feltöltése a telepítés az Azure SQL Database, a megoldás sablon telepítésének részeként lett kiépítve.

### <a name="azure-machine-learning"></a>Azure Machine Learning
A [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) használt kísérletezhet, ez a megoldás sablon szolgáltatása igény szerinti régió előrejelzését. A kísérlet felhasznált adathalmaz jellemző, és ezért módosítása vagy cseréje állapotba kerül, az adatok adott szükséges.

## <a name="monitor-progress"></a>**A figyelő folyamatban**
A Adatgenerátor eszköztárat, a feldolgozási folyamat veszi át a hidratált beolvasása és a megoldás a különböző összetevők indítsa el a következő a parancsok a Data Factory által kibocsátott kicking. Két módon figyelheti a folyamatot.

1. Ellenőrizze az adatok Azure Blob Storage-ból.

    A blob storage a nyers bejövő adatokat ír egy Stream Analytics-feladat. Ha rákattint az **Azure Blob Storage** a megoldás telepítése sikeres volt a megoldás, és kattintson a képernyő a komponens **nyitott** a jobb oldali panelen fog tartani, hogy a [Azure kezelési portál](https://portal.azure.com). Ezután kattintson a **Blobok**. A következő panelen látni fogja a tárolók listája. Kattintson a **"energysadata"**. A következő panelen megjelenik a **"demandongoing"** mappát. A rawdata mappában található látni fogja a mappák, valamint a neveket például a dátum = 2016-01-28 stb. Ha ezeket a mappákat, azt jelzi, hogy a nyers adatok sikeresen jön létre a számítógépen és a blob storage-ban tárolt. Meg kell jelennie a fájlokat, amelyben véges mérete (MB) az érintett mappákat.
2. Ellenőrizze az Azure SQL-adatbázis adatait.

    A folyamat utolsó lépését az adatokat (pl. a gépi tanulás által előrejelzéseket) az SQL-adatbázisba írni. Lehetséges, hogy várnia legfeljebb 2 órával az adatok jelennek meg az SQL-adatbázis. Figyelje az elérhető az SQL-adatbázis mennyi adatot egyike a keresztül [Azure felügyeleti portálján](https://manage.windowsazure.com/). A bal oldali panelen keresse meg az SQL-ADATBÁZISOK![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) , és kattintson rá. Ezután keresse meg az adatbázis (azaz demo123456db), és kattintson rá. A következő oldalon **"Az adatbázishoz való csatlakozás"** kattintson **"Futtatás Transact-SQL lekérdezések írásában, az SQL-adatbázis"**.

    Ide, ha rákattint az új lekérdezés és a lekérdezés megadása a sorok (pl. "select count(*) a DemandRealHourly) száma" az adatbázis növekedésével a tábla sorainak száma növelje.)
3. Ellenőrizze az adatokat a Power BI-irányítópultot.

    Power BI-irányítópultot kiemelt elérési állíthat be a nyers bejövő adatok figyelésére. Hajtsa végre az utasítást a "Power BI-irányítópulton" szakaszban.

## <a name="power-bi-dashboard"></a>**A Power BI-irányítópulton**
### <a name="overview"></a>Áttekintés
Ez a szakasz ismerteti, hogyan állíthat be az Azure stream Analytics (gyakran használt adatok elérési utat) a valós idejű adatok megjelenítése, valamint az Azure gépi tanulási a (cold elérési út) származó eredmények előrejelzése Power BI-irányítópultot.

### <a name="setup-hot-path-dashboard"></a>A telepítő gyakran használt adatok elérési útja irányítópult
Az alábbi lépéseket ismerteti hogyan jelenítheti meg a megoldás üzembe helyezése során létrehozott Stream Analytics-feladatok kimenete valós idejű adatokat. A [Power BI online](http://www.powerbi.com/) fiók szükséges a következő lépésekkel. Ha nincs fiókja, akkor [hozzon létre egyet](https://powerbi.microsoft.com/pricing).

1. Adja hozzá a Power BI-kimenet Azure Stream Analytics (ASA).

   * Kövesse az utasításokat kell [Azure Stream Analytics & Power BI: valós idejű látható-e a streamelési adatok a valós idejű elemzési irányítópult](stream-analytics/stream-analytics-power-bi-dashboard.md) , a Power BI az Azure Stream Analytics-feladat eredményének beállítása Irányítópult.
   * Keresse meg a stream analytics-feladat a [Azure felügyeleti portálján](https://manage.windowsazure.com). A feladat a névnek kell lennie: YourSolutionName + "streamingjob" + véletlenszerű szám + "asapbi" (azaz demostreamingjob123456asapbi).
   * Adjon hozzá egy Power bi-kimenet ASA feladat. Állítsa be a **kimeneti Alias** , **"PBIoutput"**. Állítsa be a **Adatkészletnevet** és **tábla neve** , **"EnergyStreamData"**. A kimeneti hozzáadását követően kattintson **"Start"** a Stream Analytics-feladat indítása az oldal alján. Egy megerősítő üzenetet kapja meg (*pl.*, "indítása stream analytics-feladat sikeres myteststreamingjob12345asablob").
2. Jelentkezzen be [online Power bi-ban](http://www.powerbi.com)

   * A bal oldali panelen adatkészletek szakasz a saját munkaterületen a bal oldali panelen a Power bi ábrázoló új adatkészlet látni kell lennie. Ez az az előző lépésben az Azure Stream Analytics leküldött streamadatok.
   * Győződjön meg arról, hogy a ***képi megjelenítések*** ablak meg nyitva, és a képernyő jobb oldalán látható.
3. Az "Igény szerint időbélyeg" csempe létrehozása:

   * Kattintson a dataset **"EnergyStreamData"** adatkészletek szakasz bal oldali panelen.
   * Kattintson a **"Vonaldiagram"** ikon ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * Kattintson a "EnergyStreamData" **mezők** panel.
   * Kattintson a **"Időbélyeg"** , és győződjön meg arról, hogy a "Tengely" mutatja. Kattintson a **"Load"** , és győződjön meg arról, hogy a "Értékek" mutatja.
   * Kattintson a **mentése** az első és a "EnergyStreamDataReport" a jelentés neve. A Navigátor ablak bal oldali jelentések szakaszának "EnergyStreamDataReport" nevű jelentés jelenik meg.
   * Kattintson a **"PIN-kód Visual"** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) jobb felső sarokban található a vonaldiagram ikonjára, "PIN-kód irányítópult" ablak lehet, hogy jelenik meg, hogy válasszon egy irányítópultot. Kérjük, válassza ki a "EnergyStreamDataReport", majd kattintson a "Rögzítés".
   * Ez a csempe az irányítópulton keresztül kattintson "Szerkesztés" ikonra jobb felső sarokban található az "Igény szerint időbélyeg" címet módosíthatja az egérrel
4. Hozzon létre másik irányítópult-csempéihez megfelelő adatkészletek alapján. A végső irányítópult-nézet alább láthatók.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>A telepítő Cold elérési irányítópult
Cold elérési adatok feldolgozási az alapvető célja az igény szerinti előrejelzés minden egyes régió beolvasása. A Power BI csatlakozik Azure SQL-adatbázis adatforrásként, az előrejelzési eredmények tárolásához.

> [!NOTE]
> 1) Az irányítópult elég előrejelzési eredmények gyűjtéséhez órát vesz igénybe. Azt javasoljuk, hogy a folyamat 2-3 órával azt követően, akkor a adatgenerátor delek indításához. 2.) Ez a lépés az előfeltétel, hogy töltse le és telepítse az ingyenes szoftvert [Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. Az adatbázis hitelesítő adatainak lekéréséhez.

   Szüksége lesz **adatbázis-kiszolgáló nevét, az adatbázisnév, felhasználónevet és jelszót** előtt áthelyezése a következő lépéseket. Az alábbiakban a lépéseit ismerteti azok megkereséséről.

   * Egyszer **"Azure SQL adatbázis"** a megoldás sablonban diagram zöldre, kattintson rá, és kattintson a **"Megnyitás"**. A varázsló az Azure felügyeleti portálra, és az adatbázis információi lap, valamint meg fog.
   * Az oldalon található "Adatbázis" szakaszt. A létrehozott adatbázis kimenő sorolja fel. Az adatbázis neve legyen **"A megoldás neve véletlenszerű szám +"adatbázis""** (pl. "mytest12345db").
   * Az adatbázis, kattintson a panel ki új pop, megtalálhatja az adatbázis-kiszolgáló nevét a felső. Az adatbázis-kiszolgáló nevét kell **"A megoldás neve véletlenszerű szám +"database.windows.net,1433""** (pl. "mytest12345.database.windows.net,1433").
   * Az adatbázis **felhasználónév** és **jelszó** ugyanaz, mint a felhasználónév és jelszó korábban tárolja, a megoldás üzembe helyezése során.
2. Az adatforrás a cold elérési útja a Power BI-fájl frissítése

   * Győződjön meg arról, hogy a legújabb verziójának telepítése [Power BI desktop](https://powerbi.microsoft.com/desktop).
   * Az a **"DemandForecastingDataGeneratorv1.0"** letöltött mappát, kattintson duplán a **"Power BI Template\DemandForecastPowerBI.pbix"** fájlt. A kezdeti képi megjelenítések dummy adatokon alapuló. **Megjegyzés:** Ha massage, ellenőrizze, hogy a Power BI Desktop legújabb verziójának telepítése hibaüzenet jelenik meg.

     Ha megnyitja, a fájl felső részén található, kattintson a **szerkesztése lekérdezések**. A pop kimenő ablak, kattintson duplán a **"Forrás"** a jobb oldali panelen.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * Cserélje le a pop ki ablakot, **"Server"** és **"Adatbázis"** a saját kiszolgáló és az adatbázis nevének, és kattintson a **"OK"**. A kiszolgálónév, ellenőrizze, hogy az 1433-as port megadott (**YourSolutionName.database.windows.net, 1433**). Figyelmen kívül hagyja a képernyőn megjelenő figyelmeztető üzeneteket.
   * A következő pop ki ablakot, két lehetőség közül választhat a bal oldali ablaktáblán látható (**Windows** és **adatbázis**). Kattintson a **"Adatbázis"**, töltse ki a **"Felhasználónév"** és **"Password"** (azt a felhasználónevet és jelszót, amikor először a megoldás telepített és létrehozott egy Azure SQL-adatbázis). A ***válassza ki, melyik szintre legyenek érvényesek a beállítások***, ellenőrizze az adatbázis-szintű beállítás. Kattintson a **"Csatlakozás"**.
   * Amennyiben most interaktív, az előző oldalra, az ablak bezárásához. Egy üzenet jelenik out - kattintson **alkalmaz**. Végül kattintson a **mentése** gombra a módosítások mentéséhez. A Power BI-fájl most hozott létre kapcsolatot a kiszolgálóval. Üres a képi megjelenítés esetén győződjön meg arról, hogy törli a kijelölt elemek összes adatok megjelenítéséhez a jelmagyarázatokból jobb felső sarkában a radír ikonjára kattint a képi megjelenítések. A frissítés gombra segítségével új adatok vizuális tükrözi. Kezdetben csak akkor jelenik meg a adatait a képi megjelenítések a, az adat-előállítóban ütemezett frissítése 3 óránként. 3 óra elteltével jelenik meg új előrejelzéseket megjelennek a képi megjelenítések, amikor az adatok frissítése.
3. (Választható) A cold elérési irányítópultot közzététele [Power BI online](http://www.powerbi.com/). Vegye figyelembe, hogy ez a lépés a Power BI fiókot (vagy Office 365-fiókkal) kell-e.

   * Kattintson a **"Publish"** és néhány másodperc múlva megjelenik egy ablak megjelenítése a "Power BI sikeres közzététel!" a zöld pipa jelzi. Kattintson az alábbi "Megnyitás demoprediction.pbix Power BI-ban" hivatkozásra. Részletes utasításokat talál [a Power BI Desktop közzététele](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Új irányítópult létrehozása: kattintson a  **+**  jelentkezzen mellett a **irányítópultok** szakaszt, a bal oldali ablaktáblán. Adja meg az új irányítópult "Igény szerinti előrejelzés bemutató" nevét.
   * Ha a jelentés megnyitásához kattintson ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) rögzítése az irányítópulton való rögzítéséhez képi megjelenítések. Részletes utasításokat talál [egy csempe rögzítése egy Power BI-irányítópult a jelentés](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Ugrás az irányítópult-oldalon, és méretének és a képi megjelenítések helyét és szerkessze a címben. Részletes utasításokat talál szerkesztése a csempéket a [Szerkesztés mozaik – átméretezési, move, nevezze át, PIN-kód, törlése, hivatkozás hozzáadása a](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Íme egy példa irányítópult néhány cold elérési megjelenítésekkel azt van rögzítve.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (Választható) Az adatforrás-frissítés ütemezése.

   * Az adatok frissítés ütemezéséhez az egérmutatóval rámutat a **EnergyBPI végleges** dataset, kattintson ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) majd **ütemezés frissítése**.
     **Megjegyzés:** Ha megjelenik egy figyelmeztetés masszírozó, kattintson a **hitelesítő adatok szerkesztése** , és győződjön meg arról, hogy az adatbázis hitelesítő adatai megegyeznek-e az 1. lépésben leírt.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Bontsa ki a **ütemezés frissítése** szakasz. Kapcsolja be "az adatok naprakészen tarthatók".
   * A frissítés ütemezése a igények alapján. További információkért lásd: [adatfrissítési a Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## <a name="how-to-delete-your-solution"></a>**A megoldás törlése**
Győződjön meg arról, hogy le a adatgenerátor használatakor nem aktív a megoldás, a adatgenerátor futtató tájékozódnia magasabb költségek. Ha nem használnak, törölje a megoldás. A megoldás törlése eltávolítja a megoldás üzembe helyezésekor az előfizetésben kiépített összetevőit. Törölje a megoldás kattintson a bal oldali panelen, a megoldás sablont, majd kattintson a Törlés a megoldás neve.

## <a name="cost-estimation-tools"></a>**Becslés eszközök költsége**
A következő két eszközök segítségével jobb megértése érdekében fut az igény szerinti előrejelzés energia Megoldássablon az előfizetésében szereplő részt a teljes költségek érhetők el:

* [A Microsoft Azure költség négyzetgyökének eszköz (online)](https://azure.microsoft.com/pricing/calculator/)
* [A Microsoft Azure költség négyzetgyökének eszköz (asztali verzió)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**A nyugtázás**
Ez a cikk adatok tudósok JI Chen és szoftver visszafejtés Qiu Min Microsoft hozta létre.
