---
title: "a Stream Analytics aaaUse Azure Machine Learning-végpont |} Microsoft Docs"
description: "A Stream Analytics gép nyelvi felhasználó által definiált függvények"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 406b258f-b8c2-4e55-953c-b7f84e8e5354
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 013b841ee85b1e0b6d8139a9ba0dde88fc3f8ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-integration-in-stream-analytics"></a><span data-ttu-id="f755c-103">A Stream Analytics tanulási integrációs számítógép</span><span class="sxs-lookup"><span data-stu-id="f755c-103">Machine Learning integration in Stream Analytics</span></span>
<span data-ttu-id="f755c-104">A Stream Analytics támogatja a felhasználó által definiált függvények, amelyek tooAzure Machine Learning-végpont hívásához.</span><span class="sxs-lookup"><span data-stu-id="f755c-104">Stream Analytics supports user-defined functions that call out tooAzure Machine Learning endpoints.</span></span> <span data-ttu-id="f755c-105">Ez a szolgáltatás REST API-támogatása részleteit a hello [Stream Analytics REST API-könyvtár](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span><span class="sxs-lookup"><span data-stu-id="f755c-105">REST API support for this feature is detailed in hello [Stream Analytics REST API library](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span></span> <span data-ttu-id="f755c-106">Ez a cikk ezt a képességet a Stream Analytics sikeres végrehajtásához szükség kiegészítő információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f755c-106">This article provides supplemental information needed for successful implementation of this capability in Stream Analytics.</span></span> <span data-ttu-id="f755c-107">Oktatóanyag is közzé lett, és elérhető [Itt](stream-analytics-machine-learning-integration-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="f755c-107">A tutorial has also been posted and is available [here](stream-analytics-machine-learning-integration-tutorial.md).</span></span>

## <a name="overview-azure-machine-learning-terminology"></a><span data-ttu-id="f755c-108">Áttekintés: Azure Machine Learning-terminológia</span><span class="sxs-lookup"><span data-stu-id="f755c-108">Overview: Azure Machine Learning terminology</span></span>
<span data-ttu-id="f755c-109">A Microsoft Azure Machine Learning használható együttműködést támogató, egérrel húzási és elengedési eszközt biztosít toobuild, tesztelését és rendszerbe adataihoz prediktív elemzési megoldások.</span><span class="sxs-lookup"><span data-stu-id="f755c-109">Microsoft Azure Machine Learning provides a collaborative, drag-and-drop tool you can use toobuild, test, and deploy predictive analytics solutions on your data.</span></span> <span data-ttu-id="f755c-110">Ez az eszköz neve hello *Azure Machine Learning Studio*.</span><span class="sxs-lookup"><span data-stu-id="f755c-110">This tool is called hello *Azure Machine Learning Studio*.</span></span> <span data-ttu-id="f755c-111">hello studio használt toointeract hello a Machine Learning erőforrások és egyszerűen hozhatók létre, tesztelheti, és a Tervező többször.</span><span class="sxs-lookup"><span data-stu-id="f755c-111">hello studio is used toointeract with hello Machine Learning resources and easily build, test, and iterate on your design.</span></span> <span data-ttu-id="f755c-112">Ezeket az erőforrásokat és a definíciójukat alatt van.</span><span class="sxs-lookup"><span data-stu-id="f755c-112">These resources and their definitions are below.</span></span>

* <span data-ttu-id="f755c-113">**Munkaterület**: hello *munkaterület* egy olyan tároló, amely tárolja az összes többi Machine Learning erőforrása együtt a felügyeleti és ellenőrzési tárolója.</span><span class="sxs-lookup"><span data-stu-id="f755c-113">**Workspace**: hello *workspace* is a container that holds all other Machine Learning resources together in a container for management and control.</span></span>
* <span data-ttu-id="f755c-114">**Kísérlet**: *kísérletek* adatok kutatók tooutilize adatkészletek hozza létre, és a gépi tanulási modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="f755c-114">**Experiment**: *Experiments* are created by data scientists tooutilize datasets and train a machine learning model.</span></span>
* <span data-ttu-id="f755c-115">**Végpont**: *végpontok* hello Azure Machine Learning használt objektum tootake szolgáltatások bemeneti adatként, alkalmazza a megadott gépi tanulási modell, és a pontozott kimenet visszaadása.</span><span class="sxs-lookup"><span data-stu-id="f755c-115">**Endpoint**: *Endpoints* are hello Azure Machine Learning object used tootake features as input, apply a specified machine learning model and return scored output.</span></span>
* <span data-ttu-id="f755c-116">**Webszolgáltatás pontozási**: A *webservice pontozási* olyan végpontok gyűjteménye, fent említett.</span><span class="sxs-lookup"><span data-stu-id="f755c-116">**Scoring Webservice**: A *scoring webservice* is a collection of endpoints as mentioned above.</span></span>

<span data-ttu-id="f755c-117">Minden egyes végpont API-k vannak a kötegelt végrehajtási és szinkron végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="f755c-117">Each endpoint has apis for batch execution and synchronous execution.</span></span> <span data-ttu-id="f755c-118">A Stream Analytics szinkron végrehajtási használja.</span><span class="sxs-lookup"><span data-stu-id="f755c-118">Stream Analytics uses synchronous execution.</span></span> <span data-ttu-id="f755c-119">hello adott szolgáltatás nevű egy [kérelem-válasz szolgáltatás](../machine-learning/machine-learning-consume-web-services.md) AzureML Studio.</span><span class="sxs-lookup"><span data-stu-id="f755c-119">hello specific service is named a [Request/Response Service](../machine-learning/machine-learning-consume-web-services.md) in AzureML studio.</span></span>

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a><span data-ttu-id="f755c-120">Gépi tanulási, amelyekre szükség van a Stream Analytics-feladatok</span><span class="sxs-lookup"><span data-stu-id="f755c-120">Machine Learning resources needed for Stream Analytics jobs</span></span>
<span data-ttu-id="f755c-121">A Stream Analytics hello alkalmazásában feladat-feldolgozás, a kérelem/válasz végpont egy [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), és a swagger-definíciójában minden szükséges, a sikeres végrehajtáshoz.</span><span class="sxs-lookup"><span data-stu-id="f755c-121">For hello purposes of Stream Analytics job processing, a Request/Response endpoint, an [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), and a swagger definition are all necessary for successful execution.</span></span> <span data-ttu-id="f755c-122">A Stream Analytics egy további végpontot, amely swagger-végpont URL-címe hello hoz létre, hello felületet és visszaadása egy alapértelmezett UDF definition toohello felhasználó rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f755c-122">Stream Analytics has an additional endpoint that constructs hello url for swagger endpoint, looks up hello interface and returns a default UDF definition toohello user.</span></span>

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a><span data-ttu-id="f755c-123">Konfiguráljon egy Stream Analytics és a gépi tanulási UDF REST API-n keresztül</span><span class="sxs-lookup"><span data-stu-id="f755c-123">Configure a Stream Analytics and Machine Learning UDF via REST API</span></span>
<span data-ttu-id="f755c-124">A feladat toocall Azure Machine nyelvi funkciókat konfigurálhatja REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="f755c-124">By using REST APIs you may configure your job toocall Azure Machine Language functions.</span></span> <span data-ttu-id="f755c-125">hello lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="f755c-125">hello steps are as follows:</span></span>

1. <span data-ttu-id="f755c-126">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f755c-126">Create a Stream Analytics job</span></span>
2. <span data-ttu-id="f755c-127">Adja meg a bemeneti</span><span class="sxs-lookup"><span data-stu-id="f755c-127">Define an input</span></span>
3. <span data-ttu-id="f755c-128">Adja meg egy kimeneti</span><span class="sxs-lookup"><span data-stu-id="f755c-128">Define an output</span></span>
4. <span data-ttu-id="f755c-129">Hozzon létre egy felhasználói függvény (UDF)</span><span class="sxs-lookup"><span data-stu-id="f755c-129">Create a user-defined function (UDF)</span></span>
5. <span data-ttu-id="f755c-130">Írás a Stream Analytics átalakítás, hogy a hívások hello UDF-ben</span><span class="sxs-lookup"><span data-stu-id="f755c-130">Write a Stream Analytics transformation that calls hello UDF</span></span>
6. <span data-ttu-id="f755c-131">Hello feladat indítása</span><span class="sxs-lookup"><span data-stu-id="f755c-131">Start hello job</span></span>

## <a name="creating-a-udf-with-basic-properties"></a><span data-ttu-id="f755c-132">Egy UDF alaptulajdonságait létrehozása</span><span class="sxs-lookup"><span data-stu-id="f755c-132">Creating a UDF with basic properties</span></span>
<span data-ttu-id="f755c-133">Tegyük fel, a következő példakód hello létrehoz egy skaláris UDF nevű *newudf* , amely tooan Azure Machine Learning-végpont van kötve.</span><span class="sxs-lookup"><span data-stu-id="f755c-133">As an example, hello following sample code creates a scalar UDF named *newudf* that binds tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="f755c-134">Vegye figyelembe, hogy hello *végpont* (URI-szolgáltatás) található a kiválasztott szolgáltatás- és hello hello hello API súgólapján *apiKey* hello szolgáltatások fő lapján található.</span><span class="sxs-lookup"><span data-stu-id="f755c-134">Note that hello *endpoint* (service URI) can be found on hello API help page for hello chosen service and hello *apiKey* can be found on hello Services main page.</span></span>

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

<span data-ttu-id="f755c-135">Példa kérelemtörzset:</span><span class="sxs-lookup"><span data-stu-id="f755c-135">Example request body:</span></span>  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a><span data-ttu-id="f755c-136">Alapértelmezett UDF RetrieveDefaultDefinition végpont meghívása</span><span class="sxs-lookup"><span data-stu-id="f755c-136">Call RetrieveDefaultDefinition endpoint for default UDF</span></span>
<span data-ttu-id="f755c-137">Egyszer hello vázat UDF jön létre a teljes definíciója hello hello UDF van szükség.</span><span class="sxs-lookup"><span data-stu-id="f755c-137">Once hello skeleton UDF is created hello complete definition of hello UDF is needed.</span></span> <span data-ttu-id="f755c-138">hello RetreiveDefaultDefinition végpont segít skaláris függvényt, amely kötött tooan Azure Machine Learning-végpont hello alapértelmezett definíciója.</span><span class="sxs-lookup"><span data-stu-id="f755c-138">hello RetreiveDefaultDefinition endpoint helps you get hello default definition for a scalar function that is bound tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="f755c-139">hello hasznos az alábbi szükséges tooget hello alapértelmezett UDF definition skaláris függvény, amely kötött tooan Azure Machine Learning-végpont esetében.</span><span class="sxs-lookup"><span data-stu-id="f755c-139">hello payload below requires you tooget hello default UDF definition for a scalar function that is bound tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="f755c-140">Mivel azt már megadott PUT kérelem során, azt az hello tényleges végpontja nem ad meg.</span><span class="sxs-lookup"><span data-stu-id="f755c-140">It doesn’t specify hello actual endpoint as it has already been provided during PUT request.</span></span> <span data-ttu-id="f755c-141">A Stream Analytics meghívja a hello kérelemben megadott, ha explicit módon megadott hello végpont.</span><span class="sxs-lookup"><span data-stu-id="f755c-141">Stream Analytics calls hello endpoint provided in hello request if it is provided explicitly.</span></span> <span data-ttu-id="f755c-142">Ellenkező esetben egy eredetileg hivatkozott hello használ.</span><span class="sxs-lookup"><span data-stu-id="f755c-142">Otherwise it uses hello one originally referenced.</span></span> <span data-ttu-id="f755c-143">Itt hello egy karakterlánc-paramétert (mondat), és értéket ad vissza, egyetlen kimeneti karakterlánc típusú, ami azt jelzi, hogy a mondatok hello "véleményeket" címke UDF vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f755c-143">Here hello UDF takes a single string parameter (a sentence) and returns a single output of type string which indicates hello “sentiment” label for that sentence.</span></span>

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

<span data-ttu-id="f755c-144">Példa kérelemtörzset:</span><span class="sxs-lookup"><span data-stu-id="f755c-144">Example request body:</span></span>  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

<span data-ttu-id="f755c-145">A kimeneti minta e keresse meg valami szeretné alatt.</span><span class="sxs-lookup"><span data-stu-id="f755c-145">A sample output of this would look something like below.</span></span>  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-hello-response"></a><span data-ttu-id="f755c-146">Javítás UDF hello válasz</span><span class="sxs-lookup"><span data-stu-id="f755c-146">Patch UDF with hello response</span></span>
<span data-ttu-id="f755c-147">Most hello UDF kell javítani hello előző válaszban, az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="f755c-147">Now hello UDF must be patched with hello previous response, as shown below.</span></span>

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

<span data-ttu-id="f755c-148">A kérelem törzsében (RetrieveDefaultDefinition kimenete):</span><span class="sxs-lookup"><span data-stu-id="f755c-148">Request Body (Output from RetrieveDefaultDefinition):</span></span>

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-toocall-hello-udf"></a><span data-ttu-id="f755c-149">A Stream Analytics átalakítása toocall hello UDF megvalósítása</span><span class="sxs-lookup"><span data-stu-id="f755c-149">Implement Stream Analytics transformation toocall hello UDF</span></span>
<span data-ttu-id="f755c-150">Most lekérdezéséhez hello (Itt nevű scoreTweet) UDF minden bemeneti esemény és az adott esemény tooan kimeneti választ.</span><span class="sxs-lookup"><span data-stu-id="f755c-150">Now query hello UDF (here named scoreTweet) for every input event and write a response for that event tooan output.</span></span>  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a><span data-ttu-id="f755c-151">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="f755c-151">Get help</span></span>
<span data-ttu-id="f755c-152">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="f755c-152">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f755c-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f755c-153">Next steps</span></span>
* [<span data-ttu-id="f755c-154">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="f755c-154">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="f755c-155">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="f755c-155">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="f755c-156">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="f755c-156">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="f755c-157">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="f755c-157">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="f755c-158">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="f755c-158">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
