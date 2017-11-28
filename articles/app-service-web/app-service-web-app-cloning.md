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
# <a name="azure-app-service-app-cloning-using-powershell"></a><span data-ttu-id="ee4f3-103">Azure App Service alkalmazás klónozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="ee4f3-103">Azure App Service App Cloning Using PowerShell</span></span>
<span data-ttu-id="ee4f3-104">Hello kiadása a Microsoft Azure PowerShell verzió 1.1.0-ás egy új beállítás lett hozzáadva egy már meglévő webalkalmazás tooa az újonnan létrehozott alkalmazást egy másik régióban található, vagy a hello adna hello felhasználói hello képességét tooclone tooNew AzureRMWebApp ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-104">With hello release of Microsoft Azure PowerShell version 1.1.0 a new option has been added tooNew-AzureRMWebApp that would give hello user hello ability tooclone an existing Web App tooa newly created app in a different region or in hello same region.</span></span> <span data-ttu-id="ee4f3-105">Ezzel a lépéssel engedélyezi az ügyfelek toodeploy alkalmazások számos különböző régiókban között gyors és egyszerű.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-105">This will enable customers toodeploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="ee4f3-106">Alkalmazás Klónozás jelenleg csak a premium szint app service-csomagokról támogatott.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="ee4f3-107">új hello szolgáltatás által használt azonos hello korlátozások Web Apps biztonsági másolat szolgáltatás, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="ee4f3-107">hello new feature uses hello same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="ee4f3-108">Azure Resource Manager használatával kapcsolatos toolearn alapú Azure PowerShell-parancsmagok toomanage a Web Apps ellenőrzés [Azure Resource Manager PowerShell-parancsok alapú Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="ee4f3-108">toolearn about using Azure Resource Manager based Azure PowerShell cmdlets toomanage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="cloning-an-existing-app"></a><span data-ttu-id="ee4f3-109">A Klónozás egy meglévő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="ee4f3-109">Cloning an existing App</span></span>
<span data-ttu-id="ee4f3-110">Forgatókönyv: Egy létező webalkalmazása régióban déli középső Régiójában, hello felhasználói szeretné tooclone hello tartalma tooa webalkalmazása északi középső Régiójában régióban.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-110">Scenario: An existing web app in South Central US region, hello user would like tooclone hello contents tooa new web app in North Central US region.</span></span> <span data-ttu-id="ee4f3-111">Ehhez hello - SourceWebApp kapcsolóval hello PowerShell parancsmag toocreate egy új webalkalmazást hello Azure Resource Manager verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-111">This can be accomplished by using hello Azure Resource Manager version of hello PowerShell cmdlet toocreate a new web app with hello -SourceWebApp option.</span></span>

<span data-ttu-id="ee4f3-112">Tudatában hello erőforrás hello forrás webalkalmazás tartalmazó csoport nevét, a következő (ebben az esetben a forrás-webalkalmazás neve) PowerShell parancs tooget hello forrás webes alkalmazás információ hello használhatjuk:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-112">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="ee4f3-113">egy új App Service-csomag toocreate, a New-AzureRmAppServicePlan parancs, mint például a következő hello használhatjuk</span><span class="sxs-lookup"><span data-stu-id="ee4f3-113">toocreate a new App Service Plan, we can use New-AzureRmAppServicePlan command as in hello following example</span></span>

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

<span data-ttu-id="ee4f3-114">Új-AzureRmWebApp hello parancs használatával, azt hello északi középső Régiójában régióban hello új webalkalmazás létrehozása, és összekötését azt tooan App Service-csomag meglévő prémium csomagban, továbbá hello ugyanazt az erőforrást hello forrás webalkalmazásként csoportban, vagy adja meg egy új erőforráscsoportot is használhatók , hello következő mutatja be, amelyek:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-114">Using hello New-AzureRmWebApp command, we can create hello new web app in hello North Central US region, and tie it tooan existing premium tier App Service Plan, moreover we can use hello same resource group as hello source web app, or define a new resource group, hello following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

<span data-ttu-id="ee4f3-115">egy már meglévő webalkalmazás többek között az összes kapcsolódó üzembe helyezési, hello felhasználónak kell toouse tooclone hello IncludeSourceWebAppSlots paraméter, a következő PowerShell-paranccsal hello mutatja be, hogy a New-AzureRmWebApp parancs hello paraméter hello használatával:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-115">tooclone an existing web app including all associated deployment slots, hello user will need toouse hello IncludeSourceWebAppSlots parameter, hello following PowerShell command demonstrates hello use of that parameter with hello New-AzureRmWebApp command:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

<span data-ttu-id="ee4f3-116">egy már meglévő webalkalmazás belül hello tooclone ugyanabban a régióban, hello a felhasználónak kell egy új erőforráscsoportot és egy új app service tervezni hello toocreate ugyanabban a régióban, majd ezzel a következő PowerShell parancs tooclone hello webalkalmazás hello</span><span class="sxs-lookup"><span data-stu-id="ee4f3-116">tooclone an existing web app within hello same region, hello user will need toocreate a new resource group and a new app service plan in hello same region, and then using hello following PowerShell command tooclone hello web app</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a><span data-ttu-id="ee4f3-117">A Klónozás egy meglévő App tooan App Service Environment-környezet</span><span class="sxs-lookup"><span data-stu-id="ee4f3-117">Cloning an existing App tooan App Service Environment</span></span>
<span data-ttu-id="ee4f3-118">Forgatókönyv: Egy létező webalkalmazása régióban déli középső Régiójában, hello felhasználói szeretné tooclone hello tartalma tooa új webes alkalmazás tooan meglévő App Service-környezet (ASE).</span><span class="sxs-lookup"><span data-stu-id="ee4f3-118">Scenario: An existing web app in South Central US region, hello user would like tooclone hello contents tooa new web app tooan existing App Service Environment (ASE).</span></span>

<span data-ttu-id="ee4f3-119">Tudatában hello erőforrás hello forrás webalkalmazás tartalmazó csoport nevét, a következő (ebben az esetben a forrás-webalkalmazás neve) PowerShell parancs tooget hello forrás webes alkalmazás információ hello használhatjuk:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-119">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="ee4f3-120">Hello ASE nevét, és hello az erőforráscsoport neve, amely hello ASE tartozik ismeretében hello felhasználói paranccsal hello New-AzureRmWebApp toocreate hello új webalkalmazásba az hello meglévő ASE, a következő hello mutatja be, amelyek:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-120">Knowing hello ASE's name, and hello resource group name that hello ASE belongs to, hello user can use hello New-AzureRmWebApp command toocreate hello new web app in hello existing ASE, hello following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

<span data-ttu-id="ee4f3-121">hello hely paraméter toolegacy ok miatt szükséges, de hello esetben létrehozása az app Service-környezetben, figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-121">hello Location parameter is required due toolegacy reason, but in hello case of creating an app in an ASE it will be ignored.</span></span> 

## <a name="cloning-an-existing-app-slot"></a><span data-ttu-id="ee4f3-122">Egy meglévő tárolóhelye klónozása</span><span class="sxs-lookup"><span data-stu-id="ee4f3-122">Cloning an existing App Slot</span></span>
<span data-ttu-id="ee4f3-123">Forgatókönyv: hello felhasználói szeretné tooclone egy már meglévő webalkalmazás tooeither egy új webalkalmazást vagy webes alkalmazás új tárhely.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-123">Scenario: hello user would like tooclone an existing Web App Slot tooeither a new Web App or a new Web App slot.</span></span> <span data-ttu-id="ee4f3-124">Új webalkalmazás lehet hello hello ugyanabban a régióban hello eredeti webalkalmazás tárhely vagy egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-124">hello new Web App can be in hello same region as hello original Web App slot or in a different region.</span></span>

<span data-ttu-id="ee4f3-125">Tudatában hello erőforrás hello forrás webalkalmazás tartalmazó csoport nevét, a következő PowerShell-paranccsal tooget hello forrás webes alkalmazás a tárhely információkat (ebben az esetben a forrás-webappslot nevű) kötött tooWeb App forrás-webalkalmazás hello használhatunk:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-125">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app slot's information (in this case named source-webappslot) tied tooWeb App source-webapp:</span></span>

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

<span data-ttu-id="ee4f3-126">hello következő mutatja be a klón hello forrás web app tooa új webalkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-126">hello following demonstrates creating a clone of hello source web app tooa new web app:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a><span data-ttu-id="ee4f3-127">Egy alkalmazás klónozása során a Traffic Manager konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ee4f3-127">Configuring Traffic Manager while cloning a App</span></span>
<span data-ttu-id="ee4f3-128">Több területi webalkalmazások létrehozása és konfigurálása az Azure Traffic Manager tooroute forgalom tooall a webalkalmazások, van egy n fontossággal bír tooinsure, amelyek ügyfelek alkalmazások magas rendelkezésre állású, egy létező webalkalmazása, hogy a Klónozás hello beállítás tooconnect mindkét webes alkalmazások tooeither új traffic manager-profil vagy egy meglévő - vegye figyelembe, hogy egyetlen Azure Resource Manager a Traffic Manager használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-128">Creating multi-region web apps and configuring Azure Traffic Manager tooroute traffic tooall these web apps, is a n important scenario tooinsure that customers' apps are highly available, when cloning an existing web app you have hello option tooconnect both web apps tooeither a new traffic manager profile or an existing one - note that only Azure Resource Manager version of Traffic Manager is supported.</span></span>

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a><span data-ttu-id="ee4f3-129">Egy alkalmazás klónozása során egy új Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee4f3-129">Creating a new Traffic Manager profile while cloning a App</span></span>
<span data-ttu-id="ee4f3-130">Forgatókönyv: hello felhasználói tooclone egy webes alkalmazás tooanother terület szeretné az Azure erőforrás-kezelő traffic manager-profilt, amely a webes alkalmazások egyaránt lehetnek konfigurálása közben.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-130">Scenario: hello user would like tooclone an web app tooanother region, while configuring an Azure Resource Manager traffic manager profile that include both web apps.</span></span> <span data-ttu-id="ee4f3-131">hello következő bemutatja a klónt hello forrás web app tooa új webalkalmazás új Traffic Manager-profil konfigurálása során:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-131">hello following demonstrates creating a clone of hello source web app tooa new web app while configuring a new Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-tooan-existing-traffic-manager-profile"></a><span data-ttu-id="ee4f3-132">Új klónozott webalkalmazás tooan meglévő Traffic Manager-profil hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ee4f3-132">Adding new cloned Web App tooan existing Traffic Manager profile</span></span>
<span data-ttu-id="ee4f3-133">Forgatókönyv: a hello felhasználó már rendelkezik az Azure erőforrás-kezelő traffic manager-profilt, hogy azt szeretné, hogy tooadd mindkét webalkalmazások neve azonosítja.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-133">Scenario: hello user already have an Azure Resource Manager traffic manager profile that he would like tooadd both web apps as endpoints.</span></span> <span data-ttu-id="ee4f3-134">toodo tehát, először meg kell tooassemble hello meglévő traffic manager profilazonosító, fel kell hello előfizetés-azonosító, erőforráscsoport-név és hello meglévő traffic manager-profil neve.</span><span class="sxs-lookup"><span data-stu-id="ee4f3-134">toodo so, we first need tooassemble hello existing traffic manager profile id, we will need hello subscription id, resource group name and hello existing traffic manager profile name.</span></span>

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

<span data-ttu-id="ee4f3-135">Miután hello traffic Manager-azonosító, hello következő mutatja be, klónt hello forrás web app tooa új webalkalmazás tooan meglévő Traffic Manager-profil hozzáadása őket közben:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-135">After having hello traffic manger id, hello following demonstrates creating a clone of hello source web app tooa new web app while adding them tooan existing Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a><span data-ttu-id="ee4f3-136">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="ee4f3-136">Current Restrictions</span></span>
<span data-ttu-id="ee4f3-137">Ez a funkció jelenleg előzetes verzióban érhetők, jelenleg is dolgozunk tooadd új képességeket adott idő alatt, a következő lista hello vannak hello hello jelenlegi verziója app Klónozás ismert korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="ee4f3-137">This feature is currently in preview, we are working tooadd new capabilities over time, hello following list are hello known restrictions on hello current version of app cloning:</span></span>

* <span data-ttu-id="ee4f3-138">Automatikus skálázási beállításokat a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="ee4f3-138">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="ee4f3-139">Biztonsági mentés ütemezése beállítások vannak nem klónozható</span><span class="sxs-lookup"><span data-stu-id="ee4f3-139">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="ee4f3-140">Vannak nem klónozható a VNET beállításait</span><span class="sxs-lookup"><span data-stu-id="ee4f3-140">VNET settings are not cloned</span></span>
* <span data-ttu-id="ee4f3-141">App Insights nem automatikusan be vannak állítva hello cél webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="ee4f3-141">App Insights are not automatically set up on hello destination web app</span></span>
* <span data-ttu-id="ee4f3-142">Egyszerű hitelesítési beállítások a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="ee4f3-142">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="ee4f3-143">A kudu bővítmény nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="ee4f3-143">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="ee4f3-144">Tipp szabályok nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="ee4f3-144">TiP rules are not cloned</span></span>
* <span data-ttu-id="ee4f3-145">Adatbázis-tartalom nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="ee4f3-145">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="ee4f3-146">Referencia</span><span class="sxs-lookup"><span data-stu-id="ee4f3-146">References</span></span>
* [<span data-ttu-id="ee4f3-147">Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="ee4f3-147">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="ee4f3-148">Webes alkalmazás Klónozás Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="ee4f3-148">Web App Cloning using Azure Portal</span></span>](app-service-web-app-cloning-portal.md)
* [<span data-ttu-id="ee4f3-149">Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="ee4f3-149">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="ee4f3-150">Az Azure Traffic Manager előzetes Azure Resource Manager támogatása</span><span class="sxs-lookup"><span data-stu-id="ee4f3-150">Azure Resource Manager support for Azure Traffic Manager Preview</span></span>](../traffic-manager/traffic-manager-powershell-arm.md)
* [<span data-ttu-id="ee4f3-151">Bevezetés tooApp Service-környezet</span><span class="sxs-lookup"><span data-stu-id="ee4f3-151">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="ee4f3-152">Az Azure PowerShell használata az Azure Resource Managerrel</span><span class="sxs-lookup"><span data-stu-id="ee4f3-152">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

