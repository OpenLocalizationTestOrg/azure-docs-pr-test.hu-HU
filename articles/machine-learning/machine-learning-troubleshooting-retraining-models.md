---
title: "az Azure Machine Learning klasszikus átképezési aaaTroubleshoot webszolgáltatás |} Microsoft Docs"
description: "Határozza meg és javítsa ki a gyakori problémák közben, amikor az Azure Machine Learning webszolgáltatás vannak mind hello modell."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 2b6a78eaba161877106dccdc23437b5e454fca7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-retraining-of-an-azure-machine-learning-classic-web-service"></a><span data-ttu-id="911fc-103">Az Azure Machine Learning klasszikus webszolgáltatás az átképezési hello hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="911fc-103">Troubleshooting hello retraining of an Azure Machine Learning Classic Web service</span></span>
## <a name="retraining-overview"></a><span data-ttu-id="911fc-104">Megőrzési áttekintése</span><span class="sxs-lookup"><span data-stu-id="911fc-104">Retraining overview</span></span>
<span data-ttu-id="911fc-105">Egy prediktív kísérletté pontozási webszolgáltatásként központi telepítésekor a rendszer egy olyan statikus modell.</span><span class="sxs-lookup"><span data-stu-id="911fc-105">When you deploy a predictive experiment as a scoring web service it is a static model.</span></span> <span data-ttu-id="911fc-106">Amint az elérhetővé válik az új adatok, vagy ha hello felhasználóinak azon hello API rendelkezik saját adataikat, hello modellnek támogatnia kell a toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="911fc-106">As new data becomes available or when hello consumer of hello API has their own data, hello model needs toobe retrained.</span></span> 

