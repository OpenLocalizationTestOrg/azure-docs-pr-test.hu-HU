---
title: "PowerShell parancsfájl minta - feltöltési fájlok tooa web app használatával FTP aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - feltöltési fájlok tooa web app használatával FTP"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: b7d46d6f-44fd-454c-8008-87dab6eefbc1
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 32a0a529e94c1315cc6730faf23fca2693c50ffb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-tooa-web-app-using-ftp"></a><span data-ttu-id="9fdb3-103">Fájlok tooa web app használatával FTP feltöltése</span><span class="sxs-lookup"><span data-stu-id="9fdb3-103">Upload files tooa web app using FTP</span></span>

<span data-ttu-id="9fdb3-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és a webes alkalmazás kódja FTP-használ, majd telepíti (keresztül [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span><span class="sxs-lookup"><span data-stu-id="9fdb3-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code using FTP (via [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span></span>

<span data-ttu-id="9fdb3-105">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="9fdb3-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9fdb3-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "Upload files tooa web app using FTP")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9fdb3-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9fdb3-107">Clean up deployment</span></span> 

<span data-ttu-id="9fdb3-108">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-108">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="9fdb3-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9fdb3-109">Script explanation</span></span>

<span data-ttu-id="9fdb3-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-110">This script uses hello following commands.</span></span> <span data-ttu-id="9fdb3-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9fdb3-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="9fdb3-112">Command</span></span> | <span data-ttu-id="9fdb3-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9fdb3-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9fdb3-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9fdb3-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="9fdb3-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9fdb3-116">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="9fdb3-116">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="9fdb3-117">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="9fdb3-118">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="9fdb3-118">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="9fdb3-119">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-119">Creates a web app.</span></span> |
| [<span data-ttu-id="9fdb3-120">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="9fdb3-120">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="9fdb3-121">A webes alkalmazás közzétételi profil beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9fdb3-121">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9fdb3-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9fdb3-122">Next steps</span></span>

<span data-ttu-id="9fdb3-123">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9fdb3-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9fdb3-124">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9fdb3-124">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
