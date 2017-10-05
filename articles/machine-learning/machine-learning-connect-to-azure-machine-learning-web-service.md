---
title: "Csatlakozás egy Machine Learning webszolgáltatás |} Microsoft Docs"
description: "A C# vagy Python az Azure Machine Learning Web engedélykulcs szolgáltatáshoz kapcsolódni."
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
redirect_document_id: TRUE
ms.openlocfilehash: 0fc6c7e921b18eb14a95fb737d8fb5ab5cc7e687
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-an-azure-machine-learning-web-service"></a><span data-ttu-id="cfe4d-103">Kapcsolódás az Azure Machine Learning webszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="cfe4d-103">Connect to an Azure Machine Learning Web Service</span></span>
<span data-ttu-id="cfe4d-104">Az Azure Machine Learning-fejlesztők számára a webszolgáltatási API a bemeneti adatokból előrejelzéseket készítsen a valós idejű vagy kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-104">The Azure Machine Learning developer experience is a Web service API to make predictions from input data in real time or in batch mode.</span></span> <span data-ttu-id="cfe4d-105">Azure Machine Learning Studio használatával előrejelzéseket létrehozása és telepítése az Azure Machine Learning Web service.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-105">You use Azure Machine Learning Studio to create predictions and deploy an Azure Machine Learning Web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="cfe4d-106">Megtudhatja, hogyan hozhat létre és telepíthet egy Machine Learning webszolgáltatás Machine Learning Studio használatával kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="cfe4d-106">To learn about how to create and deploy a Machine Learning Web service using Machine Learning Studio:</span></span>

