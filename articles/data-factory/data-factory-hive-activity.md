---
title: "Hive tevékenység - Azure adatok átalakítása |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatja a Hive tevékenység egy Azure data factoryban a-igény szerint vagy a saját HDInsight-fürtök a Hive-lekérdezések futtatásához."
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
ms.openlocfilehash: a3e9b2d0a8c851939acd228d8086ddfc9f38a4c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a><span data-ttu-id="3670e-103">Azure Data Factory használatával Hive tevékenység adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="3670e-103">Transform data using Hive Activity in Azure Data Factory</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="3670e-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="3670e-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="3670e-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="3670e-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="3670e-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="3670e-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="3670e-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="3670e-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="3670e-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="3670e-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="3670e-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="3670e-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="3670e-114">A HDInsight Hive tevékenység egy adat-előállítóban [csővezeték](data-factory-create-pipelines.md) Hive-lekérdezések végrehajtása a [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="3670e-114">The HDInsight Hive activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hive queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="3670e-115">Ez a cikk épít, a [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és a támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="3670e-115">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="3670e-116">Ha most ismerkedik az Azure Data Factory, olvassa végig [Bevezetés az Azure Data Factory](data-factory-introduction.md) hajtsa végre az oktatóanyag: [felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md) a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="3670e-116">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="3670e-117">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="3670e-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="3670e-118">Szintaxis részletei</span><span class="sxs-lookup"><span data-stu-id="3670e-118">Syntax details</span></span>
| <span data-ttu-id="3670e-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="3670e-119">Property</span></span> | <span data-ttu-id="3670e-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="3670e-120">Description</span></span> | <span data-ttu-id="3670e-121">Szükséges</span><span class="sxs-lookup"><span data-stu-id="3670e-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3670e-122">név</span><span class="sxs-lookup"><span data-stu-id="3670e-122">name</span></span> |<span data-ttu-id="3670e-123">A tevékenység neve.</span><span class="sxs-lookup"><span data-stu-id="3670e-123">Name of the activity</span></span> |<span data-ttu-id="3670e-124">Igen</span><span class="sxs-lookup"><span data-stu-id="3670e-124">Yes</span></span> |
| <span data-ttu-id="3670e-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="3670e-125">description</span></span> |<span data-ttu-id="3670e-126">Mire használható a tevékenységet leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="3670e-126">Text describing what the activity is used for</span></span> |<span data-ttu-id="3670e-127">Nem</span><span class="sxs-lookup"><span data-stu-id="3670e-127">No</span></span> |
| <span data-ttu-id="3670e-128">type</span><span class="sxs-lookup"><span data-stu-id="3670e-128">type</span></span> |<span data-ttu-id="3670e-129">HDinsightHive</span><span class="sxs-lookup"><span data-stu-id="3670e-129">HDinsightHive</span></span> |<span data-ttu-id="3670e-130">Igen</span><span class="sxs-lookup"><span data-stu-id="3670e-130">Yes</span></span> |
| <span data-ttu-id="3670e-131">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="3670e-131">inputs</span></span> |<span data-ttu-id="3670e-132">A Hive tevékenység által felhasznált bemeneti</span><span class="sxs-lookup"><span data-stu-id="3670e-132">Inputs consumed by the Hive activity</span></span> |<span data-ttu-id="3670e-133">Nem</span><span class="sxs-lookup"><span data-stu-id="3670e-133">No</span></span> |
| <span data-ttu-id="3670e-134">kimenetek</span><span class="sxs-lookup"><span data-stu-id="3670e-134">outputs</span></span> |<span data-ttu-id="3670e-135">A Hive tevékenység által létrehozott kimenet</span><span class="sxs-lookup"><span data-stu-id="3670e-135">Outputs produced by the Hive activity</span></span> |<span data-ttu-id="3670e-136">Igen</span><span class="sxs-lookup"><span data-stu-id="3670e-136">Yes</span></span> |
| <span data-ttu-id="3670e-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3670e-137">linkedServiceName</span></span> |<span data-ttu-id="3670e-138">A HDInsight-fürthöz, a Data Factory kapcsolt szolgáltatásként regisztrált mutató hivatkozás</span><span class="sxs-lookup"><span data-stu-id="3670e-138">Reference to the HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="3670e-139">Igen</span><span class="sxs-lookup"><span data-stu-id="3670e-139">Yes</span></span> |
| <span data-ttu-id="3670e-140">Parancsfájl</span><span class="sxs-lookup"><span data-stu-id="3670e-140">script</span></span> |<span data-ttu-id="3670e-141">Adja meg a Hive parancsfájl beágyazott</span><span class="sxs-lookup"><span data-stu-id="3670e-141">Specify the Hive script inline</span></span> |<span data-ttu-id="3670e-142">Nem</span><span class="sxs-lookup"><span data-stu-id="3670e-142">No</span></span> |
| <span data-ttu-id="3670e-143">parancsfájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="3670e-143">script path</span></span> |<span data-ttu-id="3670e-144">A Hive-parancsfájl az Azure blob Storage tárolóban tárolja, és adja meg a fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="3670e-144">Store the Hive script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="3670e-145">Használja a "script" vagy "scriptPath" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="3670e-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="3670e-146">Mindkettő nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="3670e-146">Both cannot be used together.</span></span> <span data-ttu-id="3670e-147">A fájlnév pedig kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="3670e-147">The file name is case-sensitive.</span></span> |<span data-ttu-id="3670e-148">Nem</span><span class="sxs-lookup"><span data-stu-id="3670e-148">No</span></span> |
| <span data-ttu-id="3670e-149">határozza meg</span><span class="sxs-lookup"><span data-stu-id="3670e-149">defines</span></span> |<span data-ttu-id="3670e-150">Kulcs/érték párok paraméterek meghatározni a Hive-parancsfájl segítségével történő "hiveconf" belül hivatkozik</span><span class="sxs-lookup"><span data-stu-id="3670e-150">Specify parameters as key/value pairs for referencing within the Hive script using 'hiveconf'</span></span> |<span data-ttu-id="3670e-151">Nem</span><span class="sxs-lookup"><span data-stu-id="3670e-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="3670e-152">Példa</span><span class="sxs-lookup"><span data-stu-id="3670e-152">Example</span></span>
<span data-ttu-id="3670e-153">Mérlegeljük, elemzés, ahol szeretné azonosítani a vállalat által elindított játékok felhasználók fordított idő játék naplók példát.</span><span class="sxs-lookup"><span data-stu-id="3670e-153">Let’s consider an example of game logs analytics where you want to identify the time spent by users playing games launched by your company.</span></span> 

<span data-ttu-id="3670e-154">A következő naplóban minta játék napló, amely vessző (`,`) elválasztva, és a következő mezőket – ProfileID, SessionStart, időtartam, SrcIPAddress és GameType tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3670e-154">The following log is a sample game log, which is comma (`,`) separated and contains the following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="3670e-155">A **Hive parancsprogram** feldolgozni az adatokat:</span><span class="sxs-lookup"><span data-stu-id="3670e-155">The **Hive script** to process this data:</span></span>

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

<span data-ttu-id="3670e-156">A Hive parancsprogram végrehajtása a Data Factory-folyamathoz, tegye a következőket kell</span><span class="sxs-lookup"><span data-stu-id="3670e-156">To execute this Hive script in a Data Factory pipeline, you need to do the following</span></span>

1. <span data-ttu-id="3670e-157">Hozzon létre egy kapcsolódó szolgáltatás regisztrálása [saját HDInsight számítási fürt](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálása [igény szerinti HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="3670e-157">Create a linked service to register [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="3670e-158">Most hívható "HDInsightLinkedService" szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="3670e-158">Let’s call this linked service “HDInsightLinkedService”.</span></span>
2. <span data-ttu-id="3670e-159">Hozzon létre egy [társított szolgáltatás](data-factory-azure-blob-connector.md) üzemeltető Azure Blob Storage a kapcsolat beállításához.</span><span class="sxs-lookup"><span data-stu-id="3670e-159">Create a [linked service](data-factory-azure-blob-connector.md) to configure the connection to Azure Blob storage hosting the data.</span></span> <span data-ttu-id="3670e-160">Most a szolgáltatásnak "StorageLinkedService" hívása</span><span class="sxs-lookup"><span data-stu-id="3670e-160">Let’s call this linked service “StorageLinkedService”</span></span>
3. <span data-ttu-id="3670e-161">Hozzon létre [adatkészletek](data-factory-create-datasets.md) mutat a bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="3670e-161">Create [datasets](data-factory-create-datasets.md) pointing to the input and the output data.</span></span> <span data-ttu-id="3670e-162">Most hívható meg a bemeneti adatkészlet "HiveSampleIn" és "HiveSampleOut" kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="3670e-162">Let’s call the input dataset “HiveSampleIn” and the output dataset “HiveSampleOut”</span></span>
4. <span data-ttu-id="3670e-163">Másolás a Hive-lekérdezést az Azure Blob Storage-fájlként #2. lépésben konfigurált.</span><span class="sxs-lookup"><span data-stu-id="3670e-163">Copy the Hive query as a file to Azure Blob Storage configured in step #2.</span></span> <span data-ttu-id="3670e-164">Ha az adatok tárolásához a tároló eltér a lekérdezés fájlt, hozzon létre egy külön Azure Storage társított szolgáltatást, és a tevékenység hivatkozik rá.</span><span class="sxs-lookup"><span data-stu-id="3670e-164">if the storage for hosting the data is different from the one hosting this query file, create a separate Azure Storage linked service and refer to it in the activity.</span></span> <span data-ttu-id="3670e-165">Használjon ** scriptPath ** a hive lekérdezés fájljának elérési útja és **scriptLinkedService** adhatja meg, amely tartalmazza a parancsfájl az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="3670e-165">Use **scriptPath **to specify the path to hive query file and **scriptLinkedService** to specify the Azure storage that contains the script file.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="3670e-166">A Hive parancsfájl beágyazottan történjen-e a tevékenység definíciójának használatával is megadhatja a **parancsfájl** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3670e-166">You can also provide the Hive script inline in the activity definition by using the **script** property.</span></span> <span data-ttu-id="3670e-167">A Microsoft nem javasolja ezt a módszert használja, a parancsfájl belül a JSON-dokumentum kell kell megjelölni a speciális karaktereket, és hibakeresési hibákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="3670e-167">We do not recommend this approach as all special characters in the script within the JSON document needs to be escaped and may cause debugging issues.</span></span> <span data-ttu-id="3670e-168">A bevált gyakorlat követéséhez #4. lépés.</span><span class="sxs-lookup"><span data-stu-id="3670e-168">The best practice is to follow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="3670e-169">Hozzon létre egy folyamatot a HDInsightHive tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3670e-169">Create a pipeline with the HDInsightHive activity.</span></span> <span data-ttu-id="3670e-170">A tevékenység folyamatok/átalakítások az adatokat.</span><span class="sxs-lookup"><span data-stu-id="3670e-170">The activity processes/transforms the data.</span></span>

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
6. <span data-ttu-id="3670e-171">Az adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="3670e-171">Deploy the pipeline.</span></span> <span data-ttu-id="3670e-172">Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="3670e-172">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="3670e-173">A data factory kiszolgálófigyelési és -kezelési nézetek segítségével folyamat figyelésére.</span><span class="sxs-lookup"><span data-stu-id="3670e-173">Monitor the pipeline using the data factory monitoring and management views.</span></span> <span data-ttu-id="3670e-174">Lásd: [figyelése és kezelése az adat-előállító adatcsatornák](data-factory-monitor-manage-pipelines.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="3670e-174">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span> 

## <a name="specifying-parameters-for-a-hive-script"></a><span data-ttu-id="3670e-175">A Hive parancsprogram paramétereinek megadása</span><span class="sxs-lookup"><span data-stu-id="3670e-175">Specifying parameters for a Hive script</span></span>
<span data-ttu-id="3670e-176">Ebben a példában játék naplók az Azure Blob Storage naponta okozhatnak, és egy dátum és idő tárolóhelyeinek mappába kerülnek.</span><span class="sxs-lookup"><span data-stu-id="3670e-176">In this example, game logs are ingested daily into Azure Blob Storage and are stored in a folder partitioned with date and time.</span></span> <span data-ttu-id="3670e-177">Szeretné parametrizálja a Hive-parancsfájl és futásidőben dinamikusan adja át a bemeneti mappa helyét és a kimeneti dátum és idő tárolóhelyeinek is készít.</span><span class="sxs-lookup"><span data-stu-id="3670e-177">You want to parameterize the Hive script and pass the input folder location dynamically during runtime and also produce the output partitioned with date and time.</span></span>

<span data-ttu-id="3670e-178">A paraméteres Hive parancsfájl használatára, tegye a következőket</span><span class="sxs-lookup"><span data-stu-id="3670e-178">To use parameterized Hive script, do the following</span></span>

* <span data-ttu-id="3670e-179">Adja meg a paramétereket a **meghatározása**.</span><span class="sxs-lookup"><span data-stu-id="3670e-179">Define the parameters in **defines**.</span></span>

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
* <span data-ttu-id="3670e-180">A Hive parancsfájl tekintse meg a paraméter használatával **${hiveconf:parameterName}**.</span><span class="sxs-lookup"><span data-stu-id="3670e-180">In the Hive Script, refer to the parameter using **${hiveconf:parameterName}**.</span></span> 
  
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
## <a name="see-also"></a><span data-ttu-id="3670e-181">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3670e-181">See Also</span></span>
* [<span data-ttu-id="3670e-182">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="3670e-182">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="3670e-183">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="3670e-183">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="3670e-184">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="3670e-184">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="3670e-185">Spark-programok meghívása</span><span class="sxs-lookup"><span data-stu-id="3670e-185">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="3670e-186">R-szkriptek meghívása</span><span class="sxs-lookup"><span data-stu-id="3670e-186">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

