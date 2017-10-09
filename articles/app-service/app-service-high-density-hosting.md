---
title: "Azure App Service üzemeltető sűrűség aaaHigh |} Microsoft Docs"
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
ms.openlocfilehash: a10cb81ace13ba6992b572a44361061ecf72b266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-density-hosting-on-azure-app-service"></a><span data-ttu-id="8c184-103">Nagy sűrűségű üzemeltetésének Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8c184-103">High density hosting on Azure App Service</span></span>
<span data-ttu-id="8c184-104">App Service használata esetén a rendszer leválasztja az alkalmazást tooit lefoglalta két fogalom hello kapacitás:</span><span class="sxs-lookup"><span data-stu-id="8c184-104">When using App Service, your application is decoupled from hello capacity allocated tooit by two concepts:</span></span>

* <span data-ttu-id="8c184-105">**Alkalmazás hello:** hello alkalmazás és a futásidejű konfigurálását jelenti.</span><span class="sxs-lookup"><span data-stu-id="8c184-105">**hello Application:** Represents hello app and its runtime configuration.</span></span> <span data-ttu-id="8c184-106">Például hello tartalmazza, amelyek futásidejű hello .NET verzióját kell betöltéséhez hello Alkalmazásbeállítások.</span><span class="sxs-lookup"><span data-stu-id="8c184-106">For example, it includes hello version of .NET that hello runtime should load, hello app settings.</span></span>
* <span data-ttu-id="8c184-107">**App Service-csomag hello:** hello kapacitás, a rendelkezésre álló készlet és a helység hello alkalmazás hello jellemzői határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8c184-107">**hello App Service Plan:** Defines hello characteristics of hello capacity, available feature set, and locality of hello application.</span></span> <span data-ttu-id="8c184-108">Nagy (négy magok) számítógép, a négy példányt, a prémium szolgáltatások, az USA keleti régiója, előfordulhat, hogy lehet jellemzőit.</span><span class="sxs-lookup"><span data-stu-id="8c184-108">For example, characteristics might be large (four cores) machine, four instances, Premium features in East US.</span></span>

<span data-ttu-id="8c184-109">Az alkalmazások mindig csatolt tooan App Service-csomag, de az App Service-csomag biztosíthat a kapacitás tooone, illetve további alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="8c184-109">An app is always linked tooan App Service plan, but an App Service plan can provide capacity tooone or more apps.</span></span>

<span data-ttu-id="8c184-110">Ennek eredményeképpen hello platform lehetővé teszi a hello rugalmasságot tooisolate egyetlen alkalmazást, vagy ossza meg az App Service-csomag erőforrások megosztása több alkalmazásokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="8c184-110">As a result, hello platform provides hello flexibility tooisolate a single app or have multiple apps share resources by sharing an App Service plan.</span></span>

<span data-ttu-id="8c184-111">Azonban amikor több alkalmazás az App Service-csomag megosztásához az alkalmazás egy példánya fut, hogy az alkalmazásszolgáltatási csomag összes példányán.</span><span class="sxs-lookup"><span data-stu-id="8c184-111">However, when multiple apps share an App Service plan, an instance of that app runs on every instance of that App Service plan.</span></span>

## <a name="per-app-scaling"></a><span data-ttu-id="8c184-112">Egy alkalmazás skálázás</span><span class="sxs-lookup"><span data-stu-id="8c184-112">Per app scaling</span></span>
<span data-ttu-id="8c184-113">*Egy alkalmazás skálázás* egy szolgáltatás, amely az App Service-csomag szintjén engedélyezhető, és alkalmazásonként használja.</span><span class="sxs-lookup"><span data-stu-id="8c184-113">*Per app scaling* is a feature that can be enabled at the App Service plan level and then used per application.</span></span>

<span data-ttu-id="8c184-114">Alkalmazásonkénti skálázás arányosan egymástól függetlenül az App Service-csomag, amelyen az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8c184-114">Per app scaling scales an app independently from the App Service plan that hosts it.</span></span> <span data-ttu-id="8c184-115">Ezzel a módszerrel egy App Service csomag lehet méretezni too10 példányok, de az alkalmazás csak öt toouse állítható be.</span><span class="sxs-lookup"><span data-stu-id="8c184-115">This way, an App Service plan can be scaled too10 instances, but an app can be set toouse only five.</span></span>

   >[!NOTE]
   ><span data-ttu-id="8c184-116">Alkalmazásonkénti skálázás csak érhető **prémium** SKU App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="8c184-116">Per app scaling is available only for **Premium** SKU App Service plans</span></span>
   >