* <span data-ttu-id="cfe4d-107">A kísérlet létrehozása a Machine Learning Studio oktatóanyag, lásd: [az első kísérlet létrehozása](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="cfe4d-107">For a tutorial on how to create an experiment in Machine Learning Studio, see [Create your first experiment](machine-learning-create-experiment.md).</span></span>
* <span data-ttu-id="cfe4d-108">Egy webszolgáltatás-bővítmény telepítésével, lásd: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="cfe4d-108">For details on how to deploy a Web service, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="cfe4d-109">További információ a gépi tanulás általában, látogasson el a [Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="cfe4d-109">For more information about Machine Learning in general, visit the [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

## <a name="azure-machine-learning-web-service"></a><span data-ttu-id="cfe4d-110">Az Azure Machine Learning webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cfe4d-110">Azure Machine Learning Web service</span></span>
<span data-ttu-id="cfe4d-111">Az Azure Machine Learning Web Service külső alkalmazás kommunikál a Machine Learning munkafolyamat pontozási modell valós időben.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-111">With the Azure Machine Learning Web service, an external application communicates with a Machine Learning workflow scoring model in real time.</span></span> <span data-ttu-id="cfe4d-112">A Machine Learning webszolgáltatás-hívás a külső alkalmazás előrejelzés eredményeket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-112">A Machine Learning Web service call returns prediction results to an external application.</span></span> <span data-ttu-id="cfe4d-113">Ahhoz, hogy egy Machine Learning webszolgáltatás hívja, át előrejelzéshez telepítésekor létrehozott API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-113">To make a Machine Learning Web service call, you pass an API key that is created when you deploy a prediction.</span></span> <span data-ttu-id="cfe4d-114">A Machine Learning webszolgáltatás REST, olyan népszerű architektúrával kapcsolatos választási lehetőség webes projektek programozási alapul.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-114">The Machine Learning Web service is based on REST, a popular architecture choice for web programming projects.</span></span>

<span data-ttu-id="cfe4d-115">Az Azure Machine Learning két különböző típusú szolgáltatást tud biztosítani:</span><span class="sxs-lookup"><span data-stu-id="cfe4d-115">Azure Machine Learning has two types of services:</span></span>

* <span data-ttu-id="cfe4d-116">Kérés-válasz szolgáltatás (RR-EKET) – egy kis késés, az állapot nélküli modellekhez létrehozott és telepített felületet biztosít a Machine Learning Studio a magas szinten méretezhető szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-116">Request-Response Service (RRS) – A low latency, highly scalable service that provides an interface to the stateless models created and deployed from the Machine Learning Studio.</span></span>
* <span data-ttu-id="cfe4d-117">Kötegelt végrehajtási szolgáltatás (BES) –-egy aszinkron szolgáltatást, hogy a rekordok köteg pontszámait.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-117">Batch Execution Service (BES) – An asynchronous service that scores a batch for data records.</span></span>

<span data-ttu-id="cfe4d-118">További információ a Machine Learning webszolgáltatások: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="cfe4d-118">For more information about Machine Learning Web services, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="get-an-azure-machine-learning-authorization-key"></a><span data-ttu-id="cfe4d-119">Azure Machine Learning engedélyezési kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="cfe4d-119">Get an Azure Machine Learning authorization key</span></span>
<span data-ttu-id="cfe4d-120">Amikor telepíti a kísérletet, API-kulcsokat jönnek létre a webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-120">When you deploy your experiment, API keys are generated for the Web service.</span></span> <span data-ttu-id="cfe4d-121">A kulcsok kérhetnek le több helyre.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-121">You can retrieve the keys from several locations.</span></span>

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a><span data-ttu-id="cfe4d-122">A Microsoft Azure Machine Learning webszolgáltatások portálról</span><span class="sxs-lookup"><span data-stu-id="cfe4d-122">From the Microsoft Azure Machine Learning Web Services portal</span></span>
<span data-ttu-id="cfe4d-123">Jelentkezzen be a [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-123">Sign in to the [Microsoft Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span>

<span data-ttu-id="cfe4d-124">Egy új Machine Learning webszolgáltatáshoz API-kulcs beolvasása:</span><span class="sxs-lookup"><span data-stu-id="cfe4d-124">To retrieve the API key for a New Machine Learning Web service:</span></span>

1. <span data-ttu-id="cfe4d-125">Az Azure Machine Learning webszolgáltatások portálon kattintson **webszolgáltatások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-125">In the Azure Machine Learning Web Services portal, click **Web Services** the top menu.</span></span>
2. <span data-ttu-id="cfe4d-126">Kattintson a webszolgáltatás legyen a kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-126">Click the Web service for which you want to retrieve the key.</span></span>
3. <span data-ttu-id="cfe4d-127">Kattintson a felső menüben **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-127">On the top menu, click **Consume**.</span></span>
4. <span data-ttu-id="cfe4d-128">Másolja ki és mentse a **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-128">Copy and save the **Primary Key**.</span></span>

<span data-ttu-id="cfe4d-129">Az API-kulcs a klasszikus Machine Learning webszolgáltatás beolvasása:</span><span class="sxs-lookup"><span data-stu-id="cfe4d-129">To retrieve the API key for a Classic Machine Learning Web service:</span></span>

1. <span data-ttu-id="cfe4d-130">Az Azure Machine Learning webszolgáltatások portálon kattintson **klasszikus webszolgáltatások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-130">In the Azure Machine Learning Web Services portal, click **Classic Web Services** the top menu.</span></span>
2. <span data-ttu-id="cfe4d-131">Kattintson a webes szolgáltatás, amellyel dolgozik.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-131">Click the Web service with which you are working.</span></span>
3. <span data-ttu-id="cfe4d-132">Kattintson a végpont legyen a kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-132">Click the endpoint for which you want to retrieve the key.</span></span>
4. <span data-ttu-id="cfe4d-133">Kattintson a felső menüben **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-133">On the top menu, click **Consume**.</span></span>
5. <span data-ttu-id="cfe4d-134">Másolja ki és mentse a **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-134">Copy and save the **Primary Key**.</span></span>

### <a name="classic-web-service"></a><span data-ttu-id="cfe4d-135">Klasszikus webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cfe4d-135">Classic Web service</span></span>
 <span data-ttu-id="cfe4d-136">A kulcs a klasszikus webszolgáltatáshoz beolvasható a Machine Learning Studio vagy a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-136">You can also retrieve a key for a Classic Web service from Machine Learning Studio or the Azure classic portal.</span></span>

#### <a name="machine-learning-studio"></a><span data-ttu-id="cfe4d-137">Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="cfe4d-137">Machine Learning Studio</span></span>
1. <span data-ttu-id="cfe4d-138">A Machine Learning Studióban, kattintson a **WEBSZOLGÁLTATÁSOK** a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-138">In Machine Learning Studio, click **WEB SERVICES** on the left.</span></span>
2. <span data-ttu-id="cfe4d-139">Kattintson egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-139">Click a Web service.</span></span> <span data-ttu-id="cfe4d-140">A **API-kulcs** megtalálható a **IRÁNYÍTÓPULT** fülre.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-140">The **API key** is on the **DASHBOARD** tab.</span></span>

#### <a name="azure-classic-portal"></a><span data-ttu-id="cfe4d-141">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="cfe4d-141">Azure classic portal</span></span>
1. <span data-ttu-id="cfe4d-142">Kattintson a **MACHINE LEARNING** a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-142">Click **MACHINE LEARNING** on the left.</span></span>
2. <span data-ttu-id="cfe4d-143">Kattintson a munkaterületen, ahol a webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-143">Click the workspace in which your Web service is located.</span></span>
3. <span data-ttu-id="cfe4d-144">Kattintson a **WEBSZOLGÁLTATÁSOK**.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-144">Click **WEB SERVICES**.</span></span>
4. <span data-ttu-id="cfe4d-145">Kattintson egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-145">Click a Web service.</span></span>
5. <span data-ttu-id="cfe4d-146">Kattintson egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-146">Click an endpoint.</span></span> <span data-ttu-id="cfe4d-147">A "API-kulcsot" nem működik az alsó sarokban.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-147">The “API KEY” is down at the lower-right.</span></span>

## <span data-ttu-id="cfe4d-148"><a id="connect"></a>Csatlakozás egy Machine Learning webszolgáltatás-bővítmény</span><span class="sxs-lookup"><span data-stu-id="cfe4d-148"><a id="connect"></a>Connect to a Machine Learning Web service</span></span>
<span data-ttu-id="cfe4d-149">A Machine Learning Web service bármely programozási nyelvet, amely támogatja a HTTP-kérelem-válasz kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-149">You can connect to a Machine Learning Web service using any programming language that supports HTTP request and response.</span></span> <span data-ttu-id="cfe4d-150">A Machine Learning webszolgáltatás súgólapja a példákban a C#, Python, és R tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-150">You can view examples in C#, Python, and R from a Machine Learning Web service help page.</span></span>

<span data-ttu-id="cfe4d-151">**Számítógép-Learning API súgó** Machine Learning API-súgó egy webszolgáltatás-bővítmény telepítésekor létrejön.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-151">**Machine Learning API help** Machine Learning API help is created when you deploy a Web service.</span></span> <span data-ttu-id="cfe4d-152">Lásd: [Azure Machine Learning forgatókönyv - webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="cfe4d-152">See [Azure Machine Learning Walkthrough- Deploy Web Service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
<span data-ttu-id="cfe4d-153">A Machine Learning API súgó webszolgáltatás előrejelzéshez részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-153">The Machine Learning API help contains details about a prediction Web service.</span></span>

1. <span data-ttu-id="cfe4d-154">Kattintson a webes szolgáltatás, amellyel dolgozik.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-154">Click the Web service with which you are working.</span></span>
2. <span data-ttu-id="cfe4d-155">Kattintson a végpont, amelynek meg szeretné tekinteni a API Súgóoldalát.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-155">Click the endpoint for which you want to view the API Help Page.</span></span>
3. <span data-ttu-id="cfe4d-156">Kattintson a felső menüben **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-156">On the top menu, click **Consume**.</span></span>
4. <span data-ttu-id="cfe4d-157">Kattintson a **API súgólap** alatt a kérelem / válasz vagy kötegelt végrehajtási végpontok.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-157">Click **API help page** under either the Request-Response or Batch Execution endpoints.</span></span>

<span data-ttu-id="cfe4d-158">**Az új webszolgáltatás nézet Machine Learning API segítségével**</span><span class="sxs-lookup"><span data-stu-id="cfe4d-158">**To view Machine Learning API help for a New Web service**</span></span>

<span data-ttu-id="cfe4d-159">Az Azure gépi tanulási webportál szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="cfe4d-159">In the Azure Machine Learning Web Services Portal:</span></span>

1. <span data-ttu-id="cfe4d-160">Kattintson a **WEBSZOLGÁLTATÁSOK** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-160">Click **WEB SERVICES** on the top menu.</span></span>
2. <span data-ttu-id="cfe4d-161">Kattintson a webszolgáltatás legyen a kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-161">Click the Web service for which you want to retrieve the key.</span></span>

<span data-ttu-id="cfe4d-162">Kattintson a **felhasználás** az URI-azonosítók lekérése a kérelem-Reposonse és kötegelt végrehajtási szolgáltatás és a minta kód a C#, R és Python.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-162">Click **Consume** to get the URIs for the Request-Reposonse and Batch Execution Services and Sample code in C#, R, and Python.</span></span>

<span data-ttu-id="cfe4d-163">Kattintson a **Swagger API** megszerezni a Swagger-alapú dokumentáció számára az API-k a megadott URI-k hívását.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-163">Click **Swagger API** to get Swagger based documentation for the APIs called from the supplied URIs.</span></span>

### <a name="c-sample"></a><span data-ttu-id="cfe4d-164">C# minta</span><span class="sxs-lookup"><span data-stu-id="cfe4d-164">C# Sample</span></span>
<span data-ttu-id="cfe4d-165">Csatlakozás egy Machine Learning webszolgáltatáshoz, használjon egy **HttpClient** ScoreData átadásakor.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-165">To connect to a Machine Learning Web service, use an **HttpClient** passing ScoreData.</span></span> <span data-ttu-id="cfe4d-166">ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort a ScoreData jelölő tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-166">ScoreData contains a FeatureVector, an n-dimensional vector of numerical features that represents the ScoreData.</span></span> <span data-ttu-id="cfe4d-167">A hitelesítést a Machine Learning szolgáltatás API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-167">You authenticate to the Machine Learning service with an API key.</span></span>

<span data-ttu-id="cfe4d-168">Csatlakozás egy Machine Learning webszolgáltatáshoz, a **Microsoft.AspNet.WebApi.Client** NuGet-csomagot kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-168">To connect to a Machine Learning Web service, the **Microsoft.AspNet.WebApi.Client** NuGet package must be installed.</span></span>

<span data-ttu-id="cfe4d-169">**Telepítse a Microsoft.AspNet.WebApi.Client NuGet a Visual Studióban**</span><span class="sxs-lookup"><span data-stu-id="cfe4d-169">**Install Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**</span></span>

1. <span data-ttu-id="cfe4d-170">A letöltési UCI adatkészlet közzététele: felnőtt 2 osztály dataset webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-170">Publish the Download dataset from UCI: Adult 2 class dataset Web Service.</span></span>
2. <span data-ttu-id="cfe4d-171">Kattintson a **Tools** (Eszközök)  > **NuGet Package Manager** (NuGet-csomagkezelő) > **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-171">Click **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="cfe4d-172">Válasszon **Install-Package Microsoft.AspNet.WebApi.Client**.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-172">Choose **Install-Package Microsoft.AspNet.WebApi.Client**.</span></span>

<span data-ttu-id="cfe4d-173">**A kód a minta futtatásához**</span><span class="sxs-lookup"><span data-stu-id="cfe4d-173">**To run the code sample**</span></span>

1. <span data-ttu-id="cfe4d-174">Közzététele "1. példa: dataset letöltését UCI: felnőtt 2 osztály adatkészlet" kísérlet, a Machine Learning minta gyűjtemény része.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-174">Publish "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of the Machine Learning sample collection.</span></span>
2. <span data-ttu-id="cfe4d-175">Rendelje hozzá a kulccsal apiKey webszolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-175">Assign apiKey with the key from a Web service.</span></span> <span data-ttu-id="cfe4d-176">Lásd: **Azure Machine Learning engedélyezési kulcs beszerzése** felett.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-176">See **Get an Azure Machine Learning authorization key** above.</span></span>
3. <span data-ttu-id="cfe4d-177">A kérelem URI-azonosítójú serviceUri hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-177">Assign serviceUri with the Request URI.</span></span>

### <a name="python-sample"></a><span data-ttu-id="cfe4d-178">Python-minta</span><span class="sxs-lookup"><span data-stu-id="cfe4d-178">Python Sample</span></span>
<span data-ttu-id="cfe4d-179">Csatlakozás egy Machine Learning webszolgáltatáshoz, használja a **urllib2** könyvtár ScoreData átadásakor.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-179">To connect to a Machine Learning Web service, use the **urllib2** library passing ScoreData.</span></span> <span data-ttu-id="cfe4d-180">ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort a ScoreData jelölő tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-180">ScoreData contains a FeatureVector, an n-dimensional  vector of numerical features that represents the ScoreData.</span></span> <span data-ttu-id="cfe4d-181">A hitelesítést a Machine Learning szolgáltatás API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-181">You authenticate to the Machine Learning service with an API key.</span></span>

<span data-ttu-id="cfe4d-182">**A kód a minta futtatásához**</span><span class="sxs-lookup"><span data-stu-id="cfe4d-182">**To run the code sample**</span></span>

1. <span data-ttu-id="cfe4d-183">Telepítése "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning minta gyűjtemény része.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-183">Deploy "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of the Machine Learning sample collection.</span></span>
2. <span data-ttu-id="cfe4d-184">Rendelje hozzá a kulccsal apiKey webszolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-184">Assign apiKey with the key from a Web service.</span></span> <span data-ttu-id="cfe4d-185">Tekintse meg a **Azure Machine Learning engedélyezési kulcs beszerzése** elején című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-185">See the **Get an Azure Machine Learning authorization key** section near the beginning of this article.</span></span>
3. <span data-ttu-id="cfe4d-186">A kérelem URI-azonosítójú serviceUri hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="cfe4d-186">Assign serviceUri with the Request URI.</span></span>

