---
title: "PowerShell parancsfájl minta - aaaAzure kötés SSL tanúsítvány tooa egyéni webalkalmazás |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - kötés egy egyéni SSL tanúsítvány tooa webalkalmazás"
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
ms.openlocfilehash: 6e249ceedb9e2b8872dd0bc8b0aea0d718b28dab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a><span data-ttu-id="91e19-103">Egy egyéni SSL tanúsítvány tooa webalkalmazás kötése</span><span class="sxs-lookup"><span data-stu-id="91e19-103">Bind a custom SSL certificate tooa web app</span></span>

<span data-ttu-id="91e19-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, majd egy egyéni tartomány nevét tooit hello SSL-tanúsítvány van kötve.</span><span class="sxs-lookup"><span data-stu-id="91e19-104">This sample script creates a web app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> 

<span data-ttu-id="91e19-105">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="91e19-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="91e19-106">Emellett győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="91e19-106">Also, ensure that:</span></span>

- <span data-ttu-id="91e19-107">A kapcsolat az Azure-ral hello használatával lett létrehozva `az login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="91e19-107">A connection with Azure has been created using hello `az login` command.</span></span>
- <span data-ttu-id="91e19-108">Lehetősége van a hozzáférés tooyour tartomány regisztráló DNS-konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="91e19-108">You have access tooyour domain registrar's DNS configuration page.</span></span>
- <span data-ttu-id="91e19-109">Lehetősége van egy érvényes. PFX-fájlt és a jelszó hello SSL tanúsítvány tooupload szeretné, majd kötést.</span><span class="sxs-lookup"><span data-stu-id="91e19-109">You have a valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

## <a name="sample-script"></a><span data-ttu-id="91e19-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="91e19-110">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate tooa web app")]

## <a name="clean-up-deployment"></a><span data-ttu-id="91e19-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="91e19-111">Clean up deployment</span></span> 

<span data-ttu-id="91e19-112">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="91e19-112">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="91e19-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="91e19-113">Script explanation</span></span>

<span data-ttu-id="91e19-114">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="91e19-114">This script uses hello following commands.</span></span> <span data-ttu-id="91e19-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="91e19-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="91e19-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="91e19-116">Command</span></span> | <span data-ttu-id="91e19-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="91e19-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="91e19-118">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="91e19-118">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="91e19-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="91e19-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="91e19-120">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="91e19-120">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="91e19-121">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="91e19-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="91e19-122">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="91e19-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="91e19-123">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91e19-123">Creates a web app.</span></span> |
| [<span data-ttu-id="91e19-124">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="91e19-124">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="91e19-125">Az App Service-csomag toochange a tarifacsomag módosítása.</span><span class="sxs-lookup"><span data-stu-id="91e19-125">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="91e19-126">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="91e19-126">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="91e19-127">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="91e19-127">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="91e19-128">Új AzureRmWebAppSSLBinding</span><span class="sxs-lookup"><span data-stu-id="91e19-128">New-AzureRmWebAppSSLBinding</span></span>](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | <span data-ttu-id="91e19-129">Létrehoz egy SSL-tanúsítvány kötésének egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91e19-129">Creates an SSL certificate binding for a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="91e19-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91e19-130">Next steps</span></span>

<span data-ttu-id="91e19-131">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="91e19-131">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="91e19-132">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="91e19-132">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
