---
title: "aaaCreate/ütemezés folyamatok, a lánc tevékenységek adat-előállítóban |} Microsoft Docs"
description: "Ismerje meg, egy data folyamatot, az Azure Data Factory toomove toocreate és az adatok átalakítása. Hozzon létre egy adatvezérelt munkafolyamat tooproduce készen toouse adatait."
keywords: "adatok sorban, adatvezérelt munkafolyamat"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 13b137c7-1033-406f-aea7-b66f25b313c0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: shlo
ms.openlocfilehash: 4a0fc20f98ce6453c16955e97fddb891926c173a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>Adatcsatornák és az Azure Data Factory tevékenységek
Ez a cikk segítséget nyújt a folyamatok és a tevékenységek az Azure Data Factoryben, amelyekkel tooconstruct végpont adatvezérelt munkafolyamatok az adatmozgás és adatfeldolgozási forgatókönyveket.  

> [!NOTE]
> Ez a cikk feltételezi, hogy elvégezték- [Data Factory bemutatása tooAzure](data-factory-introduction.md). Nincs hands-a-élmény az adat-előállítók létrehozása, ha áthaladás [data transformation oktatóanyag](data-factory-build-your-first-pipeline.md) és/vagy [adatok mozgása oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) jobban megtudhatja, hogy ez a cikk segít.  

## <a name="overview"></a>Áttekintés
A data factory egy vagy több folyamattal rendelkezhet. A folyamatok olyan tevékenységek logikus csoportosításai, amelyek együttesen vesznek részt egy feladat végrehajtásában. Egy adatcsatorna tevékenységeinek hello műveletek tooperform határozza meg az adatokat. Például előfordulhat, hogy használja a másolási tevékenység toocopy adatokat egy helyi SQL Server tooan Azure Blob Storage. Ezután használja egy Hive Hive parancsfájlok futtatására szolgál egy Azure HDInsight fürt tooprocess/átalakítási adatok hello blob storage tooproduce kimeneti adatokat a tevékenységen. Végezetül használjon egy második másolási tevékenység toocopy hello kimeneti adatok tooan Azure SQL Data Warehouse fölött mely üzleti intelligenciával jelentéskészítési megoldások épülnek. 

Minden tevékenység nulla vagy több bemeneti [adatkészletet](data-factory-create-datasets.md) képes fogadni, és egy vagy több kimeneti [adatkészletet](data-factory-create-datasets.md) képes előállítani. hello következő diagramon láthatók folyamatot, a tevékenység és a dataset hello kapcsolatát az adat-előállító: 

![Feldolgozási sor, a tevékenység és a dataset közötti kapcsolat](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

Egy folyamat lehetővé teszi a toomanage tevékenységek helyett minden egyes készletként külön-külön. Például telepítheti, ütemezhet, felfüggeszteni, és egy sorban, egymástól függetlenül hello adatcsatorna tevékenységeinek foglalkozó helyett folytatni.

A Data Factory két típusú tevékenységet támogat: az adattovábbítási tevékenységeket és az adatátalakítási tevékenységeket. Minden tevékenység állhat nulla vagy több bemeneti [adatkészletek](data-factory-create-datasets.md) , majd előállítanak egy vagy több kimeneti adatkészletek.

Egy bemeneti adatkészlet hello feldolgozási soros tevékenység hello a megadott és egy kimeneti adatkészlet hello kimeneti hello tevékenység jelenti. Az adatkészletek adatokat határoznak meg a különböző adattárakban, például táblákban, fájlokban, mappákban és dokumentumokban. Az adatkészlet létrehozását követően használhatja azt egy folyamat tevékenységei esetében. Az adatkészletek lehetnek például egy másolási tevékenység vagy egy HDInsightHive-tevékenység be- vagy kimeneti adatkészletei. Az adatkészletekről további információkat [Az Azure Data Factory adatkészletei](data-factory-create-datasets.md) cikkben talál.

### <a name="data-movement-activities"></a>Adattovábbítási tevékenységek
A Data Factory másolási tevékenység egy forrás adatokat tároló tooa fogadó adatokat tároló másol adatokat. Adat-előállítót a következő adatokat tárolja hello támogatja. Bármely forrásból származó adatok csak írható tooany fogadó. Hogyan kattintson egy adatokat tároló toolearn toocopy adatok tooand adott áruházból.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Adatokat tárolja, a * lehet helyszíni vagy Azure IaaS és használatba tooinstall [az adatkezelési átjáró](data-factory-data-management-gateway.md) a-hez vagy az Azure infrastruktúra-szolgáltatási gépen.

További információkért tekintse meg az [adattovábbítási tevékenységekről](data-factory-data-movement-activities.md) szóló cikket.

### <a name="data-transformation-activities"></a>Adatátalakítási tevékenységek
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

További információkért tekintse meg az [adatátalakítási tevékenységekről](data-factory-data-transformation-activities.md) szóló cikket.

### <a name="custom-net-activities"></a>Egyéni .NET-tevékenységek 
Ha szüksége van a toomove adatok/egy adatokból futó hello másolási tevékenység nem támogatja, vagy a saját logikát használó adatok átalakítása, hozzon létre egy **egyéni .NET tevékenység**. További információ az egyéni tevékenységek létrehozásával és használatával kapcsolatban: [Egyéni tevékenységek használata Azure Data Factory-folyamatban](data-factory-use-custom-activities.md).

## <a name="schedule-pipelines"></a>Ütemezés folyamatok
Egy folyamat aktív csak között a **start** idő és **end** idő. Nem hajtotta végre, hello kezdete előtt vagy után hello befejezési időpontja. Hello csővezeték fel van függesztve, ha azt nem azonnal végrehajtásra kerülhetnek a kezdő és záró idő függetlenül. Az a folyamat toorun azt kell nem lehet szüneteltetni. Lásd: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) toounderstand az Azure Data Factory ütemezés és a végrehajtási működése.

