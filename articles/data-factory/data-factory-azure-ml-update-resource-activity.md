---
title: "Azure Data Factory használatával gépi tanulási modellek módosítása |} Microsoft Docs"
description: "Ismerteti, hogyan hozzon létre Azure Data Factory és az Azure Machine Learning a prediktív folyamatok létrehozása"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: e31a7a59d14de4382190b39bd70f3ddf6cf673ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a><span data-ttu-id="4a0eb-103">Frissítés Azure Machine Learning modellek használata az Update-Erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-103">Updating Azure Machine Learning models using Update Resource Activity</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="4a0eb-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="4a0eb-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="4a0eb-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="4a0eb-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="4a0eb-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="4a0eb-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="4a0eb-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="4a0eb-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="4a0eb-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="4a0eb-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="4a0eb-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="4a0eb-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="4a0eb-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="4a0eb-114">Ez a cikk kiegészíti a fő Azure Data Factory - Azure Machine Learning integrációs cikk: [létrehozása az Azure Machine Learning és az Azure Data Factory használatával prediktív folyamatok](data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="4a0eb-114">This article complements the main Azure Data Factory - Azure Machine Learning integration article: [Create predictive pipelines using Azure Machine Learning and Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span></span> <span data-ttu-id="4a0eb-115">Ha még nem tette meg, tekintse át a fő cikk keresztül ez a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-115">If you haven't already done so, review the main article before reading through this article.</span></span> 

## <a name="overview"></a><span data-ttu-id="4a0eb-116">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4a0eb-116">Overview</span></span>
<span data-ttu-id="4a0eb-117">Az Azure ml kísérletek pontozási prediktív modelleket idővel kell kell retrained új bemeneti adatkészletek használata.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-117">Over time, the predictive models in the Azure ML scoring experiments need to be retrained using new input datasets.</span></span> <span data-ttu-id="4a0eb-118">Miután elkészült, az átképezési, a pontozási webszolgáltatás retrained ML-modell frissíteni kívánt.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-118">After you are done with retraining, you want to update the scoring web service with the retrained ML model.</span></span> <span data-ttu-id="4a0eb-119">A tipikus lépések elvégzésével engedélyezniük megőrzési és frissítési webszolgáltatásokkal Azure ML-modellek a következők:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-119">The typical steps to enable retraining and updating Azure ML models via web services are:</span></span>

1. <span data-ttu-id="4a0eb-120">A kísérlet létrehozásának [Azure ML Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="4a0eb-120">Create an experiment in [Azure ML Studio](https://studio.azureml.net).</span></span>
2. <span data-ttu-id="4a0eb-121">Ha elégedett a modellel, mind a webes szolgáltatásokat használja Azure ML Studio a **tanítási kísérletet** és pontozási /**prediktív kísérletté**.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-121">When you are satisfied with the model, use Azure ML Studio to publish web services for both the **training experiment** and scoring/**predictive experiment**.</span></span>

<span data-ttu-id="4a0eb-122">A következő táblázat ismerteti a webszolgáltatásokat, ebben a példában használt.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-122">The following table describes the web services used in this example.</span></span>  <span data-ttu-id="4a0eb-123">Lásd: [Machine Learning-modellek szoftveres](../machine-learning/machine-learning-retrain-models-programmatically.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-123">See [Retrain Machine Learning models programmatically](../machine-learning/machine-learning-retrain-models-programmatically.md) for details.</span></span>

- <span data-ttu-id="4a0eb-124">**Webszolgáltatás betanítása** - tanítási adatokat fogad, és hozza létre a betanított modellek.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-124">**Training web service** - Receives training data and produces trained models.</span></span> <span data-ttu-id="4a0eb-125">A átképezési eredménye egy .ilearner fájlt az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-125">The output of the retraining is an .ilearner file in an Azure Blob storage.</span></span> <span data-ttu-id="4a0eb-126">A **alapértelmezett végpont** automatikusan létrejön a képzés közzétételekor kísérletezik webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-126">The **default endpoint** is automatically created for you when you publish the training experiment as a web service.</span></span> <span data-ttu-id="4a0eb-127">Létrehozhat további végpontok, de a példában csak az alapértelmezett végpont.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-127">You can create more endpoints but the example uses only the default endpoint.</span></span>
- <span data-ttu-id="4a0eb-128">**Webszolgáltatás pontozási** – címke nélküli adatok példák kap, és lehetővé teszi az előrejelzés.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-128">**Scoring web service** - Receives unlabeled data examples and makes predictions.</span></span> <span data-ttu-id="4a0eb-129">Előrejelzés kimenete különböző formát, például egy CSV-fájlt vagy egy Azure SQL-adatbázis, a kísérlet konfigurációjától függően sorok rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-129">The output of prediction could have various forms, such as a .csv file or rows in an Azure SQL database, depending on the configuration of the experiment.</span></span> <span data-ttu-id="4a0eb-130">Az alapértelmezett végpont automatikusan létrejön egy webszolgáltatás prediktív kísérletté közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-130">The default endpoint is automatically created for you when you publish the predictive experiment as a web service.</span></span> 

<span data-ttu-id="4a0eb-131">Az alábbi képen képzési és a végpontok pontozás az Azure ml közötti kapcsolatot ábrázolja.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-131">The following picture depicts the relationship between training and scoring endpoints in Azure ML.</span></span>

![Webszolgáltatások](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

<span data-ttu-id="4a0eb-133">Hívhat meg a **webszolgáltatás betanítása** használatával a **Azure ML kötegelt végrehajtási tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-133">You can invoke the **training web service** by using the **Azure ML Batch Execution Activity**.</span></span> <span data-ttu-id="4a0eb-134">Egy képzési webszolgáltatás indítására legyen, mint az Azure gépi tanulás webszolgáltatás (pontozási webszolgáltatás) való pontozási adatokat.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-134">Invoking a training web service is same as invoking an Azure ML web service (scoring web service) for scoring data.</span></span> <span data-ttu-id="4a0eb-135">Az előző szakaszokban részletesen az Azure Data Factory-folyamat az az Azure ML webszolgáltatás meghívására foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-135">The preceding sections cover how to invoke an Azure ML web service from an Azure Data Factory pipeline in detail.</span></span> 

<span data-ttu-id="4a0eb-136">Hívhat meg a **webszolgáltatás pontozási** használatával a **Azure ML Update Erőforrástevékenység** frissítheti a webszolgáltatás a újonnan betanított modell.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-136">You can invoke the **scoring web service** by using the **Azure ML Update Resource Activity** to update the web service with the newly trained model.</span></span> <span data-ttu-id="4a0eb-137">Az alábbi példák megadják a kapcsolódószolgáltatás-definíciók:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-137">The following examples provide linked service definitions:</span></span> 

## <a name="scoring-web-service-is-a-classic-web-service"></a><span data-ttu-id="4a0eb-138">Webszolgáltatás pontozási egy klasszikus webszolgáltatás-bővítmény</span><span class="sxs-lookup"><span data-stu-id="4a0eb-138">Scoring web service is a classic web service</span></span>
<span data-ttu-id="4a0eb-139">Ha a pontozási webszolgáltatás egy **klasszikus webszolgáltatás**, hozzon létre a második **nem alapértelmezett és frissíthető végpont** használatával a [Azure-portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4a0eb-139">If the scoring web service is a **classic web service**, create the second **non-default and updatable endpoint** by using the [Azure portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="4a0eb-140">Lásd: [végpontok létrehozása](../machine-learning/machine-learning-create-endpoint.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-140">See [Create Endpoints](../machine-learning/machine-learning-create-endpoint.md) article for steps.</span></span> <span data-ttu-id="4a0eb-141">Miután létrehozta a nem alapértelmezett frissíthető végpont, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-141">After you create the non-default updatable endpoint, do the following steps:</span></span>

* <span data-ttu-id="4a0eb-142">Kattintson a **KÖTEGELT végrehajtási** URI értékének eléréséhez a **mlEndpoint** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-142">Click **BATCH EXECUTION** to get the URI value for the **mlEndpoint** JSON property.</span></span>
* <span data-ttu-id="4a0eb-143">Kattintson a **frissítés erőforrás** hivatkozásra az URI értéke a **updateResourceEndpoint** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-143">Click **UPDATE RESOURCE** link to get the URI value for the **updateResourceEndpoint** JSON property.</span></span> <span data-ttu-id="4a0eb-144">Az API-kulcsot a végpont lapon magát (a jobb alsó sarokban) van.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-144">The API key is on the endpoint page itself (in the bottom-right corner).</span></span>

![frissíthető végpont](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

<span data-ttu-id="4a0eb-146">A következő példa egy minta az AzureML társított szolgáltatás JSON-definícióból biztosít.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-146">The following example provides a sample JSON definition for the AzureML linked service.</span></span> <span data-ttu-id="4a0eb-147">A társított szolgáltatás, a apiKey használ.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-147">The linked service uses the apiKey for authentication.</span></span>  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a><span data-ttu-id="4a0eb-148">Webszolgáltatás pontozás az Azure Resource Manager webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4a0eb-148">Scoring web service is Azure Resource Manager web service</span></span> 
<span data-ttu-id="4a0eb-149">Ha a webszolgáltatás egy webszolgáltatás, amely elérhetővé teszi az Azure Resource Manager-végpont új típusú, nem kell hozzáadnia a második **nem alapértelmezett** végpont.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-149">If the web service is the new type of web service that exposes an Azure Resource Manager endpoint, you do not need to add the second **non-default** endpoint.</span></span> <span data-ttu-id="4a0eb-150">A **updateResourceEndpoint** formátumban van a hivatkozott szolgáltatásban található:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-150">The **updateResourceEndpoint** in the linked service is of the format:</span></span> 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

<span data-ttu-id="4a0eb-151">Kaphat értékek hely tartozó felhasználók számára az URL-címben a webkiszolgáló lekérdezésekor a [Azure Machine Learning Web Services portálra](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4a0eb-151">You can get values for place holders in the URL when querying the web service on the [Azure Machine Learning Web Services Portal](https://services.azureml.net/).</span></span> <span data-ttu-id="4a0eb-152">A frissítés erőforrás végpont új típusú van szükség az (Azure Active Directory) AAD-tokent.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-152">The new type of update resource endpoint requires an AAD (Azure Active Directory) token.</span></span> <span data-ttu-id="4a0eb-153">Adja meg **servicePrincipalId** és **servicePrincipalKey**az AzureML társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-153">Specify **servicePrincipalId** and **servicePrincipalKey**in AzureML linked service.</span></span> <span data-ttu-id="4a0eb-154">Lásd: [egyszerű szolgáltatásnév létrehozása és hozzárendelése az Azure erőforrások kezeléséhez szükséges jogokat](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4a0eb-154">See [how to create service principal and assign permissions to manage Azure resource](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="4a0eb-155">Íme egy minta AzureML társított szolgáltatás definíciójának:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-155">Here is a sample AzureML linked service definition:</span></span> 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

<span data-ttu-id="4a0eb-156">Az alábbi forgatókönyvet további részleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-156">The following scenario provides more details.</span></span> <span data-ttu-id="4a0eb-157">Rendelkezik egy példa átképezési, és az Azure Data Factory-folyamat az Azure ML modellek frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-157">It has an example for retraining and updating Azure ML models from an Azure Data Factory pipeline.</span></span>

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a><span data-ttu-id="4a0eb-158">Forgatókönyv: átképezési, és az Azure ML modellje frissítése</span><span class="sxs-lookup"><span data-stu-id="4a0eb-158">Scenario: retraining and updating an Azure ML model</span></span>
<span data-ttu-id="4a0eb-159">Ez a témakör egy minta folyamatot, amely használja a **Azure ML kötegelt végrehajtási tevékenység** a modell működik.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-159">This section provides a sample pipeline that uses the **Azure ML Batch Execution activity** to retrain a model.</span></span> <span data-ttu-id="4a0eb-160">A folyamatot is alkalmaz a **Azure ML Update erőforrástevékenység** az pontozási webszolgáltatás a modell frissítése.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-160">The pipeline also uses the **Azure ML Update Resource activity** to update the model in the scoring web service.</span></span> <span data-ttu-id="4a0eb-161">A szakasz is biztosít JSON kódtöredékek az összekapcsolt szolgáltatások, adatkészleteket és a példában szereplő folyamat.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-161">The section also provides JSON snippets for all the linked services, datasets, and pipeline in the example.</span></span>

<span data-ttu-id="4a0eb-162">Ez a diagram nézet a minta-feldolgozási folyamat.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-162">Here is the diagram view of the sample pipeline.</span></span> <span data-ttu-id="4a0eb-163">Ahogy látja, az Azure ML kötegelt végrehajtási tevékenység a képzés bemenetből fogad adatokat, és a képzési kimenetet (iLearner-fájlt).</span><span class="sxs-lookup"><span data-stu-id="4a0eb-163">As you can see, the Azure ML Batch Execution Activity takes the training input and produces a training output (iLearner file).</span></span> <span data-ttu-id="4a0eb-164">Az Azure ML Update-Erőforrástevékenység időt vesz igénybe a képzés kimenetet, és frissíti a modellt a pontozási webszolgáltatási végpontot.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-164">The Azure ML Update Resource Activity takes this training output and updates the model in the scoring web service endpoint.</span></span> <span data-ttu-id="4a0eb-165">Az Update-Erőforrástevékenység nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-165">The Update Resource Activity does not produce any output.</span></span> <span data-ttu-id="4a0eb-166">A placeholderBlob csak az Azure Data Factory szolgáltatásnak a feldolgozási sor futtatásához szükséges üres kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-166">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>

![folyamat diagramja](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a><span data-ttu-id="4a0eb-168">Az Azure Blob storage társított szolgáltatásnak:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-168">Azure Blob storage linked service:</span></span>
<span data-ttu-id="4a0eb-169">Az Azure Storage a következő adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-169">The Azure Storage holds the following data:</span></span>

* <span data-ttu-id="4a0eb-170">betanítási adata.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-170">training data.</span></span> <span data-ttu-id="4a0eb-171">A bemeneti adatok az Azure ML képzési webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-171">The input data for the Azure ML training web service.</span></span>  
* <span data-ttu-id="4a0eb-172">iLearner-fájlt.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-172">iLearner file.</span></span> <span data-ttu-id="4a0eb-173">Az Azure ML képzési webszolgáltatás kimenetét.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-173">The output from the Azure ML training web service.</span></span> <span data-ttu-id="4a0eb-174">Ez a fájl egyben a frissítési erőforrás tevékenység bemeneti.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-174">This file is also the input to the Update Resource activity.</span></span>  

<span data-ttu-id="4a0eb-175">Ez a minta társított szolgáltatás JSON-definícióból:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-175">Here is the sample JSON definition of the linked service:</span></span>

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a><span data-ttu-id="4a0eb-176">Képzési bemeneti adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-176">Training input dataset:</span></span>
<span data-ttu-id="4a0eb-177">A következő adatkészlet jelenti. a bemeneti betanítási adatok, az Azure ML képzési webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-177">The following dataset represents the input training data for the Azure ML training web service.</span></span> <span data-ttu-id="4a0eb-178">Az Azure ML kötegelt végrehajtási tevékenység ehhez az adatkészlethez bemenetként vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-178">The Azure ML Batch Execution activity takes this dataset as an input.</span></span>

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
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

### <a name="training-output-dataset"></a><span data-ttu-id="4a0eb-179">Képzési kimeneti adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-179">Training output dataset:</span></span>
<span data-ttu-id="4a0eb-180">A következő adatkészlet a kimenetet iLearner-fájlt az Azure ML képzési webszolgáltatás jelöli.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-180">The following dataset represents the output iLearner file from the Azure ML training web service.</span></span> <span data-ttu-id="4a0eb-181">Az Azure ML kötegelt végrehajtási tevékenység létrehozza az adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-181">The Azure ML Batch Execution Activity produces this dataset.</span></span> <span data-ttu-id="4a0eb-182">Ez az adatkészlet esetében is az Azure ML Update erőforrás tevékenység bemeneti.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-182">This dataset is also the input to the Azure ML Update Resource activity.</span></span>

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a><span data-ttu-id="4a0eb-183">Azure ML képzési végponthoz társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4a0eb-183">Linked service for Azure ML training endpoint</span></span>
<span data-ttu-id="4a0eb-184">A következő JSON-részlet egy Azure Machine Learning társított szolgáltatás mutat, az alapértelmezett végpont az képzési webszolgáltatás határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-184">The following JSON snippet defines an Azure Machine Learning linked service that points to the default endpoint of the training web service.</span></span>

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

<span data-ttu-id="4a0eb-185">A **Azure ML Studio**, hajtsa végre a következő értékek **mlEndpoint** és **apiKey**:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-185">In **Azure ML Studio**, do the following to get values for **mlEndpoint** and **apiKey**:</span></span>

1. <span data-ttu-id="4a0eb-186">Kattintson a **WEBSZOLGÁLTATÁSOK** a bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-186">Click **WEB SERVICES** on the left menu.</span></span>
2. <span data-ttu-id="4a0eb-187">Kattintson a **webszolgáltatás betanítása** a webes szolgáltatások közül.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-187">Click the **training web service** in the list of web services.</span></span>
3. <span data-ttu-id="4a0eb-188">Jelölje be a másolási **API-kulcs** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-188">Click copy next to **API key** text box.</span></span> <span data-ttu-id="4a0eb-189">Illessze be a kulcsot a vágólapra a Data Factory JSON-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-189">Paste the key in the clipboard into the Data Factory JSON editor.</span></span>
4. <span data-ttu-id="4a0eb-190">Az a **Azure ML studio**, kattintson a **KÖTEGELT végrehajtási** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-190">In the **Azure ML studio**, click **BATCH EXECUTION** link.</span></span>
5. <span data-ttu-id="4a0eb-191">Másolás a **kérelem URI-azonosítója** a a **kérelem** szakaszt, és illessze be a Data Factory JSON-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-191">Copy the **Request URI** from the **Request** section and paste it into the Data Factory JSON editor.</span></span>   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a><span data-ttu-id="4a0eb-192">Azure ML frissíthető pontozási végponthoz társított szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-192">Linked Service for Azure ML updatable scoring endpoint:</span></span>
<span data-ttu-id="4a0eb-193">A következő JSON-részlet egy csatolt Azure Machine Learning szolgáltatás, amely a nem alapértelmezett frissíthető végpontra pontozási webszolgáltatás mutató határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-193">The following JSON snippet defines an Azure Machine Learning linked service that points to the non-default updatable endpoint of the scoring web service.</span></span>  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a><span data-ttu-id="4a0eb-194">Helyőrző kimeneti adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="4a0eb-194">Placeholder output dataset:</span></span>
<span data-ttu-id="4a0eb-195">Az Azure ML Update erőforrás tevékenység nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-195">The Azure ML Update Resource activity does not generate any output.</span></span> <span data-ttu-id="4a0eb-196">Azure Data Factory azonban annak az adatcsatorna ütemezés egy kimeneti adatkészlet szükséges.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-196">However, Azure Data Factory requires an output dataset to drive the schedule of a pipeline.</span></span> <span data-ttu-id="4a0eb-197">A helyőrző/helyőrző dataset ezért ebben a példában használjuk.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-197">Therefore, we use a dummy/placeholder dataset in this example.</span></span>  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a><span data-ttu-id="4a0eb-198">Folyamat</span><span class="sxs-lookup"><span data-stu-id="4a0eb-198">Pipeline</span></span>
<span data-ttu-id="4a0eb-199">A folyamat két tevékenység rendelkezik: **AzureMLBatchExecution** és **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-199">The pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="4a0eb-200">Az Azure ML kötegelt végrehajtási tevékenység lekéri a tanítási adatokat bemeneti adatként, és egy kimenetként iLearner-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-200">The Azure ML Batch Execution activity takes the training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="4a0eb-201">A tevékenység hív meg, a képzési webszolgáltatás (a tanítási kísérletet webszolgáltatásként kitett) a bemeneti betanítási adatok, és megkapja a webservice ilearner-fájlt.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-201">The activity invokes the training web service (training experiment exposed as a web service) with the input training data and receives the ilearner file from the webservice.</span></span> <span data-ttu-id="4a0eb-202">A placeholderBlob csak az Azure Data Factory szolgáltatásnak a feldolgozási sor futtatásához szükséges üres kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="4a0eb-202">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>

![folyamat diagramja](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
