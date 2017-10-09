---
title: "aaaAzure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello új Azure Resource Manager-alapú PowerShell-parancsok toomanage az Azure Web Apps."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 4231fbba-61e5-4f92-b958-531c066fb87f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: bbb821e89daa315280436e84e11316217bb644d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-powershell-toomanage-azure-web-apps"></a><span data-ttu-id="37906-103">Azure Resource Manager-Based PowerShell tooManage Azure Web Apps használatával</span><span class="sxs-lookup"><span data-stu-id="37906-103">Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="37906-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="37906-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="37906-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="37906-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

<span data-ttu-id="37906-106">A Microsoft Azure PowerShell verzió 1.0.0 bővült, hello felhasználói hello képességét toouse Azure Resource Manager-alapú PowerShell parancsok toomanage Web Apps biztosítják, hogy új parancsok.</span><span class="sxs-lookup"><span data-stu-id="37906-106">With Microsoft Azure PowerShell version 1.0.0 new commands have been added, that give hello user hello ability toouse Azure Resource Manager-based PowerShell commands toomanage Web Apps.</span></span>

<span data-ttu-id="37906-107">toolearn erőforráscsoportok, kezelésével kapcsolatban lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="37906-107">toolearn about managing Resource Groups, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span> 

<span data-ttu-id="37906-108">toolearn paraméterek és beállítások hello PowerShell-parancsmagok teljes listája hello kapcsolatban lásd: hello [parancsmag-referencia a webes alkalmazás Azure Resource Manager-alapú PowerShell-parancsmagok teljes](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="37906-108">toolearn about hello full list of parameters and options for hello PowerShell cmdlets, see hello [full Cmdlet Reference of Web App Azure Resource Manager-based PowerShell Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>

## <a name="managing-app-service-plans"></a><span data-ttu-id="37906-109">Kezelés az App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="37906-109">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="37906-110">Az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="37906-110">Create an App Service Plan</span></span>
<span data-ttu-id="37906-111">az app service-csomag toocreate hello használata **New-AzureRmAppServicePlan** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="37906-111">toocreate an app service plan, use hello **New-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="37906-112">Alább olvasható hello más paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="37906-112">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="37906-113">**Név**: hello app service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="37906-113">**Name**: name of hello app service plan.</span></span>
* <span data-ttu-id="37906-114">**Hely**: csomag hely szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="37906-114">**Location**: service plan location.</span></span>
* <span data-ttu-id="37906-115">**ResourceGroupName**: erőforráscsoport, amely tartalmazza az újonnan létrehozott hello app service-csomag.</span><span class="sxs-lookup"><span data-stu-id="37906-115">**ResourceGroupName**: resource group that includes hello newly created app service plan.</span></span>
* <span data-ttu-id="37906-116">**Réteg**: hello szükséges tarifacsomag (alapértelmezett ingyenes, a közös, Basic, Standard és Premium is.)</span><span class="sxs-lookup"><span data-stu-id="37906-116">**Tier**:  hello desired pricing tier (Default is Free, other options are Shared, Basic, Standard, and Premium.)</span></span>
* <span data-ttu-id="37906-117">**WorkerSize**: hello mérete munkavállalók (az alapértelmezett érték kisebb, ha hello réteg paraméter van megadva, a Basic, Standard vagy prémium.</span><span class="sxs-lookup"><span data-stu-id="37906-117">**WorkerSize**: hello size of workers (Default is small if hello Tier parameter was specified as Basic, Standard, or Premium.</span></span> <span data-ttu-id="37906-118">Is, közepes és nagy.)</span><span class="sxs-lookup"><span data-stu-id="37906-118">Other options are Medium, and Large.)</span></span>
* <span data-ttu-id="37906-119">**NumberofWorkers**: hello munkavállalók száma az hello app service-csomag (alapértelmezett értéke 1).</span><span class="sxs-lookup"><span data-stu-id="37906-119">**NumberofWorkers**: hello number of workers in hello app service plan (Default value is 1).</span></span> 

<span data-ttu-id="37906-120">Példa toouse ezt a parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="37906-120">Example toouse this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a><span data-ttu-id="37906-121">Az App Service-csomag létrehozása az App Service-környezet</span><span class="sxs-lookup"><span data-stu-id="37906-121">Create an App Service Plan in an App Service Environment</span></span>
<span data-ttu-id="37906-122">toocreate egy app service-csomag egy app service environment-környezetben, használja az azonos hello parancs **New-AzureRmAppServicePlan** parancsot kiegészítő paraméterekkel toospecify hello ASE tartozó név és ASE tartozó erőforráscsoport-név.</span><span class="sxs-lookup"><span data-stu-id="37906-122">toocreate an app service plan in an app service environment, use hello same command **New-AzureRmAppServicePlan** command with extra parameters toospecify hello ASE's name and ASE's resource group name.</span></span>

<span data-ttu-id="37906-123">Példa toouse ezt a parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="37906-123">Example toouse this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

<span data-ttu-id="37906-124">További információk az app service Environment-környezet, ellenőrzés toolearn [bemutatása tooApp Service-környezet](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="37906-124">toolearn more about app service environment, check [Introduction tooApp Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="37906-125">Lista meglévő App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="37906-125">List Existing App Service Plans</span></span>
<span data-ttu-id="37906-126">toolist hello meglévő app service-csomagokról, használjon **Get-AzureRmAppServicePlan** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="37906-126">toolist hello existing app service plans, use **Get-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="37906-127">toolist az előfizetéshez tartozó összes app service csomagokban használja:</span><span class="sxs-lookup"><span data-stu-id="37906-127">toolist all app service plans under your subscription, use:</span></span> 

    Get-AzureRmAppServicePlan

<span data-ttu-id="37906-128">toolist egy adott erőforráscsoportba tartozó összes app service csomagokban használja:</span><span class="sxs-lookup"><span data-stu-id="37906-128">toolist all app service plans under a specific resource group, use:</span></span>

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="37906-129">egy adott app service-csomag tooget használja:</span><span class="sxs-lookup"><span data-stu-id="37906-129">tooget a specific app service plan, use:</span></span>

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="37906-130">Egy meglévő App Service-csomag konfigurálása</span><span class="sxs-lookup"><span data-stu-id="37906-130">Configure an existing App Service Plan</span></span>
<span data-ttu-id="37906-131">egy meglévő app service-csomag toochange hello beállításait használja hello **Set-AzureRmAppServicePlan** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="37906-131">toochange hello settings for an existing app service plan, use hello **Set-AzureRmAppServicePlan** cmdlet.</span></span> <span data-ttu-id="37906-132">Módosíthatja a hello réteg, a munkavégző méretének és a dolgozók hello száma</span><span class="sxs-lookup"><span data-stu-id="37906-132">You can change hello tier, worker size, and hello number of workers</span></span> 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="37906-133">Az App Service-csomag skálázás</span><span class="sxs-lookup"><span data-stu-id="37906-133">Scaling an App Service Plan</span></span>
<span data-ttu-id="37906-134">egy meglévő App Service-csomag, tooscale használja:</span><span class="sxs-lookup"><span data-stu-id="37906-134">tooscale an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-hello-worker-size-of-an-app-service-plan"></a><span data-ttu-id="37906-135">Az App Service-csomag hello munkavégző méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="37906-135">Changing hello worker size of an App Service Plan</span></span>
<span data-ttu-id="37906-136">egy meglévő App Service-csomag, használja a munkavállalók toochange hello mérete:</span><span class="sxs-lookup"><span data-stu-id="37906-136">toochange hello size of workers in an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-hello-tier-of-an-app-service-plan"></a><span data-ttu-id="37906-137">Hello egy App Service-csomag szintjének módosítása</span><span class="sxs-lookup"><span data-stu-id="37906-137">Changing hello Tier of an App Service Plan</span></span>
<span data-ttu-id="37906-138">toochange hello szintjének egy meglévő App Service-csomag, használja:</span><span class="sxs-lookup"><span data-stu-id="37906-138">toochange hello tier of an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="37906-139">Egy meglévő App Service-csomag törlése</span><span class="sxs-lookup"><span data-stu-id="37906-139">Delete an existing App Service Plan</span></span>
<span data-ttu-id="37906-140">egy meglévő app service-csomag toodelete, az összes hozzárendelt web apps kell toobe törölte vagy helyezte át először.</span><span class="sxs-lookup"><span data-stu-id="37906-140">toodelete an existing app service plan, all assigned web apps need toobe moved or deleted first.</span></span> <span data-ttu-id="37906-141">Hello használata **Remove-AzureRmAppServicePlan** parancsmag hello app service-csomag törlése.</span><span class="sxs-lookup"><span data-stu-id="37906-141">Then using hello **Remove-AzureRmAppServicePlan** cmdlet you can delete hello app service plan.</span></span>

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a><span data-ttu-id="37906-142">Az App Service Web Apps alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="37906-142">Managing App Service Web Apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="37906-143">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="37906-143">Create a Web App</span></span>
<span data-ttu-id="37906-144">toocreate egy webalkalmazást, használja a hello **New-AzureRmWebApp** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="37906-144">toocreate a web app, use hello **New-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="37906-145">Alább olvasható hello más paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="37906-145">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="37906-146">**Név**: hello webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="37906-146">**Name**: name for hello web app.</span></span>
* <span data-ttu-id="37906-147">**AppServicePlan**: hello service-csomag használt toohost hello webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="37906-147">**AppServicePlan**: name for hello service plan used toohost hello web app.</span></span>
* <span data-ttu-id="37906-148">**ResourceGroupName**: erőforráscsoport, amelyen hello App service-csomag.</span><span class="sxs-lookup"><span data-stu-id="37906-148">**ResourceGroupName**: resource group that hosts hello App service plan.</span></span>
* <span data-ttu-id="37906-149">**Hely**: hello webes alkalmazás helyét.</span><span class="sxs-lookup"><span data-stu-id="37906-149">**Location**: hello web app location.</span></span>

<span data-ttu-id="37906-150">Példa toouse ezt a parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="37906-150">Example toouse this cmdlet:</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a><span data-ttu-id="37906-151">A webalkalmazás létrehozása az App Service-környezet</span><span class="sxs-lookup"><span data-stu-id="37906-151">Create a Web App in an App Service Environment</span></span>
<span data-ttu-id="37906-152">toocreate egy webalkalmazást az App Service környezetben (ASE).</span><span class="sxs-lookup"><span data-stu-id="37906-152">toocreate a web app in an App Service Environment (ASE).</span></span> <span data-ttu-id="37906-153">Használja az azonos hello **New-AzureRmWebApp** kiegészítő paraméterekkel toospecify hello ASE nevét és hello az erőforráscsoport neve, amely hello ASE tartozik parancsot.</span><span class="sxs-lookup"><span data-stu-id="37906-153">Use hello same **New-AzureRmWebApp** command with extra parameters toospecify hello ASE name and hello resource group name that hello ASE belongs to.</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

<span data-ttu-id="37906-154">További információk az app service Environment-környezet, ellenőrzés toolearn [bemutatása tooApp Service-környezet](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="37906-154">toolearn more about app service environment, check [Introduction tooApp Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="delete-an-existing-web-app"></a><span data-ttu-id="37906-155">Egy már meglévő webalkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="37906-155">Delete an existing Web App</span></span>
<span data-ttu-id="37906-156">egy létező webalkalmazása, használhatja a hello toodelete **Remove-AzureRmWebApp** parancsmag, toospecify hello neve hello web app és hello az erőforráscsoport neve van szükség.</span><span class="sxs-lookup"><span data-stu-id="37906-156">toodelete an existing web app you can use hello **Remove-AzureRmWebApp** cmdlet, you need toospecify hello name of hello web app and hello resource group name.</span></span>

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a><span data-ttu-id="37906-157">Meglévő Web Apps felsorolása</span><span class="sxs-lookup"><span data-stu-id="37906-157">List existing Web Apps</span></span>
<span data-ttu-id="37906-158">toolist hello meglévő, használó webalkalmazások hello **Get-AzureRmWebApp** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="37906-158">toolist hello existing web apps, use hello **Get-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="37906-159">toolist az előfizetéshez tartozó összes webalkalmazások használja:</span><span class="sxs-lookup"><span data-stu-id="37906-159">toolist all web apps under your subscription, use:</span></span>

    Get-AzureRmWebApp

<span data-ttu-id="37906-160">toolist egy adott erőforráscsoportba tartozó összes webalkalmazások használja:</span><span class="sxs-lookup"><span data-stu-id="37906-160">toolist all web apps under a specific resource group, use:</span></span>

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="37906-161">egy adott webalkalmazás tooget használja:</span><span class="sxs-lookup"><span data-stu-id="37906-161">tooget a specific web app, use:</span></span>

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a><span data-ttu-id="37906-162">Egy már meglévő webalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="37906-162">Configure an existing Web App</span></span>
<span data-ttu-id="37906-163">toochange hello beállítás és konfiguráció egy már meglévő webalkalmazás használja hello **Set-AzureRmWebApp** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="37906-163">toochange hello settings and configurations for an existing web app, use hello **Set-AzureRmWebApp** cmdlet.</span></span> <span data-ttu-id="37906-164">A paraméterek teljes listájáért tekintse meg hello [parancsmag-hivatkozás](https://msdn.microsoft.com/library/mt652487.aspx)</span><span class="sxs-lookup"><span data-stu-id="37906-164">For a full list of parameters, check hello [Cmdlet reference link](https://msdn.microsoft.com/library/mt652487.aspx)</span></span>

<span data-ttu-id="37906-165">(1). példa: Ez a parancsmag toochange kapcsolati karakterláncok használata</span><span class="sxs-lookup"><span data-stu-id="37906-165">Example (1): use this cmdlet toochange connection strings</span></span>

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

<span data-ttu-id="37906-166">(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="37906-166">Example (2): add or change app settings</span></span>

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


<span data-ttu-id="37906-167">(3). példa: készlet hello web app toorun 64 bites üzemmódban</span><span class="sxs-lookup"><span data-stu-id="37906-167">Example (3):  set hello web app toorun in 64-bit mode</span></span>

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-hello-state-of-an-existing-web-app"></a><span data-ttu-id="37906-168">Egy már meglévő webalkalmazás hello állapotának módosítása</span><span class="sxs-lookup"><span data-stu-id="37906-168">Change hello state of an existing Web App</span></span>
#### <a name="restart-a-web-app"></a><span data-ttu-id="37906-169">A webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="37906-169">Restart a web app</span></span>
<span data-ttu-id="37906-170">a webes alkalmazás toorestart, meg kell adnia hello nevét és az erőforrás csoport hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="37906-170">toorestart a web app, you must specify hello name and resource group of hello web app.</span></span>

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a><span data-ttu-id="37906-171">A webalkalmazás leállítása</span><span class="sxs-lookup"><span data-stu-id="37906-171">Stop a web app</span></span>
<span data-ttu-id="37906-172">a webes alkalmazás toostop, meg kell adnia hello nevét és az erőforrás csoport hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="37906-172">toostop a web app, you must specify hello name and resource group of hello web app.</span></span>

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a><span data-ttu-id="37906-173">A webalkalmazás elindítása</span><span class="sxs-lookup"><span data-stu-id="37906-173">Start a web app</span></span>
<span data-ttu-id="37906-174">a webes alkalmazás toostart, meg kell adnia hello nevét és az erőforrás csoport hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="37906-174">toostart a web app, you must specify hello name and resource group of hello web app.</span></span>

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a><span data-ttu-id="37906-175">Webes alkalmazás közzétételi profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="37906-175">Manage Web App Publishing profiles</span></span>
<span data-ttu-id="37906-176">Minden webalkalmazás a közzétételi profilt, amely lehet használt toopublish az alkalmazások, a több művelet profilok közzététele hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="37906-176">Each web app has a publishing profile that can be used toopublish your apps, several operations can be executed on publishing profiles.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="37906-177">Kérhető le a közzétételi profil</span><span class="sxs-lookup"><span data-stu-id="37906-177">Get Publishing Profile</span></span>
<span data-ttu-id="37906-178">Közzétételi profil egy webalkalmazás, használja a tooget hello:</span><span class="sxs-lookup"><span data-stu-id="37906-178">tooget hello publishing profile for a web app, use:</span></span>

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

<span data-ttu-id="37906-179">Ez a parancs echók hello közzétételi profil toohello parancssor, valamint a kimeneti hello közzétételi profil tooa szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="37906-179">This command echoes hello publishing profile toohello command line as well output hello publishing profile tooa text file.</span></span>

#### <a name="reset-publishing-profile"></a><span data-ttu-id="37906-180">Közzétételi profil alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="37906-180">Reset Publishing Profile</span></span>
<span data-ttu-id="37906-181">tooreset mindkét hello közzétételi jelszót az FTP és a webes központi telepítése a webes alkalmazások esetén használja:</span><span class="sxs-lookup"><span data-stu-id="37906-181">tooreset both hello publishing password for FTP and web deploy for a web app, use:</span></span>

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a><span data-ttu-id="37906-182">Webes alkalmazás tanúsítványok kezelése</span><span class="sxs-lookup"><span data-stu-id="37906-182">Manage Web App Certificates</span></span>
<span data-ttu-id="37906-183">Hogyan toomanage web app tanúsítványok kapcsolatos toolearn lásd: [SSL-tanúsítványok kötése PowerShell használatával](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="37906-183">toolearn about how toomanage web app certificates, see [SSL Certificates binding using PowerShell](app-service-web-app-powershell-ssl-binding.md)</span></span>

### <a name="next-steps"></a><span data-ttu-id="37906-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37906-184">Next Steps</span></span>
* <span data-ttu-id="37906-185">Azure Resource Manager PowerShell-támogatással kapcsolatos toolearn lásd [Azure PowerShell használata az Azure Resource Manager eszközzel.](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="37906-185">toolearn about Azure Resource Manager PowerShell support, see [Using Azure PowerShell with Azure Resource Manager.](../powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="37906-186">App Service-környezetek kapcsolatos toolearn lásd [bemutatása tooApp Service-környezet.](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="37906-186">toolearn about App Service Environments, see [Introduction tooApp Service Environment.](app-service-app-service-environment-intro.md)</span></span>
* <span data-ttu-id="37906-187">App Service SSL-tanúsítványok PowerShell, történő kezelésével kapcsolatos toolearn lásd [SSL-tanúsítványok kötése PowerShell használatával.](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="37906-187">toolearn about managing App Service SSL certificates using PowerShell, see [SSL Certificates binding using PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span></span>
* <span data-ttu-id="37906-188">toolearn kapcsolatos hello Azure Web Apps, Azure Resource Manager-alapú PowerShell-parancsmagok teljes listáját lásd: [parancsmag-referencia Azure Web Apps Azure Resource Manager PowerShell-parancsmagokat.](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="37906-188">toolearn about hello full list of Azure Resource Manager-based PowerShell cmdlets for Azure Web Apps, see [Azure Cmdlet Reference of Web Apps Azure Resource Manager PowerShell Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>
* * <span data-ttu-id="37906-189">App Service CLI-vel, kezelésével kapcsolatos toolearn lásd: [Using Azure Resource Manager-Based XPlat parancssori Felületet Azure webalkalmazás számára.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span><span class="sxs-lookup"><span data-stu-id="37906-189">toolearn about managing App Service using CLI, see [Using Azure Resource Manager-Based XPlat CLI for Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span></span>

