---
title: "A Machine Learning webszolgáltatásba web app sablonnal felhasználása |} Microsoft Docs"
description: "Azure piactér web app sablon segítségével az Azure Machine Learning a prediktív webszolgáltatás felhasználását."
keywords: "a gépi tanulás webszolgáltatás, operationalization, REST API-t"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 95aa1fa23d83ec0dcd00870179167e803bafbd16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a><span data-ttu-id="c5e05-104">Azure Machine Learning webszolgáltatások használata webalkalmazás-sablonnal</span><span class="sxs-lookup"><span data-stu-id="c5e05-104">Consume an Azure Machine Learning web service with a web app template</span></span>

<span data-ttu-id="c5e05-105">Egyszer már a prediktív modell fejlesztett, és telepítve lett webszolgáltatásként az Azure Machine Learning Studio használatával, vagy eszközökkel, például az R vagy Python, hozzáférhet a operationalized modell egy REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="c5e05-105">Once you've developed your predictive model and deployed it as an Azure web service using Machine Learning Studio, or using tools such as R or Python, you can access the operationalized model using a REST API.</span></span>

<span data-ttu-id="c5e05-106">Számos módja a REST API-t használnak, és a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c5e05-106">There are a number of ways to consume the REST API and access the web service.</span></span> <span data-ttu-id="c5e05-107">Például C#, R vagy Python hozta létre a webszolgáltatás üzembe helyezésekor mintakód használatával írhat egy alkalmazás (elérhető a [Machine Learning Web Services portálra](https://services.azureml.net/quickstart) vagy a webes szolgáltatás irányítópultján a Machine Learning Studióban).</span><span class="sxs-lookup"><span data-stu-id="c5e05-107">For example, you can write an application in C#, R, or Python using the sample code generated for you when you deployed the web service (available in the [Machine Learning Web Services Portal](https://services.azureml.net/quickstart) or in the web service dashboard in Machine Learning Studio).</span></span> <span data-ttu-id="c5e05-108">Vagy a minta Microsoft Excel-munkafüzet hozott létre az Ön egyszerre is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c5e05-108">Or you can use the sample Microsoft Excel workbook created for you at the same time.</span></span>

