---
title: "Nagy sűrűségű üzemeltetésének Azure App Service szolgáltatásban |} Microsoft Docs"
description: "Nagy sűrűségű üzemeltetésének Azure App Service"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: byvinyal
ms.openlocfilehash: 459a310a719695f6366470976d857ec2f9d6f4a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="high-density-hosting-on-azure-app-service"></a><span data-ttu-id="276ef-103">Nagy sűrűségű üzemeltetésének Azure App Service</span><span class="sxs-lookup"><span data-stu-id="276ef-103">High density hosting on Azure App Service</span></span>
<span data-ttu-id="276ef-104">App Service használata esetén a rendszer leválasztja az alkalmazást a két fogalom által lefoglalt kapacitás:</span><span class="sxs-lookup"><span data-stu-id="276ef-104">When using App Service, your application is decoupled from the capacity allocated to it by two concepts:</span></span>

* <span data-ttu-id="276ef-105">**Az alkalmazás:** jelenti. az alkalmazás és a futtatókörnyezet konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="276ef-105">**The Application:** Represents the app and its runtime configuration.</span></span> <span data-ttu-id="276ef-106">Például a .NET verzióját, amelyet a futtatókörnyezet kell, az alkalmazás beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="276ef-106">For example, it includes the version of .NET that the runtime should load, the app settings.</span></span>
* <span data-ttu-id="276ef-107">**Az App Service-csomag:** határozza meg a kapacitás, a rendelkezésre álló készlet és a Helység mezőben az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="276ef-107">**The App Service Plan:** Defines the characteristics of the capacity, available feature set, and locality of the application.</span></span> <span data-ttu-id="276ef-108">Nagy (négy magok) számítógép, a négy példányt, a prémium szolgáltatások, az USA keleti régiója, előfordulhat, hogy lehet jellemzőit.</span><span class="sxs-lookup"><span data-stu-id="276ef-108">For example, characteristics might be large (four cores) machine, four instances, Premium features in East US.</span></span>

<span data-ttu-id="276ef-109">Az alkalmazások mindig kapcsolódik az App Service-csomag, de az App Service-csomag egy vagy több alkalmazás kapacitást biztosít.</span><span class="sxs-lookup"><span data-stu-id="276ef-109">An app is always linked to an App Service plan, but an App Service plan can provide capacity to one or more apps.</span></span>

<span data-ttu-id="276ef-110">Ennek eredményeképpen a platform rugalmasságot biztosít, és egyetlen meghatározott alkalmazás elkülönítése, vagy ossza meg az App Service-csomag erőforrások megosztása több alkalmazásokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="276ef-110">As a result, the platform provides the flexibility to isolate a single app or have multiple apps share resources by sharing an App Service plan.</span></span>

<span data-ttu-id="276ef-111">Azonban amikor több alkalmazás az App Service-csomag megosztásához az alkalmazás egy példánya fut, hogy az alkalmazásszolgáltatási csomag összes példányán.</span><span class="sxs-lookup"><span data-stu-id="276ef-111">However, when multiple apps share an App Service plan, an instance of that app runs on every instance of that App Service plan.</span></span>

## <a name="per-app-scaling"></a><span data-ttu-id="276ef-112">Egy alkalmazás skálázás</span><span class="sxs-lookup"><span data-stu-id="276ef-112">Per app scaling</span></span>
<span data-ttu-id="276ef-113">*Egy alkalmazás skálázás* egy szolgáltatás, amely az App Service-csomag szintjén engedélyezhető, és alkalmazásonként használja.</span><span class="sxs-lookup"><span data-stu-id="276ef-113">*Per app scaling* is a feature that can be enabled at the App Service plan level and then used per application.</span></span>

<span data-ttu-id="276ef-114">Alkalmazásonkénti skálázás arányosan egymástól függetlenül az App Service-csomag, amelyen az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="276ef-114">Per app scaling scales an app independently from the App Service plan that hosts it.</span></span> <span data-ttu-id="276ef-115">Így az App Service-csomag is méretezhető 10 példányok, de az alkalmazás csak öt állítható be.</span><span class="sxs-lookup"><span data-stu-id="276ef-115">This way, an App Service plan can be scaled to 10 instances, but an app can be set to use only five.</span></span>

   >[!NOTE]
   ><span data-ttu-id="276ef-116">Alkalmazásonkénti skálázás csak érhető **prémium** SKU App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="276ef-116">Per app scaling is available only for **Premium** SKU App Service plans</span></span>
   >

