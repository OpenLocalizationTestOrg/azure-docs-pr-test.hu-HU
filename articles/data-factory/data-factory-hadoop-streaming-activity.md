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
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a><span data-ttu-id="d96af-103">Azure Data Factory használatával Hadoop Streamelési tevékenységben adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="d96af-103">Transform data using Hadoop Streaming Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="d96af-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="d96af-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="d96af-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="d96af-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="d96af-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="d96af-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="d96af-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="d96af-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="d96af-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="d96af-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="d96af-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="d96af-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="d96af-114">Használhat hello HDInsightStreamingActivity tevékenység meghívni egy Azure Data Factory-folyamat a Hadoop adatfolyam-feladatot.</span><span class="sxs-lookup"><span data-stu-id="d96af-114">You can use hello HDInsightStreamingActivity Activity invoke a Hadoop Streaming job from an Azure Data Factory pipeline.</span></span> <span data-ttu-id="d96af-115">hello alábbi JSON kódrészletben láthatja hello szintaxisát hello HDInsightStreamingActivity az adatcsatorna JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="d96af-115">hello following JSON snippet shows hello syntax for using hello HDInsightStreamingActivity in a pipeline JSON file.</span></span> 

<span data-ttu-id="d96af-116">HDInsight a Data Factory Streamelési tevékenységben hello [csővezeték](data-factory-create-pipelines.md) Hadoop Streamelési programok végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight fürt.</span><span class="sxs-lookup"><span data-stu-id="d96af-116">hello HDInsight Streaming Activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hadoop Streaming programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="d96af-117">Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="d96af-117">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="d96af-118">Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="d96af-118">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="json-sample"></a><span data-ttu-id="d96af-119">JSON-mintát</span><span class="sxs-lookup"><span data-stu-id="d96af-119">JSON sample</span></span>
<span data-ttu-id="d96af-120">Példa programok (wc.exe és cat.exe) és az adatok (davinci.txt) automatikusan töltődik hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d96af-120">hello HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="d96af-121">Alapértelmezés szerint hello HDInsight-fürt által használt hello tároló neve nem hello fürt hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d96af-121">By default, name of hello container that is used by hello HDInsight cluster is hello name of hello cluster itself.</span></span> <span data-ttu-id="d96af-122">Például ha a fürt neve myhdicluster, a kapcsolódó hello blob-tároló neve lenne myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="d96af-122">For example, if your cluster name is myhdicluster, name of hello blob container associated would be myhdicluster.</span></span> 

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

<span data-ttu-id="d96af-123">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="d96af-123">Note hello following points:</span></span>

