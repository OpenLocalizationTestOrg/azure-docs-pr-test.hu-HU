---
title: "Azure Data Factory használatával aaaCreate prediktív adatok folyamatok |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate létre Azure Data Factory és az Azure Machine Learning a prediktív folyamatok"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 943210c28b1696e299ff9b7cc96369b95f182354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a><span data-ttu-id="e8ade-103">Hozzon létre prediktív folyamatok Azure Machine Learning és az Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="e8ade-103">Create predictive pipelines using Azure Machine Learning and Azure Data Factory</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="e8ade-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="e8ade-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="e8ade-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="e8ade-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="e8ade-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="e8ade-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="e8ade-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="e8ade-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="e8ade-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="e8ade-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="e8ade-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="e8ade-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="e8ade-114">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e8ade-114">Introduction</span></span>

### <a name="azure-machine-learning"></a><span data-ttu-id="e8ade-115">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="e8ade-115">Azure Machine Learning</span></span>
<span data-ttu-id="e8ade-116">[Az Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) lehetővé teszi, hogy toobuild, tesztelését és rendszerbe prediktív elemzési megoldások.</span><span class="sxs-lookup"><span data-stu-id="e8ade-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) enables you toobuild, test, and deploy predictive analytics solutions.</span></span> <span data-ttu-id="e8ade-117">Magas szintű szempontból elkészült a három lépést:</span><span class="sxs-lookup"><span data-stu-id="e8ade-117">From a high-level point of view, it is done in three steps:</span></span>

1. <span data-ttu-id="e8ade-118">**Hozzon létre egy tanítási kísérletet**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-118">**Create a training experiment**.</span></span> <span data-ttu-id="e8ade-119">Ez a lépés hello Azure ML Studio úgy teheti meg.</span><span class="sxs-lookup"><span data-stu-id="e8ade-119">You do this step by using hello Azure ML Studio.</span></span> <span data-ttu-id="e8ade-120">hello ML studio olyan együttműködési Látványelem-fejlesztési környezet tootrain használni, és tesztelje a prediktív elemzési modell betanítási adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="e8ade-120">hello ML studio is a collaborative visual development environment that you use tootrain and test a predictive analytics model using training data.</span></span>
2. <span data-ttu-id="e8ade-121">**Alakítsa át a tooa prediktív kísérletté**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-121">**Convert it tooa predictive experiment**.</span></span> <span data-ttu-id="e8ade-122">Ha a modell még betanítva meglévő adatokkal, és készen áll a toouse azt tooscore új adatokat, előkészítéséhez és pontozó kísérletbe egyszerűsítésére.</span><span class="sxs-lookup"><span data-stu-id="e8ade-122">Once your model has been trained with existing data and you are ready toouse it tooscore new data, you prepare and streamline your experiment for scoring.</span></span>
3. <span data-ttu-id="e8ade-123">**A webszolgáltatás üzembe**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-123">**Deploy it as a web service**.</span></span> <span data-ttu-id="e8ade-124">A pontozási kísérlet közzéteheti Azure webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="e8ade-124">You can publish your scoring experiment as an Azure web service.</span></span> <span data-ttu-id="e8ade-125">Is tooyour adatmodell keresztül a webes szolgáltatás végpontját küldése és fogadása eredmény előrejelzéseket eredete hello modell.</span><span class="sxs-lookup"><span data-stu-id="e8ade-125">You can send data tooyour model via this web service end point and receive result predictions fro hello model.</span></span>  

### <a name="azure-data-factory"></a><span data-ttu-id="e8ade-126">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="e8ade-126">Azure Data Factory</span></span>
<span data-ttu-id="e8ade-127">Adat-előállító koordinálja és automatizálja a hello felhőalapú adatok integrációs szolgáltatás **adatátviteli** és **átalakítása** adatok.</span><span class="sxs-lookup"><span data-stu-id="e8ade-127">Data Factory is a cloud-based data integration service that orchestrates and automates hello **movement** and **transformation** of data.</span></span> <span data-ttu-id="e8ade-128">Adatok integrációs megoldásokat Azure Data Factory használatával is különböző adattárolókhoz származó adatok, átalakító/folyamat hello adatok és közzététele hello eredmény adatok toohello adattárolókhoz hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="e8ade-128">You can create data integration solutions using Azure Data Factory that can ingest data from various data stores, transform/process hello data, and publish hello result data toohello data stores.</span></span>

<span data-ttu-id="e8ade-129">Data Factory szolgáltatásnak lehetővé teszi, hogy helyezze át, és az adatok átalakítása toocreate adatok folyamatok, és futtassa a hello folyamatok meghatározott ütemezés szerint (óránként, naponta, hetente, stb.).</span><span class="sxs-lookup"><span data-stu-id="e8ade-129">Data Factory service allows you toocreate data pipelines that move and transform data, and then run hello pipelines on a specified schedule (hourly, daily, weekly, etc.).</span></span> <span data-ttu-id="e8ade-130">Emellett biztosít gazdag képi megjelenítések toodisplay hello Leszármaztatás és az adatok folyamatok közti függőségeket és minden az adatok folyamatok egy egyetlen egyesítve láthassák tooeasily meghatározhatja problémák figyelése és beállítása a figyelési riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="e8ade-130">It also provides rich visualizations toodisplay hello lineage and dependencies between your data pipelines, and monitor all your data pipelines from a single unified view tooeasily pinpoint issues and setup monitoring alerts.</span></span>

<span data-ttu-id="e8ade-131">Lásd: [Data Factory bemutatása tooAzure](data-factory-introduction.md) és [felépítheti első folyamatát](data-factory-build-your-first-pipeline.md) cikkek tooquickly hello Azure Data Factory szolgáltatásnak az első lépései.</span><span class="sxs-lookup"><span data-stu-id="e8ade-131">See [Introduction tooAzure Data Factory](data-factory-introduction.md) and [Build your first pipeline](data-factory-build-your-first-pipeline.md) articles tooquickly get started with hello Azure Data Factory service.</span></span>

