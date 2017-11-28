---
title: "6. lépés: Hozzáférési hello Machine Learning webszolgáltatás |} Microsoft Docs"
description: "6. lépésében hello fejlesztése egy prediktív megoldás forgatókönyv: egy aktív Azure Machine Learning Web service eléréséhez."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 211de0294092c6a6b5e6eb608d5d3b88107674c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-6-access-hello-azure-machine-learning-web-service"></a><span data-ttu-id="88209-103">A forgatókönyv 6. lépés: Hozzáférési hello Azure Machine Learning webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="88209-103">Walkthrough Step 6: Access hello Azure Machine Learning web service</span></span>

<span data-ttu-id="88209-104">Ez az utolsó lépésében hello hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="88209-104">This is hello last step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="88209-105">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="88209-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="88209-106">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="88209-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="88209-107">Új kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="88209-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="88209-108">Betanítása és kiértékelése hello modellek</span><span class="sxs-lookup"><span data-stu-id="88209-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="88209-109">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="88209-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. <span data-ttu-id="88209-110">**Hozzáférés hello webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="88209-110">**Access hello Web service**</span></span>

- - -
<span data-ttu-id="88209-111">A forgatókönyv hello előző lépésben helyeztünk üzembe egy webszolgáltatás, amelyet a jóváírás kockázat előrejelzési modellt használ.</span><span class="sxs-lookup"><span data-stu-id="88209-111">In hello previous step in this walkthrough we deployed a web service that uses our credit risk prediction model.</span></span> <span data-ttu-id="88209-112">A felhasználók most már tudja toosend adatok tooit és eredményeket.</span><span class="sxs-lookup"><span data-stu-id="88209-112">Now users are able toosend data tooit and receive results.</span></span> 

<span data-ttu-id="88209-113">hello webszolgáltatás egy Azure webes szolgáltatás, amely kaphat, és térjen vissza az adatokat, és REST API-k az alábbi két módszer egyikével:</span><span class="sxs-lookup"><span data-stu-id="88209-113">hello Web service is an Azure web service that can receive and return data using REST APIs in one of two ways:</span></span>  

* <span data-ttu-id="88209-114">**Kérelem/válasz** – hello felhasználó elküldi egy vagy több sornyi jóváírás adatok toohello egy HTTP protokoll használatával szolgáltatásra, és hello szolgáltatás válasza eredményeket egy vagy több készletekkel.</span><span class="sxs-lookup"><span data-stu-id="88209-114">**Request/Response** - hello user sends one or more rows of credit data toohello service by using an HTTP protocol, and hello service responds with one or more sets of results.</span></span>
* <span data-ttu-id="88209-115">**Kötegelt végrehajtás** – hello felhasználói tárolja egy vagy több sort jóváírás adatok az Azure blob-, és ezután elküldi a hello blob hely toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="88209-115">**Batch Execution** - hello user stores one or more rows of credit data in an Azure blob and then sends hello blob location toohello service.</span></span> <span data-ttu-id="88209-116">hello szolgáltatás pontszámok összes hello adatsorokat hello bemeneti blob, tárolók hello egy másik blob eredményez, és visszaadja hello tároló URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="88209-116">hello service scores all hello rows of data in hello input blob, stores hello results in another blob, and returns hello URL of that container.</span></span>  

<span data-ttu-id="88209-117">hello keresztül történik egy klasszikus webszolgáltatás leggyorsabb és legegyszerűbb módja tooaccess hello [Azure ML kérés-válasz szolgáltatás webalkalmazás](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) vagy [Azure ML kötegelt végrehajtási szolgáltatás Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span><span class="sxs-lookup"><span data-stu-id="88209-117">hello quickest and easiest way tooaccess a Classic web service is through hello [Azure ML Request-Response Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) or [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span></span>

<span data-ttu-id="88209-118">Ezeket a webes alkalmazás sablonokat hozhat létre egy egyéni webalkalmazást, hogy ismeri a webszolgáltatás bemeneti adatokat, és mi ad vissza.</span><span class="sxs-lookup"><span data-stu-id="88209-118">These web app templates can build a custom web app that knows your web service's input data and what it will return.</span></span> <span data-ttu-id="88209-119">Toodo szüksége, adja meg a hozzáférés tooyour webszolgáltatás és az adatokat, és hello sablon hello rest.</span><span class="sxs-lookup"><span data-stu-id="88209-119">All you need toodo is provide access tooyour web service and data, and hello template does hello rest.</span></span>

<span data-ttu-id="88209-120">Hello web app sablonok használatával kapcsolatos további információkért lásd: [egy Azure Machine Learning Web service web app sablonnal felhasználásához](machine-learning-consume-web-service-with-web-app-template.md).</span><span class="sxs-lookup"><span data-stu-id="88209-120">For more information on using hello web app templates, see [Consume an Azure Machine Learning Web service with a web app template](machine-learning-consume-web-service-with-web-app-template.md).</span></span>

<span data-ttu-id="88209-121">Egy egyéni alkalmazást tooaccess hello webszolgáltatás előírt, R, C# és Python programozási nyelvek starter kód használatával is készíthet.</span><span class="sxs-lookup"><span data-stu-id="88209-121">You can also develop a custom application tooaccess hello web service using starter code provided for you in R, C#, and Python programming languages.</span></span>

<span data-ttu-id="88209-122">A teljes részletei [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="88209-122">You can find complete details in [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

