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
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a><span data-ttu-id="9003f-103">Azure Data Factory használatával Hive tevékenység adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="9003f-103">Transform data using Hive Activity in Azure Data Factory</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="9003f-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="9003f-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="9003f-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="9003f-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="9003f-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="9003f-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="9003f-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="9003f-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="9003f-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="9003f-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="9003f-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="9003f-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="9003f-114">HDInsight Hive tevékenység egy adat-előállítóban hello [csővezeték](data-factory-create-pipelines.md) Hive-lekérdezések végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="9003f-114">hello HDInsight Hive activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hive queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="9003f-115">Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="9003f-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="9003f-116">Ha új tooAzure adat-előállítót, olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) és hello oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="9003f-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="9003f-117">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="9003f-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="9003f-118">Szintaxis részletei</span><span class="sxs-lookup"><span data-stu-id="9003f-118">Syntax details</span></span>
| <span data-ttu-id="9003f-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9003f-119">Property</span></span> | <span data-ttu-id="9003f-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="9003f-120">Description</span></span> | <span data-ttu-id="9003f-121">Szükséges</span><span class="sxs-lookup"><span data-stu-id="9003f-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9003f-122">név</span><span class="sxs-lookup"><span data-stu-id="9003f-122">name</span></span> |<span data-ttu-id="9003f-123">Hello tevékenység neve.</span><span class="sxs-lookup"><span data-stu-id="9003f-123">Name of hello activity</span></span> |<span data-ttu-id="9003f-124">Igen</span><span class="sxs-lookup"><span data-stu-id="9003f-124">Yes</span></span> |
| <span data-ttu-id="9003f-125">leírás</span><span class="sxs-lookup"><span data-stu-id="9003f-125">description</span></span> |<span data-ttu-id="9003f-126">Milyen hello tevékenységgel a leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="9003f-126">Text describing what hello activity is used for</span></span> |<span data-ttu-id="9003f-127">Nem</span><span class="sxs-lookup"><span data-stu-id="9003f-127">No</span></span> |
| <span data-ttu-id="9003f-128">type</span><span class="sxs-lookup"><span data-stu-id="9003f-128">type</span></span> |<span data-ttu-id="9003f-129">HDinsightHive</span><span class="sxs-lookup"><span data-stu-id="9003f-129">HDinsightHive</span></span> |<span data-ttu-id="9003f-130">Igen</span><span class="sxs-lookup"><span data-stu-id="9003f-130">Yes</span></span> |
| <span data-ttu-id="9003f-131">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="9003f-131">inputs</span></span> |<span data-ttu-id="9003f-132">Hello Hive tevékenység által felhasznált bemeneti</span><span class="sxs-lookup"><span data-stu-id="9003f-132">Inputs consumed by hello Hive activity</span></span> |<span data-ttu-id="9003f-133">Nem</span><span class="sxs-lookup"><span data-stu-id="9003f-133">No</span></span> |
| <span data-ttu-id="9003f-134">kimenetek</span><span class="sxs-lookup"><span data-stu-id="9003f-134">outputs</span></span> |<span data-ttu-id="9003f-135">Hello Hive tevékenység által létrehozott kimenet</span><span class="sxs-lookup"><span data-stu-id="9003f-135">Outputs produced by hello Hive activity</span></span> |<span data-ttu-id="9003f-136">Igen</span><span class="sxs-lookup"><span data-stu-id="9003f-136">Yes</span></span> |
| <span data-ttu-id="9003f-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="9003f-137">linkedServiceName</span></span> |<span data-ttu-id="9003f-138">A Data Factory kapcsolt szolgáltatásként regisztrált hivatkozás toohello HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="9003f-138">Reference toohello HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="9003f-139">Igen</span><span class="sxs-lookup"><span data-stu-id="9003f-139">Yes</span></span> |
| <span data-ttu-id="9003f-140">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="9003f-140">script</span></span> |<span data-ttu-id="9003f-141">Adja meg a hello Hive parancsfájl beágyazott</span><span class="sxs-lookup"><span data-stu-id="9003f-141">Specify hello Hive script inline</span></span> |<span data-ttu-id="9003f-142">Nem</span><span class="sxs-lookup"><span data-stu-id="9003f-142">No</span></span> |
| <span data-ttu-id="9003f-143">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="9003f-143">script path</span></span> |<span data-ttu-id="9003f-144">Tároló hello Hive parancsprogram egy Azure blob Storage tárolóban, és adja meg a hello elérési toohello fájlt.</span><span class="sxs-lookup"><span data-stu-id="9003f-144">Store hello Hive script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="9003f-145">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="9003f-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="9003f-146">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="9003f-146">Both cannot be used together.</span></span> <span data-ttu-id="9003f-147">hello fájlnév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="9003f-147">hello file name is case-sensitive.</span></span> |<span data-ttu-id="9003f-148">Nem</span><span class="sxs-lookup"><span data-stu-id="9003f-148">No</span></span> |
| <span data-ttu-id="9003f-149">határozza meg</span><span class="sxs-lookup"><span data-stu-id="9003f-149">defines</span></span> |<span data-ttu-id="9003f-150">Kulcs/érték párok paraméterek meghatározni belül hello Hive parancsfájl segítségével történő "hiveconf" hivatkozik</span><span class="sxs-lookup"><span data-stu-id="9003f-150">Specify parameters as key/value pairs for referencing within hello Hive script using 'hiveconf'</span></span> |<span data-ttu-id="9003f-151">Nem</span><span class="sxs-lookup"><span data-stu-id="9003f-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="9003f-152">Példa</span><span class="sxs-lookup"><span data-stu-id="9003f-152">Example</span></span>
<span data-ttu-id="9003f-153">Mérlegeljük, játék például elemzés, ahol azt szeretné, hogy a felhasználók a vállalat által elindított játékok tooidentify hello idő naplózza.</span><span class="sxs-lookup"><span data-stu-id="9003f-153">Let’s consider an example of game logs analytics where you want tooidentify hello time spent by users playing games launched by your company.</span></span> 

