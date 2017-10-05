---
title: "Az Azure PowerShell-parancsfájl minta - kötés egy egyéni az SSL-tanúsítványt egy |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - kötés egy egyéni SSL-tanúsítvány"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: de8deccadcd9571be75447a117888bf2b3f571a0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a><span data-ttu-id="8be17-103">Egyéni SSL-tanúsítvány kötése a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="8be17-103">Bind a custom SSL certificate to a web app</span></span>

<span data-ttu-id="8be17-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, majd az SSL-tanúsítvány egy egyéni tartománynév kötődik.</span><span class="sxs-lookup"><span data-stu-id="8be17-104">This sample script creates a web app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> 

<span data-ttu-id="8be17-105">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8be17-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="8be17-106">Emellett győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="8be17-106">Also, ensure that:</span></span>

- <span data-ttu-id="8be17-107">A kapcsolat az Azure használatával lett létrehozva a `az login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8be17-107">A connection with Azure has been created using the `az login` command.</span></span>
- <span data-ttu-id="8be17-108">Rendelkezik hozzáféréssel a tartományregisztráló DNS-konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="8be17-108">You have access to your domain registrar's DNS configuration page.</span></span>
- <span data-ttu-id="8be17-109">Lehetősége van egy érvényes. PFX-fájlt és a jelszót, az SSL-tanúsítvány feltöltése és kötni kívánt.</span><span class="sxs-lookup"><span data-stu-id="8be17-109">You have a valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

## <a name="sample-script"></a><span data-ttu-id="8be17-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="8be17-110">Sample script</span></span>

<span data-ttu-id="8be17-111">[!code-powershell[fő](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "egyéni SSL-tanúsítvány kötése a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="8be17-111">[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8be17-112">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8be17-112">Clean up deployment</span></span> 

<span data-ttu-id="8be17-113">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="8be17-113">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="8be17-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="8be17-114">Script explanation</span></span>

<span data-ttu-id="8be17-115">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="8be17-115">This script uses the following commands.</span></span> <span data-ttu-id="8be17-116">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="8be17-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8be17-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="8be17-117">Command</span></span> | <span data-ttu-id="8be17-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8be17-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8be17-119">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8be17-119">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="8be17-120">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8be17-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8be17-121">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="8be17-121">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="8be17-122">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8be17-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="8be17-123">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="8be17-123">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="8be17-124">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8be17-124">Creates a web app.</span></span> |
| [<span data-ttu-id="8be17-125">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="8be17-125">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="8be17-126">Módosítja egy App Service-csomag a tarifacsomag-váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="8be17-126">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="8be17-127">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="8be17-127">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="8be17-128">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="8be17-128">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="8be17-129">Új AzureRmWebAppSSLBinding</span><span class="sxs-lookup"><span data-stu-id="8be17-129">New-AzureRmWebAppSSLBinding</span></span>](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | <span data-ttu-id="8be17-130">Létrehoz egy SSL-tanúsítvány kötésének egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8be17-130">Creates an SSL certificate binding for a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8be17-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8be17-131">Next steps</span></span>

<span data-ttu-id="8be17-132">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8be17-132">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="8be17-133">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8be17-133">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
