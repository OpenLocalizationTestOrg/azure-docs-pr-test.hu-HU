---
title: "Machine Learning webszolgáltatás aaaConnect tooa |} Microsoft Docs"
description: "C# vagy Python csatlakoztassa a hitelesítési kulcs használata Azure Machine Learning Web service tooan."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 59b07bab-b60f-48c4-a385-a162e50ec7c2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: garye
ROBOTS: NOINDEX
redirect_url: machine-learning-consume-web-services
redirect_document_id: True
ms.openlocfilehash: 0108e71e30a05539a8c0ee93d5aadb07e3d1efa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-machine-learning-web-service"></a><span data-ttu-id="767a2-103">Csatlakozás az Azure Machine Learning webszolgáltatás tooan</span><span class="sxs-lookup"><span data-stu-id="767a2-103">Connect tooan Azure Machine Learning Web Service</span></span>
<span data-ttu-id="767a2-104">hello Azure Machine Learning-fejlesztők számára egy webes API toomake előrejelzések a bemeneti adatok a valós idejű vagy kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="767a2-104">hello Azure Machine Learning developer experience is a Web service API toomake predictions from input data in real time or in batch mode.</span></span> <span data-ttu-id="767a2-105">Használja az Azure Machine Learning Studio toocreate előrejelzéseket, és az Azure Machine Learning Web-szolgáltatások üzembe.</span><span class="sxs-lookup"><span data-stu-id="767a2-105">You use Azure Machine Learning Studio toocreate predictions and deploy an Azure Machine Learning Web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="767a2-106">arról, hogyan toolearn toocreate és központi telepítése egy Machine Learning webszolgáltatás Machine Learning Studio használatával:</span><span class="sxs-lookup"><span data-stu-id="767a2-106">toolearn about how toocreate and deploy a Machine Learning Web service using Machine Learning Studio:</span></span>

