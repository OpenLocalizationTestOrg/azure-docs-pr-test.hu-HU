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
# <a name="high-density-hosting-on-azure-app-service"></a>Nagy sűrűségű üzemeltetésének Azure App Service
App Service használata esetén a rendszer leválasztja az alkalmazást tooit lefoglalta két fogalom hello kapacitás:

* **Alkalmazás hello:** hello alkalmazás és a futásidejű konfigurálását jelenti. Például hello tartalmazza, amelyek futásidejű hello .NET verzióját kell betöltéséhez hello Alkalmazásbeállítások.
* **App Service-csomag hello:** hello kapacitás, a rendelkezésre álló készlet és a helység hello alkalmazás hello jellemzői határozza meg. Nagy (négy magok) számítógép, a négy példányt, a prémium szolgáltatások, az USA keleti régiója, előfordulhat, hogy lehet jellemzőit.

Az alkalmazások mindig csatolt tooan App Service-csomag, de az App Service-csomag biztosíthat a kapacitás tooone, illetve további alkalmazásokat.

Ennek eredményeképpen hello platform lehetővé teszi a hello rugalmasságot tooisolate egyetlen alkalmazást, vagy ossza meg az App Service-csomag erőforrások megosztása több alkalmazásokkal rendelkeznek.

Azonban amikor több alkalmazás az App Service-csomag megosztásához az alkalmazás egy példánya fut, hogy az alkalmazásszolgáltatási csomag összes példányán.

## <a name="per-app-scaling"></a>Egy alkalmazás skálázás
*Egy alkalmazás skálázás* egy szolgáltatás, amely az App Service-csomag szintjén engedélyezhető, és alkalmazásonként használja.

Alkalmazásonkénti skálázás arányosan egymástól függetlenül az App Service-csomag, amelyen az alkalmazás. Ezzel a módszerrel egy App Service csomag lehet méretezni too10 példányok, de az alkalmazás csak öt toouse állítható be.

   >[!NOTE]
   >Alkalmazásonkénti skálázás csak érhető **prémium** SKU App Service-csomagok
   >

### <a name="per-app-scaling-using-powershell"></a>Alkalmazásonkénti skálázás a PowerShell használatával

Egy konfigurált tervet is létrehozhat egy */ alkalmazás skálázás* történő hello a terv ```-perSiteScaling $true``` toohello attribútum ```New-AzureRmAppServicePlan``` parancsmag

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Ha azt szeretné, hogy egy meglévő App Service tooupdate megtervezése toouse ezt a szolgáltatást: 

- hello cél terv beolvasása```Get-AzureRmAppServicePlan```
- helyileg hello tulajdonság módosítása```$newASP.PerSiteScaling = $true```
- a módosítások vissza tooazure könyvelési```Set-AzureRmAppServicePlan``` 

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

Hello alkalmazási szintű tooconfigure hello több példányban hello alkalmazás az app service-csomag hello is használni kell.

Az alábbi példa hello hello app hello alapul szolgáló app service csomag méretezik ki a rendszer korlátozott tootwo példányok, függetlenül attól, hogy hány.

```
# Get hello app we want tooconfigure toouse "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify hello NumberOfWorkers setting toohello desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back tooazure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> $newapp. SiteConfig.NumberOfWorkers másik formája $newapp, amely. MaxNumberOfWorkers. Alkalmazásonkénti $newapp skálázás használja. SiteConfig.NumberOfWorkers toodetermine hello méretezési jellemzői hello alkalmazást.

### <a name="per-app-scaling-using-azure-resource-manager"></a>Egy alkalmazás skálázás Azure Resource Manager használatával

hello következő *Azure Resource Manager sablon* hoz létre:

- Kimenő too10 példányok méretezett App Service-csomag
- olyan alkalmazás, amelynek tooscale tooa legfeljebb öt példányok van beállítva.

hello App Service-csomag állít hello **PerSiteScaling** tulajdonság tootrue ```"perSiteScaling": true```. hello app állít hello **munkavállalók száma** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.

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

## <a name="recommended-configuration-for-high-density-hosting"></a>Ajánlott konfiguráció nagy sűrűségű üzemeltetéséhez
Egy alkalmazás skálázás van olyan szolgáltatás, amely globális Azure-régiók és az App Service Environment-környezetek is engedélyezve van. Azonban hello ajánlott stratégia App Service Environment-környezetek tootake kihasználják a speciális funkciók és kapacitás nagyobb készleteinek hello használatához.  

Hajtsa végre az ezen lépések tooconfigure nagy sűrűségű tároló az alkalmazásokhoz:

1. Hello App Service Environment-környezet konfigurálása, és válassza ki a feldolgozókészleten dedikált toohello nagy sűrűségű üzemeltető forgatókönyv esetében.
1. Egy App Service-csomag létrehozása, és a méret az toouse összes hello hello feldolgozókészletek elérhető kapacitást.
1. App Service-csomag hello hello PerSiteScaling jelző tootrue beállítani.
1. Új alkalmazások létrehozása és az App Service-csomag toothat hozzárendelve a **numberOfWorkers** tulajdonsága túl**1**. Ezzel a konfigurációval hello legmagasabb sűrűség lehetséges a munkavégző készlettől adja eredményül.
1. hello száma munkavállalók egyenként beállítható egymástól függetlenül app toogrant további erőforrások igény szerint. Példa:
    - A nagy igénybevételnek kitett alkalmazás állíthatja be **numberOfWorkers** túl**3** toohave több feldolgozási kapacitás az alkalmazáshoz. 
    - Alacsony használható alkalmazások állítania **numberOfWorkers** túl**1**.

## <a name="next-steps"></a>Következő lépések

- [Az Azure App Service-csomagok részletes áttekintése](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Bevezetés tooApp Service-környezet](../app-service-web/app-service-app-service-environment-intro.md)
