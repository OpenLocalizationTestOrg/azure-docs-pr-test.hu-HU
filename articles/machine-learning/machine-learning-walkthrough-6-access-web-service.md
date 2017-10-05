---
title: "6. lépés: A Machine Learning webszolgáltatás eléréséhez |} Microsoft Docs"
description: "A prediktív megoldás bemutatóért Develop 6. lépés: egy aktív Azure Machine Learning Web service eléréséhez."
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
ms.openlocfilehash: d309f6c4749a80c81859b693a2bd5927e8fe0e54
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a><span data-ttu-id="0e47d-103">Az útmutató 6. lépése: Hozzáférés az Azure Machine Learning webszolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="0e47d-103">Walkthrough Step 6: Access the Azure Machine Learning web service</span></span>

<span data-ttu-id="0e47d-104">Ez a forgatókönyv utolsó lépését az [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="0e47d-104">This is the last step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="0e47d-105">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e47d-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="0e47d-106">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="0e47d-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="0e47d-107">Új kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e47d-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="0e47d-108">A modellek betanítása és kiértékelése</span><span class="sxs-lookup"><span data-stu-id="0e47d-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="0e47d-109">A webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0e47d-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. <span data-ttu-id="0e47d-110">**Hozzáférés a webszolgáltatáshoz**</span><span class="sxs-lookup"><span data-stu-id="0e47d-110">**Access the Web service**</span></span>

- - -
<span data-ttu-id="0e47d-111">A forgatókönyv az előző lépésben helyeztünk üzembe egy webszolgáltatás, amelyet a jóváírás kockázat előrejelzési modellt használ.</span><span class="sxs-lookup"><span data-stu-id="0e47d-111">In the previous step in this walkthrough we deployed a web service that uses our credit risk prediction model.</span></span> <span data-ttu-id="0e47d-112">Most felhasználók képesek adatokat küldeni és fogadni az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="0e47d-112">Now users are able to send data to it and receive results.</span></span> 

<span data-ttu-id="0e47d-113">A webszolgáltatás egy Azure webes szolgáltatás, amely kaphat, és térjen vissza az adatokat, és REST API-k az alábbi két módszer egyikével:</span><span class="sxs-lookup"><span data-stu-id="0e47d-113">The Web service is an Azure web service that can receive and return data using REST APIs in one of two ways:</span></span>  

* <span data-ttu-id="0e47d-114">**Kérelem/válasz** – a felhasználó küld egy vagy több sornyi adatot jóváírás a szolgáltatás egy HTTP protokoll használatával, és a szolgáltatás válaszol, az eredmények egy vagy több készletet.</span><span class="sxs-lookup"><span data-stu-id="0e47d-114">**Request/Response** - The user sends one or more rows of credit data to the service by using an HTTP protocol, and the service responds with one or more sets of results.</span></span>
* <span data-ttu-id="0e47d-115">**Kötegelt végrehajtás** – a felhasználó tárolja egy vagy több sort jóváírás adatok az Azure blob-, és ezután elküldi a blob helyére a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0e47d-115">**Batch Execution** - The user stores one or more rows of credit data in an Azure blob and then sends the blob location to the service.</span></span> <span data-ttu-id="0e47d-116">A szolgáltatás az adatok a sorokat a bemeneti BLOB pontszámaihoz, tárolja az eredményeket egy másik blob és tároló URL-CÍMÉT adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0e47d-116">The service scores all the rows of data in the input blob, stores the results in another blob, and returns the URL of that container.</span></span>  

<span data-ttu-id="0e47d-117">A leggyorsabb és legegyszerűbb módja a klasszikus webes szolgáltatás eléréséhez keresztül történik a [Azure ML kérés-válasz szolgáltatás webalkalmazás](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) vagy [Azure ML kötegelt végrehajtási szolgáltatás Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span><span class="sxs-lookup"><span data-stu-id="0e47d-117">The quickest and easiest way to access a Classic web service is through the [Azure ML Request-Response Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) or [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span></span>

<span data-ttu-id="0e47d-118">Ezeket a webes alkalmazás sablonokat hozhat létre egy egyéni webalkalmazást, hogy ismeri a webszolgáltatás bemeneti adatokat, és mi ad vissza.</span><span class="sxs-lookup"><span data-stu-id="0e47d-118">These web app templates can build a custom web app that knows your web service's input data and what it will return.</span></span> <span data-ttu-id="0e47d-119">Mindössze annyit kell tennie a webszolgáltatás és az adatok eléréséhez, és a sablon elvégzi a többit, azaz.</span><span class="sxs-lookup"><span data-stu-id="0e47d-119">All you need to do is provide access to your web service and data, and the template does the rest.</span></span>

<span data-ttu-id="0e47d-120">A webes alkalmazás sablonok használatával kapcsolatos további információkért lásd: [egy Azure Machine Learning Web service web app sablonnal felhasználásához](machine-learning-consume-web-service-with-web-app-template.md).</span><span class="sxs-lookup"><span data-stu-id="0e47d-120">For more information on using the web app templates, see [Consume an Azure Machine Learning Web service with a web app template](machine-learning-consume-web-service-with-web-app-template.md).</span></span>

<span data-ttu-id="0e47d-121">Is létrehozhat egy egyéni alkalmazást az előírt, R, C# és Python programozási nyelvek starter kód használatával webes szolgáltatás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0e47d-121">You can also develop a custom application to access the web service using starter code provided for you in R, C#, and Python programming languages.</span></span>

<span data-ttu-id="0e47d-122">A teljes részletei [hogyan kell használni az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="0e47d-122">You can find complete details in [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

