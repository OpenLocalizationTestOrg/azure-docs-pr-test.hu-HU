---
title: "Az Azure Data Factory MapReduce Program meghívása"
description: "Megtudhatja, hogyan MapReduce programok futtatásával Azure HDInsight-fürtök az egy az Azure data factory feldolgozni az adatokat."
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
ms.openlocfilehash: 55fc2196cb4ba50eced4a463914ae188217d0fed
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a><span data-ttu-id="9ee1a-103">Az adat-előállító MapReduce programok meghívása</span><span class="sxs-lookup"><span data-stu-id="9ee1a-103">Invoke MapReduce Programs from Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="9ee1a-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="9ee1a-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="9ee1a-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="9ee1a-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="9ee1a-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="9ee1a-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="9ee1a-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="9ee1a-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="9ee1a-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="9ee1a-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="9ee1a-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="9ee1a-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="9ee1a-114">A HDInsight MapReduce művelethez egy adat-előállítóban [csővezeték](data-factory-create-pipelines.md) MapReduce programok végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-114">The HDInsight MapReduce activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes MapReduce programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="9ee1a-115">Ez a cikk épít, a [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és a támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-115">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="9ee1a-116">Ha most ismerkedik az Azure Data Factory, olvassa végig [Bevezetés az Azure Data Factory](data-factory-introduction.md) hajtsa végre az oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-116">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span>  

## <a name="introduction"></a><span data-ttu-id="9ee1a-117">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9ee1a-117">Introduction</span></span>
<span data-ttu-id="9ee1a-118">Egy folyamatot egy az Azure data factory az adatokat a csatolt tárolószolgáltatások csatolt számítási szolgáltatások használatával dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-118">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="9ee1a-119">Ha minden tevékenység egyedi feldolgozása műveletet hajt végre tevékenységek sorrendje tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-119">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="9ee1a-120">Ez a cikk ismerteti a használata a HDInsight MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-120">This article describes using the HDInsight MapReduce Activity.</span></span>

<span data-ttu-id="9ee1a-121">Lásd: [Pig](data-factory-pig-activity.md) és [Hive](data-factory-hive-activity.md) Pig/Hive futtatásával kapcsolatos részletekért parancsfájlok a Windows/Linux-alapú HDInsight fürt egy láncból tevékenységek HDInsight a Pig és a Hive használatával.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-121">See [Pig](data-factory-pig-activity.md) and [Hive](data-factory-hive-activity.md) for details about running Pig/Hive scripts on a Windows/Linux-based HDInsight cluster from a pipeline by using HDInsight Pig and Hive activities.</span></span> 

## <a name="json-for-hdinsight-mapreduce-activity"></a><span data-ttu-id="9ee1a-122">A HDInsight MapReduce művelethez JSON</span><span class="sxs-lookup"><span data-stu-id="9ee1a-122">JSON for HDInsight MapReduce Activity</span></span>
<span data-ttu-id="9ee1a-123">A HDInsight tevékenység JSON-definícióban:</span><span class="sxs-lookup"><span data-stu-id="9ee1a-123">In the JSON definition for the HDInsight Activity:</span></span> 

1. <span data-ttu-id="9ee1a-124">Állítsa be a **típus** , a **tevékenység** való **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-124">Set the **type** of the **activity** to **HDInsight**.</span></span>
2. <span data-ttu-id="9ee1a-125">Adja meg az osztály nevét **osztálynév** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-125">Specify the name of the class for **className** property.</span></span>
3. <span data-ttu-id="9ee1a-126">Adja meg az elérési útját a JAR-fájlra, beleértve a fájlnevet **jarfilepath tulajdonságot** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-126">Specify the path to the JAR file including the file name for **jarFilePath** property.</span></span>
4. <span data-ttu-id="9ee1a-127">Adja meg a társított szolgáltatás, az Azure Blob Storage, amely tartalmazza a JAR-fájlra hivatkozó **jarLinkedService** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-127">Specify the linked service that refers to the Azure Blob Storage that contains the JAR file for **jarLinkedService** property.</span></span>   
5. <span data-ttu-id="9ee1a-128">Adja meg a MapReduce programhoz tartozó argumentumai a **argumentumok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-128">Specify any arguments for the MapReduce program in the **arguments** section.</span></span> <span data-ttu-id="9ee1a-129">Futásidőben, néhány további argumentumok látja (például: mapreduce.job.tags) a a MapReduce keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-129">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from the MapReduce framework.</span></span> <span data-ttu-id="9ee1a-130">A MapReduce argumentumok argumentumok megkülönböztetéséhez érdemes beállítás és value elemeit is argumentumként a következő példában látható módon (- s,--bemeneti,--kimeneti stb., amelyet közvetlenül az értékeik lehetőség áll).</span><span class="sxs-lookup"><span data-stu-id="9ee1a-130">To differentiate your arguments with the MapReduce arguments, consider using both option and value as arguments as shown in the following example (-s, --input, --output etc., are options immediately followed by their values).</span></span>

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
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
                    "description": "Custom Map Reduce to generate Mahout result",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
