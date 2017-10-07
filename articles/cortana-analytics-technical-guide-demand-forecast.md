---
title: "aaaDemand előrejelzés energia műszaki útmutató |} Microsoft Docs"
description: "A műszaki útmutatóban toohello megoldás sablont a Microsoft a Cortana Intelligence a havi előrejelzési igény."
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
ms.openlocfilehash: c97b7c19c9e3a317aecc329e61a0692d2f1ec53e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="technical-guide-toohello-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Műszaki útmutató toohello Cortana Intelligence Megoldássablon az igény szerinti energia előrejelzés
## <a name="overview"></a>**Áttekintés**
Megoldás sablonok célja tooaccelerate hello folyamatot nevezzük, egy E2E bemutató Cortana Intelligence Suite felett. Egy telepített sablon kiépítéséhez szükséges Cortana Intelligence összetevő előfizetés, és a hello közötti kapcsolatok létrehozása. Egy adatok szimuláció alkalmazás által létrehozott első mintaadatokkal hello adatok folyamat is magok. Hello hivatkozás hello adatok szimulátor letölthető és telepíthető a helyi számítógép tudnivalókat a toohello readme.txt fájlban további információk az hello szimulátor használatával. Hello szimulátor előállított adatokat fog hydrate hello adatok feldolgozási sorban lévő és a machine learning előrejelzés megjeleníthetők a hello Power BI-irányítópultot, amely generálása kezdő.