### <a name="per-app-scaling-using-powershell"></a><span data-ttu-id="8c184-117">Alkalmazásonkénti skálázás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="8c184-117">Per app scaling using PowerShell</span></span>

<span data-ttu-id="8c184-118">Egy konfigurált tervet is létrehozhat egy */ alkalmazás skálázás* történő hello a terv ```-perSiteScaling $true``` toohello attribútum ```New-AzureRmAppServicePlan``` parancsmag</span><span class="sxs-lookup"><span data-stu-id="8c184-118">You can create a plan configured as a *Per app scaling* plan by passing in hello ```-perSiteScaling $true``` attribute toohello ```New-AzureRmAppServicePlan``` commandlet</span></span>

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

<span data-ttu-id="8c184-119">Ha azt szeretné, hogy egy meglévő App Service tooupdate megtervezése toouse ezt a szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="8c184-119">If you want tooupdate an existing App Service plan toouse this feature:</span></span> 

- <span data-ttu-id="8c184-120">hello cél terv beolvasása```Get-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="8c184-120">get hello target plan ```Get-AzureRmAppServicePlan```</span></span>
- <span data-ttu-id="8c184-121">helyileg hello tulajdonság módosítása```$newASP.PerSiteScaling = $true```</span><span class="sxs-lookup"><span data-stu-id="8c184-121">modifying hello property locally ```$newASP.PerSiteScaling = $true```</span></span>
- <span data-ttu-id="8c184-122">a módosítások vissza tooazure könyvelési```Set-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="8c184-122">posting your changes back tooazure ```Set-AzureRmAppServicePlan```</span></span> 

```
# Get hello new App Service Plan and modify hello "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify hello local copy toouse "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back tooazure
Set-AzureRmAppServicePlan $newASP
```

<span data-ttu-id="8c184-123">Hello alkalmazási szintű tooconfigure hello több példányban hello alkalmazás az app service-csomag hello is használni kell.</span><span class="sxs-lookup"><span data-stu-id="8c184-123">At hello app level, we need tooconfigure hello number of instances hello app can use in hello app service plan.</span></span>

<span data-ttu-id="8c184-124">Az alábbi példa hello hello app hello alapul szolgáló app service csomag méretezik ki a rendszer korlátozott tootwo példányok, függetlenül attól, hogy hány.</span><span class="sxs-lookup"><span data-stu-id="8c184-124">In hello example below, hello app is limited tootwo instances regardless of how many instances hello underlying app service plan scales out to.</span></span>

