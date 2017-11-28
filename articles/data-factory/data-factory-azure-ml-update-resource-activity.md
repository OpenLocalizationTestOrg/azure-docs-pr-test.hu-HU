---
title: "Azure Data Factory használatával aaaUpdate gépi tanulási modellek |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate létre Azure Data Factory és az Azure Machine Learning a prediktív folyamatok"
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
ms.openlocfilehash: 6e5e4d2cfd245c7a9ed3bb9cdacca1f7f82b9620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a><span data-ttu-id="b4e78-103">Frissítés Azure Machine Learning modellek használata az Update-Erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-103">Updating Azure Machine Learning models using Update Resource Activity</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="b4e78-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="b4e78-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="b4e78-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="b4e78-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="b4e78-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="b4e78-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="b4e78-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="b4e78-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="b4e78-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="b4e78-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="b4e78-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="b4e78-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="b4e78-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="b4e78-114">Ez a cikk kiegészítés hello fő Azure Data Factory - Azure Machine Learning integrációs cikk: [létrehozása az Azure Machine Learning és az Azure Data Factory használatával prediktív folyamatok](data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="b4e78-114">This article complements hello main Azure Data Factory - Azure Machine Learning integration article: [Create predictive pipelines using Azure Machine Learning and Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span></span> <span data-ttu-id="b4e78-115">Ha még nem tette meg, tekintse át a cikk fő hello keresztül ez a cikk elolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="b4e78-115">If you haven't already done so, review hello main article before reading through this article.</span></span> 

## <a name="overview"></a><span data-ttu-id="b4e78-116">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b4e78-116">Overview</span></span>
<span data-ttu-id="b4e78-117">Az idő múlásával hello prediktív modelleket hello Azure ML pontozási kísérletekben kell toobe retrained új bemeneti adatkészletek használata.</span><span class="sxs-lookup"><span data-stu-id="b4e78-117">Over time, hello predictive models in hello Azure ML scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="b4e78-118">Miután elkészült, az átképezési, kívánt hello webszolgáltatás pontozási tooupdate hello retrained ML modellje.</span><span class="sxs-lookup"><span data-stu-id="b4e78-118">After you are done with retraining, you want tooupdate hello scoring web service with hello retrained ML model.</span></span> <span data-ttu-id="b4e78-119">hello jellemzően előforduló lépéseket tooenable átképezési és webszolgáltatásokkal frissítése Azure ML-modellek a következők:</span><span class="sxs-lookup"><span data-stu-id="b4e78-119">hello typical steps tooenable retraining and updating Azure ML models via web services are:</span></span>

1. <span data-ttu-id="b4e78-120">A kísérlet létrehozásának [Azure ML Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="b4e78-120">Create an experiment in [Azure ML Studio](https://studio.azureml.net).</span></span>
2. <span data-ttu-id="b4e78-121">Ha elégedett hello modell, használja az Azure ML Studio toopublish webszolgáltatás-mindkét hello **tanítási kísérletet** és pontozási /**prediktív kísérletté**.</span><span class="sxs-lookup"><span data-stu-id="b4e78-121">When you are satisfied with hello model, use Azure ML Studio toopublish web services for both hello **training experiment** and scoring/**predictive experiment**.</span></span>

<span data-ttu-id="b4e78-122">hello következő táblázat ismerteti a példában szereplő hello webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b4e78-122">hello following table describes hello web services used in this example.</span></span>  <span data-ttu-id="b4e78-123">Lásd: [Machine Learning-modellek szoftveres](../machine-learning/machine-learning-retrain-models-programmatically.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b4e78-123">See [Retrain Machine Learning models programmatically](../machine-learning/machine-learning-retrain-models-programmatically.md) for details.</span></span>

- <span data-ttu-id="b4e78-124">**Webszolgáltatás betanítása** - tanítási adatokat fogad, és hozza létre a betanított modellek.</span><span class="sxs-lookup"><span data-stu-id="b4e78-124">**Training web service** - Receives training data and produces trained models.</span></span> <span data-ttu-id="b4e78-125">hello hello átképezési eredménye egy .ilearner fájlt az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b4e78-125">hello output of hello retraining is an .ilearner file in an Azure Blob storage.</span></span> <span data-ttu-id="b4e78-126">Hello **alapértelmezett végpont** automatikusan létrejön a hello képzési közzétételekor kísérletezik webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="b4e78-126">hello **default endpoint** is automatically created for you when you publish hello training experiment as a web service.</span></span> <span data-ttu-id="b4e78-127">Létrehozhat további végpontok, de hello példában csak a hello alapértelmezett végpont.</span><span class="sxs-lookup"><span data-stu-id="b4e78-127">You can create more endpoints but hello example uses only hello default endpoint.</span></span>
- <span data-ttu-id="b4e78-128">**Webszolgáltatás pontozási** – címke nélküli adatok példák kap, és lehetővé teszi az előrejelzés.</span><span class="sxs-lookup"><span data-stu-id="b4e78-128">**Scoring web service** - Receives unlabeled data examples and makes predictions.</span></span> <span data-ttu-id="b4e78-129">hello kimeneti előrejelzési lehet különböző formát, például egy CSV-fájlt vagy egy Azure SQL-adatbázis hello kísérlet hello konfigurációjától függően sorokat.</span><span class="sxs-lookup"><span data-stu-id="b4e78-129">hello output of prediction could have various forms, such as a .csv file or rows in an Azure SQL database, depending on hello configuration of hello experiment.</span></span> <span data-ttu-id="b4e78-130">hello alapértelmezett végpont az Ön automatikusan jön létre, hello prediktív kísérletté webszolgáltatásként közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="b4e78-130">hello default endpoint is automatically created for you when you publish hello predictive experiment as a web service.</span></span> 

<span data-ttu-id="b4e78-131">hello alábbi kép ábrázolja képzési és a végpontok pontozás az Azure ml hello kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="b4e78-131">hello following picture depicts hello relationship between training and scoring endpoints in Azure ML.</span></span>

![Webszolgáltatások](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

<span data-ttu-id="b4e78-133">Hello hívhat meg **webszolgáltatás betanítása** hello segítségével **Azure ML kötegelt végrehajtási tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="b4e78-133">You can invoke hello **training web service** by using hello **Azure ML Batch Execution Activity**.</span></span> <span data-ttu-id="b4e78-134">Egy képzési webszolgáltatás indítására legyen, mint az Azure gépi tanulás webszolgáltatás (pontozási webszolgáltatás) való pontozási adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4e78-134">Invoking a training web service is same as invoking an Azure ML web service (scoring web service) for scoring data.</span></span> <span data-ttu-id="b4e78-135">hello hogyan tooinvoke az Azure gépi tanulás webszolgáltatás egy Azure Data Factory a csővezeték-részletesen előző szakaszokban lefedi.</span><span class="sxs-lookup"><span data-stu-id="b4e78-135">hello preceding sections cover how tooinvoke an Azure ML web service from an Azure Data Factory pipeline in detail.</span></span> 

<span data-ttu-id="b4e78-136">Hello hívhat meg **webszolgáltatás pontozási** hello segítségével **Azure ML Update-Erőforrástevékenység** tooupdate hello webszolgáltatás hello újonnan betanított modell.</span><span class="sxs-lookup"><span data-stu-id="b4e78-136">You can invoke hello **scoring web service** by using hello **Azure ML Update Resource Activity** tooupdate hello web service with hello newly trained model.</span></span> <span data-ttu-id="b4e78-137">a következő példák hello társított szolgáltatás definíciókat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b4e78-137">hello following examples provide linked service definitions:</span></span> 

## <a name="scoring-web-service-is-a-classic-web-service"></a><span data-ttu-id="b4e78-138">Webszolgáltatás pontozási egy klasszikus webszolgáltatás-bővítmény</span><span class="sxs-lookup"><span data-stu-id="b4e78-138">Scoring web service is a classic web service</span></span>
<span data-ttu-id="b4e78-139">Webszolgáltatás pontozási hello esetén egy **klasszikus webszolgáltatás**, hozzon létre második hello **nem alapértelmezett és frissíthető végpont** hello segítségével [Azure-portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b4e78-139">If hello scoring web service is a **classic web service**, create hello second **non-default and updatable endpoint** by using hello [Azure portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="b4e78-140">Lásd: [végpontok létrehozása](../machine-learning/machine-learning-create-endpoint.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="b4e78-140">See [Create Endpoints](../machine-learning/machine-learning-create-endpoint.md) article for steps.</span></span> <span data-ttu-id="b4e78-141">Miután létrehozta a hello nem alapértelmezett frissíthető végpont, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b4e78-141">After you create hello non-default updatable endpoint, do hello following steps:</span></span>

* <span data-ttu-id="b4e78-142">Kattintson a **KÖTEGELT végrehajtási** tooget hello URI érték hello **mlEndpoint** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4e78-142">Click **BATCH EXECUTION** tooget hello URI value for hello **mlEndpoint** JSON property.</span></span>
* <span data-ttu-id="b4e78-143">Kattintson a **frissítés erőforrás** tooget hello URI értéke hello hivatkozás **updateResourceEndpoint** JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b4e78-143">Click **UPDATE RESOURCE** link tooget hello URI value for hello **updateResourceEndpoint** JSON property.</span></span> <span data-ttu-id="b4e78-144">hello API-kulcs van oldalon hello végpont magát (a hello jobb alsó sarokban).</span><span class="sxs-lookup"><span data-stu-id="b4e78-144">hello API key is on hello endpoint page itself (in hello bottom-right corner).</span></span>

![frissíthető végpont](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

<span data-ttu-id="b4e78-146">a következő példa hello biztosít egy minta hello AzureML társított szolgáltatás JSON definíciója.</span><span class="sxs-lookup"><span data-stu-id="b4e78-146">hello following example provides a sample JSON definition for hello AzureML linked service.</span></span> <span data-ttu-id="b4e78-147">hello társított szolgáltatás hello apiKey, használ.</span><span class="sxs-lookup"><span data-stu-id="b4e78-147">hello linked service uses hello apiKey for authentication.</span></span>  

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

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a><span data-ttu-id="b4e78-148">Webszolgáltatás pontozás az Azure Resource Manager webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4e78-148">Scoring web service is Azure Resource Manager web service</span></span> 
<span data-ttu-id="b4e78-149">Ha hello webszolgáltatás hello új webes szolgáltatás, amely elérhetővé teszi az Azure Resource Manager-végpont típusú, nem kell tooadd hello második **nem alapértelmezett** végpont.</span><span class="sxs-lookup"><span data-stu-id="b4e78-149">If hello web service is hello new type of web service that exposes an Azure Resource Manager endpoint, you do not need tooadd hello second **non-default** endpoint.</span></span> <span data-ttu-id="b4e78-150">Hello **updateResourceEndpoint** hello a társított szolgáltatás hello formátum van:</span><span class="sxs-lookup"><span data-stu-id="b4e78-150">hello **updateResourceEndpoint** in hello linked service is of hello format:</span></span> 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

<span data-ttu-id="b4e78-151">Kaphat értékek hely tulajdonosainak hello URL-címben hello hello webhellyel lekérdezésekor [Azure Machine Learning Web Services portálra](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="b4e78-151">You can get values for place holders in hello URL when querying hello web service on hello [Azure Machine Learning Web Services Portal](https://services.azureml.net/).</span></span> <span data-ttu-id="b4e78-152">frissítés erőforrás végpont új típusú hello igényel az (Azure Active Directory) AAD-tokent.</span><span class="sxs-lookup"><span data-stu-id="b4e78-152">hello new type of update resource endpoint requires an AAD (Azure Active Directory) token.</span></span> <span data-ttu-id="b4e78-153">Adja meg **servicePrincipalId** és **servicePrincipalKey**az AzureML társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4e78-153">Specify **servicePrincipalId** and **servicePrincipalKey**in AzureML linked service.</span></span> <span data-ttu-id="b4e78-154">Lásd: [hogyan toocreate egyszerű szolgáltatást, és rendelje hozzá az engedélyek toomanage Azure-erőforrás](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b4e78-154">See [how toocreate service principal and assign permissions toomanage Azure resource](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="b4e78-155">Íme egy minta AzureML társított szolgáltatás definíciójának:</span><span class="sxs-lookup"><span data-stu-id="b4e78-155">Here is a sample AzureML linked service definition:</span></span> 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "hello linked service for AML web service.",
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

<span data-ttu-id="b4e78-156">hello következő forgatókönyv további részleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4e78-156">hello following scenario provides more details.</span></span> <span data-ttu-id="b4e78-157">Rendelkezik egy példa átképezési, és az Azure Data Factory-folyamat az Azure ML modellek frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4e78-157">It has an example for retraining and updating Azure ML models from an Azure Data Factory pipeline.</span></span>

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a><span data-ttu-id="b4e78-158">Forgatókönyv: átképezési, és az Azure ML modellje frissítése</span><span class="sxs-lookup"><span data-stu-id="b4e78-158">Scenario: retraining and updating an Azure ML model</span></span>
<span data-ttu-id="b4e78-159">Ez a témakör egy minta folyamat által használt hello **Azure ML kötegelt végrehajtási tevékenység** tooretrain egy modell.</span><span class="sxs-lookup"><span data-stu-id="b4e78-159">This section provides a sample pipeline that uses hello **Azure ML Batch Execution activity** tooretrain a model.</span></span> <span data-ttu-id="b4e78-160">hello folyamatot is alkalmaz hello **Azure ML Update erőforrástevékenység** tooupdate hello modell pontozása webszolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="b4e78-160">hello pipeline also uses hello **Azure ML Update Resource activity** tooupdate hello model in hello scoring web service.</span></span> <span data-ttu-id="b4e78-161">hello szakasz is biztosít JSON kódtöredékek összes hello összekapcsolt szolgáltatások, az adatkészleteket és a kimenetátirányítási mechanizmusával hello példa.</span><span class="sxs-lookup"><span data-stu-id="b4e78-161">hello section also provides JSON snippets for all hello linked services, datasets, and pipeline in hello example.</span></span>

<span data-ttu-id="b4e78-162">Itt található hello diagram nézet hello minta folyamatának.</span><span class="sxs-lookup"><span data-stu-id="b4e78-162">Here is hello diagram view of hello sample pipeline.</span></span> <span data-ttu-id="b4e78-163">Ahogy látja, hello Azure ML kötegelt végrehajtási tevékenység hello képzési bemenetből fogad adatokat, és a képzési kimenetet (iLearner-fájlt).</span><span class="sxs-lookup"><span data-stu-id="b4e78-163">As you can see, hello Azure ML Batch Execution Activity takes hello training input and produces a training output (iLearner file).</span></span> <span data-ttu-id="b4e78-164">hello Azure ML Update-Erőforrástevékenység időt vesz igénybe, a képzési kimenetet, és a frissítések hello webszolgáltatási végpontot pontozási hello modellt.</span><span class="sxs-lookup"><span data-stu-id="b4e78-164">hello Azure ML Update Resource Activity takes this training output and updates hello model in hello scoring web service endpoint.</span></span> <span data-ttu-id="b4e78-165">hello Update-Erőforrástevékenység nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="b4e78-165">hello Update Resource Activity does not produce any output.</span></span> <span data-ttu-id="b4e78-166">hello placeholderBlob csak hello Azure Data Factory szolgáltatás toorun hello-folyamat által igényelt üres kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="b4e78-166">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>

![folyamat diagramja](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a><span data-ttu-id="b4e78-168">Az Azure Blob storage társított szolgáltatásnak:</span><span class="sxs-lookup"><span data-stu-id="b4e78-168">Azure Blob storage linked service:</span></span>
<span data-ttu-id="b4e78-169">hello Azure Storage hello a következő adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b4e78-169">hello Azure Storage holds hello following data:</span></span>

* <span data-ttu-id="b4e78-170">betanítási adata.</span><span class="sxs-lookup"><span data-stu-id="b4e78-170">training data.</span></span> <span data-ttu-id="b4e78-171">hello bemeneti adatok hello Azure ML képzési webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b4e78-171">hello input data for hello Azure ML training web service.</span></span>  
* <span data-ttu-id="b4e78-172">iLearner-fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4e78-172">iLearner file.</span></span> <span data-ttu-id="b4e78-173">hello hello Azure ML képzési webszolgáltatás kimenetét.</span><span class="sxs-lookup"><span data-stu-id="b4e78-173">hello output from hello Azure ML training web service.</span></span> <span data-ttu-id="b4e78-174">A fájl is hello bemeneti toohello Update-erőforrástevékenység.</span><span class="sxs-lookup"><span data-stu-id="b4e78-174">This file is also hello input toohello Update Resource activity.</span></span>  

<span data-ttu-id="b4e78-175">Hello minta JSON-definícióból kapcsolódó hello szolgáltatást a következő:</span><span class="sxs-lookup"><span data-stu-id="b4e78-175">Here is hello sample JSON definition of hello linked service:</span></span>

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

### <a name="training-input-dataset"></a><span data-ttu-id="b4e78-176">Képzési bemeneti adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="b4e78-176">Training input dataset:</span></span>
<span data-ttu-id="b4e78-177">hello következő dataset hello bemeneti tanítási adatokat képvisel hello Azure ML képzési webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b4e78-177">hello following dataset represents hello input training data for hello Azure ML training web service.</span></span> <span data-ttu-id="b4e78-178">Azure ML kötegelt végrehajtási tevékenység hello ehhez az adatkészlethez bemenetként vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b4e78-178">hello Azure ML Batch Execution activity takes this dataset as an input.</span></span>

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

### <a name="training-output-dataset"></a><span data-ttu-id="b4e78-179">Képzési kimeneti adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="b4e78-179">Training output dataset:</span></span>
<span data-ttu-id="b4e78-180">hello következő dataset jelöli hello kimeneti iLearner-fájlt hello Azure ML képzési webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b4e78-180">hello following dataset represents hello output iLearner file from hello Azure ML training web service.</span></span> <span data-ttu-id="b4e78-181">hello Azure ML kötegelt végrehajtási tevékenység létrehozza az adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="b4e78-181">hello Azure ML Batch Execution Activity produces this dataset.</span></span> <span data-ttu-id="b4e78-182">Ez az adatkészlet esetében is hello bemeneti toohello Azure ML Update erőforrás tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b4e78-182">This dataset is also hello input toohello Azure ML Update Resource activity.</span></span>

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

### <a name="linked-service-for-azure-ml-training-endpoint"></a><span data-ttu-id="b4e78-183">Azure ML képzési végponthoz társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4e78-183">Linked service for Azure ML training endpoint</span></span>
<span data-ttu-id="b4e78-184">a következő JSON részlet hello határozza meg az Azure Machine Learning társított szolgáltatást az toohello alapértelmezett végpont hello képzési webszolgáltatás mutat.</span><span class="sxs-lookup"><span data-stu-id="b4e78-184">hello following JSON snippet defines an Azure Machine Learning linked service that points toohello default endpoint of hello training web service.</span></span>

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

<span data-ttu-id="b4e78-185">A **Azure ML Studio**, tooget értékeket a következő hello **mlEndpoint** és **apiKey**:</span><span class="sxs-lookup"><span data-stu-id="b4e78-185">In **Azure ML Studio**, do hello following tooget values for **mlEndpoint** and **apiKey**:</span></span>

1. <span data-ttu-id="b4e78-186">Kattintson a **WEBSZOLGÁLTATÁSOK** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="b4e78-186">Click **WEB SERVICES** on hello left menu.</span></span>
2. <span data-ttu-id="b4e78-187">Kattintson a hello **webszolgáltatás betanítása** webszolgáltatások hello listájában.</span><span class="sxs-lookup"><span data-stu-id="b4e78-187">Click hello **training web service** in hello list of web services.</span></span>
3. <span data-ttu-id="b4e78-188">Kattintson a Másolás tovább túl**API-kulcs** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="b4e78-188">Click copy next too**API key** text box.</span></span> <span data-ttu-id="b4e78-189">Illessze be hello kulcs hello vágólapon hello Data Factory JSON-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="b4e78-189">Paste hello key in hello clipboard into hello Data Factory JSON editor.</span></span>
4. <span data-ttu-id="b4e78-190">A hello **Azure ML studio**, kattintson a **KÖTEGELT végrehajtási** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b4e78-190">In hello **Azure ML studio**, click **BATCH EXECUTION** link.</span></span>
5. <span data-ttu-id="b4e78-191">Másolás hello **kérelem URI-azonosítója** a hello **kérelem** szakaszt, és illessze be hello Data Factory JSON-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="b4e78-191">Copy hello **Request URI** from hello **Request** section and paste it into hello Data Factory JSON editor.</span></span>   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a><span data-ttu-id="b4e78-192">Azure ML frissíthető pontozási végponthoz társított szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="b4e78-192">Linked Service for Azure ML updatable scoring endpoint:</span></span>
<span data-ttu-id="b4e78-193">a következő JSON részlet hello határozza meg az Azure Machine Learning társított szolgáltatást az toohello nem alapértelmezett frissíthető végpontja webszolgáltatás pontozási hello mutat.</span><span class="sxs-lookup"><span data-stu-id="b4e78-193">hello following JSON snippet defines an Azure Machine Learning linked service that points toohello non-default updatable endpoint of hello scoring web service.</span></span>  

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

### <a name="placeholder-output-dataset"></a><span data-ttu-id="b4e78-194">Helyőrző kimeneti adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="b4e78-194">Placeholder output dataset:</span></span>
<span data-ttu-id="b4e78-195">hello Azure ML Update erőforrás tevékenység nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="b4e78-195">hello Azure ML Update Resource activity does not generate any output.</span></span> <span data-ttu-id="b4e78-196">Azure Data Factory azonban egy kimeneti adatkészlet toodrive hello ütemezést adatcsatorna igényel.</span><span class="sxs-lookup"><span data-stu-id="b4e78-196">However, Azure Data Factory requires an output dataset toodrive hello schedule of a pipeline.</span></span> <span data-ttu-id="b4e78-197">A helyőrző/helyőrző dataset ezért ebben a példában használjuk.</span><span class="sxs-lookup"><span data-stu-id="b4e78-197">Therefore, we use a dummy/placeholder dataset in this example.</span></span>  

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

### <a name="pipeline"></a><span data-ttu-id="b4e78-198">Folyamat</span><span class="sxs-lookup"><span data-stu-id="b4e78-198">Pipeline</span></span>
<span data-ttu-id="b4e78-199">hello folyamat két tevékenység rendelkezik: **AzureMLBatchExecution** és **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="b4e78-199">hello pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="b4e78-200">hello Azure ML kötegelt végrehajtási tevékenység hello betanítási adatok bemenetként vesz igénybe, és egy kimenetként iLearner-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4e78-200">hello Azure ML Batch Execution activity takes hello training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="b4e78-201">hello tevékenység hello képzési webszolgáltatás (a tanítási kísérletet webszolgáltatásként kitett) hív hello bevitellel betanítási adatok, és hello ilearner-fájlt kapott hello webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4e78-201">hello activity invokes hello training web service (training experiment exposed as a web service) with hello input training data and receives hello ilearner file from hello webservice.</span></span> <span data-ttu-id="b4e78-202">hello placeholderBlob csak hello Azure Data Factory szolgáltatás toorun hello-folyamat által igényelt üres kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="b4e78-202">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>

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