hello megoldássablonban található [Itt](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

hello telepítési folyamatának végigvezeti a megoldás hitelesítő adatok mentése több lépéseket tooset. Ellenőrizze, hogy a hitelesítő adatokat, például a megoldás neve, a felhasználónevet és jelszót hello telepítése során megadandó rögzítése.

hello a jelen dokumentum célja tooexplain hello referencia-architektúrában és a különböző összetevőket az előfizetésében megoldás sablon részeként. hello dokumentum is beszél hogyan tooreplace hello mintaadatok, a saját toobe képes toosee insights/előrejelzések eltéréseinek az Ön nyert adatok valós adatokkal. Emellett hello dokumentum megbeszélések hello kell módosítani, ha azt szeretné, toocustomize hello megoldás a saját adataival toobe megoldás sablon hello részeivel kapcsolatos. Hogyan toobuild hello Power BI-irányítópulton a Megoldássablon az utasításokat a hello végén.

## <a name="big-picture"></a>**Nagy vonalakban tekinti**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architektúra ismertetése
Hello megoldás telepítésekor Cortana Analytics Suite belül különböző Azure-szolgáltatások rendszer aktiválja azokat (*azaz* Az Event Hubs, a Stream Analytics, HDInsight, adat-előállító gépi tanulás, *stb*). hello architektúra a fenti ábrán, magas szinten, hogyan hello energia megoldás sablon előrejelzés igény szerinti végpont értékekből összeállított. Ezek a szolgáltatások, a rájuk kattintva hello megoldás sablon diagram hello megoldás üzembe helyezése hello létre tud tooinvestigate lesz. a következő szakaszok hello minden egyes adatra ismertetik.

## <a name="data-source-and-ingestion"></a>**Az adatforrás és feldolgozási**
### <a name="synthetic-data-source"></a>Szintetikus adatforrás
A sablon hello adatok használt forrás egy asztali alkalmazás, amely akkor letölti és helyileg futtassa a sikeres telepítést jönnek létre. Hello utasításokat toodownload belépve és az alkalmazás telepítése hello tulajdonságok sávon hello havi előrejelzési adatokat szimulátor hívása hello megoldás sablon diagram első csomópontjának kiválasztásakor. Ez az alkalmazás hírcsatornák hello [Azure Event Hubs](#azure-event-hub) szolgáltatás, vagy ugyanazt az események, hello megoldás folyamata hello többi használandó.

hello esemény generációs alkalmazást feltölti hello Azure Event Hubs csak, amíg a számítógép végrehajtása történik.

### <a name="azure-event-hub"></a>Az Azure Event Hubs
Hello [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) szolgáltatás hello címzett hello a fent leírt szintetikus adatforrás által biztosított hello bemeneti.

## <a name="data-preparation-and-analysis"></a>**Adatok előkészítése és elemzése**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás közel valós idejű elemzés hello bemeneti adatfolyam a hello használt tooprovide [Azure Event Hubs](#azure-event-hub) szolgáltatás, és tegye közzé eredményei közül a [Power BI](https://powerbi.microsoft.com) irányítópult, valamint minden nyers bejövő események toohello Archiválás [Azure Storage](https://azure.microsoft.com/services/storage/) későbbi feldolgozásra által hello szolgáltatás [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás.

### <a name="hd-insights-custom-aggregation"></a>HD Insights egyéni összesítési
hello Azure HD Insight szolgáltatás használt toorun [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (Azure Data Factory által összehangolva) parancsfájlok tooprovide összesítések hello nyers események volt archivált hello Azure Stream Analytics szolgáltatás használatával.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) szolgáltatás használható-e (Azure Data Factory által összehangolva.) egy adott régióban kapott hello bemenetek megadott jövőbeli energiafogyasztásának előrejelzése toomake.

## <a name="data-publishing"></a>**Adatok közzététele**
### <a name="azure-sql-database-service"></a>Az Azure SQL adatbázis-szolgáltatás
Hello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) szolgáltatása (kezeli az Azure Data Factory) használt toostore hello előrejelzéseket hello fognak használni a hello Azure Machine Learning szolgáltatás által fogadott [Power BI](https://powerbi.microsoft.com) irányítópult.

## <a name="data-consumption"></a>**Adatok felhasználásához**
### <a name="power-bi"></a>Power BI
Hello [Power BI](https://powerbi.microsoft.com) szolgáltatása használt tooshow egy irányítópultot, amely tartalmazza az összesítések hello által biztosított [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás, valamint az igény szerinti előrejelzési tárolt eredmény [Azure SQL Adatbázis](https://azure.microsoft.com/services/sql-database/) hello segítségével keletkezett [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) szolgáltatás. Hogyan toobuild hello megoldás sablon Power BI-irányítópultot vonatkozó utasításokért tekintse meg a toohello szakaszban.

## <a name="how-toobring-in-your-own-data"></a>**Hogyan toobring a saját adatok**
Ez a szakasz ismerteti, hogyan toobring területek igényelnének, és a saját adatok tooAzure végrehajtott módosítások hello adatok ebbe az architektúrába állapotba.

Nem valószínű, hogy a dataset kapcsolása megfelelő megoldás sablon használt hello adatkészlet. Az adatok és a követelmények ismertetése a sablon toowork módosítása a saját adataival a kulcsfontosságú lesz. Ha ez az első kitettség toohello Azure Machine Learning szolgáltatáshoz, egy bevezető tooit hello példát használatával kaphat [hogyan toocreate az első kísérlet](machine-learning/machine-learning-create-experiment.md).

a következő szakaszok hello hello sablonból, amely a módosítások van szükség, amikor új adatkészlet megjelent hello szakaszok ismertetik.

### <a name="azure-event-hub"></a>Az Azure Event Hubs
Hello [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) szolgáltatás nagyon általános, úgy, hogy az adatok is közzé lehet tenni toohello hub vagy a fürt megosztott kötetei szolgáltatás, vagy a JSON formátumban. Hello Azure Event Hubs különleges feldolgozás nem történik, de fontos tisztában bele táplált hello adatokat.

A dokumentum nem ismerteti, hogyan tooingest az adatokat, de egy használatával egyszerűen küldhet eseményeket vagy adatokat tooan Azure Event Hubs, hello segítségével [Event Hub API](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatása közel valós idejű elemzési használt tooprovide adatfolyamok olvasása és írása adatok tooany több forráson.

Az igény szerinti előrejelzés energia megoldás sablon hello hello Azure Stream Analytics-lekérdezés két segédlekérdezéseket, minden egyes bemeneteként hello Azure Event Hubs szolgáltatás származó események fel és kimenetek tootwo különböző helyek rendelkező áll. Ezek kimenetek áll egy Power BI adathalmazt és egy Azure tárolási helyét.

Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) lekérdezés találhatók:

* Hello való bejelentkezés [Azure felügyeleti portálon](https://manage.windowsazure.com/)
* Hello stream analytics-feladatok keresése ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) , amely hello megoldás telepítésekor jött létre. Egy tárházat adattárolás tooblob (pl. mytest1streaming432822asablob) és egyéb adatok tooPower BI (pl. mytest1streaming432822asapbi) tárházat még hello.
* Kiválasztása

  * ***BEMENETEK*** tooview hello lekérdezés bemeneti
  * ***LEKÉRDEZÉS*** maga tooview hello lekérdezés
  * ***KIMENETI*** tooview hello különböző kimenetek

Azure Stream Analytics lekérdezési konstrukció kapcsolatos információk találhatók hello [Stream Analytics lekérdezési hivatkozás](https://msdn.microsoft.com/library/azure/dn834998.aspx) az MSDN Webhelyén.

Ebben a megoldásban hello Azure Stream Analytics-feladat, amelyek közel valós idejű elemzési információkat hello bejövő adatok adatfolyam tooa Power BI irányítópulttal kapcsolatos adatkészlet megoldás sablon részeként biztosított kimenete. Mivel a hello bejövő adatformátum implicit ismerete, ezeket a lekérdezéseket toobe módosítani kell az adatok formátum alapján.

hello más Azure Stream Analytics-feladat kimenetében összes [Eseményközpont](https://azure.microsoft.com/services/event-hubs/) események [Azure Storage](https://azure.microsoft.com/services/storage/) és ezért van szükség az adatformátum függetlenül nincs változás hello teljes eseményadatok továbbítja adatfolyamként toostorage.

### <a name="azure-data-factory"></a>Azure Data Factory
Hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás koordinálja a hello mozgás és az adatok feldolgozása. Az energia Megoldássablonban hello adatok igény szerinti előrejelzés hello gyári áll tizenkettő [folyamatok](data-factory/data-factory-create-pipelines.md) , helyezze át, és a különféle technológiái hello adatok feldolgozása.

  A data factory hello adat-előállító csomópont hello megoldás sablon diagram hello megoldás üzembe helyezése hello létre hello alján megnyitásával érheti el. Ez eltarthat, toohello adat-előállítót az Azure felügyeleti portálon. Ha hibaüzenet jelenik meg a adatkészletek alatt, figyelmen kívül hagyhatja azokat, mivel ezek hello adatgenerátor indítása előtt telepített toodata gyári miatt. Ezek a hibák nem akadályozzák a data factory működését.

Ez a szakasz ismerteti, amelyek hello szükséges [folyamatok](data-factory/data-factory-create-pipelines.md) és [tevékenységek](data-factory/data-factory-create-pipelines.md) hello szereplő [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Alább az hello diagram nézet hello megoldás.

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Az öt hello folyamatok, az adat-előállító tartalmazhat [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) használt toopartition és összesített hello adatok parancsfájlokat. Ha nincs jelezve, hello parancsfájlok található hello [Azure Storage](https://azure.microsoft.com/services/storage/) a telepítés során létrehozott fiók. A helyen lesz: demandforecasting\\\\parancsfájl\\\\hive\\ \\ (vagy https://[Your megoldás name].blob.core.windows.net/demandforecasting).

Hasonló toohello [Azure Stream Analytics](#azure-stream-analytics-1) lekérdezéseket, a [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok rendelkezik hello bejövő adatformátum implicit ismerete, ezeket a lekérdezéseket toobe módosítani kell az adatformátum és alapján[jellemzőkiemelés](machine-learning/machine-learning-feature-selection-and-engineering.md) követelményeinek.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*
Ez [csővezeték](data-factory/data-factory-create-pipelines.md) folyamat tartalmazza egy adott tevékenység - egy [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , amelyen fut a [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx)parancsfájl tooaggregate hello 10 másodpercenként állomás szintű toohourly régió szinten igény szerinti adatok továbbítva, és be [Azure Storage](https://azure.microsoft.com/services/storage/) keresztül hello Azure Stream Analytics-feladat.

A [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) particionálási feladat érték parancsfájl ***AggregateDemandRegion1Hr.hql***

#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*
Ez [csővezeték](data-factory/data-factory-create-pipelines.md) két tevékenységet tartalmaz:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) hogy fut a Hive parancsfájl tooaggregate hello óránkénti igény szerinti előzményadatok állomás szintű toohourly régió szinten és az Azure Storage hello Azure során A Stream Analytics-feladat
* [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) hello áthelyezi tevékenység összesített adatait alkotják az Azure Storage-blob toohello Azure SQL-adatbázis, amely hello megoldás sablon telepítésének részeként lett kiépítve.

Hello [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl esetében ez a feladat ***AggregateDemandHistoryRegion.hql***.

#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*
Ezek [folyamatok](data-factory/data-factory-create-pipelines.md) több tevékenységeket tartalmaznak, és amelynek végeredménynek hello program pontozza a mennyiségeket hello Azure Machine Learning kísérlet megoldás sablonhoz társított által. Szinte teljesen azonosak lesznek kivételével azok csak kezeli hello más régióban, amely minden egyes régió hello ADF pipeline és hello hive parancsfájl átadott különböző RegionID történik.  
Ez található hello tevékenységeket a következők:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , hogy fut a Hive parancsfájl tooperform összesítések, és jellemzőkiemelés hello Azure Machine Learning kísérlet szükséges. hello Hive parancsfájlok a feladat olyan megfelelő ***PrepareMLInputRegionX.hql***.
* [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, mely az hello eredmények hello [HDInsightHive](data-factory/data-factory-hive-activity.md) tevékenység tooa egyetlen Azure Storage-blobot, amely hello hozzáférését [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység, amely behívja hello Azure Machine Learning kísérlet, amely hello eredményez okoz, mivel ez egy put egyetlen Azure Storage-blobba.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Ezek [folyamatok](data-factory/data-factory-create-pipelines.md) tartalmaz egy adott tevékenység - egy [másolási](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, mely az hello Azure Machine Learning kísérlet eredményeit hello megfelelő hello ***MLScoringRegionXPipeline *** toohello Azure SQL-adatbázis, amely hello megoldás sablon telepítésének részeként lett kiépítve.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Ez [folyamatok](data-factory/data-factory-create-pipelines.md) tartalmaz egy adott tevékenység - egy [másolási](https://msdn.microsoft.com/library/azure/dn835035.aspx) hello áthelyezése tevékenység összesített folyamatos igény szerinti adatok ***LoadHistoryDemandDataPipeline*** toohello Azure SQL-adatbázis, amely hello megoldás sablon telepítésének részeként lett kiépítve.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Ezek [folyamatok](data-factory/data-factory-create-pipelines.md) egy adott tevékenység - tartalmaz egy [másolása](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, mely az hello referenciaadatok a régió/állomás/Topologygeo, amelyek tooAzure tárolási blob feltöltése hello megoldás részeként sablon telepítési toohello Azure SQL-adatbázis, amely hello megoldás sablon telepítésének részeként lett kiépítve.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) használt kísérletezhet, ez a megoldás sablon szolgáltatása igény szerinti régió hello előrejelzését. hello kísérlet felhasznált adathalmaz konkrét toohello, és ezért szükséges állapotba kerül, a módosítás vagy cseréje adott toohello adatokat.

## <a name="monitor-progress"></a>**A figyelő folyamatban**
Adatgenerátor hello eszköztárat, hello csővezeték kezdődik hidratált tooget, és hello különböző összetevői a megoldás indítsa el a következő hello parancsok hello adat-előállító által kibocsátott művelet kicking. Két módon figyelheti hello folyamat.

1. Az Azure Blob Storage hello adatok ellenőrzése.

    Hello nyers bejövő tooblob adattárolás ír egy hello Stream Analytics-feladat. Ha rákattint az **Azure Blob Storage** a megoldásokban hello a képernyőn, sikeresen telepített hello megoldás, és kattintson a **nyitott** hello jobb oldali panelen, akkor léphet vissza toohello [Azure felügyeleti portálján](https://portal.azure.com). Ezután kattintson a **Blobok**. A következő panelen hello látni fogja a tárolók listája. Kattintson a **"energysadata"**. A következő panelen hello látni fogja a hello **"demandongoing"** mappát. Hello rawdata mappába, látni fogja a mappák, valamint a neveket például a dátum = 2016-01-28 stb. Ha ezeket a mappákat, azt jelzi, hogy hello nyers adatok sikeresen jön létre a számítógépen és a blob storage-ban tárolt. Meg kell jelennie a fájlokat, amelyben véges mérete (MB) az érintett mappákat.
2. Ellenőrizze az Azure SQL adatbázis hello adatait.

    hello hello folyamat utolsó lépését az toowrite adatai (pl. a gépi tanulás által előrejelzéseket) az SQL-adatbázisba. Lehetséges, hogy toowait legfeljebb 2 óra hello adatok tooappear az SQL-adatbázisban. Egyirányú toomonitor keresztül van mennyi adatot érhető el az SQL-adatbázis [Azure felügyeleti portálján](https://manage.windowsazure.com/). Hello bal oldali panelen keresse meg az SQL-ADATBÁZISOK![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) , és kattintson rá. Ezután keresse meg az adatbázis (azaz demo123456db), és kattintson rá. A következő oldalon hello alatt **"Connect tooyour adatbázis"** kattintson **"Futtatás Transact-SQL lekérdezések írásában, az SQL-adatbázis"**.

    Itt, ha rákattint az új lekérdezés és a lekérdezés megadása a sorok (pl. "select count(*) a DemandRealHourly) hello száma" az adatbázis növekedésével hello tábla sorainak száma hello növelje.)
3. Ellenőrizze a Power BI-irányítópultot hello adatait.

    A Power BI gyakran használt adatok elérési útja irányítópult toomonitor hello bejövő nyersadatok állíthat be. Kérjük, kövesse a "Power BI-irányítópulton" szakasz hello hello utasítás.

## <a name="power-bi-dashboard"></a>**A Power BI-irányítópulton**
### <a name="overview"></a>Áttekintés
Ez a szakasz ismerteti, hogyan tooset fel Power BI irányítópult toovisualize a valós idejű az Azure-ból adatfolyam analytics (gyakran használt adatok elérési útja), mint jól előrejelzési eredményeinek Azure machine learning (cold elérési út).

### <a name="setup-hot-path-dashboard"></a>A telepítő gyakran használt adatok elérési útja irányítópult
a lépéseket követve hello ismerteti hogyan toovisualize valós idejű adatok kimenetét a Stream Analytics hello megoldás üzembe helyezése során létrehozott feladatok. A [Power BI online](http://www.powerbi.com/) fiók szükség tooperform hello a következő lépéseket. Ha nincs fiókja, akkor [hozzon létre egyet](https://powerbi.microsoft.com/pricing).

1. Adja hozzá a Power BI-kimenet Azure Stream Analytics (ASA).

   * Toofollow hello utasításait kell [Azure Stream Analytics & Power BI: valós idejű látható-e a streamelési adatok a valós idejű elemzési irányítópult](stream-analytics/stream-analytics-power-bi-dashboard.md) tooset mentése az Azure Stream Analytics-feladat, mint a Power hello kimenete Üzleti INTELLIGENCIA irányítópult.
   * Hello stream analytics-feladat a keresse meg a [Azure felügyeleti portálján](https://manage.windowsazure.com). hello feladat hello névnek kell lennie: YourSolutionName + "streamingjob" + véletlenszerű szám + "asapbi" (azaz demostreamingjob123456asapbi).
   * Adjon hozzá egy Power bi-kimenet hello ASA feladat. Set hello **kimeneti Alias** , **"PBIoutput"**. Állítsa be a **Adatkészletnevet** és **tábla neve** , **"EnergyStreamData"**. Hello kimeneti hozzáadását követően kattintson **"Start"** hello lap toostart hello Stream Analytics-feladat hello alján. Egy megerősítő üzenetet kapja meg (*pl.*, "indítása stream analytics-feladat sikeres myteststreamingjob12345asablob").
2. Jelentkezzen be túl[online Power bi-ban](http://www.powerbi.com)

   * A bal oldali panelen adatkészletek szakasz a saját munkaterületen hello meg kell tudni toosee egy új adatkészlet ábrázoló hello a bal oldali panelen a Power bi. Ez az előző lépésben hello Azure Stream Analytics leküldött adatfolyam hello.
   * Győződjön meg arról, hogy hello ***képi megjelenítések*** ablaktábla meg nyitva, és az üdvözlő képernyőt jobb oldalán látható.
3. "Igény szerint időbélyeg" csempe hello létrehozása:

   * Kattintson a dataset **"EnergyStreamData"** a bal oldali panelen adatkészletek szakasz hello.
   * Kattintson a **"Vonaldiagram"** ikon ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * Kattintson a "EnergyStreamData" **mezők** panel.
   * Kattintson a **"Időbélyeg"** , és győződjön meg arról, hogy a "Tengely" mutatja. Kattintson a **"Load"** , és győződjön meg arról, hogy a "Értékek" mutatja.
   * Kattintson a **mentése** felső hello és "EnergyStreamDataReport" hello jelentés neve. "EnergyStreamDataReport" nevű hello jelentés hello Navigator ablak bal oldali jelentések szakaszában jelenik meg.
   * Kattintson a **"PIN-kód Visual"** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) jobb felső sarokban található a vonaldiagram ikonjára, "PIN-kód tooDashboard" ablak lehet, hogy jelenik meg az Ön toochoose egy irányítópultot. Kérjük, válassza ki a "EnergyStreamDataReport", majd kattintson a "Rögzítés".
   * Ez a csempe hello irányítópulton keresztül kattintson "Szerkesztés" jobb felső sarokban található toochange ikonra a címet "Igény szerint időbélyeg" hello egérrel
4. Hozzon létre másik irányítópult-csempéihez megfelelő adatkészletek alapján. hello végső irányítópult-nézet alább láthatók.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>A telepítő Cold elérési irányítópult
Cold elérési adatok feldolgozási hello alapvető célja minden egyes régió előrejelzési tooget hello igény szerint. A Power BI tooan Azure SQL adatbázis adatforrásként, hello előrejelzés eredmények tároló csatlakozik.

> [!NOTE]
> 1) Igénybe vehet néhány óra toocollect hello irányítópult elég előrejelzési eredményeit. Azt javasoljuk, hogy a folyamat 2-3 órával azt követően, hogy delek hello adatgenerátor indításához. 2.) az ebben a lépésben hello előfeltétel, toodownload és a telepítés hello ingyenes szoftvert [Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. Hello adatbázis hitelesítő adatainak lekéréséhez.

   Szüksége lesz **adatbázis-kiszolgáló nevét, az adatbázisnév, felhasználónevet és jelszót** toonext lépéseket áthelyezése előtt. Az alábbiakban hello lépéseket tooguide, hogyan toofind őket.

   * Egyszer **"Azure SQL adatbázis"** a megoldás sablonban diagram zöldre, kattintson rá, és kattintson a **"Megnyitás"**. Az interaktív tooAzure felügyeleti portálján is, majd az adatbázis információi lap, valamint meg fog.
   * Hello oldalon található "Adatbázis" szakaszt. Kimenő létrehozott hello adatbázis sorolja fel. az adatbázis hello névnek kell lennie **"A megoldás neve véletlenszerű szám +"adatbázis""** (pl. "mytest12345db").
   * Kattintson az adatbázis, az új pop panel ki, az adatbázis-kiszolgáló nevét a talál hello felső hello. Az adatbázis-kiszolgáló nevét kell **"A megoldás neve véletlenszerű szám +"database.windows.net,1433""** (pl. "mytest12345.database.windows.net,1433").
   * Az adatbázis **felhasználónév** és **jelszó** hello megegyeznek a hello felhasználónév és jelszó korábban rögzített során hello megoldás üzembe helyezése.
2. Hello adatforrás hello cold elérési út Power BI-fájl frissítése.

   * Győződjön meg arról, hogy hello legújabb verziója van telepítve [Power BI desktop](https://powerbi.microsoft.com/desktop).
   * A hello **"DemandForecastingDataGeneratorv1.0"** letöltött mappát, kattintson duplán a hello **"Power BI Template\DemandForecastPowerBI.pbix"** fájlt. hello kezdeti képi megjelenítések dummy adatokon alapuló. **Megjegyzés:** Ha massage, ellenőrizze, hogy a Power BI Desktop hello legújabb verziójának telepítése hibaüzenet jelenik meg.

     Ha megnyitja, hello fájl hello felül, kattintson a **szerkesztése lekérdezések**. Hello előugró ablak meg, kattintson duplán a **"Forrás"** hello jobb oldali panelen.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * Cserélje le a hello előugró ablak ki, **"Server"** és **"Adatbázis"** a saját kiszolgáló és az adatbázis nevének, és kattintson a **"OK"**. A kiszolgálónév, győződjön meg arról, megadhatja a 1433-as port hello (**YourSolutionName.database.windows.net, 1433**). Hello képernyőn megjelenő hello figyelmeztető üzenetek figyelmen kívül.
   * Hello következő pop ki ablakot, két lehetőség hello bal oldali ablaktáblán látható (**Windows** és **adatbázis**). Kattintson a **"Adatbázis"**, töltse ki a **"Felhasználónév"** és **"Password"** (Ez az hello felhasználónevet és jelszót, amikor először telepített hello megoldás és létrehozott egy Az Azure SQL-adatbázis). A ***válassza ki, amelyek szinten tooapply ezeket a beállításokat***, ellenőrizze az adatbázis-szintű beállítás. Kattintson a **"Csatlakozás"**.
   * Ha Ön az interaktív hátsó toohello előző lapon, a hello ablak bezárásához. Egy üzenet jelenik out - kattintson **alkalmaz**. Végül kattintson a hello **mentése** toosave hello módosítások gombra. A Power BI-fájl most létesített kapcsolat toohello kiszolgáló. A képi megjelenítések üres esetén, feltétlenül törölje hello beállításokat a hello képi megjelenítések toovisualize összes hello adatok hello jobb felső sarkában hello jelmagyarázatok hello radír ikonjára kattint. Hello képi megjelenítések hello frissítési gomb tooreflect új adatok használatát. Kezdetben csak akkor jelenik meg hello adatait a a képi megjelenítések, adat-előállító hello ütemezett toorefresh 3 óránként. 3 óra elteltével jelenik meg új előrejelzéseket megjelennek a képi megjelenítések hello adatok frissítésekor.
3. (Választható) Hello cold elérési irányítópult túl közzététele[Power BI online](http://www.powerbi.com/). Vegye figyelembe, hogy ez a lépés a Power BI fiókot (vagy Office 365-fiókkal) kell-e.

   * Kattintson a **"Publish"** és néhány másodperc múlva megjelenik egy ablak megjelenítése "Közzétételi tooPower BI Success!" a zöld pipa jelzi. Kattintson alább a "Megnyitás demoprediction.pbix Power BI-ban" hello hivatkozásra. toofind részletes ismertetését lásd: [a Power BI Desktop közzététele](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * új irányítópult toocreate: hello kattintson  **+**  következő toothe jelentkezzen **irányítópultok** szakasz hello bal oldali ablaktáblán. Adja meg az új irányítópult "Igény szerinti előrejelzés bemutató" hello nevét.
   * Amikor hello jelentés megnyitásához kattintson a ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) toopin a képi megjelenítések tooyour irányítópult. toofind részletes ismertetését lásd: [rögzítheti egy mozaik tooa Power BI-irányítópultot jelentésből](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Nyissa meg toohello irányítópult-oldalon, és hello méret és elhelyezkedés a képi megjelenítést és szerkessze a címben. toofind részletes utasításokat hogyan tooedit a csempék: [Szerkesztés mozaik – átméretezési, move, nevezze át, PIN-kód, törlése, hivatkozás hozzáadása a](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Íme egy példa irányítópultot, amelynek egyes cold elérési képi megjelenítések rögzítve tooit.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (Választható) Hello adatforrás-frissítés ütemezése.

   * tooschedule frissítési hello adatok, az egérmutatóval rámutat hello **EnergyBPI végleges** dataset, kattintson a ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) majd **ütemezés frissítése**.
     **Megjegyzés:** Ha megjelenik egy figyelmeztetés masszírozó, kattintson a **hitelesítő adatok szerkesztése** , és győződjön meg arról, hogy az adatbázis hitelesítő adatai vannak hello ugyanaz, mint 1. lépésben leírt.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Bontsa ki a hello **ütemezés frissítése** szakasz. Kapcsolja be "az adatok naprakészen tarthatók".
   * Hello a frissítésütemezés igényei szerint. toofind további információkért lásd: [adatfrissítési a Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## <a name="how-toodelete-your-solution"></a>**Hogyan toodelete a megoldás**
Győződjön meg arról, hogy használatakor nem aktívan hello megoldás, magasabb költségek gyakorisága hello adatgenerátor fut le hello adatgenerátor. Ha nem használnak, törölje a hello megoldás. A megoldás törlése eltávolítja az előfizetésben kiépített hello megoldás üzembe helyezésekor hello összetevők. toodelete hello megoldás kattintson a bal oldali panelen hello hello megoldás sablon a megoldás neve, majd kattintson a törlés.

## <a name="cost-estimation-tools"></a>**Becslés eszközök költsége**
hello következő két eszközök állnak rendelkezésre toohelp megértésében igény szerinti előrejelzés hello fut energia Megoldássablon az előfizetésében szereplő részt a teljes költségek:

* [A Microsoft Azure költség négyzetgyökének eszköz (online)](https://azure.microsoft.com/pricing/calculator/)
* [A Microsoft Azure költség négyzetgyökének eszköz (asztali verzió)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**A nyugtázás**
Ez a cikk adatok tudósok JI Chen és szoftver visszafejtés Qiu Min Microsoft hozta létre.
