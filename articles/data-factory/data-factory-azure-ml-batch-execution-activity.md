---
title: "Hozzon létre Azure Data Factory használatával prediktív adatok folyamatok |} Microsoft Docs"
description: "Ismerteti, hogyan hozzon létre Azure Data Factory és az Azure Machine Learning a prediktív folyamatok létrehozása"
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
ms.openlocfilehash: d8e2c9583fc909e4e015e2d40473d2754529d8ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a><span data-ttu-id="f649b-103">Hozzon létre prediktív folyamatok Azure Machine Learning és az Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="f649b-103">Create predictive pipelines using Azure Machine Learning and Azure Data Factory</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="f649b-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="f649b-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="f649b-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="f649b-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="f649b-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="f649b-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="f649b-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="f649b-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="f649b-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="f649b-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="f649b-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="f649b-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="f649b-114">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f649b-114">Introduction</span></span>

### <a name="azure-machine-learning"></a><span data-ttu-id="f649b-115">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f649b-115">Azure Machine Learning</span></span>
<span data-ttu-id="f649b-116">[Az Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) lehetővé teszi létrehozása, tesztelése és telepítése a prediktív elemzési megoldások.</span><span class="sxs-lookup"><span data-stu-id="f649b-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) enables you to build, test, and deploy predictive analytics solutions.</span></span> <span data-ttu-id="f649b-117">Magas szintű szempontból elkészült a három lépést:</span><span class="sxs-lookup"><span data-stu-id="f649b-117">From a high-level point of view, it is done in three steps:</span></span>

1. <span data-ttu-id="f649b-118">**Hozzon létre egy tanítási kísérletet**.</span><span class="sxs-lookup"><span data-stu-id="f649b-118">**Create a training experiment**.</span></span> <span data-ttu-id="f649b-119">Ebben a lépésben az Azure ML Studio úgy teheti meg.</span><span class="sxs-lookup"><span data-stu-id="f649b-119">You do this step by using the Azure ML Studio.</span></span> <span data-ttu-id="f649b-120">Az ML studio olyan együttműködési Látványelem-fejlesztési környezet, amelyekkel betanítása és tesztelése egy prediktív elemzési modell betanítási adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="f649b-120">The ML studio is a collaborative visual development environment that you use to train and test a predictive analytics model using training data.</span></span>
2. <span data-ttu-id="f649b-121">**Alakítsa át egy prediktív kísérletté**.</span><span class="sxs-lookup"><span data-stu-id="f649b-121">**Convert it to a predictive experiment**.</span></span> <span data-ttu-id="f649b-122">Miután a modell még betanítva meglévő adatokkal, és készen áll az új adatok pontozása céljából, előkészítése, és a kísérlet pontozó egyszerűsítésére.</span><span class="sxs-lookup"><span data-stu-id="f649b-122">Once your model has been trained with existing data and you are ready to use it to score new data, you prepare and streamline your experiment for scoring.</span></span>
3. <span data-ttu-id="f649b-123">**A webszolgáltatás üzembe**.</span><span class="sxs-lookup"><span data-stu-id="f649b-123">**Deploy it as a web service**.</span></span> <span data-ttu-id="f649b-124">A pontozási kísérlet közzéteheti Azure webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="f649b-124">You can publish your scoring experiment as an Azure web service.</span></span> <span data-ttu-id="f649b-125">Adatokat küldeni a modell a webes szolgáltatás végpontját keresztül, és fogadni eredmény előrejelzéseket eredete a modell.</span><span class="sxs-lookup"><span data-stu-id="f649b-125">You can send data to your model via this web service end point and receive result predictions fro the model.</span></span>  

### <a name="azure-data-factory"></a><span data-ttu-id="f649b-126">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f649b-126">Azure Data Factory</span></span>
<span data-ttu-id="f649b-127">A Data Factory egy felhőalapú adatintegrációs szolgáltatás, amellyel előkészíthető és automatizálható az adatok **továbbítása** és **átalakítása**.</span><span class="sxs-lookup"><span data-stu-id="f649b-127">Data Factory is a cloud-based data integration service that orchestrates and automates the **movement** and **transformation** of data.</span></span> <span data-ttu-id="f649b-128">Adatok integrációs megoldásokat Azure Data Factory használatával különböző adattárolókhoz származó adatok, átalakító/folyamat az adatokat, és az eredmény adatokat közzé az adattároló hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="f649b-128">You can create data integration solutions using Azure Data Factory that can ingest data from various data stores, transform/process the data, and publish the result data to the data stores.</span></span>

<span data-ttu-id="f649b-129">A Data Factoryval az adatok továbbítására és átalakítására szolgáló adatfolyamatokat hozhat létre, majd ütemezés szerint futtathatja a folyamatot (óránként, napi, heti stb. gyakorisággal).</span><span class="sxs-lookup"><span data-stu-id="f649b-129">Data Factory service allows you to create data pipelines that move and transform data, and then run the pipelines on a specified schedule (hourly, daily, weekly, etc.).</span></span> <span data-ttu-id="f649b-130">Ezenkívül látványos vizualizációkkal jelenítheti meg az adatfolyamatok közötti leszármaztatási és függőségi kapcsolatokat, valamint egyetlen, egységesített nézetben figyelheti az összes folyamatot, így egyszerűen kiszűrheti a problémákat és beállíthatja a figyelési riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="f649b-130">It also provides rich visualizations to display the lineage and dependencies between your data pipelines, and monitor all your data pipelines from a single unified view to easily pinpoint issues and setup monitoring alerts.</span></span>

<span data-ttu-id="f649b-131">Lásd: [Bevezetés az Azure Data Factory](data-factory-introduction.md) és [felépítheti első folyamatát](data-factory-build-your-first-pipeline.md) cikkeket, ha gyorsan az Azure Data Factory szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f649b-131">See [Introduction to Azure Data Factory](data-factory-introduction.md) and [Build your first pipeline](data-factory-build-your-first-pipeline.md) articles to quickly get started with the Azure Data Factory service.</span></span>

