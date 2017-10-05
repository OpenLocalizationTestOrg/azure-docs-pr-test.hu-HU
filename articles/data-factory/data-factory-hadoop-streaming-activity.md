---
title: "Adatok Hadoop Streamelési tevékenységben - Azure használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatja a Hadoop Streamelési tevékenységben egy Azure data factoryban adatok átalakítására használja a-igény szerint vagy a saját HDInsight-fürtök Hadoop Streamelési programok futtatásával."
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
ms.openlocfilehash: bfe62aa60f5a0ff339e1d495d22a5fdfac10d5dc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a><span data-ttu-id="f7126-103">Azure Data Factory használatával Hadoop Streamelési tevékenységben adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="f7126-103">Transform data using Hadoop Streaming Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="f7126-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="f7126-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="f7126-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="f7126-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="f7126-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="f7126-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="f7126-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="f7126-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="f7126-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="f7126-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="f7126-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="f7126-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="f7126-114">Használhatja a HDInsightStreamingActivity tevékenység meghívni egy Azure Data Factory-folyamat a Hadoop adatfolyam-feladatot.</span><span class="sxs-lookup"><span data-stu-id="f7126-114">You can use the HDInsightStreamingActivity Activity invoke a Hadoop Streaming job from an Azure Data Factory pipeline.</span></span> <span data-ttu-id="f7126-115">A következő JSON-részlet a HDInsightStreamingActivity használatát mutatja be egy adatcsatorna JSON-fájl szintaxisát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f7126-115">The following JSON snippet shows the syntax for using the HDInsightStreamingActivity in a pipeline JSON file.</span></span> 