<span data-ttu-id="9003f-154">hello következő log utasítás minta játék napló, amely vessző (`,`) elválasztva, és a következő mezők – ProfileID, SessionStart, időtartam, SrcIPAddress és GameType hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9003f-154">hello following log is a sample game log, which is comma (`,`) separated and contains hello following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="9003f-155">Hello **Hive parancsprogram** tooprocess ezeket az adatokat:</span><span class="sxs-lookup"><span data-stu-id="9003f-155">hello **Hive script** tooprocess this data:</span></span>

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

<span data-ttu-id="9003f-156">a Hive parancsfájl a Data Factory-folyamathoz tooexecute, következőkre lesz szüksége toodo hello</span><span class="sxs-lookup"><span data-stu-id="9003f-156">tooexecute this Hive script in a Data Factory pipeline, you need toodo hello following</span></span>

1. <span data-ttu-id="9003f-157">Hozzon létre egy kapcsolódó szolgáltatás tooregister [saját HDInsight számítási fürt](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálása [igény szerinti HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="9003f-157">Create a linked service tooregister [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="9003f-158">Most hívható "HDInsightLinkedService" szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="9003f-158">Let’s call this linked service “HDInsightLinkedService”.</span></span>
2. <span data-ttu-id="9003f-159">Hozzon létre egy [társított szolgáltatás](data-factory-azure-blob-connector.md) tooconfigure hello kapcsolat tooAzure a Blob storage hello adatok üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="9003f-159">Create a [linked service](data-factory-azure-blob-connector.md) tooconfigure hello connection tooAzure Blob storage hosting hello data.</span></span> <span data-ttu-id="9003f-160">Most a szolgáltatásnak "StorageLinkedService" hívása</span><span class="sxs-lookup"><span data-stu-id="9003f-160">Let’s call this linked service “StorageLinkedService”</span></span>
3. <span data-ttu-id="9003f-161">Hozzon létre [adatkészletek](data-factory-create-datasets.md) toohello bemeneti és a hello mutató kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="9003f-161">Create [datasets](data-factory-create-datasets.md) pointing toohello input and hello output data.</span></span> <span data-ttu-id="9003f-162">Most hívás hello bemeneti adatkészletéből "HiveSampleIn" és a kimeneti adatkészlet "HiveSampleOut" hello</span><span class="sxs-lookup"><span data-stu-id="9003f-162">Let’s call hello input dataset “HiveSampleIn” and hello output dataset “HiveSampleOut”</span></span>
4. <span data-ttu-id="9003f-163">A fájl tooAzure #2. lépésben beállított Blob Storage hello Hive-lekérdezések másolni.</span><span class="sxs-lookup"><span data-stu-id="9003f-163">Copy hello Hive query as a file tooAzure Blob Storage configured in step #2.</span></span> <span data-ttu-id="9003f-164">Ha hello tárolási hello adatok tárolásához nem hello egyet a lekérdezés fájlt, hozzon létre egy külön Azure tárolás társított szolgáltatása, és tekintse meg a tooit hello tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9003f-164">if hello storage for hosting hello data is different from hello one hosting this query file, create a separate Azure Storage linked service and refer tooit in hello activity.</span></span> <span data-ttu-id="9003f-165">Használjon ** scriptPath ** toospecify hello elérési toohive lekérdezésfájl és **scriptLinkedService** toospecify hello hello parancsfájlt tartalmazó az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="9003f-165">Use **scriptPath **toospecify hello path toohive query file and **scriptLinkedService** toospecify hello Azure storage that contains hello script file.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="9003f-166">Hello Hive parancsfájl beágyazott hello tevékenységdefinícióban hello használatával is megadhatja **parancsfájl** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9003f-166">You can also provide hello Hive script inline in hello activity definition by using hello **script** property.</span></span> <span data-ttu-id="9003f-167">Nem ajánlott ennek a megközelítésnek speciális karaktereket hello parancsfájlban hello JSON-dokumentum belül kell toobe escape-karakterrel megjelölve, és előfordulhat, hogy OK hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="9003f-167">We do not recommend this approach as all special characters in hello script within hello JSON document needs toobe escaped and may cause debugging issues.</span></span> <span data-ttu-id="9003f-168">hello ajánlott toofollow #4. lépés.</span><span class="sxs-lookup"><span data-stu-id="9003f-168">hello best practice is toofollow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="9003f-169">Hozzon létre egy folyamatot hello HDInsightHive tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9003f-169">Create a pipeline with hello HDInsightHive activity.</span></span> <span data-ttu-id="9003f-170">hello tevékenység folyamatok/átalakítások hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="9003f-170">hello activity processes/transforms hello data.</span></span>

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
6. <span data-ttu-id="9003f-171">Hello adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="9003f-171">Deploy hello pipeline.</span></span> <span data-ttu-id="9003f-172">Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="9003f-172">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="9003f-173">Hello data factory-figyelés használatával hello pipeline és kezelési nézetek figyelése.</span><span class="sxs-lookup"><span data-stu-id="9003f-173">Monitor hello pipeline using hello data factory monitoring and management views.</span></span> <span data-ttu-id="9003f-174">Lásd: [figyelése és kezelése az adat-előállító adatcsatornák](data-factory-monitor-manage-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="9003f-174">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span> 

## <a name="specifying-parameters-for-a-hive-script"></a><span data-ttu-id="9003f-175">A Hive parancsprogram paramétereinek megadása</span><span class="sxs-lookup"><span data-stu-id="9003f-175">Specifying parameters for a Hive script</span></span>
<span data-ttu-id="9003f-176">Ebben a példában játék naplók az Azure Blob Storage naponta okozhatnak, és egy dátum és idő tárolóhelyeinek mappába kerülnek.</span><span class="sxs-lookup"><span data-stu-id="9003f-176">In this example, game logs are ingested daily into Azure Blob Storage and are stored in a folder partitioned with date and time.</span></span> <span data-ttu-id="9003f-177">Szeretné, hogy tooparameterize hello Hive parancsfájlok és a futásidőben dinamikusan adja át a hello bemeneti mappájának helye és hello kimeneti dátum és idő tárolóhelyeinek is készít.</span><span class="sxs-lookup"><span data-stu-id="9003f-177">You want tooparameterize hello Hive script and pass hello input folder location dynamically during runtime and also produce hello output partitioned with date and time.</span></span>

<span data-ttu-id="9003f-178">toouse paraméteres Hive parancsfájlokat, hajtsa végre a következő hello</span><span class="sxs-lookup"><span data-stu-id="9003f-178">toouse parameterized Hive script, do hello following</span></span>

* <span data-ttu-id="9003f-179">A hello paramétereit **meghatározása**.</span><span class="sxs-lookup"><span data-stu-id="9003f-179">Define hello parameters in **defines**.</span></span>

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
* <span data-ttu-id="9003f-180">A Hive parancsfájl hello, tekintse meg a toohello paraméter használatával **${hiveconf:parameterName}**.</span><span class="sxs-lookup"><span data-stu-id="9003f-180">In hello Hive Script, refer toohello parameter using **${hiveconf:parameterName}**.</span></span> 
  
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
## <a name="see-also"></a><span data-ttu-id="9003f-181">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9003f-181">See Also</span></span>
* [<span data-ttu-id="9003f-182">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="9003f-182">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="9003f-183">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="9003f-183">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="9003f-184">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="9003f-184">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="9003f-185">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="9003f-185">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="9003f-186">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="9003f-186">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

