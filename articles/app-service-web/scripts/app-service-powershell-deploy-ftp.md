---
title: "Az Azure PowerShell-parancsfájl minta - fájlokat egy FTP-használ, a webes alkalmazás |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - fájlokat egy FTP-használ, a webes alkalmazás"
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
ms.openlocfilehash: 96b99110b63b037746fcc40eb15db5d718eb71a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="upload-files-to-a-web-app-using-ftp"></a><span data-ttu-id="41c20-103">Fájlok feltöltése a webes alkalmazás FTP használatával</span><span class="sxs-lookup"><span data-stu-id="41c20-103">Upload files to a web app using FTP</span></span>

<span data-ttu-id="41c20-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és a webes alkalmazás kódja FTP-használ, majd telepíti (keresztül [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span><span class="sxs-lookup"><span data-stu-id="41c20-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code using FTP (via [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span></span>

<span data-ttu-id="41c20-105">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="41c20-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="41c20-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="41c20-106">Sample script</span></span>

<span data-ttu-id="41c20-107">[!code-powershell[fő](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "tölt fel egy FTP-használ, a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="41c20-107">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "Upload files to a web app using FTP")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="41c20-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="41c20-108">Clean up deployment</span></span> 

<span data-ttu-id="41c20-109">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="41c20-109">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="41c20-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="41c20-110">Script explanation</span></span>

<span data-ttu-id="41c20-111">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="41c20-111">This script uses the following commands.</span></span> <span data-ttu-id="41c20-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="41c20-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="41c20-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="41c20-113">Command</span></span> | <span data-ttu-id="41c20-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="41c20-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="41c20-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="41c20-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="41c20-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="41c20-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="41c20-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="41c20-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="41c20-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="41c20-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="41c20-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="41c20-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="41c20-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="41c20-120">Creates a web app.</span></span> |
| [<span data-ttu-id="41c20-121">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="41c20-121">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="41c20-122">A webes alkalmazás közzétételi profil beolvasása.</span><span class="sxs-lookup"><span data-stu-id="41c20-122">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="41c20-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41c20-123">Next steps</span></span>

<span data-ttu-id="41c20-124">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="41c20-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="41c20-125">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="41c20-125">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
