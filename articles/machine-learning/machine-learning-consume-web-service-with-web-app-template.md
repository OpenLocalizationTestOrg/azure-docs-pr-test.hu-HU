---
title: "a Machine Learning webszolgáltatásba web app sablonnal aaaConsume |} Microsoft Docs"
description: "Web app sablon használata a Azure piactérről tooconsume az Azure Machine Learning a prediktív webes szolgáltatás."
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
ms.openlocfilehash: 1199377bead470807d58ca7f7a667175cbb88450
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a><span data-ttu-id="d3c54-104">Azure Machine Learning webszolgáltatások használata webalkalmazás-sablonnal</span><span class="sxs-lookup"><span data-stu-id="d3c54-104">Consume an Azure Machine Learning web service with a web app template</span></span>

<span data-ttu-id="d3c54-105">Egyszer már a prediktív modell fejlesztett, és telepítve lett webszolgáltatásként az Azure Machine Learning Studio használatával, vagy használja az eszközök, például az R vagy Python, van-e hozzáférési hello operationalized modell egy REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="d3c54-105">Once you've developed your predictive model and deployed it as an Azure web service using Machine Learning Studio, or using tools such as R or Python, you can access hello operationalized model using a REST API.</span></span>

<span data-ttu-id="d3c54-106">Számos módon tooconsume hello REST API-t és hello webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d3c54-106">There are a number of ways tooconsume hello REST API and access hello web service.</span></span> <span data-ttu-id="d3c54-107">Például egy alkalmazás írhatnak C# nyelven íródtak, R, vagy Python hello segítségével példakód létrehozza a hello webszolgáltatás üzembe helyezésekor (hello elérhető [Machine Learning Web Services portálra](https://services.azureml.net/quickstart) vagy hello web service irányítópult A Machine Learning Studio).</span><span class="sxs-lookup"><span data-stu-id="d3c54-107">For example, you can write an application in C#, R, or Python using hello sample code generated for you when you deployed hello web service (available in hello [Machine Learning Web Services Portal](https://services.azureml.net/quickstart) or in hello web service dashboard in Machine Learning Studio).</span></span> <span data-ttu-id="d3c54-108">Hello Microsoft Excel-munkafüzet minta létrehozza a hello, vagy ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d3c54-108">Or you can use hello sample Microsoft Excel workbook created for you at hello same time.</span></span>

