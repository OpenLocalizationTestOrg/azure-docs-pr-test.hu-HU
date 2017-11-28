---
title: "aaaAzure PowerShell parancsfájl minta - webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról |} Microsoft Docs"
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
ms.openlocfilehash: c2f260a06bce9af6d11ad4033931d3dc18da8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="b87f8-103">Webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról</span><span class="sxs-lookup"><span data-stu-id="b87f8-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="b87f8-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és beállítja egy GitHub-tárházban folyamatos üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="b87f8-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="b87f8-105">Folyamatos üzembe helyezés nélkül GitHub telepítését: [webalkalmazás létrehozása és központi telepítése a Githubból kód](app-service-powershell-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="b87f8-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-powershell-deploy-github.md).</span></span>

<span data-ttu-id="b87f8-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b87f8-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="b87f8-107">Emellett győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="b87f8-107">Also, ensure that:</span></span>

- <span data-ttu-id="b87f8-108">A kapcsolat az Azure-ral hello használatával lett létrehozva `az login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b87f8-108">A connection with Azure has been created using hello `az login` command.</span></span>
- <span data-ttu-id="b87f8-109">hello alkalmazáskód van egy nyilvános vagy privát GitHub-tárházban, hogy Ön a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="b87f8-109">hello application code is in a public or private GitHub repository that you own.</span></span>
- <span data-ttu-id="b87f8-110">Rendelkezik [olyan hozzáférési jogkivonatot GitHub-fiókjában létrehozott](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span><span class="sxs-lookup"><span data-stu-id="b87f8-110">You have [created an access token in your GitHub account](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span></span>

## <a name="sample-script"></a><span data-ttu-id="b87f8-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b87f8-111">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b87f8-112">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b87f8-112">Clean up deployment</span></span> 

<span data-ttu-id="b87f8-113">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b87f8-113">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="b87f8-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b87f8-114">Script explanation</span></span>

<span data-ttu-id="b87f8-115">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="b87f8-115">This script uses hello following commands.</span></span> <span data-ttu-id="b87f8-116">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="b87f8-116">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b87f8-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="b87f8-117">Command</span></span> | <span data-ttu-id="b87f8-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b87f8-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b87f8-119">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b87f8-119">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="b87f8-120">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b87f8-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b87f8-121">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="b87f8-121">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="b87f8-122">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b87f8-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="b87f8-123">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="b87f8-123">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="b87f8-124">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b87f8-124">Creates a web app.</span></span> |
| [<span data-ttu-id="b87f8-125">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="b87f8-125">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="b87f8-126">Módosítja egy erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="b87f8-126">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b87f8-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b87f8-127">Next steps</span></span>

<span data-ttu-id="b87f8-128">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b87f8-128">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b87f8-129">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b87f8-129">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
