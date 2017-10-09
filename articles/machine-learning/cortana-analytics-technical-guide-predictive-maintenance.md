---
title: "az Azure - Cortana Intelligence megoldás műszaki útmutató a űrtechnikai aaaPredictive karbantartási |} Microsoft Docs"
description: "Egy technikai útmutató toohello megoldás sablont a Microsoft a Cortana Intelligence űrtechnikai, segédprogramok és szállítására prediktív karbantartás céljából."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2c4d2147-0f05-4705-8748-9527c2c1f033
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: 30ddc1c101007546ae1b303bccebae3ecdacb442
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="technical-guide-toohello-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Műszaki útmutató toohello Cortana Intelligence Megoldássablonban űrtechnikai és más vállalatok prediktív karbantartás

## <a name="important"></a>**Fontos**
Ez a cikk elavult. hello információk megírja, továbbra is megfelelő toohello probléma a űrtechnikai, azaz prediktív karbantartási, de hello legújabb cikk, amelynek legtöbb toodate információ mentése hello található [Itt](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). 

## <a name="acknowledgements"></a>**A nyugtázás**
Ez a cikk a felhasználók a adatszakértőkön Pozsony Zhang Gauher Shaheen, Fidan Boylu Uz és szoftver visszafejtés Dan Grecoe a Microsoft által készített.

## <a name="overview"></a>**Áttekintés**
Megoldás sablonok célja tooaccelerate hello folyamatot nevezzük, egy E2E bemutató Cortana Intelligence Suite felett. Egy telepített sablon kiépíteni az előfizetés szükséges a Cortana Intelligence összetevőkkel, és a build hello köztük lévő viszonyt is. Egy adatok készítő alkalmazást, amely akkor letölti és telepíti a helyi gépen hello megoldás sablon üzembe helyezése után előállított mintaadatokkal hello adatok folyamat is magok. hello készítő által létrehozott hello adatok hello adatok adatcsatorna hydrate és elindítani a gépi tanulás előrejelzéseket, amelyek megjeleníthetők a hello Power BI-irányítópult létrehozása. hello telepítési folyamatának végigvezeti a megoldás hitelesítő adatok mentése több lépéseket tooset. Ellenőrizze, hogy a hitelesítő adatokat, például a megoldás neve, a felhasználónevet és jelszót hello telepítése során megadandó rögzítése.  

hello a jelen dokumentum célja tooexplain hello referencia-architektúrában és a különböző összetevőket az előfizetésében megoldás sablon részeként. hello dokumentum is beszél hogyan tooreplace a mintaadatok saját toobe képes toosee insights és a saját adatok által valós adatokkal. Emellett hello dokumentum ismerteti, amelyek a hello kell módosítani, ha a saját adataival toocustomize hello megoldás toobe megoldás sablon részeit. Hogyan toobuild hello Power BI-irányítópulton a Megoldássablon az utasításokat a hello végén.

