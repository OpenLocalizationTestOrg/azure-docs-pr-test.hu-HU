---
title: az Azure Machine Learning Web service aaaHow tooconsume |} Microsoft Docs
description: "A machine learning szolgáltatás telepítése után hello RESTFul webszolgáltatás, amelyet szeretné elérhetővé tenni a kötegelt végrehajtási szolgáltatásként vagy a valós idejű kérés-válasz szolgáltatás képes használni."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 804f8211-9437-4982-98e9-ca841b7edf56
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/02/2017
ms.author: garye
ms.openlocfilehash: 19095604169e5af1daed12c17ba66258233178bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconsume-an-azure-machine-learning-web-service"></a><span data-ttu-id="87cb4-103">Hogyan tooconsume az Azure Machine Learning Web service</span><span class="sxs-lookup"><span data-stu-id="87cb4-103">How tooconsume an Azure Machine Learning Web service</span></span>

<span data-ttu-id="87cb4-104">Miután telepít egy Azure Machine Learning a prediktív modell webszolgáltatásként, használhatja a REST API toosend az adatok és az előrejelzés.</span><span class="sxs-lookup"><span data-stu-id="87cb4-104">Once you deploy an Azure Machine Learning predictive model as a Web service, you can use a REST API toosend it data and get predictions.</span></span> <span data-ttu-id="87cb4-105">Hello adatokat küldhet a valós idejű vagy kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="87cb4-105">You can send hello data in real-time or in batch mode.</span></span>

<span data-ttu-id="87cb4-106">További információ található toocreate és központi telepítése a Machine Learning Web service Itt a Machine Learning Studio használatával:</span><span class="sxs-lookup"><span data-stu-id="87cb4-106">You can find more information about how toocreate and deploy a Machine Learning Web service using Machine Learning Studio here:</span></span>

