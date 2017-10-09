---
title: "a webes alkalmazás, amely aaaDeploy kapcsolódó tooa GitHub-tárházban |} Microsoft Docs"
description: "Használja az Azure Resource Manager sablon toodeploy egy webalkalmazást, a GitHub-tárházban projektet tartalmaz."
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
ms.openlocfilehash: 8b23416c4c06a60991517e6ee4cd82bebc5a9d73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-linked-tooa-github-repository"></a><span data-ttu-id="dab9b-103">A webes alkalmazás kapcsolódó tooa GitHub-tárházban telepítése</span><span class="sxs-lookup"><span data-stu-id="dab9b-103">Deploy a web app linked tooa GitHub repository</span></span>
<span data-ttu-id="dab9b-104">Ebben a témakörben megismerheti, hogyan toocreate egy Azure Resource Manager-sablon, amely webes alkalmazás központi telepítését végző kapcsolódó tooa projektre a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="dab9b-104">In this topic, you will learn how toocreate an Azure Resource Manager template that deploys a web app that is linked tooa project in a GitHub repository.</span></span> <span data-ttu-id="dab9b-105">Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="dab9b-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="dab9b-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="dab9b-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="dab9b-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="dab9b-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="dab9b-108">Hello teljes sablon, lásd: [Web App csatolt tooGitHub sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="dab9b-108">For hello complete template, see [Web App Linked tooGitHub template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a><span data-ttu-id="dab9b-109">Mit fog üzembe helyezni</span><span class="sxs-lookup"><span data-stu-id="dab9b-109">What you will deploy</span></span>
<span data-ttu-id="dab9b-110">Ezzel a sablonnal a projektből a Githubon hello kódot tartalmazó webalkalmazás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="dab9b-110">With this template, you will deploy a web app that contains hello code from a project in GitHub.</span></span>

