---
title: "a Pig-tevékenység használata az Azure Data Factory aaaTransform adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan használható egy az Azure data factory toorun Pig-parancsfájlok a Pig tevékenység hello egy a-igény szerint vagy a saját HDInsight-fürt."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 3ad096c4a9e8603b09f574f6d129b4339a75d381
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>A Pig-tevékenység használata az Azure Data Factory adatok átalakítása
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

HDInsight Pig tevékenység egy adat-előállítóban hello [csővezeték](data-factory-create-pipelines.md) Pig lekérdezések végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz. Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.

> [!NOTE] 
> Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt. 

## <a name="syntax"></a>Szintaxis

```JSON
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
## <a name="syntax-details"></a>Szintaxis részletei
| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| név |Hello tevékenység neve. |Igen |
| leírás |Milyen hello tevékenységgel a leíró szöveg |Nem |
| type |HDinsightPig |Igen |
| Bemenetek |Hello által használt egy vagy több bemeneti Pig tevékenység |Nem |
| kimenetek |Egy vagy több állítanak elő által hello Pig tevékenység |Igen |
| linkedServiceName |A Data Factory kapcsolt szolgáltatásként regisztrált hivatkozás toohello HDInsight-fürt |Igen |
| Parancsfájl |Adja meg a Pig-parancsprogram beágyazott hello |Nem |
| parancsfájl elérési útja |Hello Pig-parancsprogram egy Azure blob Storage tárolóban tárolja, és adja meg a hello elérési toohello fájlt. Használja a "script" vagy "scriptPath" tulajdonságot. Mindkettő nem használható együtt. hello fájlnév a kis-és nagybetűket. |Nem |
| határozza meg |Kulcs/érték párok paraméterek meghatározni hivatkozó belül hello Pig-parancsprogram |Nem |

## <a name="example"></a>Példa
Mérlegeljük, játék például elemzés, ha azt szeretné, hogy a vállalat által elindított játékok játékosok által felhasznált tooidentify hello idő naplózza.

a következő játék napló minta hello egy olyan vesszővel (,) elválasztva fájl. A következő mezők – ProfileID, SessionStart, időtartam, SrcIPAddress és GameType hello tartalmaz.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Hello **parancsfájl sertésfelmérés** tooprocess ezeket az adatokat:

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

a Data Factory-folyamathoz, a parancsfájl a Pig tooexecute hello a következő lépéseket:

1. Hozzon létre egy kapcsolódó szolgáltatás tooregister [saját HDInsight számítási fürt](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálása [igény szerinti HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Most hívható meg a társított szolgáltatás **HDInsightLinkedService**.
2. Hozzon létre egy [társított szolgáltatás](data-factory-azure-blob-connector.md) tooconfigure hello kapcsolat tooAzure a Blob storage hello adatok üzemeltetéséhez. Most hívható meg a társított szolgáltatás **StorageLinkedService**.
3. Hozzon létre [adatkészletek](data-factory-create-datasets.md) toohello bemeneti és a hello mutató kimeneti adatokat. Most hívás hello bemeneti adatkészlet **PigSampleIn** és a kimeneti adatkészlet hello **PigSampleOut**.
4. Másolja át egy fájl hello Azure Blob Storage #2. lépésben beállított hello Pig lekérdezés. Ha hello hello adatokat tároló Azure storage hello egy hello lekérdezésfájl üzemeltető eltér, hozzon létre egy külön Azure tárolás társított szolgáltatása. Tekintse meg a kapcsolódó toohello szolgáltatás hello tevékenység konfigurációban. Használjon ** scriptPath ** toospecify hello elérési toopig parancsfájl és **scriptLinkedService**. 
   
   > [!NOTE]
   > Hello Pig parancsfájl beágyazott hello tevékenységdefinícióban hello használatával is megadhatja **parancsfájl** tulajdonság. Azonban nem javasoljuk ezt a módszert használja, speciális karaktereket a hello parancsfájl escape-karaktersorozatot toobe kell és hibakeresési hibákat okozhat. hello ajánlott toofollow #4. lépés.
   > 
   > 
5. Hozzon létre hello csővezeték hello HDInsightPig tevékenység. Ez a tevékenység hello bemeneti adatokat dolgozza fel a HDInsight-fürt Pig-parancsprogram futtatásával.

    ```JSON   
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
              "scriptLinkedService": "StorageLinkedService"
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
6. Hello adatcsatornát. Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) cikkben alább. 
7. Hello data factory-figyelés használatával hello pipeline és kezelési nézetek figyelése. Lásd: [figyelése és kezelése az adat-előállító adatcsatornák](data-factory-monitor-manage-pipelines.md) cikkben alább.

## <a name="specifying-parameters-for-a-pig-script"></a>A Pig-parancsprogram paramétereinek megadása
Vegye figyelembe a következő példa hello: játék naplók naponta okozhatnak az Azure Blob Storage és egy mappa a particionált alapján dátuma és időpontja. Szeretné, hogy tooparameterize hello Pig-parancsprogram és a futtatás során dinamikusan adja át a hello bemeneti mappájának helye és hello kimeneti dátum és idő tárolóhelyeinek is készít.

toouse paraméteres Pig-parancsprogram, hajtsa végre a következő hello:

* A hello paramétereit **meghatározása**.

    ```JSON  
    {
        "name": "PigActivitySamplePipeline",
          "properties": {
        "activities": [
            {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                      {
                        "name": "PigSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "PigSampleOut"
                      }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0:MM}/dayno={0: dd}/',SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
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
* A Pig-parancsprogram hello, tekintse meg a toohello paramétereket a "**$parameterName**" hello a következő példában látható módon:

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a>Lásd még:
* [Hive-tevékenység](data-factory-hive-activity.md)
* [MapReduce művelethez](data-factory-map-reduce.md)
* [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
* [Spark-programok meghívása](data-factory-spark.md)
* [R-szkriptek meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

