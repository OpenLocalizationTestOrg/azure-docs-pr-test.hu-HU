---
title: "Alkalmazás aaaWeb klónozása a PowerShell használatával"
description: "Megtudhatja, hogyan tooclone a Web Apps toonew webalkalmazások PowerShell használatával."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: b8882370d6db6939f8e4473ccc1414091bdcb8f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service alkalmazás klónozása a PowerShell használatával
Hello kiadása a Microsoft Azure PowerShell verzió 1.1.0-ás egy új beállítás lett hozzáadva egy már meglévő webalkalmazás tooa az újonnan létrehozott alkalmazást egy másik régióban található, vagy a hello adna hello felhasználói hello képességét tooclone tooNew AzureRMWebApp ugyanabban a régióban. Ezzel a lépéssel engedélyezi az ügyfelek toodeploy alkalmazások számos különböző régiókban között gyors és egyszerű.

Alkalmazás Klónozás jelenleg csak a premium szint app service-csomagokról támogatott. új hello szolgáltatás által használt azonos hello korlátozások Web Apps biztonsági másolat szolgáltatás, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Azure Resource Manager használatával kapcsolatos toolearn alapú Azure PowerShell-parancsmagok toomanage a Web Apps ellenőrzés [Azure Resource Manager PowerShell-parancsok alapú Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>A Klónozás egy meglévő alkalmazáshoz
Forgatókönyv: Egy létező webalkalmazása régióban déli középső Régiójában, hello felhasználói szeretné tooclone hello tartalma tooa webalkalmazása északi középső Régiójában régióban. Ehhez hello - SourceWebApp kapcsolóval hello PowerShell parancsmag toocreate egy új webalkalmazást hello Azure Resource Manager verzióját használja.

Tudatában hello erőforrás hello forrás webalkalmazás tartalmazó csoport nevét, a következő (ebben az esetben a forrás-webalkalmazás neve) PowerShell parancs tooget hello forrás webes alkalmazás információ hello használhatjuk:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

egy új App Service-csomag toocreate, a New-AzureRmAppServicePlan parancs, mint például a következő hello használhatjuk

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Új-AzureRmWebApp hello parancs használatával, azt hello északi középső Régiójában régióban hello új webalkalmazás létrehozása, és összekötését azt tooan App Service-csomag meglévő prémium csomagban, továbbá hello ugyanazt az erőforrást hello forrás webalkalmazásként csoportban, vagy adja meg egy új erőforráscsoportot is használhatók , hello következő mutatja be, amelyek:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

egy már meglévő webalkalmazás többek között az összes kapcsolódó üzembe helyezési, hello felhasználónak kell toouse tooclone hello IncludeSourceWebAppSlots paraméter, a következő PowerShell-paranccsal hello mutatja be, hogy a New-AzureRmWebApp parancs hello paraméter hello használatával:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

egy már meglévő webalkalmazás belül hello tooclone ugyanabban a régióban, hello a felhasználónak kell egy új erőforráscsoportot és egy új app service tervezni hello toocreate ugyanabban a régióban, majd ezzel a következő PowerShell parancs tooclone hello webalkalmazás hello

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a>A Klónozás egy meglévő App tooan App Service Environment-környezet
Forgatókönyv: Egy létező webalkalmazása régióban déli középső Régiójában, hello felhasználói szeretné tooclone hello tartalma tooa új webes alkalmazás tooan meglévő App Service-környezet (ASE).

Tudatában hello erőforrás hello forrás webalkalmazás tartalmazó csoport nevét, a következő (ebben az esetben a forrás-webalkalmazás neve) PowerShell parancs tooget hello forrás webes alkalmazás információ hello használhatjuk:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Hello ASE nevét, és hello az erőforráscsoport neve, amely hello ASE tartozik ismeretében hello felhasználói paranccsal hello New-AzureRmWebApp toocreate hello új webalkalmazásba az hello meglévő ASE, a következő hello mutatja be, amelyek:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

hello hely paraméter toolegacy ok miatt szükséges, de hello esetben létrehozása az app Service-környezetben, figyelmen kívül hagyja. 

## <a name="cloning-an-existing-app-slot"></a>Egy meglévő tárolóhelye klónozása
Forgatókönyv: hello felhasználói szeretné tooclone egy már meglévő webalkalmazás tooeither egy új webalkalmazást vagy webes alkalmazás új tárhely. Új webalkalmazás lehet hello hello ugyanabban a régióban hello eredeti webalkalmazás tárhely vagy egy másik régióban.

Tudatában hello erőforrás hello forrás webalkalmazás tartalmazó csoport nevét, a következő PowerShell-paranccsal tooget hello forrás webes alkalmazás a tárhely információkat (ebben az esetben a forrás-webappslot nevű) kötött tooWeb App forrás-webalkalmazás hello használhatunk:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

hello következő mutatja be a klón hello forrás web app tooa új webalkalmazás létrehozása:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Egy alkalmazás klónozása során a Traffic Manager konfigurálása
Több területi webalkalmazások létrehozása és konfigurálása az Azure Traffic Manager tooroute forgalom tooall a webalkalmazások, van egy n fontossággal bír tooinsure, amelyek ügyfelek alkalmazások magas rendelkezésre állású, egy létező webalkalmazása, hogy a Klónozás hello beállítás tooconnect mindkét webes alkalmazások tooeither új traffic manager-profil vagy egy meglévő - vegye figyelembe, hogy egyetlen Azure Resource Manager a Traffic Manager használata támogatott.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Egy alkalmazás klónozása során egy új Traffic Manager-profil létrehozása
Forgatókönyv: hello felhasználói tooclone egy webes alkalmazás tooanother terület szeretné az Azure erőforrás-kezelő traffic manager-profilt, amely a webes alkalmazások egyaránt lehetnek konfigurálása közben. hello következő bemutatja a klónt hello forrás web app tooa új webalkalmazás új Traffic Manager-profil konfigurálása során:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-tooan-existing-traffic-manager-profile"></a>Új klónozott webalkalmazás tooan meglévő Traffic Manager-profil hozzáadása
Forgatókönyv: a hello felhasználó már rendelkezik az Azure erőforrás-kezelő traffic manager-profilt, hogy azt szeretné, hogy tooadd mindkét webalkalmazások neve azonosítja. toodo tehát, először meg kell tooassemble hello meglévő traffic manager profilazonosító, fel kell hello előfizetés-azonosító, erőforráscsoport-név és hello meglévő traffic manager-profil neve.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Miután hello traffic Manager-azonosító, hello következő mutatja be, klónt hello forrás web app tooa új webalkalmazás tooan meglévő Traffic Manager-profil hozzáadása őket közben:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuális korlátozások
Ez a funkció jelenleg előzetes verzióban érhetők, jelenleg is dolgozunk tooadd új képességeket adott idő alatt, a következő lista hello vannak hello hello jelenlegi verziója app Klónozás ismert korlátozásai:

* Automatikus skálázási beállításokat a rendszer nem klónozható
* Biztonsági mentés ütemezése beállítások vannak nem klónozható
* Vannak nem klónozható a VNET beállításait
* App Insights nem automatikusan be vannak állítva hello cél webalkalmazásban
* Egyszerű hitelesítési beállítások a rendszer nem klónozható
* A kudu bővítmény nem klónozható vannak
* Tipp szabályok nem klónozható vannak
* Adatbázis-tartalom nem klónozható vannak

### <a name="references"></a>Referencia
* [Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)
* [Webes alkalmazás Klónozás Azure portál használatával](app-service-web-app-cloning-portal.md)
* [Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben](web-sites-backup.md)
* [Az Azure Traffic Manager előzetes Azure Resource Manager támogatása](../traffic-manager/traffic-manager-powershell-arm.md)
* [Bevezetés tooApp Service-környezet](app-service-app-service-environment-intro.md)
* [Az Azure PowerShell használata az Azure Resource Managerrel](../powershell-azure-resource-manager.md)

