---
title: "aaaAzure PowerShell parancsfájl minta - webalkalmazás létrehozása és központi telepítése a kódot egy helyi Git-tárház |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása és központi telepítése egy helyi Git-tárház kódot"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a90996658bf0b609315460324d0dcd3a411a6512
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="08db7-103">A webalkalmazás létrehozása és telepítése a kódot egy helyi Git-tárházat</span><span class="sxs-lookup"><span data-stu-id="08db7-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="08db7-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és ezután telepíti a webes alkalmazás kódja egy helyi Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="08db7-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>

<span data-ttu-id="08db7-105">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="08db7-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> <span data-ttu-id="08db7-106">Az alkalmazás kódjában is, egy helyi Git-tárházba való véglegesítése toobe kell.</span><span class="sxs-lookup"><span data-stu-id="08db7-106">Also, your application code needs toobe committed into a local Git repository.</span></span>

## <a name="sample-script"></a><span data-ttu-id="08db7-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="08db7-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

## <a name="clean-up-deployment"></a><span data-ttu-id="08db7-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="08db7-108">Clean up deployment</span></span> 

<span data-ttu-id="08db7-109">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="08db7-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="08db7-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="08db7-110">Script explanation</span></span>

<span data-ttu-id="08db7-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="08db7-111">This script uses hello following commands.</span></span> <span data-ttu-id="08db7-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="08db7-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="08db7-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="08db7-113">Command</span></span> | <span data-ttu-id="08db7-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="08db7-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="08db7-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="08db7-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="08db7-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="08db7-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="08db7-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="08db7-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="08db7-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="08db7-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="08db7-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="08db7-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="08db7-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="08db7-120">Creates a web app.</span></span> |
| [<span data-ttu-id="08db7-121">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="08db7-121">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="08db7-122">Módosítja egy erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="08db7-122">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="08db7-123">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="08db7-123">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="08db7-124">A webes alkalmazás közzétételi profil beolvasása.</span><span class="sxs-lookup"><span data-stu-id="08db7-124">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="08db7-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08db7-125">Next steps</span></span>

<span data-ttu-id="08db7-126">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="08db7-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="08db7-127">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="08db7-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