## <a name="pipeline-json"></a>A folyamat JSON-fájlja
Nézzük meg közelebbről, hogyan történik egy folyamat JSON-formátumban való meghatározása. hello általános struktúra folyamat a következőképpen néz ki:

```json
{
    "name": "PipelineName",
    "properties": 
    {
        "description" : "pipeline description",
        "activities":
        [

        ],
        "start": "<start date-time>",
        "end": "<end date-time>",
        "isPaused": true/false,
        "pipelineMode": "scheduled/onetime",
        "expirationTime": "15.00:00:00",
        "datasets": 
        [
        ]
    }
}
```

| Címke | Leírás | Szükséges |
| --- | --- | --- |
| név |Hello folyamat nevét. Adjon meg egy nevet, amely jelöli, hogy az adatcsatorna hello hello műveletet hajt végre. <br/><ul><li>A karakterek maximális száma: 260</li><li>Betűvel, számmal vagy aláhúzásjellel (_) kell kezdődnie</li><li>Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Igen |
| leírás | Adja meg, milyen hello csővezeték használt leíró hello szöveg. |Igen |
| tevékenységek | Hello **tevékenységek** szakasz rendelkezhet egy vagy több tevékenységet azt vannak meghatározva. Hello hello tevékenységek JSON elem részleteit a következő szakaszban talál. | Igen |  
| start | Kezdő dátum-idő hello adatcsatorna. Meg kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601). Például: `2016-10-14T16:32:41Z`. <br/><br/>Már lehetséges toospecify a helyi időt, például egy keleti TÉLI idő. Példa: `2016-02-27T06:00:00-05:00`", amely 6 AM becsült<br/><br/>hello kezdő és záró együtt adjon hello adatcsatorna aktív időszakát. Kimeneti szeletek csak előállítása és az aktív időszakban. |Nem<br/><br/>Ha hello end tulajdonság értékét adja meg, meg kell adnia hello start tulajdonság értéke.<br/><br/>hello indításának és befejezésének idejét is lehet üres toocreate folyamat. Mindkét értéket meg kell adni az aktív időszak hello csővezeték toorun tooset. Ha nem adja meg a kezdési és befejezési időpontjai folyamat létrehozásakor beállíthatja azokat később hello Set-AzureRmDataFactoryPipelineActivePeriod parancsmag használatával. |
| Vége | Befejező dátum idejű hello adatcsatorna. Ha a megadott ISO-formátumban kell lennie. Például:`2016-10-14T17:32:41Z` <br/><br/>Már lehetséges toospecify a helyi időt, például egy keleti TÉLI idő. Példa: `2016-02-27T06:00:00-05:00`, vagyis 6 AM becsült<br/><br/>toorun hello folyamat határozatlan ideig, adja meg 9999-09-09 hello hello end tulajdonság értékét. <br/><br/> Egy folyamat aktív csak a kezdési és befejezési időpontja között. Nem hajtotta végre, hello kezdete előtt vagy után hello befejezési időpontja. Hello csővezeték fel van függesztve, ha azt nem azonnal végrehajtásra kerülhetnek a kezdő és záró idő függetlenül. Az a folyamat toorun azt kell nem lehet szüneteltetni. Lásd: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) toounderstand az Azure Data Factory ütemezés és a végrehajtási működése. |Nem <br/><br/>Ha hello start tulajdonság értékét adja meg, meg kell adnia hello end tulajdonság értéke.<br/><br/>Lásd: a Megjegyzések a hello **start** tulajdonság. |
| isPaused | Ha a készlet tootrue, hello folyamat nem futtatható. A hello szünetel állapotát. Alapértelmezett érték = false. Ez a tulajdonság tooenable használja, vagy tiltsa le a folyamat. |Nem |
| pipelineMode | hello ütemezési módszer hello adatcsatorna fut. Két érték engedélyezett: (alapértelmezett), ütemezett alkalommal.<br/><br/>"Ütemezett" azt jelzi, hogy hello adatcsatorna tooits aktív időszaka (kezdő és záró idő) alapján meghatározott időközönként. "Alkalommal" azt jelzi, hogy hello folyamat csak egyszer fut le. Létrehozását követően alkalommal adatcsatornák nem lehet módosítani/frissített jelenleg. Lásd: [Onetime csővezeték](#onetime-pipeline) alkalommal beállítás vonatkozó további információért. |Nem |
| ExpirationTime | Mely hello a létrehozást követően időtartam [egyszeri folyamat](#onetime-pipeline) érvényes és kiépített kell maradnia. Ha nem rendelkezik minden aktív sikertelen volt, vagy fut, függőben lévő hello folyamat automatikusan törlése után lehet spórolni hello lejárati ideje. hello alapértelmezett érték:`"expirationTime": "3.00:00:00"`|Nem |
| Adatkészletek |Adatkészletek toobe hello az adatcsatornában definiált tevékenységeket használja listáját. Ez a tulajdonság foglalt toodefine adatkészleteket, amelyek adott toothis folyamat, és nem meghatározott hello adat-előállító lehet. Ez az adatcsatorna meghatározott adatkészleteket csak ez az adatcsatorna használható, és nem osztható meg. Lásd: [adathalmaz hatóköre](data-factory-create-datasets.md#scoped-datasets) részleteiről. |Nem |

## <a name="activity-json"></a>Tevékenység JSON-fájlja
Hello **tevékenységek** szakasz rendelkezhet egy vagy több tevékenységet azt vannak meghatározva. Minden tevékenység rendelkezik a legfelső szintű struktúra a következő hello:

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    },
    "scheduler":
    {
    }
}
```

Következő táblázat írja le tulajdonságok hello tevékenység JSON-definícióból:

| Címke | Leírás | Szükséges |
| --- | --- | --- |
| név | Hello tevékenység nevét. Adjon meg egy nevet, amely hello tevékenység hajt végre műveletet hello jelenti. <br/><ul><li>A karakterek maximális száma: 260</li><li>Betűvel, számmal vagy aláhúzásjellel (_) kell kezdődnie</li><li>Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Igen |
| leírás | Milyen tevékenység hello vagy használt leíró szöveg |Igen |
| type | Hello tevékenység típusa. Lásd: hello [adatok mozgása tevékenységek](#data-movement-activities) és [adatok átalakítása tevékenységek](#data-transformation-activities) szakaszok a tevékenységek különböző típusú. |Igen |
| Bemenetek |Hello tevékenység által felhasznált bemeneti táblák<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Igen |
| kimenetek |Hello tevékenység által használt kimeneti táblák.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Igen |
| linkedServiceName |Hello tevékenység által használt hello társított szolgáltatás neve. <br/><br/>Egy tevékenység lehet szükség, hogy megadja a kapcsolódó hello szolgáltatást, amely a toohello szükséges számítási környezet. |Igen, a HDInsight tevékenységet és az Azure Machine Learning kötegelt pontozási tevékenység <br/><br/>Minden egyéb esetében: nem |
| typeProperties |Hello tulajdonságok **typeProperties** szakasz hello tevékenység-típustól függnek. egy tevékenység toosee típus tulajdonságainak hivatkozások toohello tevékenység hello előző szakaszban kattintson. | Nem |
| szabályzat |Hello tevékenység hello futásidejű működését befolyásoló házirendek. Ha nincs megadva, az alapértelmezett házirendek használhatók. |Nem |
| A Feladatütemező | "Feladatütemező" tulajdonság hello tevékenység ütemezés használt toodefine szükséges. A altulajdonságok vannak hello megegyeznek a hello azokat a hello [availability tulajdonság DataSet adatkészletben](data-factory-create-datasets.md#dataset-availability). |Nem |


### <a name="policies"></a>Házirendek
A házirendek milyen hatással hello futtatási viselkedés tevékenység, kifejezetten egy tábla hello szelet feldolgozásakor. a következő táblázat hello hello részletes adatokat biztosít.

| Tulajdonság | Megengedett értékek | Alapértelmezett érték | Leírás |
| --- | --- | --- | --- |
| Egyidejűségi |Egész szám <br/><br/>A maximális érték: 10 |1 |Egyidejű végrehajtások hello tevékenység száma.<br/><br/>Meghatározza, hogy hello száma párhuzamos tevékenység végrehajtások, amely akkor fordulhat elő, a másik szeletek. Például ha egy tevékenység toogo keresztül elérhető adatokat, nagyobb feldolgozási értéke számos felgyorsítja a hello adatok feldolgozása. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Meghatározza, hogy hello sorrendje feldolgozott adatszeletek.<br/><br/>Például ha 2 szeletek (du. 4: egy azonban és délután 5 óra egy másik tulajdonságnak), és mindkét végrehajtási függőben van. Ha hello executionPriorityOrder toobe NewestFirst, hello szelet, délután 5 óra lesz elsőként feldolgozva. Hasonlóképpen ha hello executionPriorityORder toobe OldestFIrst, majd du. 4: hello szelet feldolgozása. |
| retry |Egész szám<br/><br/>A maximális érték 10 is lehet. |0 |Hello adatfeldolgozási hello adatszelethez előtt újrapróbálkozások száma hiba van megjelölve. Egy adatszelet tevékenység végrehajtása a rendszer ismét megkísérli megadott toohello mentése újrapróbálkozások száma. hello újrapróbálkozási minél hamarabb hello meghibásodás után történik. |
| timeout |A TimeSpan |00:00:00 |Hello tevékenység időkorlátját. . Példa: 00:10:00 (azt jelenti, időtúllépés 10 perc)<br/><br/>Ha az érték nincs megadva vagy 0, hello időtúllépési érték végtelen.<br/><br/>Szelet hello adatok feldolgozási ideje meghaladja a hello időtúllépési értéket, ha azt megszakadt, és hello rendszer próbál tooretry hello feldolgozása. Az újrapróbálkozások számát hello hello újrapróbálkozási tulajdonság függ. Időtúllépés esetén hello beállítás tooTimedOut. |
| Késleltetés |A TimeSpan |00:00:00 |Adja meg a hello késleltetés hello szelet elindítja az adatok feldolgozása előtt.<br/><br/>egy adatszelet tevékenységet hello végrehajtása után hello késleltetés hello várt végrehajtási ideje elmúlt elindult.<br/><br/>. Példa: 00:10:00 (magában foglalja a késleltetést a 10 perc) |
| hosszú újrapróbálkozás |Egész szám<br/><br/>A maximális érték: 10 |1 |hosszú újrapróbálkozások számát hello tett kísérletet, mielőtt hello szelet végrehajtása sikertelen volt.<br/><br/>hosszú újrapróbálkozás kísérletek által longRetryInterval távolságban helyezkednek el. Ha toospecify kell egy újrapróbálkozási kísérletek között eltelt idő, így hosszú újrapróbálkozás használja. Ha mind az újra gombra, és a hosszú újrapróbálkozás meg van adva, minden hosszú újrapróbálkozás kísérlet tartalmazza az ismételt kísérletek számát, és hello kísérletek maximális száma újrapróbálkozási * hosszú újrapróbálkozás.<br/><br/>Ha például tudunk hello hello tevékenység házirend beállításai a következő:<br/>Próbálkozzon újra: 3<br/>hosszú újrapróbálkozás: 2. régiója<br/>longRetryInterval: 01:00:00<br/><br/>Feltételezik, hogy csak egy szelet tooexecute (állapot vár) hello tevékenység végrehajtási minden olyan alkalommal sikertelen lesz. Eredetileg nem lenne 3 egymást követő végrehajtási kísérletek. Minden kísérlet után hello szelet állapota lenne az újra gombra. Miután először 3 kísérletet: keresztül, hello szelet állapota hosszú újrapróbálkozás lesz.<br/><br/>Egy óra (Ez azt jelenti, hogy longRetryInteval tartozó érték) nem lenne a 3 egymást követő végrehajtási kísérletek egy másik készlet. Ezt követően hello szelet állapota akkor sikertelen, és nincs további újrapróbálkozások volna kísérli meg. Ezért a teljes 6 történt kísérlet.<br/><br/>Ha bármely végrehajtása sikeres, hello szelet állapota Kész és nincs további újrapróbálkozások próbált vannak.<br/><br/>hosszú újrapróbálkozás függő adatok nem determinisztikus időpontokban érkeznek vagy hello teljes környezete alapján akkor következik be, mely az adatfeldolgozás flaky használható. Ezekben az esetekben nem újrapróbálkozások egymás után ez segíthet, és kimeneti így időt eredményez hello időköz után szükséges.<br/><br/>Járjon el a Word: nincs beállítva hosszú újrapróbálkozás vagy longRetryInterval magas értékeit. Általában a magasabb értékkel rendszeres problémákkal utalnak. |
| longRetryInterval |A TimeSpan |00:00:00 |hello kísérletek hosszú újrapróbálkozási kísérletek között eltelt idő |

## <a name="sample-copy-pipeline"></a>Minta másolási folyamat
A következő minta csővezeték hello, nincs típusú egy tevékenység **másolása** a hello **tevékenységek** szakasz. Ez a példa hello [másolási tevékenység](data-factory-data-movement-activities.md) másol adatokat az Azure Blob storage tooan Azure SQL-adatbázis. 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
} 
```

Vegye figyelembe a következő pontok hello:

* Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**.
* Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**. Az adatkészletek JSON-fáljban történő meghatározását lásd az [Adatkészletek](data-factory-create-datasets.md) cikket. 
* A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva. A hello [adatok mozgása tevékenységek](#data-movement-activities) területen kattintson az adatok tárolásához, amelyet az toouse a forrás vagy a fogadó toolearn kapcsolatos adatmozgatás a tároló és a további hello. 

Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Minta átalakítási folyamat
A következő minta csővezeték hello, nincs típusú egy tevékenység **HDInsightHive** a hello **tevékenységek** szakasz. Ez a példa hello [HDInsight Hive tevékenység](data-factory-hive-activity.md) átalakítja az adatokat a egy Azure Blob storage egy Azure HDInsight Hadoop-fürt Hive parancsfájl futtatásával. 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00Z",
        "end": "2016-04-02T00:00:00Z",
        "isPaused": false
    }
}
```

Vegye figyelembe a következő pontok hello: 

* Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**HDInsightHive**.
* hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **AzureStorageLinkedService**), majd a  **parancsfájl** hello tároló mappa **adfgetstarted**.
* Hello `defines` szakaszban használt toospecify hello futtatási beállítások Hive értékként toohello hive parancsfájl átadott (például `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Hello **typeProperties** szakasz különbözik a átalakítása tevékenységeiről. toolearn típusú tulajdonságok támogatott átalakítása tevékenységhez, kattintson a hello átalakítása tevékenység hello [adatok átalakítása tevékenységek](#data-transformation-activities) tábla. 

Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: az első adatcsatorna tooprocess adatok Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md). 

