---
title: "A GitHub-tárházban kapcsolódó webalkalmazás üzembe helyezése |} Microsoft Docs"
description: "Az Azure Resource Manager-sablon használatával webalkalmazás üzembe helyezése a GitHub-tárházban projektet tartalmaz."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 32739607-85fe-43c8-a4dc-1feb46d93a4d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: cephalin
ms.openlocfilehash: 77064802814296d0c21f004534e4264d2f97252e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-web-app-linked-to-a-github-repository"></a><span data-ttu-id="093e5-103">A GitHub-tárházban kapcsolódó webalkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="093e5-103">Deploy a web app linked to a GitHub repository</span></span>
<span data-ttu-id="093e5-104">Ebben a témakörben, megtudhatja, hogyan egy Azure Resource Manager-sablon, amely telepít egy webalkalmazást, amely csatolva van egy GitHub-tárházban a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="093e5-104">In this topic, you will learn how to create an Azure Resource Manager template that deploys a web app that is linked to a project in a GitHub repository.</span></span> <span data-ttu-id="093e5-105">Megtudhatja, hogyan határozza meg, mely erőforrásokat központilag telepíti, és hogyan adhat meg a paramétereket, amelyek megadott, amikor a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="093e5-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="093e5-106">Ez a sablont használhatja a saját környezeteiben, vagy testre is szabhatja a saját követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="093e5-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="093e5-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="093e5-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="093e5-108">Tekintse meg a teljes sablon [Web App csatolt GitHub sablonhoz](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="093e5-108">For the complete template, see [Web App Linked to GitHub template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a><span data-ttu-id="093e5-109">Mit fog üzembe helyezni</span><span class="sxs-lookup"><span data-stu-id="093e5-109">What you will deploy</span></span>
<span data-ttu-id="093e5-110">Ezzel a sablonnal a projektből a Githubon kódot tartalmazó webalkalmazás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="093e5-110">With this template, you will deploy a web app that contains the code from a project in GitHub.</span></span>

<span data-ttu-id="093e5-111">Az automatikus üzembe helyezéshez kattintson az alábbi gombra:</span><span class="sxs-lookup"><span data-stu-id="093e5-111">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="093e5-112">[![Üzembe helyezés az Azure-ban](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="093e5-112">[![Deploy to Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="093e5-113">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="093e5-113">Parameters</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a><span data-ttu-id="093e5-114">repoURL</span><span class="sxs-lookup"><span data-stu-id="093e5-114">repoURL</span></span>
<span data-ttu-id="093e5-115">A projekt telepítése tartalmazó GitHub-tárházban URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="093e5-115">The URL for GitHub repository that contains the project to deploy.</span></span> <span data-ttu-id="093e5-116">Ez a paraméter alapértelmezett értéket tartalmaz, de ez az érték csak olyan bemutatják a tárház URL-CÍMÉT adja meg.</span><span class="sxs-lookup"><span data-stu-id="093e5-116">This parameter contains a default value but this value is only intended to show you how to provide the URL for repository.</span></span> <span data-ttu-id="093e5-117">Is használhatja ezt az értéket, a sablon tesztelése során, de a rendszer szeretne biztosítani a saját tárház URL-CÍMÉT az a sablon használatakor.</span><span class="sxs-lookup"><span data-stu-id="093e5-117">You can use this value when testing the template but you will want to provide the URL your own repository when working with the template.</span></span>

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a><span data-ttu-id="093e5-118">Fiókiroda</span><span class="sxs-lookup"><span data-stu-id="093e5-118">branch</span></span>
<span data-ttu-id="093e5-119">A tárház az alkalmazás telepítése során használandó helyezendő ága.</span><span class="sxs-lookup"><span data-stu-id="093e5-119">The branch of the repository to use when deploying the application.</span></span> <span data-ttu-id="093e5-120">Az alapértelmezett érték a főadatbázis, de megadhatja a telepíteni kívánt tárházban bármely ág nevét.</span><span class="sxs-lookup"><span data-stu-id="093e5-120">The default value is master, but you can provide the name of any branch in the repository that you wish to deploy.</span></span>

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-to-deploy"></a><span data-ttu-id="093e5-121">Üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="093e5-121">Resources to deploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a><span data-ttu-id="093e5-122">Webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="093e5-122">Web app</span></span>
<span data-ttu-id="093e5-123">Létrehozza a webalkalmazást, amely csatolva van a projektet a Githubról.</span><span class="sxs-lookup"><span data-stu-id="093e5-123">Creates the web app that is linked to the project in GitHub.</span></span> 

<span data-ttu-id="093e5-124">A nevet a webalkalmazás keresztül megadnia a **siteName** paraméter, és a web app használatával helye a **siteLocation** paraméter.</span><span class="sxs-lookup"><span data-stu-id="093e5-124">You specify the name of the web app through the **siteName** parameter, and the location of the web app through the **siteLocation** parameter.</span></span> <span data-ttu-id="093e5-125">Az a **dependsOn** elem, a sablon határozza meg a webalkalmazás az üzemeltetési terv szolgáltatástól függ.</span><span class="sxs-lookup"><span data-stu-id="093e5-125">In the **dependsOn** element, the template defines the web app as dependent on the service hosting plan.</span></span> <span data-ttu-id="093e5-126">Mivel a üzemeltetési terv függ, a webes alkalmazás nem jön létre, amíg a üzemeltetési terv létrehozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="093e5-126">Because it is dependent on the hosting plan, the web app is not created until the hosting plan has finished being created.</span></span> <span data-ttu-id="093e5-127">A **dependsOn** elem csak használatával adja meg a telepítési sorrendet.</span><span class="sxs-lookup"><span data-stu-id="093e5-127">The **dependsOn** element is only used to specify deployment order.</span></span> <span data-ttu-id="093e5-128">Ha nem jelöli be a üzemeltetési terv függ a webes alkalmazást, Azure Resource Manager megkísérli mindkét erőforrás létrehozása egy időben, és hiba jelenhet meg, ha a webalkalmazás létrehozása előtt a üzemeltetési terv.</span><span class="sxs-lookup"><span data-stu-id="093e5-128">If you do not mark the web app as dependent on the hosting plan, Azure Resource Mananger will attempt to create both resources at the same time and you may receive an error if the web app is created before the hosting plan.</span></span>

<span data-ttu-id="093e5-129">A webes alkalmazás is van egy gyermek erőforrás, amelyet a **erőforrások** az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="093e5-129">The web app also has a child resource which is defined in **resources** section below.</span></span> <span data-ttu-id="093e5-130">A gyermek-erőforrás határozza meg a projekt telepítik a webalkalmazás verziókövetését.</span><span class="sxs-lookup"><span data-stu-id="093e5-130">This child resource defines source control for the project deployed with the web app.</span></span> <span data-ttu-id="093e5-131">A sablonban a verziókezelőt egy adott GitHub-tárházban van csatolva.</span><span class="sxs-lookup"><span data-stu-id="093e5-131">In this template, the source control is linked to a particular GitHub repository.</span></span> <span data-ttu-id="093e5-132">A GitHub-tárházban van definiálva a kód **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** , előfordulhat, hogy kódolnia a tárház URL-CÍMÉT Ha azt szeretné, amely többször telepít egy sablon létrehozása a projekt közben igénylő paraméterek minimális száma.</span><span class="sxs-lookup"><span data-stu-id="093e5-132">The GitHub repository is defined with the code **"RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git"** You might hard-code the repository URL when you want to create a template that repeatedly deploys a single project while requiring the minimum number of parameters.</span></span>
<span data-ttu-id="093e5-133">Rögzített megadás a tárház URL-CÍMÉT, helyett paraméter hozzáadása a tárház URL-címhez és használja ezt az értéket a a **RepoUrl** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="093e5-133">Instead of hard-coding the repository URL, you can add a parameter for the repository URL and use that value for the **RepoUrl** property.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a><span data-ttu-id="093e5-134">Az üzembe helyezést futtató parancsok</span><span class="sxs-lookup"><span data-stu-id="093e5-134">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="093e5-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="093e5-135">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="093e5-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="093e5-136">Azure CLI</span></span>

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a><span data-ttu-id="093e5-137">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="093e5-137">Azure CLI 2.0</span></span>

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> <span data-ttu-id="093e5-138">A paraméterek JSON-fájl tartalmát, lásd: [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span><span class="sxs-lookup"><span data-stu-id="093e5-138">For content of the parameters JSON file, see [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span></span>
>
>

