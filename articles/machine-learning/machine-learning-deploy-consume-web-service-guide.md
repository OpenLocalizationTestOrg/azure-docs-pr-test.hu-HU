---
title: "Az Azure Machine Learning webszolgáltatások: Telepítés és használat |} Microsoft Docs"
description: "Erőforrások üzembe helyezéséhez és webszolgáltatások felhasználása."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: 47635376-d1f4-4ea4-a6af-bd1f99f69a69
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 539c2abb053a0f981be0374defe45cf4d96b740b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a><span data-ttu-id="d11f9-103">Azure Machine Learning webszolgáltatások: telepítés és használat</span><span class="sxs-lookup"><span data-stu-id="d11f9-103">Azure Machine Learning Web Services: Deployment and consumption</span></span>
<span data-ttu-id="d11f9-104">Azure Machine Learning toodeploy gépi tanulásra munkafolyamatok és modellek webszolgáltatásként használhatja.</span><span class="sxs-lookup"><span data-stu-id="d11f9-104">You can use Azure Machine Learning toodeploy machine-learning workflows and models as web services.</span></span> <span data-ttu-id="d11f9-105">Ezek a webszolgáltatások majd lehet használt toocall hello gépi tanulási modelljeit alkalmazásokból keresztül hello Internet toodo előrejelzéseket valós időben vagy kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="d11f9-105">These web services can then be used toocall hello machine-learning models from applications over hello Internet toodo predictions in real time or in batch mode.</span></span> <span data-ttu-id="d11f9-106">Mivel hello webszolgáltatások RESTful, hívása azokat a különböző programozási nyelveket és platformok, például a .NET és a Java, és az alkalmazások, például az Excel.</span><span class="sxs-lookup"><span data-stu-id="d11f9-106">Because hello web services are RESTful, you can call them from various programming languages and platforms, such as .NET and Java, and from applications, such as Excel.</span></span>

<span data-ttu-id="d11f9-107">hello következő szakaszokban hivatkozások toowalkthroughs, a kód és a dokumentáció toohelp első lépések megtételéhez.</span><span class="sxs-lookup"><span data-stu-id="d11f9-107">hello next sections provide links toowalkthroughs, code, and documentation toohelp get you started.</span></span>

## <a name="deploy-a-web-service"></a><span data-ttu-id="d11f9-108">Webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d11f9-108">Deploy a web service</span></span>
### <a name="with-azure-machine-learning-studio"></a><span data-ttu-id="d11f9-109">Az Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="d11f9-109">With Azure Machine Learning Studio</span></span>
<span data-ttu-id="d11f9-110">A Machine Learning Studio és hello Microsoft Azure Machine Learning webszolgáltatások portal segítségével telepítheti és kezelheti egy webszolgáltatás-bővítmény kód írása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d11f9-110">Machine Learning Studio and hello Microsoft Azure Machine Learning Web Services portal help you deploy and manage a web service without writing code.</span></span>

<span data-ttu-id="d11f9-111">hello következő hivatkozásokra kattintva kapcsolatos általános tudnivalók toodeploy egy új webszolgáltatás-bővítmény:</span><span class="sxs-lookup"><span data-stu-id="d11f9-111">hello following links provide general Information about how toodeploy a new web service:</span></span>

