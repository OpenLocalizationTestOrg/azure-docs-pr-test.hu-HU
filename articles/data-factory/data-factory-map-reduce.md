---
title: az Azure Data Factory MapReduce Program aaaInvoke
description: "Ismerje meg, hogyan tooprocess adatokat az Azure HDInsight MapReduce programok futtatásával egy az Azure data factory a fürtön."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c34db93f-570a-44f1-a7d6-00390f4dc0fa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 448ef93a10bd97e7ecd4be4f04f88f8a05decc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Az adat-előállító MapReduce programok meghívása
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

HDInsight MapReduce művelethez egy adat-előállítóban hello [folyamat](data-factory-create-pipelines.md) MapReduce programok végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz. Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.

> [!NOTE] 
> Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.  

## <a name="introduction"></a>Bevezetés
Egy folyamatot egy az Azure data factory az adatokat a csatolt tárolószolgáltatások csatolt számítási szolgáltatások használatával dolgozza fel. Ha minden tevékenység egyedi feldolgozása műveletet hajt végre tevékenységek sorrendje tartalmaz. Ez a cikk ismerteti a HDInsight MapReduce művelethez hello segítségével.

Lásd: [Pig](data-factory-pig-activity.md) és [Hive](data-factory-hive-activity.md) Pig/Hive futtatásával kapcsolatos részletekért parancsfájlok a Windows/Linux-alapú HDInsight fürt egy láncból tevékenységek HDInsight a Pig és a Hive használatával. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>A HDInsight MapReduce művelethez JSON
A HDInsight tevékenység hello JSON-definícióból hello: 

1. Set hello **típus** a hello **tevékenység** túl**HDInsight**.
2. Adja meg a hello osztály hello nevét **osztálynév** tulajdonság.
3. Adja meg a hello elérési toohello JAR-fájlra beleértve a fájlnevet hello **jarfilepath tulajdonságot** tulajdonság.
4. Adja meg, amely tartalmazza a hello JAR-fájlt az Azure Blob Storage toohello kapcsolódó hello szolgáltatás **jarLinkedService** tulajdonság.   
5. Adja meg a hello MapReduce program argumentumai hello **argumentumok** szakasz. Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a hello MapReduce keretrendszer. toodifferentiate az argumentumok hello MapReduce argumentumokkal, érdemes lehetőség és value elemeit is argumentumként látható módon a következő példa hello (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll).

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix toodetermine hello similarity between 2 items",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "-s",
                            "SIMILARITY_LOGLIKELIHOOD",
                            "--input",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                            "--output",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                            "--maxSimilaritiesPerItem",
                            "500",
                            "--tempDir",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                        ]
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
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
Hello HDInsight MapReduce művelethez toorun használhatja a MapReduce jar-fájlt egy HDInsight-fürt. Hello következő minta JSON-definícióból adatcsatorna, HDInsight tevékenység hello toorun Mahout JAR fájl beállítva.

## <a name="sample-on-github"></a>Mintát a Githubon
HDInsight MapReduce művelethez hello használatához minta letöltheti a: [Data Factory minták a Githubon](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-hello-word-count-program"></a>Hello szószámot programot futtat
Ebben a példában hello csővezeték hello térkép vagy csökkentse a szószámot program Azure HDInsight-fürtön található fut.   

### <a name="linked-services"></a>Kapcsolódó szolgáltatások
Először hozzon létre egy kapcsolódó szolgáltatás toolink hello hello Azure HDInsight fürt toohello az Azure data factory által használt Azure Storage. Ha Ön másolja és illessze be a következő kód hello, ne feledje tooreplace **fióknév** és **fiókkulcs** hello nevű és kulcsát az Azure Storage. 

#### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a>Az Azure HDInsight társított szolgáltatás
Ezután létrehozhat egy kapcsolódó szolgáltatás toolink az Azure HDInsight fürt toohello az Azure data factory. Ha Ön másolja és illessze be a következő kód hello, cserélje le **HDInsight-fürt neve** hello nevet, a HDInsight-fürtöt, és a felhasználói nevet és jelszót értékek módosítása.   

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a>Adathalmazok
#### <a name="output-dataset"></a>Kimeneti adatkészlet
Ebben a példában hello folyamat nem veszi a bemeneti adatok. Megadhat egy kimeneti adatkészletet hello HDInsight MapReduce művelethez. Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés.  

```JSON
{
    "name": "MROutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "WordCountOutput1.txt",
            "folderPath": "example/data/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Folyamat
hello csővezeték ebben a példában csak egy tevékenység, amelynek típusa van: HDInsightMapReduce. Hello JSON hello fontos tulajdonságai vannak: 

| Tulajdonság | Megjegyzések |
|:--- |:--- |
| type |hello típusa túl be kell állítani**HDInsightMapReduce**. |
| Osztálynév |Hello osztály neve: **wordcount** |
| jarfilepath tulajdonságot |Elérési út toohello jar-fájlt tartalmazó hello osztály. Ha Ön másolja és illessze be a következő kód hello, ne feledje toochange hello hello fürt nevét. |
| jarLinkedService |Az Azure tárolás társított szolgáltatása, amely tartalmazza a hello jar-fájlra. A társított szolgáltatás toohello tárolási hello HDInsight-fürthöz társított hivatkozik. |
| Argumentumok |hello wordcount program két argumentumot, bemeneti és egy kimeneti vesz igénybe. a bemeneti fájl hello hello davinci.txt fájl. |
| frequency/interval |hello ezen tulajdonságok megegyeznek hello kimeneti adatkészletet. |
| linkedServiceName |toohello kapcsolódó HDInsight-szolgáltatás volt korábban létrehozott hivatkozik. |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun hello Word Count Program",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "wordcount",
                    "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": [
                        "/example/data/gutenberg/davinci.txt",
                        "/example/data/WordCountOutput1"
                    ]
                },
                "outputs": [
                    {
                        "name": "MROutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "MRActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-03T00:00:00Z",
        "end": "2014-01-04T00:00:00Z"
    }
}
```

## <a name="run-spark-programs"></a>Spark programok futtatása
A HDInsight Spark-fürt a MapReduce tevékenység toorun Spark programokat is használhat. A részletekért lásd: [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) (Spark-programok meghívása az Azure Data Factory-ból).  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a>Lásd még:
* [Hive-tevékenység](data-factory-hive-activity.md)
* [A Pig-tevékenység](data-factory-pig-activity.md)
* [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
* [Spark-programok meghívása](data-factory-spark.md)
* [R-szkriptek meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