### <a name="per-app-scaling-using-powershell"></a><span data-ttu-id="276ef-117">Alkalmazásonkénti skálázás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="276ef-117">Per app scaling using PowerShell</span></span>

<span data-ttu-id="276ef-118">Egy konfigurált tervet is létrehozhat egy */ alkalmazás skálázás* történő a terv a ```-perSiteScaling $true``` attribútumot a ```New-AzureRmAppServicePlan``` parancsmag</span><span class="sxs-lookup"><span data-stu-id="276ef-118">You can create a plan configured as a *Per app scaling* plan by passing in the ```-perSiteScaling $true``` attribute to the ```New-AzureRmAppServicePlan``` commandlet</span></span>

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

<span data-ttu-id="276ef-119">Ha azt szeretné, a szolgáltatás használatához egy meglévő App Service-csomag frissítése:</span><span class="sxs-lookup"><span data-stu-id="276ef-119">If you want to update an existing App Service plan to use this feature:</span></span> 

- <span data-ttu-id="276ef-120">a cél terv beolvasása```Get-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="276ef-120">get the target plan ```Get-AzureRmAppServicePlan```</span></span>
- <span data-ttu-id="276ef-121">helyileg a tulajdonság módosítása```$newASP.PerSiteScaling = $true```</span><span class="sxs-lookup"><span data-stu-id="276ef-121">modifying the property locally ```$newASP.PerSiteScaling = $true```</span></span>
- <span data-ttu-id="276ef-122">a módosításokat vissza az Azure-bA küldése```Set-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="276ef-122">posting your changes back to azure ```Set-AzureRmAppServicePlan```</span></span> 

```
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

<span data-ttu-id="276ef-123">Az alkalmazás szintjén igazolnia kell a példányok is használhatja az alkalmazást az app service-csomag konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="276ef-123">At the app level, we need to configure the number of instances the app can use in the app service plan.</span></span>

<span data-ttu-id="276ef-124">Az alábbi példában az alkalmazás korlátozódik két példányt, függetlenül attól, hány példányban kimenő alkalmazkodnak az alapul szolgáló app service-csomag.</span><span class="sxs-lookup"><span data-stu-id="276ef-124">In the example below, the app is limited to two instances regardless of how many instances the underlying app service plan scales out to.</span></span>

```
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> <span data-ttu-id="276ef-125">$newapp. SiteConfig.NumberOfWorkers másik formája $newapp, amely. MaxNumberOfWorkers.</span><span class="sxs-lookup"><span data-stu-id="276ef-125">$newapp.SiteConfig.NumberOfWorkers is different form $newapp.MaxNumberOfWorkers.</span></span> <span data-ttu-id="276ef-126">Alkalmazásonkénti $newapp skálázás használja. A skála jellemzőit az alkalmazás SiteConfig.NumberOfWorkers.</span><span class="sxs-lookup"><span data-stu-id="276ef-126">Per app scaling uses $newapp.SiteConfig.NumberOfWorkers to determine the scale characteristics of the app.</span></span>

### <a name="per-app-scaling-using-azure-resource-manager"></a><span data-ttu-id="276ef-127">Egy alkalmazás skálázás Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="276ef-127">Per app scaling using Azure Resource Manager</span></span>

<span data-ttu-id="276ef-128">A következő *Azure Resource Manager sablon* hoz létre:</span><span class="sxs-lookup"><span data-stu-id="276ef-128">The following *Azure Resource Manager template* creates:</span></span>

- <span data-ttu-id="276ef-129">App Service-csomagot, amely 10 példányok horizontálisan</span><span class="sxs-lookup"><span data-stu-id="276ef-129">An App Service plan that's scaled out to 10 instances</span></span>
- <span data-ttu-id="276ef-130">egy alkalmazás, amely konfigurálva van egy legfeljebb öt példány számára.</span><span class="sxs-lookup"><span data-stu-id="276ef-130">an app that's configured to scale to a max of five instances.</span></span>

