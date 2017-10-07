---
title: "aaaDeep alaposabban előre jelezni a vehicle állapotát, és ki irányítja az szokásait - Azure |} Microsoft Docs"
description: "Használja a Cortana Intelligence toogain valós idejű és prediktív elemzések hello lehetőségeit a vehicle állapotát, és ki irányítja az szokásokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: ba1448a5081762292561f904d9ec54617c9a5330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-hello-solution"></a>Vehicle telemetriai analytics megoldás forgatókönyv: hello megoldásba történő részletes bemutatója
Ez **menü** hivatkozik, ez a forgatókönyv toohello szakaszait: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Ez a szakasz részletezi mindegyik hello szakaszból hello megoldásarchitektúra utasításokat és testreszabási mutatók írja le. 

## <a name="data-sources"></a>Adatforrások
hello megoldás két különböző adatforrásból használ:

* **Szimulált vehicle jelek és diagnosztikai adatkészlet** és 
* **vehicle katalógus**

A vehicle telematika szimulátor Ez a megoldás részét. Diagnosztikai adatok bocsát ki, és hello vehicle és a minta egy időben befolyásoló tényezők toohello megfelelő toohello állapotát jelzi. Kattintson a [Vehicle telematika szimulátor](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle telematika szimulátor Visual Studio megoldás** testreszabni a követelmények alapján. hello vehicle katalógus tartalmazza a referencia-adatkészletnek a VIN toomodel leképezéssel.

![Vehicle telematika szimulátor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

*1. ábra – Vehicle telematika szimulátor*

Ez a séma a következő hello tartalmazó JSON-formátumú dataset.

| Oszlop | Leírás | Értékek |
| --- | --- | --- |
| VIN |Véletlenszerűen létrehozott Vehicle azonosító szám |10 000 véletlenszerűen létrehozott vehicle azonosító számok fő listájának származik. |
| Külső hőmérséklet |hello kívül hőmérséklet, ahol hello vehicle befolyásoló tényezők |Véletlenszerűen létrehozott szám 0 – 100 |
| Motor |hello motor hőmérséklete hello vehicle |Véletlenszerűen létrehozott száma 0-500 |
| Gyorsaság |hello motor sebessége, mely hello vehicle befolyásoló tényezők |Véletlenszerűen létrehozott szám 0 – 100 |
| Üzemanyag |hello vehicle hello üzemanyag szintje |Véletlenszerűen létrehozott szám 0 – 100 (üzemanyag százalékos értéke jelzi) |
| EngineOil |hello motor olaj szintű hello vehicle |Véletlenszerűen létrehozott szám 0 – 100 (motor olaj százalékos értéke jelzi) |
| Kulcsszava nyomás |hello kulcsszava nyomás hello vehicle |Véletlenszerűen előállított száma 0-50 (kulcsszava nyomás százalékos értéke jelzi) |
| Kilométer |hello vehicle hello kilométer olvasása |Véletlenszerűen létrehozott szám 0-200000 |
| Accelerator_pedal_position |hello gyorsító tartásához lehetőleg keveset pozíciója hello vehicle |Véletlenszerűen létrehozott szám 0 – 100 (gyorsító százalékos értéke jelzi) |
| Parking_brake_status |Azt jelzi, hogy hello vehicle kell itt tartózkodnia vagy sem |IGAZ vagy hamis |
| Headlamp_status |Azt jelzi, ahol hello fényszóró megtalálható-e |IGAZ vagy hamis |
| Brake_pedal_status |Azt jelzi, hogy hello fékpedál nélkül, vagy sem |IGAZ vagy hamis |
| Transmission_gear_position |hello átviteli fogaskerék hello vehicle helyzete |Állapotok: először, a második, harmadik, negyedik, ötödik, hatodik, hetedik, nyolcadik |
| Ignition_status |Azt jelzi, hogy hello vehicle fut vagy leállt |IGAZ vagy hamis |
| Windshield_wiper_status |Azt jelzi, hogy az hello szélvédőkeret ablaktörlő be van kapcsolva vagy nem |IGAZ vagy hamis |
| ABS |Azt jelzi, hogy ABS részt vevő vagy sem |IGAZ vagy hamis |
| időbélyeg |hello időbélyeg hello adatpont létrehozásakor |Dátum |
| Város |hello vehicle hello helye |Ebben a megoldásban 4 városokat: Bellevue, Redmond, Sammamish, Seattle |

hello vehicle modell referencia-adatkészletnek VIN toohello modell leképezést tartalmaz. 

| VIN | Modell |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Sedan |
| 8J0U8XCPRGW4Z3NQE |Hibrid |
| WORG68Z2PLTNZDBI7 |Családbiztonsági limuzin |
| JTHMYHQTEPP4WBMRN |Sedan |
| W9FTHG27LZN1YWO0Y |Hibrid |
| MHTP9N792PHK08WJM |Családbiztonsági limuzin |
| EI4QXI2AXVQQING4I |Sedan |
| 5KKR2VB4WHQH97PF8 |Hibrid |
| W9NSZ423XZHAONYXB |Családbiztonsági limuzin |
| 26WJSGHX4MA5ROHNL |Váltható |
| GHLUB6ONKMOSI7E77 |Állomás kocsi |
| 9C2RHVRVLMEJDBXLP |Kompakt autó |
| BRNHVMZOUJ6EOCP32 |Kis SUV |
| VCYVW0WUZNBTM594J |Sport autó |
| HNVCE6YFZSA5M82NY |Közepes SUV |
| 4R30FOR7NUOBL05GJ |Állomás kocsi |
| WYNIIY42VKV6OQS1J |Nagy SUV |
| 8Y5QKG27QET1RBK7I |Nagy SUV |
| DF6OX2WSRA6511BVG |Coupe |
| Z2EOZWZBXAEW3E60T |Sedan |
| M4TV6IEALD5QDS3IR |Hibrid |
| VHRA1Y2TGTA84F00H |Családbiztonsági limuzin |
| R0JAUHT1L1R3BIKI0 |Sedan |
| 9230C202Z60XX84AU |Hibrid |
| T8DNDN5UDCWL7M72H |Családbiztonsági limuzin |
| 4WPYRUZII5YV7YA42 |Sedan |
| D1ZVY26UV2BFGHZNO |Hibrid |
| XUF99EW9OIQOMV7Q7 |Családbiztonsági limuzin |
| 8OMCL3LGI7XNCC21U |Váltható |
| ……. | |

### <a name="references"></a>Referencia
[Vehicle telematika szimulátor Visual Studio megoldás](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Az Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a>Adatfeldolgozást
Azure Event Hubs, a Stream Analytics és a Data Factory kombinációi kihasználhatók tooingest hello vehicle jelek, hello diagnosztikai eseményeket, és valós idejű és kötegelt elemzés. Ezek az összetevők létrehozni és konfigurálni hello megoldás központi telepítésének részeként. 

### <a name="real-time-analysis"></a>Valós idejű elemzés
hello hello Vehicle telematika szimulátor által előállított eseményeket közzétett toohello Eseményközpont használatával hello Event Hub SDK. hello Stream Analytics-feladat ingests ezeket az eseményeket az Event Hubs hello és folyamatok hello valós idejű tooanalyze hello vehicle egészségügyi adatokat. 

![Event hub irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

*4. ábra - Eseményközpont irányítópult*

![A Stream Analytics-feladat adatainak feldolgozása](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

*5. ábra - adatfeldolgozás Stream Analytics-feladat*

hello Stream Analytics-feladat;

* az Event Hubs hello adatait ingests 
* hajt végre egy csatlakoztatás hello adatok toomap hello vehicle VIN toohello megfelelő referenciamodellje 
* az Azure blob-tároló gazdag kötegelt elemzés fenntartása őket. 

a következő Stream Analytics lekérdezési hello használt toopersist hello adatok Azure blob Storage tárolóban. 

![Stream Analytics feladat lekérdezés adatfeldolgozást](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*6. ábra – a Stream Analytics-feladat adatfeldolgozást lekérdezése*

### <a name="batch-analysis"></a>Kötegelt elemzés
Azt is generál egy szimulált vehicle jelek és diagnosztikai adatkészlet gazdagabb kötegelt elemzés további mennyiségét. Ez a helyes reprezentatív kötetet kötegelt feldolgozáshoz szükséges tooensure. Erre a célra egy elnevezett "PrepareSampleDataPipeline" hello Azure Data Factory munkafolyamat toogenerate folyamat használunk szimulált vehicle jelek és diagnosztikai adatkészlet egy év alatt érkezett. Kattintson a [adat-előállító egyéni tevékenység](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello adat-előállító egyéni DotNet tevékenység Visual Studio megoldás testreszabni a követelmények alapján. 

![Kötegfeldolgozási munkafolyamat mintaadatok előkészítése](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*7. ábra - mintaadatok előkészítése kötegelt feldolgozásra munkafolyamat*

hello csővezeték áll egy egyéni ADF .net tevékenység, itt megjelenítése:

![PrepareSampleDataPipeline tevékenység](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

*8. ábra - PrepareSampleDataPipeline*

Miután hello folyamat sikeresen lefut, és "RawCarEventsTable" adatkészlet "Kész" van megjelölve egyéves érdemes szimulált vehicle jelek és diagnosztikai adatainak előállítása. Lásd a következő hello mappához és fájlhoz hello "connectedcar" tárolóban a storage-fiókban létrehozni:

![PrepareSampleDataPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*9. ábra - PrepareSampleDataPipeline kimeneti*

### <a name="references"></a>Referencia
[Az Azure Event Hub SDK a streameket](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[A mozgás képességek az Azure Data Factory adatok](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet tevékenység](../data-factory/data-factory-use-custom-activities.md)

[Az Azure Data Factory DotNet tevékenység visual studio megoldás a mintaadatok előkészítése](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-hello-dataset"></a>Partíció hello adatkészlet
hello nyers félig strukturált vehicle jelek és diagnosztikai adatkészlet particionáltak hello adatok előkészítő lépésében egy év/hónap formátumú fájlba. A particionálás elősegíti hatékonyabb kérdez le, és kitölti-e hiba átvevő egy blob fiók toohello a következő hello első fiókként engedélyezésével méretezhető, hosszú távú tároláshoz. 

>[!NOTE] 
>Ez a lépés hello megoldásban alkalmazható csak toobatch feldolgozásával.

Bemeneti és kimeneti adatok adatkezelés:

* Hello **kimeneti adatai** (feliratú *PartitionedCarEventsTable*) toobe másolatok hosszú időn hello eligazodást / "rawest" képernyő "Data Lake" hello felhasználói adatok. 
* Hello **bemeneti adatai** toothis csővezeték volna általában elvesznek, mivel hello kimeneti adatokat adjon meg teljes visszaadása toohello rendelkezik – csak tárolt (particionált) jobban későbbi használatra.

![Partíció car események munkafolyamat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

*10. ábra – partíció Car események munkafolyamat*

hello nyers adatok particionálása használata a Hive HDInsight tevékenység "PartitionCarEventsPipeline". egy évig az 1. lépésben létrehozott hello mintaadatok év/hónap szerint van particionálva. hello partíció található, az év havonta (összesen 12 partíciók) használt toogenerate vehicle jelek és diagnosztikai adatokat. 

![PartitionCarEventsPipeline tevékenység](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

*11. ábra - PartitionCarEventsPipeline*

***PartitionConnectedCarEvents Hive-parancsfájl***

hello következő Hive parancsfájl, "partitioncarevents.hql" nevű particionálás szolgál, és a hello letöltött zip hello "\demo\src\connectedcar\scripts" mappában található. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.

![A particionált kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

*12. ábra - particionált kimeneti*

hello adatok most már megfelelően lett optimalizálva, kezelhető, és készen áll a toogain gazdag kötegelt insights további feldolgozásra. 

## <a name="data-analysis"></a>Adatok elemzése
Ebben a szakaszban láthatja, hogyan toocombine Azure Stream Analytics, az Azure Machine Learning, az Azure Data Factory és a gazdag Azure HDInsight speciális elemzés vehicle állapotát, és ki irányítja a szokásokat. Itt három alszakaszokat van:

1. **Gépi tanulás**: a jelen alszakasz ismerteti a hello anomáliadetektálási észlelési kísérlet, amely a megoldás toopredict járművekről gyűjtött igénylő karbantartási karbantartási és járművekről gyűjtött igénylő visszahívások toosafety problémák miatt a használtuk.
2. **Valós idejű elemzési**: a jelen alszakasz vonatkozó hello valós idejű elemzési hello Stream Analytics lekérdezési nyelv használatával, és hello gépi tanulás végrehajtott kísérletet használ egy egyéni alkalmazást valós idejű információkat tartalmaz.
3. **Kötegelt elemzés**: a jelen alszakasz hello átalakítása és az Azure HDInsight és az Azure Machine Learning Azure Data Factory által operationalized hello kötegelt adatok feldolgozása vonatkozó információkat tartalmazza.

### <a name="machine-learning"></a>Machine Learning
Itt célunk toopredict hello járművekről gyűjtött vagy a karbantartási és a visszaírási bizonyos heath statisztika alapján. A következő feltételek hello biztosítjuk

* A következő három feltétel hello egyike igaz, ha hello járművekről gyűjtött szükséges **karbantartási karbantartási**:
  
  * Kulcsszava nyomás értéke alacsony
  * Motor olaj szintje alacsony
  * Motor túl magas
* Előfordulhat, ha hello a következő feltételek egyike teljesül, hello járművekről gyűjtött egy **biztonsági kérdés** és a szükséges **visszaírási**:
  
  * Motor magas, de külső hőmérséklet alacsony
  * Motor alacsony, de külső hőmérséklet magas

Hello előző követelmények alapján létrehoztunk két külön modellek toodetect rendellenességek észlelését, egy a vehicle karbantartási észleléséhez és egy a vehicle visszaírási észleléséhez. Mindkét modellek hello beépített egyszerű összetevő elemzés (PEM) algoritmus használják a anomáliadetektálás. 

**Karbantartási modell**

-Kulcsszava nyomás, motor olaj vagy motor hőmérséklet - három mutatók egyikét megfelel az megfelelő állapotba, ha a modell hello karbantartási jelentések az anomáliadetektálási. Ennek eredményeképpen csak kell tooconsider három változókhoz hello modell fejlesztése során. A kísérlet az Azure Machine Learning, először használjuk a **Select Columns in Dataset** modul tooextract három változókhoz. Ezután hello PCA alapú észlelési modul toobuild hello anomáliadetektálási modell használjuk. 

Egyszerű összetevő elemzésre (PEM) egy meglévő technika, a gépi tanulás alkalmazott toofeature kijelölés, a besorolást és a anomáliadetektálási észleléséhez használható. PEM alakít egy készletét tartalmazó esetlegesen kapcsolódó változók neve a fő összetevőit értékek esetében. hello kulcs a PEM-alapú modellezési lényege, alsó dimenziós szóközzel tooproject adatokat, hogy a szolgáltatások és rendellenességeket könnyebben azonosítható legyen.

Az egyes új bemeneti túl hello modell, hello anomáliadetektálási érzékelő először kiszámítja a leképezés a hello eigenvectors, és majd kiszámítja hello normalizált újjáépítése hiba. Ez a normalizált hiba hello anomáliadetektálási pontszám. hello nagyobb hello hiba hello több rendellenes hello példány. 

Hello karbantartási észlelési problémát, a rekordokban tekinthető kulcsszava nyomás motor olaj és motor által definiált egy pont a 3-dimenziós térben koordinátái. toocapture ezek rendellenességek észlelését, azt kivetíthetik hello eredeti adatok hello 3 dimenziós területen a 2-dimenziós területére PEM használatával. Ebből kifolyólag hello paraméter összetevők toouse száma hivatott PEM toobe 2. Ez a paraméter a PEM-alapú anomáliadetektálás alkalmazása fontos szerepet játszik. PEM kiálló adatokat, miután azt azonosíthatja ezeket a rendellenességeket könnyebben.

**Visszaírási anomáliadetektálási modell** hello Select Columns in Dataset és PCA alapú használjuk hello visszaírási anomáliadetektálási modell, észlelési modulok hasonló módon. Pontosabban, azt először nyerje ki három változók - motor, a külső hőmérséklet és a sebességét – hello segítségével **Select Columns in Dataset** modul. Is magában foglalja az hello sebesség változó, mert hello motor hőmérséklet általában korrelált toohello sebessége. PCA alapú észlelési modul tooproject hello adatok mellett a 2-dimenziós területére hello 3 dimenziós területéből használjuk. hello visszaírási feltételek teljesülnek, és ehhez hello vehicle kell-e visszaírási amikor motor és a külső hőmérséklet magas negatívan közötti kapcsolatot. PCA alapú észlelési algoritmus használ, azt rögzítheti a hello rendellenességeket PEM végrehajtása után. 

Vagy a modell betanításakor kell toouse megszokott adatforgalmi, amely nem igényli a karbantartás vagy visszaírási hello bemeneti adatok tootrain hello PCA alapú észlelési modellre. A pontozási kísérlet hello használjuk betanítása hello anomáliadetektálási észlelési modell toodetect hello vehicle karbantartás vagy visszaírási szükséges-e. 

### <a name="real-time-analysis"></a>Valós idejű elemzés
Stream Analytics SQL-lekérdezés a következő hello használt összes hello átlaga tooget hello például vehicle sebessége, üzemanyag szint, motor, kilométer olvasási, kulcsszava nyomás, motor olaj szint és más fontos vehicle paraméterek. hello átlagok használt toodetect rendellenességek észlelését, ki riasztásokat, illetve meghatározni hello járművekről gyűjtött adott régióban üzemeltetett feltételeinek általános állapotát, és toodemographics összefüggéseket. 

![A valós idejű feldolgozással Stream Analytics-lekérdezés](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

*13. ábra: valós idejű feldolgozással Stream Analytics-lekérdezés*

Minden hello átlagok kiszámítása a 3-második TumblingWindow keresztül. Használjuk TubmlingWindow ebben az esetben óta mozaikként, átfedés nélkül és összefüggő időközök szükségesek. 

További információ az Azure Stream Analytics összes hello "Ablakozó" képességei toolearn kattintson [Ablakozó (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Valós idejű előrejelzés**

Az alkalmazás megtalálható részeként hello megoldás toooperationalize hello gépi tanulási modell valós időben. A "RealTimeDashboardApp" nevű alkalmazás létrehozásáról és hello megoldás központi telepítésének részeként. hello alkalmazás hello következőket végzi:

1. Tooan Eseményközpont példány ahol Stream Analytics közzétesz hello események bankkártyaszám folyamatosan figyeli. ![Stream Analytics lekérdezési hello adatok közzétételére vonatkozó](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *14. ábra: a Stream Analytics lekérdezési hello adatok tooan közzététel Event Hub-példány kimeneti* 
2. Minden eseményhez, amely megkapja ezt az alkalmazást: 
   
   * Folyamatok hello adatok használata a Machine Learning kérés-válasz pontozási (RR-EKET) végpont. hello RR-EKET végpontot automatikusan közzétesz hello központi telepítésének részeként.
   * hello RR-EKET eredménye a közzétett tooa Power BI dataset hello leküldéses API-k használatával.

Ebben a mintában esetében is alkalmazandó tooscenarios használni kívánt toointegrate egy üzletági (LoB) alkalmazás hello valós idejű elemzési folyamat, például a riasztások, értesítések és üzenetkezelési forgatókönyvek esetén.

Kattintson a [RealtimeDashboardApp letöltési](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello RealtimeDashboardApp Visual Studio megoldás testreszabásokat. 

**valós idejű irányítópulton alkalmazás tooexecute hello**
1. Bontsa ki és mentse helyileg ![RealtimeDashboardApp mappa](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *16. ábra – RealtimeDashboardApp mappa*  
2. Hello alkalmazás RealtimeDashboardApp.exe végrehajtása
3. Adjon meg érvényes Power BI hitelesítő adatokat, jelentkezzen be, és kattintson az elfogadás ![Valós idejű irányítópulton app bejelentkezési tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Valós idejű irányítópult-alkalmazás tooPower BI-bejelentkezés befejezése](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*17. ábra – RealtimeDashboardApp: Bejelentkezési tooPower BI*

>[!NOTE] 
>Ha azt szeretné, hogy tooflush hello Power BI dataset, vezető hello RealtimeDashboardApp hello "flushdata" paraméterrel: 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Kötegelt elemzés
hello itt célja hogyan Contoso motorok tooshow hello Azure számítási képességei tooharness big Data típusú adatok toogain részletes információkat a mintát, a használati működésének és a vehicle állapotát befolyásoló tényezők használja. Ez lehetővé teszi:

* Hello felhasználói élmény javításához, és teszi olcsóbb szokásait és üzemanyag hatékony vezetői viselkedések vezetői szóló insights megadásával
* Proaktív információ az ügyfelek és a vezetői patters toogovern üzleti döntéseket hozhat és hello legjobb biztosított osztály termékek és szolgáltatások

Ebben a megoldásban azt céloz meg a következő metrikák hello:

1. **Agresszív viselkedését befolyásoló tényezők**: hello trend hello modellek, helyek, vezetői feltételek és hello év toogain áttekinthetik a agresszív vezetői minták idején azonosítja. Contoso motor ezeket insights segítségével marketingkampányok, új személyre szabott szolgáltatásokat és a használat alapú biztosítási vezetői.
2. **Hatékony vezetői viselkedés üzemanyag-**: hello trend hello modellek, helyek, vezetői feltételek és hello év toogain áttekinthetik a üzemanyag hatékony vezetői minták idején azonosítja. Contoso motorok használhatja ezeket insights marketingkampányok, ki irányítja az új funkciók, és proaktív jelentéskészítési toohello illesztőprogramjait hatályos és a környezet rövid vezetői szokásait költség. 
3. **Modellek visszahívása**: visszahívások igénylő által végrehajtott hello anomáliadetektálási észlelési gépi tanulási kísérlet modelljeit azonosítja

Nézzük meg az egyes ezeket a mérési hello részleteinek

**Agresszív vezetői minta**

hello particionálva vehicle jelek és diagnosztikai adatok feldolgozásának Hive toodetermine hello modellek, a hely, a vehicle használatával "AggresiveDrivingPatternPipeline" nevű hello folyamat befolyásoló tényezők feltételeket, és más paramétereket, amelyeket mutat agresszív vezetői mintát.

![Agresszív vezetői minta munkafolyamat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*. ábra 18 – agresszív vezetői minta munkafolyamat*


***Agresszív vezetői minta Hive-lekérdezések***

hello Hive parancsfájl agresszív vezetői feltétel mintát elemzéséhez használt "aggresivedriving.hql" nevű, "\demo\src\connectedcar\scripts" mappában található hello letöltött zip helyezkedik el. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


Akkor használja a hello vehicle tartozó átviteli fogaskerék pozíció, berendezés tartásához lehetőleg keveset állapotát, és kombinációja sebesség toodetect reckless/agresszív befolyásoló tényezők viselkedését fékezés nagy sebességű minta alapján. 

Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.

![AggressiveDrivingPatternPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

*19. ábra – AggressiveDrivingPatternPipeline kimeneti*

**Üzemanyag hatékony vezetői minta**

hello particionálva vehicle jelek és diagnosztikai adatok hello adatcsatorna "FuelEfficientDrivingPatternPipeline" nevű dolgozza fel. Hive használt toodetermine hello modellek, hely, vehicle, üzemi, de más tulajdonságok, amelyeket üzemanyag hatékony vezetői mintát.

![Tíz vezetői minta](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*20. ábra – tíz vezetői minta munkafolyamat*

***Üzemanyag hatékony vezetői minta Hive lekérdezés***

hello Hive parancsfájl agresszív vezetői feltétel mintát elemzéséhez használt "fuelefficientdriving.hql" nevű, "\demo\src\connectedcar\scripts" mappában található hello letöltött zip helyezkedik el. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Vehicle tartozó átviteli fogaskerék pozícióját, berendezés tartásához lehetőleg keveset állapota, sebesség és gyorsító tartásához lehetőleg keveset pozíció toodetect üzemanyag hatékony hajtott viselkedés alapján fékezés, a gyorsítás hello kombinációja használ, és minták sebessége. 

Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.

![FuelEfficientDrivingPatternPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*21. ábra – FuelEfficientDrivingPatternPipeline kimeneti*

**Előrejelzés visszahívása**

hello gépi tanulási kísérlet kiosztásakor és közzétett webszolgáltatásként hello megoldás központi telepítésének részeként. hello kötegelt pontozási végpont a rendszer a munkafolyamatban, a data factory kapcsolt szolgáltatásként regisztrált és operationalized használatával a data factory kötegelt pontozási tevékenység elkészítéséhez használja.

![Machine Learning-végpont](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

*A data factory kapcsolt szolgáltatásként regisztrált 22. ábra – gépi tanulási végpont*

hello regisztrált kapcsolódó szolgáltatás szerepel hello DetectAnomalyPipeline tooscore hello adatok hello anomáliadetektálási modell használatával. 

![Számítógép-tanulás kötegelt pontozási tevékenység adat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

*23. ábra – a data factory Azure Machine Learning kötegelt pontozási tevékenység* 

Az adatok előkészítése az adatcsatorna végre, hogy a kötegelt pontozás webszolgáltatás hello is operationalized néhány lépésből áll. 

![Járművekről gyűjtött visszahívások igénylő előrejelzésére DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

*24. ábra – járművekről gyűjtött visszahívások igénylő előrejelzésére DetectAnomalyPipeline* 

***Az anomáliadetektálási észlelési Hive-lekérdezések***

Ha hello pontozási befejeződött, egy HDInsight tevékenysége használt tooprocess és hello modell valószínűségét jelző pontszámot 0,60 vagy nagyobb a rendellenességeket eszközként besorolt összesített hello adatok.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.

![DetectAnomalyPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*25. ábra – DetectAnomalyPipeline kimeneti*

## <a name="publish"></a>Közzététel

### <a name="real-time-analysis"></a>Valós idejű elemzés
A Stream Analytics-feladat hello hello lekérdezések egyik hello események tooan kimeneti tesz közzé az Event Hubs-példány. 

![A Stream Analytics-feladat közzéteszi tooan kimeneti Event Hub-példány](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*26. ábra – Stream Analytics-feladat közzéteszi tooan kimeneti Event Hub-példány*

![Stream Analytics lekérdezési toopublish toohello kimeneti Event Hub-példány](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*27. ábra – a Stream Analytics lekérdezési toopublish toohello kimeneti Event Hub-példány*

Ez az adatfolyam események RealTimeDashboardApp hello megoldásban szereplő hello fel. Ez az alkalmazás kihasználja a Machine Learning kérés-válasz webszolgáltatás hello valós idejű pontozó, és közzéteszi a hello eredő adatok tooa Power BI dataset felhasználásra. 

### <a name="batch-analysis"></a>Kötegelt elemzés
hello hello kötegelt és valós idejű feldolgozással eredményei közzétett toohello Azure SQL Database táblák felhasználásra. hello Azure SQL Server adatbázis és hello táblázatok jönnek létre automatikusan hello telepítési parancsfájl részeként. 

![Kötegelt eredmények másolása toodata adatközpont munkafolyamat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*28. ábra – kötegfeldolgozási eredmények másolás toodata adatközpont munkafolyamat*

![A Stream Analytics-feladat közzéteszi toodata adatközpont](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*29. ábra – Stream Analytics-feladat közzéteszi toodata adatközpont*

![A Stream Analytics-feladatban Data adatközpont beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*30. ábra – a Stream Analytics-feladat beállítása adatpiac*

## <a name="consume"></a>Felhasználás
A Power BI ad a megoldás részletes irányítópult valós idejű adatok és a prediktív elemzés képi megjelenítések. 

Kattintson ide a hello Power BI-jelentéseket és hello irányítópult beállításával kapcsolatos részletes információkra van szüksége. hello végső irányítópult így néz ki:

![A Power BI-irányítópulton](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

*Ábra 31 - Power BI-irányítópulton*

## <a name="summary"></a>Összefoglalás
Ez a dokumentum a Vehicle Telemetriai elemzési megoldások hello részletes Lehatolás tartalmaz. Ez egy lambda architektúra mintát bővíthető valós idejű és kötegelt elemzés előrejelzéseket és műveletek. Ebben a mintában vonatkozik tooa azokat a gyakran használt adatok elérési útja (valós időben) igénylő használati esetek és az elemzések cold elérési útja (kötegelt). 

