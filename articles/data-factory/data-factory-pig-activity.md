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
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a><span data-ttu-id="422e6-103">A Pig-tevékenység használata az Azure Data Factory adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="422e6-103">Transform data using Pig Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="422e6-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="422e6-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="422e6-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="422e6-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="422e6-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="422e6-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="422e6-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="422e6-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="422e6-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="422e6-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="422e6-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="422e6-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="422e6-114">HDInsight Pig tevékenység egy adat-előállítóban hello [csővezeték](data-factory-create-pipelines.md) Pig lekérdezések végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="422e6-114">hello HDInsight Pig activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Pig queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="422e6-115">Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="422e6-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="422e6-116">Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="422e6-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="422e6-117">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="422e6-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="422e6-118">Szintaxis részletei</span><span class="sxs-lookup"><span data-stu-id="422e6-118">Syntax details</span></span>
| <span data-ttu-id="422e6-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="422e6-119">Property</span></span> | <span data-ttu-id="422e6-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="422e6-120">Description</span></span> | <span data-ttu-id="422e6-121">Szükséges</span><span class="sxs-lookup"><span data-stu-id="422e6-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="422e6-122">név</span><span class="sxs-lookup"><span data-stu-id="422e6-122">name</span></span> |<span data-ttu-id="422e6-123">Hello tevékenység neve.</span><span class="sxs-lookup"><span data-stu-id="422e6-123">Name of hello activity</span></span> |<span data-ttu-id="422e6-124">Igen</span><span class="sxs-lookup"><span data-stu-id="422e6-124">Yes</span></span> |
| <span data-ttu-id="422e6-125">leírás</span><span class="sxs-lookup"><span data-stu-id="422e6-125">description</span></span> |<span data-ttu-id="422e6-126">Milyen hello tevékenységgel a leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="422e6-126">Text describing what hello activity is used for</span></span> |<span data-ttu-id="422e6-127">Nem</span><span class="sxs-lookup"><span data-stu-id="422e6-127">No</span></span> |
| <span data-ttu-id="422e6-128">type</span><span class="sxs-lookup"><span data-stu-id="422e6-128">type</span></span> |<span data-ttu-id="422e6-129">HDinsightPig</span><span class="sxs-lookup"><span data-stu-id="422e6-129">HDinsightPig</span></span> |<span data-ttu-id="422e6-130">Igen</span><span class="sxs-lookup"><span data-stu-id="422e6-130">Yes</span></span> |
| <span data-ttu-id="422e6-131">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="422e6-131">inputs</span></span> |<span data-ttu-id="422e6-132">Hello által használt egy vagy több bemeneti Pig tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-132">One or more inputs consumed by hello Pig activity</span></span> |<span data-ttu-id="422e6-133">Nem</span><span class="sxs-lookup"><span data-stu-id="422e6-133">No</span></span> |
| <span data-ttu-id="422e6-134">kimenetek</span><span class="sxs-lookup"><span data-stu-id="422e6-134">outputs</span></span> |<span data-ttu-id="422e6-135">Egy vagy több állítanak elő által hello Pig tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-135">One or more outputs produced by hello Pig activity</span></span> |<span data-ttu-id="422e6-136">Igen</span><span class="sxs-lookup"><span data-stu-id="422e6-136">Yes</span></span> |
| <span data-ttu-id="422e6-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="422e6-137">linkedServiceName</span></span> |<span data-ttu-id="422e6-138">A Data Factory kapcsolt szolgáltatásként regisztrált hivatkozás toohello HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="422e6-138">Reference toohello HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="422e6-139">Igen</span><span class="sxs-lookup"><span data-stu-id="422e6-139">Yes</span></span> |
| <span data-ttu-id="422e6-140">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="422e6-140">script</span></span> |<span data-ttu-id="422e6-141">Adja meg a Pig-parancsprogram beágyazott hello</span><span class="sxs-lookup"><span data-stu-id="422e6-141">Specify hello Pig script inline</span></span> |<span data-ttu-id="422e6-142">Nem</span><span class="sxs-lookup"><span data-stu-id="422e6-142">No</span></span> |
| <span data-ttu-id="422e6-143">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="422e6-143">script path</span></span> |<span data-ttu-id="422e6-144">Hello Pig-parancsprogram egy Azure blob Storage tárolóban tárolja, és adja meg a hello elérési toohello fájlt.</span><span class="sxs-lookup"><span data-stu-id="422e6-144">Store hello Pig script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="422e6-145">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="422e6-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="422e6-146">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="422e6-146">Both cannot be used together.</span></span> <span data-ttu-id="422e6-147">hello fájlnév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="422e6-147">hello file name is case-sensitive.</span></span> |<span data-ttu-id="422e6-148">Nem</span><span class="sxs-lookup"><span data-stu-id="422e6-148">No</span></span> |
| <span data-ttu-id="422e6-149">határozza meg</span><span class="sxs-lookup"><span data-stu-id="422e6-149">defines</span></span> |<span data-ttu-id="422e6-150">Kulcs/érték párok paraméterek meghatározni hivatkozó belül hello Pig-parancsprogram</span><span class="sxs-lookup"><span data-stu-id="422e6-150">Specify parameters as key/value pairs for referencing within hello Pig script</span></span> |<span data-ttu-id="422e6-151">Nem</span><span class="sxs-lookup"><span data-stu-id="422e6-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="422e6-152">Példa</span><span class="sxs-lookup"><span data-stu-id="422e6-152">Example</span></span>
<span data-ttu-id="422e6-153">Mérlegeljük, játék például elemzés, ha azt szeretné, hogy a vállalat által elindított játékok játékosok által felhasznált tooidentify hello idő naplózza.</span><span class="sxs-lookup"><span data-stu-id="422e6-153">Let’s consider an example of game logs analytics where you want tooidentify hello time spent by players playing games launched by your company.</span></span>