1. <span data-ttu-id="d96af-124">Set hello **linkedServiceName** hello toohello neve társított szolgáltatás mutat, mely streaming mapreduce hello feladat fut tooyour HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="d96af-124">Set hello **linkedServiceName** toohello name of hello linked service that points tooyour HDInsight cluster on which hello streaming mapreduce job is run.</span></span>
2. <span data-ttu-id="d96af-125">Állítsa be a hello tevékenység hello típusa túl**HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="d96af-125">Set hello type of hello activity too**HDInsightStreaming**.</span></span>
3. <span data-ttu-id="d96af-126">A hello **leképező** tulajdonság, leképező végrehajtható hello neve adható meg.</span><span class="sxs-lookup"><span data-stu-id="d96af-126">For hello **mapper** property, specify hello name of mapper executable.</span></span> <span data-ttu-id="d96af-127">Hello példában cat.exe: hello leképező végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="d96af-127">In hello example, cat.exe is hello mapper executable.</span></span>
4. <span data-ttu-id="d96af-128">A hello **nyomáscsökkentő** tulajdonság, adja meg a végrehajtható nyomáscsökkentő hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d96af-128">For hello **reducer** property, specify hello name of reducer executable.</span></span> <span data-ttu-id="d96af-129">Hello példában wc.exe: hello nyomáscsökkentő végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="d96af-129">In hello example, wc.exe is hello reducer executable.</span></span>
5. <span data-ttu-id="d96af-130">A hello **bemeneti** írja be a tulajdonságot, hello leképező hello bemeneti fájl (beleértve a hello helye) megadása.</span><span class="sxs-lookup"><span data-stu-id="d96af-130">For hello **input** type property, specify hello input file (including hello location) for hello mapper.</span></span> <span data-ttu-id="d96af-131">Hello példa: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample hello blob tároló, például/data/Gutenberg hello mappában, pedig davinci.txt hello blob.</span><span class="sxs-lookup"><span data-stu-id="d96af-131">In hello example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is hello blob container, example/data/Gutenberg is hello folder, and davinci.txt is hello blob.</span></span>
6. <span data-ttu-id="d96af-132">A hello **kimeneti** írja be a tulajdonságot, hello nyomáscsökkentő hello kimeneti fájl (beleértve a hello helye) megadása.</span><span class="sxs-lookup"><span data-stu-id="d96af-132">For hello **output** type property, specify hello output file (including hello location) for hello reducer.</span></span> <span data-ttu-id="d96af-133">hello Hadoop adatfolyam-feladat eredményének hello toohello helyére a tulajdonság írása.</span><span class="sxs-lookup"><span data-stu-id="d96af-133">hello output of hello Hadoop Streaming job is written toohello location specified for this property.</span></span>
7. <span data-ttu-id="d96af-134">A hello **filePaths** területén hello hello hozzárendelést és nyomáscsökkentő végrehajtható fájlok elérési útját adja meg.</span><span class="sxs-lookup"><span data-stu-id="d96af-134">In hello **filePaths** section, specify hello paths for hello mapper and reducer executables.</span></span> <span data-ttu-id="d96af-135">Hello példa: "adfsample/example/apps/wc.exe" adfsample egy hello blob tároló, például/alkalmazások hello mappa, pedig wc.exe hello végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="d96af-135">In hello example: "adfsample/example/apps/wc.exe", adfsample is hello blob container, example/apps is hello folder, and wc.exe is hello executable.</span></span>
8. <span data-ttu-id="d96af-136">A hello **fileLinkedService** tulajdonság, adja meg az Azure Storage társított szolgáltatás, amely jelöli az Azure storage hello filePaths szakaszában megadott hello fájlokat tartalmazó hello hello.</span><span class="sxs-lookup"><span data-stu-id="d96af-136">For hello **fileLinkedService** property, specify hello Azure Storage linked service that represents hello Azure storage that contains hello files specified in hello filePaths section.</span></span>
9. <span data-ttu-id="d96af-137">A hello **argumentumok** tulajdonság, a folyamatos átviteli feladat hello hello argumentumainak megadása.</span><span class="sxs-lookup"><span data-stu-id="d96af-137">For hello **arguments** property, specify hello arguments for hello streaming job.</span></span>
10. <span data-ttu-id="d96af-138">Hello **getDebugInfo** tulajdonsága egy nem kötelező elemet.</span><span class="sxs-lookup"><span data-stu-id="d96af-138">hello **getDebugInfo** property is an optional element.</span></span> <span data-ttu-id="d96af-139">Ha ezt a beállítást tooFailure, hello naplók csak az hiba lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="d96af-139">When it is set tooFailure, hello logs are downloaded only on failure.</span></span> <span data-ttu-id="d96af-140">Ha ezt a beállítást tooAlways, naplók mindig letöltődnek függetlenül hello végrehajtási állapotát.</span><span class="sxs-lookup"><span data-stu-id="d96af-140">When it is set tooAlways, logs are always downloaded irrespective of hello execution status.</span></span>

> [!NOTE]
> <span data-ttu-id="d96af-141">Példában látható módon hello, akkor adja meg egy kimeneti adatkészletet hello a Hadoop Streamelési tevékenységben hello **kimenete** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d96af-141">As shown in hello example, you specify an output dataset for hello Hadoop Streaming Activity for hello **outputs** property.</span></span> <span data-ttu-id="d96af-142">Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés.</span><span class="sxs-lookup"><span data-stu-id="d96af-142">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span> <span data-ttu-id="d96af-143">Nem kell toospecify bármely bemeneti adatkészlet hello tevékenység a hello **bemenetek** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d96af-143">You do not need toospecify any input dataset for hello activity for hello **inputs** property.</span></span>  
> 
> 

