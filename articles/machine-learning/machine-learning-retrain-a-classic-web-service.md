---
title: "A klasszikus webszolgáltatás újratanítása |} Microsoft Docs"
description: "Megtudhatja, hogyan programozott módon modell működik, és frissíti a az újonnan betanított modell használatára az Azure Machine Learning webszolgáltatás."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 3f9f4cd5ed36262845f7a3139073ccd4e49f472a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-classic-web-service"></a><span data-ttu-id="cab2b-103">Klasszikus webszolgáltatás újratanítása</span><span class="sxs-lookup"><span data-stu-id="cab2b-103">Retrain a Classic web service</span></span>
<span data-ttu-id="cab2b-104">A prediktív telepített webes szolgáltatás az alapértelmezett érték scoring-végpontja.</span><span class="sxs-lookup"><span data-stu-id="cab2b-104">The Predictive Web Service you deployed is the default scoring endpoint.</span></span> <span data-ttu-id="cab2b-105">Alapértelmezett végpontok szinkronban vannak az eredeti képzési és kísérletek pontozási tárolják, és ezért a betanított modell alapértelmezett végpont nem cserélhető le.</span><span class="sxs-lookup"><span data-stu-id="cab2b-105">Default endpoints are kept in sync with the original training and scoring experiments, and therefore the trained model for the default endpoint cannot be replaced.</span></span> <span data-ttu-id="cab2b-106">A webszolgáltatás működik, hozzá kell adnia egy új végpont a webszolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="cab2b-106">To retrain the web service, you must add a new endpoint to the web service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cab2b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cab2b-107">Prerequisites</span></span>
<span data-ttu-id="cab2b-108">Akkor be kell állítania egy tanítási kísérletet, és egy prediktív kísérletté látható módon [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="cab2b-108">You must have set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="cab2b-109">A prediktív kísérletté kell telepíteni, mint a klasszikus machine learning-webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cab2b-109">The predictive experiment must be deployed as a Classic machine learning web service.</span></span> 
> 
> 

<span data-ttu-id="cab2b-110">A web Services további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="cab2b-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="add-a-new-endpoint"></a><span data-ttu-id="cab2b-111">Új végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cab2b-111">Add a new Endpoint</span></span>
<span data-ttu-id="cab2b-112">A prediktív webszolgáltatás központilag telepített tartalmaz egy alapértelmezett scoring-végpontja, hogy szinkronban vannak az eredeti képzési és kísérletek betanított modell pontozása.</span><span class="sxs-lookup"><span data-stu-id="cab2b-112">The Predictive Web Service that you deployed contains a default scoring endpoint that is kept in sync with the original training and scoring experiments trained model.</span></span> <span data-ttu-id="cab2b-113">A webszolgáltatás számára új betanított modell egy új pontozási végpontot kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="cab2b-113">To update your web service to with a new trained model, you must create a new scoring endpoint.</span></span> 

<span data-ttu-id="cab2b-114">Egy új pontozási végpont létrehozásához a betanított modell frissíthető prediktív webszolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="cab2b-114">To create a new scoring endpoint, on the Predictive Web Service that can be updated with the trained model:</span></span>

> [!NOTE]
> <span data-ttu-id="cab2b-115">Győződjön meg arról, hogy a végpont a prediktív webszolgáltatás, nem a képzés webszolgáltatás kíván hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="cab2b-115">Be sure you are adding the endpoint to the Predictive Web Service, not the Training Web Service.</span></span> <span data-ttu-id="cab2b-116">Ha megfelelően telepítette, a képzési és a prediktív webszolgáltatás, megtekintheti az felsorolt két külön webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="cab2b-116">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate web services listed.</span></span> <span data-ttu-id="cab2b-117">A prediktív webszolgáltatás "[prediktív exp.]" kell végződnie.</span><span class="sxs-lookup"><span data-stu-id="cab2b-117">The Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

<span data-ttu-id="cab2b-118">Háromféleképpen, amelyben hozzáadhat egy új végpont egy webszolgáltatás-bővítmény:</span><span class="sxs-lookup"><span data-stu-id="cab2b-118">There are three ways in which you can add a new end point to a web service:</span></span>

1. <span data-ttu-id="cab2b-119">Automatizáltan</span><span class="sxs-lookup"><span data-stu-id="cab2b-119">Programmatically</span></span>
2. <span data-ttu-id="cab2b-120">A Microsoft Azure Web Services portál</span><span class="sxs-lookup"><span data-stu-id="cab2b-120">Use the Microsoft Azure Web Services portal</span></span>
3. <span data-ttu-id="cab2b-121">Használja a klasszikus Azure portált</span><span class="sxs-lookup"><span data-stu-id="cab2b-121">Use the Azure classic portal</span></span>

### <a name="programmatically-add-an-endpoint"></a><span data-ttu-id="cab2b-122">Programozott módon a végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cab2b-122">Programmatically add an endpoint</span></span>
<span data-ttu-id="cab2b-123">Pontozó végpontjaitól jelen mintakód is hozzáadhat [github-tárházban](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="cab2b-123">You can add scoring endpoints using the sample code provided in this [github repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span></span>

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a><span data-ttu-id="cab2b-124">Végpont hozzáadása a Microsoft Azure Web Services portál használatával</span><span class="sxs-lookup"><span data-stu-id="cab2b-124">Use the Microsoft Azure Web Services portal to add an endpoint</span></span>
1. <span data-ttu-id="cab2b-125">A Machine Learning Studio a bal oldali oszlopban kattintson a webszolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="cab2b-125">In Machine Learning Studio, on the left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="cab2b-126">A webes szolgáltatás irányítópultját alján kattintson **kezelése végpontok előzetes**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-126">At the bottom of the web service dashboard, click **Manage endpoints preview**.</span></span>
3. <span data-ttu-id="cab2b-127">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="cab2b-127">Click **Add**.</span></span>
4. <span data-ttu-id="cab2b-128">Írja be nevét és leírását, az új végpont.</span><span class="sxs-lookup"><span data-stu-id="cab2b-128">Type a name and description for the new endpoint.</span></span> <span data-ttu-id="cab2b-129">Válassza ki a naplózási szint, és hogy engedélyezve van-e mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="cab2b-129">Select the logging level and whether sample data is enabled.</span></span> <span data-ttu-id="cab2b-130">További információt a naplózást, [naplózását a Machine Learning webszolgáltatások](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="cab2b-130">For more information on logging, see [Enable logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>

### <a name="use-the-azure-classic-portal-to-add-an-endpoint"></a><span data-ttu-id="cab2b-131">Végpont hozzáadása a klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="cab2b-131">Use the Azure classic portal to add an endpoint</span></span>
1. <span data-ttu-id="cab2b-132">Jelentkezzen be a [klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="cab2b-132">Sign in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="cab2b-133">A bal oldali menüben kattintson **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-133">In the left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="cab2b-134">A név, kattintson a munkaterület majd **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-134">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="cab2b-135">A név, kattintson a **nyilvántartásba modell [prediktív exp].** .</span><span class="sxs-lookup"><span data-stu-id="cab2b-135">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="cab2b-136">Kattintson a lap alján **végpont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-136">At the bottom of the page, click **Add Endpoint**.</span></span> <span data-ttu-id="cab2b-137">Végpontok hozzáadásával kapcsolatos további információkért lásd: [végpontok létrehozása](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="cab2b-137">For more information on adding endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span> 

## <a name="update-the-added-endpoints-trained-model"></a><span data-ttu-id="cab2b-138">A hozzáadott végpont betanított modell frissítése</span><span class="sxs-lookup"><span data-stu-id="cab2b-138">Update the added endpoint’s Trained Model</span></span>
<span data-ttu-id="cab2b-139">A megőrzési befejezéséhez, frissítenie kell a hozzáadott új végpont a betanított modell.</span><span class="sxs-lookup"><span data-stu-id="cab2b-139">To complete the retraining process, you must update the trained model of the new endpoint that you added.</span></span>

* <span data-ttu-id="cab2b-140">Ha hozzáadta a klasszikus Azure portál használatával új végpont, kattintson az új végpont neve a portálon, majd a **UpdateResource** hivatkozásra az URL-címet a végpont modell frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="cab2b-140">If you added the new endpoint using the classic Azure portal, you can click the new endpoint's name in the portal, then the **UpdateResource** link to get the URL you would need to update the endpoint's model.</span></span>
* <span data-ttu-id="cab2b-141">Ha hozzáadta a végpont a mintakódot, ez magában foglalja a súgó által azonosított URL-cím helye a *HelpLocationURL* a kimeneti értéket.</span><span class="sxs-lookup"><span data-stu-id="cab2b-141">If you added the endpoint using the sample code, this includes location of the help URL identified by the *HelpLocationURL* value in the output.</span></span>

<span data-ttu-id="cab2b-142">Az útvonal URL-cím beolvasása:</span><span class="sxs-lookup"><span data-stu-id="cab2b-142">To retrieve the path URL:</span></span>

1. <span data-ttu-id="cab2b-143">Másolja és illessze be az URL-CÍMÉT a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="cab2b-143">Copy and paste the URL into your browser.</span></span>
2. <span data-ttu-id="cab2b-144">A frissítés erőforrás hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="cab2b-144">Click the Update Resource link.</span></span>
3. <span data-ttu-id="cab2b-145">Másolja a PATCH kérés POST URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="cab2b-145">Copy the POST URL of the PATCH request.</span></span> <span data-ttu-id="cab2b-146">Példa:</span><span class="sxs-lookup"><span data-stu-id="cab2b-146">For example:</span></span>
   
     <span data-ttu-id="cab2b-147">JAVÍTÁS URL-CÍM: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2</span><span class="sxs-lookup"><span data-stu-id="cab2b-147">PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2</span></span>

<span data-ttu-id="cab2b-148">Most már használhatja a betanított modell a korábban létrehozott pontozási végpont frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cab2b-148">You can now use the trained model to update the scoring endpoint that you created previously.</span></span>

<span data-ttu-id="cab2b-149">Az alábbi mintakód bemutatja, hogyan használható a *BaseLocation*, *RelativeLocation*, *SasBlobToken*, és az URL-cím javítására, de a végpont frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cab2b-149">The following sample code shows you how to use the *BaseLocation*, *RelativeLocation*, *SasBlobToken*, and PATCH URL to update the endpoint.</span></span>

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

<span data-ttu-id="cab2b-150">A *apiKey* és a *végponti URL-cím* vonatkozó hívása végpont irányítópult lehet lekérni.</span><span class="sxs-lookup"><span data-stu-id="cab2b-150">The *apiKey* and the *endpointUrl* for the call can be obtained from endpoint dashboard.</span></span>

<span data-ttu-id="cab2b-151">Értékét a *neve* paraméterének *erőforrások* meg kell felelnie a prediktív kísérletté a mentett betanított modell erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="cab2b-151">The value of the *Name* parameter in *Resources* should match the Resource Name of the saved Trained Model in the predictive experiment.</span></span> <span data-ttu-id="cab2b-152">Az erőforrásnév beolvasása:</span><span class="sxs-lookup"><span data-stu-id="cab2b-152">To get the Resource Name:</span></span>

1. <span data-ttu-id="cab2b-153">Jelentkezzen be a [klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="cab2b-153">Sign in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="cab2b-154">A bal oldali menüben kattintson **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-154">In the left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="cab2b-155">A név, kattintson a munkaterület majd **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-155">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="cab2b-156">A név, kattintson a **nyilvántartásba modell [prediktív exp].** .</span><span class="sxs-lookup"><span data-stu-id="cab2b-156">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="cab2b-157">Kattintson a hozzáadott új végpontot.</span><span class="sxs-lookup"><span data-stu-id="cab2b-157">Click the new endpoint you added.</span></span>
6. <span data-ttu-id="cab2b-158">A végpont irányítópultján kattintson **frissítés erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-158">On the endpoint dashboard, click **Update Resource**.</span></span>
7. <span data-ttu-id="cab2b-159">A webszolgáltatás frissítés erőforrás API-JÁNAK dokumentációja lapon található a **erőforrásnév** alatt **frissíthető erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="cab2b-159">On the Update Resource API Documentation page for the web service, you can find the **Resource Name** under **Updatable Resources**.</span></span>

<span data-ttu-id="cab2b-160">Ha befejezte a végpont frissítése a SAS-jogkivonat nem, el kell végeznie egy GET friss jogkivonat beszerzése feladatazonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cab2b-160">If your SAS token expires before you finish updating the endpoint, you must perform a GET with the Job Id to obtain a fresh token.</span></span>

<span data-ttu-id="cab2b-161">Amikor a kód sikeresen lefutott, az új végpont kell kezdődnie, körülbelül 30 másodpercet retrained modellt használja.</span><span class="sxs-lookup"><span data-stu-id="cab2b-161">When the code has successfully run, the new endpoint should start using the retrained model in approximately 30 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="cab2b-162">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="cab2b-162">Summary</span></span>
<span data-ttu-id="cab2b-163">Az Átképezési API-kkal, frissítheti a betanított modell egy prediktív webszolgáltatás forgatókönyvek például engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="cab2b-163">Using the Retraining APIs, you can update the trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="cab2b-164">Az új adatokat átképezési rendszeres modell.</span><span class="sxs-lookup"><span data-stu-id="cab2b-164">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="cab2b-165">Terjesztési ügyfelek-modell, azzal a céllal, hogy működik a modell használatával saját adataikat.</span><span class="sxs-lookup"><span data-stu-id="cab2b-165">Distribution of a model to customers with the goal of letting them retrain the model using their own data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cab2b-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cab2b-166">Next steps</span></span>
[<span data-ttu-id="cab2b-167">A klasszikus Azure Machine Learning webszolgáltatás átképezési hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="cab2b-167">Troubleshooting the retraining of an Azure Machine Learning classic web service</span></span>](machine-learning-troubleshooting-retraining-models.md)