<span data-ttu-id="d3c54-109">De hello Web App hello elérhető sablonok keresztül van a webszolgáltatás leggyorsabb és legegyszerűbb módja tooaccess hello [Azure Web App piactér](https://azure.microsoft.com/marketplace/web-applications/all/).</span><span class="sxs-lookup"><span data-stu-id="d3c54-109">But hello quickest and easiest way tooaccess your web service is through hello Web App Templates available in hello [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-azure-machine-learning-web-app-templates"></a><span data-ttu-id="d3c54-110">hello Azure Machine Learning Web App sablonok</span><span class="sxs-lookup"><span data-stu-id="d3c54-110">hello Azure Machine Learning Web App Templates</span></span>
<span data-ttu-id="d3c54-111">hello web app hello Azure piactéren elérhető sablonok hozhat létre egy egyéni webalkalmazást, hogy ismeri a webszolgáltatás bemeneti adatok és a kívánt eredmény elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="d3c54-111">hello web app templates available in hello Azure Marketplace can build a custom web app that knows your web service's input data and expected results.</span></span> <span data-ttu-id="d3c54-112">Mindössze annyit kell toodo hello web access tooyour webszolgáltatás és az adatokat, és hello sablon hello rest.</span><span class="sxs-lookup"><span data-stu-id="d3c54-112">All you need toodo is give hello web app access tooyour web service and data, and hello template does hello rest.</span></span>

<span data-ttu-id="d3c54-113">Két sablonok érhetők el:</span><span class="sxs-lookup"><span data-stu-id="d3c54-113">Two templates are available:</span></span>

* [<span data-ttu-id="d3c54-114">Az Azure ML kérés-válasz szolgáltatás Web App-sablon</span><span class="sxs-lookup"><span data-stu-id="d3c54-114">Azure ML Request-Response Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [<span data-ttu-id="d3c54-115">Azure ML kötegelt végrehajtási szolgáltatás Web App-sablon</span><span class="sxs-lookup"><span data-stu-id="d3c54-115">Azure ML Batch Execution Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

<span data-ttu-id="d3c54-116">Minden sablon egy minta ASP.NET alkalmazást, a webszolgáltatás hello API URI és kulcs használatával hoz létre, és telepíti, a webhely tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d3c54-116">Each template creates a sample ASP.NET application, using hello API URI and Key for your web service, and deploys it as a web site tooAzure.</span></span> <span data-ttu-id="d3c54-117">hello kérés-válasz szolgáltatás (RR-EKET) sablon létrehoz egy webalkalmazást, amely lehetővé teszi az adatok toohello web service tooget egyetlen eredmény egyetlen sor toosend.</span><span class="sxs-lookup"><span data-stu-id="d3c54-117">hello Request-Response Service (RRS) template creates a web app that allows you toosend a single row of data toohello web service tooget a single result.</span></span> <span data-ttu-id="d3c54-118">hello kötegelt végrehajtási szolgáltatás (BES) sablon létrehoz egy webalkalmazást, amely lehetővé teszi toosend adatok tooget hány sornyi több eredményt.</span><span class="sxs-lookup"><span data-stu-id="d3c54-118">hello Batch Execution Service (BES) template creates a web app that allows you toosend many rows of data tooget multiple results.</span></span>

<span data-ttu-id="d3c54-119">Nincs kódolási van szükség toouse ezeket a sablonokat.</span><span class="sxs-lookup"><span data-stu-id="d3c54-119">No coding is required toouse these templates.</span></span> <span data-ttu-id="d3c54-120">Csak akkor adja meg, hello API-kulcsot és URI-t, és hello sablon buildek hello alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="d3c54-120">You just supply hello API Key and URI, and hello template builds hello application for you.</span></span>

<span data-ttu-id="d3c54-121">tooget hello API-kulcs és a kérelem URI-azonosítója az adott webszolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="d3c54-121">tooget hello API key and Request URI for a web service:</span></span>

1. <span data-ttu-id="d3c54-122">A hello [Web Services portálra](https://services.azureml.net/quickstart), egy új webszolgáltatás kattintson **webszolgáltatások** hello tetején.</span><span class="sxs-lookup"><span data-stu-id="d3c54-122">In hello [Web Services Portal](https://services.azureml.net/quickstart), for a New web service, click **Web Services** at hello top.</span></span> <span data-ttu-id="d3c54-123">Vagy a klasszikus web service kattintson **klasszikus webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="d3c54-123">Or for a Classic web service click **Classic Web Services**.</span></span>
2. <span data-ttu-id="d3c54-124">Kattintson a kívánt tooaccess hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="d3c54-124">Click hello web service you want tooaccess.</span></span>
3. <span data-ttu-id="d3c54-125">Klasszikus webszolgáltatáshoz kattintson a kívánt tooaccess hello végpont.</span><span class="sxs-lookup"><span data-stu-id="d3c54-125">For a Classic web service, click hello endpoint you want tooaccess.</span></span>
4. <span data-ttu-id="d3c54-126">Kattintson a **felhasználás** hello tetején.</span><span class="sxs-lookup"><span data-stu-id="d3c54-126">Click **Consume** at hello top.</span></span>
5. <span data-ttu-id="d3c54-127">Másolás hello **elsődleges** vagy **másodlagos kulcs** és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="d3c54-127">Copy hello **Primary** or **Secondary Key** and save it.</span></span>
6. <span data-ttu-id="d3c54-128">A kérés-válasz szolgáltatás (RR-EKET) sablon létrehozásakor, másolja a hello **kérés-válasz** URI és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="d3c54-128">If you're creating a Request-Response Service (RRS) template, copy hello **Request-Response** URI and save it.</span></span> <span data-ttu-id="d3c54-129">Ha egy kötegelt végrehajtási szolgáltatás (BES) sablont hoz létre, másolja a hello **kötegelt kérelmekben** URI és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="d3c54-129">If you're creating a Batch Execution Service (BES) template, copy hello **Batch Requests** URI and save it.</span></span>


## <a name="how-toouse-hello-request-response-service-rrs-template"></a><span data-ttu-id="d3c54-130">Hogyan toouse hello kérés-válasz szolgáltatás (RR-EKET) sablon</span><span class="sxs-lookup"><span data-stu-id="d3c54-130">How toouse hello Request-Response Service (RRS) template</span></span>
<span data-ttu-id="d3c54-131">Kövesse ezeket lépéseket toouse hello RR-EKET web app-sablon, a hello a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="d3c54-131">Follow these steps toouse hello RRS web app template, as shown in hello following diagram.</span></span>

![Folyamat toouse RRS webes sablon][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. <span data-ttu-id="d3c54-133">Nyissa meg toohello [Azure-portálon](https://portal.azure.com), **bejelentkezési**, kattintson **új**, és adja meg **Azure ML kérés-válasz szolgáltatás Web App**, kattintson a **Létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d3c54-133">Go toohello [Azure portal](https://portal.azure.com), **Login**, click **New**, search for and select **Azure ML Request-Response Service Web App**, then click **Create**.</span></span> 
   
   * <span data-ttu-id="d3c54-134">Adjon meg egy egyedi nevet a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d3c54-134">Give your web app a unique name.</span></span> <span data-ttu-id="d3c54-135">hello webalkalmazás URL-címe hello lesz a nevét, majd `.azurewebsites.net.` például`http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="d3c54-135">hello URL of hello web app will be this name followed by `.azurewebsites.net.` For example, `http://carprediction.azurewebsites.net.`</span></span>
   * <span data-ttu-id="d3c54-136">Válassza ki a hello Azure-előfizetés és a szolgáltatások alatt a webszolgáltatás fut..</span><span class="sxs-lookup"><span data-stu-id="d3c54-136">Select hello Azure subscription and services under which your web service is running.</span></span>
   * <span data-ttu-id="d3c54-137">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d3c54-137">Click **Create**.</span></span>
     
     ![Webalkalmazás létrehozása][image5]

4. <span data-ttu-id="d3c54-139">Ha Azure hello webalkalmazás telepítése befejeződött, kattintson a hello **URL-cím** a webes alkalmazás fiókbeállítási oldalára Azure hello, vagy meg hello URL-címet egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="d3c54-139">When Azure has finished deploying hello web app, click hello **URL** on hello web app settings page in Azure, or enter hello URL in a web browser.</span></span> <span data-ttu-id="d3c54-140">Például: `http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="d3c54-140">For example, `http://carprediction.azurewebsites.net.`</span></span>
5. <span data-ttu-id="d3c54-141">Ha a webes alkalmazás első futtatásakor, ekkor megkérdezi, hogy a hello hello **API Post URL-címe** és **API-kulcs**.</span><span class="sxs-lookup"><span data-stu-id="d3c54-141">When hello web app first runs it will ask you for hello **API Post URL** and **API Key**.</span></span>
   <span data-ttu-id="d3c54-142">Adja meg a korábban mentett hello értékek (**kérelem URI-azonosítója** és **API-kulcs**, illetve).</span><span class="sxs-lookup"><span data-stu-id="d3c54-142">Enter hello values you saved earlier (**Request URI** and **API Key**, respectively).</span></span>
     
     <span data-ttu-id="d3c54-143">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="d3c54-143">Click **Submit**.</span></span>
     
     ![Adja meg a feladás egy vagy több URI és az API-kulcs][image6]

6. <span data-ttu-id="d3c54-145">hello webes alkalmazás megjelenik a **Web App konfigurációs** hello aktuális webszolgáltatások beállításai lapon.</span><span class="sxs-lookup"><span data-stu-id="d3c54-145">hello web app displays its **Web App Configuration** page with hello current web service settings.</span></span> <span data-ttu-id="d3c54-146">Itt módosíthatja hello webes alkalmazás által használt toohello beállításokat.</span><span class="sxs-lookup"><span data-stu-id="d3c54-146">Here you can make changes toohello settings used by hello web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d3c54-147">Itt hello szolgáltatás beállításainak módosítása csak akkor változik meg azokat a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d3c54-147">Changing hello settings here only changes them for this web app.</span></span> <span data-ttu-id="d3c54-148">Hello alapértelmezett beállítások a webszolgáltatás nem módosítja.</span><span class="sxs-lookup"><span data-stu-id="d3c54-148">It doesn't change hello default settings of your web service.</span></span> <span data-ttu-id="d3c54-149">Például, ha módosítja a hello **leírás** itt látható hello webes szolgáltatás irányítópultján a Machine Learning Studióban hello leírás nem módosítja.</span><span class="sxs-lookup"><span data-stu-id="d3c54-149">For example, if you change hello **Description** here it doesn't change hello description shown on hello web service dashboard in Machine Learning Studio.</span></span>
   > 
   > 
   
    <span data-ttu-id="d3c54-150">Amikor elkészült, kattintson a **módosítások mentése**, és kattintson a **tooHome lapon lépjen**.</span><span class="sxs-lookup"><span data-stu-id="d3c54-150">When you're done, click **Save changes**, and then click **Go tooHome Page**.</span></span>

7. <span data-ttu-id="d3c54-151">A hello kezdőlap adhatja értékek toosend tooyour webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d3c54-151">From hello home page you can enter values toosend tooyour web service.</span></span> <span data-ttu-id="d3c54-152">Kattintson a **Submit** amikor elkészült, és hello eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d3c54-152">Click **Submit** when you're done, and hello result will be returned.</span></span>

<span data-ttu-id="d3c54-153">Ha azt szeretné, hogy tooreturn toohello **konfigurációs** lapon, nyissa meg toohello `setting.aspx` hello webalkalmazás oldalán.</span><span class="sxs-lookup"><span data-stu-id="d3c54-153">If you want tooreturn toohello **Configuration** page, go toohello `setting.aspx` page of hello web app.</span></span> <span data-ttu-id="d3c54-154">Például: `http://carprediction.azurewebsites.net/setting.aspx.` újra lesz felszólító tooenter hello API-kulcs –, hogy tooaccess hello lapot, és hello-beállítások frissítése van szükség.</span><span class="sxs-lookup"><span data-stu-id="d3c54-154">For example: `http://carprediction.azurewebsites.net/setting.aspx.` You will be prompted tooenter hello API key again - you need that tooaccess hello page and update hello settings.</span></span>

<span data-ttu-id="d3c54-155">Állítsa le, indítsa újra, vagy az Azure portálon, mint bármely más webalkalmazás hello hello a webalkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="d3c54-155">You can stop, restart, or delete hello web app in hello Azure portal like any other web app.</span></span> <span data-ttu-id="d3c54-156">Mindaddig, amíg fut-e otthoni webcímet toohello navigálhat, és adja meg új értékeket.</span><span class="sxs-lookup"><span data-stu-id="d3c54-156">As long as it is running you can browse toohello home web address and enter new values.</span></span>

## <a name="how-toouse-hello-batch-execution-service-bes-template"></a><span data-ttu-id="d3c54-157">Hogyan toouse hello kötegelt végrehajtási szolgáltatás (BES) sablon</span><span class="sxs-lookup"><span data-stu-id="d3c54-157">How toouse hello Batch Execution Service (BES) template</span></span>
<span data-ttu-id="d3c54-158">Használhatja a hello BES azonos módon-sablonként hello RR-EKET, kivéve létrehozott hello webes alkalmazást hello web app-sablon lehetővé teszi a toosubmit több sornyi adatot és több eredményeket.</span><span class="sxs-lookup"><span data-stu-id="d3c54-158">You can use hello BES web app template in hello same way as hello RRS template, except that hello web app that's created will allow you toosubmit multiple rows of data and receive multiple results.</span></span>

<span data-ttu-id="d3c54-159">hello bemeneti értékeket a kötegelt végrehajtási webszolgáltatáshoz származhatnak az Azure storage vagy a helyi fájl; hello eredményeket egy az Azure storage-tároló tárolja.</span><span class="sxs-lookup"><span data-stu-id="d3c54-159">hello input values for a batch execution web service can come from Azure storage or a local file; hello results are stored in an Azure storage container.</span></span>
<span data-ttu-id="d3c54-160">Igen, akkor lesz szüksége egy Azure storage-tároló toohold hello hello webalkalmazás által visszaadott eredmények, és frissítenie kell tooget a bemeneti adatok készen áll.</span><span class="sxs-lookup"><span data-stu-id="d3c54-160">So, you'll need an Azure storage container toohold hello results returned by hello web app, and you'll need tooget your input data ready.</span></span>

![Toouse BES feldolgozni webes sablon][image2]

1. <span data-ttu-id="d3c54-162">Hajtsa végre az azonos hello eljárás toocreate hello BES webalkalmazás mint hello RR-EKET sablon, kivéve Ugrás túl[Azure ML kötegelt végrehajtási szolgáltatás Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES sablon Azure piactéren, és kattintson a **webalkalmazás létrehozása** .</span><span class="sxs-lookup"><span data-stu-id="d3c54-162">Follow hello same procedure toocreate hello BES web app as for hello RRS template, except go too[Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES template on Azure Marketplace and click **Create Web App**.</span></span>

2. <span data-ttu-id="d3c54-163">hello eredmények tárolt, ahová toospecify hello cél tároló adatokat be hello web app kezdőlap.</span><span class="sxs-lookup"><span data-stu-id="d3c54-163">toospecify where you want hello results stored, enter hello destination container information on hello web app home page.</span></span> <span data-ttu-id="d3c54-164">Ha hello webalkalmazás kérheti le a hello bemeneti értékeket, vagy egy helyi fájl vagy egy Azure storage-tárolót is megadhat.</span><span class="sxs-lookup"><span data-stu-id="d3c54-164">Also specify where hello web app can get hello input values, either in a local file or an Azure storage container.</span></span>
   <span data-ttu-id="d3c54-165">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="d3c54-165">Click **Submit**.</span></span>
   
    ![Szolgáltatás adatai][image7]

<span data-ttu-id="d3c54-167">hello web app egy feladat állapota lapon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d3c54-167">hello web app will display a page with job status.</span></span>
<span data-ttu-id="d3c54-168">Hello feladat befejezése fogja az Azure blob storage hello eredmények hello helyét adni.</span><span class="sxs-lookup"><span data-stu-id="d3c54-168">When hello job has completed you'll be given hello location of hello results in Azure blob storage.</span></span> <span data-ttu-id="d3c54-169">Akkor is hello eredmények tooa helyi fájl letöltése hello lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="d3c54-169">You also have hello option of downloading hello results tooa local file.</span></span>

## <a name="for-more-information"></a><span data-ttu-id="d3c54-170">További információ</span><span class="sxs-lookup"><span data-stu-id="d3c54-170">For more information</span></span>
<span data-ttu-id="d3c54-171">További információk toolearn...</span><span class="sxs-lookup"><span data-stu-id="d3c54-171">toolearn more about...</span></span>

* <span data-ttu-id="d3c54-172">Tekintse meg a Machine Learning Studio, a gépi tanulási kísérlet létrehozása [az első kísérlet létrehozása az Azure Machine Learning Studióban](machine-learning-create-experiment.md)</span><span class="sxs-lookup"><span data-stu-id="d3c54-172">creating a machine learning experiment with Machine Learning Studio, see [Create your first experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)</span></span>
* <span data-ttu-id="d3c54-173">Hogyan a machine learning webszolgáltatásként, kísérletezhet toodeploy: [az Azure Machine Learning webszolgáltatás telepítése](machine-learning-publish-a-machine-learning-web-service.md)</span><span class="sxs-lookup"><span data-stu-id="d3c54-173">how toodeploy your machine learning experiment as a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md)</span></span>
* <span data-ttu-id="d3c54-174">más módokon tooaccess a webszolgáltatás, lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="d3c54-174">other ways tooaccess your web service, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span></span>

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
