---
title: "Az Azure PowerShell-parancsfájl minta - tárfiókba webes alkalmazás csatlakoztatása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - tárfiókba webes alkalmazás csatlakoztatása"
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
ms.openlocfilehash: 481f3efdb1cbbeba328183da7e320c7e5b819b3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-a-web-app-to-a-storage-account"></a><span data-ttu-id="aa5ba-103">Webes alkalmazás csatlakoztatása a tárolási fiók</span><span class="sxs-lookup"><span data-stu-id="aa5ba-103">Connect a web app to a storage account</span></span>

<span data-ttu-id="aa5ba-104">Ebben a forgatókönyvben, megtudhatja, hogyan hozzon létre egy Azure storage-fiókot és egy Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-104">In this scenario you will learn how to create an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="aa5ba-105">Majd a tárfiók összekapcsolja a web app alkalmazást beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-105">Then you will link the storage account to the web app using app settings.</span></span>

<span data-ttu-id="aa5ba-106">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="aa5ba-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="aa5ba-107">Sample script</span></span>

<span data-ttu-id="aa5ba-108">[!code-powershell[fő](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "tárfiókba webes alkalmazás csatlakoztatása")]</span><span class="sxs-lookup"><span data-stu-id="aa5ba-108">[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app to a storage account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="aa5ba-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="aa5ba-109">Clean up deployment</span></span> 

<span data-ttu-id="aa5ba-110">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="aa5ba-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="aa5ba-111">Script explanation</span></span>

<span data-ttu-id="aa5ba-112">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-112">This script uses the following commands.</span></span> <span data-ttu-id="aa5ba-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="aa5ba-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="aa5ba-114">Command</span></span> | <span data-ttu-id="aa5ba-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="aa5ba-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="aa5ba-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="aa5ba-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="aa5ba-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="aa5ba-118">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="aa5ba-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="aa5ba-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="aa5ba-120">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="aa5ba-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="aa5ba-121">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-121">Creates a web app.</span></span> |
| [<span data-ttu-id="aa5ba-122">Új-AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="aa5ba-122">New-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="aa5ba-123">Létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-123">Creates a Storage account.</span></span> |
| [<span data-ttu-id="aa5ba-124">Get-AzureRMStorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="aa5ba-124">Get-AzureRMStorageAccountKey</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | <span data-ttu-id="aa5ba-125">Lekérdezi az Azure-tárfiók hozzáférési kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-125">Gets the access keys for an Azure Storage account.</span></span> |
| [<span data-ttu-id="aa5ba-126">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="aa5ba-126">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="aa5ba-127">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="aa5ba-127">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="aa5ba-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa5ba-128">Next steps</span></span>

<span data-ttu-id="aa5ba-129">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="aa5ba-129">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="aa5ba-130">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="aa5ba-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