<span data-ttu-id="422e6-154">a következő játék napló minta hello egy olyan vesszővel (,) elválasztva fájl.</span><span class="sxs-lookup"><span data-stu-id="422e6-154">hello following sample game log is a comma (,) separated file.</span></span> <span data-ttu-id="422e6-155">A következő mezők – ProfileID, SessionStart, időtartam, SrcIPAddress és GameType hello tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="422e6-155">It contains hello following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="422e6-156">Hello **parancsfájl sertésfelmérés** tooprocess ezeket az adatokat:</span><span class="sxs-lookup"><span data-stu-id="422e6-156">hello **Pig script** tooprocess this data:</span></span>

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

<span data-ttu-id="422e6-157">a Data Factory-folyamathoz, a parancsfájl a Pig tooexecute hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="422e6-157">tooexecute this Pig script in a Data Factory pipeline, do hello following steps:</span></span>

1. <span data-ttu-id="422e6-158">Hozzon létre egy kapcsolódó szolgáltatás tooregister [saját HDInsight számítási fürt](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálása [igény szerinti HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="422e6-158">Create a linked service tooregister [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="422e6-159">Most hívható meg a társított szolgáltatás **HDInsightLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="422e6-159">Let’s call this linked service **HDInsightLinkedService**.</span></span>
2. <span data-ttu-id="422e6-160">Hozzon létre egy [társított szolgáltatás](data-factory-azure-blob-connector.md) tooconfigure hello kapcsolat tooAzure a Blob storage hello adatok üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="422e6-160">Create a [linked service](data-factory-azure-blob-connector.md) tooconfigure hello connection tooAzure Blob storage hosting hello data.</span></span> <span data-ttu-id="422e6-161">Most hívható meg a társított szolgáltatás **StorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="422e6-161">Let’s call this linked service **StorageLinkedService**.</span></span>
3. <span data-ttu-id="422e6-162">Hozzon létre [adatkészletek](data-factory-create-datasets.md) toohello bemeneti és a hello mutató kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="422e6-162">Create [datasets](data-factory-create-datasets.md) pointing toohello input and hello output data.</span></span> <span data-ttu-id="422e6-163">Most hívás hello bemeneti adatkészlet **PigSampleIn** és a kimeneti adatkészlet hello **PigSampleOut**.</span><span class="sxs-lookup"><span data-stu-id="422e6-163">Let’s call hello input dataset **PigSampleIn** and hello output dataset **PigSampleOut**.</span></span>
4. <span data-ttu-id="422e6-164">Másolja át egy fájl hello Azure Blob Storage #2. lépésben beállított hello Pig lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="422e6-164">Copy hello Pig query in a file hello Azure Blob Storage configured in step #2.</span></span> <span data-ttu-id="422e6-165">Ha hello hello adatokat tároló Azure storage hello egy hello lekérdezésfájl üzemeltető eltér, hozzon létre egy külön Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="422e6-165">If hello Azure storage that hosts hello data is different from hello one that hosts hello query file, create a separate Azure Storage linked service.</span></span> <span data-ttu-id="422e6-166">Tekintse meg a kapcsolódó toohello szolgáltatás hello tevékenység konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="422e6-166">Refer toohello linked service in hello activity configuration.</span></span> <span data-ttu-id="422e6-167">Használjon ** scriptPath ** toospecify hello elérési toopig parancsfájl és **scriptLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="422e6-167">Use **scriptPath **toospecify hello path toopig script file and **scriptLinkedService**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="422e6-168">Hello Pig parancsfájl beágyazott hello tevékenységdefinícióban hello használatával is megadhatja **parancsfájl** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="422e6-168">You can also provide hello Pig script inline in hello activity definition by using hello **script** property.</span></span> <span data-ttu-id="422e6-169">Azonban nem javasoljuk ezt a módszert használja, speciális karaktereket a hello parancsfájl escape-karaktersorozatot toobe kell és hibakeresési hibákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="422e6-169">However, we do not recommend this approach as all special characters in hello script needs toobe escaped and may cause debugging issues.</span></span> <span data-ttu-id="422e6-170">hello ajánlott toofollow #4. lépés.</span><span class="sxs-lookup"><span data-stu-id="422e6-170">hello best practice is toofollow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="422e6-171">Hozzon létre hello csővezeték hello HDInsightPig tevékenység.</span><span class="sxs-lookup"><span data-stu-id="422e6-171">Create hello pipeline with hello HDInsightPig activity.</span></span> <span data-ttu-id="422e6-172">Ez a tevékenység hello bemeneti adatokat dolgozza fel a HDInsight-fürt Pig-parancsprogram futtatásával.</span><span class="sxs-lookup"><span data-stu-id="422e6-172">This activity processes hello input data by running Pig script on HDInsight cluster.</span></span>

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
6. <span data-ttu-id="422e6-173">Hello adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="422e6-173">Deploy hello pipeline.</span></span> <span data-ttu-id="422e6-174">Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="422e6-174">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="422e6-175">Hello data factory-figyelés használatával hello pipeline és kezelési nézetek figyelése.</span><span class="sxs-lookup"><span data-stu-id="422e6-175">Monitor hello pipeline using hello data factory monitoring and management views.</span></span> <span data-ttu-id="422e6-176">Lásd: [figyelése és kezelése az adat-előállító adatcsatornák](data-factory-monitor-manage-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="422e6-176">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span>

## <a name="specifying-parameters-for-a-pig-script"></a><span data-ttu-id="422e6-177">A Pig-parancsprogram paramétereinek megadása</span><span class="sxs-lookup"><span data-stu-id="422e6-177">Specifying parameters for a Pig script</span></span>
<span data-ttu-id="422e6-178">Vegye figyelembe a következő példa hello: játék naplók naponta okozhatnak az Azure Blob Storage és egy mappa a particionált alapján dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="422e6-178">Consider hello following example: game logs are ingested daily into Azure Blob Storage and stored in a folder partitioned based on date and time.</span></span> <span data-ttu-id="422e6-179">Szeretné, hogy tooparameterize hello Pig-parancsprogram és a futtatás során dinamikusan adja át a hello bemeneti mappájának helye és hello kimeneti dátum és idő tárolóhelyeinek is készít.</span><span class="sxs-lookup"><span data-stu-id="422e6-179">You want tooparameterize hello Pig script and pass hello input folder location dynamically during runtime and also produce hello output partitioned with date and time.</span></span>

<span data-ttu-id="422e6-180">toouse paraméteres Pig-parancsprogram, hajtsa végre a következő hello:</span><span class="sxs-lookup"><span data-stu-id="422e6-180">toouse parameterized Pig script, do hello following:</span></span>

* <span data-ttu-id="422e6-181">A hello paramétereit **meghatározása**.</span><span class="sxs-lookup"><span data-stu-id="422e6-181">Define hello parameters in **defines**.</span></span>

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
* <span data-ttu-id="422e6-182">A Pig-parancsprogram hello, tekintse meg a toohello paramétereket a "**$parameterName**" hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="422e6-182">In hello Pig Script, refer toohello parameters using '**$parameterName**' as shown in hello following example:</span></span>

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a><span data-ttu-id="422e6-183">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="422e6-183">See Also</span></span>
* [<span data-ttu-id="422e6-184">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="422e6-184">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="422e6-185">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="422e6-185">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="422e6-186">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="422e6-186">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="422e6-187">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="422e6-187">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="422e6-188">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="422e6-188">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

