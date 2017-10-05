---
title: "Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti az Azure Web alkalmazásokat az új Azure Resource Manager-alapú PowerShell-parancsok segítségével."
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
ms.openlocfilehash: 8d574f051a327ba0409e6f25a5886af673d3d5e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a><span data-ttu-id="c1ce0-103">Az Azure Web Apps alkalmazások kezelése az Azure Resource Manager-alapú PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="c1ce0-103">Using Azure Resource Manager-Based PowerShell to Manage Azure Web Apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c1ce0-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c1ce0-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="c1ce0-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1ce0-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

<span data-ttu-id="c1ce0-106">A Microsoft Azure PowerShell 1.0.0 verzió új parancsok váltak elérhetővé, hogy a felhasználónak, Azure Resource Manager-alapú PowerShell-parancsokat használhatja a webes alkalmazásokat kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-106">With Microsoft Azure PowerShell version 1.0.0 new commands have been added, that give the user the ability to use Azure Resource Manager-based PowerShell commands to manage Web Apps.</span></span>

<span data-ttu-id="c1ce0-107">Erőforráscsoportok kezelésével kapcsolatos információkért lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c1ce0-107">To learn about managing Resource Groups, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span> 

<span data-ttu-id="c1ce0-108">Paraméterek és beállítások megadása a PowerShell-parancsmagok teljes listája, lásd: a [parancsmag-referencia a webes alkalmazás Azure Resource Manager-alapú PowerShell-parancsmagok teljes](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-108">To learn about the full list of parameters and options for the PowerShell cmdlets, see the [full Cmdlet Reference of Web App Azure Resource Manager-based PowerShell Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>

## <a name="managing-app-service-plans"></a><span data-ttu-id="c1ce0-109">Kezelés az App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="c1ce0-109">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="c1ce0-110">Az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-110">Create an App Service Plan</span></span>
<span data-ttu-id="c1ce0-111">Az app service-csomag létrehozásához használja a **New-AzureRmAppServicePlan** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-111">To create an app service plan, use the **New-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="c1ce0-112">A különböző paraméterek leírását a következők:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-112">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="c1ce0-113">**Név**: az app service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-113">**Name**: name of the app service plan.</span></span>
* <span data-ttu-id="c1ce0-114">**Hely**: csomag hely szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-114">**Location**: service plan location.</span></span>
* <span data-ttu-id="c1ce0-115">**ResourceGroupName**: erőforráscsoport, amely tartalmazza az újonnan létrehozott app service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-115">**ResourceGroupName**: resource group that includes the newly created app service plan.</span></span>
* <span data-ttu-id="c1ce0-116">**Réteg**: a kívánt tarifacsomagot (alapértelmezett ingyenes, a közös, Basic, Standard és Premium is.)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-116">**Tier**:  the desired pricing tier (Default is Free, other options are Shared, Basic, Standard, and Premium.)</span></span>
* <span data-ttu-id="c1ce0-117">**WorkerSize**: (alapértelmezett érték kisebb, ha a réteg paraméter van megadva, a Basic, Standard vagy Premium munkavállalók mérete.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-117">**WorkerSize**: the size of workers (Default is small if the Tier parameter was specified as Basic, Standard, or Premium.</span></span> <span data-ttu-id="c1ce0-118">Is, közepes és nagy.)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-118">Other options are Medium, and Large.)</span></span>
* <span data-ttu-id="c1ce0-119">**NumberofWorkers**: szolgáltatási terv (alapértelmezett értéke 1) az alkalmazásban munkavállalók száma.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-119">**NumberofWorkers**: the number of workers in the app service plan (Default value is 1).</span></span> 

<span data-ttu-id="c1ce0-120">Például, hogy használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-120">Example to use this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a><span data-ttu-id="c1ce0-121">Az App Service-csomag létrehozása az App Service-környezet</span><span class="sxs-lookup"><span data-stu-id="c1ce0-121">Create an App Service Plan in an App Service Environment</span></span>
<span data-ttu-id="c1ce0-122">Az app service environment-környezetben az app service-csomag létrehozásához használja a ugyanazzal a paranccsal **New-AzureRmAppServicePlan** parancsot a további paramétereket adja meg a ASE nevét és ASE tartozó erőforráscsoport-név.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-122">To create an app service plan in an app service environment, use the same command **New-AzureRmAppServicePlan** command with extra parameters to specify the ASE's name and ASE's resource group name.</span></span>

<span data-ttu-id="c1ce0-123">Például, hogy használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-123">Example to use this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

<span data-ttu-id="c1ce0-124">App service Environment-környezet kapcsolatos további információkért ellenőrizze a következőket [App Service Environment bemutatása](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-124">To learn more about app service environment, check [Introduction to App Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="c1ce0-125">Lista meglévő App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="c1ce0-125">List Existing App Service Plans</span></span>
<span data-ttu-id="c1ce0-126">A meglévő app service-csomagokról kilistázhatja **Get-AzureRmAppServicePlan** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-126">To list the existing app service plans, use **Get-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="c1ce0-127">Az előfizetéshez tartozó összes app service csomagokban listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-127">To list all app service plans under your subscription, use:</span></span> 

    Get-AzureRmAppServicePlan

<span data-ttu-id="c1ce0-128">Egy adott erőforráscsoportba tartozó összes app service csomagokban listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-128">To list all app service plans under a specific resource group, use:</span></span>

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="c1ce0-129">Egy adott app service-csomag beszerzéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-129">To get a specific app service plan, use:</span></span>

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="c1ce0-130">Egy meglévő App Service-csomag konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-130">Configure an existing App Service Plan</span></span>
<span data-ttu-id="c1ce0-131">Egy meglévő app service-csomag beállításainak módosításához használja a **Set-AzureRmAppServicePlan** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-131">To change the settings for an existing app service plan, use the **Set-AzureRmAppServicePlan** cmdlet.</span></span> <span data-ttu-id="c1ce0-132">Módosíthatja a réteg, a munkavégző mérete és a dolgozók száma</span><span class="sxs-lookup"><span data-stu-id="c1ce0-132">You can change the tier, worker size, and the number of workers</span></span> 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="c1ce0-133">Az App Service-csomag skálázás</span><span class="sxs-lookup"><span data-stu-id="c1ce0-133">Scaling an App Service Plan</span></span>
<span data-ttu-id="c1ce0-134">Egy meglévő App Service-csomag méretezéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-134">To scale an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a><span data-ttu-id="c1ce0-135">Az App Service-csomag munkavégző méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-135">Changing the worker size of an App Service Plan</span></span>
<span data-ttu-id="c1ce0-136">Egy meglévő App Service-csomag munkavállalók méretének módosításához használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-136">To change the size of workers in an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a><span data-ttu-id="c1ce0-137">Az App Service-csomag szintjének módosítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-137">Changing the Tier of an App Service Plan</span></span>
<span data-ttu-id="c1ce0-138">Egy meglévő App Service-csomag szintjének módosításához használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-138">To change the tier of an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="c1ce0-139">Egy meglévő App Service-csomag törlése</span><span class="sxs-lookup"><span data-stu-id="c1ce0-139">Delete an existing App Service Plan</span></span>
<span data-ttu-id="c1ce0-140">Egy meglévő app service-csomag törlése, hozzárendelt webalkalmazások áthelyezését vagy először törölni kell.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-140">To delete an existing app service plan, all assigned web apps need to be moved or deleted first.</span></span> <span data-ttu-id="c1ce0-141">Ezután használja a **Remove-AzureRmAppServicePlan** parancsmag az app service-csomag törlése.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-141">Then using the **Remove-AzureRmAppServicePlan** cmdlet you can delete the app service plan.</span></span>

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a><span data-ttu-id="c1ce0-142">Az App Service Web Apps alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="c1ce0-142">Managing App Service Web Apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="c1ce0-143">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-143">Create a Web App</span></span>
<span data-ttu-id="c1ce0-144">A webes alkalmazás létrehozásához használja a **New-AzureRmWebApp** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-144">To create a web app, use the **New-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="c1ce0-145">A különböző paraméterek leírását a következők:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-145">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="c1ce0-146">**Név**: a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-146">**Name**: name for the web app.</span></span>
* <span data-ttu-id="c1ce0-147">**AppServicePlan**: a webalkalmazás üzemeltetéséhez használt service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-147">**AppServicePlan**: name for the service plan used to host the web app.</span></span>
* <span data-ttu-id="c1ce0-148">**ResourceGroupName**: erőforráscsoport, amelyen az App service-csomag.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-148">**ResourceGroupName**: resource group that hosts the App service plan.</span></span>
* <span data-ttu-id="c1ce0-149">**Hely**: a webes alkalmazás helyét.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-149">**Location**: the web app location.</span></span>

<span data-ttu-id="c1ce0-150">Például, hogy használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-150">Example to use this cmdlet:</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a><span data-ttu-id="c1ce0-151">A webalkalmazás létrehozása az App Service-környezet</span><span class="sxs-lookup"><span data-stu-id="c1ce0-151">Create a Web App in an App Service Environment</span></span>
<span data-ttu-id="c1ce0-152">A webalkalmazás létrehozása az App Service környezetben (ASE).</span><span class="sxs-lookup"><span data-stu-id="c1ce0-152">To create a web app in an App Service Environment (ASE).</span></span> <span data-ttu-id="c1ce0-153">Ugyanazon **New-AzureRmWebApp** kiegészítő paraméterekkel rendelkező parancsot adja meg a ASE nevét és az erőforráscsoport neve, amely a ASE tartozik.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-153">Use the same **New-AzureRmWebApp** command with extra parameters to specify the ASE name and the resource group name that the ASE belongs to.</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

<span data-ttu-id="c1ce0-154">App service Environment-környezet kapcsolatos további információkért ellenőrizze a következőket [App Service Environment bemutatása](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-154">To learn more about app service environment, check [Introduction to App Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="delete-an-existing-web-app"></a><span data-ttu-id="c1ce0-155">Egy már meglévő webalkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="c1ce0-155">Delete an existing Web App</span></span>
<span data-ttu-id="c1ce0-156">Használhat meglévő webes alkalmazás törlése a **Remove-AzureRmWebApp** parancsmag, meg kell adni a webalkalmazás nevét és az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-156">To delete an existing web app you can use the **Remove-AzureRmWebApp** cmdlet, you need to specify the name of the web app and the resource group name.</span></span>

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a><span data-ttu-id="c1ce0-157">Meglévő Web Apps felsorolása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-157">List existing Web Apps</span></span>
<span data-ttu-id="c1ce0-158">A meglévő web Apps alkalmazások listájában, használja a **Get-AzureRmWebApp** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-158">To list the existing web apps, use the **Get-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="c1ce0-159">Az előfizetéshez tartozó összes webes alkalmazások listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-159">To list all web apps under your subscription, use:</span></span>

    Get-AzureRmWebApp

<span data-ttu-id="c1ce0-160">Egy adott erőforráscsoportba tartozó összes webes alkalmazások listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-160">To list all web apps under a specific resource group, use:</span></span>

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="c1ce0-161">Egy adott webalkalmazás, amelyet:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-161">To get a specific web app, use:</span></span>

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a><span data-ttu-id="c1ce0-162">Egy már meglévő webalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-162">Configure an existing Web App</span></span>
<span data-ttu-id="c1ce0-163">A beállítások és egy már meglévő webalkalmazás konfiguráció módosításához használja a **Set-AzureRmWebApp** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-163">To change the settings and configurations for an existing web app, use the **Set-AzureRmWebApp** cmdlet.</span></span> <span data-ttu-id="c1ce0-164">A paraméterek teljes listájáért tekintse meg a [parancsmag-hivatkozás](https://msdn.microsoft.com/library/mt652487.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-164">For a full list of parameters, check the [Cmdlet reference link](https://msdn.microsoft.com/library/mt652487.aspx)</span></span>

<span data-ttu-id="c1ce0-165">(1). példa: Ez a parancsmag segítségével módosíthatja a kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="c1ce0-165">Example (1): use this cmdlet to change connection strings</span></span>

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

<span data-ttu-id="c1ce0-166">(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-166">Example (2): add or change app settings</span></span>

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


<span data-ttu-id="c1ce0-167">(3). példa: a webes alkalmazás 64 bites módban való futásra beállítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-167">Example (3):  set the web app to run in 64-bit mode</span></span>

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a><span data-ttu-id="c1ce0-168">Egy már meglévő webalkalmazás állapotának módosítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-168">Change the state of an existing Web App</span></span>
#### <a name="restart-a-web-app"></a><span data-ttu-id="c1ce0-169">A webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-169">Restart a web app</span></span>
<span data-ttu-id="c1ce0-170">A webalkalmazás újraindítása, meg kell adnia a webalkalmazás nevét és az erőforrás csoportja.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-170">To restart a web app, you must specify the name and resource group of the web app.</span></span>

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a><span data-ttu-id="c1ce0-171">A webalkalmazás leállítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-171">Stop a web app</span></span>
<span data-ttu-id="c1ce0-172">A webalkalmazás leállítása, meg kell adnia a webalkalmazás nevét és az erőforrás csoportja.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-172">To stop a web app, you must specify the name and resource group of the web app.</span></span>

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a><span data-ttu-id="c1ce0-173">A webalkalmazás elindítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-173">Start a web app</span></span>
<span data-ttu-id="c1ce0-174">A webalkalmazás elindítása, meg kell adnia a webalkalmazás nevét és az erőforrás csoportja.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-174">To start a web app, you must specify the name and resource group of the web app.</span></span>

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a><span data-ttu-id="c1ce0-175">Webes alkalmazás közzétételi profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="c1ce0-175">Manage Web App Publishing profiles</span></span>
<span data-ttu-id="c1ce0-176">Minden webalkalmazás a közzétételi profilt, amely segítségével az alkalmazások közzététele, akkor több művelet profilok közzététele hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-176">Each web app has a publishing profile that can be used to publish your apps, several operations can be executed on publishing profiles.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="c1ce0-177">Kérhető le a közzétételi profil</span><span class="sxs-lookup"><span data-stu-id="c1ce0-177">Get Publishing Profile</span></span>
<span data-ttu-id="c1ce0-178">A közzétételi profil egy webalkalmazás, amelyet:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-178">To get the publishing profile for a web app, use:</span></span>

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

<span data-ttu-id="c1ce0-179">Ez a parancs echók a közzétételi profilhoz, amellyel a parancssorból, valamint a kimeneti a közzétételi profil egy szövegfájlba.</span><span class="sxs-lookup"><span data-stu-id="c1ce0-179">This command echoes the publishing profile to the command line as well output the publishing profile to a text file.</span></span>

#### <a name="reset-publishing-profile"></a><span data-ttu-id="c1ce0-180">Közzétételi profil alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="c1ce0-180">Reset Publishing Profile</span></span>
<span data-ttu-id="c1ce0-181">FTP és a webes közzétételi jelszavát alaphelyzetbe állítása mindkét egy webalkalmazást, használja a központi telepítése:</span><span class="sxs-lookup"><span data-stu-id="c1ce0-181">To reset both the publishing password for FTP and web deploy for a web app, use:</span></span>

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a><span data-ttu-id="c1ce0-182">Webes alkalmazás tanúsítványok kezelése</span><span class="sxs-lookup"><span data-stu-id="c1ce0-182">Manage Web App Certificates</span></span>
<span data-ttu-id="c1ce0-183">Webes alkalmazás tanúsítványok kezelése kapcsolatos további tudnivalókért lásd: [SSL-tanúsítványok kötése PowerShell használatával](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-183">To learn about how to manage web app certificates, see [SSL Certificates binding using PowerShell](app-service-web-app-powershell-ssl-binding.md)</span></span>

### <a name="next-steps"></a><span data-ttu-id="c1ce0-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1ce0-184">Next Steps</span></span>
* <span data-ttu-id="c1ce0-185">Azure Resource Manager PowerShell-támogatással kapcsolatos információkért lásd: [Azure PowerShell használata az Azure Resource Manager eszközzel.](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-185">To learn about Azure Resource Manager PowerShell support, see [Using Azure PowerShell with Azure Resource Manager.](../powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="c1ce0-186">App Service Environment-környezetek kapcsolatos további tudnivalókért lásd: [App Service Environment bemutatása.](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-186">To learn about App Service Environments, see [Introduction to App Service Environment.](app-service-app-service-environment-intro.md)</span></span>
* <span data-ttu-id="c1ce0-187">App Service SSL-tanúsítványok PowerShell használatával történő kezelésével kapcsolatos információkért lásd: [SSL-tanúsítványok kötése PowerShell használatával.](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-187">To learn about managing App Service SSL certificates using PowerShell, see [SSL Certificates binding using PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span></span>
* <span data-ttu-id="c1ce0-188">Az Azure Resource Manager-alapú PowerShell-parancsmagok teljes listája az Azure Web Apps, lásd: [parancsmag-referencia Azure Web Apps Azure Resource Manager PowerShell-parancsmagokat.](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-188">To learn about the full list of Azure Resource Manager-based PowerShell cmdlets for Azure Web Apps, see [Azure Cmdlet Reference of Web Apps Azure Resource Manager PowerShell Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>
* * <span data-ttu-id="c1ce0-189">App Service parancssori felület használatának kezelésével kapcsolatos információkért lásd: [Using Azure Resource Manager-Based XPlat parancssori Felületet Azure webalkalmazás számára.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span><span class="sxs-lookup"><span data-stu-id="c1ce0-189">To learn about managing App Service using CLI, see [Using Azure Resource Manager-Based XPlat CLI for Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span></span>

