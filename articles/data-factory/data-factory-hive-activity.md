---
title: "Hive tevékenység - Azure aaaTransform adatok |} Microsoft Docs"
description: "Ismerje meg, hogy használatát egy az Azure data factory toorun Hive-lekérdezéseket a Hive tevékenység hello egy a-igény szerint vagy a saját HDInsight-fürtre."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 032400cdb8e8f9873f85b811b4ad7380f4410edf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a>Azure Data Factory használatával Hive tevékenység adatok átalakítása 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-tevékenység](data-factory-hive-activity.md) 
> * [A Pig-tevékenység](data-factory-pig-activity.md)
> * [MapReduce művelethez](data-factory-map-reduce.md)
> * [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
> * [A Spark-tevékenység](data-factory-spark.md)
> * [Machine Learning kötegelt végrehajtási tevékenység](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update-erőforrástevékenység](data-factory-azure-ml-update-resource-activity.md)
> * [Tárolt eljárási tevékenység](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md)
> * [.NET egyéni tevékenység](data-factory-use-custom-activities.md)

HDInsight Hive tevékenység egy adat-előállítóban hello [csővezeték](data-factory-create-pipelines.md) Hive-lekérdezések végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz. Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.

> [!NOTE] 
> Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt. 

## <a name="syntax"></a>Szintaxis

```JSON
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
## <a name="syntax-details"></a>Szintaxis részletei
| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| név |Hello tevékenység neve. |Igen |
| leírás |Milyen hello tevékenységgel a leíró szöveg |Nem |
| type |HDinsightHive |Igen |
| Bemenetek |Hello Hive tevékenység által felhasznált bemeneti |Nem |
| kimenetek |Hello Hive tevékenység által létrehozott kimenet |Igen |
| linkedServiceName |A Data Factory kapcsolt szolgáltatásként regisztrált hivatkozás toohello HDInsight-fürt |Igen |
| Parancsfájl |Adja meg a hello Hive parancsfájl beágyazott |Nem |
| parancsfájl elérési útja |Tároló hello Hive parancsprogram egy Azure blob Storage tárolóban, és adja meg a hello elérési toohello fájlt. Használja a "script" vagy "scriptPath" tulajdonságot. Mindkettő nem használható együtt. hello fájlnév a kis-és nagybetűket. |Nem |
| határozza meg |Kulcs/érték párok paraméterek meghatározni belül hello Hive parancsfájl segítségével történő "hiveconf" hivatkozik |Nem |

## <a name="example"></a>Példa
Mérlegeljük, játék például elemzés, ahol azt szeretné, hogy a felhasználók a vállalat által elindított játékok tooidentify hello idő naplózza. 

hello következő log utasítás minta játék napló, amely vessző (`,`) elválasztva, és a következő mezők – ProfileID, SessionStart, időtartam, SrcIPAddress és GameType hello tartalmazza.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Hello **Hive parancsprogram** tooprocess ezeket az adatokat:

```
DROP TABLE IF EXISTS HiveSampleIn; 
CREATE EXTERNAL TABLE HiveSampleIn 
(
    ProfileID        string, 
    SessionStart     string, 
    Duration         int, 
    SrcIPAddress     string, 
    GameType         string
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 

DROP TABLE IF EXISTS HiveSampleOut; 
CREATE EXTERNAL TABLE HiveSampleOut 
(    
    ProfileID     string, 
    Duration     int
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';

INSERT OVERWRITE TABLE HiveSampleOut
Select 
    ProfileID,
    SUM(Duration)
FROM HiveSampleIn Group by ProfileID
```

a Hive parancsfájl a Data Factory-folyamathoz tooexecute, következőkre lesz szüksége toodo hello

1. Hozzon létre egy kapcsolódó szolgáltatás tooregister [saját HDInsight számítási fürt](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálása [igény szerinti HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Most hívható "HDInsightLinkedService" szolgáltatásnak.
2. Hozzon létre egy [társított szolgáltatás](data-factory-azure-blob-connector.md) tooconfigure hello kapcsolat tooAzure a Blob storage hello adatok üzemeltetéséhez. Most a szolgáltatásnak "StorageLinkedService" hívása
3. Hozzon létre [adatkészletek](data-factory-create-datasets.md) toohello bemeneti és a hello mutató kimeneti adatokat. Most hívás hello bemeneti adatkészletéből "HiveSampleIn" és a kimeneti adatkészlet "HiveSampleOut" hello
4. A fájl tooAzure #2. lépésben beállított Blob Storage hello Hive-lekérdezések másolni. Ha hello tárolási hello adatok tárolásához nem hello egyet a lekérdezés fájlt, hozzon létre egy külön Azure tárolás társított szolgáltatása, és tekintse meg a tooit hello tevékenység. Használjon ** scriptPath ** toospecify hello elérési toohive lekérdezésfájl és **scriptLinkedService** toospecify hello hello parancsfájlt tartalmazó az Azure storage. 
   
   > [!NOTE]
   > Hello Hive parancsfájl beágyazott hello tevékenységdefinícióban hello használatával is megadhatja **parancsfájl** tulajdonság. Nem ajánlott ennek a megközelítésnek speciális karaktereket hello parancsfájlban hello JSON-dokumentum belül kell toobe escape-karakterrel megjelölve, és előfordulhat, hogy OK hibáinak feltárására. hello ajánlott toofollow #4. lépés.
   > 
   > 
5. Hozzon létre egy folyamatot hello HDInsightHive tevékenység. hello tevékenység folyamatok/átalakítások hello adatokat.

    ```JSON   
    {   
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                {
                    "name": "HiveSampleIn"
                }
                ],
                "outputs": [
                {
                    "name": "HiveSampleOut"
                }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
            ]
        }
    }
    ```
6. Hello adatcsatornát. Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) cikkben alább. 
7. Hello data factory-figyelés használatával hello pipeline és kezelési nézetek figyelése. Lásd: [figyelése és kezelése az adat-előállító adatcsatornák](data-factory-monitor-manage-pipelines.md) cikkben alább. 

## <a name="specifying-parameters-for-a-hive-script"></a>A Hive parancsprogram paramétereinek megadása
Ebben a példában játék naplók az Azure Blob Storage naponta okozhatnak, és egy dátum és idő tárolóhelyeinek mappába kerülnek. Szeretné, hogy tooparameterize hello Hive parancsfájlok és a futásidőben dinamikusan adja át a hello bemeneti mappájának helye és hello kimeneti dátum és idő tárolóhelyeinek is készít.

toouse paraméteres Hive parancsfájlokat, hajtsa végre a következő hello

* A hello paramétereit **meghatározása**.

    ```JSON  
    {
        "name": "HiveActivitySamplePipeline",
          "properties": {
        "activities": [
             {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                      {
                        "name": "HiveSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "HiveSampleOut"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      },
                       "scheduler": {
                          "frequency": "Hour",
                          "interval": 1
                    }
                }
              }
        ]
      }
    }
    ```
* A Hive parancsfájl hello, tekintse meg a toohello paraméter használatával **${hiveconf:parameterName}**. 
  
    ```
    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID     string, 
        SessionStart     string, 
        Duration     int, 
        SrcIPAddress     string, 
        GameType     string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 

    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (
        ProfileID     string, 
        Duration     int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';

    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID
    ```
## <a name="see-also"></a>Lásd még:
* [A Pig-tevékenység](data-factory-pig-activity.md)
* [MapReduce művelethez](data-factory-map-reduce.md)
* [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
* [Spark-programok meghívása](data-factory-spark.md)
* [R-szkriptek meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