<span data-ttu-id="276ef-131">Az App Service-csomag állítja a **PerSiteScaling** tulajdonság igaz értékre ```"perSiteScaling": true```.</span><span class="sxs-lookup"><span data-stu-id="276ef-131">The App Service plan is setting the **PerSiteScaling** property to true ```"perSiteScaling": true```.</span></span> <span data-ttu-id="276ef-132">Az alkalmazás beállítja a **munkavállalók száma** 5 használandó ```"properties": { "numberOfWorkers": "5" }```.</span><span class="sxs-lookup"><span data-stu-id="276ef-132">The app is setting the **number of workers** to use to 5 ```"properties": { "numberOfWorkers": "5" }```.</span></span>

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a><span data-ttu-id="276ef-133">Ajánlott konfiguráció nagy sűrűségű üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="276ef-133">Recommended configuration for high density hosting</span></span>
<span data-ttu-id="276ef-134">Egy alkalmazás skálázás van olyan szolgáltatás, amely globális Azure-régiók és az App Service Environment-környezetek is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="276ef-134">Per app scaling is a feature that is enabled in both global Azure regions and App Service Environments.</span></span> <span data-ttu-id="276ef-135">Azonban ajánlott, hogy kihasználják a speciális funkciók és kapacitás nagyobb készletek App Service Environment-környezetek felhasználva.</span><span class="sxs-lookup"><span data-stu-id="276ef-135">However, the recommended strategy is to use App Service Environments to take advantage of their advanced features and the larger pools of capacity.</span></span>  

<span data-ttu-id="276ef-136">Ezek a lépések segítségével állítsa be az alkalmazások közötti üzemeltetési nagy sűrűségű:</span><span class="sxs-lookup"><span data-stu-id="276ef-136">Follow these steps to configure high density hosting for your apps:</span></span>

1. <span data-ttu-id="276ef-137">Az App Service Environment-környezet konfigurálása, és válassza ki a feldolgozókészleten, amely a nagy sűrűségű üzemeltetés van kijelölve.</span><span class="sxs-lookup"><span data-stu-id="276ef-137">Configure the App Service Environment and choose a worker pool that is dedicated to the high density hosting scenario.</span></span>
1. <span data-ttu-id="276ef-138">Egy App Service-csomag létrehozása, és a méret úgy, hogy a munkavégző készletét használja a rendelkezésre álló kapacitásból.</span><span class="sxs-lookup"><span data-stu-id="276ef-138">Create a single App Service plan, and scale it to use all the available capacity on the worker pool.</span></span>
1. <span data-ttu-id="276ef-139">Értékre van állítva a PerSiteScaling jelző igaz az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="276ef-139">Set the PerSiteScaling flag to true on the App Service plan.</span></span>
1. <span data-ttu-id="276ef-140">Új alkalmazások létrehozása és, hogy az App Service-csomag rendelve a **numberOfWorkers** tulajdonsága **1**.</span><span class="sxs-lookup"><span data-stu-id="276ef-140">New apps are created and assigned to that App Service plan with the **numberOfWorkers** property set to **1**.</span></span> <span data-ttu-id="276ef-141">Ezzel a konfigurációval a munkavégző készlettől lehetséges legmagasabb sűrűség adja eredményül.</span><span class="sxs-lookup"><span data-stu-id="276ef-141">Using this configuration yields the highest density possible on this worker pool.</span></span>
1. <span data-ttu-id="276ef-142">Dolgozók számának igény szerint további erőforrások megadását alkalmazásonkénti egymástól függetlenül konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="276ef-142">The number of workers can be configured independently per app to grant additional resources as needed.</span></span> <span data-ttu-id="276ef-143">Példa:</span><span class="sxs-lookup"><span data-stu-id="276ef-143">For example:</span></span>
    - <span data-ttu-id="276ef-144">A nagy igénybevételnek kitett alkalmazás állíthatja be **numberOfWorkers** való **3** kell rendelkeznie az adott alkalmazáshoz több feldolgozási kapacitás.</span><span class="sxs-lookup"><span data-stu-id="276ef-144">A high-use app can set **numberOfWorkers** to **3** to have more processing capacity for that app.</span></span> 
    - <span data-ttu-id="276ef-145">Alacsony használható alkalmazások állítania **numberOfWorkers** való **1**.</span><span class="sxs-lookup"><span data-stu-id="276ef-145">Low-use apps would set **numberOfWorkers** to **1**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="276ef-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="276ef-146">Next Steps</span></span>

- [<span data-ttu-id="276ef-147">Az Azure App Service-csomagok részletes áttekintése</span><span class="sxs-lookup"><span data-stu-id="276ef-147">Azure App Service plans in-depth overview</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [<span data-ttu-id="276ef-148">Az App Service Environment bemutatása</span><span class="sxs-lookup"><span data-stu-id="276ef-148">Introduction to App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)