```
# Get hello app we want tooconfigure toouse "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify hello NumberOfWorkers setting toohello desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back tooazure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> <span data-ttu-id="8c184-125">$newapp. SiteConfig.NumberOfWorkers másik formája $newapp, amely. MaxNumberOfWorkers.</span><span class="sxs-lookup"><span data-stu-id="8c184-125">$newapp.SiteConfig.NumberOfWorkers is different form $newapp.MaxNumberOfWorkers.</span></span> <span data-ttu-id="8c184-126">Alkalmazásonkénti $newapp skálázás használja. SiteConfig.NumberOfWorkers toodetermine hello méretezési jellemzői hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8c184-126">Per app scaling uses $newapp.SiteConfig.NumberOfWorkers toodetermine hello scale characteristics of hello app.</span></span>

### <a name="per-app-scaling-using-azure-resource-manager"></a><span data-ttu-id="8c184-127">Egy alkalmazás skálázás Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="8c184-127">Per app scaling using Azure Resource Manager</span></span>

<span data-ttu-id="8c184-128">hello következő *Azure Resource Manager sablon* hoz létre:</span><span class="sxs-lookup"><span data-stu-id="8c184-128">hello following *Azure Resource Manager template* creates:</span></span>

- <span data-ttu-id="8c184-129">Kimenő too10 példányok méretezett App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="8c184-129">An App Service plan that's scaled out too10 instances</span></span>
- <span data-ttu-id="8c184-130">olyan alkalmazás, amelynek tooscale tooa legfeljebb öt példányok van beállítva.</span><span class="sxs-lookup"><span data-stu-id="8c184-130">an app that's configured tooscale tooa max of five instances.</span></span>

<span data-ttu-id="8c184-131">hello App Service-csomag állít hello **PerSiteScaling** tulajdonság tootrue ```"perSiteScaling": true```.</span><span class="sxs-lookup"><span data-stu-id="8c184-131">hello App Service plan is setting hello **PerSiteScaling** property tootrue ```"perSiteScaling": true```.</span></span> <span data-ttu-id="8c184-132">hello app állít hello **munkavállalók száma** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.</span><span class="sxs-lookup"><span data-stu-id="8c184-132">hello app is setting hello **number of workers** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.</span></span>

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

## <a name="recommended-configuration-for-high-density-hosting"></a><span data-ttu-id="8c184-133">Ajánlott konfiguráció nagy sűrűségű üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="8c184-133">Recommended configuration for high density hosting</span></span>
<span data-ttu-id="8c184-134">Egy alkalmazás skálázás van olyan szolgáltatás, amely globális Azure-régiók és az App Service Environment-környezetek is engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="8c184-134">Per app scaling is a feature that is enabled in both global Azure regions and App Service Environments.</span></span> <span data-ttu-id="8c184-135">Azonban hello ajánlott stratégia App Service Environment-környezetek tootake kihasználják a speciális funkciók és kapacitás nagyobb készleteinek hello használatához.</span><span class="sxs-lookup"><span data-stu-id="8c184-135">However, hello recommended strategy is to use App Service Environments tootake advantage of their advanced features and hello larger pools of capacity.</span></span>  

<span data-ttu-id="8c184-136">Hajtsa végre az ezen lépések tooconfigure nagy sűrűségű tároló az alkalmazásokhoz:</span><span class="sxs-lookup"><span data-stu-id="8c184-136">Follow these steps tooconfigure high density hosting for your apps:</span></span>

1. <span data-ttu-id="8c184-137">Hello App Service Environment-környezet konfigurálása, és válassza ki a feldolgozókészleten dedikált toohello nagy sűrűségű üzemeltető forgatókönyv esetében.</span><span class="sxs-lookup"><span data-stu-id="8c184-137">Configure hello App Service Environment and choose a worker pool that is dedicated toohello high density hosting scenario.</span></span>
1. <span data-ttu-id="8c184-138">Egy App Service-csomag létrehozása, és a méret az toouse összes hello hello feldolgozókészletek elérhető kapacitást.</span><span class="sxs-lookup"><span data-stu-id="8c184-138">Create a single App Service plan, and scale it toouse all hello available capacity on hello worker pool.</span></span>
1. <span data-ttu-id="8c184-139">App Service-csomag hello hello PerSiteScaling jelző tootrue beállítani.</span><span class="sxs-lookup"><span data-stu-id="8c184-139">Set hello PerSiteScaling flag tootrue on hello App Service plan.</span></span>
1. <span data-ttu-id="8c184-140">Új alkalmazások létrehozása és az App Service-csomag toothat hozzárendelve a **numberOfWorkers** tulajdonsága túl**1**.</span><span class="sxs-lookup"><span data-stu-id="8c184-140">New apps are created and assigned toothat App Service plan with the **numberOfWorkers** property set too**1**.</span></span> <span data-ttu-id="8c184-141">Ezzel a konfigurációval hello legmagasabb sűrűség lehetséges a munkavégző készlettől adja eredményül.</span><span class="sxs-lookup"><span data-stu-id="8c184-141">Using this configuration yields hello highest density possible on this worker pool.</span></span>
1. <span data-ttu-id="8c184-142">hello száma munkavállalók egyenként beállítható egymástól függetlenül app toogrant további erőforrások igény szerint.</span><span class="sxs-lookup"><span data-stu-id="8c184-142">hello number of workers can be configured independently per app toogrant additional resources as needed.</span></span> <span data-ttu-id="8c184-143">Példa:</span><span class="sxs-lookup"><span data-stu-id="8c184-143">For example:</span></span>
    - <span data-ttu-id="8c184-144">A nagy igénybevételnek kitett alkalmazás állíthatja be **numberOfWorkers** túl**3** toohave több feldolgozási kapacitás az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8c184-144">A high-use app can set **numberOfWorkers** too**3** toohave more processing capacity for that app.</span></span> 
    - <span data-ttu-id="8c184-145">Alacsony használható alkalmazások állítania **numberOfWorkers** túl**1**.</span><span class="sxs-lookup"><span data-stu-id="8c184-145">Low-use apps would set **numberOfWorkers** too**1**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c184-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c184-146">Next Steps</span></span>

- [<span data-ttu-id="8c184-147">Az Azure App Service-csomagok részletes áttekintése</span><span class="sxs-lookup"><span data-stu-id="8c184-147">Azure App Service plans in-depth overview</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [<span data-ttu-id="8c184-148">Bevezetés tooApp Service-környezet</span><span class="sxs-lookup"><span data-stu-id="8c184-148">Introduction tooApp Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