<span data-ttu-id="9ee1a-131">Bármely MapReduce jar-fájlra a HDInsight-fürtök futtatásához használhatja a HDInsight MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-131">You can use the HDInsight MapReduce Activity to run any MapReduce jar file on an HDInsight cluster.</span></span> <span data-ttu-id="9ee1a-132">A következő minta JSON-definícióban adatcsatorna a HDInsight konfigurálta egy olyan Mahout JAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-132">In the following sample JSON definition of a pipeline, the HDInsight Activity is configured to run a Mahout JAR file.</span></span>

## <a name="sample-on-github"></a><span data-ttu-id="9ee1a-133">Mintát a Githubon</span><span class="sxs-lookup"><span data-stu-id="9ee1a-133">Sample on GitHub</span></span>
<span data-ttu-id="9ee1a-134">Letöltheti a minta használata a HDInsight MapReduce tevékenységet: [Data Factory minták a Githubon](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span><span class="sxs-lookup"><span data-stu-id="9ee1a-134">You can download a sample for using the HDInsight MapReduce Activity from: [Data Factory Samples on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span></span>  

## <a name="running-the-word-count-program"></a><span data-ttu-id="9ee1a-135">A Word-Count futtatása</span><span class="sxs-lookup"><span data-stu-id="9ee1a-135">Running the Word Count program</span></span>
<span data-ttu-id="9ee1a-136">Ebben a példában az adatcsatorna a térkép vagy csökkentse a szószámot programot futtatja az Azure HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-136">The pipeline in this example runs the Word Count Map/Reduce program on your Azure HDInsight cluster.</span></span>   

### <a name="linked-services"></a><span data-ttu-id="9ee1a-137">Kapcsolódó szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9ee1a-137">Linked Services</span></span>
<span data-ttu-id="9ee1a-138">Először hozzon létre az Azure Storage Azure data factoryval való az Azure HDInsight-fürt által használt csatolásához összekapcsolt szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-138">First, you create a linked service to link the Azure Storage that is used by the Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="9ee1a-139">Ha Ön másolja és illessze be az alábbi kód, ne felejtse el lecserélni **fióknév** és **fiókkulcs** nevű és kulcsát az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-139">If you copy/paste the following code, do not forget to replace **account name** and **account key** with the name and key of your Azure Storage.</span></span> 

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="9ee1a-140">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9ee1a-140">Azure Storage linked service</span></span>

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="9ee1a-141">Az Azure HDInsight társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9ee1a-141">Azure HDInsight linked service</span></span>
<span data-ttu-id="9ee1a-142">Ezután hozzon létre egy Azure HDInsight-fürtjéhez csatolása az Azure data factory társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-142">Next, you create a linked service to link your Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="9ee1a-143">Ha Ön másolja és illessze be az alábbi kód, cserélje le **HDInsight-fürt neve** nevű, a HDInsight-fürtöt, és a felhasználói nevet és jelszót értékek módosítása.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-143">If you copy/paste the following code, replace **HDInsight cluster name** with the name of your HDInsight cluster, and change user name and password values.</span></span>   

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

### <a name="datasets"></a><span data-ttu-id="9ee1a-144">Adathalmazok</span><span class="sxs-lookup"><span data-stu-id="9ee1a-144">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="9ee1a-145">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="9ee1a-145">Output dataset</span></span>
<span data-ttu-id="9ee1a-146">Ebben a példában az adatcsatorna nem veszi a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-146">The pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="9ee1a-147">Egy kimeneti adatkészlet állít be a HDInsight MapReduce művelethez.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-147">You specify an output dataset for the HDInsight MapReduce Activity.</span></span> <span data-ttu-id="9ee1a-148">Ez az adatkészlet csak egy üres adatkészlet szükséges ahhoz, hogy az adatcsatorna ütemezés meghajtó.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-148">This dataset is just a dummy dataset that is required to drive the pipeline schedule.</span></span>  

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

### <a name="pipeline"></a><span data-ttu-id="9ee1a-149">Folyamat</span><span class="sxs-lookup"><span data-stu-id="9ee1a-149">Pipeline</span></span>
<span data-ttu-id="9ee1a-150">Ebben a példában az adatcsatorna csak egy tevékenység, amelynek típusa van: HDInsightMapReduce.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-150">The pipeline in this example has only one activity that is of type: HDInsightMapReduce.</span></span> <span data-ttu-id="9ee1a-151">A JSON-ban fontos tulajdonságai vannak:</span><span class="sxs-lookup"><span data-stu-id="9ee1a-151">Some of the important properties in the JSON are:</span></span> 

| <span data-ttu-id="9ee1a-152">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9ee1a-152">Property</span></span> | <span data-ttu-id="9ee1a-153">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9ee1a-153">Notes</span></span> |
|:--- |:--- |
| <span data-ttu-id="9ee1a-154">type</span><span class="sxs-lookup"><span data-stu-id="9ee1a-154">type</span></span> |<span data-ttu-id="9ee1a-155">A típus értékre kell állítani **HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-155">The type must be set to **HDInsightMapReduce**.</span></span> |
| <span data-ttu-id="9ee1a-156">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="9ee1a-156">className</span></span> |<span data-ttu-id="9ee1a-157">Az osztály neve: **wordcount**</span><span class="sxs-lookup"><span data-stu-id="9ee1a-157">Name of the class is: **wordcount**</span></span> |
| <span data-ttu-id="9ee1a-158">jarfilepath tulajdonságot</span><span class="sxs-lookup"><span data-stu-id="9ee1a-158">jarFilePath</span></span> |<span data-ttu-id="9ee1a-159">Az osztály tartalmazó jar-fájlt elérési útja.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-159">Path to the jar file containing the class.</span></span> <span data-ttu-id="9ee1a-160">Ha Ön másolja és illessze be az alábbi kód, ne feledje módosítani a fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-160">If you copy/paste the following code, don't forget to change the name of the cluster.</span></span> |
| <span data-ttu-id="9ee1a-161">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="9ee1a-161">jarLinkedService</span></span> |<span data-ttu-id="9ee1a-162">Az Azure tárolás társított szolgáltatása, amely tartalmazza a jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-162">Azure Storage linked service that contains the jar file.</span></span> <span data-ttu-id="9ee1a-163">A társított szolgáltatás a tárolót a HDInsight-fürthöz társított hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-163">This linked service refers to the storage that is associated with the HDInsight cluster.</span></span> |
| <span data-ttu-id="9ee1a-164">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="9ee1a-164">arguments</span></span> |<span data-ttu-id="9ee1a-165">A wordcount program két argumentumot, bemeneti és egy kimeneti vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-165">The wordcount program takes two arguments, an input and an output.</span></span> <span data-ttu-id="9ee1a-166">A bemeneti fájl a davinci.txt fájlt.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-166">The input file is the davinci.txt file.</span></span> |
| <span data-ttu-id="9ee1a-167">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="9ee1a-167">frequency/interval</span></span> |<span data-ttu-id="9ee1a-168">Ezek a tulajdonságok értékeit felel meg a kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-168">The values for these properties match the output dataset.</span></span> |
| <span data-ttu-id="9ee1a-169">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="9ee1a-169">linkedServiceName</span></span> |<span data-ttu-id="9ee1a-170">a csatolt HDInsight-szolgáltatás volt korábban létrehozott hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-170">refers to the HDInsight linked service you had created earlier.</span></span> |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run the Word Count Program",
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

## <a name="run-spark-programs"></a><span data-ttu-id="9ee1a-171">Spark programok futtatása</span><span class="sxs-lookup"><span data-stu-id="9ee1a-171">Run Spark programs</span></span>
<span data-ttu-id="9ee1a-172">A MapReduce tevékenység használatával Spark-programokat futtathat a HDInsight Spark-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="9ee1a-172">You can use MapReduce activity to run Spark programs on your HDInsight Spark cluster.</span></span> <span data-ttu-id="9ee1a-173">A részletekért lásd: [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) (Spark-programok meghívása az Azure Data Factory-ból).</span><span class="sxs-lookup"><span data-stu-id="9ee1a-173">See [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) for details.</span></span>  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a><span data-ttu-id="9ee1a-174">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9ee1a-174">See Also</span></span>
* [<span data-ttu-id="9ee1a-175">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-175">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="9ee1a-176">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9ee1a-176">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="9ee1a-177">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="9ee1a-177">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="9ee1a-178">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="9ee1a-178">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="9ee1a-179">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="9ee1a-179">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

