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
# <a name="invoke-mapreduce-programs-from-data-factory"></a><span data-ttu-id="fc83e-103">Az adat-előállító MapReduce programok meghívása</span><span class="sxs-lookup"><span data-stu-id="fc83e-103">Invoke MapReduce Programs from Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="fc83e-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="fc83e-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="fc83e-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="fc83e-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="fc83e-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="fc83e-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="fc83e-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="fc83e-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="fc83e-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="fc83e-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="fc83e-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="fc83e-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="fc83e-114">HDInsight MapReduce művelethez egy adat-előállítóban hello [folyamat](data-factory-create-pipelines.md) MapReduce programok végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fc83e-114">hello HDInsight MapReduce activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes MapReduce programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="fc83e-115">Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="fc83e-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="fc83e-116">Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="fc83e-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span>  

## <a name="introduction"></a><span data-ttu-id="fc83e-117">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="fc83e-117">Introduction</span></span>
<span data-ttu-id="fc83e-118">Egy folyamatot egy az Azure data factory az adatokat a csatolt tárolószolgáltatások csatolt számítási szolgáltatások használatával dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="fc83e-118">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="fc83e-119">Ha minden tevékenység egyedi feldolgozása műveletet hajt végre tevékenységek sorrendje tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fc83e-119">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="fc83e-120">Ez a cikk ismerteti a HDInsight MapReduce művelethez hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="fc83e-120">This article describes using hello HDInsight MapReduce Activity.</span></span>

<span data-ttu-id="fc83e-121">Lásd: [Pig](data-factory-pig-activity.md) és [Hive](data-factory-hive-activity.md) Pig/Hive futtatásával kapcsolatos részletekért parancsfájlok a Windows/Linux-alapú HDInsight fürt egy láncból tevékenységek HDInsight a Pig és a Hive használatával.</span><span class="sxs-lookup"><span data-stu-id="fc83e-121">See [Pig](data-factory-pig-activity.md) and [Hive](data-factory-hive-activity.md) for details about running Pig/Hive scripts on a Windows/Linux-based HDInsight cluster from a pipeline by using HDInsight Pig and Hive activities.</span></span> 

## <a name="json-for-hdinsight-mapreduce-activity"></a><span data-ttu-id="fc83e-122">A HDInsight MapReduce művelethez JSON</span><span class="sxs-lookup"><span data-stu-id="fc83e-122">JSON for HDInsight MapReduce Activity</span></span>
<span data-ttu-id="fc83e-123">A HDInsight tevékenység hello JSON-definícióból hello:</span><span class="sxs-lookup"><span data-stu-id="fc83e-123">In hello JSON definition for hello HDInsight Activity:</span></span> 

1. <span data-ttu-id="fc83e-124">Set hello **típus** a hello **tevékenység** túl**HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="fc83e-124">Set hello **type** of hello **activity** too**HDInsight**.</span></span>
2. <span data-ttu-id="fc83e-125">Adja meg a hello osztály hello nevét **osztálynév** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fc83e-125">Specify hello name of hello class for **className** property.</span></span>
3. <span data-ttu-id="fc83e-126">Adja meg a hello elérési toohello JAR-fájlra beleértve a fájlnevet hello **jarfilepath tulajdonságot** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fc83e-126">Specify hello path toohello JAR file including hello file name for **jarFilePath** property.</span></span>
4. <span data-ttu-id="fc83e-127">Adja meg, amely tartalmazza a hello JAR-fájlt az Azure Blob Storage toohello kapcsolódó hello szolgáltatás **jarLinkedService** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fc83e-127">Specify hello linked service that refers toohello Azure Blob Storage that contains hello JAR file for **jarLinkedService** property.</span></span>   
5. <span data-ttu-id="fc83e-128">Adja meg a hello MapReduce program argumentumai hello **argumentumok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="fc83e-128">Specify any arguments for hello MapReduce program in hello **arguments** section.</span></span> <span data-ttu-id="fc83e-129">Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a hello MapReduce keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="fc83e-129">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="fc83e-130">toodifferentiate az argumentumok hello MapReduce argumentumokkal, érdemes lehetőség és value elemeit is argumentumként látható módon a következő példa hello (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll).</span><span class="sxs-lookup"><span data-stu-id="fc83e-130">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values).</span></span>

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
<span data-ttu-id="fc83e-131">Hello HDInsight MapReduce művelethez toorun használhatja a MapReduce jar-fájlt egy HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="fc83e-131">You can use hello HDInsight MapReduce Activity toorun any MapReduce jar file on an HDInsight cluster.</span></span> <span data-ttu-id="fc83e-132">Hello következő minta JSON-definícióból adatcsatorna, HDInsight tevékenység hello toorun Mahout JAR fájl beállítva.</span><span class="sxs-lookup"><span data-stu-id="fc83e-132">In hello following sample JSON definition of a pipeline, hello HDInsight Activity is configured toorun a Mahout JAR file.</span></span>