<span data-ttu-id="f7126-116">A HDInsight Streamelési tevékenységben egy adat-előállítóban [csővezeték](data-factory-create-pipelines.md) Hadoop Streamelési programok végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f7126-116">The HDInsight Streaming Activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hadoop Streaming programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="f7126-117">Ez a cikk épít, a [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és a támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="f7126-117">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="f7126-118">Ha most ismerkedik az Azure Data Factory, olvassa végig [Bevezetés az Azure Data Factory](data-factory-introduction.md) hajtsa végre az oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="f7126-118">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="json-sample"></a><span data-ttu-id="f7126-119">JSON-mintát</span><span class="sxs-lookup"><span data-stu-id="f7126-119">JSON sample</span></span>
<span data-ttu-id="f7126-120">A HDInsight-fürt automatikusan kitölti a rendszer például programok (wc.exe és cat.exe) és az adatokat (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="f7126-120">The HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="f7126-121">Alapértelmezés szerint a tárolót a HDInsight-fürt által használt neve egyrészt a fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="f7126-121">By default, name of the container that is used by the HDInsight cluster is the name of the cluster itself.</span></span> <span data-ttu-id="f7126-122">Például ha a fürt neve myhdicluster, a társított blob-tároló neve lenne myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="f7126-122">For example, if your cluster name is myhdicluster, name of the blob container associated would be myhdicluster.</span></span> 

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

<span data-ttu-id="f7126-123">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="f7126-123">Note the following points:</span></span>

1. <span data-ttu-id="f7126-124">Állítsa be a **linkedServiceName** nevére, a társított szolgáltatás mutat, a HDInsight fürt a folyamatos átviteli mapreduce-feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f7126-124">Set the **linkedServiceName** to the name of the linked service that points to your HDInsight cluster on which the streaming mapreduce job is run.</span></span>
2. <span data-ttu-id="f7126-125">Állítsa be a tevékenység típusát **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="f7126-125">Set the type of the activity to **HDInsightStreaming**.</span></span>
3. <span data-ttu-id="f7126-126">Az a **leképező** tulajdonság, adja meg a leképező végrehajtható fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="f7126-126">For the **mapper** property, specify the name of mapper executable.</span></span> <span data-ttu-id="f7126-127">A példában a cat.exe végrehajtható leképező.</span><span class="sxs-lookup"><span data-stu-id="f7126-127">In the example, cat.exe is the mapper executable.</span></span>
4. <span data-ttu-id="f7126-128">Az a **nyomáscsökkentő** tulajdonság, adja meg a végrehajtható nyomáscsökkentő nevét.</span><span class="sxs-lookup"><span data-stu-id="f7126-128">For the **reducer** property, specify the name of reducer executable.</span></span> <span data-ttu-id="f7126-129">A példában a wc.exe végrehajtható nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="f7126-129">In the example, wc.exe is the reducer executable.</span></span>
5. <span data-ttu-id="f7126-130">Az a **bemeneti** írja be a tulajdonságot, adja meg a bemeneti fájl (például helye) a leképező.</span><span class="sxs-lookup"><span data-stu-id="f7126-130">For the **input** type property, specify the input file (including the location) for the mapper.</span></span> <span data-ttu-id="f7126-131">A példában: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample a blob-tároló, például/data/Gutenberg az a mappa, pedig davinci.txt a blob.</span><span class="sxs-lookup"><span data-stu-id="f7126-131">In the example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is the blob container, example/data/Gutenberg is the folder, and davinci.txt is the blob.</span></span>
6. <span data-ttu-id="f7126-132">Az a **kimeneti** írja be a tulajdonságot, adja meg a kimeneti fájl (például helye) a nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="f7126-132">For the **output** type property, specify the output file (including the location) for the reducer.</span></span> <span data-ttu-id="f7126-133">Az adatfolyam-Hadoop-feladat eredményének írása ehhez a tulajdonsághoz megadott helyre.</span><span class="sxs-lookup"><span data-stu-id="f7126-133">The output of the Hadoop Streaming job is written to the location specified for this property.</span></span>
7. <span data-ttu-id="f7126-134">Az a **filePaths** területén adja meg a hozzárendelést és nyomáscsökkentő végrehajtható fájlok elérési útját.</span><span class="sxs-lookup"><span data-stu-id="f7126-134">In the **filePaths** section, specify the paths for the mapper and reducer executables.</span></span> <span data-ttu-id="f7126-135">A példában: "adfsample/example/apps/wc.exe" adfsample a blob-tároló, például/alkalmazások az a mappa, pedig wc.exe a végrehajtható fájl.</span><span class="sxs-lookup"><span data-stu-id="f7126-135">In the example: "adfsample/example/apps/wc.exe", adfsample is the blob container, example/apps is the folder, and wc.exe is the executable.</span></span>
8. <span data-ttu-id="f7126-136">Az a **fileLinkedService** tulajdonság, adja meg az Azure tárolás társított szolgáltatása, amely az Azure storage filePaths szakaszában megadott fájlt tartalmazó jelöli.</span><span class="sxs-lookup"><span data-stu-id="f7126-136">For the **fileLinkedService** property, specify the Azure Storage linked service that represents the Azure storage that contains the files specified in the filePaths section.</span></span>
9. <span data-ttu-id="f7126-137">Az a **argumentumok** tulajdonság, a folyamatos átviteli feladat argumentumainak megadása.</span><span class="sxs-lookup"><span data-stu-id="f7126-137">For the **arguments** property, specify the arguments for the streaming job.</span></span>
10. <span data-ttu-id="f7126-138">A **getDebugInfo** tulajdonsága egy nem kötelező elemet.</span><span class="sxs-lookup"><span data-stu-id="f7126-138">The **getDebugInfo** property is an optional element.</span></span> <span data-ttu-id="f7126-139">Hiba-ra van állítva, a naplók csak az hiba lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="f7126-139">When it is set to Failure, the logs are downloaded only on failure.</span></span> <span data-ttu-id="f7126-140">Ha ezt a beállítást mindig, naplók mindig letöltődnek függetlenül a végrehajtási állapotot.</span><span class="sxs-lookup"><span data-stu-id="f7126-140">When it is set to Always, logs are always downloaded irrespective of the execution status.</span></span>

> [!NOTE]
> <span data-ttu-id="f7126-141">A példa szerint, állít be egy kimeneti adatkészlet esetében a Hadoop Streamelési tevékenységben a **kimenete** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f7126-141">As shown in the example, you specify an output dataset for the Hadoop Streaming Activity for the **outputs** property.</span></span> <span data-ttu-id="f7126-142">Ez az adatkészlet csak egy üres adatkészlet szükséges ahhoz, hogy az adatcsatorna ütemezés meghajtó.</span><span class="sxs-lookup"><span data-stu-id="f7126-142">This dataset is just a dummy dataset that is required to drive the pipeline schedule.</span></span> <span data-ttu-id="f7126-143">Nem kell megadnia a bármely bemeneti adatkészlet a tevékenység a **bemenetek** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f7126-143">You do not need to specify any input dataset for the activity for the **inputs** property.</span></span>  
> 
> 

## <a name="example"></a><span data-ttu-id="f7126-144">Példa</span><span class="sxs-lookup"><span data-stu-id="f7126-144">Example</span></span>
<span data-ttu-id="f7126-145">A kimenetátirányítási mechanizmusával Ez a forgatókönyv az Azure HDInsight-fürt a szószámot adatfolyam térkép/csökkentse programot futtatja.</span><span class="sxs-lookup"><span data-stu-id="f7126-145">The pipeline in this walkthrough runs the Word Count streaming Map/Reduce program on your Azure HDInsight cluster.</span></span> 

### <a name="linked-services"></a><span data-ttu-id="f7126-146">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f7126-146">Linked services</span></span>
#### <a name="azure-storage-linked-service"></a><span data-ttu-id="f7126-147">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f7126-147">Azure Storage linked service</span></span>
<span data-ttu-id="f7126-148">Először hozzon létre az Azure Storage Azure data factoryval való az Azure HDInsight-fürt által használt csatolásához összekapcsolt szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f7126-148">First, you create a linked service to link the Azure Storage that is used by the Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="f7126-149">Ha Ön másolja és illessze be az alábbi kód, ne feledje cseréje a fióknevet és a fiókkulcsot nevét és az Azure Storage kulcsa.</span><span class="sxs-lookup"><span data-stu-id="f7126-149">If you copy/paste the following code, do not forget to replace account name and account key with the name and key of your Azure Storage.</span></span> 

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="f7126-150">Az Azure HDInsight társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f7126-150">Azure HDInsight linked service</span></span>
<span data-ttu-id="f7126-151">Ezután hozzon létre egy Azure HDInsight-fürtjéhez csatolása az Azure data factory társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f7126-151">Next, you create a linked service to link your Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="f7126-152">Ha, másolja és illessze be az alábbi kód, cserélje le a HDInsight-fürt neve a HDInsight-fürt nevét, és módosítsa a felhasználói nevet és jelszót értékek.</span><span class="sxs-lookup"><span data-stu-id="f7126-152">If you copy/paste the following code, replace HDInsight cluster name with the name of your HDInsight cluster, and change user name and password values.</span></span> 

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

### <a name="datasets"></a><span data-ttu-id="f7126-153">Adathalmazok</span><span class="sxs-lookup"><span data-stu-id="f7126-153">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="f7126-154">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="f7126-154">Output dataset</span></span>
<span data-ttu-id="f7126-155">Ebben a példában az adatcsatorna nem veszi a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="f7126-155">The pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="f7126-156">Egy kimeneti adatkészlet állít be a HDInsight Streamelési tevékenységben.</span><span class="sxs-lookup"><span data-stu-id="f7126-156">You specify an output dataset for the HDInsight Streaming Activity.</span></span> <span data-ttu-id="f7126-157">Ez az adatkészlet csak egy üres adatkészlet szükséges ahhoz, hogy az adatcsatorna ütemezés meghajtó.</span><span class="sxs-lookup"><span data-stu-id="f7126-157">This dataset is just a dummy dataset that is required to drive the pipeline schedule.</span></span> 

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

### <a name="pipeline"></a><span data-ttu-id="f7126-158">Folyamat</span><span class="sxs-lookup"><span data-stu-id="f7126-158">Pipeline</span></span>
<span data-ttu-id="f7126-159">Ebben a példában az adatcsatorna csak egy tevékenység, amelynek típusa van: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="f7126-159">The pipeline in this example has only one activity that is of type: **HDInsightStreaming**.</span></span> 

<span data-ttu-id="f7126-160">A HDInsight-fürt automatikusan kitölti a rendszer például programok (wc.exe és cat.exe) és az adatokat (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="f7126-160">The HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="f7126-161">Alapértelmezés szerint a tárolót a HDInsight-fürt által használt neve egyrészt a fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="f7126-161">By default, name of the container that is used by the HDInsight cluster is the name of the cluster itself.</span></span> <span data-ttu-id="f7126-162">Például ha a fürt neve myhdicluster, a társított blob-tároló neve lenne myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="f7126-162">For example, if your cluster name is myhdicluster, name of the blob container associated would be myhdicluster.</span></span>  

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
## <a name="see-also"></a><span data-ttu-id="f7126-163">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f7126-163">See Also</span></span>
* [<span data-ttu-id="f7126-164">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-164">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="f7126-165">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f7126-165">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="f7126-166">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="f7126-166">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="f7126-167">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="f7126-167">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="f7126-168">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="f7126-168">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