* <span data-ttu-id="87cb4-107">Hogyan toocreate egy kísérletben a Machine Learning Studióban, tekintse meg a oktatóanyagért [az első kísérlet létrehozása](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="87cb4-107">For a tutorial on how toocreate an experiment in Machine Learning Studio, see [Create your first experiment](machine-learning-create-experiment.md).</span></span>
* <span data-ttu-id="87cb4-108">További részletekért toodeploy egy webszolgáltatás-bővítmény, lásd: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="87cb4-108">For details on how toodeploy a Web service, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="87cb4-109">További információ a gépi tanulás általában a Microsoft hello [Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="87cb4-109">For more information about Machine Learning in general, visit hello [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a><span data-ttu-id="87cb4-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="87cb4-110">Overview</span></span>
<span data-ttu-id="87cb4-111">Hello Azure Machine Learning webszolgáltatás, a külső alkalmazás kommunikál a Machine Learning munkafolyamat pontozási modell valós időben.</span><span class="sxs-lookup"><span data-stu-id="87cb4-111">With hello Azure Machine Learning Web service, an external application communicates with a Machine Learning workflow scoring model in real time.</span></span> <span data-ttu-id="87cb4-112">A Machine Learning webszolgáltatás-hívások tooan külső alkalmazás előrejelzés eredményeket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="87cb4-112">A Machine Learning Web service call returns prediction results tooan external application.</span></span> <span data-ttu-id="87cb4-113">a Machine Learning webszolgáltatás-hívások toomake, az előrejelzéshez telepítésekor létrehozott API-kulcs át.</span><span class="sxs-lookup"><span data-stu-id="87cb4-113">toomake a Machine Learning Web service call, you pass an API key that is created when you deploy a prediction.</span></span> <span data-ttu-id="87cb4-114">Machine Learning webszolgáltatás hello REST, olyan népszerű architektúrával kapcsolatos választási lehetőség webes projektek programozási alapul.</span><span class="sxs-lookup"><span data-stu-id="87cb4-114">hello Machine Learning Web service is based on REST, a popular architecture choice for web programming projects.</span></span>

<span data-ttu-id="87cb4-115">Az Azure Machine Learning két különböző típusú szolgáltatást tud biztosítani:</span><span class="sxs-lookup"><span data-stu-id="87cb4-115">Azure Machine Learning has two types of services:</span></span>

* <span data-ttu-id="87cb4-116">Kérés-válasz szolgáltatás (RR-EKET) – egy kis késés, magas szinten méretezhető illesztőfelület-toohello állapot nélküli modellekhez létrehozott és telepített, a Machine Learning Studio hello biztosító szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="87cb4-116">Request-Response Service (RRS) – A low latency, highly scalable service that provides an interface toohello stateless models created and deployed from hello Machine Learning Studio.</span></span>
* <span data-ttu-id="87cb4-117">Kötegelt végrehajtási szolgáltatás (BES) –-egy aszinkron szolgáltatást, hogy a rekordok köteg pontszámait.</span><span class="sxs-lookup"><span data-stu-id="87cb4-117">Batch Execution Service (BES) – An asynchronous service that scores a batch for data records.</span></span>

<span data-ttu-id="87cb4-118">További információ a Machine Learning webszolgáltatások: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="87cb4-118">For more information about Machine Learning Web services, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="get-an-azure-machine-learning-authorization-key"></a><span data-ttu-id="87cb4-119">Azure Machine Learning engedélyezési kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="87cb4-119">Get an Azure Machine Learning authorization key</span></span>
<span data-ttu-id="87cb4-120">Amikor telepíti a kísérletet, API-kulcsokat hello webszolgáltatás jön létre.</span><span class="sxs-lookup"><span data-stu-id="87cb4-120">When you deploy your experiment, API keys are generated for hello Web service.</span></span> <span data-ttu-id="87cb4-121">Hello kulcsok kérhetnek le több helyre.</span><span class="sxs-lookup"><span data-stu-id="87cb4-121">You can retrieve hello keys from several locations.</span></span>

### <a name="from-hello-microsoft-azure-machine-learning-web-services-portal"></a><span data-ttu-id="87cb4-122">Hello Microsoft Azure Machine Learning webszolgáltatások portálról</span><span class="sxs-lookup"><span data-stu-id="87cb4-122">From hello Microsoft Azure Machine Learning Web Services portal</span></span>
<span data-ttu-id="87cb4-123">Jelentkezzen be toohello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon.</span><span class="sxs-lookup"><span data-stu-id="87cb4-123">Sign in toohello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span>

<span data-ttu-id="87cb4-124">tooretrieve hello API-kulcs egy új Machine Learning webszolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="87cb4-124">tooretrieve hello API key for a New Machine Learning Web service:</span></span>

1. <span data-ttu-id="87cb4-125">Hello Azure Machine Learning webszolgáltatások portálon kattintson **webszolgáltatások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="87cb4-125">In hello Azure Machine Learning Web Services portal, click **Web Services** hello top menu.</span></span>
2. <span data-ttu-id="87cb4-126">Kattintson a kívánt tooretrieve hello kulcs hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="87cb4-126">Click hello Web service for which you want tooretrieve hello key.</span></span>
3. <span data-ttu-id="87cb4-127">Kattintson a felső menüben hello **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="87cb4-127">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="87cb4-128">Másolja ki és mentse a hello **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="87cb4-128">Copy and save hello **Primary Key**.</span></span>

<span data-ttu-id="87cb4-129">tooretrieve hello API-kulcs egy klasszikus Machine Learning webszolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="87cb4-129">tooretrieve hello API key for a Classic Machine Learning Web service:</span></span>

1. <span data-ttu-id="87cb4-130">Hello Azure Machine Learning webszolgáltatások portálon kattintson **klasszikus webszolgáltatások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="87cb4-130">In hello Azure Machine Learning Web Services portal, click **Classic Web Services** hello top menu.</span></span>
2. <span data-ttu-id="87cb4-131">Kattintson a hello webes szolgáltatás, amellyel dolgozik.</span><span class="sxs-lookup"><span data-stu-id="87cb4-131">Click hello Web service with which you are working.</span></span>
3. <span data-ttu-id="87cb4-132">Kattintson a kívánt tooretrieve hello kulcs hello végpontra.</span><span class="sxs-lookup"><span data-stu-id="87cb4-132">Click hello endpoint for which you want tooretrieve hello key.</span></span>
4. <span data-ttu-id="87cb4-133">Kattintson a felső menüben hello **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="87cb4-133">On hello top menu, click **Consume**.</span></span>
5. <span data-ttu-id="87cb4-134">Másolja ki és mentse a hello **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="87cb4-134">Copy and save hello **Primary Key**.</span></span>

### <a name="classic-web-service"></a><span data-ttu-id="87cb4-135">Klasszikus webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="87cb4-135">Classic Web service</span></span>
 <span data-ttu-id="87cb4-136">A kulcs a klasszikus webszolgáltatáshoz beolvasható a Machine Learning Studio vagy a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="87cb4-136">You can also retrieve a key for a Classic Web service from Machine Learning Studio or hello Azure classic portal.</span></span>

#### <a name="machine-learning-studio"></a><span data-ttu-id="87cb4-137">Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="87cb4-137">Machine Learning Studio</span></span>
1. <span data-ttu-id="87cb4-138">A Machine Learning Studióban, kattintson a **WEBSZOLGÁLTATÁSOK** hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="87cb4-138">In Machine Learning Studio, click **WEB SERVICES** on hello left.</span></span>
2. <span data-ttu-id="87cb4-139">Kattintson egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="87cb4-139">Click a Web service.</span></span> <span data-ttu-id="87cb4-140">Hello **API-kulcs** a hello **IRÁNYÍTÓPULT** fülre.</span><span class="sxs-lookup"><span data-stu-id="87cb4-140">hello **API key** is on hello **DASHBOARD** tab.</span></span>

#### <a name="azure-classic-portal"></a><span data-ttu-id="87cb4-141">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="87cb4-141">Azure classic portal</span></span>
1. <span data-ttu-id="87cb4-142">Kattintson a **MACHINE LEARNING** hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="87cb4-142">Click **MACHINE LEARNING** on hello left.</span></span>
2. <span data-ttu-id="87cb4-143">Kattintson a hello munkaterületen, ahol a webszolgáltatás is található.</span><span class="sxs-lookup"><span data-stu-id="87cb4-143">Click hello workspace in which your Web service is located.</span></span>
3. <span data-ttu-id="87cb4-144">Kattintson a **WEBSZOLGÁLTATÁSOK**.</span><span class="sxs-lookup"><span data-stu-id="87cb4-144">Click **WEB SERVICES**.</span></span>
4. <span data-ttu-id="87cb4-145">Kattintson egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="87cb4-145">Click a Web service.</span></span>
5. <span data-ttu-id="87cb4-146">Kattintson egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="87cb4-146">Click an endpoint.</span></span> <span data-ttu-id="87cb4-147">hello "API-kulcsot" hello jobb alsó, nem működik.</span><span class="sxs-lookup"><span data-stu-id="87cb4-147">hello “API KEY” is down at hello lower-right.</span></span>

## <span data-ttu-id="87cb4-148"><a id="connect"></a>Csatlakozás tooa Machine Learning webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="87cb4-148"><a id="connect"></a>Connect tooa Machine Learning Web service</span></span>
<span data-ttu-id="87cb4-149">Machine Learning Web service bármely programozási nyelvet, amely támogatja a HTTP-kérelem-válasz tooa is elérheti.</span><span class="sxs-lookup"><span data-stu-id="87cb4-149">You can connect tooa Machine Learning Web service using any programming language that supports HTTP request and response.</span></span> <span data-ttu-id="87cb4-150">A Machine Learning webszolgáltatás súgólapja a példákban a C#, Python, és R tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="87cb4-150">You can view examples in C#, Python, and R from a Machine Learning Web service help page.</span></span>

<span data-ttu-id="87cb4-151">**Számítógép-Learning API súgó** Machine Learning API-súgó egy webszolgáltatás-bővítmény telepítésekor létrejön.</span><span class="sxs-lookup"><span data-stu-id="87cb4-151">**Machine Learning API help** Machine Learning API help is created when you deploy a Web service.</span></span> <span data-ttu-id="87cb4-152">Lásd: [Azure Machine Learning forgatókönyv - webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="87cb4-152">See [Azure Machine Learning Walkthrough- Deploy Web Service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
<span data-ttu-id="87cb4-153">Machine Learning API súgó hello webszolgáltatás előrejelzéshez részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="87cb4-153">hello Machine Learning API help contains details about a prediction Web service.</span></span>

1. <span data-ttu-id="87cb4-154">Kattintson a hello webes szolgáltatás, amellyel dolgozik.</span><span class="sxs-lookup"><span data-stu-id="87cb4-154">Click hello Web service with which you are working.</span></span>
2. <span data-ttu-id="87cb4-155">Kattintson a kívánt tooview hello API Súgóoldalát hello végpontra.</span><span class="sxs-lookup"><span data-stu-id="87cb4-155">Click hello endpoint for which you want tooview hello API Help Page.</span></span>
3. <span data-ttu-id="87cb4-156">Kattintson a felső menüben hello **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="87cb4-156">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="87cb4-157">Kattintson a **API súgólap** hello kérés-válasz vagy kötegelt végrehajtási végpontok alatt.</span><span class="sxs-lookup"><span data-stu-id="87cb4-157">Click **API help page** under either hello Request-Response or Batch Execution endpoints.</span></span>

<span data-ttu-id="87cb4-158">**tooview a Machine Learning API súgóját egy új webszolgáltatás-bővítmény**</span><span class="sxs-lookup"><span data-stu-id="87cb4-158">**tooview Machine Learning API help for a New Web service**</span></span>

<span data-ttu-id="87cb4-159">Az Azure Machine Learning Web Services portálra. hello:</span><span class="sxs-lookup"><span data-stu-id="87cb4-159">In hello Azure Machine Learning Web Services Portal:</span></span>

1. <span data-ttu-id="87cb4-160">Kattintson a **WEBSZOLGÁLTATÁSOK** hello felső menüben.</span><span class="sxs-lookup"><span data-stu-id="87cb4-160">Click **WEB SERVICES** on hello top menu.</span></span>
2. <span data-ttu-id="87cb4-161">Kattintson a kívánt tooretrieve hello kulcs hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="87cb4-161">Click hello Web service for which you want tooretrieve hello key.</span></span>

<span data-ttu-id="87cb4-162">Kattintson a **felhasználás** tooget hello URI-azonosítók hello kérelem-Reposonse és a C#, R és Python kötegelt végrehajtási szolgáltatás és a minta kódot.</span><span class="sxs-lookup"><span data-stu-id="87cb4-162">Click **Consume** tooget hello URIs for hello Request-Reposonse and Batch Execution Services and Sample code in C#, R, and Python.</span></span>

<span data-ttu-id="87cb4-163">Kattintson a **Swagger API** tooget hello API-k alapján dokumentációja hívása hello Swagger megadott URI-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="87cb4-163">Click **Swagger API** tooget Swagger based documentation for hello APIs called from hello supplied URIs.</span></span>

### <a name="c-sample"></a><span data-ttu-id="87cb4-164">C# minta</span><span class="sxs-lookup"><span data-stu-id="87cb4-164">C# Sample</span></span>
<span data-ttu-id="87cb4-165">tooconnect tooa Machine Learning webszolgáltatás, használjon egy **HttpClient** ScoreData átadásakor.</span><span class="sxs-lookup"><span data-stu-id="87cb4-165">tooconnect tooa Machine Learning Web service, use an **HttpClient** passing ScoreData.</span></span> <span data-ttu-id="87cb4-166">ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort hello ScoreData jelölő tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="87cb4-166">ScoreData contains a FeatureVector, an n-dimensional vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="87cb4-167">API-kulcs a Machine Learning szolgáltatás toohello hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="87cb4-167">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="87cb4-168">Machine Learning webszolgáltatás, tooconnect tooa hello **Microsoft.AspNet.WebApi.Client** NuGet-csomagot kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="87cb4-168">tooconnect tooa Machine Learning Web service, hello **Microsoft.AspNet.WebApi.Client** NuGet package must be installed.</span></span>

<span data-ttu-id="87cb4-169">**Telepítse a Microsoft.AspNet.WebApi.Client NuGet a Visual Studióban**</span><span class="sxs-lookup"><span data-stu-id="87cb4-169">**Install Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**</span></span>

1. <span data-ttu-id="87cb4-170">UCI hello letöltési adatkészlet közzététele: felnőtt 2 osztály dataset webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="87cb4-170">Publish hello Download dataset from UCI: Adult 2 class dataset Web Service.</span></span>
2. <span data-ttu-id="87cb4-171">Kattintson a **Tools** (Eszközök)  > **NuGet Package Manager** (NuGet-csomagkezelő) > **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="87cb4-171">Click **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="87cb4-172">Válasszon **Install-Package Microsoft.AspNet.WebApi.Client**.</span><span class="sxs-lookup"><span data-stu-id="87cb4-172">Choose **Install-Package Microsoft.AspNet.WebApi.Client**.</span></span>

<span data-ttu-id="87cb4-173">**toorun hello kódminta**</span><span class="sxs-lookup"><span data-stu-id="87cb4-173">**toorun hello code sample**</span></span>

1. <span data-ttu-id="87cb4-174">Közzététele "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning mintavételt hello részét.</span><span class="sxs-lookup"><span data-stu-id="87cb4-174">Publish "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="87cb4-175">Adjon meg apiKey hello kulccsal egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="87cb4-175">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="87cb4-176">Lásd: **Azure Machine Learning engedélyezési kulcs beszerzése** felett.</span><span class="sxs-lookup"><span data-stu-id="87cb4-176">See **Get an Azure Machine Learning authorization key** above.</span></span>
3. <span data-ttu-id="87cb4-177">A kérelem URI-azonosítója hello serviceUri hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="87cb4-177">Assign serviceUri with hello Request URI.</span></span>

### <a name="python-sample"></a><span data-ttu-id="87cb4-178">Python-minta</span><span class="sxs-lookup"><span data-stu-id="87cb4-178">Python Sample</span></span>
<span data-ttu-id="87cb4-179">tooconnect tooa Machine Learning webszolgáltatás, használja a hello **urllib2** könyvtár ScoreData átadásakor.</span><span class="sxs-lookup"><span data-stu-id="87cb4-179">tooconnect tooa Machine Learning Web service, use hello **urllib2** library passing ScoreData.</span></span> <span data-ttu-id="87cb4-180">ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort hello ScoreData jelölő tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="87cb4-180">ScoreData contains a FeatureVector, an n-dimensional  vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="87cb4-181">API-kulcs a Machine Learning szolgáltatás toohello hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="87cb4-181">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="87cb4-182">**toorun hello kódminta**</span><span class="sxs-lookup"><span data-stu-id="87cb4-182">**toorun hello code sample**</span></span>

1. <span data-ttu-id="87cb4-183">Telepítése "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning mintavételt hello részét.</span><span class="sxs-lookup"><span data-stu-id="87cb4-183">Deploy "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="87cb4-184">Adjon meg apiKey hello kulccsal egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="87cb4-184">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="87cb4-185">Lásd: hello **Azure Machine Learning engedélyezési kulcs beszerzése** közelében hello kezdete című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="87cb4-185">See hello **Get an Azure Machine Learning authorization key** section near hello beginning of this article.</span></span>
3. <span data-ttu-id="87cb4-186">A kérelem URI-azonosítója hello serviceUri hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="87cb4-186">Assign serviceUri with hello Request URI.</span></span>