## <a name="sample-on-github"></a><span data-ttu-id="fc83e-133">Mintát a Githubon</span><span class="sxs-lookup"><span data-stu-id="fc83e-133">Sample on GitHub</span></span>
<span data-ttu-id="fc83e-134">HDInsight MapReduce művelethez hello használatához minta letöltheti a: [Data Factory minták a Githubon](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span><span class="sxs-lookup"><span data-stu-id="fc83e-134">You can download a sample for using hello HDInsight MapReduce Activity from: [Data Factory Samples on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span></span>  

## <a name="running-hello-word-count-program"></a><span data-ttu-id="fc83e-135">Hello szószámot programot futtat</span><span class="sxs-lookup"><span data-stu-id="fc83e-135">Running hello Word Count program</span></span>
<span data-ttu-id="fc83e-136">Ebben a példában hello csővezeték hello térkép vagy csökkentse a szószámot program Azure HDInsight-fürtön található fut.</span><span class="sxs-lookup"><span data-stu-id="fc83e-136">hello pipeline in this example runs hello Word Count Map/Reduce program on your Azure HDInsight cluster.</span></span>   

### <a name="linked-services"></a><span data-ttu-id="fc83e-137">Kapcsolódó szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="fc83e-137">Linked Services</span></span>
<span data-ttu-id="fc83e-138">Először hozzon létre egy kapcsolódó szolgáltatás toolink hello hello Azure HDInsight fürt toohello az Azure data factory által használt Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fc83e-138">First, you create a linked service toolink hello Azure Storage that is used by hello Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="fc83e-139">Ha Ön másolja és illessze be a következő kód hello, ne feledje tooreplace **fióknév** és **fiókkulcs** hello nevű és kulcsát az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fc83e-139">If you copy/paste hello following code, do not forget tooreplace **account name** and **account key** with hello name and key of your Azure Storage.</span></span> 

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="fc83e-140">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="fc83e-140">Azure Storage linked service</span></span>

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="fc83e-141">Az Azure HDInsight társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="fc83e-141">Azure HDInsight linked service</span></span>
<span data-ttu-id="fc83e-142">Ezután létrehozhat egy kapcsolódó szolgáltatás toolink az Azure HDInsight fürt toohello az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="fc83e-142">Next, you create a linked service toolink your Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="fc83e-143">Ha Ön másolja és illessze be a következő kód hello, cserélje le **HDInsight-fürt neve** hello nevet, a HDInsight-fürtöt, és a felhasználói nevet és jelszót értékek módosítása.</span><span class="sxs-lookup"><span data-stu-id="fc83e-143">If you copy/paste hello following code, replace **HDInsight cluster name** with hello name of your HDInsight cluster, and change user name and password values.</span></span>   

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

### <a name="datasets"></a><span data-ttu-id="fc83e-144">Adathalmazok</span><span class="sxs-lookup"><span data-stu-id="fc83e-144">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="fc83e-145">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="fc83e-145">Output dataset</span></span>
<span data-ttu-id="fc83e-146">Ebben a példában hello folyamat nem veszi a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="fc83e-146">hello pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="fc83e-147">Megadhat egy kimeneti adatkészletet hello HDInsight MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="fc83e-147">You specify an output dataset for hello HDInsight MapReduce Activity.</span></span> <span data-ttu-id="fc83e-148">Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés.</span><span class="sxs-lookup"><span data-stu-id="fc83e-148">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span>  

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

### <a name="pipeline"></a><span data-ttu-id="fc83e-149">Folyamat</span><span class="sxs-lookup"><span data-stu-id="fc83e-149">Pipeline</span></span>
<span data-ttu-id="fc83e-150">hello csővezeték ebben a példában csak egy tevékenység, amelynek típusa van: HDInsightMapReduce.</span><span class="sxs-lookup"><span data-stu-id="fc83e-150">hello pipeline in this example has only one activity that is of type: HDInsightMapReduce.</span></span> <span data-ttu-id="fc83e-151">Hello JSON hello fontos tulajdonságai vannak:</span><span class="sxs-lookup"><span data-stu-id="fc83e-151">Some of hello important properties in hello JSON are:</span></span> 

| <span data-ttu-id="fc83e-152">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fc83e-152">Property</span></span> | <span data-ttu-id="fc83e-153">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fc83e-153">Notes</span></span> |
|:--- |:--- |
| <span data-ttu-id="fc83e-154">type</span><span class="sxs-lookup"><span data-stu-id="fc83e-154">type</span></span> |<span data-ttu-id="fc83e-155">hello típusa túl be kell állítani**HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="fc83e-155">hello type must be set too**HDInsightMapReduce**.</span></span> |
| <span data-ttu-id="fc83e-156">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="fc83e-156">className</span></span> |<span data-ttu-id="fc83e-157">Hello osztály neve: **wordcount**</span><span class="sxs-lookup"><span data-stu-id="fc83e-157">Name of hello class is: **wordcount**</span></span> |
| <span data-ttu-id="fc83e-158">jarfilepath tulajdonságot</span><span class="sxs-lookup"><span data-stu-id="fc83e-158">jarFilePath</span></span> |<span data-ttu-id="fc83e-159">Elérési út toohello jar-fájlt tartalmazó hello osztály.</span><span class="sxs-lookup"><span data-stu-id="fc83e-159">Path toohello jar file containing hello class.</span></span> <span data-ttu-id="fc83e-160">Ha Ön másolja és illessze be a következő kód hello, ne feledje toochange hello hello fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="fc83e-160">If you copy/paste hello following code, don't forget toochange hello name of hello cluster.</span></span> |
| <span data-ttu-id="fc83e-161">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="fc83e-161">jarLinkedService</span></span> |<span data-ttu-id="fc83e-162">Az Azure tárolás társított szolgáltatása, amely tartalmazza a hello jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="fc83e-162">Azure Storage linked service that contains hello jar file.</span></span> <span data-ttu-id="fc83e-163">A társított szolgáltatás toohello tárolási hello HDInsight-fürthöz társított hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="fc83e-163">This linked service refers toohello storage that is associated with hello HDInsight cluster.</span></span> |
| <span data-ttu-id="fc83e-164">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="fc83e-164">arguments</span></span> |<span data-ttu-id="fc83e-165">hello wordcount program két argumentumot, bemeneti és egy kimeneti vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fc83e-165">hello wordcount program takes two arguments, an input and an output.</span></span> <span data-ttu-id="fc83e-166">a bemeneti fájl hello hello davinci.txt fájl.</span><span class="sxs-lookup"><span data-stu-id="fc83e-166">hello input file is hello davinci.txt file.</span></span> |
| <span data-ttu-id="fc83e-167">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="fc83e-167">frequency/interval</span></span> |<span data-ttu-id="fc83e-168">hello ezen tulajdonságok megegyeznek hello kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="fc83e-168">hello values for these properties match hello output dataset.</span></span> |
| <span data-ttu-id="fc83e-169">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="fc83e-169">linkedServiceName</span></span> |<span data-ttu-id="fc83e-170">toohello kapcsolódó HDInsight-szolgáltatás volt korábban létrehozott hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="fc83e-170">refers toohello HDInsight linked service you had created earlier.</span></span> |

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

## <a name="run-spark-programs"></a><span data-ttu-id="fc83e-171">Spark programok futtatása</span><span class="sxs-lookup"><span data-stu-id="fc83e-171">Run Spark programs</span></span>
<span data-ttu-id="fc83e-172">A HDInsight Spark-fürt a MapReduce tevékenység toorun Spark programokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="fc83e-172">You can use MapReduce activity toorun Spark programs on your HDInsight Spark cluster.</span></span> <span data-ttu-id="fc83e-173">A részletekért lásd: [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) (Spark-programok meghívása az Azure Data Factory-ból).</span><span class="sxs-lookup"><span data-stu-id="fc83e-173">See [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) for details.</span></span>  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a><span data-ttu-id="fc83e-174">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="fc83e-174">See Also</span></span>
* [<span data-ttu-id="fc83e-175">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-175">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="fc83e-176">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fc83e-176">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="fc83e-177">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="fc83e-177">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="fc83e-178">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="fc83e-178">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="fc83e-179">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="fc83e-179">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