## <a name="multiple-activities-in-a-pipeline"></a>Több tevékenység egy adott folyamatban
hello előző két minta adatcsatornák csak egy tevékenység rendelkezik rajtuk. Egy folyamathoz azonban több tevékenység is tartozhat.  

Ha több tevékenység rendelkezik egy folyamatot, és egy tevékenység kimenete nem bemenet egy másik tevékenység, hello tevékenységek párhuzamos futtathatja, ha készen áll a bemeneti adatok szeletek hello tevékenységekhez. 

Akkor is láncolt két tevékenység hello kimeneti adatkészlet egy bemeneti adatkészlet hello hello a tevékenységet, hogy más tevékenység. hello második tevékenység végrehajtása csak akkor, ha hello először egy sikeresen befejeződik.

![Láncolás hello tevékenységek ugyanazt a következő feldolgozási sorban](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Ez a példa hello folyamat két tevékenység rendelkezik: Activity1 és az Activity2. hello Activity1 bemenetként Dataset1 vesz igénybe, és egy kimenetet Dataset2. hello tevékenység bemeneti adatokként Dataset2 vesz igénybe, és egy kimenetet Dataset3. Az Activity1 hello kimeneti óta (Dataset2) Activity2 hello bemeneti, hello Activity2 futtat csak hello tevékenység sikeresen befejeződik, és előállított hello Dataset2 szelet. Ha hello Activity1 valamilyen okból sikertelen, és nem hozhatók létre hello Dataset2 szelet, hello 2. tevékenység nem működik az, hogy a szelet (például: 9 AM too10 VAGYOK). 

Tevékenységek más folyamatok láncában is találhatók.

![Két kimenetátirányítási titkosításblokkoló tevékenységek](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Ez a példa Pipeline1 Dataset1 bemenetként veszi, és hozza létre a kimenetként Dataset2 csak egy tevékenység rendelkezik. hello Pipeline2 is rendelkezik, amely Dataset2 bemeneti és kimenetként Dataset3 csak egy tevékenységgel. 

További információkért lásd: [ütemezés és a végrehajtási](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="create-and-monitor-pipelines"></a>Hozzon létre és folyamatok figyelése
Folyamatok valamelyikének ezen eszközök vagy az SDK-k használatával hozhat létre. 

- Másolása varázsló. 
- Azure Portal
- Visual Studio
- Azure PowerShell
- Azure Resource Manager-sablon
- REST API
- .NET API

Tekintse meg a következő folyamatok egyikével a következő eszközök, illetve az SDK-k létrehozására vonatkozó részletes utasításokat oktatóanyagok hello.
 
- [Adatátalakítási tevékenységgel rendelkező folyamat létrehozása](data-factory-build-your-first-pipeline.md)
- [Adatok mozgása tevékenységgel folyamat létrehozása](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Ha egy folyamatot, akkor létre/telepített, felügyelheti és figyelheti a folyamatok használatával hello Azure portál paneleken vagy a figyelő és App kezelése. Tekintse meg a következő témakörök részletes utasításokat hello. 

- [Figyelheti és folyamatok kezelése az Azure portál panel segítségével](data-factory-monitor-manage-pipelines.md).
- [Figyelheti és kezelheti a folyamatok figyelése és kezelése App használatával](data-factory-monitor-manage-app.md)


## <a name="onetime-pipeline"></a>Alkalommal folyamat
Hozzon létre, és rendszeres időközönként ütemezni a feldolgozási sor toorun (például: óránként vagy naponta) belül hello kezdési és befejezési időpontja, hello csővezeték-definíció adható meg. Lásd: [tevékenységek ütemezése](#scheduling-and-execution) részleteiről. Létrehozhat egy folyamatot, amely csak egyszer fut le. toodo Igen, állítsa be az hello **pipelineMode** hello tulajdonsága túl csővezeték-definíció**alkalommal** hello JSON-mintát a következő ábrán. hello alapértelmezett érték a tulajdonság **ütemezett**.

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ]
                "name": "CopyActivity-0"
            }
        ]
        "pipelineMode": "OneTime"
    }
}
```

Vegye figyelembe a következőket hello:

* **Start** és **end** alkalommal hello adatcsatornához nincs megadva.
* **Rendelkezésre állási** bemeneti és kimeneti adatkészletek van megadva (**gyakoriság** és **időköz**), annak ellenére, hogy a Data Factory nem hello értékeket használja.  
* Diagram nézet nem szerepelnek a egyszeri folyamatok. Ez a működésmód szándékos.
* Egyszeri adatcsatornák nem lehet frissíteni. Klónozni egy egyszeri folyamat, nevezze át, tulajdonságainak frissítése, és telepítheti azt toocreate egy másikat.


## <a name="next-steps"></a>Következő lépések
- Adatkészletek kapcsolatos további információkért lásd: [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. 
- Hogyan adatcsatornák ütemezett és végrehajtott kapcsolatos további információkért lásd: [ütemezés és a végrehajtása az Azure Data Factory](data-factory-scheduling-and-execution.md) cikk. 
  

