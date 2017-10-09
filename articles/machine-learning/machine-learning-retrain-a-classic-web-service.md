---
title: "a klasszikus webszolgáltatás aaaRetrain |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprogrammatically újratanítása a modell és a frissítés hello webes toouse hello újonnan betanított modell az Azure Machine Learning."
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
ms.openlocfilehash: d3ba21ed75f02868535cb2fcac607643303a9554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-classic-web-service"></a><span data-ttu-id="717ef-103">Klasszikus webszolgáltatás újratanítása</span><span class="sxs-lookup"><span data-stu-id="717ef-103">Retrain a Classic web service</span></span>
<span data-ttu-id="717ef-104">hello prediktív webszolgáltatás üzembe helyezett scoring-végpontja hello alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="717ef-104">hello Predictive Web Service you deployed is hello default scoring endpoint.</span></span> <span data-ttu-id="717ef-105">Alapértelmezett végpontok tartják hello szinkronban eredeti képzési és kísérletek pontozási, és ezért hello betanított modell hello alapértelmezett végpont nem cserélhető le.</span><span class="sxs-lookup"><span data-stu-id="717ef-105">Default endpoints are kept in sync with hello original training and scoring experiments, and therefore hello trained model for hello default endpoint cannot be replaced.</span></span> <span data-ttu-id="717ef-106">tooretrain hello webszolgáltatás, hozzá kell adnia egy új végpont toohello webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="717ef-106">tooretrain hello web service, you must add a new endpoint toohello web service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="717ef-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="717ef-107">Prerequisites</span></span>
<span data-ttu-id="717ef-108">Akkor be kell állítania egy tanítási kísérletet, és egy prediktív kísérletté látható módon [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="717ef-108">You must have set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="717ef-109">hello prediktív kísérletté kell telepíteni, mint a klasszikus machine learning-webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="717ef-109">hello predictive experiment must be deployed as a Classic machine learning web service.</span></span> 
> 
> 

<span data-ttu-id="717ef-110">A web Services további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="717ef-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="add-a-new-endpoint"></a><span data-ttu-id="717ef-111">Új végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="717ef-111">Add a new Endpoint</span></span>
<span data-ttu-id="717ef-112">hello prediktív webszolgáltatás központilag telepített scoring-végpontja, hogy szinkronban hello eredeti képzési és pontozási kísérletek betanított modell alapértelmezett tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="717ef-112">hello Predictive Web Service that you deployed contains a default scoring endpoint that is kept in sync with hello original training and scoring experiments trained model.</span></span> <span data-ttu-id="717ef-113">tooupdate a webes szolgáltatás toowith új modell betanítását, létre kell hoznia egy új pontozási végpontot.</span><span class="sxs-lookup"><span data-stu-id="717ef-113">tooupdate your web service toowith a new trained model, you must create a new scoring endpoint.</span></span> 

<span data-ttu-id="717ef-114">új pontozási végpont, a betanított modell hello frissíthető prediktív webszolgáltatásból hello toocreate:</span><span class="sxs-lookup"><span data-stu-id="717ef-114">toocreate a new scoring endpoint, on hello Predictive Web Service that can be updated with hello trained model:</span></span>

> [!NOTE]
> <span data-ttu-id="717ef-115">Győződjön meg arról, hogy hello végpont toohello prediktív webes szolgáltatás, a képzési webszolgáltatás hello nem ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="717ef-115">Be sure you are adding hello endpoint toohello Predictive Web Service, not hello Training Web Service.</span></span> <span data-ttu-id="717ef-116">Ha megfelelően telepítette, a képzési és a prediktív webszolgáltatás, megtekintheti az felsorolt két külön webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="717ef-116">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate web services listed.</span></span> <span data-ttu-id="717ef-117">"[prediktív exp.]" Hello prediktív webszolgáltatás kell végződnie.</span><span class="sxs-lookup"><span data-stu-id="717ef-117">hello Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

<span data-ttu-id="717ef-118">Háromféleképpen, amelyben hozzáadhat egy új végpont tooa webszolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="717ef-118">There are three ways in which you can add a new end point tooa web service:</span></span>

1. <span data-ttu-id="717ef-119">Automatizáltan</span><span class="sxs-lookup"><span data-stu-id="717ef-119">Programmatically</span></span>
2. <span data-ttu-id="717ef-120">Hello a Microsoft Azure Web Services portálon</span><span class="sxs-lookup"><span data-stu-id="717ef-120">Use hello Microsoft Azure Web Services portal</span></span>
3. <span data-ttu-id="717ef-121">A klasszikus Azure portálon hello használata</span><span class="sxs-lookup"><span data-stu-id="717ef-121">Use hello Azure classic portal</span></span>

### <a name="programmatically-add-an-endpoint"></a><span data-ttu-id="717ef-122">Programozott módon a végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="717ef-122">Programmatically add an endpoint</span></span>
<span data-ttu-id="717ef-123">Pontozó végpontjaitól jelen hello mintakód is hozzáadhat [github-tárházban](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="717ef-123">You can add scoring endpoints using hello sample code provided in this [github repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span></span>

### <a name="use-hello-microsoft-azure-web-services-portal-tooadd-an-endpoint"></a><span data-ttu-id="717ef-124">Hello Microsoft Azure Web Services portál tooadd végpont használata</span><span class="sxs-lookup"><span data-stu-id="717ef-124">Use hello Microsoft Azure Web Services portal tooadd an endpoint</span></span>
1. <span data-ttu-id="717ef-125">A Machine Learning Studio hello bal oldali oszlopban kattintson a webszolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="717ef-125">In Machine Learning Studio, on hello left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="717ef-126">Hello web service irányítópult hello alul kattintson **kezelése végpontok előzetes**.</span><span class="sxs-lookup"><span data-stu-id="717ef-126">At hello bottom of hello web service dashboard, click **Manage endpoints preview**.</span></span>
3. <span data-ttu-id="717ef-127">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="717ef-127">Click **Add**.</span></span>
4. <span data-ttu-id="717ef-128">Írja be nevét és leírását hello új végpont.</span><span class="sxs-lookup"><span data-stu-id="717ef-128">Type a name and description for hello new endpoint.</span></span> <span data-ttu-id="717ef-129">Válassza ki a hello naplózási szint, és hogy engedélyezve van-e mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="717ef-129">Select hello logging level and whether sample data is enabled.</span></span> <span data-ttu-id="717ef-130">További információt a naplózást, [naplózását a Machine Learning webszolgáltatások](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="717ef-130">For more information on logging, see [Enable logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>

### <a name="use-hello-azure-classic-portal-tooadd-an-endpoint"></a><span data-ttu-id="717ef-131">Az Azure klasszikus portál tooadd végpont hello használata</span><span class="sxs-lookup"><span data-stu-id="717ef-131">Use hello Azure classic portal tooadd an endpoint</span></span>
1. <span data-ttu-id="717ef-132">Jelentkezzen be toohello [klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="717ef-132">Sign in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="717ef-133">Hello bal oldali menüben kattintson a **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="717ef-133">In hello left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="717ef-134">A név, kattintson a munkaterület majd **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="717ef-134">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="717ef-135">A név, kattintson a **nyilvántartásba modell [prediktív exp].** .</span><span class="sxs-lookup"><span data-stu-id="717ef-135">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="717ef-136">Hello a hello lap alján, kattintson **végpont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="717ef-136">At hello bottom of hello page, click **Add Endpoint**.</span></span> <span data-ttu-id="717ef-137">Végpontok hozzáadásával kapcsolatos további információkért lásd: [végpontok létrehozása](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="717ef-137">For more information on adding endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span> 

## <a name="update-hello-added-endpoints-trained-model"></a><span data-ttu-id="717ef-138">Frissítés hello hozzáadott végpont betanított modell</span><span class="sxs-lookup"><span data-stu-id="717ef-138">Update hello added endpoint’s Trained Model</span></span>
<span data-ttu-id="717ef-139">toocomplete hello megőrzési folyamat, frissítenie kell a betanított modell hello hello új végpont hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="717ef-139">toocomplete hello retraining process, you must update hello trained model of hello new endpoint that you added.</span></span>

* <span data-ttu-id="717ef-140">Ha új végpont hello hello a klasszikus Azure portál használatával adott hozzá, továbbra is hello portal hello új végpont nevére kattint, majd hello **UpdateResource** tooget hello URL-cím tooupdate hello végpont modell kell kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="717ef-140">If you added hello new endpoint using hello classic Azure portal, you can click hello new endpoint's name in hello portal, then hello **UpdateResource** link tooget hello URL you would need tooupdate hello endpoint's model.</span></span>
* <span data-ttu-id="717ef-141">Hello végpont hello mintakód használatával adott hozzá, ha ez magában foglalja a hely hello súgó URL-címének hello által azonosított *HelpLocationURL* hello kimeneti értéket.</span><span class="sxs-lookup"><span data-stu-id="717ef-141">If you added hello endpoint using hello sample code, this includes location of hello help URL identified by hello *HelpLocationURL* value in hello output.</span></span>

<span data-ttu-id="717ef-142">tooretrieve hello elérési URL-címe:</span><span class="sxs-lookup"><span data-stu-id="717ef-142">tooretrieve hello path URL:</span></span>

1. <span data-ttu-id="717ef-143">Másolással illessze be a hello URL-CÍMÉT a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="717ef-143">Copy and paste hello URL into your browser.</span></span>
2. <span data-ttu-id="717ef-144">Hello frissítés erőforrás hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="717ef-144">Click hello Update Resource link.</span></span>
3. <span data-ttu-id="717ef-145">Másolja a hello hello a JAVÍTÁSI kérelemben POST URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="717ef-145">Copy hello POST URL of hello PATCH request.</span></span> <span data-ttu-id="717ef-146">Példa:</span><span class="sxs-lookup"><span data-stu-id="717ef-146">For example:</span></span>
   
     <span data-ttu-id="717ef-147">JAVÍTÁS URL-CÍM: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2</span><span class="sxs-lookup"><span data-stu-id="717ef-147">PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2</span></span>

<span data-ttu-id="717ef-148">Most már használhatja a hello betanított modell tooupdate hello scoring-végpontja, amelyet korábban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="717ef-148">You can now use hello trained model tooupdate hello scoring endpoint that you created previously.</span></span>

<span data-ttu-id="717ef-149">hello a következő mintakód bemutatja, hogyan toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*, és a javítás URL-cím tooupdate hello végpont.</span><span class="sxs-lookup"><span data-stu-id="717ef-149">hello following sample code shows you how toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*, and PATCH URL tooupdate hello endpoint.</span></span>

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
                        RelativeLocation = "your endpoint relative location", //from hello output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from hello output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
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

<span data-ttu-id="717ef-150">Hello *apiKey* és hello *végponti URL-cím* a hello hívás végpont irányítópult lehet lekérni.</span><span class="sxs-lookup"><span data-stu-id="717ef-150">hello *apiKey* and hello *endpointUrl* for hello call can be obtained from endpoint dashboard.</span></span>

<span data-ttu-id="717ef-151">hello értékének hello *neve* paraméterének *erőforrások* kell hello erőforrás neve egyezik hello mentett Trained Model hello prediktív kísérletté.</span><span class="sxs-lookup"><span data-stu-id="717ef-151">hello value of hello *Name* parameter in *Resources* should match hello Resource Name of hello saved Trained Model in hello predictive experiment.</span></span> <span data-ttu-id="717ef-152">tooget hello erőforrás neve:</span><span class="sxs-lookup"><span data-stu-id="717ef-152">tooget hello Resource Name:</span></span>

1. <span data-ttu-id="717ef-153">Jelentkezzen be toohello [klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="717ef-153">Sign in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="717ef-154">Hello bal oldali menüben kattintson a **Machine Learning**.</span><span class="sxs-lookup"><span data-stu-id="717ef-154">In hello left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="717ef-155">A név, kattintson a munkaterület majd **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="717ef-155">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="717ef-156">A név, kattintson a **nyilvántartásba modell [prediktív exp].** .</span><span class="sxs-lookup"><span data-stu-id="717ef-156">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="717ef-157">Kattintson a hello hozzáadott új végpont.</span><span class="sxs-lookup"><span data-stu-id="717ef-157">Click hello new endpoint you added.</span></span>
6. <span data-ttu-id="717ef-158">Az hello végpont irányítópultján kattintson **frissítés erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="717ef-158">On hello endpoint dashboard, click **Update Resource**.</span></span>
7. <span data-ttu-id="717ef-159">Hello webszolgáltatás hello frissítés erőforrás API-JÁNAK dokumentációja lapján található hello **erőforrásnév** alatt **frissíthető erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="717ef-159">On hello Update Resource API Documentation page for hello web service, you can find hello **Resource Name** under **Updatable Resources**.</span></span>

<span data-ttu-id="717ef-160">Ha befejezte a hello végpont frissítése a SAS-jogkivonat nem, végre kell hajtania a hello feladatazonosító tooobtain friss jogkivonat GET.</span><span class="sxs-lookup"><span data-stu-id="717ef-160">If your SAS token expires before you finish updating hello endpoint, you must perform a GET with hello Job Id tooobtain a fresh token.</span></span>

<span data-ttu-id="717ef-161">Hello kódot már sikeresen futott, amikor új végpont hello kell kezdődnie, körülbelül 30 másodpercet hello retrained modellt használja.</span><span class="sxs-lookup"><span data-stu-id="717ef-161">When hello code has successfully run, hello new endpoint should start using hello retrained model in approximately 30 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="717ef-162">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="717ef-162">Summary</span></span>
<span data-ttu-id="717ef-163">Használatával hello Átképezési API-kat, frissítheti az hello betanított modell egy prediktív webszolgáltatás forgatókönyvek például engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="717ef-163">Using hello Retraining APIs, you can update hello trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="717ef-164">Az új adatokat átképezési rendszeres modell.</span><span class="sxs-lookup"><span data-stu-id="717ef-164">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="717ef-165">A modell toocustomers terjesztési azzal hello céllal, hogy újratanítása hello modell használatával saját adataikat.</span><span class="sxs-lookup"><span data-stu-id="717ef-165">Distribution of a model toocustomers with hello goal of letting them retrain hello model using their own data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="717ef-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="717ef-166">Next steps</span></span>
[<span data-ttu-id="717ef-167">Hibaelhárítás a klasszikus Azure Machine Learning webszolgáltatás átképezési hello</span><span class="sxs-lookup"><span data-stu-id="717ef-167">Troubleshooting hello retraining of an Azure Machine Learning classic web service</span></span>](machine-learning-troubleshooting-retraining-models.md)

