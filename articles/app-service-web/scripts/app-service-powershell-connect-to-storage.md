---
title: "PowerShell parancsfájl minta - aaaAzure csatlakozzon a webes alkalmazás tooa storage-fiók |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - csatlakozás a webes alkalmazás tooa storage-fiók"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4831bdc-2068-4883-9474-0b34c2e3e255
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 07693366d32fbaefe92c18df67ded81661e1a2df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-storage-account"></a><span data-ttu-id="4635b-103">Csatlakozás a webes alkalmazás tooa storage-fiók</span><span class="sxs-lookup"><span data-stu-id="4635b-103">Connect a web app tooa storage account</span></span>

<span data-ttu-id="4635b-104">Ebben a forgatókönyvben megtudhatja, hogyan toocreate egy Azure storage-fiókot és egy Azure webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4635b-104">In this scenario you will learn how toocreate an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="4635b-105">Majd hello tárolási fiók toohello web app használatával Alkalmazásbeállítások csatolást.</span><span class="sxs-lookup"><span data-stu-id="4635b-105">Then you will link hello storage account toohello web app using app settings.</span></span>

<span data-ttu-id="4635b-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="4635b-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="4635b-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4635b-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app tooa storage account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4635b-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4635b-108">Clean up deployment</span></span> 

<span data-ttu-id="4635b-109">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="4635b-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="4635b-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4635b-110">Script explanation</span></span>

<span data-ttu-id="4635b-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="4635b-111">This script uses hello following commands.</span></span> <span data-ttu-id="4635b-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="4635b-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4635b-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="4635b-113">Command</span></span> | <span data-ttu-id="4635b-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4635b-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4635b-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4635b-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="4635b-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4635b-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4635b-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="4635b-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="4635b-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4635b-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="4635b-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="4635b-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="4635b-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4635b-120">Creates a web app.</span></span> |
| [<span data-ttu-id="4635b-121">Új-AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="4635b-121">New-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="4635b-122">Létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="4635b-122">Creates a Storage account.</span></span> |
| [<span data-ttu-id="4635b-123">Get-AzureRMStorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="4635b-123">Get-AzureRMStorageAccountKey</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | <span data-ttu-id="4635b-124">Lekérdezi az Azure Storage-fiók hello hozzáférési kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="4635b-124">Gets hello access keys for an Azure Storage account.</span></span> |
| [<span data-ttu-id="4635b-125">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="4635b-125">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="4635b-126">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="4635b-126">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4635b-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4635b-127">Next steps</span></span>

<span data-ttu-id="4635b-128">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4635b-128">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="4635b-129">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4635b-129">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