* <span data-ttu-id="767a2-107">Hogyan toocreate egy kísérletben a Machine Learning Studióban, tekintse meg a oktatóanyagért [az első kísérlet létrehozása](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="767a2-107">For a tutorial on how toocreate an experiment in Machine Learning Studio, see [Create your first experiment](machine-learning-create-experiment.md).</span></span>
* <span data-ttu-id="767a2-108">További részletekért toodeploy egy webszolgáltatás-bővítmény, lásd: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="767a2-108">For details on how toodeploy a Web service, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="767a2-109">További információ a gépi tanulás általában a Microsoft hello [Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="767a2-109">For more information about Machine Learning in general, visit hello [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

## <a name="azure-machine-learning-web-service"></a><span data-ttu-id="767a2-110">Az Azure Machine Learning webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="767a2-110">Azure Machine Learning Web service</span></span>
<span data-ttu-id="767a2-111">Hello Azure Machine Learning webszolgáltatás, a külső alkalmazás kommunikál a Machine Learning munkafolyamat pontozási modell valós időben.</span><span class="sxs-lookup"><span data-stu-id="767a2-111">With hello Azure Machine Learning Web service, an external application communicates with a Machine Learning workflow scoring model in real time.</span></span> <span data-ttu-id="767a2-112">A Machine Learning webszolgáltatás-hívások tooan külső alkalmazás előrejelzés eredményeket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="767a2-112">A Machine Learning Web service call returns prediction results tooan external application.</span></span> <span data-ttu-id="767a2-113">a Machine Learning webszolgáltatás-hívások toomake, az előrejelzéshez telepítésekor létrehozott API-kulcs át.</span><span class="sxs-lookup"><span data-stu-id="767a2-113">toomake a Machine Learning Web service call, you pass an API key that is created when you deploy a prediction.</span></span> <span data-ttu-id="767a2-114">Machine Learning webszolgáltatás hello REST, olyan népszerű architektúrával kapcsolatos választási lehetőség webes projektek programozási alapul.</span><span class="sxs-lookup"><span data-stu-id="767a2-114">hello Machine Learning Web service is based on REST, a popular architecture choice for web programming projects.</span></span>

<span data-ttu-id="767a2-115">Az Azure Machine Learning két különböző típusú szolgáltatást tud biztosítani:</span><span class="sxs-lookup"><span data-stu-id="767a2-115">Azure Machine Learning has two types of services:</span></span>

* <span data-ttu-id="767a2-116">Kérés-válasz szolgáltatás (RR-EKET) – egy kis késés, magas szinten méretezhető illesztőfelület-toohello állapot nélküli modellekhez létrehozott és telepített, a Machine Learning Studio hello biztosító szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="767a2-116">Request-Response Service (RRS) – A low latency, highly scalable service that provides an interface toohello stateless models created and deployed from hello Machine Learning Studio.</span></span>
* <span data-ttu-id="767a2-117">Kötegelt végrehajtási szolgáltatás (BES) –-egy aszinkron szolgáltatást, hogy a rekordok köteg pontszámait.</span><span class="sxs-lookup"><span data-stu-id="767a2-117">Batch Execution Service (BES) – An asynchronous service that scores a batch for data records.</span></span>

<span data-ttu-id="767a2-118">További információ a Machine Learning webszolgáltatások: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="767a2-118">For more information about Machine Learning Web services, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="get-an-azure-machine-learning-authorization-key"></a><span data-ttu-id="767a2-119">Azure Machine Learning engedélyezési kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="767a2-119">Get an Azure Machine Learning authorization key</span></span>
<span data-ttu-id="767a2-120">Amikor telepíti a kísérletet, API-kulcsokat hello webszolgáltatás jön létre.</span><span class="sxs-lookup"><span data-stu-id="767a2-120">When you deploy your experiment, API keys are generated for hello Web service.</span></span> <span data-ttu-id="767a2-121">Hello kulcsok kérhetnek le több helyre.</span><span class="sxs-lookup"><span data-stu-id="767a2-121">You can retrieve hello keys from several locations.</span></span>

### <a name="from-hello-microsoft-azure-machine-learning-web-services-portal"></a><span data-ttu-id="767a2-122">Hello Microsoft Azure Machine Learning webszolgáltatások portálról</span><span class="sxs-lookup"><span data-stu-id="767a2-122">From hello Microsoft Azure Machine Learning Web Services portal</span></span>
<span data-ttu-id="767a2-123">Jelentkezzen be toohello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon.</span><span class="sxs-lookup"><span data-stu-id="767a2-123">Sign in toohello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span>

<span data-ttu-id="767a2-124">tooretrieve hello API-kulcs egy új Machine Learning webszolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="767a2-124">tooretrieve hello API key for a New Machine Learning Web service:</span></span>

1. <span data-ttu-id="767a2-125">Hello Azure Machine Learning webszolgáltatások portálon kattintson **webszolgáltatások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="767a2-125">In hello Azure Machine Learning Web Services portal, click **Web Services** hello top menu.</span></span>
2. <span data-ttu-id="767a2-126">Kattintson a kívánt tooretrieve hello kulcs hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="767a2-126">Click hello Web service for which you want tooretrieve hello key.</span></span>
3. <span data-ttu-id="767a2-127">Kattintson a felső menüben hello **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="767a2-127">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="767a2-128">Másolja ki és mentse a hello **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="767a2-128">Copy and save hello **Primary Key**.</span></span>

<span data-ttu-id="767a2-129">tooretrieve hello API-kulcs egy klasszikus Machine Learning webszolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="767a2-129">tooretrieve hello API key for a Classic Machine Learning Web service:</span></span>

1. <span data-ttu-id="767a2-130">Hello Azure Machine Learning webszolgáltatások portálon kattintson **klasszikus webszolgáltatások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="767a2-130">In hello Azure Machine Learning Web Services portal, click **Classic Web Services** hello top menu.</span></span>
2. <span data-ttu-id="767a2-131">Kattintson a hello webes szolgáltatás, amellyel dolgozik.</span><span class="sxs-lookup"><span data-stu-id="767a2-131">Click hello Web service with which you are working.</span></span>
3. <span data-ttu-id="767a2-132">Kattintson a kívánt tooretrieve hello kulcs hello végpontra.</span><span class="sxs-lookup"><span data-stu-id="767a2-132">Click hello endpoint for which you want tooretrieve hello key.</span></span>
4. <span data-ttu-id="767a2-133">Kattintson a felső menüben hello **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="767a2-133">On hello top menu, click **Consume**.</span></span>
5. <span data-ttu-id="767a2-134">Másolja ki és mentse a hello **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="767a2-134">Copy and save hello **Primary Key**.</span></span>

### <a name="classic-web-service"></a><span data-ttu-id="767a2-135">Klasszikus webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="767a2-135">Classic Web service</span></span>
 <span data-ttu-id="767a2-136">A kulcs a klasszikus webszolgáltatáshoz beolvasható a Machine Learning Studio vagy a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="767a2-136">You can also retrieve a key for a Classic Web service from Machine Learning Studio or hello Azure classic portal.</span></span>

#### <a name="machine-learning-studio"></a><span data-ttu-id="767a2-137">Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="767a2-137">Machine Learning Studio</span></span>
1. <span data-ttu-id="767a2-138">A Machine Learning Studióban, kattintson a **WEBSZOLGÁLTATÁSOK** hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="767a2-138">In Machine Learning Studio, click **WEB SERVICES** on hello left.</span></span>
2. <span data-ttu-id="767a2-139">Kattintson egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="767a2-139">Click a Web service.</span></span> <span data-ttu-id="767a2-140">Hello **API-kulcs** a hello **IRÁNYÍTÓPULT** fülre.</span><span class="sxs-lookup"><span data-stu-id="767a2-140">hello **API key** is on hello **DASHBOARD** tab.</span></span>

#### <a name="azure-classic-portal"></a><span data-ttu-id="767a2-141">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="767a2-141">Azure classic portal</span></span>
1. <span data-ttu-id="767a2-142">Kattintson a **MACHINE LEARNING** hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="767a2-142">Click **MACHINE LEARNING** on hello left.</span></span>
2. <span data-ttu-id="767a2-143">Kattintson a hello munkaterületen, ahol a webszolgáltatás is található.</span><span class="sxs-lookup"><span data-stu-id="767a2-143">Click hello workspace in which your Web service is located.</span></span>
3. <span data-ttu-id="767a2-144">Kattintson a **WEBSZOLGÁLTATÁSOK**.</span><span class="sxs-lookup"><span data-stu-id="767a2-144">Click **WEB SERVICES**.</span></span>
4. <span data-ttu-id="767a2-145">Kattintson egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="767a2-145">Click a Web service.</span></span>
5. <span data-ttu-id="767a2-146">Kattintson egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="767a2-146">Click an endpoint.</span></span> <span data-ttu-id="767a2-147">hello "API-kulcsot" hello jobb alsó, nem működik.</span><span class="sxs-lookup"><span data-stu-id="767a2-147">hello “API KEY” is down at hello lower-right.</span></span>

## <span data-ttu-id="767a2-148"><a id="connect"></a>Csatlakozás tooa Machine Learning webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="767a2-148"><a id="connect"></a>Connect tooa Machine Learning Web service</span></span>
<span data-ttu-id="767a2-149">Machine Learning Web service bármely programozási nyelvet, amely támogatja a HTTP-kérelem-válasz tooa is elérheti.</span><span class="sxs-lookup"><span data-stu-id="767a2-149">You can connect tooa Machine Learning Web service using any programming language that supports HTTP request and response.</span></span> <span data-ttu-id="767a2-150">A Machine Learning webszolgáltatás súgólapja a példákban a C#, Python, és R tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="767a2-150">You can view examples in C#, Python, and R from a Machine Learning Web service help page.</span></span>

<span data-ttu-id="767a2-151">**Számítógép-Learning API súgó** Machine Learning API-súgó egy webszolgáltatás-bővítmény telepítésekor létrejön.</span><span class="sxs-lookup"><span data-stu-id="767a2-151">**Machine Learning API help** Machine Learning API help is created when you deploy a Web service.</span></span> <span data-ttu-id="767a2-152">Lásd: [Azure Machine Learning forgatókönyv - webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="767a2-152">See [Azure Machine Learning Walkthrough- Deploy Web Service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
<span data-ttu-id="767a2-153">Machine Learning API súgó hello webszolgáltatás előrejelzéshez részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="767a2-153">hello Machine Learning API help contains details about a prediction Web service.</span></span>

1. <span data-ttu-id="767a2-154">Kattintson a hello webes szolgáltatás, amellyel dolgozik.</span><span class="sxs-lookup"><span data-stu-id="767a2-154">Click hello Web service with which you are working.</span></span>
2. <span data-ttu-id="767a2-155">Kattintson a kívánt tooview hello API Súgóoldalát hello végpontra.</span><span class="sxs-lookup"><span data-stu-id="767a2-155">Click hello endpoint for which you want tooview hello API Help Page.</span></span>
3. <span data-ttu-id="767a2-156">Kattintson a felső menüben hello **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="767a2-156">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="767a2-157">Kattintson a **API súgólap** hello kérés-válasz vagy kötegelt végrehajtási végpontok alatt.</span><span class="sxs-lookup"><span data-stu-id="767a2-157">Click **API help page** under either hello Request-Response or Batch Execution endpoints.</span></span>

<span data-ttu-id="767a2-158">**tooview a Machine Learning API súgóját egy új webszolgáltatás-bővítmény**</span><span class="sxs-lookup"><span data-stu-id="767a2-158">**tooview Machine Learning API help for a New Web service**</span></span>

<span data-ttu-id="767a2-159">Az Azure Machine Learning Web Services portálra. hello:</span><span class="sxs-lookup"><span data-stu-id="767a2-159">In hello Azure Machine Learning Web Services Portal:</span></span>

1. <span data-ttu-id="767a2-160">Kattintson a **WEBSZOLGÁLTATÁSOK** hello felső menüben.</span><span class="sxs-lookup"><span data-stu-id="767a2-160">Click **WEB SERVICES** on hello top menu.</span></span>
2. <span data-ttu-id="767a2-161">Kattintson a kívánt tooretrieve hello kulcs hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="767a2-161">Click hello Web service for which you want tooretrieve hello key.</span></span>

<span data-ttu-id="767a2-162">Kattintson a **felhasználás** tooget hello URI-azonosítók hello kérelem-Reposonse és a C#, R és Python kötegelt végrehajtási szolgáltatás és a minta kódot.</span><span class="sxs-lookup"><span data-stu-id="767a2-162">Click **Consume** tooget hello URIs for hello Request-Reposonse and Batch Execution Services and Sample code in C#, R, and Python.</span></span>

<span data-ttu-id="767a2-163">Kattintson a **Swagger API** tooget hello API-k alapján dokumentációja hívása hello Swagger megadott URI-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="767a2-163">Click **Swagger API** tooget Swagger based documentation for hello APIs called from hello supplied URIs.</span></span>

### <a name="c-sample"></a><span data-ttu-id="767a2-164">C# minta</span><span class="sxs-lookup"><span data-stu-id="767a2-164">C# Sample</span></span>
<span data-ttu-id="767a2-165">tooconnect tooa Machine Learning webszolgáltatás, használjon egy **HttpClient** ScoreData átadásakor.</span><span class="sxs-lookup"><span data-stu-id="767a2-165">tooconnect tooa Machine Learning Web service, use an **HttpClient** passing ScoreData.</span></span> <span data-ttu-id="767a2-166">ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort hello ScoreData jelölő tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="767a2-166">ScoreData contains a FeatureVector, an n-dimensional vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="767a2-167">API-kulcs a Machine Learning szolgáltatás toohello hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="767a2-167">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="767a2-168">Machine Learning webszolgáltatás, tooconnect tooa hello **Microsoft.AspNet.WebApi.Client** NuGet-csomagot kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="767a2-168">tooconnect tooa Machine Learning Web service, hello **Microsoft.AspNet.WebApi.Client** NuGet package must be installed.</span></span>

<span data-ttu-id="767a2-169">**Telepítse a Microsoft.AspNet.WebApi.Client NuGet a Visual Studióban**</span><span class="sxs-lookup"><span data-stu-id="767a2-169">**Install Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**</span></span>

1. <span data-ttu-id="767a2-170">UCI hello letöltési adatkészlet közzététele: felnőtt 2 osztály dataset webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="767a2-170">Publish hello Download dataset from UCI: Adult 2 class dataset Web Service.</span></span>
2. <span data-ttu-id="767a2-171">Kattintson a **Tools** (Eszközök)  > **NuGet Package Manager** (NuGet-csomagkezelő) > **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="767a2-171">Click **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="767a2-172">Válasszon **Install-Package Microsoft.AspNet.WebApi.Client**.</span><span class="sxs-lookup"><span data-stu-id="767a2-172">Choose **Install-Package Microsoft.AspNet.WebApi.Client**.</span></span>

<span data-ttu-id="767a2-173">**toorun hello kódminta**</span><span class="sxs-lookup"><span data-stu-id="767a2-173">**toorun hello code sample**</span></span>

1. <span data-ttu-id="767a2-174">Közzététele "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning mintavételt hello részét.</span><span class="sxs-lookup"><span data-stu-id="767a2-174">Publish "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="767a2-175">Adjon meg apiKey hello kulccsal egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="767a2-175">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="767a2-176">Lásd: **Azure Machine Learning engedélyezési kulcs beszerzése** felett.</span><span class="sxs-lookup"><span data-stu-id="767a2-176">See **Get an Azure Machine Learning authorization key** above.</span></span>
3. <span data-ttu-id="767a2-177">A kérelem URI-azonosítója hello serviceUri hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="767a2-177">Assign serviceUri with hello Request URI.</span></span>

### <a name="python-sample"></a><span data-ttu-id="767a2-178">Python-minta</span><span class="sxs-lookup"><span data-stu-id="767a2-178">Python Sample</span></span>
<span data-ttu-id="767a2-179">tooconnect tooa Machine Learning webszolgáltatás, használja a hello **urllib2** könyvtár ScoreData átadásakor.</span><span class="sxs-lookup"><span data-stu-id="767a2-179">tooconnect tooa Machine Learning Web service, use hello **urllib2** library passing ScoreData.</span></span> <span data-ttu-id="767a2-180">ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort hello ScoreData jelölő tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="767a2-180">ScoreData contains a FeatureVector, an n-dimensional  vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="767a2-181">API-kulcs a Machine Learning szolgáltatás toohello hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="767a2-181">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="767a2-182">**toorun hello kódminta**</span><span class="sxs-lookup"><span data-stu-id="767a2-182">**toorun hello code sample**</span></span>

1. <span data-ttu-id="767a2-183">Telepítése "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning mintavételt hello részét.</span><span class="sxs-lookup"><span data-stu-id="767a2-183">Deploy "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="767a2-184">Adjon meg apiKey hello kulccsal egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="767a2-184">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="767a2-185">Lásd: hello **Azure Machine Learning engedélyezési kulcs beszerzése** közelében hello kezdete című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="767a2-185">See hello **Get an Azure Machine Learning authorization key** section near hello beginning of this article.</span></span>
3. <span data-ttu-id="767a2-186">A kérelem URI-azonosítója hello serviceUri hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="767a2-186">Assign serviceUri with hello Request URI.</span></span>

