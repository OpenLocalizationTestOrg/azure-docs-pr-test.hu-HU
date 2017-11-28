---
title: "A Pig-tevékenység használata az Azure Data Factory adatok átalakítása |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatja a Pig-tevékenység egy Azure data factoryban a-igény szerint vagy a saját HDInsight-fürtök Pig-parancsfájlok futtatásra."
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
ms.openlocfilehash: 182a637ab98955129d269e2afc3ba581aa1a7c03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a><span data-ttu-id="b1dfa-103">A Pig-tevékenység használata az Azure Data Factory adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="b1dfa-103">Transform data using Pig Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="b1dfa-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="b1dfa-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="b1dfa-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="b1dfa-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="b1dfa-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="b1dfa-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="b1dfa-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="b1dfa-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="b1dfa-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="b1dfa-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="b1dfa-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="b1dfa-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="b1dfa-114">A HDInsight a Pig-tevékenység egy adat-előállítóban [csővezeték](data-factory-create-pipelines.md) Pig lekérdezések végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-114">The HDInsight Pig activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Pig queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="b1dfa-115">Ez a cikk épít, a [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és a támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-115">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="b1dfa-116">Ha most ismerkedik az Azure Data Factory, olvassa végig [Bevezetés az Azure Data Factory](data-factory-introduction.md) hajtsa végre az oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-116">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="b1dfa-117">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="b1dfa-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="b1dfa-118">Szintaxis részletei</span><span class="sxs-lookup"><span data-stu-id="b1dfa-118">Syntax details</span></span>
| <span data-ttu-id="b1dfa-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b1dfa-119">Property</span></span> | <span data-ttu-id="b1dfa-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="b1dfa-120">Description</span></span> | <span data-ttu-id="b1dfa-121">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b1dfa-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b1dfa-122">név</span><span class="sxs-lookup"><span data-stu-id="b1dfa-122">name</span></span> |<span data-ttu-id="b1dfa-123">A tevékenység neve.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-123">Name of the activity</span></span> |<span data-ttu-id="b1dfa-124">Igen</span><span class="sxs-lookup"><span data-stu-id="b1dfa-124">Yes</span></span> |
| <span data-ttu-id="b1dfa-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="b1dfa-125">description</span></span> |<span data-ttu-id="b1dfa-126">Mire használható a tevékenységet leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="b1dfa-126">Text describing what the activity is used for</span></span> |<span data-ttu-id="b1dfa-127">Nem</span><span class="sxs-lookup"><span data-stu-id="b1dfa-127">No</span></span> |
| <span data-ttu-id="b1dfa-128">type</span><span class="sxs-lookup"><span data-stu-id="b1dfa-128">type</span></span> |<span data-ttu-id="b1dfa-129">HDinsightPig</span><span class="sxs-lookup"><span data-stu-id="b1dfa-129">HDinsightPig</span></span> |<span data-ttu-id="b1dfa-130">Igen</span><span class="sxs-lookup"><span data-stu-id="b1dfa-130">Yes</span></span> |
| <span data-ttu-id="b1dfa-131">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="b1dfa-131">inputs</span></span> |<span data-ttu-id="b1dfa-132">Egy vagy több, a Pig tevékenység által felhasznált bemeneti</span><span class="sxs-lookup"><span data-stu-id="b1dfa-132">One or more inputs consumed by the Pig activity</span></span> |<span data-ttu-id="b1dfa-133">Nem</span><span class="sxs-lookup"><span data-stu-id="b1dfa-133">No</span></span> |
| <span data-ttu-id="b1dfa-134">kimenetek</span><span class="sxs-lookup"><span data-stu-id="b1dfa-134">outputs</span></span> |<span data-ttu-id="b1dfa-135">Egy vagy több, a Pig tevékenység által létrehozott kimenet</span><span class="sxs-lookup"><span data-stu-id="b1dfa-135">One or more outputs produced by the Pig activity</span></span> |<span data-ttu-id="b1dfa-136">Igen</span><span class="sxs-lookup"><span data-stu-id="b1dfa-136">Yes</span></span> |
| <span data-ttu-id="b1dfa-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="b1dfa-137">linkedServiceName</span></span> |<span data-ttu-id="b1dfa-138">A HDInsight-fürthöz, a Data Factory kapcsolt szolgáltatásként regisztrált mutató hivatkozás</span><span class="sxs-lookup"><span data-stu-id="b1dfa-138">Reference to the HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="b1dfa-139">Igen</span><span class="sxs-lookup"><span data-stu-id="b1dfa-139">Yes</span></span> |
| <span data-ttu-id="b1dfa-140">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b1dfa-140">script</span></span> |<span data-ttu-id="b1dfa-141">Adja meg a Pig-parancsprogram beágyazott</span><span class="sxs-lookup"><span data-stu-id="b1dfa-141">Specify the Pig script inline</span></span> |<span data-ttu-id="b1dfa-142">Nem</span><span class="sxs-lookup"><span data-stu-id="b1dfa-142">No</span></span> |
| <span data-ttu-id="b1dfa-143">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="b1dfa-143">script path</span></span> |<span data-ttu-id="b1dfa-144">A Pig-parancsprogram egy Azure blob Storage tárolóban tárolja, és adja meg a fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-144">Store the Pig script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="b1dfa-145">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="b1dfa-146">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-146">Both cannot be used together.</span></span> <span data-ttu-id="b1dfa-147">A fájlnév pedig kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-147">The file name is case-sensitive.</span></span> |<span data-ttu-id="b1dfa-148">Nem</span><span class="sxs-lookup"><span data-stu-id="b1dfa-148">No</span></span> |
| <span data-ttu-id="b1dfa-149">határozza meg</span><span class="sxs-lookup"><span data-stu-id="b1dfa-149">defines</span></span> |<span data-ttu-id="b1dfa-150">Kulcs/érték párok paraméterek meghatározni a Pig-parancsprogram belül hivatkozik</span><span class="sxs-lookup"><span data-stu-id="b1dfa-150">Specify parameters as key/value pairs for referencing within the Pig script</span></span> |<span data-ttu-id="b1dfa-151">Nem</span><span class="sxs-lookup"><span data-stu-id="b1dfa-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="b1dfa-152">Példa</span><span class="sxs-lookup"><span data-stu-id="b1dfa-152">Example</span></span>
<span data-ttu-id="b1dfa-153">Mérlegeljük, elemzés, ahol szeretné azonosítani a vállalat által elindított játékok játékosok szerint fordított idő játék naplók példát.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-153">Let’s consider an example of game logs analytics where you want to identify the time spent by players playing games launched by your company.</span></span>

