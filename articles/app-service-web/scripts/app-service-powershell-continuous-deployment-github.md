---
title: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: fc594a94bb64ceb88370be8617036a73402ade4e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="1ab8d-103">Webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról</span><span class="sxs-lookup"><span data-stu-id="1ab8d-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="1ab8d-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és beállítja egy GitHub-tárházban folyamatos üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="1ab8d-105">Folyamatos üzembe helyezés nélkül GitHub telepítését: [webalkalmazás létrehozása és központi telepítése a Githubból kód](app-service-powershell-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="1ab8d-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-powershell-deploy-github.md).</span></span>

<span data-ttu-id="1ab8d-106">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1ab8d-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="1ab8d-107">Emellett győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="1ab8d-107">Also, ensure that:</span></span>

- <span data-ttu-id="1ab8d-108">A kapcsolat az Azure használatával lett létrehozva a `az login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-108">A connection with Azure has been created using the `az login` command.</span></span>
- <span data-ttu-id="1ab8d-109">Az alkalmazás kódjának van egy nyilvános vagy privát GitHub-tárházban, hogy Ön a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-109">The application code is in a public or private GitHub repository that you own.</span></span>
- <span data-ttu-id="1ab8d-110">Rendelkezik [olyan hozzáférési jogkivonatot GitHub-fiókjában létrehozott](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span><span class="sxs-lookup"><span data-stu-id="1ab8d-110">You have [created an access token in your GitHub account](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span></span>

## <a name="sample-script"></a><span data-ttu-id="1ab8d-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1ab8d-111">Sample script</span></span>

<span data-ttu-id="1ab8d-112">[!code-powershell[fő](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról")]</span><span class="sxs-lookup"><span data-stu-id="1ab8d-112">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="1ab8d-113">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1ab8d-113">Clean up deployment</span></span> 

<span data-ttu-id="1ab8d-114">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-114">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="1ab8d-115">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1ab8d-115">Script explanation</span></span>

<span data-ttu-id="1ab8d-116">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-116">This script uses the following commands.</span></span> <span data-ttu-id="1ab8d-117">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1ab8d-118">Parancs</span><span class="sxs-lookup"><span data-stu-id="1ab8d-118">Command</span></span> | <span data-ttu-id="1ab8d-119">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1ab8d-119">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1ab8d-120">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1ab8d-120">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="1ab8d-121">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1ab8d-122">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="1ab8d-122">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="1ab8d-123">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-123">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="1ab8d-124">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="1ab8d-124">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="1ab8d-125">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-125">Creates a web app.</span></span> |
| [<span data-ttu-id="1ab8d-126">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="1ab8d-126">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="1ab8d-127">Módosítja egy erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="1ab8d-127">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1ab8d-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ab8d-128">Next steps</span></span>

<span data-ttu-id="1ab8d-129">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1ab8d-129">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1ab8d-130">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1ab8d-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
