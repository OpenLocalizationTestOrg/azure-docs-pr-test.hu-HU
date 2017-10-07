---
title: "aaaAzure Data Factory - JSON-parancsfájl-kezelési referencia |} Microsoft Docs"
description: "JSON-sémákat biztosít a Data Factory-entitásokhoz."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 813fd752bb0ecb1b513d022b9f302325105dac31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a>Az Azure Data Factory - JSON-Parancsprogramokról
A cikkben JSON-sémákat és példák meghatározásához az Azure Data Factory entitások (adatcsatorna, tevékenység, adatkészlet és társított szolgáltatás).  

## <a name="pipeline"></a>Folyamat 
hello magas szintű struktúra folyamat meghatározása a következőképpen történik: 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Következő táblázat hello adatcsatorna JSON-definícióból hello tulajdonságokat:

| Tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
| név | Hello folyamat nevét. Adjon meg egy nevet, amely jelöli, hogy a tevékenység hello hello műveletet, vagy csővezeték konfigurált toodo<br/><ul><li>A karakterek maximális száma: 260</li><li>Betűvel, számmal vagy aláhúzásjellel (_) kell kezdődnie</li><li>Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Igen |
| leírás |Milyen hello tevékenység vagy csővezeték leíró szöveg | Nem |
| tevékenységek | A tevékenységek listáját tartalmazza. | Igen |
| start |Kezdő dátum-idő hello adatcsatorna. Meg kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601). Például: 2014-10-14T16:32:41. <br/><br/>Már lehetséges toospecify a helyi időt, például egy keleti TÉLI idő. Példa: `2016-02-27T06:00:00**-05:00`, vagyis 6 AM becsült<br/><br/>hello kezdő és záró együtt adjon hello adatcsatorna aktív időszakát. Kimeneti szeletek csak előállítása és az aktív időszakban. |Nem<br/><br/>Ha hello end tulajdonság értékét adja meg, meg kell adnia hello start tulajdonság értéke.<br/><br/>hello indításának és befejezésének idejét is lehet üres toocreate folyamat. Mindkét értéket meg kell adni az aktív időszak hello csővezeték toorun tooset. Ha nem adja meg a kezdési és befejezési időpontjai folyamat létrehozásakor beállíthatja azokat később hello Set-AzureRmDataFactoryPipelineActivePeriod parancsmag használatával. |
| Vége |Befejező dátum idejű hello adatcsatorna. Ha a megadott ISO-formátumban kell lennie. Például: 2014-10-14T17:32:41 <br/><br/>Már lehetséges toospecify a helyi időt, például egy keleti TÉLI idő. Példa: `2016-02-27T06:00:00**-05:00`, vagyis 6 AM becsült<br/><br/>toorun hello folyamat határozatlan ideig, adja meg 9999-09-09 hello hello end tulajdonság értékét. |Nem <br/><br/>Ha hello start tulajdonság értékét adja meg, meg kell adnia hello end tulajdonság értéke.<br/><br/>Lásd: a Megjegyzések a hello **start** tulajdonság. |
| isPaused |Ha a készlet tootrue hello folyamat nem futtatható. Alapértelmezett érték = false. Ez a tulajdonság tooenable használja, vagy tiltsa le. |Nem |
| pipelineMode |hello ütemezési módszer hello adatcsatorna fut. Két érték engedélyezett: (alapértelmezett), ütemezett alkalommal.<br/><br/>"Ütemezett" azt jelzi, hogy hello adatcsatorna tooits aktív időszaka (kezdő és záró idő) alapján meghatározott időközönként. "Alkalommal" azt jelzi, hogy hello folyamat csak egyszer fut le. Létrehozását követően alkalommal adatcsatornák nem lehet módosítani/frissített jelenleg. Lásd: [Onetime csővezeték](data-factory-create-pipelines.md#onetime-pipeline) alkalommal beállítás vonatkozó további információért. |Nem |
| ExpirationTime |Időtartam, mely hello a feldolgozási sorban lévő érvényes és kiépített maradjon létrehozása után. Nincs minden aktív sikertelen volt, vagy fut, függőben lévő hello feldolgozási sor törlése esetén automatikusan egyszer eléri a hello lejárati ideje. |Nem |


## <a name="activity"></a>Tevékenység 
Az adatcsatorna definícióját (tevékenységek elem) tevékenységet hello magas szintű struktúráját a következőképpen történik:

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
    }
    "scheduler":
    {
    }
}
```

A következő táblázat azt mutatják be hello tevékenység JSON-definícióból hello tulajdonságokat:

| Címke | Leírás | Szükséges |
| --- | --- | --- |
| név |Hello tevékenység nevét. Adja meg egy nevet, hogy hello tevékenység hello műveletet képviselő úgy toodo<br/><ul><li>A karakterek maximális száma: 260</li><li>Betűvel, számmal vagy aláhúzásjellel (_) kell kezdődnie</li><li>Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Igen |
| leírás |Milyen hello tevékenységet leíró szöveg használható. |Igen |
| type |Hello tevékenység hello típusát határozza meg. Lásd: hello [ADATTÁROLÓKHOZ](#data-stores) és [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakaszok a tevékenységek különböző típusú. |Igen |
| Bemenetek |Hello tevékenység által felhasznált bemeneti táblák<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Igen |
| kimenetek |Hello tevékenység által használt kimeneti táblák.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Igen |
| linkedServiceName |Hello tevékenység által használt hello társított szolgáltatás neve. <br/><br/>Egy tevékenység lehet szükség, hogy megadja a kapcsolódó hello szolgáltatást, amely a toohello szükséges számítási környezet. |HDInsight tevékenységek, az Azure Machine Learning tevékenységek és a tárolt eljárási tevékenység igen. <br/><br/>Minden egyéb esetében: nem |
| typeProperties |Hello typeProperties szakaszban tulajdonságok attól függnek, hogy hello tevékenység típusa. |Nem |
| szabályzat |Hello tevékenység hello futásidejű működését befolyásoló házirendek. Ha nincs megadva, az alapértelmezett házirendek használhatók. |Nem |
| A Feladatütemező |"Feladatütemező" tulajdonság hello tevékenység ütemezés használt toodefine szükséges. A altulajdonságok vannak hello megegyeznek a hello azokat a hello [availability tulajdonság DataSet adatkészletben](data-factory-create-datasets.md#dataset-availability). |Nem |

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

### <a name="typeproperties-section"></a>typeProperties szakasz
hello typeProperties szakaszban nem egyezik, minden egyes tevékenységhez. Átalakítás tevékenység csak a hello típus tulajdonságokkal rendelkezik. Lásd: [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakasz ebben a cikkben egy folyamat átalakítása tevékenységek meghatározó JSON-minták. 

**Másolási tevékenység** rendelkezik-e két alszakaszokat hello typeProperties szakasz: **forrás** és **fogadó**. Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz ebben a cikkben a JSON-minták, hogy hogyan toouse egy adatok tárolót, mint a forrás és fogadó vagy megjelenítése. 

### <a name="sample-copy-pipeline"></a>Minta másolási folyamat
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
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Vegye figyelembe a következő pontok hello:

* Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**.
* Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**.
* A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva.

Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz ebben a cikkben a JSON-minták, hogy hogyan toouse egy adatok tárolót, mint a forrás és fogadó vagy megjelenítése.    

Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="sample-transformation-pipeline"></a>Minta átalakítási folyamat
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
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

Vegye figyelembe a következő pontok hello: 

* Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**HDInsightHive**.
* hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **AzureStorageLinkedService**), majd a  **parancsfájl** hello tároló mappa **adfgetstarted**.
* Hello **meghatározása** szakaszban használt toospecify hello futtatási beállítások Hive értékként toohello hive parancsfájl átadott (például `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Lásd: [adatok ÁTALAKÍTÁSA tevékenységek](#data-transformation-activities) szakasz ebben a cikkben egy folyamat átalakítása tevékenységek meghatározó JSON-minták.

Ez az adatcsatorna létrehozásának részletes útmutatást lásd: [oktatóanyag: az első adatcsatorna tooprocess adatok Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md). 

## <a name="linked-service"></a>Társított szolgáltatások
a társított szolgáltatás definíciójának hello magas szintű struktúrája a következőképpen történik:

```json
{
    "name": "<name of hello linked service>",
    "properties": {
        "type": "<type of hello linked service>",
        "typeProperties": {
        }
    }
}
```

A következő táblázat azt mutatják be hello tevékenység JSON-definícióból hello tulajdonságokat:

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| név | Hello társított szolgáltatás neve. | Igen | 
| Tulajdonságok - típus | Hello típusú társított szolgáltatás. Például: az Azure Storage, Azure SQL Database. |
| typeProperties | hello typeProperties szakasz különböző minden adattároló vagy számítási környezet elemeket tartalmaz. Lásd: [adattárolókhoz](#datastores) szakasz az összes hello adattároló társított szolgáltatások és [számítási környezetek](#compute-environments) összes hello a számítási összekapcsolt szolgáltatások |   

## <a name="dataset"></a>Adatkészlet 
Az Azure Data Factory dataset a következők:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

a következő táblázat hello hello fent JSON-tulajdonságokat ismerteti:   

| Tulajdonság | Leírás | Szükséges | Alapértelmezett |
| --- | --- | --- | --- |
| név | Hello DataSet adatkészlet neve. Lásd: [Azure Data Factory - elnevezési szabályait](data-factory-naming-rules.md) elnevezési szabályait. |Igen |NA |
| type | Hello dataset típusa. Adjon meg egy Azure Data Factory által támogatott hello típusok (például: AzureBlob, AzureSqlTable). Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakasz az összes hello adattárolókhoz és a Data Factory által támogatott adatkészlet-típusok. | 
| struktúra | Hello adatkészlet sémája. Tartalmaz oszlopok, azok típusok, stb. | Nem |NA |
| typeProperties | Tulajdonságok toohello megfelelő típust választotta. Lásd: [ADATTÁROLÓKHOZ](#data-stores) szakaszban a támogatott típusok és azok tulajdonságait. |Igen |NA |
| external | Logikai jelző toospecify, hogy a DataSet adatkészlet explicit módon hozzák a data factory-folyamat vagy nem. |Nem |hamis |
| rendelkezésre állás | Feldolgozási időszakát, vagy a felosztás hello dataset üzemi modell hello hello határozza meg. További részletek a felosztás modell hello adatkészlet: [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk. |Igen |NA |
| szabályzat |Hello feltételek vagy hello dataset szeletek kell néhány előfeltételnek hello feltételt határoz meg. <br/><br/>További információkért lásd: [Dataset házirend](#Policy) szakasz. |Nem |NA |

Minden egyes oszlopának hello **struktúra** szakasz hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| név |Hello oszlop neve. |Igen |
| type |Hello oszlop adattípusát.  |Nem |
| Kulturális környezet |.NET-alapú kulturális környezet toobe használható, ha a típus meg van adva, de a .NET-típus `Datetime` vagy `Datetimeoffset`. Alapértelmezett érték a `en-us`. |Nem |
| Formátumban |Formázza használható, ha a típus meg van adva, de .NET típusú karakterlánc toobe `Datetime` vagy `Datetimeoffset`. |Nem |

A következő példa hello, hello dataset adatkészletben három oszlopot `slicetimestamp`, `projectname`, és `pageviews` és típus: karakterlánc, karakterlánc és decimális rendre.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

hello következő táblázat ismerteti a hello használható tulajdonságok **rendelkezésre állási** szakasz:

| Tulajdonság | Leírás | Szükséges | Alapértelmezett |
| --- | --- | --- | --- |
| frequency |Megadja a dataset szelet üzemi hello időegységét.<br/><br/><b>Támogatott gyakoriság</b>: perc, óra, nap, hét, hónap |Igen |NA |
| interval |Megadja egy szorzóval gyakoriság<br/><br/>"X időköz" határozza meg, milyen gyakran hello szelet jön létre.<br/><br/>Ha dataset toobe szeletelhetők óránként kell hello, akkor be <b>gyakoriság</b> túl<b>óra</b>, és <b>időköz</b> túl<b>1</b>.<br/><br/><b>Megjegyzés:</b>: perces gyakoriságot ad meg, ha azt javasoljuk, hogy állítsa a 15-nál kisebb hello időköz toono |Igen |NA |
| stílus |Meghatározza, hogy kell-e hello szelet előállított hello kezdő/záró hello időköz.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Ha gyakoriságának beállítása tooMonth és stílus tooEndOfInterval van beállítva, hello szelet a hónap utolsó napján hello elő. Ha hello stílus be van állítva tooStartOfInterval, hello szelet elő hello a hónap első napján.<br/><br/>Ha gyakoriságának beállítása tooDay és stílus tooEndOfInterval van beállítva, hello szelet elő az elmúlt egy órában hello nap hello.<br/><br/>Ha gyakoriság tooHour és stílus tooEndOfInterval van beállítva, hello szelet hello végének hello keletkezik. Például a szelet du. 1 – 2 óra időszakban, a hello szelet hozzák 2 du.. |Nem |EndOfInterval |
| anchorDateTime |Hello abszolút pozíciója a ennyi másodpercig használta a Feladatütemező toocompute dataset szelet határok meghatározása. <br/><br/><b>Megjegyzés:</b>: Ha hello AnchorDateTime részekből dátum, amelyek részletesebben, mint a hello gyakorisága, akkor hello részletesebb részek figyelmen kívül lesznek hagyva. <br/><br/>Például, ha hello <b>időköz</b> van <b>óránkénti</b> (gyakoriság: óra és időköz: 1) és hello <b>AnchorDateTime</b> tartalmaz <b>percet és másodpercet</b>majd hello <b>percet és másodpercet</b> hello AnchorDateTime részei a rendszer figyelmen kívül hagyja. |Nem |01/01/0001 |
| Az offset |TimeSpan érték, mely hello által kezdő és záró összes adatkészlet szeletek vette. <br/><br/><b>Megjegyzés:</b>: Ha anchorDateTime és az offset is meg van adva, hello eredménye kombinált hello shift. |Nem |NA |

a következő rendelkezésre állással kapcsolatos szakaszának hello Megadja, hogy adott hello kimeneti adatkészlet előállított óránként (vagy) bemeneti adatkészlet óránkénti áll rendelkezésre:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Hello **házirend** hello feltételek rész az adatkészlet-definícióban vagy hello feltétellel, hogy a dataset szeletek hello teljesítenie kell.

| Házirend neve | Leírás | Alkalmazott túl| Szükséges | Alapértelmezett |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Ellenőrzi, hogy hello adatokat egy **Azure blob** megfelel-e hello (megabájtban) a minimális méret követelményeinek. |Azure-blob |Nem |NA |
| minimumRows |Ellenőrzi, hogy hello adatokat egy **Azure SQL adatbázis** vagy egy **Azure-tábla** hello a sorok legkisebb számát tartalmazza. |<ul><li>Azure SQL Database</li><li>Azure-tábla</li></ul> |Nem |NA |

**Példa**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

Azure Data Factory hozzák alatt álló adatkészlet, kivéve azt állapotúként kell megjelölni **külső**. Ez a beállítás toohello bemenet az adatcsatorna első tevékenység általában vonatkozik, kivéve, ha a tevékenység vagy csővezeték-láncolás használatban van.

| Név | Leírás | Szükséges | Alapértelmezett érték |
| --- | --- | --- | --- |
| dataDelay |Idő toodelay hello ellenőrzése hello külső adatok számára megadott szelet hello hello rendelkezésre állását. Például ha hello érhetők el adatok óránkénti, hello ellenőrzés toosee hello külső adatok elérhetők legyenek és hello megfelelő szelet készen felszámítása dataDelay használatával.<br/><br/>Csak érvényes toohello jelenlegi idő.  Például ha 1:00 PM azonnal, és az értéke 10 perc hello érvényesítési kezdődik, 1:10 óra.<br/><br/>Ez a beállítás nem befolyásolja az elmúlt hello szeletek (szelet befejezési időpontja + dataDelay szeletek < most) dolgoznak fel késedelem nélkül.<br/><br/>23:59 óra kell hello segítségével toospecified hosszabb idő `day.hours:minutes:seconds` formátumban. Például toospecify 24 órát, ne használjon 24:00:00. Ehelyett használjon 1.00:00:00. Ha 24:00:00 használja, akkor a rendszer 24 napos (24.00:00:00). 1 nap és 4 óra adja meg 1:04:00:00. |Nem |0 |
| RetryInterval |hello várakozási idő a hibákhoz és hello következő újrapróbálkozások között. Ha nem sikerül egy try, hello következő kísérlet retryInterval utánra esik. <br/><br/>Ha 1:00 PM most, az első lépések hello első próbálkozás. Hello időtartama toocomplete hello első eredetiség ellenőrzésének 1 perc és hello művelet végrehajtása sikertelen volt, hello legközelebbi újrapróbálkozás akkor 1:00 + 1 perc (időtartam) + (újrapróbálkozási időköz) 1 perc = 1:02 PM. <br/><br/>Az elmúlt hello szeletek nincs késleltetés. hello újrapróbálkozási azonnal történik. |Nem |00:01:00 (1 perc) |
| retryTimeout |hello időkorlátjának újrapróbálkozási kísérletei.<br/><br/>Ha ez a tulajdonság értéke too10 perc hello érvényesítési igények toobe 10 percen belül fejeződött be. Ha hosszabb ideig tart, mint 10 percig tooperform hello érvényesítési, hello időtúllépést próbálkozzon újra.<br/><br/>Ha bármilyen kísérlet hello érvényesítéshez időkorlátja lejár, hello szelet időtúllépésbe került van megjelölve. |Nem |00:10:00 (10 perc) |
| maximumRetry |Számos alkalommal fordult elő az toocheck a hello hello külső adatok rendelkezésre állását. hello engedélyezett maximális értéke 10. |Nem |3 |


## <a name="data-stores"></a>ADATTÁROLÓ
Hello [társított szolgáltatás](#linked-service) szakaszban megadott leírásainak összekapcsolt szolgáltatások gyakori tooall típusa JSON-elemek szerepelnek. Ez a szakasz ismerteti a JSON-elemek szerepelnek, amelyek adott tooeach adattár részleteit.

Hello [Dataset](#dataset) szakaszban megadott JSON-elemek szerepelnek, amelyek közös tooall fajta adatkészlet leírása. Ez a szakasz ismerteti a JSON-elemek szerepelnek, amelyek adott tooeach adattár részleteit.

Hello [tevékenység](#activity) JSON-elemek szerepelnek, amelyek közös tooall típusú tevékenységek leírásainak szakaszát. Ez a szakasz ismerteti, amelyek adott tooeach adattár, mikor kell azokat alkalmazni, a másolási tevékenység során a forrás/fogadó JSON-elemek szerepelnek.  

Megtudhatja, hogyan készítheti toosee hello JSON sémák kapcsolódó szolgáltatás, adatkészlet és forrás/fogadó hello másolási tevékenységhez hello hello tároló hello hivatkozásra kattintva.

| Kategória | Adattár 
|:--- |:--- |
| **Azure** |[Azure Blob Storage](#azure-blob-storage) |
| &nbsp; |[Azure Data Lake Store](#azure-datalake-store) |
| &nbsp; |[Azure Cosmos DB](#azure-cosmos-db) |
| &nbsp; |[Azure SQL Database](#azure-sql-database) |
| &nbsp; |[Azure SQL Data Warehouse](#azure-sql-data-warehouse) |
| &nbsp; |[Azure Search](#azure-search) |
| &nbsp; |[Azure Table storage](#azure-table-storage) |
| **Adatbázisok** |[Amazon Redshift](#amazon-redshift) |
| &nbsp; |[IBM DB2](#ibm-db2) |
| &nbsp; |[MySQL](#mysql) |
| &nbsp; |[Oracle](#oracle) |
| &nbsp; |[PostgreSQL](#postgresql) |
| &nbsp; |[SAP Business Warehouse](#sap-business-warehouse) |
| &nbsp; |[SAP HANA](#sap-hana) |
| &nbsp; |[SQL Server](#sql-server) |
| &nbsp; |[Sybase](#sybase) |
| &nbsp; |[Teradata](#teradata) |
| **NoSQL** |[Cassandra](#cassandra) |
| &nbsp; |[MongoDB](#mongodb) |
| **Fájl** |[Amazon S3](#amazon-s3) |
| &nbsp; |[Fájlrendszer](#file-system) |
| &nbsp; |[FTP](#ftp) |
| &nbsp; |[HDFS](#hdfs) |
| &nbsp; |[SFTP](#sftp) |
| **Egyéb** |[HTTP](#http) |
| &nbsp; |[OData](#odata) |
| &nbsp; |[ODBC](#odbc) |
| &nbsp; |[Salesforce](#salesforce) |
| &nbsp; |[Webes tábla](#web-table) |

## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="linked-service"></a>Társított szolgáltatások
Társított szolgáltatások két típusa van: Azure Storage társított szolgáltatásnak, és a társított szolgáltatásnak Azure Storage SAS.

#### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
toolink a az Azure storage-tooa data factory hello segítségével **fiókkulcs**, hozzon létre egy Azure Storage társított szolgáltatást. toodefine egy Azure Storage társított szolgáltatásnak, set hello **típus** hello a társított szolgáltatás túl**AzureStorage**. Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| connectionString |Adjon meg információt tooconnect tooAzure tárolási hello connectionString tulajdonság szükséges. |Igen |

##### <a name="example"></a>Példa  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a>Az Azure Storage SAS társított szolgáltatás
hello Azure Storage SAS kapcsolódó szolgáltatás lehetővé teszi egy Azure Storage-fiók tooan az Azure data factory toolink egy közös hozzáférésű Jogosultságkód (SAS) használatával. Ez hozzáférést biztosít a hello adat-előállító korlátozott/időhöz kötött tooall/specifikus erőforrások (blobtárolóban /) hello tárolóban. toolink a az Azure storage-tooa data factory használatával a közös hozzáférésű Jogosultságkód, Azure Storage SAS társított szolgáltatás létrehozása. egy Azure Storage SAS toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureStorageSas**. Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:   

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| sasUri |Adja meg a megosztott hozzáférési aláírást URI toohello Azure tárolási erőforrások, például blob, a tároló vagy a tábla. |Igen |

##### <a name="example"></a>Példa

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

További információ a következő összekapcsolt szolgáltatások: [Azure Blob Storage összekötő](data-factory-azure-blob-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
egy Azure Blob-adathalmazra toodefine set hello **típus** hello adatkészlet túl**AzureBlob**. Ezt követően adja a következő hello Azure Blob tulajdonságokat hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Elérési út toohello tároló és mappa hello blob Storage tárolóban. Példa: myblobcontainer\myblobfolder\ |Igen |
| fileName |Hello blob neve. Fájlnév nem, kötelező, és a kis-és nagybetűket.<br/><br/>Ha meg kell adnia egy fájlnevet, hello (például a Másolás) tevékenység works hello adott Blob.<br/><br/>Ha nincs megadva fájlnév, másolása összes BLOB bemeneti adatkészlet hello folderPath tartalmazza.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl lenne a következő formátumban hello: adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| partitionedBy |partitionedBy egy nem kötelező tulajdonság. Használhatja a dinamikus folderPath toospecify és a fájlnév idő adatsorozat adatok. Például folderPath is paraméteres adatok óránkénti. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
 ```


További információkért lásd: [Azure Blob összekötő](data-factory-azure-blob-connector.md#dataset-properties) cikk.

### <a name="blobsource-in-copy-activity"></a>A másolási tevékenység BlobSource
Adatok másolása az Azure Blob-tárolóból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**BlobSource**, és adja meg a következő tulajdonságok hello ** forrás ** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |TRUE hamis (alapértelmezés) |Nem |

#### <a name="example-blobsource"></a>Példa: BlobSource **
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a>A másolási tevékenység BlobSink
Adatok tooan Azure Blob Storage másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**BlobSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| copyBehavior |Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer. |<b>PreserveHierarchy</b>: megtartja hello fájl hierarchia hello célmappában. hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.<br/><br/><b>FlattenHierarchy</b>: összes fájl hello forrásmappából a hello első szintű tároló mappa. hello fájljaira automatikusan létrehozott nevet adni. <br/><br/><b>Mergefiles típusú (alapértelmezett):</b> forrásfájlból hello mappa tooone minden fájl egyesíti. Ha hello fájl/Blob neve meg van adva, egyesített hello neve legyen hello megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét. |Nem |

#### <a name="example-blobsink"></a>Példa: BlobSink

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Azure Blob összekötő](data-factory-azure-blob-connector.md#copy-activity-properties) cikk. 

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Társított szolgáltatások
egy Azure Data Lake Store kapcsolódó szolgáltatás toodefine, hello beállítása hello típusú társított szolgáltatás túl**AzureDataLakeStore**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | hello type tulajdonságot kell beállítani: **AzureDataLakeStore** | Igen |
| dataLakeStoreUri | Hello Azure Data Lake Store-fiók adatainak megadása. Az hello a következő formátumban: `https://[accountname].azuredatalakestore.net/webhdfs/v1` vagy `adl://[accountname].azuredatalakestore.net/`. | Igen |
| subscriptionId | Azure-előfizetés azonosítója toowhich Data Lake Store tartozik. | A fogadó szükséges |
| erőforráscsoport-név | Azure-erőforrás csoport neve toowhich Data Lake Store tartozik. | A fogadó szükséges |
| servicePrincipalId | Adja meg a hello alkalmazás ügyfél-azonosítót. | Igen (a szolgáltatás egyszerű hitelesítés) |
| servicePrincipalKey | Adja meg a hello kulcsát. | Igen (a szolgáltatás egyszerű hitelesítés) |
| Bérlői | Adja meg a hello bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található. Ez által rámutató hello egér hello Azure-portál jobb felső sarkában hello kérheti le. | Igen (a szolgáltatás egyszerű hitelesítés) |
| Engedélyezési | Kattintson a **engedélyezés** hello gombjára **Data Factory Editor** , és írja be a hitelesítő adat, amelyet hozzárendel hello automatikusan létrehozott engedélyezési URL-cím toothis tulajdonság. | Igen (a felhasználói hitelesítő)|
| Munkamenet-azonosító | OAuth munkamenet-azonosító hello OAuth hitelesítési munkamenetből. Minden munkamenet-azonosító egyedi, és előfordulhat, hogy csak egyszer használható. Ez a beállítás automatikusan létrejön a Data Factory Editor használatakor. | Igen (a felhasználói hitelesítő) |

#### <a name="example-using-service-principal-authentication"></a>Példa: a szolgáltatás egyszerű hitelesítést használó
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a>Példa: a felhasználói hitelesítő adatok hitelesítést használnak
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy Azure Data Lake Store adatkészlet set hello **típus** hello adatkészlet túl**AzureDataLakeStore**, és adja meg a következő tulajdonságai a hello hello **typeProperties**szakasz: 

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| folderPath |Elérési út toohello tároló és mappa hello Azure Data Lake tárolja. |Igen |
| fileName |Hello fájl hello Azure Data Lake store nevét. Fájlnév nem, kötelező, és a kis-és nagybetűket. <br/><br/>Ha megad egy fájlnevet, hello tevékenység (például a Másolás) működik-e az hello adott fájlt.<br/><br/>Ha nincs megadva fájlnév, másolása hello folderPath bemeneti adatkészlet összes fájl tartalmazza.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl lenne a következő formátumban hello: adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| partitionedBy |partitionedBy egy nem kötelező tulajdonság. Használhatja a dinamikus folderPath toospecify és a fájlnév idő adatsorozat adatok. Például folderPath is paraméteres adatok óránkénti. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

#### <a name="example"></a>Példa
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#dataset-properties) cikk. 

### <a name="azure-data-lake-store-source-in-copy-activity"></a>A másolási tevékenység az Azure Data Lake Store-forrás
Adatok másolása az Azure Data Lake Store a, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**AzureDataLakeStoreSource**, és adja meg a következő tulajdonságok hello **forrás**  szakasz:

**AzureDataLakeStoreSource** támogatja a következő tulajdonságai hello **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |TRUE hamis (alapértelmezés) |Nem |

#### <a name="example-azuredatalakestoresource"></a>Példa: AzureDataLakeStoreSource

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#copy-activity-properties) cikk.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>A másolási tevékenység az Azure Data Lake Store fogadó
Adatok tooan Azure Data Lake Store másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**AzureDataLakeStoreSink**, és adja meg a következő tulajdonságok hello **fogadó**szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| copyBehavior |Meghatározza a hello másolási viselkedését. |<b>PreserveHierarchy</b>: megtartja hello fájl hierarchia hello célmappában. hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.<br/><br/><b>FlattenHierarchy</b>: hello forrásmappából minden fájl első szintű hello célmappában jönnek létre. hello fájljaira jönnek létre automatikusan létrehozott névvel.<br/><br/><b>Mergefiles típusú</b>: összes fájl forrásfájlból hello mappa tooone egyesíti. Ha hello fájl/Blob neve meg van adva, egyesített hello neve legyen hello megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét. |Nem |

#### <a name="example-azuredatalakestoresink"></a>Példa: AzureDataLakeStoreSink
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Azure Data Lake Store összekötő](data-factory-azure-datalake-connector.md#copy-activity-properties) cikk. 

## <a name="azure-cosmos-db"></a>Azure Cosmos DB  

### <a name="linked-service"></a>Társított szolgáltatások
egy Azure Cosmos DB toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**DocumentDb**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:  

| **Tulajdonság** | **Leírás** | **Szükséges** |
| --- | --- | --- |
| connectionString |Adja meg a szükséges adatok tooconnect tooAzure Cosmos DB adatbázisban. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine egy Azure Cosmos DB adatkészlet set hello **típus** hello adatkészlet túl**DocumentDbCollection**, és adja meg a következő tulajdonságai a hello hello **typeProperties** a szakasz: 

| **Tulajdonság** | **Leírás** | **Szükséges** |
| --- | --- | --- |
| CollectionName |Hello Azure Cosmos DB gyűjtemény neve. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#dataset-properties) cikk.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>A másolási tevékenység Azure Cosmos DB gyűjtemény forrás
Adatok másolása az Azure Cosmos adatbázisából, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**DocumentDbCollectionSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:


| **Tulajdonság** | **Leírás** | **Megengedett értékek** | **Szükséges** |
| --- | --- | --- | --- |
| lekérdezés |Adja meg a hello tooread adatait kérdezi le. |Lekérdezés-karakterlánc hossza Azure Cosmos DB által támogatott. <br/><br/>Példa:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nem <br/><br/>Ha nincs megadva, hello végrehajtott SQL-utasítást:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciális karakter tooindicate, hogy a dokumentum hello van beágyazva. |Bármely karakter. <br/><br/>Azure Cosmos-adatbázis egy NoSQL-tároló JSON-dokumentumok, amelyben beágyazott struktúrákat engedélyezett. Az Azure Data Factory lehetővé teszi, hogy a felhasználó toodenote hierarchia keresztül nestingSeparator, amely "." a fenti példák hello. Hello elválasztóval hello másolási tevékenység hello "Name" objektum három gyermekek elemekkel hozza létre első, középső és utolsó, függően too"Name.First", "Name.Middle" és "Name.Last" hello a tábla megadása. |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Az Azure Cosmos DB gyűjtemény fogadó a másolási tevékenység
Adatok tooAzure Cosmos DB másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**DocumentDbCollectionSink**, és adja meg a következő tulajdonságok hello **fogadó**szakasz:

| **Tulajdonság** | **Leírás** | **Megengedett értékek** | **Szükséges** |
| --- | --- | --- | --- |
| nestingSeparator |A különleges karakterek hello forrás oszlop neve tooindicate, amely a beágyazott dokumentumok van szükség. <br/><br/>Például fent: `Name.First` hello kimeneti táblát hoz létre hello JSON struktúrában hello Cosmos DB dokumentumban a következő:<br/><br/>"Name": {<br/>    "Első": "John"<br/>}, |Az karakter, amely használt tooseparate beágyazási szinttel.<br/><br/>Alapértelmezett érték `.` (pont). |Az karakter, amely használt tooseparate beágyazási szinttel. <br/><br/>Alapértelmezett érték `.` (pont). |
| WriteBatchSize |A lekérdezések tooAzure Cosmos DB toocreate dokumentumok párhuzamos száma.<br/><br/>Hello teljesítmény úgy finomhangolhatja, e tulajdonság használatával vagy az Azure Cosmos Adatbázisból adatok másolásakor. A jobb teljesítmény számíthat, mivel a rendszer további kérelmeket párhuzamos tooAzure Cosmos DB elküldi a writeBatchSize növelésével. Azonban szüksége lesz, amelyek sávszélesség-szabályozás tooavoid is throw hello hibaüzenet: "Ez nagy lekérő".<br/><br/>Sávszélesség-szabályozás tényező, beleértve a dokumentumok, a dokumentumok számát méretét, indexelő házirend célgyűjteményt stb határoz meg. A másolási műveletek, használhat egy jobb gyűjtemény (például S3) toohave hello legtöbb átviteli sebesség érhető el (2500 kérés egység/másodperc). |Egész szám |Nem (alapértelmezett: 5) |
| writeBatchTimeout |Várnia kell az hello művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

További információkért lásd: [Azure Cosmos DB összekötő](data-factory-azure-documentdb-connector.md#copy-activity-properties) cikk.

## <a name="azure-sql-database"></a>Azure SQL Database

### <a name="linked-service"></a>Társított szolgáltatások
egy Azure SQL Database toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDatabase**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| connectionString |Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Database-példányt. |Igen |

#### <a name="example"></a>Példa
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy Azure SQL Database adatkészlet set hello **típus** hello adatkészlet túl**AzureSqlTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** a szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla vagy nézet hello Azure SQL Database-példányt, amelyre a társított szolgáltatás neve hivatkozik. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties) cikk. 

### <a name="sql-source-in-copy-activity"></a>A másolási tevékenység SQL-forrás
Adatok másolása az Azure SQL-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**SqlSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Példa: `select * from MyTable`. |Nem |
| sqlReaderStoredProcedureName |Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat. |Hello neve tárolt eljárást. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#copy-activity-properties) cikk. 

### <a name="sql-sink-in-copy-activity"></a>A másolási tevékenység SQL fogadó
Adatok tooAzure SQL-adatbázis másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**SqlSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |
| WriteBatchSize |Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat. |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait. |A lekérdezési utasítást. |Nem |
| sliceIdentifierColumnName |Adja meg oszlop nevét, a másolási tevékenység toofill szelet azonosító automatikusan létrejön, vagyis amikor futtassa újra a megadott szelet adatainak használt tooclean. |Egy oszlop binary(32) adattípusú oszlop neve. |Nem |
| sqlWriterStoredProcedureName |Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába. |Hello neve tárolt eljárást. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |
| sqlWriterTableType |Adjon meg egy tábla Típus neve toobe hello tárolt eljárásban használt. Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus. Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat. |Egy tábla környezettípus nevét. |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#copy-activity-properties) cikk. 

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

### <a name="linked-service"></a>Társított szolgáltatások
Azure SQL Data Warehouse toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDW**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| connectionString |Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Data Warehouse-példányhoz. |Igen |



#### <a name="example"></a>Példa

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy Azure SQL Data Warehouse adatkészlet set hello **típus** hello adatkészlet túl**AzureSqlDWTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties**szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla vagy nézet hello Azure SQL Data Warehouse-adatbázis, amely a társított szolgáltatás hello nevére hivatkozik. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) cikk. 

### <a name="sql-dw-source-in-copy-activity"></a>A másolási tevékenység SQL DW-forrás
Adatok másolása az Azure SQL Data Warehouse, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**SqlDWSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `select * from MyTable`. |Nem |
| sqlReaderStoredProcedureName |Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat. |Hello neve tárolt eljárást. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) cikk. 

### <a name="sql-dw-sink-in-copy-activity"></a>A másolási tevékenység SQL DW fogadó
Adatok tooAzure SQL Data Warehouse másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**SqlDWSink**, és adja meg a következő tulajdonságok hello **fogadó** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait. |A lekérdezési utasítást. |Nem |
| allowPolyBase |Azt jelzi, hogy (ha alkalmazható) PolyBase toouse BULKINSERT mechanizmus helyett. <br/><br/> **A PolyBase használata javasolt módja tooload adatokat az SQL Data Warehouse hello.** |True (Igaz) <br/>Hamis (alapértelmezés) |Nem |
| kapcsolódó polyBaseSettings |Egy csoport lehet megadni, ha hello tulajdonságok **allowPolybase** tulajdonsága túl**igaz**. |&nbsp; |Nem |
| rejectValue |Megadja a hello számát vagy a sorok, amelyekre el lehet utasítani, mielőtt hello lekérdezés nem sikerült százalékát. <br/><br/>Bővebben a hello PolyBase elutasítása hello lehetőségeit további **argumentumok** szakasza [külső tábla létrehozása (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) témakör. |0 (alapértelmezés), 1, 2... |Nem |
| rejectType |Meghatározza, hogy hello rejectValue beállítás konstans értéket vagy százalékában van megadva. |Érték (alapértelmezett), százalékos aránya |Nem |
| rejectSampleValue |Határozza meg, hogy hello sorok tooretrieve előtt hello PolyBase újraszámítja a visszautasított sorok hello százalékát. |1, 2, … |Igen, ha **rejectType** van **százalékos aránya** |
| useTypeDefault |Itt adhatja meg, hogyan értékek a hiányzó toohandle tagolt-e szövegfájlok amikor PolyBase hello szövegfájlból kér le adatokat.<br/><br/>Ezt a tulajdonságot hello argumentumok című szakaszában olvashat [létrehozása külső FÁJLFORMÁTUM (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |IGAZ, hamis (alapértelmezés) |Nem |
| WriteBatchSize |Adatok beszúrása hello SQL táblázatba, amikor hello puffer mérete eléri writeBatchSize |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) cikk. 

## <a name="azure-search"></a>Azure Search

### <a name="linked-service"></a>Társított szolgáltatások
az Azure Search toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSearch**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:  

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| URL-címe | Hello Azure Search szolgáltatás URL-címe. | Igen |
| kulcs | Adminisztrátori kulcsot a hello Azure Search szolgáltatás. | Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine egy Azure Search adatkészlet set hello **típus** hello adatkészlet túl**AzureSearchIndex**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz : 

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| type | hello type tulajdonság túl be kell állítani**AzureSearchIndex**.| Igen |
| indexName | Hello Azure Search-index neve. Adat-előállító nem hoz létre hello index. az Azure Search léteznie kell a hello index. | Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#dataset-properties) cikk.

### <a name="azure-search-index-sink-in-copy-activity"></a>A másolási tevékenység az Azure Search-Index fogadó
Adatok tooan Azure Search-index másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**AzureSearchIndexSink**, és adja meg a következő tulajdonságok hello **fogadó**szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Meghatározza, hogy toomerge vagy cserélje le a dokumentum már létezik hello index. | Egyesítés (alapértelmezett)<br/>Feltöltés| Nem |
| WriteBatchSize | Fájlfeltöltések hello Azure Search-index az adatokat, amikor hello puffer mérete eléri writeBatchSize. | 1 too1 000. Alapértelmezett érték 1000. | Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Azure Search összekötő](data-factory-azure-search-connector.md#copy-activity-properties) cikk.

## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="linked-service"></a>Társított szolgáltatások
Társított szolgáltatások két típusa van: Azure Storage társított szolgáltatásnak, és a társított szolgáltatásnak Azure Storage SAS.

#### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
toolink a az Azure storage-tooa data factory hello segítségével **fiókkulcs**, hozzon létre egy Azure Storage társított szolgáltatást. toodefine egy Azure Storage társított szolgáltatásnak, set hello **típus** hello a társított szolgáltatás túl**AzureStorage**. Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type |hello type tulajdonságot kell beállítani: **AzureStorage** |Igen |
| connectionString |Adjon meg információt tooconnect tooAzure tárolási hello connectionString tulajdonság szükséges. |Igen |

**Példa**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

#### <a name="azure-storage-sas-linked-service"></a>Az Azure Storage SAS társított szolgáltatás
hello Azure Storage SAS kapcsolódó szolgáltatás lehetővé teszi egy Azure Storage-fiók tooan az Azure data factory toolink egy közös hozzáférésű Jogosultságkód (SAS) használatával. Ez hozzáférést biztosít a hello adat-előállító korlátozott/időhöz kötött tooall/specifikus erőforrások (blobtárolóban /) hello tárolóban. toolink a az Azure storage-tooa data factory használatával a közös hozzáférésű Jogosultságkód, Azure Storage SAS társított szolgáltatás létrehozása. egy Azure Storage SAS toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureStorageSas**. Ezután megadhatja tulajdonságok hello a következő **typeProperties** szakasz:   

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type |hello type tulajdonságot kell beállítani: **AzureStorageSas** |Igen |
| sasUri |Adja meg a megosztott hozzáférési aláírást URI toohello Azure tárolási erőforrások, például blob, a tároló vagy a tábla. |Igen |

**Példa**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine az Azure tábla adatkészlethez set hello **típus** hello adatkészlet túl**AzureTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello hello Azure tábla adatbázispéldány táblájának, amelyre a társított szolgáltatás neve hivatkozik. |Igen. Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél. Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél. |

#### <a name="example"></a>Példa

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#dataset-properties) cikk. 

### <a name="azure-table-source-in-copy-activity"></a>A másolási tevékenység az Azure tábla forrása
Adatok másolása az Azure Table Storage, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**AzureTableSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| azureTableSourceQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |Azure-tábla lekérdezési karakterlánc. Példák a következő szakaszban hello. |Nem. Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél. Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél. |
| azureTableSourceIgnoreTableNotFound |Azt jelzi, hogy swallow hello kivétel tábla nem létezik. |IGAZ<br/>HAMIS |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#copy-activity-properties) cikk. 

### <a name="azure-table-sink-in-copy-activity"></a>A másolási tevékenység az Azure tábla fogadó
A Table Storage adatok tooAzure másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**AzureTableSink**, és adja meg a következő tulajdonságok hello **fogadó** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Alapértelmezett partíció kulcsérték hello a fogadó által használható. |Egy karakterlánc-érték. |Nem |
| azureTablePartitionKeyName |Adja meg, amelynek értékeket fogja használni, mint partíciókulcsok hello oszlop neve. Ha nincs megadva, AzureTableDefaultPartitionKeyValue hello partíciókulcs lesz. |Egy oszlop neve. |Nem |
| azureTableRowKeyName |Adja meg, amelynek oszlop értékeit kulcsként sor hello oszlop neve. Ha nincs megadva, minden egyes sorára használjon a GUID Azonosítót. |Egy oszlop neve. |Nem |
| azureTableInsertType |hello mód tooinsert adatait az Azure táblájába.<br/><br/>Ez a tulajdonság szabja meg, hogy meglévő hello kimeneti táblát, amely megfelelő a partíció-és sorkulcsok sora cseréje vagy egyesített értékükre. <br/><br/>toolearn arról, hogyan működnek ezek a beállítások (lemezegyesítési és -csere), lásd: [Insert vagy az egyesítéses entitás](https://msdn.microsoft.com/library/azure/hh452241.aspx) és [Insert vagy az entitás cseréje](https://msdn.microsoft.com/library/azure/hh452242.aspx) témaköröket. <br/><br> Ez a beállítás érvényes szinten hello sor nem hello táblaszintű, és sem a lehetőség törli a nem létező hello bemeneti hello kimeneti tábla sorainak. |Egyesítés (alapértelmezett)<br/>cserélje le |Nem |
| WriteBatchSize |Amikor hello writeBatchSize vagy writeBatchTimeout találati adatok beillesztése hello Azure-tábla. |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| writeBatchTimeout |Adatbeszúrást hello Azure-tábla amikor hello writeBatchSize vagy writeBatchTimeout találati |A TimeSpan<br/><br/>Példa: "00: 20:00" (20 perc) |Nem (alapértelmezett toostorage ügyfél alapértelmezett időtúllépési érték 90 másodperc) |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
További információ a következő összekapcsolt szolgáltatások: [Azure Table Storage összekötő](data-factory-azure-table-connector.md#copy-activity-properties) cikk. 

## <a name="amazon-redshift"></a>Amazon RedShift

### <a name="linked-service"></a>Társított szolgáltatások
az Amazon Redshift toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AmazonRedshift**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kiszolgáló |Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello Amazon Redshift. |Igen |
| port |Amazon Redshift server hello hello TCP port száma hello toolisten ügyfél-kommunikációhoz használ. |Nem, alapértelmezett érték: 5439 |
| adatbázis |Hello Amazon Redshift adatbázis neve. |Igen |
| felhasználónév |Hozzáférés toohello adatbázis-felhasználó nevét. |Igen |
| jelszó |Hello felhasználói fiókhoz tartozó jelszót. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine az Amazon Redshift adatkészlethez set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** a szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello Amazon Redshift adatbázis, amelyre a társított szolgáltatás neve hivatkozik. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |


#### <a name="example"></a>Példa

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#dataset-properties) cikk.

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás 
Adatok másolása az Amazon Redshift, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `select * from MyTable`. |Nem (Ha **tableName** a **dataset** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
További információkért lásd: [Amazon Redshift összekötő](#data-factory-amazon-redshift-connector.md#copy-activity-properties) cikk.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Társított szolgáltatások
az IBM DB2 toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesDB2**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kiszolgáló |Hello DB2-kiszolgáló neve. |Igen |
| adatbázis |Hello DB2-adatbázis neve. |Igen |
| Séma |Hello séma hello adatbázis neve. hello sémanév a kis-és nagybetűket. |Nem |
| AuthenticationType |Tooconnect toohello DB2-adatbázishoz használt hitelesítés típusa. Lehetséges értékek a következők: névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni DB2-adatbázishoz. |Igen |

#### <a name="example"></a>Példa
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine DB2 dataset, set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello hello DB2 adatbázispéldány táblájának, amelyre a társított szolgáltatás neve hivatkozik. hello táblanév a kis-és nagybetűket. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) 

#### <a name="example"></a>Példa
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#dataset-properties) cikk.

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása az IBM DB2, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `"query": "select * from "MySchema"."MyTable""`. |Nem (Ha **tableName** a **dataset** van megadva) |

#### <a name="example"></a>Példa
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
További információkért lásd: [IBM DB2-összekötő](#data-factory-onprem-db2-connector.md#copy-activity-properties) cikk.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Társított szolgáltatások
egy MySQL toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesMySql**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kiszolgáló |Hello MySQL kiszolgáló neve. |Igen |
| adatbázis |Hello MySQL-adatbázis neve. |Igen |
| Séma |Hello séma hello adatbázis neve. |Nem |
| AuthenticationType |Tooconnect toohello MySQL-adatbázis használt hitelesítés típusa. Lehetséges értékek a következők: `Basic`. |Igen |
| felhasználónév |Adja meg a felhasználó nevét tooconnect toohello MySQL-adatbázis. |Igen |
| jelszó |Adja meg a megadott hello felhasználói fiók jelszavát. |Igen |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét kell tooconnect toohello helyszíni MySQL-adatbázis használata. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy MySQL dataset set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello MySQL-adatbázis-példány, amelyre a társított szolgáltatás neve hivatkozik. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#dataset-properties) cikk. 

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása a MySQL-adatbázis, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `select * from MyTable`. |Nem (Ha **tableName** a **dataset** van megadva) |


#### <a name="example"></a>Példa
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

További információkért lásd: [MySQL összekötő](data-factory-onprem-mysql-connector.md#copy-activity-properties) cikk. 

## <a name="oracle"></a>Oracle 

### <a name="linked-service"></a>Társított szolgáltatások
az Oracle toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesOracle**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| driverType | Adja meg, mely illesztőprogram toouse toocopy adatait / tooOracle adatbázis. Két érték engedélyezett **Microsoft** vagy **ODP** (alapértelmezett). Lásd: [verziójától és a telepítés támogatott](#supported-versions-and-installation) illesztőprogram adatai szakaszban. | Nem |
| connectionString | Adjon meg információt tooconnect toohello Oracle adatbázispéldány hello connectionString tulajdonság szükséges. | Igen |
| gatewayName | Hello átjáró, amely használt tooconnect toohello helyszíni Oracle-kiszolgáló neve |Igen |

#### <a name="example"></a>Példa
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine az Oracle adatkészlethez set hello **típus** hello adatkészlet túl**OracleTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Oracle-adatbázishoz kapcsolódó szolgáltatás hello hello hello tábla neve hivatkozik. |Nem (Ha **oracleReaderQuery** a **OracleSource** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#dataset-properties) cikk.

### <a name="oracle-source-in-copy-activity"></a>A másolási tevékenység Oracle-forrás
Adatok másolása az Oracle-adatbázishoz, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**OracleSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| oracleReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például:`select * from MyTable` <br/><br/>Ha nincs megadva, hello végrehajtott SQL-utasítást:`select * from MyTable` |Nem (Ha **tableName** a **dataset** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#copy-activity-properties) cikk.

### <a name="oracle-sink-in-copy-activity"></a>A másolási tevékenység Oracle fogadó
Adatok tooam Oracle adatbázis másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**OracleSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> . Példa: 00:30:00 (30 perc). |Nem |
| WriteBatchSize |Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat. |Egész szám (sorok száma) |Nem (alapértelmezett: 100) |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait. |A lekérdezési utasítást. |Nem |
| sliceIdentifierColumnName |Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean. |Egy oszlop binary(32) adattípusú oszlop neve. |Nem |

#### <a name="example"></a>Példa
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
További információkért lásd: [Oracle összekötő](data-factory-onprem-oracle-connector.md#copy-activity-properties) cikk.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Társított szolgáltatások
egy PostgreSQL toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesPostgreSql**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kiszolgáló |Hello PostgreSQL-kiszolgáló neve. |Igen |
| adatbázis |Hello PostgreSQL-adatbázis neve. |Igen |
| Séma |Hello séma hello adatbázis neve. hello sémanév a kis-és nagybetűket. |Nem |
| AuthenticationType |Tooconnect toohello PostgreSQL-adatbázishoz használt hitelesítés típusa. Lehetséges értékek a következők: névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello a helyszíni PostgreSQL-adatbázisból. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine egy PostgreSQL dataset set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello PostgreSQL-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik. hello táblanév a kis-és nagybetűket. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

#### <a name="example"></a>Példa
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#dataset-properties) cikk.

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása egy PostgreSQL-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: "query": "Válasszon * a \"MySchema\".\" MyTable\"". |Nem (Ha **tableName** a **dataset** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

További információkért lásd: [PostgreSQL összekötő](data-factory-onprem-postgresql-connector.md#copy-activity-properties) cikk.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Társított szolgáltatások
egy SAP Business Warehouse (BW) toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**SapBw**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:  

Tulajdonság | Leírás | Megengedett értékek | Szükséges
-------- | ----------- | -------------- | --------
kiszolgáló | Hello kiszolgálóra mely hello SAP BW példány neve. | Karakterlánc | Igen
systemNumber | Hello SAP BW rendszer rendszer száma. | Kétjegyű tizedes tört karakterláncból. | Igen
clientId | Hello ügyfél hello SAP W rendszer ügyfél-azonosító. | Három számjegyből tizedes tört karakterláncból. | Igen
felhasználónév | Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve | Karakterlánc | Igen
jelszó | Hello felhasználó jelszavát. | Karakterlánc | Igen
gatewayName | Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP BW példányát kell használnia. | Karakterlánc | Igen
encryptedCredential | hello titkosított hitelesítő adat karakterlánc. | Karakterlánc | Nem

#### <a name="example"></a>Példa

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy SAP BW dataset set hello **típus** hello adatkészlet túl**RelationalTable**. Nincsenek hello SAP BW adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.  

#### <a name="example"></a>Példa

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#dataset-properties) cikk. 

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása az SAP Business Warehouse, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés | Megadja a hello MDX tooread adatait hello SAP BW-példányból. | MDX-lekérdezés. | Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<MDX query for SAP BW>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

További információkért lásd: [SAP Business Warehouse-összekötő](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) cikk. 

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Társított szolgáltatások
egy SAP HANA toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**SapHana**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

Tulajdonság | Leírás | Megengedett értékek | Szükséges
-------- | ----------- | -------------- | --------
kiszolgáló | Hello kiszolgálóra mely hello SAP HANA példány neve. Ha a kiszolgáló egy testreszabott portot használ, adja meg a `server:port`. | Karakterlánc | Igen
AuthenticationType | Hitelesítés típusa. | Karakterlánc. "Basic" vagy "Windows" | Igen 
felhasználónév | Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve | Karakterlánc | Igen
jelszó | Hello felhasználó jelszavát. | Karakterlánc | Igen
gatewayName | Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP HANA-példányt kell használnia. | Karakterlánc | Igen
encryptedCredential | hello titkosított hitelesítő adat karakterlánc. | Karakterlánc | Nem

#### <a name="example"></a>Példa

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#linked-service-properties) cikk.
 
### <a name="dataset"></a>Adatkészlet
toodefine egy SAP HANA dataset set hello **típus** hello adatkészlet túl**RelationalTable**. Nincsenek hello SAP HANA-adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**. 

#### <a name="example"></a>Példa

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#dataset-properties) cikk. 

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása egy SAP HANA adattárból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés | Megadja a hello SQL lekérdezés tooread adatait hello SAP HANA-példányból. | SQL-lekérdezésben. | Igen |


#### <a name="example"></a>Példa


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

További információkért lásd: [SAP HANA-összekötő](data-factory-sap-hana-connector.md#copy-activity-properties) cikk.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Társított szolgáltatások
Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** toolink egy helyi SQL Server adatbázis tooa adat-előállítóban. a következő táblázat hello biztosít JSON elemek adott tooon-hez kapcsolódó SQL Server szolgáltatás leírását.

a következő táblázat hello biztosít JSON-elemek adott tooSQL csatolt kiszolgáló szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello tulajdonságra kell megadni: **OnPremisesSqlServer**. |Igen |
| connectionString |Adja meg a connectionString információ tooconnect toohello a helyszíni SQL Server-adatbázis SQL-hitelesítéssel vagy a Windows-hitelesítés szükséges. |Igen |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello a helyszíni SQL Server-adatbázist használja. |Igen |
| felhasználónév |Adja meg a felhasználónevet, ha a Windows-hitelesítést használ. Példa: **tartománynév\\felhasználónév**. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |

Hitelesítő adatok hello segítségével titkosíthatja **New-AzureRmDataFactoryEncryptValue** parancsmag, amelyekkel hello kapcsolati karakterlánc, ahogy az alábbi példa hello (**EncryptedCredential** tulajdonság):  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Példa: JSON az SQL-hitelesítéssel

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Példa: JSON a Windows-hitelesítés használatával

Ha a felhasználónév és jelszó van adva, átjáró használja őket tooimpersonate hello megadott felhasználói fiók tooconnect toohello a helyszíni SQL Server-adatbázist. Ellenkező esetben átjáró csatlakozik toohello SQL Server közvetlenül (az indítási fiókjához) átjáró hello biztonsági környezetében.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy SQL Server dataset set hello **típus** hello adatkészlet túl**SqlServerTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla vagy nézet hello SQL Server-adatbázispéldány, amelyre a társított szolgáltatás neve hivatkozik. |Igen |

#### <a name="example"></a>Példa
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#dataset-properties) cikk. 

### <a name="sql-source-in-copy-activity"></a>A másolási tevékenység SQL-forrás
Adatok másolása egy SQL Server-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**SqlSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `select * from MyTable`. Hivatkozhatnak hello bemeneti adatkészlet által hivatkozott hello adatbázisból táblákat. Ha nincs megadva, az SQL-utasítás végrehajtása hello: táblanév kiválaszthatja. |Nem |
| sqlReaderStoredProcedureName |Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat. |Hello neve tárolt eljárást. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |

Ha hello **sqlReaderQuery** megadott hello SqlSource, hello másolási tevékenység során ez a lekérdezés futtatása hello SQL Server-adatbázis forrás tooget hello adatok alapján.

Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).

Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.

> [!NOTE]
> Használata esetén **sqlReaderStoredProcedureName**, továbbra is szükséges toospecify értéket hello **tableName** hello adatkészlet JSON tulajdonság. Nincs érvényesítést hajt végre ezt a táblázatot, ha van.


#### <a name="example"></a>Példa
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Ebben a példában **sqlReaderQuery** hello SqlSource van megadva. hello másolási tevékenység fut ez a lekérdezés hello hello tooget SQL Server-adatbázis forrásadatot. Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el). hello sqlReaderQuery hello bemeneti adatkészlet által hivatkozott hello adatbázison belül több táblát is hivatkozik. Már nem korlátozott tooonly hello tábla adatkészlet tableName typeProperty hello beállítani.

Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.

További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#copy-activity-properties) cikk. 

### <a name="sql-sink-in-copy-activity"></a>A másolási tevékenység SQL fogadó
Adatok tooa SQL Server-adatbázis másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**SqlSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |
| WriteBatchSize |Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat. |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute lekérdezést, úgy, hogy egy adott szelet adatait. További információkért lásd: [ismételhetőség](#repeatability-during-copy) szakasz. |A lekérdezési utasítást. |Nem |
| sliceIdentifierColumnName |Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean. További információkért lásd: [ismételhetőség](#repeatability-during-copy) szakasz. |Egy oszlop binary(32) adattípusú oszlop neve. |Nem |
| sqlWriterStoredProcedureName |Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába. |Hello neve tárolt eljárást. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |
| sqlWriterTableType |Adja meg a tábla Típus neve toobe hello tárolt eljárásban használt. Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus. Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat. |Egy tábla környezettípus nevét. |Nem |

#### <a name="example"></a>Példa
hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#copy-activity-properties) cikk. 

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Társított szolgáltatások
egy Sybase toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesSybase**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kiszolgáló |Hello Sybase-kiszolgáló neve. |Igen |
| adatbázis |Hello Sybase-adatbázis neve. |Igen |
| Séma |Hello séma hello adatbázis neve. |Nem |
| AuthenticationType |Tooconnect toohello Sybase-adatbázishoz használt hitelesítés típusa. Lehetséges értékek a következők: névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni Sybase-adatbázishoz. |Igen |

#### <a name="example"></a>Példa
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy Sybase dataset set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello Sybase-adatbázispéldányt, amely a társított szolgáltatás neve hivatkozik. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#dataset-properties) cikk. 

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása egy Sybase-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `select * from MyTable`. |Nem (Ha **tableName** a **dataset** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

További információkért lásd: [Sybase összekötő](data-factory-onprem-sybase-connector.md#copy-activity-properties) cikk.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Társított szolgáltatások
a Teradata toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesTeradata**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kiszolgáló |Hello Teradata-kiszolgáló neve. |Igen |
| AuthenticationType |Tooconnect toohello Teradata-adatbázishoz használt hitelesítés típusa. Lehetséges értékek a következők: névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Adja meg a felhasználónevet Basic vagy Windows-hitelesítés használata. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello a helyszíni Teradata-adatbázishoz. |Igen |

#### <a name="example"></a>Példa
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine egy Teradata-Blob adatkészletet set hello **típus** hello adatkészlet túl**RelationalTable**. Jelenleg nem támogatott hello Teradata adatkészlet tulajdonságokat. 

#### <a name="example"></a>Példa
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#dataset-properties) cikk.

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása egy Teradata-adatbázisból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás**szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `select * from MyTable`. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

További információkért lásd: [Teradata összekötő](data-factory-onprem-teradata-connector.md#copy-activity-properties) cikk.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Társított szolgáltatások
egy Cassandra toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesCassandra**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| állomás |Egy vagy több IP-címek vagy Cassandra kiszolgálók állomás nevét.<br/><br/>IP-címek vagy nevek tooconnect tooall kiszolgálók vesszővel tagolt listáját adja meg a egyidejűleg. |Igen |
| port |hello hello Cassandra server TCP-port toolisten ügyfél-kommunikációhoz használ. |Nem, alapértelmezett érték: 9042 |
| AuthenticationType |Basic vagy Anonymous |Igen |
| felhasználónév |Adja meg a hello felhasználói fiókhoz tartozó felhasználónevet. |Igen, ha authenticationType tooBasic van beállítva. |
| jelszó |Adja meg a hello felhasználói fiókhoz tartozó jelszót. |Igen, ha authenticationType tooBasic van beállítva. |
| gatewayName |hello hello átjáró, amely használt tooconnect toohello helyszíni Cassandra adatbázis neve. |Igen |
| encryptedCredential |A hitelesítő adatok hello átjáró titkosítja. |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine Cassandra dataset, set hello **típus** hello adatkészlet túl**CassandraTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kulcstérértesítések használatával |Hello kulcstérértesítések használatával vagy a séma Cassandra adatbázis neve. |Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva). |
| tableName |Hello tábla Cassandra adatbázis neve. |Igen (Ha **lekérdezés** a **CassandraSource** nincs definiálva). |

#### <a name="example"></a>Példa

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#dataset-properties) cikk. 

### <a name="cassandra-source-in-copy-activity"></a>A másolási tevékenység Cassandra forrás
Adatok másolása az Cassandra, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**CassandraSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz :

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-92 vagy CQL lekérdezés. Lásd: [CQL hivatkozás](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL-lekérdezés használata esetén adja meg a **kulcstérértesítések használatával name.table neve** tooquery kívánt toorepresent hello tábla. |Nem (ha van megadva a tableName és a dataset kulcstérértesítések használatával). |
| consistencyLevel |hello konzisztencia szint határozza meg, hány replikák tooa olvasási kérelem kell válaszolnia kell a visszatérésre adatok toohello ügyfélalkalmazás. Cassandra ellenőrzések hello replikák megadott számú, az adatok toosatisfy hello olvasni a kérelmet. |EGY, KETTŐ, HÁROM, KVÓRUM, AZ ÖSSZES, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Lásd: [konfigurálása az adatok konzisztenciájának](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) részleteiről. |Nem. Alapértelmezett érték: egyet. |

#### <a name="example"></a>Példa
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [Cassandra összekötő](data-factory-onprem-cassandra-connector.md#copy-activity-properties) cikk.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Társított szolgáltatások
a MongoDB toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesMongoDB**, és adja meg a következő tulajdonságok hello **typeProperties** a szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| kiszolgáló |Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello MongoDB. |Igen |
| port |TCP-portot, amely a MongoDB-kiszolgálóhoz hello toolisten ügyfél-kommunikációhoz használ. |Nem kötelező, alapértelmezett érték: 27017 |
| AuthenticationType |Alapszintű, vagy névtelen. |Igen |
| felhasználónév |Felhasználói fiók MongoDB tooaccess. |Igen (Ha alapszintű hitelesítést használ). |
| jelszó |Hello felhasználó jelszavát. |Igen (Ha alapszintű hitelesítést használ). |
| authSource |Hello MongoDB-adatbázist, amelyet toouse toocheck a hitelesítő adatok hitelesítéshez neve. |Választható (Ha alapszintű hitelesítést használ). alapértelmezett: hello rendszergazdai fiókot és a databaseName tulajdonsággal megadott hello adatbázis használ. |
| DatabaseName |Hello MongoDB-adatbázist, amelyet az tooaccess neve. |Igen |
| gatewayName |Hello adattár hozzáférő hello átjáró neve. |Igen |
| encryptedCredential |A hitelesítőadat-átjáró által titkosított. |Optional |

#### <a name="example"></a>Példa

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Adatkészlet
toodefine a MongoDB dataset set hello **típus** hello adatkészlet túl**MongoDbCollection**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| CollectionName |A MongoDB adatbázis hello gyűjtemény nevét. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>A másolási tevékenység MongoDB-forrás
Adatok másolása a MongoDB, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**MongoDbSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-92 lekérdezési karakterlánc. Például: `select * from MyTable`. |Nem (Ha **collectionName** a **dataset** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

További információkért lásd: [MongoDB összekötő cikk](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Társított szolgáltatások
az Amazon S3 toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AwsAccessKey**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz :  

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| accessKeyID |Hello titkos hívóbetű azonosítója. |Karakterlánc |Igen |
| secretAccessKey |hello titkos hívóbetű magát. |Titkosított titkos karakterlánc |Igen |

#### <a name="example"></a>Példa
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).

### <a name="dataset"></a>Adatkészlet
az Amazon S3 toodefine dataset, set hello **típus** hello adatkészlet túl**AmazonS3**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| bucketName |hello S3 gyűjtő neve. |Karakterlánc |Igen |
| kulcs |hello S3 objektum kulcsának. |Karakterlánc |Nem |
| előtag |Hello S3 Objektumkulcs előtagját. Kiválasztott objektumok, amelynek kulcsait a előtaggal kezdődik. Érvényes, csak ha kulcsa üres. |Karakterlánc |Nem |
| Verzió |Ha engedélyezve van a S3 versioning S3 objektum hello verziója. |Karakterlánc |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem | |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. hello támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem | |


> [!NOTE]
> bucketName + kulcs hello S3 objektum, ahol a gyűjtő S3 objektumok hello legfelső szintű tárolója, és a kulcs hello teljes elérési útja tooS3 objektum hello helyét adja meg.

#### <a name="example-sample-dataset-with-prefix"></a>Példa: Minta-adatkészleteken előtaggal

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a>Példa: Mintát adatkészletre (verziójával)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a>Példa: A dinamikus útvonalak S3
Hello minta rögzített értékeket használjuk kulcs és bucketName tulajdonságok hello Amazon S3 adatkészletben.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

Hello kulcs, és dinamikusan futásidőben bucketName kiszámításához rendszerváltozók SliceStart például a Data Factory lehet.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Mindent hello azonos hello előtag tulajdonsága az Amazon S3 adatkészlethez. Lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md) támogatott funkciók és változók listáját.

További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>A másolási tevékenység rendszer forrás
Adatok másolása az Amazon S3, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz :


| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Meghatározza, hogy toorecursively lista S3 objektumokat hello könyvtára alatt tárolja. |Igaz/hamis |Nem |


#### <a name="example"></a>Példa


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

További információkért lásd: [Amazon S3 összekötő cikk](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).

## <a name="file-system"></a>Fájlrendszer


### <a name="linked-service"></a>Társított szolgáltatások
Egy helyi fájl rendszer tooan az Azure data factory hello társíthatja **a helyi fájlkiszolgáló** társított szolgáltatás. hello a következő táblázat ismerteti, amelyek adott toohello a helyi fájlkiszolgáló társított szolgáltatás JSON-elemek szerepelnek.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |Győződjön meg arról, hogy hello típusú tulajdonsága túl**OnPremisesFileServer**. |Igen |
| állomás |Hello legfelső szintű megjeleníteni kívánt toocopy hello mappa elérési útját adja meg. Hello escape-karakter használata "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat. |Igen |
| felhasználói azonosítóját |Adja meg a toohello kiszolgáló hello felhasználó hello Azonosítót. |Nem (Ha úgy dönt, hogy encryptedCredential) |
| jelszó |Adja meg a hello jelszó hello (userid). |Nem (Ha úgy dönt, hogy encryptedCredential |
| encryptedCredential |Adja meg a hello titkosított hitelesítő adatokat, amelyek a parancsmagot a New-AzureRmDataFactoryEncryptValue hello kaphat. |Nem (Ha úgy dönt, toospecify felhasználói azonosítót és jelszót a szövegként) |
| gatewayName |Megadja, hogy a Data Factory kell használnia tooconnect toohello a helyi fájlkiszolgáló hello átjáró hello nevét. |Igen |

#### <a name="sample-folder-path-definitions"></a>A minta mappa elérési útja definíciók 
| Forgatókönyv | A társított szolgáltatás definíciójának üzemeltetéséhez | Az adatkészlet-definícióban folderPath |
| --- | --- | --- |
| Az adatkezelési átjáró gépen helyi mappában: <br/><br/>Példák: D:\\ \* vagy D:\folder\subfolder\\* |D:\\ \\ (az adatok felügyeleti átjáró 2.0-s és újabb verziók) <br/><br/> a localhost (korábbi verzióihoz mint adatok felügyeleti átjáró 2.0-s) |. \\ \\ vagy mappa\\\\almappa (az adatok felügyeleti átjáró 2.0-s és újabb verziók) <br/><br/>D:\\ \\ vagy D:\\\\mappa\\\\almappa (az átjáró verziója alatt 2.0-s) |
| Távoli megosztott mappa: <br/><br/>Példák: \\ \\myserver\\megosztása\\ \* vagy \\ \\myserver\\megosztása\\mappa\\almappa\\* |\\\\\\\\myserver\\\\megosztása |. \\ \\ vagy mappa\\\\almappa |


#### <a name="example-using-username-and-password-in-plain-text"></a>Példa: Felhasználónév és jelszó használatával egyszerű szöveges

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a>Példa: Encryptedcredential használatával

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#linked-service-properties).

### <a name="dataset"></a>Adatkészlet
toodefine egy fájlrendszer dataset set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Hello részleges toohello mappáját adja meg. Hello escape-karakter használata "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő. |Igen |
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello név hello létrehozott fájl van hello formátuma a következő: <br/><br/>`Data.<Guid>.txt`(Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nem |
| fileFilter |Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét. <br/><br/>Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).<br/><br/>1. példa: "fileFilter": "* .log"<br/>2. példa: "fileFilter": 2016 - 1-?. txt"<br/><br/>Vegye figyelembe, hogy fileFilter egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható. |Nem |
| partitionedBy |Az idősorozat adatok partitionedBy toospecify dinamikus folderPath/fileName is használhatja. Példa: az adatok óránkénti paraméteres folderPath. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**; és a támogatott szintek a következők: **Optimal** és **leggyorsabb**. Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

> [!NOTE]
> Nem használható egyszerre fájlnév és fileFilter.

#### <a name="example"></a>Példa

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>A másolási tevékenység rendszer forrás
Adatok másolása a fájlrendszerből, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |IGAZ, hamis (alapértelmezés) |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#copy-activity-properties).

### <a name="file-system-sink-in-copy-activity"></a>A másolási tevékenység gyűjtése fájlrendszer
Adatok tooFile rendszer másolása, állítsa be a hello **típus gyűjtése** hello a másolási tevékenység túl**FileSystemSink**, és adja meg a következő tulajdonságok hello **fogadó** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| copyBehavior |Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer. |**PreserveHierarchy:** hello fájl hierarchia hello célmappában megőrzi. Ez azt jelenti, hogy van hello ugyanaz, mint a hello relatív elérési útja hello cél fájl toohello célmappa hello hello forrás fájl toohello forrásmappa relatív elérési út.<br/><br/>**FlattenHierarchy:** hello forrásmappából minden fájl első szintű hello célmappában jönnek létre. hello fájljaira jönnek létre automatikusan létrehozott névvel.<br/><br/>**Mergefiles típusú:** forrásfájlból hello mappa tooone minden fájl egyesíti. Ha hello fájl neve/blob neve meg van adva, a hello egyesített fájl neve az hello megadott név. Ellenkező esetben egy automatikusan létrehozott nevét. |Nem |
automatikus-

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [fájlrendszer összekötő cikk](data-factory-onprem-file-system-connector.md#copy-activity-properties).

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Társított szolgáltatások
az FTP toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**FTP-kiszolgáló**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges | Alapértelmezett |
| --- | --- | --- | --- |
| állomás |Hello FTP-kiszolgáló neve vagy IP-címe |Igen |&nbsp; |
| AuthenticationType |Adja meg a hitelesítés típusa |Igen |Alapszintű, a névtelen |
| felhasználónév |Felhasználó, aki rendelkezik hozzáférési toohello FTP-kiszolgáló |Nem |&nbsp; |
| jelszó |Jelszó hello (felhasználónév) |Nem |&nbsp; |
| encryptedCredential |Titkosított hitelesítő adatokban tooaccess hello FTP-kiszolgáló |Nem |&nbsp; |
| gatewayName |Hello az adatkezelési átjáró átjáró tooconnect tooan nevét a helyszíni FTP-kiszolgáló |Nem |&nbsp; |
| port |Mely hello FTP-kiszolgáló figyel port |Nem |21 |
| enableSsl |Adja meg, hogy toouse SSL/TLS-csatorna feletti FTP-e |Nem |Igaz |
| enableServerCertificateValidation |Adja meg, hogy a tooenable server SSL FTP használata a TLS/SSL csatornán keresztül tanúsítvány érvényesítése |Nem |Igaz |

#### <a name="example-using-anonymous-authentication"></a>Példa: A névtelen hitelesítést használó

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a>Példa: Használatával felhasználónevet és jelszót egyszerű szövegként az egyszerű hitelesítés

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a>Példa: Port, enableSsl, enableServerCertificateValidation használatával

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Példa: EncryptedCredential használata a hitelesítéshez és az átjáró

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine egy FTP-adatkészlet, set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Elérési út toohello almappa. Használja az escape-karakter "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő. |Igen 
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne: <br/><br/>Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| fileFilter |Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.<br/><br/>Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).<br/><br/>1. példa:`"fileFilter": "*.log"`<br/>2. példa:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez. Ez a tulajdonság a HDFS nem támogatott. |Nem |
| partitionedBy |partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét. Például folderPath adatok óránkénti paraméteres. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**; és a támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |
| useBinaryTransfer |Adja meg, hogy a bináris átviteli mód használata. A bináris mód és a hamis értéket ASCII igaz. Alapértelmezett érték: igaz. A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló. |Nem |

> [!NOTE]
> fájlnév és fileFilter nem használható egyszerre.

#### <a name="example"></a>Példa

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#dataset-properties) cikk.

### <a name="file-system-source-in-copy-activity"></a>A másolási tevékenység rendszer forrás
Adatok másolása az FTP-kiszolgálóról, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |IGAZ, hamis (alapértelmezés) |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

További információkért lásd: [FTP-összekötő](data-factory-ftp-connector.md#copy-activity-properties) cikk.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Társított szolgáltatások
a HDFS toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Hdfs**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **Hdfs** |Igen |
| URL-cím |URL-cím toohello HDFS |Igen |
| AuthenticationType |Névtelen, vagy a Windows. <br><br> toouse **Kerberos-hitelesítés** HDFS-összekötőhöz, tekintse meg túl[ebben a szakaszban](#use-kerberos-authentication-for-hdfs-connector) a helyszíni környezet tooset ennek megfelelően. |Igen |
| Felhasználónév |Felhasználónév a Windows-hitelesítést. |Igen (a Windows-hitelesítés) |
| jelszó |A Windows-hitelesítés jelszót. |Igen (a Windows-hitelesítés) |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello HDFS kell használnia. |Igen |
| encryptedCredential |[Új AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) hello hozzáférési hitelesítő adatok kimenetét. |Nem |

#### <a name="example-using-anonymous-authentication"></a>Példa: A névtelen hitelesítést használó

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a>Példa: A Windows-hitelesítés használatával

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine a HDFS dataset set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Toohello mappa elérési útja. Példa:`myfolder`<br/><br/>Használja az escape-karakter "\" hello karakterlánc speciális karakter. Például: folder\subfolder, adja meg a mappa\\\\almappa és d:\samplefolder, adja meg a d:\\\\mappába.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő. |Igen |
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne: <br/><br/>Adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| partitionedBy |partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét. Példa: folderPath adatok óránkénti paraméteres. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

> [!NOTE]
> fájlnév és fileFilter nem használható egyszerre.

#### <a name="example"></a>Példa

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#dataset-properties) cikk. 

### <a name="file-system-source-in-copy-activity"></a>A másolási tevékenység rendszer forrás
Adatok másolása a HDFS, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:

**FileSystemSource** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |IGAZ, hamis (alapértelmezés) |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

További információkért lásd: [HDFS összekötő](#data-factory-hdfs-connector.md#copy-activity-properties) cikk.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Társított szolgáltatások
az SFTP toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Sftp**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- | --- |
| állomás | Hello SFTP kiszolgáló neve vagy IP-címét. |Igen |
| port |Az port, mely hello SFTP kiszolgáló figyel. hello alapértelmezett érték: 21. |Nem |
| AuthenticationType |Adja meg a hitelesítés típusát. Megengedett értékek: **alapvető**, **SshPublicKey**. <br><br> Tekintse meg a túl[használja az egyszerű hitelesítés](#using-basic-authentication) és [használatával SSH nyilvános kulcsos hitelesítés](#using-ssh-public-key-authentication) további tulajdonságokat és JSON-minták szakasz. |Igen |
| skipHostKeyValidation | Adja meg, hogy tooskip gazdagép kulcs érvényesítése. | Nem. alapértelmezett érték hello: hamis |
| hostKeyFingerprint | Adja meg a hello ujjlenyomat hello állomás kulcs. | Igen, ha a hello `skipHostKeyValidation` toofalse van beállítva.  |
| gatewayName |Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni SFTP kiszolgáló. | Igen, ha az adatok másolása egy helyszíni SFTP-kiszolgálón. |
| encryptedCredential | Titkosított hitelesítő adatokban tooaccess hello SFTP kiszolgáló. Automatikusan létrehozott Ha megadja az egyszerű hitelesítés (felhasználónév + jelszó) vagy az SshPublicKey hitelesítési (felhasználónév + titkos kulcs elérési útja vagy tartalom) másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen. | Nem. Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni. |

#### <a name="example-using-basic-authentication"></a>Például: Alapszintű hitelesítést használ

Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `Basic`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- | --- |
| felhasználónév | Az felhasználó, aki rendelkezik toohello SFTP kiszolgálót. |Igen |
| jelszó | (Felhasználónév) hello felhasználó jelszavát. | Igen |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Példa: Egyszerű hitelesítést a titkosított hitelesítő adat **

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a>SSH nyilvános kulcsos hitelesítés használatával: **

Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `SshPublicKey`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- | --- |
| felhasználónév |Felhasználó, aki rendelkezik toohello SFTP kiszolgáló |Igen |
| privateKeyPath | Adjon meg abszolút elérési út toohello titkos kulcsot tartalmazó fájlt, hogy az átjáró férhetnek hozzá. | Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`. <br><br> Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni. |
| privateKeyContent | Hello titkos kulcs tartalmát, mivel a szerializált karakterlánc. hello másolása varázsló olvashatja hello titkos kulcsot tartalmazó fájlt, és bontsa ki a titkos kulcs tartalmát hello automatikusan. Minden egyéb eszköz/SDK használatakor használja hello privateKeyPath tulajdonságot. | Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`. |
| hozzáférési kód | Adja meg hello hozzáférési kódot vagy jelszót toodecrypt hello titkos kulcsot, ha hello kulcsfájl védi egy hozzáférési kódot. | Igen, ha a hozzáférési kód hello titkos kulcsot tartalmazó fájlt védi. |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Példa: Az SshPublicKey hitelesítés titkos kulcs tartalom **

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine az SFTP adatkészlethez set hello **típus** hello adatkészlet túl**fájlmegosztási**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| folderPath |Elérési út toohello almappa. Használja az escape-karakter "\" hello karakterlánc speciális karakter. Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.<br/><br/>Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő. |Igen |
| fileName |Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában. Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.<br/><br/>Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne: <br/><br/>Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nem |
| fileFilter |Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.<br/><br/>Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).<br/><br/>1. példa:`"fileFilter": "*.log"`<br/>2. példa:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez. Ez a tulajdonság a HDFS nem támogatott. |Nem |
| partitionedBy |partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét. Például folderPath adatok óránkénti paraméteres. |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |
| useBinaryTransfer |Adja meg, hogy a bináris átviteli mód használata. A bináris mód és a hamis értéket ASCII igaz. Alapértelmezett érték: igaz. A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló. |Nem |

> [!NOTE]
> fájlnév és fileFilter nem használható egyszerre.

#### <a name="example"></a>Példa

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#dataset-properties) cikk. 

### <a name="file-system-source-in-copy-activity"></a>A másolási tevékenység rendszer forrás
SFTP forrásból származó adatok másolása, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**FileSystemSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |IGAZ, hamis (alapértelmezés) |Nem |



#### <a name="example"></a>Példa

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

További információkért lásd: [SFTP összekötő](data-factory-sftp-connector.md#copy-activity-properties) cikk.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Társított szolgáltatások
a HTTP toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Http**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| URL-címe | Alap URL-cím toohello webkiszolgáló | Igen |
| AuthenticationType | Hello hitelesítés típusát határozza meg. Két érték engedélyezett: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, **ClientCertificate**. <br><br> További tulajdonságokat és JSON-minták a táblázat alatti toosections rendre adott hitelesítési típusok olvassa. | Igen |
| enableServerCertificateValidation | Adja meg, hogy tooenable server SSL tanúsítvány érvényesítése, ha forrás HTTPS webkiszolgáló | Nem, alapértelmezett érték true |
| gatewayName | Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni HTTP-forrás. | Igen, ha a helyszíni HTTP forrásból származó adatok másolása. |
| encryptedCredential | Titkosított hitelesítő adatokban tooaccess hello HTTP-végpont. Automatikusan létrehozott hello hitelesítési adatok másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen konfigurálásakor. | Nem. Csak akkor, ha az adatok másolása helyi HTTP-kiszolgáló alkalmazni. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Példa: Basic, a kivonatoló vagy a Windows-hitelesítés használatával
Állítsa be `authenticationType` , `Basic`, `Digest`, vagy `Windows`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| felhasználónév | Felhasználónév tooaccess hello HTTP-végpont. | Igen |
| jelszó | (Felhasználónév) hello felhasználó jelszavát. | Igen |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a>Példa: ClientCertificate hitelesítés használatával

Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `ClientCertificate`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| embeddedCertData | hello Base64-kódolású tartalmak a bináris adatok hello személyes információcseréhez kapcsolódó (PFX) fájl. | Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`. |
| CertThumbprint | hello telepítve lett az átjáró gépen tanúsítványtároló hello tanúsítvány ujjlenyomata. Csak akkor, ha a helyszíni HTTP forrásból származó adat másolása alkalmazni. | Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`. |
| jelszó | Hello tanúsítványhoz tartozó jelszót. | Nem |

Ha `certThumbprint` hitelesítési és hello tanúsítvány telepítve van a hello hello helyi számítógép személyes tárolójában, kell toogrant hello olvasási hozzáférést toohello átjáró szolgáltatás:

1. Indítsa el a Microsoft Management Console (MMC). Adja hozzá a hello **tanúsítványok** beépülő modul a célok hello **helyi számítógép**.
2. Bontsa ki a **tanúsítványok**, **személyes**, és kattintson a **tanúsítványok**.
3. Kattintson a jobb gombbal a hello tanúsítványt hello személyes tárolójában, és válassza ki **feladataival**->**titkos kulcsok kezelése...**
3. A hello **biztonsági** lapon maradva adja hozzá a hello felhasználói fiók alatt az adatkezelési átjáró gazdaszolgáltatása fut. hello olvasási hozzáférés toohello tanúsítvánnyal.  

**Példa: ügyfél-tanúsítványt használ:** a társított szolgáltatás hivatkozások a data factory tooan helyszíni HTTP webkiszolgáló. Az adatkezelési átjáró telepített hello gépen telepített ügyféltanúsítványt használ.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Példa: ügyfél-tanúsítványt használ egy fájlban
Ez a data factory tooan helyszíni HTTP webkiszolgáló kapcsolódó szolgáltatás hivatkozásokat. Az adatkezelési átjáró telepítése egy ügyfél tanúsítványfájl hello gépen használ.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine egy HTTP-adatkészlet, set hello **típus** hello adatkészlet túl**Http**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| relativeUrl | Relatív URL-cím toohello erőforrás hello adatokat tartalmaz. Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál. <br><br> tooconstruct dinamikus URL-címe, használhat [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md), például: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Nem |
| requestMethod | HTTP-metódus. Két érték engedélyezett **beolvasása** vagy **POST**. | Nem. Alapértelmezett érték a `GET`. |
| additionalHeaders | További HTTP-kérelemfejlécekben. | Nem |
| requestBody | A HTTP-kérelmek törzsében. | Nem |
| Formátumban | Ha azt szeretné, hogy toosimply **hello adatainak lekérése, a HTTP-végpont-van** nélkül elemzés azt, hagyja ki a formátumot beállítások. <br><br> Ha szeretné tooparse hello HTTP-válasz tartalom másolása során, a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. |Nem |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

#### <a name="example-using-hello-get-default-method"></a>Példa: hello (alapértelmezett) GET metódussal

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-hello-post-method"></a>Példa: hello POST metódussal

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#dataset-properties) cikk.

### <a name="http-source-in-copy-activity"></a>A másolási tevékenység HTTP-forrás
Adatok másolása egy HTTP-bejegyzéseket, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**HttpSource**, és adja meg a következő tulajdonságok hello **forrás** szakasz:

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| httpRequestTimeout | hello hello HTTP kérelem tooget választ (időtartam) időkorlátját. Hello időtúllépés tooget választ, hello időtúllépés tooread érkezett válasz adatait is. | Nem. Alapértelmezett érték: 00:01:40 |


#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [HTTP összekötő](data-factory-http-connector.md#copy-activity-properties) cikk.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Társított szolgáltatások
egy OData toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OData**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| URL-címe |Hello OData-szolgáltatás URL-címét. |Igen |
| AuthenticationType |Tooconnect toohello OData-forrásra használt hitelesítés típusa. <br/><br/> A felhőbeli OData a lehetséges értékek: névtelen, alapszintű és OAuth (Megjegyzés: jelenleg csak Azure Data Factory támogatási Azure Active Directory-alapú OAuth). <br/><br/> A helyszíni OData lehetséges értékei a névtelen, alapszintű és a Windows. |Igen |
| felhasználónév |Ha egyszerű hitelesítést használ, adja meg a felhasználónevet. |Igen (Ha csak az egyszerű hitelesítés használata esetén) |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Igen (Ha csak az egyszerű hitelesítés használata esetén) |
| authorizedCredential |Ha OAuth használ, kattintson a **engedélyezés** hello Data Factory másolása varázsló vagy a szerkesztő gombra, és adja meg a hitelesítő adatok, akkor a tulajdonság értékének hello lesz automatikusan generált. |Igen (csak ha OAuth-hitelesítés használata esetén) |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni OData-szolgáltatás. Csak adja meg, ha a másolt adatokat a helyszíni OData-forrásra. |Nem |

#### <a name="example---using-basic-authentication"></a>Példa – egyszerű hitelesítés használata
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a>Példa - névtelen hitelesítés használatával

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Példa - használatával Windows hitelesítés használata a helyszíni OData-forrásra

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Példa - felhő OData-forrásra elérése OAuth-hitelesítés használatával
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

További információkért lásd: [OData összekötő](data-factory-odata-connector.md#linked-service-properties) cikk.

### <a name="dataset"></a>Adatkészlet
toodefine egy OData adatkészlet set hello **típus** hello adatkészlet túl**ODataResource**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| Elérési út |Elérési út toohello OData-erőforrás |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

További információkért lásd: [OData összekötő](data-factory-odata-connector.md#dataset-properties) cikk.

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
OData forrásból származó adatok másolása, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Példa | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |"? $select neve, leírása és $top = = 5" |Nem |

#### <a name="example"></a>Példa

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

További információkért lásd: [OData összekötő](data-factory-odata-connector.md#copy-activity-properties) cikk.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Társított szolgáltatások
az ODBC toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**OnPremisesOdbc**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| connectionString |hello nem hozzáférési hitelesítő adatok része hello kapcsolati karakterláncot, és egy nem kötelező hitelesítő adat titkosítva. Példák a következő részekben hello. |Igen |
| hitelesítő adatok |hello hozzáférési hitelesítő adatok része, illesztőprogram-specifikus tulajdonság-érték formátumban megadott hello kapcsolati karakterlánc. Példa: "Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>; ". |Nem |
| AuthenticationType |Tooconnect toohello ODBC adattár használt hitelesítés típusa. Lehetséges értékek a következők: névtelen és alapvető. |Igen |
| felhasználónév |Ha egyszerű hitelesítést használ, adja meg a felhasználónevet. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello ODBC adattárat kell használnia. |Igen |

#### <a name="example---using-basic-authentication"></a>Példa – egyszerű hitelesítés használata

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Példa – egyszerű hitelesítés használata a titkosított hitelesítő adatokat
Hello hitelesítő adatok hello segítségével titkosíthatja [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (Azure PowerShell 1.0-ás verziója) parancsmag vagy [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 vagy korábbi verziójú hello Az Azure PowerShell).  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a>Példa: A névtelen hitelesítést használó

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy ODBC adatkészlet set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello ODBC adattár hello tábla neve. |Igen |


#### <a name="example"></a>Példa

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#dataset-properties) cikk. 

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása egy ODBC adattárból, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: `select * from MyTable`. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

További információkért lásd: [ODBC összekötő](data-factory-odbc-connector.md#copy-activity-properties) cikk.

## <a name="salesforce"></a>Salesforce


### <a name="linked-service"></a>Társított szolgáltatások
a Salesforce toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**Salesforce**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| environmentUrl | Adja meg a hello URL-címet a Salesforce-példány. <br><br> -Alapértelmezett érték a "https://login.salesforce.com". <br> -toocopy adatok védőfalak, adja meg a "https://test.salesforce.com". <br> -toocopy adatait egyéni tartományt, adja meg, például "https://[domain].my.salesforce.com". |Nem |
| felhasználónév |Adjon meg egy felhasználónevet hello felhasználói fiókhoz. |Igen |
| jelszó |Adjon meg egy hello felhasználói fiók jelszavát. |Igen |
| securityToken |Adjon meg egy biztonsági jogkivonatot hello felhasználói fiókhoz. Lásd: [biztonsági jogkivonatának beszerzéséhez](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) útmutatást tooreset/get egy biztonsági jogkivonatot. biztonsági jogkivonatok kapcsolatos toolearn általában, lásd: [biztonsági és hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine a Salesforce-adatkészlet set hello **típus** hello adatkészlet túl**RelationalTable**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |A Salesforce-ban hello tábla neve. |Nem (Ha egy **lekérdezés** a **RelationalSource** van megadva) |

#### <a name="example"></a>Példa

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#dataset-properties) cikk. 

### <a name="relational-source-in-copy-activity"></a>A másolási tevékenység relációs adatforrás
Adatok másolása a Salesforce, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**RelationalSource**, és adja meg a következő tulajdonságok hello **forrás** a szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |Egy SQL-92 lekérdezés vagy [Salesforce objektum Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) lekérdezés. Például: `select * from MyTable__c`. |Nem (ha hello **tableName** a hello **dataset** van megadva) |

#### <a name="example"></a>Példa  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> hello API neve "__c" részét hello bármilyen egyéni objektum szükséges.

További információkért lásd: [Salesforce összekötő](data-factory-salesforce-connector.md#copy-activity-properties) cikk. 

## <a name="web-data"></a>Webes adatok 

### <a name="linked-service"></a>Társított szolgáltatások
a webes toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**webes**, és adja meg a következő tulajdonságok hello **typeProperties** szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| URL-cím |URL-cím toohello webes forrás |Igen |
| AuthenticationType |Névtelen. |Igen |
 

#### <a name="example"></a>Példa


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#linked-service-properties) cikk. 

### <a name="dataset"></a>Adatkészlet
toodefine egy webes dataset set hello **típus** hello adatkészlet túl**Webtábla**, és adja meg a következő tulajdonságai a hello hello **typeProperties** szakasz: 

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type |Hello dataset típusa. be kell állítani túl**Webtábla** |Igen |
| Elérési út |Relatív URL-cím toohello erőforrás hello táblát tartalmaz. |Nem. Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál. |
| Index |hello tábla hello erőforrás hello indexe. Lásd: [Get index egy tábla egy HTML-lapon](#get-index-of-a-table-in-an-html-page) szakasz lépéseit toogetting index egy tábla egy HTML-lapon. |Igen |

#### <a name="example"></a>Példa

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#dataset-properties) cikk. 

### <a name="web-source-in-copy-activity"></a>A másolási tevékenység webes forrás
Adatok másolása egy webes táblából, állítsa be a hello **adatforrástípust** hello a másolási tevékenység túl**WebSource**. Ha a másolási tevékenység hello forrás jelenleg típusú **WebSource**, további tulajdonságok nem támogatottak.

#### <a name="example"></a>Példa

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

További információkért lásd: [webes tábla összekötő](data-factory-web-table-connector.md#copy-activity-properties) cikk. 

## <a name="compute-environments"></a>SZÁMÍTÁSI KÖRNYEZETEK
hello következő táblázatban hello számítási környezetek futtathat rajtuk adat-előállító és hello átalakítása tevékenységek által támogatott. Ön hello számítási hello hivatkozásra kattintva érdeklődik toosee hello JSON-sémák társított szolgáltatás toolink azt tooa adat-előállítóban. 

| Számítási környezet | Tevékenységek |
| --- | --- |
| [Igény szerinti HDInsight-fürt](#on-demand-azure-hdinsight-cluster) vagy [saját HDInsight-fürt](#existing-azure-hdinsight-cluster) |[.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce művelethez](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [ A Spark-tevékenység](#hdinsight-spark-activity) |
| [Az Azure Batch](#azure-batch) |[.NET egyéni tevékenység](#net-custom-activity) |
| [Azure Machine Learning](#azure-machine-learning) | [Számítógép-kötegelt végrehajtási tevékenység tanulási](#machine-learning-batch-execution-activity), [gép tanulási Update-Erőforrástevékenység](#machine-learning-update-resource-activity) |
| [Az Azure Data Lake Analytics](#azure-data-lake-analytics) |[Data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) |
| [Az Azure SQL adatbázis](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1) |[Tárolt eljárás](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>Igény szerinti Azure HDInsight-fürt
hello Azure Data Factory szolgáltatásnak automatikusan hozhat létre egy Windows/Linux-alapú igény szerinti HDInsight fürt tooprocess adatok. hello fürt hello és hello (hello JSON linkedServiceName tulajdonsága) storage-fiók ugyanabban a régióban társított hello fürt jön létre. Átalakítás tevékenységek a szolgáltatásnak a következő hello futtatása: [.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce művelethez ](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [tevékenység Spark](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Társított szolgáltatások 
a következő táblázat hello hello Azure JSON-definícióból igény szerinti HDInsight kapcsolódó szolgáltatási használt hello tulajdonságok ismerteti.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello típusú tulajdonságot kell beállítani, túl**HDInsightOnDemand**. |Igen |
| Nagyobbnak |Hello fürt/adatok csomópontok száma. Ez a tulajdonság a megadott munkavégző csomópontokhoz hello száma együtt 2 átjárócsomópontokkal hello HDInsight-fürt jön létre. hello csomópontra van, amely nem rendelkezik 4 mag, így egy 4 munkavégző csomópontot tartalmazó fürtben veszi 24 mag standard, D3 méretű (4\*a munkavégző csomópontokról, valamint 2 processzormag, 4 = 16\*az átjárócsomópontokkal processzormag, 4 = 8). Lásd: [hdinsight létrehozása Linux-alapú Hadoop-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) hello standard, D3 réteg vonatkozó további információért. |Igen |
| a TimeToLive tulajdonság |hello megengedett üresjárati idő hello igény szerinti HDInsight-fürthöz. Meghatározza, mennyi ideig hello igény szerinti HDInsight-fürt aktív marad a művelet végrehajtása, ha nincsenek más aktív feladatok hello fürt tevékenység befejezését követően.<br/><br/>Például ha egy tevékenységfuttatási fogad 6 percnél és timetolive nem beállítása too5 perc, hello fürt marad a 5 perc után hello életben hello tevékenységfuttatási feldolgozásának 6 percnél. Ha egy másik tevékenységfuttatási hello 6 percnél ablak, hello által feldolgozott ugyanabban a fürtben.<br/><br/>Igény szerinti HDInsight-fürtök létrehozása során költséges (igénybe vehet), így használják ezt a beállítást egy adat-előállító szükséges tooimprove teljesítményének újból felhasználja az igény szerinti HDInsight-fürtök által.<br/><br/>Ha timetolive érték too0, amint hello tevékenység fut feldolgozott hello-fürtök törlése. A hello ugyanakkor, ha a magas érték, hello fürt felfüggesztheti üresjárati feleslegesen magas költségeket eredményez. Ezért fontos, hogy állítsa a igények alapján hello megfelelő értékre.<br/><br/>Több folyamatok megoszthatja hello hello igény szerinti HDInsight-fürt ugyanazt a példányát, ha hello timetolive tulajdonság értékének megfelelően van beállítva. |Igen |
| Verzió |Hello HDInsight-fürt verziószáma. További információkért lásd: [HDInsight-verziókról támogatott az Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). |Nem |
| linkedServiceName |Az Azure Storage társított szolgáltatás toobe történő tárolására és feldolgozására adatok hello igény fürt által használt. <p>Jelenleg nem hozható létre, amely egy Azure Data Lake Store hello tárolóként használ igény szerinti HDInsight-fürtöt. Ha azt szeretné, hogy toostore hello ezért az adatok a HDInsight-feldolgozás alatt álló egy Azure Data Lake Store-ból, a hello Azure Blob Storage toohello Azure Data Lake Store másolási tevékenység toocopy hello adatait használják.</p>  | Igen |
| additionalLinkedServiceNames |Adja meg a további tárfiókok hello HDInsight a társított szolgáltatás, így hello Data Factory szolgáltatásnak is regisztrálja őket az Ön nevében. |Nem |
| osType |Az operációs rendszer típusát. Két érték engedélyezett: (alapértelmezett) Windows és Linux |Nem |
| hcatalogLinkedServiceName |Azure SQL társított hello neve szolgáltatási pont toohello HCatalog adatbázis. hello igény szerinti HDInsight-fürt létrehozása hello metaadattárhoz hello Azure SQL database segítségével. |Nem |

### <a name="json-example"></a>JSON-példa
a következő JSON hello igény kapcsolódó HDInsight Linux-alapú szolgáltatás határozza meg. hello Data Factory szolgáltatásnak automatikusan létrehoz egy **Linux-alapú** HDInsight-fürt adatok szelet feldolgozása közben. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

További információkért lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) cikk. 

## <a name="existing-azure-hdinsight-cluster"></a>Meglévő Azure HDInsight-fürt
A Data Factory egy Azure HDInsight kapcsolódó szolgáltatás tooregister saját HDInsight-fürtöt hozhat létre. Adatok átalakítása tevékenységek a szolgáltatásnak a következő hello is futtathatja: [.NET egyéni tevékenység](#net-custom-activity), [tevékenység Hive](#hdinsight-hive-activity), [tevékenység sertésfelmérés] (hdinsight-pig-tevékenység # [MapReduce tevékenység](#hdinsight-mapreduce-activity), [Hadoop-Stream tevékenység](#hdinsight-streaming-activityd), [tevékenység Spark](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Társított szolgáltatások
hello a következő táblázat ismerteti az Azure HDInsight társított szolgáltatásnak Azure JSON-definícióból hello használt hello tulajdonságok.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello típusú tulajdonságot kell beállítani, túl**HDInsight**. |Igen |
| clusterUri |hello hello HDInsight-fürt URI. |Igen |
| felhasználónév |Adja meg felhasználói toobe hello hello nevét használja tooconnect tooan meglévő HDInsight-fürtre. |Igen |
| jelszó |Adja meg a hello felhasználói fiókhoz tartozó jelszót. |Igen |
| linkedServiceName | Hello toohello Azure blob storage Azure Storage társított szolgáltatás neve hello által használt HDInsight-fürthöz. <p>Jelenleg nem adhat meg egy Azure Data Lake Store társított szolgáltatás ehhez a tulajdonsághoz. Előfordulhat, hogy éri hello Azure Data Lake Store a Hive/Pig-parancsfájlok hozzáférés toohello Data Lake Store hello HDInsight-fürt-e. </p>  |Igen |

A HDInsight-fürtök támogatott verzióit lásd: [HDInsight-verziókról támogatott](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). 

#### <a name="json-example"></a>JSON-példa

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a>Azure Batch
A data Factory egy csatolt Azure Batch szolgáltatás tooregister Batch-készlet a virtuális gépek (VM) hozhat létre. .NET-egyéni tevékenységek Azure Batch vagy Azure HDInsight segítségével is futtathatja. Futtathatja a [.NET egyéni tevékenység](#net-custom-activity) a társított szolgáltatás. 

### <a name="linked-service"></a>Társított szolgáltatások
a következő táblázat hello hello Azure JSON-definícióból egy csatolt Azure Batch szolgáltatás használt hello tulajdonságok ismerteti.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello típusú tulajdonságot kell beállítani, túl**AzureBatch**. |Igen |
| Fióknév |Hello Azure Batch-fiók neve. |Igen |
| accessKey |Hello Azure Batch-fiók elérési kulcsának. |Igen |
| poolName |A virtuális gépek hello készlet neve. |Igen |
| linkedServiceName |Hello a kapcsolódó Azure Batch-szolgáltatás társított Azure Storage társított szolgáltatás neve. A társított szolgáltatás szolgál átmeneti fájlok toorun hello tevékenység és a hello tevékenység végrehajtási naplók tárolásához szükséges. |Igen |


#### <a name="json-example"></a>JSON-példa

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a>Azure Machine Learning
A Machine Learning kötegelt pontozási végpont a data Factory létrehozása az Azure Machine Learning kapcsolódó szolgáltatás tooregister. A társított szolgáltatás futtatható ez két adatok átalakítása tevékenységek: [Machine Learning kötegelt végrehajtási tevékenység](#machine-learning-batch-execution-activity), [Machine Learning Update-Erőforrástevékenység](#machine-learning-update-resource-activity). 

### <a name="linked-service"></a>Társított szolgáltatások
hello a következő táblázat ismerteti az Azure Machine Learning társított szolgáltatásnak Azure JSON-definícióból hello használt hello tulajdonságok.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| Típus |hello tulajdonságra kell megadni: **AzureML**. |Igen |
| mlEndpoint |hello kötegelt pontozás URL-CÍMÉT. |Igen |
| apiKey |hello közzétett munkaterület-modell API. |Igen |

#### <a name="json-example"></a>JSON-példa

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
Létrehozhat egy **Azure Data Lake Analytics** szolgáltatás toolink egy Azure Data Lake Analytics számítási szolgáltatás tooan az Azure data factory kapcsolt hello használata előtt [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md) egy folyamaton belül .

### <a name="linked-service"></a>Társított szolgáltatások

hello a következő táblázat ismerteti az Azure Data Lake Analytics társított szolgáltatás JSON-definícióból hello használt hello tulajdonságok. 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| Típus |hello tulajdonságra kell megadni: **AzureDataLakeAnalytics**. |Igen |
| Fióknév |Az Azure Data Lake Analytics-fiók neve. |Igen |
| datalakeanalyticsuri paraméter |Az Azure Data Lake Analytics URI. |Nem |
| Engedélyezési |Engedélyezési kód automatikusan beolvassa kattintás után **engedélyezés** gombra a Data Factory Editor hello és épp hello OAuth-bejelentkezés. |Igen |
| subscriptionId |Az Azure előfizetés-azonosító |Nem (Ha nincs megadva, az adat-előállító használt hello előfizetés). |
| erőforráscsoport-név |Azure erőforráscsoport-név |Nem (Ha nincs megadva, az adat-előállító használt hello erőforráscsoport). |
| Munkamenet-azonosító |munkamenet-azonosító hello OAuth hitelesítési munkamenetből. Minden munkamenet-azonosító egyedi, és előfordulhat, hogy csak egyszer használható. Data Factory Editor hello használata esetén ezt az Azonosítót jön létre automatikusan. |Igen |


#### <a name="json-example"></a>JSON-példa
a következő példa hello biztosít az Azure Data Lake Analytics társított szolgáltatás JSON-definícióból.

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a>Azure SQL Database
Hozzon létre egy Azure SQL társított szolgáltatást, és használatához a hello [tárolt eljárási tevékenység](#stored-procedure-activity) tooinvoke a Data Factory-folyamathoz tárolt eljárást. 

### <a name="linked-service"></a>Társított szolgáltatások
egy Azure SQL Database toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDatabase**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| connectionString |Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Database-példányt. |Igen |

#### <a name="json-example"></a>JSON-példa

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Lásd: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#linked-service-properties) szóló cikkben olvashat a szolgáltatásnak.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Azure SQL Data Warehouse társított szolgáltatás létrehozása és használatához a hello [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) tooinvoke a Data Factory-folyamathoz tárolt eljárást. 

### <a name="linked-service"></a>Társított szolgáltatások
Azure SQL Data Warehouse toodefine társított szolgáltatás, állítsa hello **típus** hello a társított szolgáltatás túl**AzureSqlDW**, és adja meg a következő tulajdonságok hello **typeProperties**szakasz:  

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| connectionString |Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Data Warehouse-példányhoz. |Igen |

#### <a name="json-example"></a>JSON-példa

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

További információkért lásd: [Azure SQL Data Warehouse-összekötő](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) cikk. 

## <a name="sql-server"></a>SQL Server 
Hozzon létre csatolt SQL Server szolgáltatást, és használatához a hello [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) tooinvoke a Data Factory-folyamathoz tárolt eljárást. 

### <a name="linked-service"></a>Társított szolgáltatások
Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** toolink egy helyi SQL Server adatbázis tooa adat-előállítóban. a következő táblázat hello biztosít JSON elemek adott tooon-hez kapcsolódó SQL Server szolgáltatás leírását.

a következő táblázat hello biztosít JSON-elemek adott tooSQL csatolt kiszolgáló szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello tulajdonságra kell megadni: **OnPremisesSqlServer**. |Igen |
| connectionString |Adja meg a connectionString információ tooconnect toohello a helyszíni SQL Server-adatbázis SQL-hitelesítéssel vagy a Windows-hitelesítés szükséges. |Igen |
| gatewayName |Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello a helyszíni SQL Server-adatbázist használja. |Igen |
| felhasználónév |Adja meg a felhasználónevet, ha a Windows-hitelesítést használ. Példa: **tartománynév\\felhasználónév**. |Nem |
| jelszó |Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót. |Nem |

Hitelesítő adatok hello segítségével titkosíthatja **New-AzureRmDataFactoryEncryptValue** parancsmag, amelyekkel hello kapcsolati karakterlánc, ahogy az alábbi példa hello (**EncryptedCredential** tulajdonság):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Példa: JSON az SQL-hitelesítéssel

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Példa: JSON a Windows-hitelesítés használatával

Ha a felhasználónév és jelszó van adva, átjáró használja őket tooimpersonate hello megadott felhasználói fiók tooconnect toohello a helyszíni SQL Server-adatbázist. Ellenkező esetben átjáró csatlakozik toohello SQL Server közvetlenül (az indítási fiókjához) átjáró hello biztonsági környezetében.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

További információkért lásd: [SQL Server-összekötő](data-factory-sqlserver-connector.md#linked-service-properties) cikk.

## <a name="data-transformation-activities"></a>ADATOK ÁTALAKÍTÁSA TEVÉKENYSÉGEK

Tevékenység | Leírás
-------- | -----------
[HDInsight Hive tevékenység](#hdinsight-hive-activity) | a Data Factory-folyamathoz HDInsight Hive tevékenység hello Hive-lekérdezéseket a saját vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre. 
[HDInsight Pig tevékenység](#hdinsight-pig-activity) | a Data Factory-folyamathoz HDInsight Pig tevékenység hello Pig lekérdezések saját vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.
[HDInsight MapReduce-tevékenység](#hdinsight-mapreduce-activity) | hello HDInsight MapReduce művelethez a Data Factory-folyamat saját MapReduce programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.
[HDInsight Streaming-tevékenység](#hdinsight-streaming-activity) | hello Streamelési tevékenységben HDInsight a Data Factory-folyamat saját Hadoop Streamelési programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre.
[HDInsight Spark-tevékenység](#hdinsight-spark-activity) | a Data Factory-folyamathoz HDInsight Spark tevékenység hello Spark programok saját HDInsight-fürt hajtja végre. 
[Machine Learning kötegelt végrehajtási tevékenység](#machine-learning-batch-execution-activity) | Az Azure Data Factory lehetővé teszi, hogy Ön tooeasily létrehozása folyamatok, amelyek egy közzétett Azure Machine Learning a prediktív elemzés webszolgáltatás. Egy Azure Data Factory-folyamat a kötegelt végrehajtási tevékenység hello használ, a Machine Learning web toomake előrejelzések kötegben hello adatokon hívhat meg. 
[Machine Learning Update-erőforrástevékenység](#machine-learning-update-resource-activity) | Idővel hello saját prediktív modelljeit hello pontozási kísérletek kell toobe retrained új Machine Learning bemeneti adatkészletek. Miután elkészült, az átképezési, kívánt hello webszolgáltatás pontozási tooupdate hello retrained gépi tanulási modell. Hello Update-Erőforrástevékenység tooupdate hello webszolgáltatás újonnan betanítása hello modell használható.
[Tárolt eljárási tevékenység](#stored-procedure-activity) | Hello tárolt eljárási tevékenység használható egy adat-előállító adatcsatorna tooinvoke valamelyik hello adattárolókhoz a következő tárolt eljárást: Azure SQL Database, Azure SQL Data Warehouse szolgáltatásban a vállalat SQL Server-adatbázis vagy egy Azure virtuális Gépen. 
[Data Lake Analytics U-SQL-tevékenység](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL-tevékenység egy Azure Data Lake Analytics-fürt U-SQL parancsfájlt futtat.  
[.NET egyéni tevékenység](#net-custom-activity) | Ha tootransform adatokat oly módon, amely nem támogatja a Data Factory van szüksége, hozzon létre egy egyéni tevékenység saját adatokat feldolgozó logika, és hello tevékenység hello folyamat használja. Hello egyéni .NET tevékenység toorun az Azure Batch szolgáltatás vagy az Azure HDInsight-fürtöt is konfigurálhat. 

     
## <a name="hdinsight-hive-activity"></a>HDInsight Hive-tevékenység
A következő tulajdonságok a tevékenység JSON Hive definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightHive**. Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightHive beállításakor:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| Parancsfájl |Adja meg a hello Hive parancsfájl beágyazott |Nem |
| parancsfájl elérési útja |Tároló hello Hive parancsprogram egy Azure blob Storage tárolóban, és adja meg a hello elérési toohello fájlt. Használja a "script" vagy "scriptPath" tulajdonságot. Mindkettő nem használható együtt. hello fájlnév a kis-és nagybetűket. |Nem |
| határozza meg |Kulcs/érték párok paraméterek meghatározni belül hello Hive parancsfájl segítségével történő "hiveconf" hivatkozik |Nem |

Adott toohello Hive tevékenység rendszer így ezeket a típus tulajdonságokat. Az összes tevékenység egyéb tulajdonságok (kívül hello typeProperties szakaszban) támogatottak.   

### <a name="json-example"></a>JSON-példa
a következő JSON hello egy HDInsight Hive tevékenységet egy folyamaton belül határozza meg.  

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

További információkért lásd: [Hive tevékenység](data-factory-hive-activity.md) cikk. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-tevékenység
A következő tulajdonságok a Pig tevékenység JSON-definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightPig**. Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightPig beállításakor: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| Parancsfájl |Adja meg a Pig-parancsprogram beágyazott hello |Nem |
| parancsfájl elérési útja |Hello Pig-parancsprogram egy Azure blob Storage tárolóban tárolja, és adja meg a hello elérési toohello fájlt. Használja a "script" vagy "scriptPath" tulajdonságot. Mindkettő nem használható együtt. hello fájlnév a kis-és nagybetűket. |Nem |
| határozza meg |Kulcs/érték párok paraméterek meghatározni hivatkozó belül hello Pig-parancsprogram |Nem |

Adott toohello Pig tevékenység rendszer így ezeket a típus tulajdonságokat. Az összes tevékenység egyéb tulajdonságok (kívül hello typeProperties szakaszban) támogatottak.   

### <a name="json-example"></a>JSON-példa

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

További információkért lásd: [Pig tevékenység](#data-factory-pig-activity.md) cikk. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-tevékenység
A következő tulajdonságok MapReduce tevékenység JSON-definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightMapReduce**. Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightMapReduce beállításakor: 

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| jarLinkedService | Hello nevét csatolt Azure Storage hello JAR-fájlt tartalmazó hello szolgáltatást. | Igen |
| jarfilepath tulajdonságot | Elérési út toohello JAR-fájlra a hello Azure Storage. | Igen | 
| Osztálynév | Hello fő osztály hello JAR-fájlra a neve. | Igen | 
| Argumentumok | Vesszővel elválasztott argumentumokat hello MapReduce program listáját. Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a hello MapReduce keretrendszer. toodifferentiate az argumentumok hello MapReduce argumentumokkal, érdemes lehetőség és value elemeit is argumentumként látható módon a következő példa hello (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll) | Nem | 

### <a name="json-example"></a>JSON-példa

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix toodetermine hello similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce toogenerate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

További információkért lásd: [MapReduce művelethez](data-factory-map-reduce.md) cikk. 

## <a name="hdinsight-streaming-activity"></a>HDInsight Streaming-tevékenység
A következő tulajdonságok a Hadoop Streamelési tevékenység JSON-definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightStreaming**. Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightStreaming beállításakor: 

| Tulajdonság | Leírás | 
| --- | --- |
| eseményleképező | Hello leképező végrehajtható fájl nevét. Hello példában cat.exe: hello leképező végrehajtható.| 
| Nyomáscsökkentő | Hello nyomáscsökkentő végrehajtható fájl nevét. Hello példában wc.exe: hello nyomáscsökkentő végrehajtható. | 
| Bemeneti | Hello leképező bemeneti (beleértve a hely) fájl. Hello példa: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample hello blob tároló, például/data/Gutenberg hello mappában, pedig davinci.txt hello blob. |
| Kimeneti | Kimeneti fájl (beleértve a hely) hello nyomáscsökkentő. hello Hadoop adatfolyam-feladat eredményének hello toohello helyére a tulajdonság írása. |
| filePaths | Elérési utak hello hozzárendelést és nyomáscsökkentő végrehajtható fájlok számára. Hello példa: "adfsample/example/apps/wc.exe" adfsample egy hello blob tároló, például/alkalmazások hello mappa, pedig wc.exe hello végrehajtható. | 
| fileLinkedService | Az Azure tárolás társított szolgáltatása, amely az Azure storage hello filePaths szakaszában megadott hello fájlokat tartalmazó hello jelöli. | 
| Argumentumok | Vesszővel elválasztott argumentumokat hello MapReduce program listáját. Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a hello MapReduce keretrendszer. toodifferentiate az argumentumok hello MapReduce argumentumokkal, érdemes lehetőség és value elemeit is argumentumként látható módon a következő példa hello (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll) | 
| getDebugInfo | Választható eleme. Ha ezt a beállítást tooFailure, hello naplók csak az hiba lesznek letöltve. Ha ezt a beállítást tooAll, naplók mindig letöltődnek függetlenül hello végrehajtási állapotát. | 

> [!NOTE]
> Meg kell adnia egy kimeneti adatkészletet hello a Hadoop Streamelési tevékenységben hello **kimenete** tulajdonság. Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés (óránként, naponta, stb.) lehet. Hello tevékenység bemeneti nem veszi, kihagyhatja, ha egy bemeneti adatkészlet hello tevékenység a hello megadó **bemenetek** tulajdonság.  

## <a name="json-example"></a>JSON-példa

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

További információkért lásd: [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md) cikk. 

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-tevékenység
A következő tulajdonságok Spark tevékenység JSON-definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **HDInsightSpark**. Létre kell hoznia egy HDInsight kapcsolódó szolgáltatás először és hello értékeként adja meg a hello nevét az **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooHDInsightSpark beállításakor: 

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| rootPath | hello Azure blobtároló és hello Spark fájlt tartalmazó mappát. hello fájlnév a kis-és nagybetűket. | Igen |
| entryFilePath | Relatív elérési út toohello gyökérmappájában hello Spark kódcsomag. | Igen |
| Osztálynév | Az alkalmazás Java/Spark fő osztály | Nem | 
| Argumentumok | Parancssori argumentumok toohello Spark program listáját. | Nem | 
| proxyUser | hello felhasználói fiók tooimpersonate tooexecute hello Spark program | Nem | 
| sparkConfig | Spark konfigurációs tulajdonságaiban. | Nem | 
| getDebugInfo | Itt adhatja meg, amikor hello Spark naplófájlok másolt toohello az Azure storage HDInsight-fürt által használt (vagy) leírt módon sparkJobLinkedService. Megengedett értékek: None, mindig, vagy sikertelen. Alapértelmezett érték: nincs. | Nem | 
| sparkJobLinkedService | hello Azure tárolás társított szolgáltatása, amely tárolja a hello Spark feladat fájl, a függőségeket és a naplókat.  Ha nem ad meg egy értéket ehhez a tulajdonsághoz, HDInsight-fürthöz társított hello tárolót használja a rendszer. | Nem |

### <a name="json-example"></a>JSON-példa

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
Vegye figyelembe a következő pontok hello: 

- Hello **típus** tulajdonsága túl**HDInsightSpark**.
- Hello **rootPath** értéke túl**adfspark\\pyFiles** ahol adfspark hello Azure Blob-tároló és pyFiles pedig az adott tároló finom mappát. Ebben a példában a hello Azure Blob Storage egy Spark-fürt hello társított hello. Feltöltheti a hello fájl tooa különböző Azure Storage. Ha így tesz, hozzon létre egy Azure Storage társított szolgáltatás toolink adott tárolási fiók toohello adat-előállítóban. Ezt követően adja a hello társított szolgáltatás neve hello hello értékeként **sparkJobLinkedService** tulajdonság. Lásd: [Spark-tevékenység tulajdonságai](#spark-activity-properties) Ez a tulajdonság és az egyéb hello Spark tevékenység által támogatott tulajdonságokról.
- Hello **entryFilePath** értéke toohello **test.py**, vagyis hello python-fájl. 
- Hello **getDebugInfo** tulajdonsága túl**mindig**, ami azt jelenti, hogy hello naplófájlok helyét a rendszer mindig generált (sikeres vagy sikertelen).  

    > [!IMPORTANT]
    > Azt javasoljuk, hogy nincs megadva a tulajdonság tooAlways éles környezetben kivéve, ha a probléma hibaelhárítást. 
- Hello **kimenete** szakasz tartalmaz egy kimeneti adatkészletet. Meg kell adnia egy kimeneti adatkészletet, még akkor is, ha hello spark program nem ad kimenetet. hello kimeneti adatkészlet meghajtók hello ütemezés hello adatcsatorna (óránként, naponta, stb.).

Hello tevékenységgel kapcsolatos további információkért lásd: [Spark tevékenység](data-factory-spark.md) cikk.  

## <a name="machine-learning-batch-execution-activity"></a>Machine Learning kötegelt végrehajtási tevékenység
A következő tulajdonságok az Azure ML kötegelt végrehajtási tevékenység JSON-definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **AzureMLBatchExecution**. Létre kell hoznia az Azure Machine Learning-szolgáltatásnak először és adja meg azt hello nevét hello értékeként **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooAzureMLBatchExecution beállításakor:

Tulajdonság | Leírás | Szükséges 
-------- | ----------- | --------
Típus | hello dataset toobe át az Azure ML web service hello bemenetként. Ez az adatkészlet hello bemenetek hello tevékenység is kell szerepelnie. |Használja a típus vagy webServiceInputs. | 
webServiceInputs | Adja meg az Azure ML web service hello bemeneteként átadott adatkészletek toobe. Ha hello webszolgáltatás több bemeneti adatokat fogad, tulajdonsággal hello webServiceInputs hello típus tulajdonság használata helyett. Hello által hivatkozott adatkészletek **webServiceInputs** is szerepelnie kell hello tevékenység **bemenetek**. | Használja a típus vagy webServiceInputs. | 
webServiceOutputs | hello adathalmaz kimeneti hello Azure ML web service rendelt. hello webszolgáltatás kimeneti adatokat adja vissza ehhez az adatkészlethez. | Igen | 
globalParameters | Ez a szakasz hello webszolgáltatás-paramétereket értékeket megadni. | Nem | 

### <a name="json-example"></a>JSON-példa
Ebben a példában hello tevékenység rendelkezik hello dataset **MLSqlInput** bemenetként és **MLSqlOutput** hello output típusúként. Hello **MLSqlInput** átadása egy bemeneti toohello webszolgáltatásként által hello segítségével **típus** JSON tulajdonság. Hello **MLSqlOutput** átadása egy kimeneti toohello webszolgáltatás által hello segítségével **webServiceOutputs** JSON tulajdonság. 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

Hello JSON példában hello telepítette az Azure Machine Learning Web szolgáltatás használja az olvasót, és egy író modul tooread/adatok írása az / tooan Azure SQL Database. Ez a webszolgáltatás mutatja meg a következő négy paraméterek hello: adatbázis-kiszolgáló neve, a adatbázis neve, a kiszolgáló felhasználói fiók nevét és a kiszolgáló felhasználói fiók jelszavát.

> [!NOTE]
> Csak be- és kimenetekkel hello AzureMLBatchExecution tevékenység paraméterek toohello webszolgáltatás argumentumként átadhatók. A fenti JSON részlet hello, például a MLSqlInput egy bemeneti toohello AzureMLBatchExecution tevékenység, mint egy bemeneti toohello webszolgáltatás átadott típus paraméteren keresztül.

## <a name="machine-learning-update-resource-activity"></a>Machine Learning Update-erőforrástevékenység
A következő tulajdonságok az Azure ML Update erőforrás tevékenység JSON-definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **AzureMLUpdateResource**. Létre kell hoznia az Azure Machine Learning-szolgáltatásnak először és adja meg azt hello nevét hello értékeként **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooAzureMLUpdateResource beállításakor:

Tulajdonság | Leírás | Szükséges 
-------- | ----------- | --------
trainedModelName | Hello neve retrained modell. | Igen |  
trainedModelDatasetName | Adatkészlet mutat toohello iLearner-fájlt hello átképezési művelet által visszaadott. | Igen | 

### <a name="json-example"></a>JSON-példa
hello folyamat két tevékenység rendelkezik: **AzureMLBatchExecution** és **AzureMLUpdateResource**. hello Azure ML kötegelt végrehajtási tevékenység hello betanítási adatok bemenetként vesz igénybe, és egy kimenetként iLearner-fájlt hoz létre. hello tevékenység hello képzési webszolgáltatás (a tanítási kísérletet webszolgáltatásként kitett) hív hello bevitellel betanítási adatok, és hello ilearner-fájlt kapott hello webszolgáltatás. hello placeholderBlob csak hello Azure Data Factory szolgáltatás toorun hello-folyamat által igényelt üres kimeneti adatkészlet.


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-tevékenység
A következő tulajdonságok a U-SQL tevékenység JSON-definícióban hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **DataLakeAnalyticsU-SQL**. Létre kell hoznia egy Azure Data Lake Analytics kapcsolódó szolgáltatás és adja meg azt hello nevét hello értékeként **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooDataLakeAnalyticsU-SQL beállításakor: 

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| scriptPath |Elérési út toofolder hello U-SQL parancsfájlt tartalmazó. Hello fájl neve nem kis-és nagybetűket. |Nem (Ha a parancsfájl használata) |
| scriptLinkedService |Kapcsolódó szolgáltatás, amely a hello parancsfájl toohello adat-előállító tartalmazó hello tároló |Nem (Ha a parancsfájl használata) |
| Parancsfájl |Adja meg a beágyazott parancsfájlja scriptPath és a scriptLinkedService megadása helyett. Például: "script": "Adatbázis létrehozása test". |Nem (Ha a ScriptPath tulajdonságot is és a scriptLinkedService használ) |
| degreeOfParallelism |csomópontok maximális száma hello egyidejűleg használt toorun hello feladat. |Nem |
| Prioritás |Meghatározza, hogy szereplő várólistáján szereplő feladatok közül kell lennie a kijelölt toorun először. hello hello kevesebb, hello hello elsőbbséget. |Nem |
| paraméterek |Hello U-SQL parancsfájl paramétereinek |Nem |

### <a name="json-example"></a>JSON-példa

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

További információkért lásd: [Data Lake Analytics U-SQL tevékenység](data-factory-usql-activity.md). 

## <a name="stored-procedure-activity"></a>Tárolt eljárási tevékenység
A következő tárolt eljárás tevékenység JSON-definícióban tulajdonságok hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **SqlServerStoredProcedure**. Létre kell hoznia egy valamelyik a következő összekapcsolt szolgáltatások hello és hello értékeként adja meg a kapcsolódó hello szolgáltatás hello neve **linkedServiceName** tulajdonság:

- SQL Server 
- Azure SQL Database
- Azure SQL Data Warehouse

hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooSqlServerStoredProcedure beállításakor:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| storedProcedureName |Hello Azure SQL database vagy az Azure SQL Data Warehouse hello kapcsolódó szolgáltatás, amely a kimeneti tábla által használt hello által képviselt hello hello tárolt eljárás nevét adja meg. |Igen |
| storedProcedureParameters |Adja meg a tárolt eljárás paraméter értékét. Ha toopass null egy paraméter, szintaxissal hello: "param1": (összes kisbetű) NULL értékű. Tekintse meg a következő minta toolearn e tulajdonság használatával kapcsolatos hello. |Nem |

Ha megad egy bemeneti adatkészlet, elérhetőnek kell lennie (a "Kész" állapotú) hello a tárolt eljárás tevékenység toorun. hello bemeneti adatkészlet hello tárolt eljárás nem használható paraméterként. Csak a felhasznált toocheck hello függőségi kezdési hello tárolt eljárási tevékenység előtt. Meg kell adnia egy tárolt eljárás tevékenység egy kimeneti adatkészletet. 

Kimeneti adatkészlet megadja hello **ütemezés** hello a tárolt eljárási tevékenység (óránként, heti, havi, stb.). hello kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely hivatkozik tooan Azure SQL Database vagy az Azure SQL Data Warehouse vagy a használni kívánt tárolt eljárás toorun hello SQL Server-adatbázis. hello kimeneti adatkészlet ki tud szolgálni hello tárolt eljárás későbbi feldolgozásra módon toopass hello eredményként egy másik tevékenység ([tevékenységek láncolás](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) hello folyamat. Adat-előállító azonban nem a hello kimeneti a tárolt eljárás toothis DataSet adatkészlet automatikusan írnia. Hello tárolt eljárás írások tooa SQL táblát, amely hello kimeneti adatkészlet mutat. Bizonyos esetekben hello kimeneti adatkészlet lehet egy **dummy dataset**, amellyel csak hello futtatási toospecify hello ütemezésének tárolt eljárási tevékenység.  

### <a name="json-example"></a>JSON-példa

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

További információkért lásd: [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) cikk. 

## <a name="net-custom-activity"></a>.NET egyéni tevékenység
A következő tulajdonságok .NET egyéni tevékenység JSON-definícióból hello adhatja meg. hello type tulajdonság hello tevékenységnek kell lennie: **DotNetActivity**. Létre kell hoznia egy Azure HDInsight kapcsolódó szolgáltatás, vagy egy társított Azure Batch szolgáltatás, és adja meg a hello társított szolgáltatás neve hello hello értéket **linkedServiceName** tulajdonság. hello következő tulajdonságok támogatottak hello **typeProperties** szakasz hello típusú tevékenység tooDotNetActivity beállításakor:
 
| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| AssemblyName | Hello szerelvény neve. Hello például, hogy a rendszer: **MyDotnetActivity.dll**. | Igen |
| Belépési pont |Hello IDotNetActivity felületet megvalósító hello osztály neve. Hello például, hogy a rendszer: **MyDotNetActivityNS.MyDotNetActivity** ahol a MyDotNetActivityNS hello névtere, ezért a MyDotNetActivity hello osztály.  | Igen | 
| PackageLinkedService | Hello Azure Storage társított szolgáltatás mutat, toohello blob-tároló, amely tartalmazza a hello egyéni tevékenység zip-fájl neve. Hello például, hogy a rendszer: **AzureStorageLinkedService**.| Igen |
| PackageFile | Hello zip-fájl neve. Hello például, hogy a rendszer: **customactivitycontainer/MyDotNetActivity.zip**. | Igen |
| Az ExtendedProperties | A kiterjesztett tulajdonságok, amely meghatározza, és adja át a toohello .NET-kódot. Ebben a példában hello **SliceStart** változó értéke alapján hello SliceStart rendszerváltozó tooa érték. | Nem | 

### <a name="json-example"></a>JSON-példa

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

Részletes információkért lásd: [egyéni tevékenységeket felhasználni a Data Factory](data-factory-use-custom-activities.md) cikk. 

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő oktatóanyagok hello: 

- [Oktatóanyag: hozzon létre egy folyamatot a másolási tevékenység](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Oktatóanyag: hozzon létre egy folyamatot egy hive-tevékenység](data-factory-build-your-first-pipeline-using-editor.md)