<span data-ttu-id="dab9b-111">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="dab9b-111">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="dab9b-112">[![TooAzure telepítése](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="dab9b-112">[![Deploy tooAzure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="dab9b-113">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="dab9b-113">Parameters</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a><span data-ttu-id="dab9b-114">repoURL</span><span class="sxs-lookup"><span data-stu-id="dab9b-114">repoURL</span></span>
<span data-ttu-id="dab9b-115">hello hello projekt toodeploy tartalmazó GitHub-tárház URL-címe</span><span class="sxs-lookup"><span data-stu-id="dab9b-115">hello URL for GitHub repository that contains hello project toodeploy.</span></span> <span data-ttu-id="dab9b-116">Ez a paraméter alapértelmezett értéket tartalmaz, de ez az érték csak a megfelelő tooshow, hogyan tooprovide hello tárház URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="dab9b-116">This parameter contains a default value but this value is only intended tooshow you how tooprovide hello URL for repository.</span></span> <span data-ttu-id="dab9b-117">Ezt az értéket is használhatja, ha a tesztelés hello sablon, de érdemes tooprovide hello URL-cím a tárház az hello sablon használatakor.</span><span class="sxs-lookup"><span data-stu-id="dab9b-117">You can use this value when testing hello template but you will want tooprovide hello URL your own repository when working with hello template.</span></span>

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a><span data-ttu-id="dab9b-118">Fiókiroda</span><span class="sxs-lookup"><span data-stu-id="dab9b-118">branch</span></span>
<span data-ttu-id="dab9b-119">hello a ága hello tárház toouse hello alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="dab9b-119">hello branch of hello repository toouse when deploying hello application.</span></span> <span data-ttu-id="dab9b-120">hello alapértelmezett értéke fő, de megadhat bármilyen ág hello tárházban hello nevét, hogy kívánja-e toodeploy.</span><span class="sxs-lookup"><span data-stu-id="dab9b-120">hello default value is master, but you can provide hello name of any branch in hello repository that you wish toodeploy.</span></span>

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-toodeploy"></a><span data-ttu-id="dab9b-121">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="dab9b-121">Resources toodeploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a><span data-ttu-id="dab9b-122">Webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="dab9b-122">Web app</span></span>
<span data-ttu-id="dab9b-123">A Githubon csatolt toohello projekt hello webalkalmazás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dab9b-123">Creates hello web app that is linked toohello project in GitHub.</span></span> 

<span data-ttu-id="dab9b-124">Hello webalkalmazás keresztül hello hello nevének megadása **siteName** paraméter és a webalkalmazás hello keresztül hello hello hely **siteLocation** paraméter.</span><span class="sxs-lookup"><span data-stu-id="dab9b-124">You specify hello name of hello web app through hello **siteName** parameter, and hello location of hello web app through hello **siteLocation** parameter.</span></span> <span data-ttu-id="dab9b-125">A hello **dependsOn** elem, hello sablon meghatározása hello webalkalmazás szerint üzemeltetési terv hello szolgáltatás függ.</span><span class="sxs-lookup"><span data-stu-id="dab9b-125">In hello **dependsOn** element, hello template defines hello web app as dependent on hello service hosting plan.</span></span> <span data-ttu-id="dab9b-126">Mivel üzemeltetési terv hello függ, hello webalkalmazás nem jön létre, amíg hello üzemeltetési terv létrehozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="dab9b-126">Because it is dependent on hello hosting plan, hello web app is not created until hello hosting plan has finished being created.</span></span> <span data-ttu-id="dab9b-127">Hello **dependsOn** eleme csak a felhasznált toospecify telepítési sorrenddel.</span><span class="sxs-lookup"><span data-stu-id="dab9b-127">hello **dependsOn** element is only used toospecify deployment order.</span></span> <span data-ttu-id="dab9b-128">Ha nem jelöli be hello webes alkalmazást hello üzemeltetési terv függ, Azure Resource Manager megkísérli toocreate mindkét erőforrás: hello azonos idő, és egy hibaüzenet jelenik Ha hello webalkalmazás üzemeltetési terv hello előtt hozza létre.</span><span class="sxs-lookup"><span data-stu-id="dab9b-128">If you do not mark hello web app as dependent on hello hosting plan, Azure Resource Mananger will attempt toocreate both resources at hello same time and you may receive an error if hello web app is created before hello hosting plan.</span></span>

<span data-ttu-id="dab9b-129">hello webes alkalmazás is van egy gyermek erőforrás, amelyet a **erőforrások** az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="dab9b-129">hello web app also has a child resource which is defined in **resources** section below.</span></span> <span data-ttu-id="dab9b-130">A gyermek-erőforrás hello projekt telepítik hello webalkalmazás verziókövetését határozza meg.</span><span class="sxs-lookup"><span data-stu-id="dab9b-130">This child resource defines source control for hello project deployed with hello web app.</span></span> <span data-ttu-id="dab9b-131">A sablonban hello verziókezelő rendszer csatolt tooa adott GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="dab9b-131">In this template, hello source control is linked tooa particular GitHub repository.</span></span> <span data-ttu-id="dab9b-132">hello GitHub-tárházban hello kódra van definiálva **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** kódolnia hello tárház URL-cím lehet, ha azt szeretné, hogy a sablont, amely többször telepíti toocreate egy egyetlen projekt közben igénylő hello paraméterek minimális száma.</span><span class="sxs-lookup"><span data-stu-id="dab9b-132">hello GitHub repository is defined with hello code **"RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git"** You might hard-code hello repository URL when you want toocreate a template that repeatedly deploys a single project while requiring hello minimum number of parameters.</span></span>
<span data-ttu-id="dab9b-133">Rögzített megadás helyett hello tárház URL-CÍMÉT, a paraméter hozzáadása hello tárház URL-címhez, és használja ezt az értéket hello **RepoUrl** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="dab9b-133">Instead of hard-coding hello repository URL, you can add a parameter for hello repository URL and use that value for hello **RepoUrl** property.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="dab9b-134">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="dab9b-134">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="dab9b-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dab9b-135">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="dab9b-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="dab9b-136">Azure CLI</span></span>

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a><span data-ttu-id="dab9b-137">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="dab9b-137">Azure CLI 2.0</span></span>

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> <span data-ttu-id="dab9b-138">Hello paraméterek JSON-fájl tartalmát, lásd: [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span><span class="sxs-lookup"><span data-stu-id="dab9b-138">For content of hello parameters JSON file, see [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span></span>
>
>