<span data-ttu-id="911fc-107">A folyamat egy klasszikus webszolgáltatás átképezési hello részletes útmutatást lásd: [újratanítása Machine Learning modellek szoftveres](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="911fc-107">For a complete walkthrough of hello retraining process of a Classic Web service, see [Retrain Machine Learning Models Programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

## <a name="retraining-process"></a><span data-ttu-id="911fc-108">Megőrzési folyamat</span><span class="sxs-lookup"><span data-stu-id="911fc-108">Retraining process</span></span>
<span data-ttu-id="911fc-109">Ha tooretrain hello webszolgáltatás van szüksége, hozzá kell adnia néhány további eleme:</span><span class="sxs-lookup"><span data-stu-id="911fc-109">When you need tooretrain hello Web service, you must add some additional pieces:</span></span>

* <span data-ttu-id="911fc-110">A webszolgáltatás telepítve hello tanítási kísérletet.</span><span class="sxs-lookup"><span data-stu-id="911fc-110">A Web service deployed from hello Training Experiment.</span></span> <span data-ttu-id="911fc-111">hello kísérlet rendelkeznie kell egy **webes szolgáltatás kimeneti** modul csatlakoztatva hello toohello kimenete **tanítási modell** modul.</span><span class="sxs-lookup"><span data-stu-id="911fc-111">hello experiment must have a **Web Service Output** module attached toohello output of hello **Train Model** module.</span></span>  
  
    ![Hello webes szolgáltatás kimeneti toohello tanítási modell csatolni.][image1]
* <span data-ttu-id="911fc-113">Új végpont webszolgáltatás pontozási tooyour hozzá.</span><span class="sxs-lookup"><span data-stu-id="911fc-113">A new endpoint added tooyour scoring Web service.</span></span>  <span data-ttu-id="911fc-114">Programozott módon adhat hozzá hello végpont használatával hello hivatkozott hello mintakód Machine Learning-modellek szoftveres témakör vagy hello a klasszikus Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="911fc-114">You can add hello endpoint programmatically using hello sample code referenced in hello Retrain Machine Learning models programmatically topic or through hello Azure classic portal.</span></span>

<span data-ttu-id="911fc-115">Hello C# mintakód hello képzési Web Service API Súgó lap tooretrain modellből használhatja.</span><span class="sxs-lookup"><span data-stu-id="911fc-115">You can then use hello sample C# code from hello Training Web Service's API help page tooretrain model.</span></span> <span data-ttu-id="911fc-116">Miután kiértékelték hello eredményeit, és elégedett őket, hello betanított modell hello új végpont hozzáadott webszolgáltatás pontozási frissítenie.</span><span class="sxs-lookup"><span data-stu-id="911fc-116">Once you have evaluated hello results and are satisfied with them, you update hello trained model scoring web service using hello new endpoint that you added.</span></span>

<span data-ttu-id="911fc-117">A hely összes hello darabot hello fő lépést kell végrehajtania, tooretrain hello modell a következők:</span><span class="sxs-lookup"><span data-stu-id="911fc-117">With all hello pieces in place, hello major steps you must take tooretrain hello model are as follows:</span></span>

1. <span data-ttu-id="911fc-118">Hello képzési webes szolgáltatás hívása: hello hívás toohello kötegelt végrehajtási szolgáltatás (BES), nem hello kérelem válasz szolgáltatás (RR-EKET).</span><span class="sxs-lookup"><span data-stu-id="911fc-118">Call hello Training Web Service:  hello call is toohello Batch Execution Service (BES), not hello Request Response Service (RRS).</span></span> <span data-ttu-id="911fc-119">Hello példakód C# hello API Súgó lap toomake hello hívásakor használható.</span><span class="sxs-lookup"><span data-stu-id="911fc-119">You can use hello sample C# code on hello API help page toomake hello call.</span></span> 
2. <span data-ttu-id="911fc-120">Hello értékek keresése hello *BaseLocation*, *RelativeLocation*, és *SasBlobToken*: ezeket az értékeket ad vissza a hello kimeneti a hívás toohello képzési webes A szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="911fc-120">Find hello values for hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken*: These values are returned in hello output from your call toohello Training Web Service.</span></span> 
   <span data-ttu-id="911fc-121">![jelenít meg a minta és az hello BaseLocation RelativeLocation és SasBlobToken értékek átképezési hello hello kimenetét.][image6]</span><span class="sxs-lookup"><span data-stu-id="911fc-121">![showing hello output of hello retraining sample and hello BaseLocation, RelativeLocation, and  SasBlobToken values.][image6]</span></span>
3. <span data-ttu-id="911fc-122">Frissítés hello hozzáadni az új pontozási hello webszolgáltatás hello végpont modell betanítása: hello mintakód használatával megadott hello újratanítása Machine Learning modellek szoftveres, frissítse hello új végpont toohello hello modell pontozása újonnan hozzáadott betanított modell a tanítási webszolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="911fc-122">Update hello added endpoint from hello scoring web service with hello new trained model: Using hello sample code provided in hello Retrain Machine Learning models programmatically, update hello new endpoint you added toohello scoring model with hello newly trained model from hello Training Web Service.</span></span>

## <a name="common-obstacles"></a><span data-ttu-id="911fc-123">Közös akadályok</span><span class="sxs-lookup"><span data-stu-id="911fc-123">Common obstacles</span></span>
### <a name="check-toosee-if-you-have-hello-correct-patch-url"></a><span data-ttu-id="911fc-124">Toosee ellenőrizze, hogy javítsa ki az URL-cím javítás hello rendelkezik</span><span class="sxs-lookup"><span data-stu-id="911fc-124">Check toosee if you have hello correct PATCH URL</span></span>
<span data-ttu-id="911fc-125">hello javítás URL-címet használ kell lennie egy hello társított hello webszolgáltatás pontozási toohello hozzá új scoring-végpontja.</span><span class="sxs-lookup"><span data-stu-id="911fc-125">hello PATCH URL you are using must be hello one associated with hello new scoring endpoint you added toohello scoring Web service.</span></span> <span data-ttu-id="911fc-126">Számos módon tooobtain hello javítás URL-címe:</span><span class="sxs-lookup"><span data-stu-id="911fc-126">There are a number of ways tooobtain hello PATCH URL:</span></span>

<span data-ttu-id="911fc-127">**1. lehetőség: programozottan**</span><span class="sxs-lookup"><span data-stu-id="911fc-127">**Option 1: Programatically**</span></span>

<span data-ttu-id="911fc-128">tooget hello javítsa ki a javítás URL-címe:</span><span class="sxs-lookup"><span data-stu-id="911fc-128">tooget hello correct PATCH URL:</span></span>

1. <span data-ttu-id="911fc-129">Futtassa a hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) példakód.</span><span class="sxs-lookup"><span data-stu-id="911fc-129">Run hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>
2. <span data-ttu-id="911fc-130">A AddEndpoint hello kimenetből megkeresi a hello *HelpLocation* értékét, és másolja hello URL-címet.</span><span class="sxs-lookup"><span data-stu-id="911fc-130">From hello output of AddEndpoint, find hello *HelpLocation* value and copy hello URL.</span></span>
   
   ![HelpLocation hello kimenet hello addEndpoint minta.][image2]
3. <span data-ttu-id="911fc-132">Hello URL-cím illessze be egy böngésző toonavigate tooa lap, amely súgóhivatkozások biztosít hello webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="911fc-132">Paste hello URL into a browser toonavigate tooa page that provides help links for hello Web service.</span></span>
4. <span data-ttu-id="911fc-133">Kattintson a hello **frissítés erőforrás** hivatkozás tooopen hello javítás súgólap.</span><span class="sxs-lookup"><span data-stu-id="911fc-133">Click hello **Update Resource** link tooopen hello patch help page.</span></span>

<span data-ttu-id="911fc-134">**2. lehetőség: Hello klasszikus Azure portál használata**</span><span class="sxs-lookup"><span data-stu-id="911fc-134">**Option 2: Use hello Azure classic portal**</span></span>

1. <span data-ttu-id="911fc-135">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="911fc-135">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="911fc-136">Nyissa meg hello Machine Learning fülre. ![Gép leaning lapján.][image4]</span><span class="sxs-lookup"><span data-stu-id="911fc-136">Open hello Machine Learning tab. ![Machine leaning tab.][image4]</span></span>
3. <span data-ttu-id="911fc-137">A munkaterület neve, majd kattintson **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="911fc-137">Click your workspace name, then **Web Services**.</span></span>
4. <span data-ttu-id="911fc-138">Kattintson a hello pontozási webszolgáltatás dolgozik.</span><span class="sxs-lookup"><span data-stu-id="911fc-138">Click hello scoring Web service you are working with.</span></span> <span data-ttu-id="911fc-139">(Ha Ön nem módosította a következő hello webszolgáltatás hello alapértelmezett nevét, akkor lejár [pontozási Exp.].)</span><span class="sxs-lookup"><span data-stu-id="911fc-139">(If you did not modify hello default name of hello web service, it will end in [Scoring Exp.].)</span></span>
5. <span data-ttu-id="911fc-140">Kattintson a **végpont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="911fc-140">Click **Add Endpoint**.</span></span>
6. <span data-ttu-id="911fc-141">Miután hello végpontot hoz létre, kattintson a hello végpont nevét.</span><span class="sxs-lookup"><span data-stu-id="911fc-141">After hello endpoint is added, click hello endpoint name.</span></span> <span data-ttu-id="911fc-142">Kattintson a **frissítés erőforrás** tooopen hello javítási súgólap.</span><span class="sxs-lookup"><span data-stu-id="911fc-142">Then click **Update Resource** tooopen hello patching help page.</span></span>

> [!NOTE]
> <span data-ttu-id="911fc-143">Hello végpont toohello képzési webszolgáltatás helyett hello prediktív webszolgáltatás felvett, kapni fog a hello hello kattintva a következő hiba **frissítés erőforrás** hivatkozás: Sajnáljuk, ez a funkció nem támogatott, de vagy érhető el ebben a környezetben.</span><span class="sxs-lookup"><span data-stu-id="911fc-143">If you have added hello endpoint toohello Training Web Service instead of hello Predictive Web Service, you will receive hello following error when you click hello **Update Resource** link: Sorry, but this feature is not supported or available in this context.</span></span> <span data-ttu-id="911fc-144">Ez a webszolgáltatás nem frissíthető erőforrással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="911fc-144">This Web Service has no updatable resources.</span></span> <span data-ttu-id="911fc-145">A Microsoft hello kellemetlenségért elnézését kérjük, és javítása a munkafolyamat dolgozik.</span><span class="sxs-lookup"><span data-stu-id="911fc-145">We apologize for hello inconvenience and are working on improving this workflow.</span></span>
> 
> 

![Új endpoint irányítópult.][image3]

<span data-ttu-id="911fc-147">hello javítás súgólap tartalmaz hello javítás URL-címet kell használnia, és használhatja példakódot tartalmaz toocall azt.</span><span class="sxs-lookup"><span data-stu-id="911fc-147">hello PATCH help page contains hello PATCH URL you must use and provides sample code you can use toocall it.</span></span>

![Javítás URL-címe.][image5]

### <a name="check-toosee-that-you-are-updating-hello-correct-scoring-endpoint"></a><span data-ttu-id="911fc-149">Ellenőrizze, hogy a frissítő hello megfelelő pontozási végpont toosee</span><span class="sxs-lookup"><span data-stu-id="911fc-149">Check toosee that you are updating hello correct scoring endpoint</span></span>
* <span data-ttu-id="911fc-150">A javítás nem hello képzési webszolgáltatás: hello webszolgáltatás pontozási hello javítási műveletet kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="911fc-150">Do not patch hello Training Web Service: hello patch operation must be performed on hello scoring Web service.</span></span>
* <span data-ttu-id="911fc-151">A javítás nem hello alapértelmezett végpont a webszolgáltatás: hello webszolgáltatási végpontot hozzáadott új pontozási hello javítási műveletet kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="911fc-151">Do not patch hello default endpoint on Web service: hello patch operation must be performed on hello new scoring Web service endpoint that you added.</span></span>

<span data-ttu-id="911fc-152">Ellenőrizheti, hogy melyik webes szolgáltatásvégpont hello szerint be kapcsolva látogató hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="911fc-152">You can verify which Web service hello endpoint is on by visiting hello Azure classic portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="911fc-153">Győződjön meg arról, hogy hello végpont toohello prediktív webes szolgáltatás, a képzési webszolgáltatás hello nem ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="911fc-153">Be sure you are adding hello endpoint toohello Predictive Web Service, not hello Training Web Service.</span></span> <span data-ttu-id="911fc-154">Ha megfelelően telepítette, a képzési és a prediktív webszolgáltatás, megtekintheti az felsorolt két külön webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="911fc-154">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate Web services listed.</span></span> <span data-ttu-id="911fc-155">"[prediktív exp.]" Hello prediktív webszolgáltatás kell végződnie.</span><span class="sxs-lookup"><span data-stu-id="911fc-155">hello Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

1. <span data-ttu-id="911fc-156">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="911fc-156">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="911fc-157">Nyissa meg hello Machine Learning fülre. ![Machine learning-munkaterület felhasználói felületén.][image4]</span><span class="sxs-lookup"><span data-stu-id="911fc-157">Open hello Machine Learning tab. ![Machine learning workspace UI.][image4]</span></span>
3. <span data-ttu-id="911fc-158">A munkaterület kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="911fc-158">Select your workspace.</span></span>
4. <span data-ttu-id="911fc-159">Kattintson a **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="911fc-159">Click **Web Services**.</span></span>
5. <span data-ttu-id="911fc-160">Válassza ki a prediktív webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="911fc-160">Select your Predictive Web Service.</span></span>
6. <span data-ttu-id="911fc-161">Győződjön meg arról, hogy az új végpont toohello webszolgáltatás lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="911fc-161">Verify that your new endpoint was added toohello Web service.</span></span>

### <a name="check-hello-workspace-that-your-web-service-is-in-tooensure-it-is-in-hello-correct-region"></a><span data-ttu-id="911fc-162">Ellenőrizze, hogy a webszolgáltatás megtalálható-e megfelelő régióban található hello tooensure hello munkaterület</span><span class="sxs-lookup"><span data-stu-id="911fc-162">Check hello workspace that your web service is in tooensure it is in hello correct region</span></span>
1. <span data-ttu-id="911fc-163">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="911fc-163">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="911fc-164">Válassza ki a Machine Learning hello menüből.</span><span class="sxs-lookup"><span data-stu-id="911fc-164">Select Machine Learning from hello menu.</span></span>
   <span data-ttu-id="911fc-165">![Machine learning régió felhasználói felületén.][image4]</span><span class="sxs-lookup"><span data-stu-id="911fc-165">![Machine learning region UI.][image4]</span></span>
3. <span data-ttu-id="911fc-166">Ellenőrizze a munkaterület hello helyét.</span><span class="sxs-lookup"><span data-stu-id="911fc-166">Verify hello location of your workspace.</span></span>

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