### <a name="data-factory-and-machine-learning-together"></a><span data-ttu-id="e8ade-132">Adat-előállító és a gépi tanulás együtt</span><span class="sxs-lookup"><span data-stu-id="e8ade-132">Data Factory and Machine Learning together</span></span>
<span data-ttu-id="e8ade-133">Az Azure Data Factory lehetővé teszi, hogy Ön tooeasily hozzon létre egy közzétett használó folyamatok [Azure Machine Learning] [ azure-machine-learning] webszolgáltatás prediktív elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="e8ade-133">Azure Data Factory enables you tooeasily create pipelines that use a published [Azure Machine Learning][azure-machine-learning] web service for predictive analytics.</span></span> <span data-ttu-id="e8ade-134">Hello segítségével **kötegelt végrehajtási tevékenység** egy Azure Data Factory-folyamat a hívhat meg az Azure ML web toomake előrejelzések kötegben hello adatokon.</span><span class="sxs-lookup"><span data-stu-id="e8ade-134">Using hello **Batch Execution Activity** in an Azure Data Factory pipeline, you can invoke an Azure ML web service toomake predictions on hello data in batch.</span></span> <span data-ttu-id="e8ade-135">Lásd: [meghívása az Azure ML kötegelt végrehajtási tevékenység webes szolgáltatás használatával hello](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="e8ade-135">See [Invoking an Azure ML web service using hello Batch Execution Activity](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) section for details.</span></span>

<span data-ttu-id="e8ade-136">Az idő múlásával hello prediktív modelleket hello Azure ML pontozási kísérletekben kell toobe retrained új bemeneti adatkészletek használata.</span><span class="sxs-lookup"><span data-stu-id="e8ade-136">Over time, hello predictive models in hello Azure ML scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="e8ade-137">A Data Factory-folyamat az Azure ML modellje is újratanítása hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="e8ade-137">You can retrain an Azure ML model from a Data Factory pipeline by doing hello following steps:</span></span>

1. <span data-ttu-id="e8ade-138">Tegye közzé a hello tanítási kísérletet (nem prediktív kísérletté) webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="e8ade-138">Publish hello training experiment (not predictive experiment) as a web service.</span></span> <span data-ttu-id="e8ade-139">Ezt megteheti ezt a lépést, az Azure ML Studio hello úgy, ahogy tooexpose prediktív kísérletté hello előző forgatókönyvben webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="e8ade-139">You do this step in hello Azure ML Studio as you did tooexpose predictive experiment as a web service in hello previous scenario.</span></span>
2. <span data-ttu-id="e8ade-140">Hello Azure ML kötegelt végrehajtási tevékenység tooinvoke hello webes szolgáltatás hello tanítási kísérletet segítségével.</span><span class="sxs-lookup"><span data-stu-id="e8ade-140">Use hello Azure ML Batch Execution Activity tooinvoke hello web service for hello training experiment.</span></span> <span data-ttu-id="e8ade-141">Alapvetően hello Azure ML kötegelt végrehajtási tevékenység tooinvoke webszolgáltatás képzési és a webszolgáltatás pontozási is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e8ade-141">Basically, you can use hello Azure ML Batch Execution activity tooinvoke both training web service and scoring web service.</span></span>

<span data-ttu-id="e8ade-142">Után átképezési befejezése, frissítse a webszolgáltatás (prediktív kísérletté webszolgáltatásként kitett) pontozási hello újonnan betanított modell hello segítségével hello **Azure ML Update Erőforrástevékenység**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-142">After you are done with retraining, update hello scoring web service (predictive experiment exposed as a web service) with hello newly trained model by using hello **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="e8ade-143">Lásd: [modellek használata az Update-Erőforrástevékenység frissítése](data-factory-azure-ml-update-resource-activity.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="e8ade-143">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

## <a name="invoking-a-web-service-using-batch-execution-activity"></a><span data-ttu-id="e8ade-144">Meghívja a webszolgáltatást kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-144">Invoking a web service using Batch Execution Activity</span></span>
<span data-ttu-id="e8ade-145">Azure Data Factory tooorchestrate adatmozgatást és feldolgozási használja, és hajtsa végre az Azure Machine Learning kötegelt végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="e8ade-145">You use Azure Data Factory tooorchestrate data movement and processing, and then perform batch execution using Azure Machine Learning.</span></span> <span data-ttu-id="e8ade-146">Az alábbiakban hello legfelső szintű lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e8ade-146">Here are hello top-level steps:</span></span>

1. <span data-ttu-id="e8ade-147">Hozzon létre egy Azure Machine Learning társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e8ade-147">Create an Azure Machine Learning linked service.</span></span> <span data-ttu-id="e8ade-148">A következő értékek hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="e8ade-148">You need hello following values:</span></span>

   1. <span data-ttu-id="e8ade-149">**A kérelmi URI** hello kötegelt végrehajtási API számára.</span><span class="sxs-lookup"><span data-stu-id="e8ade-149">**Request URI** for hello Batch Execution API.</span></span> <span data-ttu-id="e8ade-150">Hello kérelem URI-CÍMÉN található hello kattintva **KÖTEGELT végrehajtási** hello szolgáltatások weblap hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="e8ade-150">You can find hello Request URI by clicking hello **BATCH EXECUTION** link in hello web services page.</span></span>
   2. <span data-ttu-id="e8ade-151">**Az API-kulcs** a hello közzétett Azure Machine Learning webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="e8ade-151">**API key** for hello published Azure Machine Learning web service.</span></span> <span data-ttu-id="e8ade-152">Hello API-kulcs hello webszolgáltatás, amelyet a közzétett kattintva találja.</span><span class="sxs-lookup"><span data-stu-id="e8ade-152">You can find hello API key by clicking hello web service that you have published.</span></span>
   3. <span data-ttu-id="e8ade-153">Használjon hello **AzureMLBatchExecution** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e8ade-153">Use hello **AzureMLBatchExecution** activity.</span></span>

      ![Machine Learning irányítópult](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Kötegelt URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-toodata-in-azure-blob-storage"></a><span data-ttu-id="e8ade-156">Forgatókönyv: Kísérletek webes szolgáltatás bemenetek/kimenetek, tekintse meg az Azure Blob Storage toodata használatával</span><span class="sxs-lookup"><span data-stu-id="e8ade-156">Scenario: Experiments using Web service inputs/outputs that refer toodata in Azure Blob Storage</span></span>
<span data-ttu-id="e8ade-157">Ebben a forgatókönyvben a hello Azure Machine Learning webszolgáltatás lehetővé teszi az adatok segítségével az Azure blob Storage-fájlból való előrejelzéseket, és hello előrejelzés eredmények hello blob Storage tárolóban tárolja.</span><span class="sxs-lookup"><span data-stu-id="e8ade-157">In this scenario, hello Azure Machine Learning Web service makes predictions using data from a file in an Azure blob storage and stores hello prediction results in hello blob storage.</span></span> <span data-ttu-id="e8ade-158">hello következő JSON határozza meg a Data Factory-folyamathoz egy AzureMLBatchExecution tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="e8ade-158">hello following JSON defines a Data Factory pipeline with an AzureMLBatchExecution activity.</span></span> <span data-ttu-id="e8ade-159">hello tevékenység rendelkezik hello dataset **DecisionTreeInputBlob** bemenetként és **DecisionTreeResultBlob** hello output típusúként.</span><span class="sxs-lookup"><span data-stu-id="e8ade-159">hello activity has hello dataset **DecisionTreeInputBlob** as input and **DecisionTreeResultBlob** as hello output.</span></span> <span data-ttu-id="e8ade-160">Hello **DecisionTreeInputBlob** átadása egy bemeneti toohello webszolgáltatásként által hello segítségével **típus** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8ade-160">hello **DecisionTreeInputBlob** is passed as an input toohello web service by using hello **webServiceInput** JSON property.</span></span> <span data-ttu-id="e8ade-161">Hello **DecisionTreeResultBlob** átadása egy kimeneti toohello webszolgáltatás által hello segítségével **webServiceOutputs** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8ade-161">hello **DecisionTreeResultBlob** is passed as an output toohello Web service by using hello **webServiceOutputs** JSON property.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="e8ade-162">Ha hello webszolgáltatás több bemeneti vesz igénybe, használja a hello **webServiceInputs** tulajdonság használata helyett **típus**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-162">If hello web service takes multiple inputs, use hello **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="e8ade-163">Lásd: hello [webszolgáltatás több bemeneti értéket igényel](#web-service-requires-multiple-inputs) szakasz egy példát hello webServiceInputs tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="e8ade-163">See hello [Web service requires multiple inputs](#web-service-requires-multiple-inputs) section for an example of using hello webServiceInputs property.</span></span>
>
> <span data-ttu-id="e8ade-164">Hello által hivatkozott adatkészletek **típus**/**webServiceInputs** és **webServiceOutputs** tulajdonságok (a  **typeProperties**) is szerepelnie kell hello tevékenység **bemenetek** és **kimenete**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-164">Datasets that are referenced by hello **webServiceInput**/**webServiceInputs** and **webServiceOutputs** properties (in **typeProperties**) must also be included in hello Activity **inputs** and **outputs**.</span></span>
>
> <span data-ttu-id="e8ade-165">Az Azure ML kísérletben webszolgáltatás bemenetét és a kimeneti portok és a globális paraméterek alapértelmezett neve lehet ("input1", "input2"), amely testre szabható.</span><span class="sxs-lookup"><span data-stu-id="e8ade-165">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="e8ade-166">webServiceInputs, webServiceOutputs és globalParameters beállítások használt hello neveket hello kísérletekben hello neveket pontosan egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="e8ade-166">hello names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match hello names in hello experiments.</span></span> <span data-ttu-id="e8ade-167">Az Azure ML végpont tooverify várt hello leképezés hello minta-kérések forgalma hello kötegelt végrehajtási súgó lapon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e8ade-167">You can view hello sample request payload on hello Batch Execution Help page for your Azure ML endpoint tooverify hello expected mapping.</span></span>
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> <span data-ttu-id="e8ade-168">Csak be- és kimenetekkel hello AzureMLBatchExecution tevékenység paraméterek toohello webszolgáltatás argumentumként átadhatók.</span><span class="sxs-lookup"><span data-stu-id="e8ade-168">Only inputs and outputs of hello AzureMLBatchExecution activity can be passed as parameters toohello Web service.</span></span> <span data-ttu-id="e8ade-169">A fenti JSON részlet hello, például a DecisionTreeInputBlob egy bemeneti toohello AzureMLBatchExecution tevékenység, mint egy bemeneti toohello webszolgáltatás átadott típus paraméteren keresztül.</span><span class="sxs-lookup"><span data-stu-id="e8ade-169">For example, in hello above JSON snippet, DecisionTreeInputBlob is an input toohello AzureMLBatchExecution activity, which is passed as an input toohello Web service via webServiceInput parameter.</span></span>   
>
>

### <a name="example"></a><span data-ttu-id="e8ade-170">Példa</span><span class="sxs-lookup"><span data-stu-id="e8ade-170">Example</span></span>
<span data-ttu-id="e8ade-171">A példa használja az Azure Storage toohold mindkét hello bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8ade-171">This example uses Azure Storage toohold both hello input and output data.</span></span>

<span data-ttu-id="e8ade-172">Javasoljuk, hogy olvassa végig hello [felépítheti első folyamatát Data Factory] [ adf-build-1st-pipeline] oktatóanyag, mielőtt továbblépne ebben a példában keresztül.</span><span class="sxs-lookup"><span data-stu-id="e8ade-172">We recommend that you go through hello [Build your first pipeline with Data Factory][adf-build-1st-pipeline] tutorial before going through this example.</span></span> <span data-ttu-id="e8ade-173">Ebben a példában hello Data Factory Editor toocreate adat-előállító összetevők (társított szolgáltatások, adatkészleteket, pipeline) használja.</span><span class="sxs-lookup"><span data-stu-id="e8ade-173">Use hello Data Factory Editor toocreate Data Factory artifacts (linked services, datasets, pipeline) in this example.</span></span>   

1. <span data-ttu-id="e8ade-174">Hozzon létre egy **társított szolgáltatás** a a **Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-174">Create a **linked service** for your **Azure Storage**.</span></span> <span data-ttu-id="e8ade-175">Ha hello bemeneti és kimeneti fájlok eltérő tárfiókokból, két összekapcsolt szolgáltatások szüksége.</span><span class="sxs-lookup"><span data-stu-id="e8ade-175">If hello input and output files are in different storage accounts, you need two linked services.</span></span> <span data-ttu-id="e8ade-176">Íme egy JSON-példa:</span><span class="sxs-lookup"><span data-stu-id="e8ade-176">Here is a JSON example:</span></span>

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. <span data-ttu-id="e8ade-177">Hozzon létre hello **bemeneti** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-177">Create hello **input** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="e8ade-178">Néhány egyéb adat-előállító adatkészletek ellentétben ezek az adatkészletek tartalmaznia kell mindkét **folderPath** és **Fájlnév** értékeket.</span><span class="sxs-lookup"><span data-stu-id="e8ade-178">Unlike some other Data Factory datasets, these datasets must contain both **folderPath** and **fileName** values.</span></span> <span data-ttu-id="e8ade-179">Használhat particionálási toocause minden kötegelt végrehajtási (minden adatszelet) tooprocess vagy tud létrehozni egyedi bemeneti és kimeneti fájlok.</span><span class="sxs-lookup"><span data-stu-id="e8ade-179">You can use partitioning toocause each batch execution (each data slice) tooprocess or produce unique input and output files.</span></span> <span data-ttu-id="e8ade-180">Szükség lehet néhány felsőbb szintű tevékenység tootransform hello hello CSV-fájlformátumot bemeneteként, és helyezheti el hello tárfiókot az egyes szeletek tooinclude.</span><span class="sxs-lookup"><span data-stu-id="e8ade-180">You may need tooinclude some upstream activity tootransform hello input into hello CSV file format and place it in hello storage account for each slice.</span></span> <span data-ttu-id="e8ade-181">Ebben az esetben nem tartalmazná hello **külső** és **externalData** látható példa, és a DecisionTreeInputBlob lenne hello kimeneti adatkészlet egy másik tevékenység a következő hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="e8ade-181">In that case, you would not include hello **external** and **externalData** settings shown in hello following example, and your DecisionTreeInputBlob would be hello output dataset of a different Activity.</span></span>

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }
    ```

    <span data-ttu-id="e8ade-182">A bemeneti csv-fájlban hello oszlop fejlécsor kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="e8ade-182">Your input csv file must have hello column header row.</span></span> <span data-ttu-id="e8ade-183">Hello használata **másolási tevékenység** toocreate/move hello csv hello blob tárolóba, célszerű hello fogadó tulajdonság **blobWriterAddHeader** túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-183">If you are using hello **Copy Activity** toocreate/move hello csv into hello blob storage, you should set hello sink property **blobWriterAddHeader** too**true**.</span></span> <span data-ttu-id="e8ade-184">Példa:</span><span class="sxs-lookup"><span data-stu-id="e8ade-184">For example:</span></span>

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    <span data-ttu-id="e8ade-185">Hello csv-fájl nem rendelkezik hello fejlécsor, jelenhet meg a következő hiba hello: **hiba a tevékenység: Hiba történt a karakterlánc olvasásakor. Váratlan lexikális elem: StartObject. Elérési út ", 1. sor, 1 elhelyezése**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-185">If hello csv file does not have hello header row, you may see hello following error: **Error in Activity: Error reading string. Unexpected token: StartObject. Path '', line 1, position 1**.</span></span>
3. <span data-ttu-id="e8ade-186">Hozzon létre hello **kimeneti** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-186">Create hello **output** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="e8ade-187">A példa particionálási toocreate egy egyedi kimeneti elérési út minden szelet végrehajtásra.</span><span class="sxs-lookup"><span data-stu-id="e8ade-187">This example uses partitioning toocreate a unique output path for each slice execution.</span></span> <span data-ttu-id="e8ade-188">Hello particionálás, nélkül hello tevékenység felülírná hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="e8ade-188">Without hello partitioning, hello activity would overwrite hello file.</span></span>

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. <span data-ttu-id="e8ade-189">Hozzon létre egy **társított szolgáltatás** típusú: **AzureMLLinkedService**, hello API-kulcsot biztosító és modellhez tartozó kötegelt végrehajtás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="e8ade-189">Create a **linked service** of type: **AzureMLLinkedService**, providing hello API key and model batch execution URL.</span></span>

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. <span data-ttu-id="e8ade-190">Végezetül szerzői a folyamat, amely tartalmazza egy **AzureMLBatchExecution** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e8ade-190">Finally, author a pipeline containing an **AzureMLBatchExecution** Activity.</span></span> <span data-ttu-id="e8ade-191">Futásidőben a folyamat hello a lépéseket követve hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="e8ade-191">At runtime, pipeline performs hello following steps:</span></span>

   1. <span data-ttu-id="e8ade-192">A bemeneti adatkészletek hello bemeneti fájl helye hello lekérése.</span><span class="sxs-lookup"><span data-stu-id="e8ade-192">Gets hello location of hello input file from your input datasets.</span></span>
   2. <span data-ttu-id="e8ade-193">Meghívja a hello Azure Machine Learning kötegelt végrehajtási API</span><span class="sxs-lookup"><span data-stu-id="e8ade-193">Invokes hello Azure Machine Learning batch execution API</span></span>
   3. <span data-ttu-id="e8ade-194">Másolja a kötegelt végrehajtási kimeneti toohello blob a kimeneti adatkészlet megadott hello.</span><span class="sxs-lookup"><span data-stu-id="e8ade-194">Copies hello batch execution output toohello blob given in your output dataset.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e8ade-195">AzureMLBatchExecution tevékenység állhat nulla vagy több be- és egy vagy több kimenetekkel.</span><span class="sxs-lookup"><span data-stu-id="e8ade-195">AzureMLBatchExecution activity can have zero or more inputs and one or more outputs.</span></span>
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      <span data-ttu-id="e8ade-196">Mindkét **start** és **end** időpontok szerepelnie kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="e8ade-196">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="e8ade-197">Például: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="e8ade-197">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="e8ade-198">Hello **end** idő megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e8ade-198">hello **end** time is optional.</span></span> <span data-ttu-id="e8ade-199">Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra.**"</span><span class="sxs-lookup"><span data-stu-id="e8ade-199">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="e8ade-200">toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8ade-200">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span> <span data-ttu-id="e8ade-201">A JSON-tulajdonságokkal kapcsolatos információkért lásd: [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referencia a JSON-parancsprogramokhoz).</span><span class="sxs-lookup"><span data-stu-id="e8ade-201">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e8ade-202">A megadott hello AzureMLBatchExecution tevékenység megadása nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="e8ade-202">Specifying input for hello AzureMLBatchExecution activity is optional.</span></span>
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-toorefer-toodata-in-various-storages"></a><span data-ttu-id="e8ade-203">Forgatókönyv: Kísérletek olvasási/írási modulok toorefer toodata különböző tárolók használata</span><span class="sxs-lookup"><span data-stu-id="e8ade-203">Scenario: Experiments using Reader/Writer Modules toorefer toodata in various storages</span></span>
<span data-ttu-id="e8ade-204">Egy másik forgatókönyve az Azure ML kísérletek létrehozásakor toouse írási és olvasási szerepkörökhöz modulok.</span><span class="sxs-lookup"><span data-stu-id="e8ade-204">Another common scenario when creating Azure ML experiments is toouse Reader and Writer modules.</span></span> <span data-ttu-id="e8ade-205">hello olvasó modul kísérlet használt tooload adatokat, és hello író modul toosave adatok a kísérletekből.</span><span class="sxs-lookup"><span data-stu-id="e8ade-205">hello reader module is used tooload data into an experiment and hello writer module is toosave data from your experiments.</span></span> <span data-ttu-id="e8ade-206">Írási és olvasási szerepkörökhöz modullal kapcsolatos részletekért lásd: [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) témakörök MSDN könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="e8ade-206">For details about reader and writer modules, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span>     

<span data-ttu-id="e8ade-207">Hello írási és olvasási szerepkörökhöz modulok használata esetén célszerű toouse olvasási/írási modulok minden egyes tulajdonság egy webszolgáltatási paraméter is.</span><span class="sxs-lookup"><span data-stu-id="e8ade-207">When using hello reader and writer modules, it is good practice toouse a Web service parameter for each property of these reader/writer modules.</span></span> <span data-ttu-id="e8ade-208">Ezek a paraméterek lehetővé teszik a tooconfigure hello értékek futásidőben.</span><span class="sxs-lookup"><span data-stu-id="e8ade-208">These web parameters enable you tooconfigure hello values during runtime.</span></span> <span data-ttu-id="e8ade-209">Például egy olvasó modul, amely használja az Azure SQL Database segítségével létrehozhat egy kísérlet: XXX.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="e8ade-209">For example, you could create an experiment with a reader module that uses an Azure SQL Database: XXX.database.windows.net.</span></span> <span data-ttu-id="e8ade-210">Hello webszolgáltatás telepítése után szeretné tooenable hello fogyasztói hello web service toospecify egy másik Azure SQL Server YYY.database.windows.net hívása.</span><span class="sxs-lookup"><span data-stu-id="e8ade-210">After hello web service has been deployed, you want tooenable hello consumers of hello web service toospecify another Azure SQL Server called YYY.database.windows.net.</span></span> <span data-ttu-id="e8ade-211">Egy webes szolgáltatás paraméter tooallow a konfigurált érték toobe használható.</span><span class="sxs-lookup"><span data-stu-id="e8ade-211">You can use a Web service parameter tooallow this value toobe configured.</span></span>

> [!NOTE]
> <span data-ttu-id="e8ade-212">Webes szolgáltatás bemeneti és kimeneti eltérnek a webszolgáltatás-paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e8ade-212">Web service input and output are different from Web service parameters.</span></span> <span data-ttu-id="e8ade-213">Hello első forgatókönyvben láthatta, hogyan egy bemeneti és kimeneti adható meg az Azure gépi tanulás webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e8ade-213">In hello first scenario, you have seen how an input and output can be specified for an Azure ML Web service.</span></span> <span data-ttu-id="e8ade-214">Ebben a forgatókönyvben át egy webszolgáltatás, amelyek megfelelnek az olvasási/írási modulok tooproperties paramétereit.</span><span class="sxs-lookup"><span data-stu-id="e8ade-214">In this scenario, you pass parameters for a Web service that correspond tooproperties of reader/writer modules.</span></span>
>
>

<span data-ttu-id="e8ade-215">Vizsgáljuk meg egy olyan helyzetben használhat webszolgáltatás-paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e8ade-215">Let's look at a scenario for using Web service parameters.</span></span> <span data-ttu-id="e8ade-216">Egy telepített Azure Machine Learning webszolgáltatás, amelyet egy olvasó modul tooread adatokat használ az Azure Machine Learning által támogatott adatforrások hello egyik rendelkezik (például: Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="e8ade-216">You have a deployed Azure Machine Learning web service that uses a reader module tooread data from one of hello data sources supported by Azure Machine Learning (for example: Azure SQL Database).</span></span> <span data-ttu-id="e8ade-217">Hello kötegelt végrehajtási hajtja végre, hello eredmények írt író modul (az Azure SQL Database) használatával.</span><span class="sxs-lookup"><span data-stu-id="e8ade-217">After hello batch execution is performed, hello results are written using a Writer module (Azure SQL Database).</span></span>  <span data-ttu-id="e8ade-218">Nincs a webszolgáltatás bemenetei és kimenetei hello kísérletek vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="e8ade-218">No web service inputs and outputs are defined in hello experiments.</span></span> <span data-ttu-id="e8ade-219">Ebben az esetben ajánlott, hogy konfigurálja a hello írási és olvasási szerepkörökhöz modulok vonatkozó webszolgáltatás-paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e8ade-219">In this case, we recommend that you configure relevant web service parameters for hello reader and writer modules.</span></span> <span data-ttu-id="e8ade-220">Ez a konfiguráció lehetővé teszi, hogy hello olvasási/írási modulok toobe az hello AzureMLBatchExecution tevékenység konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e8ade-220">This configuration allows hello reader/writer modules toobe configured when using hello AzureMLBatchExecution activity.</span></span> <span data-ttu-id="e8ade-221">Webszolgáltatás-paramétereket ad meg hello **globalParameters** hello tevékenység JSON szakasz az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="e8ade-221">You specify Web service parameters in hello **globalParameters** section in hello activity JSON as follows.</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

<span data-ttu-id="e8ade-222">Is [Data Factory funkciók](data-factory-functions-variables.md) a sikeres hello értékeinek webszolgáltatás-paramétereket a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="e8ade-222">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for hello Web service parameters as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="e8ade-223">Webszolgáltatás-paramétereket hello kis-és nagybetűket, ezért figyeljen oda arra, hogy hello hello tevékenységben megadott JSON megfelel hello hello webszolgáltatás által elérhetővé tett néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="e8ade-223">hello Web service parameters are case-sensitive, so ensure that hello names you specify in hello activity JSON match hello ones exposed by hello Web service.</span></span>
>
>

### <a name="using-a-reader-module-tooread-data-from-multiple-files-in-azure-blob"></a><span data-ttu-id="e8ade-224">Az Azure Blob több fájlból egy olvasó modul tooread adatok használata</span><span class="sxs-lookup"><span data-stu-id="e8ade-224">Using a Reader module tooread data from multiple files in Azure Blob</span></span>
<span data-ttu-id="e8ade-225">Big Data típusú adatok folyamatok a tevékenységeket, például a Pig és Hive is létrehozhat egy vagy több kimeneti fájlokat a bővítmények nincsenek.</span><span class="sxs-lookup"><span data-stu-id="e8ade-225">Big data pipelines with activities such as Pig and Hive can produce one or more output files with no extensions.</span></span> <span data-ttu-id="e8ade-226">Például egy külső Hive tábla megadásakor hello külső Hive tábla adatai hello tárolhatók az Azure blob storage a következő neve 000000_0 hello.</span><span class="sxs-lookup"><span data-stu-id="e8ade-226">For example, when you specify an external Hive table, hello data for hello external Hive table can be stored in Azure blob storage with hello following name 000000_0.</span></span> <span data-ttu-id="e8ade-227">Egy kísérletben tooread hello Adatolvasó modulja több fájlt használja, és előrejelzéseket használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="e8ade-227">You can use hello reader module in an experiment tooread multiple files, and use them for predictions.</span></span>

<span data-ttu-id="e8ade-228">Az Azure Machine Learning kísérletben hello Adatolvasó modulja használatakor a Azure Blob bemenetként is megadhat.</span><span class="sxs-lookup"><span data-stu-id="e8ade-228">When using hello reader module in an Azure Machine Learning experiment, you can specify Azure Blob as an input.</span></span> <span data-ttu-id="e8ade-229">az Azure blob storage hello hello fájlok lehetnek a hello kimeneti fájlok (Példa: 000000_0), amely a HDInsight-on futó Pig és a Hive parancsfájlok hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="e8ade-229">hello files in hello Azure blob storage can be hello output files (Example: 000000_0) that are produced by a Pig and Hive script running on HDInsight.</span></span> <span data-ttu-id="e8ade-230">hello olvasó modul lehetővé teszi tooread fájlok (nincs) hello konfigurálásával **elérési toocontainer, könyvtár vagy blob**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-230">hello reader module allows you tooread files (with no extensions) by configuring hello **Path toocontainer, directory/blob**.</span></span> <span data-ttu-id="e8ade-231">Hello **elérési toocontainer** pontok toohello tároló és **könyvtár/blob** toofolder, ahogy az a következő kép hello hello fájlokat tartalmazó mutat.</span><span class="sxs-lookup"><span data-stu-id="e8ade-231">hello **Path toocontainer** points toohello container and **directory/blob** points toofolder that contains hello files as shown in hello following image.</span></span> <span data-ttu-id="e8ade-232">hello csillag, ez azt jelenti, hogy \*) **határozza meg, hogy az összes hello hello tároló/mappában található fájlokat (Ez azt jelenti, hogy adatokat/aggregateddata/év 2014/hónap-6 = /\*)** hello kísérlet részeként olvasható.</span><span class="sxs-lookup"><span data-stu-id="e8ade-232">hello asterisk that is, \*) **specifies that all hello files in hello container/folder (that is, data/aggregateddata/year=2014/month-6/\*)** are read as part of hello experiment.</span></span>

![Az Azure Blob tulajdonságai](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a><span data-ttu-id="e8ade-234">Példa</span><span class="sxs-lookup"><span data-stu-id="e8ade-234">Example</span></span>
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a><span data-ttu-id="e8ade-235">A webszolgáltatás-paramétereket AzureMLBatchExecution tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="e8ade-235">Pipeline with AzureMLBatchExecution activity with Web Service Parameters</span></span>

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

<span data-ttu-id="e8ade-236">A fenti JSON példa hello:</span><span class="sxs-lookup"><span data-stu-id="e8ade-236">In hello above JSON example:</span></span>

* <span data-ttu-id="e8ade-237">hello telepített Azure Machine Learning Web szolgáltatás használja az olvasót, és egy író modul tooread/adatok írása az / tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e8ade-237">hello deployed Azure Machine Learning Web service uses a reader and a writer module tooread/write data from/tooan Azure SQL Database.</span></span> <span data-ttu-id="e8ade-238">Ez a webszolgáltatás mutatja meg a következő négy paraméterek hello: adatbázis-kiszolgáló neve, a adatbázis neve, a kiszolgáló felhasználói fiók nevét és a kiszolgáló felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e8ade-238">This Web service exposes hello following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>  
* <span data-ttu-id="e8ade-239">Mindkét **start** és **end** időpontok szerepelnie kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="e8ade-239">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="e8ade-240">Például: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="e8ade-240">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="e8ade-241">Hello **end** idő megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e8ade-241">hello **end** time is optional.</span></span> <span data-ttu-id="e8ade-242">Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra.**"</span><span class="sxs-lookup"><span data-stu-id="e8ade-242">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="e8ade-243">toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8ade-243">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span> <span data-ttu-id="e8ade-244">A JSON-tulajdonságokkal kapcsolatos információkért lásd: [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referencia a JSON-parancsprogramokhoz).</span><span class="sxs-lookup"><span data-stu-id="e8ade-244">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

### <a name="other-scenarios"></a><span data-ttu-id="e8ade-245">Egyéb forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="e8ade-245">Other scenarios</span></span>
#### <a name="web-service-requires-multiple-inputs"></a><span data-ttu-id="e8ade-246">Webszolgáltatás több bemeneti értéket igényel</span><span class="sxs-lookup"><span data-stu-id="e8ade-246">Web service requires multiple inputs</span></span>
<span data-ttu-id="e8ade-247">Ha hello webszolgáltatás több bemeneti vesz igénybe, használja a hello **webServiceInputs** tulajdonság használata helyett **típus**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-247">If hello web service takes multiple inputs, use hello **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="e8ade-248">Hello által hivatkozott adatkészletek **webServiceInputs** is szerepelnie kell hello tevékenység **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-248">Datasets that are referenced by hello **webServiceInputs** must also be included in hello Activity **inputs**.</span></span>

<span data-ttu-id="e8ade-249">Az Azure ML kísérletben webszolgáltatás bemenetét és a kimeneti portok és a globális paraméterek alapértelmezett neve lehet ("input1", "input2"), amely testre szabható.</span><span class="sxs-lookup"><span data-stu-id="e8ade-249">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="e8ade-250">webServiceInputs, webServiceOutputs és globalParameters beállítások használt hello neveket hello kísérletekben hello neveket pontosan egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="e8ade-250">hello names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match hello names in hello experiments.</span></span> <span data-ttu-id="e8ade-251">Az Azure ML végpont tooverify várt hello leképezés hello minta-kérések forgalma hello kötegelt végrehajtási súgó lapon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e8ade-251">You can view hello sample request payload on hello Batch Execution Help page for your Azure ML endpoint tooverify hello expected mapping.</span></span>

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a><span data-ttu-id="e8ade-252">Webszolgáltatás nem szükséges bemeneti</span><span class="sxs-lookup"><span data-stu-id="e8ade-252">Web Service does not require an input</span></span>
<span data-ttu-id="e8ade-253">Az Azure ML kötegelt végrehajtási webszolgáltatások lehet használt toorun munkafolyamatokat, például R vagy Python parancsfájlok, nem követelheti meg, amely a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="e8ade-253">Azure ML batch execution web services can be used toorun any workflows, for example R or Python scripts, that may not require any inputs.</span></span> <span data-ttu-id="e8ade-254">Vagy hello kísérlet az olvasó modul, amely nem fedi fel minden GlobalParameters is megadhatók.</span><span class="sxs-lookup"><span data-stu-id="e8ade-254">Or, hello experiment might be configured with a Reader module that does not expose any GlobalParameters.</span></span> <span data-ttu-id="e8ade-255">Ebben az esetben hello AzureMLBatchExecution tevékenység úgy lesz beállítva, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e8ade-255">In that case, hello AzureMLBatchExecution Activity would be configured as follows:</span></span>

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a><span data-ttu-id="e8ade-256">Webszolgáltatás nem szükséges egy bemeneti/kimeneti</span><span class="sxs-lookup"><span data-stu-id="e8ade-256">Web Service does not require an input/output</span></span>
<span data-ttu-id="e8ade-257">Azure ML kötegelt végrehajtási webszolgáltatás hello esetleg nincs konfigurálva webszolgáltatás kimenetet.</span><span class="sxs-lookup"><span data-stu-id="e8ade-257">hello Azure ML batch execution web service might not have any Web Service output configured.</span></span> <span data-ttu-id="e8ade-258">Ebben a példában nincs webszolgáltatás bemeneti vagy kimeneti, sem bármely GlobalParameters vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e8ade-258">In this example, there is no Web Service input or output, nor are any GlobalParameters configured.</span></span> <span data-ttu-id="e8ade-259">Továbbra is maga hello tevékenység konfigurált kimenettel, de ez nem egy webServiceOutput van megadva.</span><span class="sxs-lookup"><span data-stu-id="e8ade-259">There is still an output configured on hello activity itself, but it is not given as a webServiceOutput.</span></span>

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-hello-activity-runs-only-when-other-activities-have-succeeded"></a><span data-ttu-id="e8ade-260">Webes szolgáltatás által használt olvasók és írók és hello tevékenység fut csak akkor, ha más tevékenységek sikeres volt</span><span class="sxs-lookup"><span data-stu-id="e8ade-260">Web Service uses readers and writers, and hello activity runs only when other activities have succeeded</span></span>
<span data-ttu-id="e8ade-261">hello Azure ML web service írási és olvasási szerepkörökhöz modulok vagy bármely GlobalParameters nélkül konfigurált toorun lehet.</span><span class="sxs-lookup"><span data-stu-id="e8ade-261">hello Azure ML web service reader and writer modules might be configured toorun with or without any GlobalParameters.</span></span> <span data-ttu-id="e8ade-262">Azonban érdemes lehet Szolgáltatáshívások tooembed egy folyamatot, amely a dataset függőségek tooinvoke hello szolgáltatást használ, csak akkor, ha egy fölérendelt feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="e8ade-262">However, you may want tooembed service calls in a pipeline that uses dataset dependencies tooinvoke hello service only when some upstream processing has completed.</span></span> <span data-ttu-id="e8ade-263">Más műveleteket is el lehet indítani, hello kötegelt végrehajtáshoz a ezzel a megközelítéssel befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="e8ade-263">You can also trigger some other action after hello batch execution has completed using this approach.</span></span> <span data-ttu-id="e8ade-264">Ebben az esetben is express hello függőségek tevékenység bemenetekhez és kimenetekhez, használja valamelyiket, a webszolgáltatás bemeneti vagy kimeneti megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e8ade-264">In that case, you can express hello dependencies using activity inputs and outputs, without naming any of them as Web Service inputs or outputs.</span></span>

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

<span data-ttu-id="e8ade-265">Hello **takeaways** vannak:</span><span class="sxs-lookup"><span data-stu-id="e8ade-265">hello **takeaways** are:</span></span>

* <span data-ttu-id="e8ade-266">Ha a kísérlet végpont egy típus használ: egy blob-adathalmazra képviseli, és a hello tevékenység bemenetei és hello típus tulajdonság tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e8ade-266">If your experiment endpoint uses a webServiceInput: it is represented by a blob dataset and is included in hello activity inputs and hello webServiceInput property.</span></span> <span data-ttu-id="e8ade-267">Ellenkező esetben hello típus tulajdonság nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="e8ade-267">Otherwise, hello webServiceInput property is omitted.</span></span>
* <span data-ttu-id="e8ade-268">Ha a kísérlet végpont használ webServiceOutput(s): blob adatkészletek képviseli, és tartalmazza a hello tevékenység kimeneteiből és hello webServiceOutputs tulajdonságában.</span><span class="sxs-lookup"><span data-stu-id="e8ade-268">If your experiment endpoint uses webServiceOutput(s): they are represented by blob datasets and are included in hello activity outputs and in hello webServiceOutputs property.</span></span> <span data-ttu-id="e8ade-269">hello tevékenység kimenete és webServiceOutputs minden kimenetet hello kísérletben hello név szerint vannak leképezve.</span><span class="sxs-lookup"><span data-stu-id="e8ade-269">hello activity outputs and webServiceOutputs are mapped by hello name of each output in hello experiment.</span></span> <span data-ttu-id="e8ade-270">Ellenkező esetben hello webServiceOutputs tulajdonság nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="e8ade-270">Otherwise, hello webServiceOutputs property is omitted.</span></span>
* <span data-ttu-id="e8ade-271">Ha a kísérlet végpont globalParameter(s) azt mutatja, a számukra hello tevékenység globalParameters tulajdonságában kulcs, érték párként.</span><span class="sxs-lookup"><span data-stu-id="e8ade-271">If your experiment endpoint exposes globalParameter(s), they are given in hello activity globalParameters property as key, value pairs.</span></span> <span data-ttu-id="e8ade-272">Ellenkező esetben hello globalParameters tulajdonság nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="e8ade-272">Otherwise, hello globalParameters property is omitted.</span></span> <span data-ttu-id="e8ade-273">hello kulcsai kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="e8ade-273">hello keys are case-sensitive.</span></span> <span data-ttu-id="e8ade-274">[Az Azure Data Factory funkciók](data-factory-functions-variables.md) hello értékek használható.</span><span class="sxs-lookup"><span data-stu-id="e8ade-274">[Azure Data Factory functions](data-factory-functions-variables.md) may be used in hello values.</span></span>
* <span data-ttu-id="e8ade-275">További adathalmazokat szerepelni fog hello tevékenység bemenetekhez és kimenetekhez tulajdonságait, a hello tevékenység typeProperties rendelés nélkül.</span><span class="sxs-lookup"><span data-stu-id="e8ade-275">Additional datasets may be included in hello Activity inputs and outputs properties, without being referenced in hello Activity typeProperties.</span></span> <span data-ttu-id="e8ade-276">Ezek az adatkészletek szelet függőségek végrehajtását szabályozására, de hello AzureMLBatchExecution tevékenység ellenkező esetben figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="e8ade-276">These datasets govern execution using slice dependencies but are otherwise ignored by hello AzureMLBatchExecution Activity.</span></span>


## <a name="updating-models-using-update-resource-activity"></a><span data-ttu-id="e8ade-277">A modellek használata az Update-Erőforrástevékenység frissítése</span><span class="sxs-lookup"><span data-stu-id="e8ade-277">Updating models using Update Resource Activity</span></span>
<span data-ttu-id="e8ade-278">Után átképezési befejezése, frissítse a webszolgáltatás (prediktív kísérletté webszolgáltatásként kitett) pontozási hello újonnan betanított modell hello segítségével hello **Azure ML Update Erőforrástevékenység**.</span><span class="sxs-lookup"><span data-stu-id="e8ade-278">After you are done with retraining, update hello scoring web service (predictive experiment exposed as a web service) with hello newly trained model by using hello **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="e8ade-279">Lásd: [modellek használata az Update-Erőforrástevékenység frissítése](data-factory-azure-ml-update-resource-activity.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="e8ade-279">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

### <a name="reader-and-writer-modules"></a><span data-ttu-id="e8ade-280">Olvasási és írási modulok</span><span class="sxs-lookup"><span data-stu-id="e8ade-280">Reader and Writer Modules</span></span>
<span data-ttu-id="e8ade-281">Egy általános forgatókönyv webszolgáltatás-paramétereket az Azure SQL-olvasók és írók hello használata.</span><span class="sxs-lookup"><span data-stu-id="e8ade-281">A common scenario for using Web service parameters is hello use of Azure SQL Readers and Writers.</span></span> <span data-ttu-id="e8ade-282">hello olvasó modul az Azure Machine Learning Studio kívül adatszolgáltatásokból felügyeleti kísérlet használt tooload adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8ade-282">hello reader module is used tooload data into an experiment from data management services outside Azure Machine Learning Studio.</span></span> <span data-ttu-id="e8ade-283">hello író modul az adatok felügyeleti szolgáltatás Azure Machine Learning Studio kívül a kísérletekből toosave adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8ade-283">hello writer module is toosave data from your experiments into data management services outside Azure Machine Learning Studio.</span></span>  

<span data-ttu-id="e8ade-284">Azure Blob vagy az Azure SQL olvasási/írási kapcsolatos részletekért lásd: [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) témakörök MSDN könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="e8ade-284">For details about Azure Blob/Azure SQL reader/writer, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span> <span data-ttu-id="e8ade-285">hello példa az előző szakaszban hello használt hello Azure Blob és Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="e8ade-285">hello example in hello previous section used hello Azure Blob reader and Azure Blob writer.</span></span> <span data-ttu-id="e8ade-286">Ez a szakasz ismerteti az Azure SQL-olvasó és az Azure SQL-író segítségével.</span><span class="sxs-lookup"><span data-stu-id="e8ade-286">This section discusses using Azure SQL reader and Azure SQL writer.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="e8ade-287">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="e8ade-287">Frequently asked questions</span></span>
<span data-ttu-id="e8ade-288">**K:** a big Data típusú adatok folyamatok által létrehozott több fájl van.</span><span class="sxs-lookup"><span data-stu-id="e8ade-288">**Q:** I have multiple files that are generated by my big data pipelines.</span></span> <span data-ttu-id="e8ade-289">Használható összes hello fájljának hello AzureMLBatchExecution tevékenység toowork?</span><span class="sxs-lookup"><span data-stu-id="e8ade-289">Can I use hello AzureMLBatchExecution Activity toowork on all hello files?</span></span>

<span data-ttu-id="e8ade-290">**V:** Igen.</span><span class="sxs-lookup"><span data-stu-id="e8ade-290">**A:** Yes.</span></span> <span data-ttu-id="e8ade-291">Lásd: hello **használatával egy olvasó modul tooread adatok Azure BLOB több fájlból** című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="e8ade-291">See hello **Using a Reader module tooread data from multiple files in Azure Blob** section for details.</span></span>

## <a name="azure-ml-batch-scoring-activity"></a><span data-ttu-id="e8ade-292">Azure ML kötegelt pontozási tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-292">Azure ML Batch Scoring Activity</span></span>
<span data-ttu-id="e8ade-293">Hello használata **AzureMLBatchScoring** tevékenység toointegrate Azure Machine Learning segítségével, azt javasoljuk, hogy használjon hello legújabb **AzureMLBatchExecution** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e8ade-293">If you are using hello **AzureMLBatchScoring** activity toointegrate with Azure Machine Learning, we recommend that you use hello latest **AzureMLBatchExecution** activity.</span></span>

<span data-ttu-id="e8ade-294">hello AzureMLBatchExecution tevékenység hello Azure SDK és Azure PowerShell 2015. augusztus kiadása be.</span><span class="sxs-lookup"><span data-stu-id="e8ade-294">hello AzureMLBatchExecution activity is introduced in hello August 2015 release of Azure SDK and Azure PowerShell.</span></span>

<span data-ttu-id="e8ade-295">Ha azt szeretné, hogy az hello AzureMLBatchScoring tevékenység toocontinue, továbbra is egész ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="e8ade-295">If you want toocontinue using hello AzureMLBatchScoring activity, continue reading through this section.</span></span>  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a><span data-ttu-id="e8ade-296">Bemeneti/kimeneti az Azure Storage használata az Azure ML kötegelt pontozási tevékenység</span><span class="sxs-lookup"><span data-stu-id="e8ade-296">Azure ML Batch Scoring activity using Azure Storage for input/output</span></span>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a><span data-ttu-id="e8ade-297">Webszolgáltatás-paramétereket</span><span class="sxs-lookup"><span data-stu-id="e8ade-297">Web Service Parameters</span></span>
<span data-ttu-id="e8ade-298">Webszolgáltatás-paramétereket, toospecify értékeinek hozzáadása egy **typeProperties** szakasz toohello **AzureMLBatchScoringActivty** hello adatcsatorna JSON, ahogy az alábbi példa hello szakaszában:</span><span class="sxs-lookup"><span data-stu-id="e8ade-298">toospecify values for Web service parameters, add a **typeProperties** section toohello **AzureMLBatchScoringActivty** section in hello pipeline JSON as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
<span data-ttu-id="e8ade-299">Is [Data Factory funkciók](data-factory-functions-variables.md) a sikeres hello értékeinek webszolgáltatás-paramétereket a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="e8ade-299">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for hello Web service parameters as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="e8ade-300">Webszolgáltatás-paramétereket hello kis-és nagybetűket, ezért figyeljen oda arra, hogy hello hello tevékenységben megadott JSON megfelel hello hello webszolgáltatás által elérhetővé tett néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="e8ade-300">hello Web service parameters are case-sensitive, so ensure that hello names you specify in hello activity JSON match hello ones exposed by hello Web service.</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="e8ade-301">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e8ade-301">See Also</span></span>
* [<span data-ttu-id="e8ade-302">Az Azure blogbejegyzést: Ismerkedés az Azure Data Factory és az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="e8ade-302">Azure blog post: Getting started with Azure Data Factory and Azure Machine Learning</span></span>](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