## <a name="example"></a><span data-ttu-id="d96af-144">Példa</span><span class="sxs-lookup"><span data-stu-id="d96af-144">Example</span></span>
<span data-ttu-id="d96af-145">Ebben a bemutatóban hello csővezeték hello szószámot adatfolyam térkép/csökkentse programot futtatja Azure HDInsight-fürtön található.</span><span class="sxs-lookup"><span data-stu-id="d96af-145">hello pipeline in this walkthrough runs hello Word Count streaming Map/Reduce program on your Azure HDInsight cluster.</span></span> 

### <a name="linked-services"></a><span data-ttu-id="d96af-146">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d96af-146">Linked services</span></span>
#### <a name="azure-storage-linked-service"></a><span data-ttu-id="d96af-147">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d96af-147">Azure Storage linked service</span></span>
<span data-ttu-id="d96af-148">Először hozzon létre egy kapcsolódó szolgáltatás toolink hello hello Azure HDInsight fürt toohello az Azure data factory által használt Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d96af-148">First, you create a linked service toolink hello Azure Storage that is used by hello Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="d96af-149">Ha Ön másolja és illessze be a következő kód hello, ne feledje tooreplace fiók név és fiókkulcs hello nevű kulcs és az Azure Storage kulcsa.</span><span class="sxs-lookup"><span data-stu-id="d96af-149">If you copy/paste hello following code, do not forget tooreplace account name and account key with hello name and key of your Azure Storage.</span></span> 

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="d96af-150">Az Azure HDInsight társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d96af-150">Azure HDInsight linked service</span></span>
<span data-ttu-id="d96af-151">Ezután létrehozhat egy kapcsolódó szolgáltatás toolink az Azure HDInsight fürt toohello az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="d96af-151">Next, you create a linked service toolink your Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="d96af-152">Ha Ön másolja és illessze be a következő kód hello, cserélje le a HDInsight-fürt neve a HDInsight fürt hello neve, és módosítsa a felhasználói nevet és jelszót értékek.</span><span class="sxs-lookup"><span data-stu-id="d96af-152">If you copy/paste hello following code, replace HDInsight cluster name with hello name of your HDInsight cluster, and change user name and password values.</span></span> 

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

### <a name="datasets"></a><span data-ttu-id="d96af-153">Adathalmazok</span><span class="sxs-lookup"><span data-stu-id="d96af-153">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="d96af-154">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="d96af-154">Output dataset</span></span>
<span data-ttu-id="d96af-155">Ebben a példában hello folyamat nem veszi a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="d96af-155">hello pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="d96af-156">Megadhat egy kimeneti adatkészletet hello HDInsight Streamelési tevékenységben.</span><span class="sxs-lookup"><span data-stu-id="d96af-156">You specify an output dataset for hello HDInsight Streaming Activity.</span></span> <span data-ttu-id="d96af-157">Ez az adatkészlet csak egy üres adathalmazt, amelyet szükséges toodrive hello csővezeték ütemezés.</span><span class="sxs-lookup"><span data-stu-id="d96af-157">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span> 

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

### <a name="pipeline"></a><span data-ttu-id="d96af-158">Folyamat</span><span class="sxs-lookup"><span data-stu-id="d96af-158">Pipeline</span></span>
<span data-ttu-id="d96af-159">hello csővezeték ebben a példában csak egy tevékenység, amelynek típusa van: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="d96af-159">hello pipeline in this example has only one activity that is of type: **HDInsightStreaming**.</span></span> 

<span data-ttu-id="d96af-160">Példa programok (wc.exe és cat.exe) és az adatok (davinci.txt) automatikusan töltődik hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d96af-160">hello HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="d96af-161">Alapértelmezés szerint hello HDInsight-fürt által használt hello tároló neve nem hello fürt hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d96af-161">By default, name of hello container that is used by hello HDInsight cluster is hello name of hello cluster itself.</span></span> <span data-ttu-id="d96af-162">Például ha a fürt neve myhdicluster, a kapcsolódó hello blob-tároló neve lenne myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="d96af-162">For example, if your cluster name is myhdicluster, name of hello blob container associated would be myhdicluster.</span></span>  

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
## <a name="see-also"></a><span data-ttu-id="d96af-163">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d96af-163">See Also</span></span>
* [<span data-ttu-id="d96af-164">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-164">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="d96af-165">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="d96af-165">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="d96af-166">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="d96af-166">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="d96af-167">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="d96af-167">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="d96af-168">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="d96af-168">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