<span data-ttu-id="c5e05-109">A leggyorsabb és legegyszerűbb módja a webes szolgáltatás eléréséhez keresztül elérhető a Web App sablon, de a [Azure Web App piactér](https://azure.microsoft.com/marketplace/web-applications/all/).</span><span class="sxs-lookup"><span data-stu-id="c5e05-109">But the quickest and easiest way to access your web service is through the Web App Templates available in the [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a><span data-ttu-id="c5e05-110">Az Azure Machine Learning Web App sablonok</span><span class="sxs-lookup"><span data-stu-id="c5e05-110">The Azure Machine Learning Web App Templates</span></span>
<span data-ttu-id="c5e05-111">A webes alkalmazás sablonok érhető el az Azure piactéren hozhat létre egy egyéni webalkalmazást, hogy ismeri a webszolgáltatás bemeneti adatok és a kívánt eredmény elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="c5e05-111">The web app templates available in the Azure Marketplace can build a custom web app that knows your web service's input data and expected results.</span></span> <span data-ttu-id="c5e05-112">Mindössze annyit kell tennie hozzáférést a webes alkalmazás a webszolgáltatás és az adatokat, és a sablon elvégzi a többit, azaz.</span><span class="sxs-lookup"><span data-stu-id="c5e05-112">All you need to do is give the web app access to your web service and data, and the template does the rest.</span></span>

<span data-ttu-id="c5e05-113">Két sablonok érhetők el:</span><span class="sxs-lookup"><span data-stu-id="c5e05-113">Two templates are available:</span></span>

* [<span data-ttu-id="c5e05-114">Az Azure ML kérés-válasz szolgáltatás Web App-sablon</span><span class="sxs-lookup"><span data-stu-id="c5e05-114">Azure ML Request-Response Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [<span data-ttu-id="c5e05-115">Azure ML kötegelt végrehajtási szolgáltatás Web App-sablon</span><span class="sxs-lookup"><span data-stu-id="c5e05-115">Azure ML Batch Execution Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

<span data-ttu-id="c5e05-116">Minden sablon létrehoz egy minta ASP.NET alkalmazást, az API-URI és kulcs használatával a webszolgáltatáshoz és az Azure web helyként telepíti.</span><span class="sxs-lookup"><span data-stu-id="c5e05-116">Each template creates a sample ASP.NET application, using the API URI and Key for your web service, and deploys it as a web site to Azure.</span></span> <span data-ttu-id="c5e05-117">A kérés-válasz szolgáltatás (RR-EKET) sablont hoz létre egy webalkalmazást, amely lehetővé teszi a webszolgáltatás számára, hogy egyetlen eredmény egyetlen sornyi adatot küldeni.</span><span class="sxs-lookup"><span data-stu-id="c5e05-117">The Request-Response Service (RRS) template creates a web app that allows you to send a single row of data to the web service to get a single result.</span></span> <span data-ttu-id="c5e05-118">A kötegelt végrehajtási szolgáltatás (BES) sablont hoz létre egy webalkalmazást, amely lehetővé teszi, hogy hány sornyi adatot több lekéréséhez küldhet.</span><span class="sxs-lookup"><span data-stu-id="c5e05-118">The Batch Execution Service (BES) template creates a web app that allows you to send many rows of data to get multiple results.</span></span>

<span data-ttu-id="c5e05-119">Ezek a sablonok használatához nem kódolás szükséges.</span><span class="sxs-lookup"><span data-stu-id="c5e05-119">No coding is required to use these templates.</span></span> <span data-ttu-id="c5e05-120">Csak akkor adja meg, az API-kulcsot és URI, és a sablon épít fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c5e05-120">You just supply the API Key and URI, and the template builds the application for you.</span></span>

<span data-ttu-id="c5e05-121">Az API-kulcs és a kérelem URI-azonosítója lekérése egy webszolgáltatás-bővítmény:</span><span class="sxs-lookup"><span data-stu-id="c5e05-121">To get the API key and Request URI for a web service:</span></span>

1. <span data-ttu-id="c5e05-122">Az a [Web Services portálra](https://services.azureml.net/quickstart), egy új webszolgáltatás kattintson **webszolgáltatások** tetején.</span><span class="sxs-lookup"><span data-stu-id="c5e05-122">In the [Web Services Portal](https://services.azureml.net/quickstart), for a New web service, click **Web Services** at the top.</span></span> <span data-ttu-id="c5e05-123">Vagy a klasszikus web service kattintson **klasszikus webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="c5e05-123">Or for a Classic web service click **Classic Web Services**.</span></span>
2. <span data-ttu-id="c5e05-124">Kattintson a használni kívánt webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c5e05-124">Click the web service you want to access.</span></span>
3. <span data-ttu-id="c5e05-125">Klasszikus webszolgáltatáshoz kattintson a használni kívánt végpont.</span><span class="sxs-lookup"><span data-stu-id="c5e05-125">For a Classic web service, click the endpoint you want to access.</span></span>
4. <span data-ttu-id="c5e05-126">Kattintson a **felhasználás** tetején.</span><span class="sxs-lookup"><span data-stu-id="c5e05-126">Click **Consume** at the top.</span></span>
5. <span data-ttu-id="c5e05-127">Másolás a **elsődleges** vagy **másodlagos kulcs** és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="c5e05-127">Copy the **Primary** or **Secondary Key** and save it.</span></span>
6. <span data-ttu-id="c5e05-128">Ha a kérés-válasz szolgáltatás (RR-EKET) sablont hoz létre, másolja a **kérés-válasz** URI és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="c5e05-128">If you're creating a Request-Response Service (RRS) template, copy the **Request-Response** URI and save it.</span></span> <span data-ttu-id="c5e05-129">Ha egy kötegelt végrehajtási szolgáltatás (BES) sablont hoz létre, másolja a **kötegelt kérelmekben** URI és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="c5e05-129">If you're creating a Batch Execution Service (BES) template, copy the **Batch Requests** URI and save it.</span></span>


## <a name="how-to-use-the-request-response-service-rrs-template"></a><span data-ttu-id="c5e05-130">A kérés-válasz szolgáltatás (RR-EKET) sablon használata</span><span class="sxs-lookup"><span data-stu-id="c5e05-130">How to use the Request-Response Service (RRS) template</span></span>
<span data-ttu-id="c5e05-131">Kövesse az alábbi lépéseket ahhoz, hogy a RR-EKET web app sablon használható az alábbi ábrán is látható.</span><span class="sxs-lookup"><span data-stu-id="c5e05-131">Follow these steps to use the RRS web app template, as shown in the following diagram.</span></span>

![Folyamat RR-EKET webes sablon használata][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. <span data-ttu-id="c5e05-133">Lépjen a [Azure-portálon](https://portal.azure.com), **bejelentkezési**, kattintson **új**, és adja meg **Azure ML kérés-válasz szolgáltatás Web App**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c5e05-133">Go to the [Azure portal](https://portal.azure.com), **Login**, click **New**, search for and select **Azure ML Request-Response Service Web App**, then click **Create**.</span></span> 
   
   * <span data-ttu-id="c5e05-134">Adjon meg egy egyedi nevet a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c5e05-134">Give your web app a unique name.</span></span> <span data-ttu-id="c5e05-135">A webalkalmazás URL-CÍMÉT a nevét, majd lesz `.azurewebsites.net.` például`http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="c5e05-135">The URL of the web app will be this name followed by `.azurewebsites.net.` For example, `http://carprediction.azurewebsites.net.`</span></span>
   * <span data-ttu-id="c5e05-136">Válassza ki az Azure-előfizetés és az alatt a webszolgáltatás fut. szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c5e05-136">Select the Azure subscription and services under which your web service is running.</span></span>
   * <span data-ttu-id="c5e05-137">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c5e05-137">Click **Create**.</span></span>
     
     ![Webalkalmazás létrehozása][image5]

4. <span data-ttu-id="c5e05-139">Ha Azure webalkalmazás telepítése befejeződött, kattintson a **URL-cím** az Azure-ban a webalkalmazás-beállítások lapon, vagy adja meg az URL-címet egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="c5e05-139">When Azure has finished deploying the web app, click the **URL** on the web app settings page in Azure, or enter the URL in a web browser.</span></span> <span data-ttu-id="c5e05-140">Például: `http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="c5e05-140">For example, `http://carprediction.azurewebsites.net.`</span></span>
5. <span data-ttu-id="c5e05-141">A webes alkalmazás első futtatásakor, meg kell adni a **API Post URL-címe** és **API-kulcs**.</span><span class="sxs-lookup"><span data-stu-id="c5e05-141">When the web app first runs it will ask you for the **API Post URL** and **API Key**.</span></span>
   <span data-ttu-id="c5e05-142">Adja meg a korábban mentett értékeket (**kérelem URI-azonosítója** és **API-kulcs**, illetve).</span><span class="sxs-lookup"><span data-stu-id="c5e05-142">Enter the values you saved earlier (**Request URI** and **API Key**, respectively).</span></span>
     
     <span data-ttu-id="c5e05-143">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="c5e05-143">Click **Submit**.</span></span>
     
     ![Adja meg a feladás egy vagy több URI és az API-kulcs][image6]

6. <span data-ttu-id="c5e05-145">A webes alkalmazás megjelenik a **Web App konfigurációs** az aktuális webes szolgáltatás beállításait tartalmazó lapra.</span><span class="sxs-lookup"><span data-stu-id="c5e05-145">The web app displays its **Web App Configuration** page with the current web service settings.</span></span> <span data-ttu-id="c5e05-146">Itt módosíthatja a beállításokat, a webes alkalmazás által használt.</span><span class="sxs-lookup"><span data-stu-id="c5e05-146">Here you can make changes to the settings used by the web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c5e05-147">Itt a szolgáltatás beállításainak módosítása csak akkor változik meg azokat a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c5e05-147">Changing the settings here only changes them for this web app.</span></span> <span data-ttu-id="c5e05-148">Ez az alapértelmezett beállításokat, a webszolgáltatás nem változik.</span><span class="sxs-lookup"><span data-stu-id="c5e05-148">It doesn't change the default settings of your web service.</span></span> <span data-ttu-id="c5e05-149">Például, ha módosítja a **leírás** itt nem módosítja a webes szolgáltatás irányítópultján a Machine Learning Studióban látható leírás.</span><span class="sxs-lookup"><span data-stu-id="c5e05-149">For example, if you change the **Description** here it doesn't change the description shown on the web service dashboard in Machine Learning Studio.</span></span>
   > 
   > 
   
    <span data-ttu-id="c5e05-150">Amikor elkészült, kattintson a **módosítások mentése**, és kattintson a **Ugrás a kezdőlapra**.</span><span class="sxs-lookup"><span data-stu-id="c5e05-150">When you're done, click **Save changes**, and then click **Go to Home Page**.</span></span>

7. <span data-ttu-id="c5e05-151">A kezdőoldalon a webszolgáltatás küldendő értékeket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c5e05-151">From the home page you can enter values to send to your web service.</span></span> <span data-ttu-id="c5e05-152">Kattintson a **Submit** amikor elkészült, és az eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c5e05-152">Click **Submit** when you're done, and the result will be returned.</span></span>

<span data-ttu-id="c5e05-153">Ha vissza szeretne a **konfigurációs** lap megnyitásához válassza a `setting.aspx` a webes alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="c5e05-153">If you want to return to the **Configuration** page, go to the `setting.aspx` page of the web app.</span></span> <span data-ttu-id="c5e05-154">Például: `http://carprediction.azurewebsites.net/setting.aspx.` kérni fogja a írja be újra az API-kulcs - van szüksége, amely a lap eléréséhez, és frissítse a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="c5e05-154">For example: `http://carprediction.azurewebsites.net/setting.aspx.` You will be prompted to enter the API key again - you need that to access the page and update the settings.</span></span>

<span data-ttu-id="c5e05-155">Állítsa le, indítsa újra, vagy törölje a webalkalmazást, mint bármely más webalkalmazás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="c5e05-155">You can stop, restart, or delete the web app in the Azure portal like any other web app.</span></span> <span data-ttu-id="c5e05-156">Mindaddig, amíg fut-e otthoni webcímére navigálhat, és adja meg új értékeket.</span><span class="sxs-lookup"><span data-stu-id="c5e05-156">As long as it is running you can browse to the home web address and enter new values.</span></span>

## <a name="how-to-use-the-batch-execution-service-bes-template"></a><span data-ttu-id="c5e05-157">A kötegelt végrehajtási szolgáltatás (BES) sablon használata</span><span class="sxs-lookup"><span data-stu-id="c5e05-157">How to use the Batch Execution Service (BES) template</span></span>
<span data-ttu-id="c5e05-158">A BES web app sablon használhatja ugyanúgy, mint az RRS sablon, azzal a különbséggel, hogy a létrehozott webalkalmazás lehetővé teszi több sornyi adatot nyújt, és több eredményeket.</span><span class="sxs-lookup"><span data-stu-id="c5e05-158">You can use the BES web app template in the same way as the RRS template, except that the web app that's created will allow you to submit multiple rows of data and receive multiple results.</span></span>

<span data-ttu-id="c5e05-159">A bemeneti értékek egy kötegelt végrehajtási webszolgáltatáshoz származhatnak az Azure storage vagy a helyi fájl; az eredményeket egy az Azure storage-tároló tárolja.</span><span class="sxs-lookup"><span data-stu-id="c5e05-159">The input values for a batch execution web service can come from Azure storage or a local file; the results are stored in an Azure storage container.</span></span>
<span data-ttu-id="c5e05-160">Igen szüksége lesz egy Azure storage tárolót a webes alkalmazás által visszaadott eredményeit, és kész állapotba hozásához a bemeneti adatok lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="c5e05-160">So, you'll need an Azure storage container to hold the results returned by the web app, and you'll need to get your input data ready.</span></span>

![Folyamat BES webes sablon használata][image2]

1. <span data-ttu-id="c5e05-162">Kövesse ugyanezt az eljárást, mint a RR-EKET sablon, kivéve, nyissa meg a BES webalkalmazás létrehozása [Azure ML kötegelt végrehajtási szolgáltatás Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) nyissa meg a BES sablont az Azure piactérről elemre kattintva **webalkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c5e05-162">Follow the same procedure to create the BES web app as for the RRS template, except go to [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) to open the BES template on Azure Marketplace and click **Create Web App**.</span></span>

2. <span data-ttu-id="c5e05-163">Adja meg a tárolt eredményt, írja be a cél tároló adatokat a webes alkalmazás kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="c5e05-163">To specify where you want the results stored, enter the destination container information on the web app home page.</span></span> <span data-ttu-id="c5e05-164">Ha a webes alkalmazás kérheti le a bemenő érték, vagy egy helyi fájl vagy egy Azure storage-tárolót is megadhat.</span><span class="sxs-lookup"><span data-stu-id="c5e05-164">Also specify where the web app can get the input values, either in a local file or an Azure storage container.</span></span>
   <span data-ttu-id="c5e05-165">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="c5e05-165">Click **Submit**.</span></span>
   
    ![Szolgáltatás adatai][image7]

<span data-ttu-id="c5e05-167">A web app egy feladat állapota lapon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c5e05-167">The web app will display a page with job status.</span></span>
<span data-ttu-id="c5e05-168">A feldolgozás befejezése lesz az eredményeket az Azure blob storage helyének adni.</span><span class="sxs-lookup"><span data-stu-id="c5e05-168">When the job has completed you'll be given the location of the results in Azure blob storage.</span></span> <span data-ttu-id="c5e05-169">Lehetősége is van, az eredményeket egy helyi fájl letöltése.</span><span class="sxs-lookup"><span data-stu-id="c5e05-169">You also have the option of downloading the results to a local file.</span></span>

## <a name="for-more-information"></a><span data-ttu-id="c5e05-170">További információ</span><span class="sxs-lookup"><span data-stu-id="c5e05-170">For more information</span></span>
<span data-ttu-id="c5e05-171">További részletek...</span><span class="sxs-lookup"><span data-stu-id="c5e05-171">To learn more about...</span></span>

* <span data-ttu-id="c5e05-172">Tekintse meg a Machine Learning Studio, a gépi tanulási kísérlet létrehozása [az első kísérlet létrehozása az Azure Machine Learning Studióban](machine-learning-create-experiment.md)</span><span class="sxs-lookup"><span data-stu-id="c5e05-172">creating a machine learning experiment with Machine Learning Studio, see [Create your first experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)</span></span>
* <span data-ttu-id="c5e05-173">a gépi tanulási kísérlet egy webszolgáltatás telepítése tudnivalókat [az Azure Machine Learning webszolgáltatás telepítése](machine-learning-publish-a-machine-learning-web-service.md)</span><span class="sxs-lookup"><span data-stu-id="c5e05-173">how to deploy your machine learning experiment as a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md)</span></span>
* <span data-ttu-id="c5e05-174">a webes szolgáltatás más módon lásd [hogyan kell használni az Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="c5e05-174">other ways to access your web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span></span>

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
