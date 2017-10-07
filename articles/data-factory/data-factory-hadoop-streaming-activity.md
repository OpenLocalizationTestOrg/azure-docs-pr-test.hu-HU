---
title: "Hadoop Streamelési tevékenységben - Azure aaaTransform adatok |} Microsoft Docs"
description: "Ismerje meg, hogy használatát hello Hadoop Streamelési tevékenységben egy az Azure data factory tootransform adatokban lévő-igény szerint vagy a saját HDInsight-fürtök Hadoop Streamelési programok futtatásával."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: a7ddb7268f47162709a9c8136ccd69e0b7d4ad7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Azure Data Factory használatával Hadoop Streamelési tevékenységben adatok átalakítása
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

Használhat hello HDInsightStreamingActivity tevékenység meghívni egy Azure Data Factory-folyamat a Hadoop adatfolyam-feladatot. hello alábbi JSON kódrészletben láthatja hello szintaxisát hello HDInsightStreamingActivity az adatcsatorna JSON-fájlt. 

HDInsight a Data Factory Streamelési tevékenységben hello [csővezeték](data-factory-create-pipelines.md) Hadoop Streamelési programok végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight fürt. Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.

> [!NOTE] 
> Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt. 

## <a name="json-sample"></a>JSON-mintát
Példa programok (wc.exe és cat.exe) és az adatok (davinci.txt) automatikusan töltődik hello HDInsight-fürthöz. Alapértelmezés szerint hello HDInsight-fürt által használt hello tároló neve nem hello fürt hello nevét. Például ha a fürt neve myhdicluster, a kapcsolódó hello blob-tároló neve lenne myhdicluster. 

```JSON
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
                    "filePaths": [
                        "<nameofthecluster>/example/apps/wc.exe",
                        "<nameofthecluster>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "AzureStorageLinkedService",
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
        "start": "2014-01-04T00:00:00Z",
        "end": "2014-01-05T00:00:00Z"
    }
}
```

Vegye figyelembe a következő pontok hello:

1. Set hello **linkedServiceName** hello toohello neve társított szolgáltatás mutat, mely streaming mapreduce hello feladat fut tooyour HDInsight-fürt.
2. Állítsa be a hello tevékenység hello típusa túl**HDInsightStreaming**.
3. A hello **leképező** tulajdonság, leképező végrehajtható hello neve adható meg. Hello példában cat.exe: hello leképező végrehajtható.
4. A hello **nyomáscsökkentő** tulajdonság, adja meg a végrehajtható nyomáscsökkentő hello nevét. Hello példában wc.exe: hello nyomáscsökkentő végrehajtható.
5. A hello **bemeneti** írja be a tulajdonságot, hello leképező hello bemeneti fájl (beleértve a hello helye) megadása. Hello példa: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample hello blob tároló, például/data/Gutenberg hello mappában, pedig davinci.txt hello blob.
6. A hello **kimeneti** írja be a tulajdonságot, hello nyomáscsökkentő hello kimeneti fájl (beleértve a hello helye) megadása. hello Hadoop adatfolyam-feladat eredményének hello toohello helyére a tulajdonság írása.
7. A hello **filePaths** területén hello hello hozzárendelést és nyomáscsökkentő végrehajtható fájlok elérési útját adja meg. Hello példa: "adfsample/example/apps/wc.exe" adfsample egy hello blob tároló, például/alkalmazások hello mappa, pedig wc.exe hello végrehajtható.
8. A hello **fileLinkedService** tulajdonság, adja meg az Azure Storage társított szolgáltatás, amely jelöli az Azure storage hello filePaths szakaszában megadott hello fájlokat tartalmazó hello hello.
9. A hello **argumentumok** tulajdonság, a folyamatos átviteli feladat hello hello argumentumainak megadása.
10. Hello **getDebugInfo** tulajdonsága egy nem kötelező elemet. Ha ezt a beállítást tooFailure, hello naplók csak az hiba lesznek letöltve. Ha ezt a beállítást tooAlways, naplók mindig letöltődnek függetlenül hello végrehajtási állapotát.

> [!NOTE]
> Példában látható módon hello, akkor adja meg egy kimeneti adatkészletet hello a Hadoop Streamelési tevékenységben hello **kimenete** tulajdonság. Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés. Nem kell toospecify bármely bemeneti adatkészlet hello tevékenység a hello **bemenetek** tulajdonság.  
> 
> 

## <a name="example"></a>Példa
Ebben a bemutatóban hello csővezeték hello szószámot adatfolyam térkép/csökkentse programot futtatja Azure HDInsight-fürtön található. 

### <a name="linked-services"></a>Társított szolgáltatások
#### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
Először hozzon létre egy kapcsolódó szolgáltatás toolink hello hello Azure HDInsight fürt toohello az Azure data factory által használt Azure Storage. Ha Ön másolja és illessze be a következő kód hello, ne feledje tooreplace fiók név és fiókkulcs hello nevű kulcs és az Azure Storage kulcsa. 

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
Ezután létrehozhat egy kapcsolódó szolgáltatás toolink az Azure HDInsight fürt toohello az Azure data factory. Ha Ön másolja és illessze be a következő kód hello, cserélje le a HDInsight-fürt neve a HDInsight fürt hello neve, és módosítsa a felhasználói nevet és jelszót értékek. 

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
Ebben a példában hello folyamat nem veszi a bemeneti adatok. Megadhat egy kimeneti adatkészletet hello HDInsight Streamelési tevékenységben. Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés. 

```JSON
{
    "name": "StreamingOutputDataset",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "adftutorial/streamingdata/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Folyamat
hello csővezeték ebben a példában csak egy tevékenység, amelynek típusa van: **HDInsightStreaming**. 

Példa programok (wc.exe és cat.exe) és az adatok (davinci.txt) automatikusan töltődik hello HDInsight-fürthöz. Alapértelmezés szerint hello HDInsight-fürt által használt hello tároló neve nem hello fürt hello nevét. Például ha a fürt neve myhdicluster, a kapcsolódó hello blob-tároló neve lenne myhdicluster.  

```JSON
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
                    "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<blobcontainer>/example/apps/wc.exe",
                        "<blobcontainer>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "StorageLinkedService"
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
        "start": "2017-01-03T00:00:00Z",
        "end": "2017-01-04T00:00:00Z"
    }
}
```
## <a name="see-also"></a>Lásd még:
* [Hive-tevékenység](data-factory-hive-activity.md)
* [A Pig-tevékenység](data-factory-pig-activity.md)
* [MapReduce művelethez](data-factory-map-reduce.md)
* [Spark-programok meghívása](data-factory-spark.md)
* [R-szkriptek meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