* <span data-ttu-id="d11f9-112">Az áttekintést arról, hogyan toodeploy egy új webes szolgáltatás, amely az Azure Resource Manageren alapul: [egy új webszolgáltatás-bővítmény telepítése](machine-learning-webservice-deploy-a-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="d11f9-112">For an overview about how toodeploy a new web service that's based on Azure Resource Manager, see [Deploy a new web service](machine-learning-webservice-deploy-a-web-service.md).</span></span>
* <span data-ttu-id="d11f9-113">A forgatókönyv leírja, toodeploy egy webszolgáltatás-bővítmény, lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="d11f9-113">For a walkthrough about how toodeploy a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="d11f9-114">Egy kapcsolatos teljes forgatókönyv toocreate és egy webszolgáltatás-bővítmény telepítése, lásd: [forgatókönyv 1. lépés: a Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="d11f9-114">For a full walkthrough about how toocreate and deploy a web service, see [Walkthrough Step 1: Create a Machine Learning workspace](machine-learning-walkthrough-1-create-ml-workspace.md).</span></span>
* <span data-ttu-id="d11f9-115">Adott, amely egy webszolgáltatás-bővítmény telepítése című részben talál példákat:</span><span class="sxs-lookup"><span data-stu-id="d11f9-115">For specific examples that deploy a web service, see:</span></span>

  * [<span data-ttu-id="d11f9-116">Útmutató 5. lépés: Hello Azure Machine Learning webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="d11f9-116">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
  * [<span data-ttu-id="d11f9-117">Hogyan toodeploy egy webes szolgáltatás toomultiple régiók</span><span class="sxs-lookup"><span data-stu-id="d11f9-117">How toodeploy a web service toomultiple regions</span></span>](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a><span data-ttu-id="d11f9-118">A web services erőforrás-szolgáltató API-k (Azure Resource Manager API-k)</span><span class="sxs-lookup"><span data-stu-id="d11f9-118">With web services resource provider APIs (Azure Resource Manager APIs)</span></span>
<span data-ttu-id="d11f9-119">hello Azure Machine Learning erőforrás-szolgáltató web Services lehetővé teszi, hogy üzembe helyezési és kezelési webes szolgáltatás REST API-hívásokkal.</span><span class="sxs-lookup"><span data-stu-id="d11f9-119">hello Azure Machine Learning resource provider for web services enables deployment and management of web services by using REST API calls.</span></span> <span data-ttu-id="d11f9-120">További részletekért lásd: a [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="d11f9-120">For additional details, see the [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) reference.</span></span>

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a><span data-ttu-id="d11f9-121">A PowerShell-parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="d11f9-121">With PowerShell cmdlets</span></span>
<span data-ttu-id="d11f9-122">Az Azure Machine Learning erőforrás-szolgáltató web Services lehetővé teszi, hogy üzembe helyezési és kezelési webszolgáltatások PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="d11f9-122">Azure Machine Learning resource provider for web services enables deployment and management of web services by using PowerShell cmdlets.</span></span>

<span data-ttu-id="d11f9-123">toouse hello parancsmagok kell először bejelentkezik tooyour hello PowerShell környezetből Azure fiók hello segítségével [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d11f9-123">toouse hello cmdlets, you must first sign in tooyour Azure account from within hello PowerShell environment by using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="d11f9-124">Ha nincs tisztában a hogyan toocall PowerShell parancsokat, amelyek a erőforrás-kezelő alapja című [az Azure PowerShell használata Azure Resource Managerrel](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span><span class="sxs-lookup"><span data-stu-id="d11f9-124">If you are unfamiliar with how toocall PowerShell commands that are based on Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span></span>

<span data-ttu-id="d11f9-125">tooexport a prediktív kísérletezhet, használja a [a mintakód](https://github.com/ritwik20/AzureML-WebServices).</span><span class="sxs-lookup"><span data-stu-id="d11f9-125">tooexport your predictive experiment, use [this sample code](https://github.com/ritwik20/AzureML-WebServices).</span></span> <span data-ttu-id="d11f9-126">Miután létrehozta a hello .exe fájl hello kódból, adhatja meg:</span><span class="sxs-lookup"><span data-stu-id="d11f9-126">After you create hello .exe file from hello code, you can type:</span></span>

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

<span data-ttu-id="d11f9-127">Egy webes szolgáltatás JSON-sablon hello alkalmazást futtató hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d11f9-127">Running hello application creates a web service JSON template.</span></span> <span data-ttu-id="d11f9-128">toouse hello sablon toodeploy egy webszolgáltatás, hozzá kell adnia a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="d11f9-128">toouse hello template toodeploy a web service, you must add hello following information:</span></span>

* <span data-ttu-id="d11f9-129">A tárfiók neve vagy a kulcs</span><span class="sxs-lookup"><span data-stu-id="d11f9-129">Storage account name and key</span></span>

    <span data-ttu-id="d11f9-130">Hello tárfiók neve vagy a kulcs letölthető vagy hello [Azure-portálon](https://portal.azure.com/) vagy hello [a klasszikus Azure portálon](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d11f9-130">You can get hello storage account name and key from either hello [Azure portal](https://portal.azure.com/) or hello [Azure classic portal](http://manage.windowsazure.com/).</span></span>
* <span data-ttu-id="d11f9-131">Előfizetési csomag azonosítója</span><span class="sxs-lookup"><span data-stu-id="d11f9-131">Commitment plan ID</span></span>

    <span data-ttu-id="d11f9-132">Hello terv azonosítója letölthető hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portál jelentkezik be, majd kattintson a csomag neve.</span><span class="sxs-lookup"><span data-stu-id="d11f9-132">You can get hello plan ID from hello [Azure Machine Learning Web Services](https://services.azureml.net) portal by signing in and clicking a plan name.</span></span>

<span data-ttu-id="d11f9-133">Adja hozzá toohello JSON-sablon hello gyermekeként *tulajdonságok* hello csomóponthoz azonos szinten, hello *MachineLearningWorkspace* csomópont.</span><span class="sxs-lookup"><span data-stu-id="d11f9-133">Add them toohello JSON template as children of hello *Properties* node at hello same level as hello *MachineLearningWorkspace* node.</span></span>

<span data-ttu-id="d11f9-134">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="d11f9-134">Here's an example:</span></span>

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

<span data-ttu-id="d11f9-135">Tekintse meg a következő cikkek hello és a mintakódot további részletek:</span><span class="sxs-lookup"><span data-stu-id="d11f9-135">See hello following articles and sample code for additional details:</span></span>

* <span data-ttu-id="d11f9-136">[Az Azure Machine Learning parancsmagok](https://msdn.microsoft.com/library/azure/mt767952.aspx) hivatkozást az MSDN webhelyen</span><span class="sxs-lookup"><span data-stu-id="d11f9-136">[Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN</span></span>
* <span data-ttu-id="d11f9-137">A minta [forgatókönyv](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) a Githubon</span><span class="sxs-lookup"><span data-stu-id="d11f9-137">Sample [walkthrough](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) on GitHub</span></span>

## <a name="consume-hello-web-services"></a><span data-ttu-id="d11f9-138">Hello webszolgáltatások felhasználása</span><span class="sxs-lookup"><span data-stu-id="d11f9-138">Consume hello web services</span></span>
### <a name="from-hello-azure-machine-learning-web-services-ui-testing"></a><span data-ttu-id="d11f9-139">A hello Azure Machine Learning Web Services felhasználói felületén (tesztelés)</span><span class="sxs-lookup"><span data-stu-id="d11f9-139">From hello Azure Machine Learning Web Services UI (Testing)</span></span>
<span data-ttu-id="d11f9-140">A webszolgáltatás hello Azure Machine Learning webszolgáltatások portálról tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d11f9-140">You can test your web service from hello Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="d11f9-141">Ez magában foglalja a tesztelés hello kérés-válasz szolgáltatás (RR-EKET), és hogyan kötegelt végrehajtási szolgáltatás (BES).</span><span class="sxs-lookup"><span data-stu-id="d11f9-141">This includes testing hello Request-Response service (RRS) and Batch Execution service (BES) interfaces.</span></span>

* [<span data-ttu-id="d11f9-142">Új webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d11f9-142">Deploy a new web service</span></span>](machine-learning-webservice-deploy-a-web-service.md)
* [<span data-ttu-id="d11f9-143">Az Azure Machine Learning webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="d11f9-143">Deploy an Azure Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
* [<span data-ttu-id="d11f9-144">Útmutató 5. lépés: Hello Azure Machine Learning webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="d11f9-144">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a><span data-ttu-id="d11f9-145">Az Excelből</span><span class="sxs-lookup"><span data-stu-id="d11f9-145">From Excel</span></span>
<span data-ttu-id="d11f9-146">Egy Excel-sablont, amely akkor hello webszolgáltatás tölthető le:</span><span class="sxs-lookup"><span data-stu-id="d11f9-146">You can download an Excel template that consumes hello web service:</span></span>

* [<span data-ttu-id="d11f9-147">Az Azure Machine Learning webszolgáltatásba az Excelből felhasználása</span><span class="sxs-lookup"><span data-stu-id="d11f9-147">Consuming an Azure Machine Learning web service from Excel</span></span>](machine-learning-consuming-from-excel.md)
* [<span data-ttu-id="d11f9-148">Az Excel beépülő modul az Azure Machine Learning webszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d11f9-148">Excel add-in for Azure Machine Learning Web Services</span></span>](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a><span data-ttu-id="d11f9-149">Egy REST-alapú ügyfél</span><span class="sxs-lookup"><span data-stu-id="d11f9-149">From a REST-based client</span></span>
<span data-ttu-id="d11f9-150">Az Azure Machine Learning webszolgáltatások RESTful API-k.</span><span class="sxs-lookup"><span data-stu-id="d11f9-150">Azure Machine Learning Web Services are RESTful APIs.</span></span> <span data-ttu-id="d11f9-151">Használatba vehetné a különböző platformokon, például a .NET, Python, R, Java, stb. hello API- **felhasználás** lap a web Service hello [Microsoft Azure Machine Learning webszolgáltatások portal](https://services.azureml.net) minta rendelkezik Ismerkedés a kódot, amely segítségével.</span><span class="sxs-lookup"><span data-stu-id="d11f9-151">You can consume these APIs from various platforms, such as .NET, Python, R, Java, etc. hello **Consume** page for your web service on hello [Microsoft Azure Machine Learning Web Services portal](https://services.azureml.net) has sample code that can help you get started.</span></span> <span data-ttu-id="d11f9-152">További információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="d11f9-152">For more information, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>