### <a name="data-factory-and-machine-learning-together"></a><span data-ttu-id="f649b-132">Adat-előállító és a gépi tanulás együtt</span><span class="sxs-lookup"><span data-stu-id="f649b-132">Data Factory and Machine Learning together</span></span>
<span data-ttu-id="f649b-133">Az Azure Data Factory lehetővé teszi, hogy könnyen létrehozhat egy közzétett használó folyamatok [Azure Machine Learning] [ azure-machine-learning] webszolgáltatás prediktív elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="f649b-133">Azure Data Factory enables you to easily create pipelines that use a published [Azure Machine Learning][azure-machine-learning] web service for predictive analytics.</span></span> <span data-ttu-id="f649b-134">Használja a **kötegelt végrehajtási tevékenység** egy Azure Data Factory-folyamat a hívhat meg az Azure gépi tanulás webszolgáltatás számára a előrejelzéseket készítsen a kötegben lévő adatokat.</span><span class="sxs-lookup"><span data-stu-id="f649b-134">Using the **Batch Execution Activity** in an Azure Data Factory pipeline, you can invoke an Azure ML web service to make predictions on the data in batch.</span></span> <span data-ttu-id="f649b-135">Lásd: [meghívása az Azure gépi tanulás webszolgáltatás a kötegelt végrehajtási tevékenység](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="f649b-135">See [Invoking an Azure ML web service using the Batch Execution Activity](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) section for details.</span></span>

<span data-ttu-id="f649b-136">Az Azure ml kísérletek pontozási prediktív modelleket idővel kell kell retrained új bemeneti adatkészletek használata.</span><span class="sxs-lookup"><span data-stu-id="f649b-136">Over time, the predictive models in the Azure ML scoring experiments need to be retrained using new input datasets.</span></span> <span data-ttu-id="f649b-137">A Data Factory-folyamat az Azure ML modellje is működik, az alábbi lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="f649b-137">You can retrain an Azure ML model from a Data Factory pipeline by doing the following steps:</span></span>

1. <span data-ttu-id="f649b-138">Tegye közzé a tanítási kísérletet (nem prediktív kísérletté) webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="f649b-138">Publish the training experiment (not predictive experiment) as a web service.</span></span> <span data-ttu-id="f649b-139">Az előző példában a webszolgáltatásként teszi közzé a prediktív kísérletté hasonló módon teheti meg ebben a lépésben az Azure ML Studio.</span><span class="sxs-lookup"><span data-stu-id="f649b-139">You do this step in the Azure ML Studio as you did to expose predictive experiment as a web service in the previous scenario.</span></span>
2. <span data-ttu-id="f649b-140">Az Azure ML kötegelt végrehajtási tevékenység segítségével meghívni a webszolgáltatás a tanítási kísérletet.</span><span class="sxs-lookup"><span data-stu-id="f649b-140">Use the Azure ML Batch Execution Activity to invoke the web service for the training experiment.</span></span> <span data-ttu-id="f649b-141">Alapvetően használhatja az Azure ML kötegelt végrehajtási tevékenység képzési webszolgáltatás és a pontozási webszolgáltatás meghívására.</span><span class="sxs-lookup"><span data-stu-id="f649b-141">Basically, you can use the Azure ML Batch Execution activity to invoke both training web service and scoring web service.</span></span>

<span data-ttu-id="f649b-142">Miután elkészült, az átképezési, frissítése a pontozási webszolgáltatás (prediktív kísérletté webszolgáltatásként kitett) újonnan betanított modell használatával a **Azure ML Update Erőforrástevékenység**.</span><span class="sxs-lookup"><span data-stu-id="f649b-142">After you are done with retraining, update the scoring web service (predictive experiment exposed as a web service) with the newly trained model by using the **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="f649b-143">Lásd: [modellek használata az Update-Erőforrástevékenység frissítése](data-factory-azure-ml-update-resource-activity.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="f649b-143">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

## <a name="invoking-a-web-service-using-batch-execution-activity"></a><span data-ttu-id="f649b-144">Meghívja a webszolgáltatást kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-144">Invoking a web service using Batch Execution Activity</span></span>
<span data-ttu-id="f649b-145">Azure Data Factory használatával levezényelni adatmozgatást és a feldolgozás, és hajtsa végre az Azure Machine Learning segítségével kötegelt végrehajtáshoz.</span><span class="sxs-lookup"><span data-stu-id="f649b-145">You use Azure Data Factory to orchestrate data movement and processing, and then perform batch execution using Azure Machine Learning.</span></span> <span data-ttu-id="f649b-146">A legfelső szintű lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="f649b-146">Here are the top-level steps:</span></span>

1. <span data-ttu-id="f649b-147">Hozzon létre egy Azure Machine Learning társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f649b-147">Create an Azure Machine Learning linked service.</span></span> <span data-ttu-id="f649b-148">A következő értékek lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="f649b-148">You need the following values:</span></span>

   1. <span data-ttu-id="f649b-149">**A kérelmi URI** a kötegelt végrehajtás API számára.</span><span class="sxs-lookup"><span data-stu-id="f649b-149">**Request URI** for the Batch Execution API.</span></span> <span data-ttu-id="f649b-150">A kérelem URI-CÍMÉN található kattintva a **KÖTEGELT végrehajtási** hivatkozás a szolgáltatások weblapon.</span><span class="sxs-lookup"><span data-stu-id="f649b-150">You can find the Request URI by clicking the **BATCH EXECUTION** link in the web services page.</span></span>
   2. <span data-ttu-id="f649b-151">**Az API-kulcs** a közzétett Azure Machine Learning webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f649b-151">**API key** for the published Azure Machine Learning web service.</span></span> <span data-ttu-id="f649b-152">Az API-kulcsot a webszolgáltatás, amelyet a közzétett kattintva találja.</span><span class="sxs-lookup"><span data-stu-id="f649b-152">You can find the API key by clicking the web service that you have published.</span></span>
   3. <span data-ttu-id="f649b-153">Használja a **AzureMLBatchExecution** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f649b-153">Use the **AzureMLBatchExecution** activity.</span></span>

      ![Machine Learning irányítópult](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Kötegelt URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a><span data-ttu-id="f649b-156">Forgatókönyv: Kísérletek Web service bemenetek/kimenetek adataira az Azure Blob Storage használata</span><span class="sxs-lookup"><span data-stu-id="f649b-156">Scenario: Experiments using Web service inputs/outputs that refer to data in Azure Blob Storage</span></span>
<span data-ttu-id="f649b-157">Ebben a forgatókönyvben az Azure Machine Learning Web service lehetővé teszi az adatok segítségével az Azure blob Storage-fájlból való előrejelzéseket és az előrejelzés eredményt a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f649b-157">In this scenario, the Azure Machine Learning Web service makes predictions using data from a file in an Azure blob storage and stores the prediction results in the blob storage.</span></span> <span data-ttu-id="f649b-158">A következő JSON határozza meg a Data Factory-folyamathoz egy AzureMLBatchExecution tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="f649b-158">The following JSON defines a Data Factory pipeline with an AzureMLBatchExecution activity.</span></span> <span data-ttu-id="f649b-159">A tevékenység rendelkezik az adatkészlet **DecisionTreeInputBlob** bemenetként és **DecisionTreeResultBlob** kimeneteként.</span><span class="sxs-lookup"><span data-stu-id="f649b-159">The activity has the dataset **DecisionTreeInputBlob** as input and **DecisionTreeResultBlob** as the output.</span></span> <span data-ttu-id="f649b-160">A **DecisionTreeInputBlob** átadása a webszolgáltatás által bemeneteként használja a **típus** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f649b-160">The **DecisionTreeInputBlob** is passed as an input to the web service by using the **webServiceInput** JSON property.</span></span> <span data-ttu-id="f649b-161">A **DecisionTreeResultBlob** objektumnak átadott kimenetként a webszolgáltatás által használ a **webServiceOutputs** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f649b-161">The **DecisionTreeResultBlob** is passed as an output to the Web service by using the **webServiceOutputs** JSON property.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="f649b-162">Ha a webszolgáltatás több bemeneti vesz igénybe, használja a **webServiceInputs** tulajdonság használata helyett **típus**.</span><span class="sxs-lookup"><span data-stu-id="f649b-162">If the web service takes multiple inputs, use the **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="f649b-163">Tekintse meg a [webszolgáltatás több bemeneti értéket igényel](#web-service-requires-multiple-inputs) szakasz egy példát a webServiceInputs tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="f649b-163">See the [Web service requires multiple inputs](#web-service-requires-multiple-inputs) section for an example of using the webServiceInputs property.</span></span>
>
> <span data-ttu-id="f649b-164">Által hivatkozott adatkészletek a **típus**/**webServiceInputs** és **webServiceOutputs** tulajdonságok (a  **typeProperties**) is szerepelnie kell a tevékenység **bemenetek** és **kimenete**.</span><span class="sxs-lookup"><span data-stu-id="f649b-164">Datasets that are referenced by the **webServiceInput**/**webServiceInputs** and **webServiceOutputs** properties (in **typeProperties**) must also be included in the Activity **inputs** and **outputs**.</span></span>
>
> <span data-ttu-id="f649b-165">Az Azure ML kísérletben webszolgáltatás bemenetét és a kimeneti portok és a globális paraméterek alapértelmezett neve lehet ("input1", "input2"), amely testre szabható.</span><span class="sxs-lookup"><span data-stu-id="f649b-165">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="f649b-166">WebServiceInputs, webServiceOutputs és globalParameters beállítások használt neveket pontosan egyeznie kell a kísérleti nevét.</span><span class="sxs-lookup"><span data-stu-id="f649b-166">The names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match the names in the experiments.</span></span> <span data-ttu-id="f649b-167">A minta-kérések forgalma a a várt leképezés ellenőrzése az Azure ML végpont kötegelt végrehajtási súgó lapon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="f649b-167">You can view the sample request payload on the Batch Execution Help page for your Azure ML endpoint to verify the expected mapping.</span></span>
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
> <span data-ttu-id="f649b-168">Csak be- és kimenetekkel a AzureMLBatchExecution tevékenység argumentumként átadhatók paraméterek a webszolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="f649b-168">Only inputs and outputs of the AzureMLBatchExecution activity can be passed as parameters to the Web service.</span></span> <span data-ttu-id="f649b-169">Például a fenti JSON-részlet DecisionTreeInputBlob a AzureMLBatchExecution tevékenység, amelyet a rendszer továbbad a webszolgáltatás bemeneteként típus paraméteren keresztül bemenete.</span><span class="sxs-lookup"><span data-stu-id="f649b-169">For example, in the above JSON snippet, DecisionTreeInputBlob is an input to the AzureMLBatchExecution activity, which is passed as an input to the Web service via webServiceInput parameter.</span></span>   
>
>

### <a name="example"></a><span data-ttu-id="f649b-170">Példa</span><span class="sxs-lookup"><span data-stu-id="f649b-170">Example</span></span>
<span data-ttu-id="f649b-171">Ebben a példában a bemeneti és kimeneti adatok tárolásához Azure tárolást használ.</span><span class="sxs-lookup"><span data-stu-id="f649b-171">This example uses Azure Storage to hold both the input and output data.</span></span>

<span data-ttu-id="f649b-172">Javasoljuk, hogy olvassa végig a [felépítheti első folyamatát Data Factory] [ adf-build-1st-pipeline] oktatóanyag, mielőtt továbblépne ebben a példában keresztül.</span><span class="sxs-lookup"><span data-stu-id="f649b-172">We recommend that you go through the [Build your first pipeline with Data Factory][adf-build-1st-pipeline] tutorial before going through this example.</span></span> <span data-ttu-id="f649b-173">A Data Factory Editor használja ebben a példában az adat-előállító összetevők (társított szolgáltatások, adatkészleteket, adatcsatorna) létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f649b-173">Use the Data Factory Editor to create Data Factory artifacts (linked services, datasets, pipeline) in this example.</span></span>   

1. <span data-ttu-id="f649b-174">Hozzon létre egy **társított szolgáltatás** a a **Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="f649b-174">Create a **linked service** for your **Azure Storage**.</span></span> <span data-ttu-id="f649b-175">Ha a bemeneti és kimeneti fájlok eltérő tárfiókokból, két összekapcsolt szolgáltatások szüksége.</span><span class="sxs-lookup"><span data-stu-id="f649b-175">If the input and output files are in different storage accounts, you need two linked services.</span></span> <span data-ttu-id="f649b-176">Íme egy JSON-példa:</span><span class="sxs-lookup"><span data-stu-id="f649b-176">Here is a JSON example:</span></span>

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
2. <span data-ttu-id="f649b-177">Hozzon létre a **bemeneti** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="f649b-177">Create the **input** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="f649b-178">Néhány egyéb adat-előállító adatkészletek ellentétben ezek az adatkészletek tartalmaznia kell mindkét **folderPath** és **Fájlnév** értékeket.</span><span class="sxs-lookup"><span data-stu-id="f649b-178">Unlike some other Data Factory datasets, these datasets must contain both **folderPath** and **fileName** values.</span></span> <span data-ttu-id="f649b-179">Particionálás segítségével minden egyes kötegelt végrehajtási (minden adatszelet) feldolgozni vagy tud létrehozni egyedi bemeneti és kimeneti fájlok miatt.</span><span class="sxs-lookup"><span data-stu-id="f649b-179">You can use partitioning to cause each batch execution (each data slice) to process or produce unique input and output files.</span></span> <span data-ttu-id="f649b-180">Szükség lehet néhány felsőbb szintű tevékenység bemeneti átalakítása a CSV fájlformátum, és naplózza azt a tárfiókot az egyes szeletek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f649b-180">You may need to include some upstream activity to transform the input into the CSV file format and place it in the storage account for each slice.</span></span> <span data-ttu-id="f649b-181">Ebben az esetben, nem tartalmazhat a **külső** és **externalData** látható a következő példa, és a DecisionTreeInputBlob beállításait egy másik tevékenység kimeneti adatkészlet lenne.</span><span class="sxs-lookup"><span data-stu-id="f649b-181">In that case, you would not include the **external** and **externalData** settings shown in the following example, and your DecisionTreeInputBlob would be the output dataset of a different Activity.</span></span>

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

    <span data-ttu-id="f649b-182">A bemeneti csv-fájlban a oszlop fejlécsor kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f649b-182">Your input csv file must have the column header row.</span></span> <span data-ttu-id="f649b-183">Ha használja a **másolási tevékenység** létrehozása/áthelyezése a fürt megosztott kötetei szolgáltatás a blob-tárolóba, célszerű a fogadó tulajdonság **blobWriterAddHeader** a **igaz**.</span><span class="sxs-lookup"><span data-stu-id="f649b-183">If you are using the **Copy Activity** to create/move the csv into the blob storage, you should set the sink property **blobWriterAddHeader** to **true**.</span></span> <span data-ttu-id="f649b-184">Példa:</span><span class="sxs-lookup"><span data-stu-id="f649b-184">For example:</span></span>

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    <span data-ttu-id="f649b-185">A csv-fájl nem rendelkezik a fejlécsor, jelenhet meg a következő hibával: **hiba a tevékenység: Hiba történt a karakterlánc olvasásakor. Váratlan lexikális elem: StartObject. Elérési út ", 1. sor, 1 elhelyezése**.</span><span class="sxs-lookup"><span data-stu-id="f649b-185">If the csv file does not have the header row, you may see the following error: **Error in Activity: Error reading string. Unexpected token: StartObject. Path '', line 1, position 1**.</span></span>
3. <span data-ttu-id="f649b-186">Hozzon létre a **kimeneti** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="f649b-186">Create the **output** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="f649b-187">A példa particionálás minden szelet végrehajtásra egyedi kimeneti elérési utat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f649b-187">This example uses partitioning to create a unique output path for each slice execution.</span></span> <span data-ttu-id="f649b-188">A particionálás nélkül a tevékenység felülírná a fájlt.</span><span class="sxs-lookup"><span data-stu-id="f649b-188">Without the partitioning, the activity would overwrite the file.</span></span>

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
4. <span data-ttu-id="f649b-189">Hozzon létre egy **társított szolgáltatás** típusú: **AzureMLLinkedService**, az API-kulcsot biztosító, és a modell a kötegelt végrehajtás URL-cím.</span><span class="sxs-lookup"><span data-stu-id="f649b-189">Create a **linked service** of type: **AzureMLLinkedService**, providing the API key and model batch execution URL.</span></span>

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
5. <span data-ttu-id="f649b-190">Végezetül szerzői a folyamat, amely tartalmazza egy **AzureMLBatchExecution** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f649b-190">Finally, author a pipeline containing an **AzureMLBatchExecution** Activity.</span></span> <span data-ttu-id="f649b-191">Futásidőben a folyamat a következő lépéseket végzi el:</span><span class="sxs-lookup"><span data-stu-id="f649b-191">At runtime, pipeline performs the following steps:</span></span>

   1. <span data-ttu-id="f649b-192">A bemeneti fájl helyét lekérése a bemeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="f649b-192">Gets the location of the input file from your input datasets.</span></span>
   2. <span data-ttu-id="f649b-193">Meghívja az Azure Machine Learning kötegelt végrehajtási API</span><span class="sxs-lookup"><span data-stu-id="f649b-193">Invokes the Azure Machine Learning batch execution API</span></span>
   3. <span data-ttu-id="f649b-194">A kötegelt végrehajtás kimenetének másolása a kimeneti adatkészlet megadott blob.</span><span class="sxs-lookup"><span data-stu-id="f649b-194">Copies the batch execution output to the blob given in your output dataset.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f649b-195">AzureMLBatchExecution tevékenység állhat nulla vagy több be- és egy vagy több kimenetekkel.</span><span class="sxs-lookup"><span data-stu-id="f649b-195">AzureMLBatchExecution activity can have zero or more inputs and one or more outputs.</span></span>
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

      <span data-ttu-id="f649b-196">Mindkét **start** és **end** időpontok szerepelnie kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="f649b-196">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="f649b-197">Például: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="f649b-197">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="f649b-198">A **end** idő megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="f649b-198">The **end** time is optional.</span></span> <span data-ttu-id="f649b-199">Ha nem ad meg értéket a **end** tulajdonságot, akkor a program "**kezdés + 48 óra.**"</span><span class="sxs-lookup"><span data-stu-id="f649b-199">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="f649b-200">A folyamat határozatlan ideig történő futtatásához adja meg a **9999-09-09** értéket az **end** (befejezés) tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="f649b-200">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span> <span data-ttu-id="f649b-201">A JSON-tulajdonságokkal kapcsolatos információkért lásd: [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referencia a JSON-parancsprogramokhoz).</span><span class="sxs-lookup"><span data-stu-id="f649b-201">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f649b-202">Adja meg a AzureMLBatchExecution a megadott tevékenység nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="f649b-202">Specifying input for the AzureMLBatchExecution activity is optional.</span></span>
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a><span data-ttu-id="f649b-203">Forgatókönyv: Kísérleteket, tekintse meg a különböző tárolók adatainak olvasási/írási modulok használata</span><span class="sxs-lookup"><span data-stu-id="f649b-203">Scenario: Experiments using Reader/Writer Modules to refer to data in various storages</span></span>
<span data-ttu-id="f649b-204">Azure ML kísérletek létrehozásakor egy másik gyakori forgatókönyv, ha írási és olvasási szerepkörökhöz modulok használják.</span><span class="sxs-lookup"><span data-stu-id="f649b-204">Another common scenario when creating Azure ML experiments is to use Reader and Writer modules.</span></span> <span data-ttu-id="f649b-205">Az olvasó modul használatával adatok betöltése az a kísérlet, és a író modul az adatok mentése a kísérletekből.</span><span class="sxs-lookup"><span data-stu-id="f649b-205">The reader module is used to load data into an experiment and the writer module is to save data from your experiments.</span></span> <span data-ttu-id="f649b-206">Írási és olvasási szerepkörökhöz modullal kapcsolatos részletekért lásd: [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) témakörök MSDN könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="f649b-206">For details about reader and writer modules, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span>     

<span data-ttu-id="f649b-207">Az írási és olvasási szerepkörökhöz modulok használata esetén ajánlott használni egy webszolgáltatási paraméter olvasási/írási modulok mindegyik tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f649b-207">When using the reader and writer modules, it is good practice to use a Web service parameter for each property of these reader/writer modules.</span></span> <span data-ttu-id="f649b-208">Ezek a paraméterek lehetővé teszik a adja meg a beállításokat futásidőben.</span><span class="sxs-lookup"><span data-stu-id="f649b-208">These web parameters enable you to configure the values during runtime.</span></span> <span data-ttu-id="f649b-209">Például egy olvasó modul, amely használja az Azure SQL Database segítségével létrehozhat egy kísérlet: XXX.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="f649b-209">For example, you could create an experiment with a reader module that uses an Azure SQL Database: XXX.database.windows.net.</span></span> <span data-ttu-id="f649b-210">A webszolgáltatás telepítése után később engedélyezni kívánja a fogyasztók webszolgáltatás egy másik Azure SQL Server YYY.database.windows.net nevű megadásához.</span><span class="sxs-lookup"><span data-stu-id="f649b-210">After the web service has been deployed, you want to enable the consumers of the web service to specify another Azure SQL Server called YYY.database.windows.net.</span></span> <span data-ttu-id="f649b-211">A webszolgáltatási paraméter segítségével engedélyezze ezt az értéket be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="f649b-211">You can use a Web service parameter to allow this value to be configured.</span></span>

> [!NOTE]
> <span data-ttu-id="f649b-212">Webes szolgáltatás bemeneti és kimeneti eltérnek a webszolgáltatás-paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f649b-212">Web service input and output are different from Web service parameters.</span></span> <span data-ttu-id="f649b-213">Az első esetben láthatta, hogyan egy bemeneti és kimeneti adható meg az Azure gépi tanulás webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f649b-213">In the first scenario, you have seen how an input and output can be specified for an Azure ML Web service.</span></span> <span data-ttu-id="f649b-214">Ebben a forgatókönyvben a paraméternek az adott webszolgáltatás, amely megfelel az olvasási/írási modulok tulajdonságai át.</span><span class="sxs-lookup"><span data-stu-id="f649b-214">In this scenario, you pass parameters for a Web service that correspond to properties of reader/writer modules.</span></span>
>
>

<span data-ttu-id="f649b-215">Vizsgáljuk meg egy olyan helyzetben használhat webszolgáltatás-paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f649b-215">Let's look at a scenario for using Web service parameters.</span></span> <span data-ttu-id="f649b-216">Egy telepített Azure Machine Learning webszolgáltatás, amelyet használ egy olvasó modul adatokat olvasni az adatforrásokat, Azure Machine Learning által támogatott egyik rendelkezik (például: Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="f649b-216">You have a deployed Azure Machine Learning web service that uses a reader module to read data from one of the data sources supported by Azure Machine Learning (for example: Azure SQL Database).</span></span> <span data-ttu-id="f649b-217">A kötegelt végrehajtás hajtja végre, az eredmények írt író modul (az Azure SQL Database) használatával.</span><span class="sxs-lookup"><span data-stu-id="f649b-217">After the batch execution is performed, the results are written using a Writer module (Azure SQL Database).</span></span>  <span data-ttu-id="f649b-218">Nincs a webszolgáltatás bemenetei és kimenetei a kísérletek vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="f649b-218">No web service inputs and outputs are defined in the experiments.</span></span> <span data-ttu-id="f649b-219">Ebben az esetben ajánlott úgy beállítani, hogy az írási és olvasási szerepkörökhöz modulok vonatkozó webszolgáltatás-paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f649b-219">In this case, we recommend that you configure relevant web service parameters for the reader and writer modules.</span></span> <span data-ttu-id="f649b-220">Ez a konfiguráció lehetővé teszi, hogy az olvasási/írási modulok kell konfigurálni a AzureMLBatchExecution tevékenység használatakor.</span><span class="sxs-lookup"><span data-stu-id="f649b-220">This configuration allows the reader/writer modules to be configured when using the AzureMLBatchExecution activity.</span></span> <span data-ttu-id="f649b-221">Azt adja meg a webszolgáltatás-paramétereket a **globalParameters** a tevékenység JSON szakasz az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="f649b-221">You specify Web service parameters in the **globalParameters** section in the activity JSON as follows.</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

<span data-ttu-id="f649b-222">Is [Data Factory funkciók](data-factory-functions-variables.md) paraméterek szereplő értéket átadja a webes szolgáltatás, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f649b-222">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for the Web service parameters as shown in the following example:</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="f649b-223">A webszolgáltatás-paramétereket kis-és nagybetűket, ezért figyeljen oda arra, hogy a nevét adja meg, ha a tevékenység JSON egyeznek azokkal a webszolgáltatás által elérhetővé tett tárolókra.</span><span class="sxs-lookup"><span data-stu-id="f649b-223">The Web service parameters are case-sensitive, so ensure that the names you specify in the activity JSON match the ones exposed by the Web service.</span></span>
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a><span data-ttu-id="f649b-224">Egy olvasó modul segítségével több fájlból az Azure Blob-adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="f649b-224">Using a Reader module to read data from multiple files in Azure Blob</span></span>
<span data-ttu-id="f649b-225">Big Data típusú adatok folyamatok a tevékenységeket, például a Pig és Hive is létrehozhat egy vagy több kimeneti fájlokat a bővítmények nincsenek.</span><span class="sxs-lookup"><span data-stu-id="f649b-225">Big data pipelines with activities such as Pig and Hive can produce one or more output files with no extensions.</span></span> <span data-ttu-id="f649b-226">Például egy külső Hive tábla megadása esetén a külső Hive tábla adatai tárolhatja az Azure blob storage a következő név 000000_0 rendelkező.</span><span class="sxs-lookup"><span data-stu-id="f649b-226">For example, when you specify an external Hive table, the data for the external Hive table can be stored in Azure blob storage with the following name 000000_0.</span></span> <span data-ttu-id="f649b-227">Kísérlet az olvasó modul segítségével több fájlok olvasását, és előrejelzéseket használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="f649b-227">You can use the reader module in an experiment to read multiple files, and use them for predictions.</span></span>

<span data-ttu-id="f649b-228">Az olvasó modul használata az Azure Machine Learning a kísérlet, a Azure Blob bemenetként is megadhat.</span><span class="sxs-lookup"><span data-stu-id="f649b-228">When using the reader module in an Azure Machine Learning experiment, you can specify Azure Blob as an input.</span></span> <span data-ttu-id="f649b-229">A fájlok az Azure blob Storage tárolóban lehetnek a kimeneti fájlok (Példa: 000000_0), amely a HDInsight-on futó Pig és a Hive parancsfájlok hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="f649b-229">The files in the Azure blob storage can be the output files (Example: 000000_0) that are produced by a Pig and Hive script running on HDInsight.</span></span> <span data-ttu-id="f649b-230">Az olvasó modul lehetővé teszi, hogy olvassa el a (nincs kiterjesztésű) fájlokat úgy konfigurálja a **elérési útját, tároló könyvtár/blob**.</span><span class="sxs-lookup"><span data-stu-id="f649b-230">The reader module allows you to read files (with no extensions) by configuring the **Path to container, directory/blob**.</span></span> <span data-ttu-id="f649b-231">A **tároló elérési útja** mutat, a tároló és **könyvtár/blob** mutat, a következő ábrán látható módon a fájlokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="f649b-231">The **Path to container** points to the container and **directory/blob** points to folder that contains the files as shown in the following image.</span></span> <span data-ttu-id="f649b-232">A csillag Ez azt jelenti, hogy \*) **azt jelenti, hogy a tároló/mappában lévő összes fájl (Ez azt jelenti, hogy adatokat/aggregateddata/év 2014/hónap-6 = /\*)** olvassa el a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="f649b-232">The asterisk that is, \*) **specifies that all the files in the container/folder (that is, data/aggregateddata/year=2014/month-6/\*)** are read as part of the experiment.</span></span>

![Az Azure Blob tulajdonságai](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a><span data-ttu-id="f649b-234">Példa</span><span class="sxs-lookup"><span data-stu-id="f649b-234">Example</span></span>
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a><span data-ttu-id="f649b-235">A webszolgáltatás-paramétereket AzureMLBatchExecution tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="f649b-235">Pipeline with AzureMLBatchExecution activity with Web Service Parameters</span></span>

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

<span data-ttu-id="f649b-236">A fenti JSON-példa:</span><span class="sxs-lookup"><span data-stu-id="f649b-236">In the above JSON example:</span></span>

* <span data-ttu-id="f649b-237">A telepített Azure Machine Learning Web service olvasási/írási adatokat az vagy egy Azure SQL-adatbázis egy olvasó és író modul használatával.</span><span class="sxs-lookup"><span data-stu-id="f649b-237">The deployed Azure Machine Learning Web service uses a reader and a writer module to read/write data from/to an Azure SQL Database.</span></span> <span data-ttu-id="f649b-238">Ez a webszolgáltatás mutatja meg a következő négy paraméterek: adatbázis-kiszolgáló neve, a adatbázis neve, a kiszolgáló felhasználói fiók nevét és a kiszolgáló felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f649b-238">This Web service exposes the following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>  
* <span data-ttu-id="f649b-239">Mindkét **start** és **end** időpontok szerepelnie kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="f649b-239">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="f649b-240">Például: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="f649b-240">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="f649b-241">A **end** idő megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="f649b-241">The **end** time is optional.</span></span> <span data-ttu-id="f649b-242">Ha nem ad meg értéket a **end** tulajdonságot, akkor a program "**kezdés + 48 óra.**"</span><span class="sxs-lookup"><span data-stu-id="f649b-242">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="f649b-243">A folyamat határozatlan ideig történő futtatásához adja meg a **9999-09-09** értéket az **end** (befejezés) tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="f649b-243">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span> <span data-ttu-id="f649b-244">A JSON-tulajdonságokkal kapcsolatos információkért lásd: [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referencia a JSON-parancsprogramokhoz).</span><span class="sxs-lookup"><span data-stu-id="f649b-244">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

### <a name="other-scenarios"></a><span data-ttu-id="f649b-245">Egyéb forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="f649b-245">Other scenarios</span></span>
#### <a name="web-service-requires-multiple-inputs"></a><span data-ttu-id="f649b-246">Webszolgáltatás több bemeneti értéket igényel</span><span class="sxs-lookup"><span data-stu-id="f649b-246">Web service requires multiple inputs</span></span>
<span data-ttu-id="f649b-247">Ha a webszolgáltatás több bemeneti vesz igénybe, használja a **webServiceInputs** tulajdonság használata helyett **típus**.</span><span class="sxs-lookup"><span data-stu-id="f649b-247">If the web service takes multiple inputs, use the **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="f649b-248">Által hivatkozott adatkészletek a **webServiceInputs** is szerepelnie kell a tevékenység **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="f649b-248">Datasets that are referenced by the **webServiceInputs** must also be included in the Activity **inputs**.</span></span>

<span data-ttu-id="f649b-249">Az Azure ML kísérletben webszolgáltatás bemenetét és a kimeneti portok és a globális paraméterek alapértelmezett neve lehet ("input1", "input2"), amely testre szabható.</span><span class="sxs-lookup"><span data-stu-id="f649b-249">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="f649b-250">WebServiceInputs, webServiceOutputs és globalParameters beállítások használt neveket pontosan egyeznie kell a kísérleti nevét.</span><span class="sxs-lookup"><span data-stu-id="f649b-250">The names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match the names in the experiments.</span></span> <span data-ttu-id="f649b-251">A minta-kérések forgalma a a várt leképezés ellenőrzése az Azure ML végpont kötegelt végrehajtási súgó lapon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="f649b-251">You can view the sample request payload on the Batch Execution Help page for your Azure ML endpoint to verify the expected mapping.</span></span>

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

#### <a name="web-service-does-not-require-an-input"></a><span data-ttu-id="f649b-252">Webszolgáltatás nem szükséges bemeneti</span><span class="sxs-lookup"><span data-stu-id="f649b-252">Web Service does not require an input</span></span>
<span data-ttu-id="f649b-253">Az Azure ML kötegelt végrehajtási webszolgáltatások segítségével futtassa a munkafolyamatokat, például R vagy Python parancsfájlok, amelyekhez nem szükséges a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="f649b-253">Azure ML batch execution web services can be used to run any workflows, for example R or Python scripts, that may not require any inputs.</span></span> <span data-ttu-id="f649b-254">Vagy a kísérlet az olvasó modul, amely nem fedi fel minden GlobalParameters is megadhatók.</span><span class="sxs-lookup"><span data-stu-id="f649b-254">Or, the experiment might be configured with a Reader module that does not expose any GlobalParameters.</span></span> <span data-ttu-id="f649b-255">Ebben az esetben a AzureMLBatchExecution tevékenység úgy lesz beállítva, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f649b-255">In that case, the AzureMLBatchExecution Activity would be configured as follows:</span></span>

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

#### <a name="web-service-does-not-require-an-inputoutput"></a><span data-ttu-id="f649b-256">Webszolgáltatás nem szükséges egy bemeneti/kimeneti</span><span class="sxs-lookup"><span data-stu-id="f649b-256">Web Service does not require an input/output</span></span>
<span data-ttu-id="f649b-257">Az Azure ML kötegelt végrehajtási webszolgáltatás esetleg nincs konfigurálva webszolgáltatás kimenetet.</span><span class="sxs-lookup"><span data-stu-id="f649b-257">The Azure ML batch execution web service might not have any Web Service output configured.</span></span> <span data-ttu-id="f649b-258">Ebben a példában nincs webszolgáltatás bemeneti vagy kimeneti, sem bármely GlobalParameters vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f649b-258">In this example, there is no Web Service input or output, nor are any GlobalParameters configured.</span></span> <span data-ttu-id="f649b-259">Még nincs konfigurálva a tevékenység, maga a kimenettel, de ez nem egy webServiceOutput van megadva.</span><span class="sxs-lookup"><span data-stu-id="f649b-259">There is still an output configured on the activity itself, but it is not given as a webServiceOutput.</span></span>

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

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a><span data-ttu-id="f649b-260">Webes szolgáltatás által használt olvasók és írók, és csak akkor, ha más tevékenységek sikeres volt a tevékenység fut</span><span class="sxs-lookup"><span data-stu-id="f649b-260">Web Service uses readers and writers, and the activity runs only when other activities have succeeded</span></span>
<span data-ttu-id="f649b-261">Az Azure ML web service írási és olvasási szerepkörökhöz modulok vagy bármely GlobalParameters anélkül futtatásához is megadhatók.</span><span class="sxs-lookup"><span data-stu-id="f649b-261">The Azure ML web service reader and writer modules might be configured to run with or without any GlobalParameters.</span></span> <span data-ttu-id="f649b-262">Azonban érdemes lehet hívások beágyazása a folyamat által használt adatkészlet függőségek meghívni a szolgáltatás csak akkor, ha egy fölérendelt feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f649b-262">However, you may want to embed service calls in a pipeline that uses dataset dependencies to invoke the service only when some upstream processing has completed.</span></span> <span data-ttu-id="f649b-263">Más műveleteket is el lehet indítani, a kötegelt végrehajtás ezzel a megközelítéssel befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f649b-263">You can also trigger some other action after the batch execution has completed using this approach.</span></span> <span data-ttu-id="f649b-264">Ebben az esetben is express nélkül elnevezési valamelyiket, a webszolgáltatás bemeneti vagy kimeneti tevékenység bemenetekhez és kimenetekhez, használja a függőségeket.</span><span class="sxs-lookup"><span data-stu-id="f649b-264">In that case, you can express the dependencies using activity inputs and outputs, without naming any of them as Web Service inputs or outputs.</span></span>

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

<span data-ttu-id="f649b-265">A **takeaways** vannak:</span><span class="sxs-lookup"><span data-stu-id="f649b-265">The **takeaways** are:</span></span>

* <span data-ttu-id="f649b-266">Ha a kísérlet végpont egy típus használ: egy blob-adathalmazra képviseli, és a tevékenység bemeneti és a típus tulajdonság tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f649b-266">If your experiment endpoint uses a webServiceInput: it is represented by a blob dataset and is included in the activity inputs and the webServiceInput property.</span></span> <span data-ttu-id="f649b-267">A típus tulajdonság nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="f649b-267">Otherwise, the webServiceInput property is omitted.</span></span>
* <span data-ttu-id="f649b-268">Ha a kísérlet végpont használ webServiceOutput(s): blob adatkészletek képviseli, és tartalmazza a tevékenység kimeneteiből és webServiceOutputs tulajdonságában.</span><span class="sxs-lookup"><span data-stu-id="f649b-268">If your experiment endpoint uses webServiceOutput(s): they are represented by blob datasets and are included in the activity outputs and in the webServiceOutputs property.</span></span> <span data-ttu-id="f649b-269">A tevékenység kimenete és webServiceOutputs vannak leképezve a kísérletben minden kimeneti nevével.</span><span class="sxs-lookup"><span data-stu-id="f649b-269">The activity outputs and webServiceOutputs are mapped by the name of each output in the experiment.</span></span> <span data-ttu-id="f649b-270">Ellenkező esetben a webServiceOutputs tulajdonság nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="f649b-270">Otherwise, the webServiceOutputs property is omitted.</span></span>
* <span data-ttu-id="f649b-271">Ha a kísérlet végpont globalParameter(s) azt mutatja, a számukra tevékenység globalParameters tulajdonságában kulcs, érték párként.</span><span class="sxs-lookup"><span data-stu-id="f649b-271">If your experiment endpoint exposes globalParameter(s), they are given in the activity globalParameters property as key, value pairs.</span></span> <span data-ttu-id="f649b-272">Ellenkező esetben a globalParameters tulajdonság nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="f649b-272">Otherwise, the globalParameters property is omitted.</span></span> <span data-ttu-id="f649b-273">A kulcsokban kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="f649b-273">The keys are case-sensitive.</span></span> <span data-ttu-id="f649b-274">[Az Azure Data Factory funkciók](data-factory-functions-variables.md) értékek használható.</span><span class="sxs-lookup"><span data-stu-id="f649b-274">[Azure Data Factory functions](data-factory-functions-variables.md) may be used in the values.</span></span>
* <span data-ttu-id="f649b-275">További adathalmazokat szerepelni fog a tevékenység bemenetei és kimenetei tulajdonságait, anélkül, hogy a tevékenység typeProperties hivatkozik rá.</span><span class="sxs-lookup"><span data-stu-id="f649b-275">Additional datasets may be included in the Activity inputs and outputs properties, without being referenced in the Activity typeProperties.</span></span> <span data-ttu-id="f649b-276">Ezek az adatkészletek szelet függőségek végrehajtását szabályozására, de a AzureMLBatchExecution tevékenység ellenkező esetben figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="f649b-276">These datasets govern execution using slice dependencies but are otherwise ignored by the AzureMLBatchExecution Activity.</span></span>


## <a name="updating-models-using-update-resource-activity"></a><span data-ttu-id="f649b-277">A modellek használata az Update-Erőforrástevékenység frissítése</span><span class="sxs-lookup"><span data-stu-id="f649b-277">Updating models using Update Resource Activity</span></span>
<span data-ttu-id="f649b-278">Miután elkészült, az átképezési, frissítése a pontozási webszolgáltatás (prediktív kísérletté webszolgáltatásként kitett) újonnan betanított modell használatával a **Azure ML Update Erőforrástevékenység**.</span><span class="sxs-lookup"><span data-stu-id="f649b-278">After you are done with retraining, update the scoring web service (predictive experiment exposed as a web service) with the newly trained model by using the **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="f649b-279">Lásd: [modellek használata az Update-Erőforrástevékenység frissítése](data-factory-azure-ml-update-resource-activity.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="f649b-279">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

### <a name="reader-and-writer-modules"></a><span data-ttu-id="f649b-280">Olvasási és írási modulok</span><span class="sxs-lookup"><span data-stu-id="f649b-280">Reader and Writer Modules</span></span>
<span data-ttu-id="f649b-281">Egy általános forgatókönyv webszolgáltatás-paramétereket az Azure SQL-olvasók és írók.</span><span class="sxs-lookup"><span data-stu-id="f649b-281">A common scenario for using Web service parameters is the use of Azure SQL Readers and Writers.</span></span> <span data-ttu-id="f649b-282">Az olvasó modul az adatok betöltése az Azure Machine Learning Studio kívül adatszolgáltatásokból felügyeleti kísérlet szolgál.</span><span class="sxs-lookup"><span data-stu-id="f649b-282">The reader module is used to load data into an experiment from data management services outside Azure Machine Learning Studio.</span></span> <span data-ttu-id="f649b-283">Az író modul az adatok mentése a kísérletekből az Azure Machine Learning Studio kívüli adatok szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f649b-283">The writer module is to save data from your experiments into data management services outside Azure Machine Learning Studio.</span></span>  

<span data-ttu-id="f649b-284">Azure Blob vagy az Azure SQL olvasási/írási kapcsolatos részletekért lásd: [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) témakörök MSDN könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="f649b-284">For details about Azure Blob/Azure SQL reader/writer, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span> <span data-ttu-id="f649b-285">A példa az előző szakaszban használt Azure-Blob és az Azure Blob olvasási.</span><span class="sxs-lookup"><span data-stu-id="f649b-285">The example in the previous section used the Azure Blob reader and Azure Blob writer.</span></span> <span data-ttu-id="f649b-286">Ez a szakasz ismerteti az Azure SQL-olvasó és az Azure SQL-író segítségével.</span><span class="sxs-lookup"><span data-stu-id="f649b-286">This section discusses using Azure SQL reader and Azure SQL writer.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="f649b-287">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="f649b-287">Frequently asked questions</span></span>
<span data-ttu-id="f649b-288">**K:** a big Data típusú adatok folyamatok által létrehozott több fájl van.</span><span class="sxs-lookup"><span data-stu-id="f649b-288">**Q:** I have multiple files that are generated by my big data pipelines.</span></span> <span data-ttu-id="f649b-289">A AzureMLBatchExecution tevékenység használatával a fájlok?</span><span class="sxs-lookup"><span data-stu-id="f649b-289">Can I use the AzureMLBatchExecution Activity to work on all the files?</span></span>

<span data-ttu-id="f649b-290">**V:** Igen.</span><span class="sxs-lookup"><span data-stu-id="f649b-290">**A:** Yes.</span></span> <span data-ttu-id="f649b-291">Tekintse meg a **egy olvasó modul segítségével adatokat olvasni az Azure Blob több fájl** című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="f649b-291">See the **Using a Reader module to read data from multiple files in Azure Blob** section for details.</span></span>

## <a name="azure-ml-batch-scoring-activity"></a><span data-ttu-id="f649b-292">Azure ML kötegelt pontozási tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-292">Azure ML Batch Scoring Activity</span></span>
<span data-ttu-id="f649b-293">Ha használja a **AzureMLBatchScoring** tevékenység integrálása az Azure Machine Learning, azt javasoljuk, hogy használja-e a legújabb **AzureMLBatchExecution** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f649b-293">If you are using the **AzureMLBatchScoring** activity to integrate with Azure Machine Learning, we recommend that you use the latest **AzureMLBatchExecution** activity.</span></span>

<span data-ttu-id="f649b-294">A AzureMLBatchExecution tevékenység megjelent az Azure SDK és Azure PowerShell 2015. augusztus kiadásában.</span><span class="sxs-lookup"><span data-stu-id="f649b-294">The AzureMLBatchExecution activity is introduced in the August 2015 release of Azure SDK and Azure PowerShell.</span></span>

<span data-ttu-id="f649b-295">Ha azt szeretné, hogy tovább használja a AzureMLBatchScoring tevékenység, továbbra is az egész ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="f649b-295">If you want to continue using the AzureMLBatchScoring activity, continue reading through this section.</span></span>  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a><span data-ttu-id="f649b-296">Bemeneti/kimeneti az Azure Storage használata az Azure ML kötegelt pontozási tevékenység</span><span class="sxs-lookup"><span data-stu-id="f649b-296">Azure ML Batch Scoring activity using Azure Storage for input/output</span></span>

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

### <a name="web-service-parameters"></a><span data-ttu-id="f649b-297">Webszolgáltatás-paramétereket</span><span class="sxs-lookup"><span data-stu-id="f649b-297">Web Service Parameters</span></span>
<span data-ttu-id="f649b-298">Adható meg érték a webszolgáltatás-paramétereket, vegye fel a **typeProperties** szakaszban a **AzureMLBatchScoringActivty** az adatcsatorna JSON szakasz a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f649b-298">To specify values for Web service parameters, add a **typeProperties** section to the **AzureMLBatchScoringActivty** section in the pipeline JSON as shown in the following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
<span data-ttu-id="f649b-299">Is [Data Factory funkciók](data-factory-functions-variables.md) paraméterek szereplő értéket átadja a webes szolgáltatás, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f649b-299">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for the Web service parameters as shown in the following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="f649b-300">A webszolgáltatás-paramétereket kis-és nagybetűket, ezért figyeljen oda arra, hogy a nevét adja meg, ha a tevékenység JSON egyeznek azokkal a webszolgáltatás által elérhetővé tett tárolókra.</span><span class="sxs-lookup"><span data-stu-id="f649b-300">The Web service parameters are case-sensitive, so ensure that the names you specify in the activity JSON match the ones exposed by the Web service.</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="f649b-301">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f649b-301">See Also</span></span>
* [<span data-ttu-id="f649b-302">Az Azure blogbejegyzést: Ismerkedés az Azure Data Factory és az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="f649b-302">Azure blog post: Getting started with Azure Data Factory and Azure Machine Learning</span></span>](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