<span data-ttu-id="b1dfa-154">A következő játék mintanaplót egy olyan vesszővel (,) elválasztva fájl.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-154">The following sample game log is a comma (,) separated file.</span></span> <span data-ttu-id="b1dfa-155">A következő mezők – ProfileID, SessionStart, időtartam, SrcIPAddress és GameType tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-155">It contains the following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="b1dfa-156">A **parancsfájl sertésfelmérés** feldolgozni az adatokat:</span><span class="sxs-lookup"><span data-stu-id="b1dfa-156">The **Pig script** to process this data:</span></span>

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

<span data-ttu-id="b1dfa-157">A Pig-parancsprogram végrehajtása a Data Factory-folyamathoz, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b1dfa-157">To execute this Pig script in a Data Factory pipeline, do the following steps:</span></span>

1. <span data-ttu-id="b1dfa-158">Hozzon létre egy kapcsolódó szolgáltatás regisztrálása [saját HDInsight számítási fürt](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálása [igény szerinti HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="b1dfa-158">Create a linked service to register [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="b1dfa-159">Most hívható meg a társított szolgáltatás **HDInsightLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-159">Let’s call this linked service **HDInsightLinkedService**.</span></span>
2. <span data-ttu-id="b1dfa-160">Hozzon létre egy [társított szolgáltatás](data-factory-azure-blob-connector.md) üzemeltető Azure Blob Storage a kapcsolat beállításához.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-160">Create a [linked service](data-factory-azure-blob-connector.md) to configure the connection to Azure Blob storage hosting the data.</span></span> <span data-ttu-id="b1dfa-161">Most hívható meg a társított szolgáltatás **StorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-161">Let’s call this linked service **StorageLinkedService**.</span></span>
3. <span data-ttu-id="b1dfa-162">Hozzon létre [adatkészletek](data-factory-create-datasets.md) mutat a bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-162">Create [datasets](data-factory-create-datasets.md) pointing to the input and the output data.</span></span> <span data-ttu-id="b1dfa-163">Most hívható meg a bemeneti adatkészlet **PigSampleIn** és a kimeneti adatkészlet **PigSampleOut**.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-163">Let’s call the input dataset **PigSampleIn** and the output dataset **PigSampleOut**.</span></span>
4. <span data-ttu-id="b1dfa-164">A Pig lekérdezés másolja az Azure Blob Storage #2. lépésben beállított fájlba.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-164">Copy the Pig query in a file the Azure Blob Storage configured in step #2.</span></span> <span data-ttu-id="b1dfa-165">Ha az Azure storage az adatokat tároló eltérő, amelyen a lekérdezés fájlt, hozzon létre egy külön Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-165">If the Azure storage that hosts the data is different from the one that hosts the query file, create a separate Azure Storage linked service.</span></span> <span data-ttu-id="b1dfa-166">A tevékenység konfigurációban kapcsolódó szolgáltatásra hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-166">Refer to the linked service in the activity configuration.</span></span> <span data-ttu-id="b1dfa-167">Használjon ** scriptPath ** adhatja meg a pig-parancsprogram fájl elérési útját és **scriptLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-167">Use **scriptPath **to specify the path to pig script file and **scriptLinkedService**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="b1dfa-168">A Pig parancsfájl beágyazottan történjen-e a tevékenység definíciójának használatával is megadhatja a **parancsfájl** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-168">You can also provide the Pig script inline in the activity definition by using the **script** property.</span></span> <span data-ttu-id="b1dfa-169">Azonban a Microsoft nem javasolja ezt a módszert használja, a parancsfájl igényeinek, meg kell jelölni a speciális karaktereket, és hibakeresési hibákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-169">However, we do not recommend this approach as all special characters in the script needs to be escaped and may cause debugging issues.</span></span> <span data-ttu-id="b1dfa-170">A bevált gyakorlat követéséhez #4. lépés.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-170">The best practice is to follow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="b1dfa-171">A folyamat létrehozása a HDInsightPig tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-171">Create the pipeline with the HDInsightPig activity.</span></span> <span data-ttu-id="b1dfa-172">Ez a tevékenység HDInsight-fürt Pig-parancsprogram futtatásával dolgozza fel a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-172">This activity processes the input data by running Pig script on HDInsight cluster.</span></span>

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
6. <span data-ttu-id="b1dfa-173">Az adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-173">Deploy the pipeline.</span></span> <span data-ttu-id="b1dfa-174">Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-174">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="b1dfa-175">A data factory kiszolgálófigyelési és -kezelési nézetek segítségével folyamat figyelésére.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-175">Monitor the pipeline using the data factory monitoring and management views.</span></span> <span data-ttu-id="b1dfa-176">Lásd: [figyelése és kezelése az adat-előállító adatcsatornák](data-factory-monitor-manage-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-176">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span>

## <a name="specifying-parameters-for-a-pig-script"></a><span data-ttu-id="b1dfa-177">A Pig-parancsprogram paramétereinek megadása</span><span class="sxs-lookup"><span data-stu-id="b1dfa-177">Specifying parameters for a Pig script</span></span>
<span data-ttu-id="b1dfa-178">Tekintse meg a következő példát: játék naplók naponta okozhatnak az Azure Blob Storage és egy mappa a particionált alapján dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-178">Consider the following example: game logs are ingested daily into Azure Blob Storage and stored in a folder partitioned based on date and time.</span></span> <span data-ttu-id="b1dfa-179">Szeretné parametrizálja a Pig-parancsprogram és futásidőben dinamikusan adja át a bemeneti mappa helyét, és a kimeneti dátum és idő tárolóhelyeinek is készít.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-179">You want to parameterize the Pig script and pass the input folder location dynamically during runtime and also produce the output partitioned with date and time.</span></span>

<span data-ttu-id="b1dfa-180">A paraméteres Pig-parancsprogram használatához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b1dfa-180">To use parameterized Pig script, do the following:</span></span>

* <span data-ttu-id="b1dfa-181">Adja meg a paramétereket a **meghatározása**.</span><span class="sxs-lookup"><span data-stu-id="b1dfa-181">Define the parameters in **defines**.</span></span>

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
* <span data-ttu-id="b1dfa-182">A Pig-parancsprogram, tekintse meg a paramétereket, a "**$parameterName**" a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="b1dfa-182">In the Pig Script, refer to the parameters using '**$parameterName**' as shown in the following example:</span></span>

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a><span data-ttu-id="b1dfa-183">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b1dfa-183">See Also</span></span>
* [<span data-ttu-id="b1dfa-184">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b1dfa-184">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="b1dfa-185">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="b1dfa-185">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="b1dfa-186">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="b1dfa-186">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="b1dfa-187">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="b1dfa-187">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="b1dfa-188">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="b1dfa-188">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