> [!TIP]
> Töltse le, és nyomtassa ki a [PDF-verziót a dokumentum](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="big-picture"></a>**Nagyméretű kép**
![A prediktív karbantartási architektúrája](media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Hello megoldás telepítésekor Cortana Analytics Suite belül különböző Azure-szolgáltatások rendszer aktiválja azokat (*azaz* Az Event Hubs, a Stream Analytics, HDInsight, adat-előállító gépi tanulás, *stb*). hello architektúra a fenti ábrán, magas szinten, hogyan hello prediktív karbantartási megoldás repüléstechnikai sablon végpont értékekből összeállított. Ezek a szolgáltatások hello azure-portálon parancsával hello megoldás sablon diagramon létre hello megoldás üzembe helyezése hello kivétellel hello HDInsight, mert ez a szolgáltatás ki van építve az igény szerinti hello kapcsolatos képes tooinvestigate fog feldolgozási sor tevékenységek szükséges toorun és törölni ezt követően.
Letöltheti a [hello diagram teljes méretű változatát](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

a következő szakaszok hello minden egyes adatra ismertetik.

## <a name="data-source-and-ingestion"></a>**Az adatforrás és feldolgozási**
### <a name="synthetic-data-source"></a>Szintetikus adatforrás
A sablon hello adatok használt forrás egy asztali alkalmazás, amely akkor letölti és helyileg futtassa a sikeres telepítést jönnek létre. Hello utasításokat toodownload belépve és az alkalmazás telepítése hello tulajdonságok sávon hello prediktív karbantartási Adatgenerátor hívása hello megoldás sablon diagram első csomópontjának kiválasztásakor. Ez az alkalmazás hírcsatornák hello [Azure Event Hubs](#azure-event-hub) szolgáltatás, vagy ugyanazt az események, hello megoldás folyamata hello többi használandó. Ez az adatforrás áll, vagy származó nyilvánosan elérhető adatait a [NASA adattárház](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) hello segítségével [turbóventillátoros motor teljesítménycsökkenést szimuláció adatkészlet](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

hello esemény generációs alkalmazást feltölti hello Azure Event Hubs csak, amíg a számítógép végrehajtása történik.

### <a name="azure-event-hub"></a>Az Azure event hubs
Hello [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) szolgáltatás hello címzett hello a fent leírt szintetikus adatforrás által biztosított hello bemeneti.

## <a name="data-preparation-and-analysis"></a>**Adatok előkészítése és elemzése**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás közel valós idejű elemzés hello bemeneti adatfolyam a hello használt tooprovide [Azure Event Hubs](#azure-event-hub) szolgáltatás, és tegye közzé eredményei közül a [Power BI](https://powerbi.microsoft.com) irányítópult, valamint minden nyers bejövő események toohello Archiválás [Azure Storage](https://azure.microsoft.com/services/storage/) későbbi feldolgozásra által hello szolgáltatás [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás.

### <a name="hd-insights-custom-aggregation"></a>Egyéni összesítési HD Insights
hello Azure HD Insight szolgáltatás használt toorun [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (Azure Data Factory által összehangolva) parancsfájlok tooprovide összesítések hello nyers események volt archivált hello Azure Stream Analytics szolgáltatás használatával.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) szolgáltatás használható-e (Azure Data Factory által összehangolva.) a fennmaradó élettartama (Szabályainak) egy adott repülőgép motor kapott hello bemenetek megadott hello toomake előrejelzéseket.

## <a name="data-publishing"></a>**Adatok közzététele**
### <a name="azure-sql-database-service"></a>Az Azure SQL adatbázis-szolgáltatás
Hello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) szolgáltatása (kezeli az Azure Data Factory) használt toostore hello előrejelzéseket hello fognak használni a hello Azure Machine Learning szolgáltatás által fogadott [Power BI](https://powerbi.microsoft.com) irányítópult.

## <a name="data-consumption"></a>**Adatok felhasználásához**
### <a name="power-bi"></a>Power BI
Hello [Power BI](https://powerbi.microsoft.com) szolgáltatása használt tooshow egy irányítópultot, amely tartalmazza az összesítések és hello által biztosított értesítések [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás, valamint tárolt Szabályainak előrejelzéseket [Azure SQL Adatbázis](https://azure.microsoft.com/services/sql-database/) hello segítségével keletkezett [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) szolgáltatás. Hogyan toobuild hello megoldás sablon Power BI-irányítópultot vonatkozó utasításokért tekintse meg a toohello szakaszban.

## <a name="how-toobring-in-your-own-data"></a>**Hogyan toobring a saját adatok**
Ez a szakasz ismerteti, hogyan toobring területek igényelnének, és a saját adatok tooAzure végrehajtott módosítások hello adatok ebbe az architektúrába állapotba.

Nem valószínű, hogy a dataset kapcsolása megfelelő hello által használt hello dataset [turbóventillátoros motor teljesítménycsökkenést szimuláció adatkészlet](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) megoldás sablon használatos. Az adatok és a követelmények ismertetése a sablon toowork módosítása a saját adataival a kulcsfontosságú lesz. Ha ez az első kitettség toohello Azure Machine Learning szolgáltatáshoz, egy bevezető tooit hello példát használatával kaphat [hogyan toocreate az első kísérlet](machine-learning-create-experiment.md).

a következő szakaszok hello hello sablonból, amely a módosítások van szükség, amikor új adatkészlet megjelent hello szakaszok ismertetik.

### <a name="azure-event-hub"></a>Az Azure Event Hubs
hello Azure Event Hubs szolgáltatás nagyon általános, úgy, hogy az adatok is közzé lehet tenni toohello hub vagy a fürt megosztott kötetei szolgáltatás, vagy a JSON formátumban. Különleges feldolgozás nem lép fel, hello Azure Event hubs Eseményközpontot, de fontos, hogy tudomásul veszi be táplált hello adatokat.

A dokumentum nem ismerteti, hogyan tooingest az adatokat, de használatával egyszerűen küldhet eseményeket vagy adatokat tooan Azure Event Hubs használatával hello Event Hub API.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
hello Azure Stream Analytics szolgáltatás adatfolyamok olvasása és írása források száma adatok tooany által használt tooprovide közel valós idejű elemzési.

A prediktív karbantartási megoldás repüléstechnikai sablon hello az Azure Stream Analytics lekérdezési négy segédlekérdezéseket, minden felhasználó események hello Azure Event Hubs szolgáltatás és a kimeneti kellene négy különböző helyek áll. A kimenetek három Power BI-adatkészletek és egy Azure tárolási helye állnak.

hello Azure Stream Analytics lekérdezési találhatók:

* Hello Azure-portálra való bejelentkezéskor
* Hello Stream Analytics-feladatok keresése ![Stream Analytics ikon](media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) , amely hello megoldás telepítésekor jött létre (*pl.*, **maintenancesa02asapbi** és **maintenancesa02asablob** a prediktív karbantartási megoldás)
* Kiválasztása
  
  * ***BEMENETEK*** tooview hello lekérdezés bemeneti
  * ***LEKÉRDEZÉS*** maga tooview hello lekérdezés
  * ***KIMENETI*** tooview hello különböző kimenetek

Azure Stream Analytics lekérdezési konstrukció kapcsolatos információk találhatók hello [Stream Analytics lekérdezési hivatkozás](https://msdn.microsoft.com/library/azure/dn834998.aspx) az MSDN Webhelyén.

Ebben a megoldásban hello lekérdezések kimeneti közel valós idejű elemzési információ hello bejövő adatok adatfolyam tooa Power BI-irányítópultot, amely része a megoldás sablon a három adatkészletek. Mivel a hello bejövő adatformátum implicit ismerete, ezeket a lekérdezéseket toobe módosítani kell az adatok formátum alapján.

hello második Stream Analytics-feladat hello lekérdezést **maintenancesa02asablob** egyszerűen kiírja az összes [Eseményközpont](https://azure.microsoft.com/services/event-hubs/) események [Azure Storage](https://azure.microsoft.com/services/storage/) és ezért nem módosítása az adatok formátuma hello függetlenül teljes eseményadatok toostorage továbbítja adatfolyamként.

### <a name="azure-data-factory"></a>Azure Data Factory
Hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás koordinálja a hello mozgás és az adatok feldolgozása. A prediktív karbantartási repüléstechnikai Megoldássablonban hello adatok gyári áll három [folyamatok](../data-factory/data-factory-create-pipelines.md) , helyezze át, és a különféle technológiái hello adatok feldolgozása.  A data factory hello hello adat-előállító csomópont hello megoldás sablon diagram hello megoldás üzembe helyezése hello létre hello alján megnyitásával érheti el. Ez eltarthat, toohello adat-előállítót az Azure-portál. Ha hibaüzenet jelenik meg a adatkészletek alatt, figyelmen kívül hagyhatja azokat, mivel ezek hello adatgenerátor indítása előtt telepített toodata gyári miatt. Ezek a hibák nem akadályozzák a data factory működését.

![Data Factory dataset hibák](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Ez a szakasz ismerteti, amelyek hello szükséges [folyamatok](../data-factory/data-factory-create-pipelines.md) és [tevékenységek](../data-factory/data-factory-create-pipelines.md) hello szereplő [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Alább az hello diagram nézet hello megoldás.

![Azure Data Factory](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Tartalmazza az adat-előállító kimenetátirányítási mechanizmusát használó műveletekről hello két [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) használt toopartition és összesített hello adatok parancsfájlokat. Ha nincs jelezve, hello parancsfájlok található hello [Azure Storage](https://azure.microsoft.com/services/storage/) a telepítés során létrehozott fiók. A helyen lesz: maintenancesascript\\\\parancsfájl\\\\hive\\ \\ (vagy https://[Your megoldás name].blob.core.windows.net/maintenancesascript).

Hasonló toohello [Azure Stream Analytics](#azure-stream-analytics-1) lekérdezéseket, a [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok rendelkezik hello bejövő adatformátum implicit ismerete, ezeket a lekérdezéseket toobe módosítani kell az adatformátum és alapján[jellemzőkiemelés](machine-learning-feature-selection-and-engineering.md) követelményeinek.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
Ez [csővezeték](../data-factory/data-factory-create-pipelines.md) tartalmazza egy adott tevékenység - egy [HDInsightHive](../data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , amelyen fut a [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl toopartition hello adatok be [Azure Storage](https://azure.microsoft.com/services/storage/) során a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) feladat.

A [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) particionálási feladat érték parancsfájl ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Ez [csővezeték](../data-factory/data-factory-create-pipelines.md) tartalmaz a több tevékenységet, és amelynek végeredménynek hello a program pontozza a mennyiségeket hello előrejelzéseket [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) megoldás sablonhoz társított kísérlet.

Ez található hello tevékenységeket a következők:

* [HDInsightHive](../data-factory/data-factory-hive-activity.md) tevékenység segítségével egy [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , amelyen fut a [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) tooperform összesítések parancsfájl és hello szükséges jellemzőkiemelés [Azure Gépi tanulás](https://azure.microsoft.com/services/machine-learning/) kipróbálásához.
  A [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) particionálási feladat érték parancsfájl ***PrepareMLInput.hql***.
* [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, amely hello eredményeinek helyezi át a [HDInsightHive](../data-factory/data-factory-hive-activity.md) tevékenység tooa egyetlen [Azure Storage](https://azure.microsoft.com/services/storage/) blob, amely lehet hozzáférését a [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység, amely behívja hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) kísérlet, amely egyetlen üzembe hello eredmények eredményez [Azure Storage](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Ez [folyamat](../data-factory/data-factory-create-pipelines.md) tartalmazza egy adott tevékenység - egy [másolása](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység, amely helyezi át a hello hello eredményeit [Azure Machine Learning](#azure-machine-learning) a kísérletezhet a *** MLScoringPipeline*** toohello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) , hogy a megoldás sablon telepítésének részeként lett kiépítve.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) használt kísérlet megoldás sablon szolgáltatása hello fennmaradó hasznos élettartama (Szabályainak) repülőgép motor. hello kísérlet felhasznált adathalmaz konkrét toohello, és ezért szükséges állapotba kerül, a módosítás vagy cseréje adott toohello adatokat.

További információ a hello Azure Machine Learning kísérlet létrehozásának módja: [prediktív karbantartási: 1. lépés 3-as adatok előkészítése, a szolgáltatás fejlesztés](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**A figyelő folyamatban**
 Adatgenerátor hello eszköztárat, hello csővezeték kezdődik hidratált tooget, és hello különböző összetevői a megoldás indítsa el a következő hello parancsok hello adat-előállító által kibocsátott művelet kicking. Két módon figyelheti hello folyamat.

1. Hello nyers bejövő tooblob adattárolás ír egy hello Stream Analytics-feladat. Kattintson a Blob Storage-összetevő a megoldás az üdvözlő képernyőt, sikeresen telepített hello megoldás, és kattintson a Megnyitás hello jobb oldali panelen, ha azt elindítjuk ezt a toohello [kezelési portál](https://portal.azure.com/). Ezután kattintson a Blobok. A következő panelen hello látni fogja a tárolók listája. Kattintson a **maintenancesadata**. A következő panelen hello látni fogja a hello **rawdata** mappa. Hello rawdata mappába, látni fogja a mappák, valamint a neveket például órát = 17 óra = 18 stb. Ha ezeket a mappákat, azt jelzi, hogy hello nyers adatok sikeresen jön létre a számítógépen és a blob storage-ban tárolt. Meg kell jelennie a csv-fájlok, amelyben véges mérete (MB) az érintett mappákat.
2. hello hello folyamat utolsó lépését az toowrite adatai (pl. a gépi tanulás által előrejelzéseket) az SQL-adatbázisba. Lehetséges, hogy toowait legfeljebb három óra hello adatok tooappear az SQL-adatbázisban. Egyirányú toomonitor keresztül van mennyi adatot érhető el az SQL-adatbázis [azure-portálon](https://manage.windowsazure.com/). Hello bal oldali panelen keresse meg az SQL-ADATBÁZISOK ![SQL ikon](media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) , és kattintson rá. Keresse meg az adatbázis **pmaintenancedb** , és kattintson rá. Hello következő oldalon hello lap alján kattintson a kezelés
   
    ![Ikon kezelése](media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png).
   
    Itt kattintson az új lekérdezés és a lekérdezés megadása a sorok (pl. a PMResult válassza count(*)) hello száma. Az adatbázis növekedésének megfelelően, növelje a hello hello tábla sorainak száma.

## <a name="power-bi-dashboard"></a>**A Power BI-irányítópulton**
### <a name="overview"></a>Áttekintés
Ez a szakasz ismerteti, hogyan tooset fel Power BI irányítópult toovisualize a valós idejű adatok Azure Stream Analytics (gyakran használt adatok elérési útja), valamint kötegelt előrejelzés ki annak az eredménye az Azure machine learning (cold elérési út).

### <a name="setup-cold-path-dashboard"></a>A telepítő cold elérési irányítópult
Hello cold elérési adatok feldolgozási hello alapvető célja a prediktív Szabályainak (fennmaradó élettartama) minden egyes repülőgép motor tooget egy repülési (ciklus) művelet befejeződése után. hello előrejelzés eredménye, hogy végzett a felhőszolgáltató közötti átviteléhez során hello elmúlt 3 óra hello repülőgép motorok előrejelzésére 3 óránként frissül.

A Power BI tooan Azure SQL adatbázis adatforrásként, az előrejelzési eredmények tároló csatlakozik. Megjegyzés: 1), a megoldás telepítéséhez, egy valódi előrejelzés fog megjelenni hello adatbázis 3 órán belül.
hello generátor letöltési mellékelt hello pbix-fájl néhány kezdőérték adatokat tartalmaz, így azonnal létrehozhat hello Power BI-irányítópultot. 2.) az ebben a lépésben hello előfeltétel, toodownload és a telepítés hello ingyenes szoftvert [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

hello következő lépéseit ismerteti a hogyan tooconnect hello pbix-fájlt, amely a megoldás központi telepítési adatokat tartalmazó hello időpontjában hoz létre SQL adatbázis hello fájlt (*pl.*. Előrejelzés eredmények) a képi megjelenítéshez tartozó.

1. Hello adatbázis hitelesítő adatainak lekéréséhez.
   
   Szüksége lesz **adatbázis-kiszolgáló nevét, az adatbázisnév, felhasználónevet és jelszót** toonext lépéseket áthelyezése előtt. Az alábbiakban hello lépéseket tooguide, hogyan toofind őket.
   
   * Egyszer **"Azure SQL Database"** a megoldás sablonban diagram zöldre, kattintson rá, és kattintson a **"Megnyitás"**.
   * Láthatja, hogy egy új lap/böngészőablakot amely hello Azure portálon. Kattintson a **"Erőforráscsoportok"** hello bal oldali panelen.
   * Hello előfizetés hello megoldás telepítéséhez használ, majd válassza ki és **"YourSolutionName\_ResourceGroup"**.
   * A hello új pop kimenő panelen, kattintson a hello ![SQL ikon](media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) ikon tooaccess az adatbázis. Az adatbázisnév van a következő toohello erre az ikonra (*pl.*, **"pmaintenancedb"**), és hello **adatbázis-kiszolgáló neve** megtalálható-e hello Server name tulajdonság és az alábbihoz hasonló túl**YourSoutionName.database.windows.net**.
   * Az adatbázis **felhasználónév** és **jelszó** hello megegyeznek a hello felhasználónév és jelszó korábban rögzített során hello megoldás üzembe helyezése.
2. Hello cold elérési jelentésfájl hello adatforrás frissítése. a Power BI Desktop.
   
   * A számítógépen, amelyen le, és a kódgenerátor fájl unzipped hello mappában kattintson duplán a **Power bi\\PredictiveMaintenanceAerospace.pbix** fájlt. Ha a figyelmeztető üzeneteket hello fájl megnyitásakor, figyelmen kívül hagyja azokat. A hello hello fájl felső részén kattintson **szerkesztése lekérdezések**.
     
     ![Lekérdezések szerkesztése](media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * Látni fogja, két tábla **RemainingUsefulLife** és **PMResult**. Ki kell jelölnie hello első táblázatot, és kattintson ![lekérdezés beállítások ikonra](media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) tovább túl**"Forrás"** alatt **alkalmazott LÉPÉSEKET** a jobb oldali hello **"Lekérdezés beállításai"**panel. Hagyja figyelmen kívül minden mellőzött figyelmeztető üzenet jelenik meg.
   * Cserélje le a hello előugró ablak ki, **"Server"** és **"Database"** a saját kiszolgáló és az adatbázis nevének, és kattintson a **"OK"**. A kiszolgálónév, győződjön meg arról, megadhatja a 1433-as port hello (**YourSoutionName.database.windows.net, 1433**). Hello adatbázis mezőt hagyja **pmaintenancedb**. Hello képernyőn megjelenő hello figyelmeztető üzenetek figyelmen kívül.
   * Hello következő pop ki ablakot, két lehetőség hello bal oldali ablaktáblán látható (**Windows** és **adatbázis**). Kattintson a **"Database"**, töltse ki a **"Username adat"** és **'Password'** (Ez az hello felhasználónevet és jelszót, amikor először telepített hello megoldás és létrehozott egy Az Azure SQL-adatbázis). A ***válassza ki, amelyek szinten tooapply ezeket a beállításokat***, ellenőrizze az adatbázis-szintű beállítás. Kattintson a **"Csatlakozás"**.
   * Hello második táblán kattintson **PMResult** kattintson ![navigációs ikon](media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) következő túl**"Forrás"** alatt **alkalmazott LÉPÉSEKET** a jobb oldali hello **"Lekérdezés beállításai"** panelen, és hello kiszolgáló és az adatbázis neve, ahogy fent lépéseket hello frissítése, majd kattintson az OK gombra.
   * Ha Ön az interaktív hátsó toohello előző lapon, a hello ablak bezárásához. Egy üzenet jelenik out - kattintson **alkalmaz**. Végül kattintson a hello **mentése** toosave hello módosítások gombra. A Power BI-fájl most létesített kapcsolat toohello kiszolgáló. A képi megjelenítések üres esetén, feltétlenül törölje hello beállításokat a hello képi megjelenítések toovisualize összes hello adatok hello jobb felső sarkában hello jelmagyarázatok hello radír ikonjára kattint. Hello képi megjelenítések hello frissítési gomb tooreflect új adatok használatát. Kezdetben csak akkor jelenik meg hello adatait a a képi megjelenítések, adat-előállító hello ütemezett toorefresh 3 óránként. 3 óra elteltével jelenik meg új előrejelzéseket megjelennek a képi megjelenítések hello adatok frissítésekor.
3. (Választható) Hello cold elérési irányítópult túl közzététele[Power BI online](http://www.powerbi.com/). Vegye figyelembe, hogy ez a lépés a Power BI fiókot (vagy Office 365-fiókkal) kell-e.
   
   * Kattintson a **"Publish"** és néhány másodperc múlva megjelenik egy ablak megjelenítése "Közzétételi tooPower BI Success!" a zöld pipa jelzi. Kattintson alább a "Megnyitás PredictiveMaintenanceAerospace.pbix a Power BI" hello hivatkozásra. toofind részletes ismertetését lásd: [a Power BI Desktop közzététele](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * új irányítópult toocreate: hello kattintson  **+**  következő toothe jelentkezzen **irányítópultok** szakasz hello bal oldali ablaktáblán. Adja meg az új irányítópult "Prediktív karbantartási bemutató" hello nevét.
   * Amikor hello jelentés megnyitásához kattintson a ![RAJZSZÖG ikonra](media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) toopin a képi megjelenítések tooyour irányítópult. toofind részletes ismertetését lásd: [rögzítheti egy mozaik tooa Power BI-irányítópultot jelentésből](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Nyissa meg toohello irányítópult-oldalon, és hello méret és elhelyezkedés a képi megjelenítést és szerkessze a címben. toofind részletes utasításokat hogyan tooedit a csempék: [Szerkesztés mozaik – átméretezési, move, nevezze át, PIN-kód, törlése, hivatkozás hozzáadása a](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Íme egy példa irányítópultot, amelynek egyes cold elérési képi megjelenítések rögzítve tooit.  Attól függően, hogy mennyi ideig a adatgenerátor futtatja a hello képi megjelenítések a számok lehetnek.
     <br/>
     ![A végső nézete](media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * tooschedule frissítési hello adatok, az egérmutatóval rámutat hello **PredictiveMaintenanceAerospace** dataset, kattintson a ![Elipsis ikon](media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) majd **ütemezés frissítése**.
     <br/>
     **Megjegyzés:** Ha megjelenik egy figyelmeztetés masszírozó, kattintson a **hitelesítő adatok szerkesztése** , és győződjön meg arról, hogy az adatbázis hitelesítő adatai vannak hello ugyanaz, mint 1. lépésben leírt.
     <br/>
     ![A frissítésütemezés](media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * Bontsa ki a hello **ütemezés frissítése** szakasz. Kapcsolja be "az adatok naprakészen tarthatók".
     <br/>
   * Hello a frissítésütemezés igényei szerint. toofind további információkért lásd: [adatfrissítési a Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>A telepítő gyakran használt adatok elérési útja irányítópult
a lépéseket követve hello ismerteti hogyan toovisualize valós idejű adatok kimenetét a Stream Analytics hello megoldás üzembe helyezése során létrehozott feladatok. A [Power BI online](http://www.powerbi.com/) fiók szükség tooperform hello a következő lépéseket. Ha nincs fiókja, akkor [hozzon létre egyet](https://powerbi.microsoft.com/pricing).

1. Adja hozzá a Power BI-kimenet Azure Stream Analytics (ASA).
   
   * Toofollow hello utasításait kell [Azure Stream Analytics & Power BI: valós idejű látható-e a streamelési adatok a valós idejű elemzési irányítópult](../stream-analytics/stream-analytics-power-bi-dashboard.md) tooset mentése az Azure Stream Analytics-feladat, mint a Power hello kimenete Üzleti INTELLIGENCIA irányítópult.
   * hello ASA lekérdezés rendelkezik három kimenetek, amelyek **aircraftmonitor**, **aircraftalert**, és **flightsbyhour**. A lekérdezés fülre kattintva megtekintheti a hello lekérdezés. Megfelelő tooeach ezeket a táblákat, szüksége lesz egy kimeneti tooASA tooadd. Hello első kimeneti hozzáadásakor (*pl.* **aircraftmonitor**) Győződjön meg arról, hogy hello **kimeneti Alias**, **Adatkészletnevet** és  **Tábla neve** azonos hello (**aircraftmonitor**). Ismétlődő hello lépéseket tooadd kiírja a **aircraftalert**, és **flightsbyhour**. Miután hozzáadta, mind a hármat kimeneti táblák és elindított hello ASA, egy megerősítő üzenetet kapja meg (*pl.*, "Indítása Stream Analytics-feladat sikeres maintenancesa02asapbi").
2. Jelentkezzen be túl[online Power bi-ban](http://www.powerbi.com)
   
   * A bal oldali panelen adatkészletek szakasz a saját munkaterületen hello a ***DATASET*** nevek **aircraftmonitor**, **aircraftalert**, és **flightsbyhour**megjelenjen-e. Ez az adatfolyam hello előző step.hello adatkészlet Azure Stream Analytics leküldött hello **flightsbyhour** előfordulhat, hogy nem jelenik meg: hello azonos egyidejűleg más két adatkészletet hello SQL-lekérdezés mögött toohello jellege miatt hello azt. Azonban azt kell jelenik meg egy óra múlva.
   * Győződjön meg arról, hogy hello ***képi megjelenítések*** ablaktábla meg nyitva, és az üdvözlő képernyőt jobb oldalán látható.
3. Után hello adatok továbbítására Power BI-bA rendelkezik, megkezdheti a streamelési adatok hello megjelenítése. Az alábbiakban egy példa irányítópult néhány gyakran használt adatok elérési útja megjelenítésekkel tooit rögzítve. Más irányítópult-csempéihez megfelelő adatkészletek alapján hozhat létre. Attól függően, hogy mennyi ideig a adatgenerátor futtatja a hello képi megjelenítések a számok lehetnek.

    ![Irányítópult-nézet](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

1. Az alábbiakban néhány lépést toocreate valamelyik fenti – hello csempék hello "érzékelő 11 vs flotta ábrázolása. Küszöbérték 48.26" csempe:
   
   * Kattintson a dataset **aircraftmonitor** a bal oldali panelen adatkészletek szakasz hello.
   * Kattintson a hello **vonaldiagram** ikonra.
   * Kattintson a **feldolgozott** a hello **mezők** ablaktábla úgy, hogy az informatikai jeleníti meg a "Tengely" hello **képi megjelenítések** ablaktáblán.
   * Kattintson a "s11" és "s11\_riasztás", mindkettő "Értékek" alatt jelenik meg. Kattintson a hello kis nyílra tovább túl**s11** és **s11\_riasztás**, módosítsa a "Sum" túl "átlagos".
   * Kattintson a **mentése** felső hello és hello jelentés "aircraftmonitor" nevet. "Aircraftmonitor" nevű jelentés megjelenő hello **jelentések** hello szakasz **Navigator** hello bal oldali ablaktáblán.
   * Kattintson a hello **PIN-kód Visual** hello jobb felső sarkában a vonaldiagram ikonjára. "PIN-kód tooDashboard" ablak lehet, hogy jelenik meg, hogy válasszon egy irányítópultot. "A prediktív karbantartási bemutató", majd kattintson a "PIN-kód".
   * Ez a csempe hello irányítópulton keresztül hello egérrel, kattintson a hello jobb felső sarokban található toochange hello "edit" ikonra a cím túl "érzékelő 11 nézet járműflotta vs. Küszöbérték 48.26" és a felirat túl"átlagos között időbeli járműflotta".

## <a name="how-toodelete-your-solution"></a>**Hogyan toodelete a megoldás**
Győződjön meg arról, hogy használatakor nem aktívan hello megoldás, magasabb költségek gyakorisága hello adatgenerátor fut le hello adatgenerátor. Ha nem használnak, törölje a hello megoldás. A megoldás törlése eltávolítja az előfizetésben kiépített hello megoldás üzembe helyezésekor hello összetevők. toodelete hello megoldás kattintson a bal oldali panelen hello hello megoldás sablon a megoldás neve, majd kattintson a törlés.

## <a name="cost-estimation-tools"></a>**Költség becslése eszközök**
hello következő két eszközök állnak rendelkezésre toohelp megértésében hello prediktív karbantartási futtató repüléstechnikai Megoldássablon az előfizetésében szereplő részt a teljes költségek:

* [A Microsoft Azure költség négyzetgyökének eszköz (online)](https://azure.microsoft.com/pricing/calculator/)
* [A Microsoft Azure költség négyzetgyökének eszköz (asztali verzió)](http://www.microsoft.com/download/details.aspx?id=43376)

