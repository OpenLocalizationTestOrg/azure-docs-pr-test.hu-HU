---
title: "Webes alkalmazás Klónozás PowerShell használatával"
description: "Útmutató a webalkalmazások a PowerShell használatával új webalkalmazások klónozását."
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
ms.openlocfilehash: d47d5a2f7d2462525bf37718a234e222b4f64e6d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a><span data-ttu-id="10ade-103">Azure App Service alkalmazás klónozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="10ade-103">Azure App Service App Cloning Using PowerShell</span></span>
<span data-ttu-id="10ade-104">A Microsoft Azure PowerShell verziója 1.1.0-ás kiadása egy új beállítás klónozni egy már meglévő webalkalmazás egy újonnan létrehozott alkalmazás egy másik régióban vagy ugyanabban a régióban képes tenné a felhasználónak új AzureRMWebApp bővült.</span><span class="sxs-lookup"><span data-stu-id="10ade-104">With the release of Microsoft Azure PowerShell version 1.1.0 a new option has been added to New-AzureRMWebApp that would give the user the ability to clone an existing Web App to a newly created app in a different region or in the same region.</span></span> <span data-ttu-id="10ade-105">Ez lehetővé teszi az ügyfelek központi telepítése a alkalmazások számos különböző régiókban teljes gyorsan és egyszerűen.</span><span class="sxs-lookup"><span data-stu-id="10ade-105">This will enable customers to deploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="10ade-106">Alkalmazás Klónozás jelenleg csak a premium szint app service-csomagokról támogatott.</span><span class="sxs-lookup"><span data-stu-id="10ade-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="10ade-107">Az új szolgáltatás ugyanazokkal a korlátozásokkal használja, mint a Web Apps biztonsági másolat szolgáltatás című [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="10ade-107">The new feature uses the same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="10ade-108">Azure Resource Manager-alapú kezelheti a Web Apps ellenőrzése az Azure PowerShell-parancsmagok használatával kapcsolatos további [Azure Resource Manager PowerShell-parancsok alapú Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="10ade-108">To learn about using Azure Resource Manager based Azure PowerShell cmdlets to manage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="cloning-an-existing-app"></a><span data-ttu-id="10ade-109">A Klónozás egy meglévő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="10ade-109">Cloning an existing App</span></span>
<span data-ttu-id="10ade-110">Forgatókönyv: Egy létező webalkalmazása régióban déli középső Régiójában, a felhasználó szeretné klónozza a tartalom egy új webalkalmazást északi középső Régiójában régióban.</span><span class="sxs-lookup"><span data-stu-id="10ade-110">Scenario: An existing web app in South Central US region, the user would like to clone the contents to a new web app in North Central US region.</span></span> <span data-ttu-id="10ade-111">Ehhez az Azure Resource Manager verziója, a PowerShell-parancsmag segítségével hozzon létre egy új webalkalmazást a - SourceWebApp kapcsolóval.</span><span class="sxs-lookup"><span data-stu-id="10ade-111">This can be accomplished by using the Azure Resource Manager version of the PowerShell cmdlet to create a new web app with the -SourceWebApp option.</span></span>

<span data-ttu-id="10ade-112">Az erőforráscsoport neve, amely tartalmazza a forrás-webalkalmazás ismeretében is használjuk a következő PowerShell-parancs adatokat lekérdezni a forrás web app (ebben az esetben a forrás-webalkalmazás neve):</span><span class="sxs-lookup"><span data-stu-id="10ade-112">Knowing the resource group name that contains the source web app, we can use the following PowerShell command to get the source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="10ade-113">Hozzon létre egy új App Service-csomag, az alábbi példában látható módon a New-AzureRmAppServicePlan parancs is használjuk</span><span class="sxs-lookup"><span data-stu-id="10ade-113">To create a new App Service Plan, we can use New-AzureRmAppServicePlan command as in the following example</span></span>

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

<span data-ttu-id="10ade-114">A New-AzureRmWebApp paranccsal azt az új webalkalmazás létrehozása a régióban északi középső Régiójában, és azt összekötését egy App Service-csomag meglévő prémium tarifacsomagra, továbbá a Microsoft ugyanabban az erőforráscsoportban használják a forrás web app, vagy adja meg egy új erőforráscsoportot, a következő mutatja be, amelyek:</span><span class="sxs-lookup"><span data-stu-id="10ade-114">Using the New-AzureRmWebApp command, we can create the new web app in the North Central US region, and tie it to an existing premium tier App Service Plan, moreover we can use the same resource group as the source web app, or define a new resource group, the following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

<span data-ttu-id="10ade-115">Egy létező webalkalmazása, beleértve az összes társított telepítési klónozását tárolóhely, a felhasználónak kell használnia a IncludeSourceWebAppSlots paraméter, a következő PowerShell-parancs bemutatja, hogy a paramétert a New-AzureRmWebApp paranccsal:</span><span class="sxs-lookup"><span data-stu-id="10ade-115">To clone an existing web app including all associated deployment slots, the user will need to use the IncludeSourceWebAppSlots parameter, the following PowerShell command demonstrates the use of that parameter with the New-AzureRmWebApp command:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

<span data-ttu-id="10ade-116">Klónozásához egy már meglévő webalkalmazás belül ugyanabban a régióban, a felhasználó kell hoznia egy új erőforráscsoportot és egy új app service tervezze meg ugyanabban a régióban, és a következő PowerShell-parancs segítségével klónozza a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="10ade-116">To clone an existing web app within the same region, the user will need to create a new resource group and a new app service plan in the same region, and then using the following PowerShell command to clone the web app</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a><span data-ttu-id="10ade-117">Egy meglévő alkalmazást az App Service-környezetek való klónozása</span><span class="sxs-lookup"><span data-stu-id="10ade-117">Cloning an existing App to an App Service Environment</span></span>
<span data-ttu-id="10ade-118">Forgatókönyv: Egy létező webalkalmazása régióban déli középső Régiójában, a felhasználó szeretné klónozza a tartalom egy új webalkalmazást a egy meglévő App Service környezetben (ASE).</span><span class="sxs-lookup"><span data-stu-id="10ade-118">Scenario: An existing web app in South Central US region, the user would like to clone the contents to a new web app to an existing App Service Environment (ASE).</span></span>

<span data-ttu-id="10ade-119">Az erőforráscsoport neve, amely tartalmazza a forrás-webalkalmazás ismeretében is használjuk a következő PowerShell-parancs adatokat lekérdezni a forrás web app (ebben az esetben a forrás-webalkalmazás neve):</span><span class="sxs-lookup"><span data-stu-id="10ade-119">Knowing the resource group name that contains the source web app, we can use the following PowerShell command to get the source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="10ade-120">TUDATÁBAN a ASE nevét, és az erőforráscsoport neve, amely a ASE tartozik, a felhasználó használhatja a New-AzureRmWebApp parancsot az új webalkalmazás létrehozása a meglévő ASE a, a következő mutatja be, amelyek:</span><span class="sxs-lookup"><span data-stu-id="10ade-120">Knowing the ASE's name, and the resource group name that the ASE belongs to, the user can use the New-AzureRmWebApp command to create the new web app in the existing ASE, the following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

<span data-ttu-id="10ade-121">A hely paraméter örökölt ok miatt szükséges, de az app Service-környezetben létrehozása esetén, figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="10ade-121">The Location parameter is required due to legacy reason, but in the case of creating an app in an ASE it will be ignored.</span></span> 

## <a name="cloning-an-existing-app-slot"></a><span data-ttu-id="10ade-122">Egy meglévő tárolóhelye klónozása</span><span class="sxs-lookup"><span data-stu-id="10ade-122">Cloning an existing App Slot</span></span>
<span data-ttu-id="10ade-123">Forgatókönyv: A felhasználó szeretné klónozni egy meglévő Web App hellyel vagy egy új webalkalmazást vagy webes alkalmazás új tárhely.</span><span class="sxs-lookup"><span data-stu-id="10ade-123">Scenario: The user would like to clone an existing Web App Slot to either a new Web App or a new Web App slot.</span></span> <span data-ttu-id="10ade-124">Az új webalkalmazásba lehet ugyanabban a régióban, mint az eredeti webalkalmazás tárhely vagy egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="10ade-124">The new Web App can be in the same region as the original Web App slot or in a different region.</span></span>

<span data-ttu-id="10ade-125">Az erőforráscsoport neve, amely tartalmazza a forrás-webalkalmazás ismeretében is használjuk a következő PowerShell-parancs lekérése a forrás web app tárolóhely (ebben az esetben a forrás-webappslot nevű) webalkalmazás forrás-webalkalmazás kötve:</span><span class="sxs-lookup"><span data-stu-id="10ade-125">Knowing the resource group name that contains the source web app, we can use the following PowerShell command to get the source web app slot's information (in this case named source-webappslot) tied to Web App source-webapp:</span></span>

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

<span data-ttu-id="10ade-126">A következő mutatja be, a forrás webes alkalmazás egy új webalkalmazást a klón létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="10ade-126">The following demonstrates creating a clone of the source web app to a new web app:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a><span data-ttu-id="10ade-127">Egy alkalmazás klónozása során a Traffic Manager konfigurálása</span><span class="sxs-lookup"><span data-stu-id="10ade-127">Configuring Traffic Manager while cloning a App</span></span>
<span data-ttu-id="10ade-128">Több területi webalkalmazások létrehozása és konfigurálása az Azure Traffic Manager forgalom irányítására a webes alkalmazásokhoz való, egy webfarmos n fontos annak érdekében, hogy ügyfelek alkalmazások magas rendelkezésre állású, egy létező webalkalmazása klónozásakor lehetősége van a mindkét webalkalmazások csatlakozni vagy egy új traffic manager-profilt, vagy egy meglévő,-vegye figyelembe, hogy egyetlen Azure Resource Manager Traffic Manager verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="10ade-128">Creating multi-region web apps and configuring Azure Traffic Manager to route traffic to all these web apps, is a n important scenario to insure that customers' apps are highly available, when cloning an existing web app you have the option to connect both web apps to either a new traffic manager profile or an existing one - note that only Azure Resource Manager version of Traffic Manager is supported.</span></span>

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a><span data-ttu-id="10ade-129">Egy alkalmazás klónozása során egy új Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="10ade-129">Creating a new Traffic Manager profile while cloning a App</span></span>
<span data-ttu-id="10ade-130">Forgatókönyv: A felhasználó szeretné klónozni egy webalkalmazás egy másik régióban, az Azure erőforrás-kezelő traffic manager-profilt, amely a webes alkalmazások egyaránt lehetnek konfigurálása közben.</span><span class="sxs-lookup"><span data-stu-id="10ade-130">Scenario: The user would like to clone an web app to another region, while configuring an Azure Resource Manager traffic manager profile that include both web apps.</span></span> <span data-ttu-id="10ade-131">A következő bemutatja klónt készíteni egy új webalkalmazást a forrás webalkalmazás új Traffic Manager-profil konfigurálása során:</span><span class="sxs-lookup"><span data-stu-id="10ade-131">The following demonstrates creating a clone of the source web app to a new web app while configuring a new Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a><span data-ttu-id="10ade-132">Webalkalmazás hozzáadása az új klónozott egy meglévő Traffic Manager-profilt</span><span class="sxs-lookup"><span data-stu-id="10ade-132">Adding new cloned Web App to an existing Traffic Manager profile</span></span>
<span data-ttu-id="10ade-133">Forgatókönyv: A felhasználó már rendelkezik az Azure erőforrás-kezelő traffic manager-profil, akkor mindkét webalkalmazások hozzáadása végpontként szeretne.</span><span class="sxs-lookup"><span data-stu-id="10ade-133">Scenario: The user already have an Azure Resource Manager traffic manager profile that he would like to add both web apps as endpoints.</span></span> <span data-ttu-id="10ade-134">Ehhez először igazolnia kell a meglévő traffic manager Profilazonosító összeállítása, fel kell az előfizetés-azonosító, erőforráscsoport-név és a meglévő traffic manager-profil neve.</span><span class="sxs-lookup"><span data-stu-id="10ade-134">To do so, we first need to assemble the existing traffic manager profile id, we will need the subscription id, resource group name and the existing traffic manager profile name.</span></span>

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

<span data-ttu-id="10ade-135">Miután a traffic Manager-azonosító, a következő bemutatja klónt készíteni a forrás-webalkalmazás, egy új webalkalmazást a meglévő Traffic Manager-profil való hozzáadás során:</span><span class="sxs-lookup"><span data-stu-id="10ade-135">After having the traffic manger id, the following demonstrates creating a clone of the source web app to a new web app while adding them to an existing Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a><span data-ttu-id="10ade-136">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="10ade-136">Current Restrictions</span></span>
<span data-ttu-id="10ade-137">Ez a funkció jelenleg előzetes verzióban érhetők, új képességeket adhat a időbeli dolgozunk, a következő lista rendszer app Klónozás aktuális verziójának ismert korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="10ade-137">This feature is currently in preview, we are working to add new capabilities over time, the following list are the known restrictions on the current version of app cloning:</span></span>

* <span data-ttu-id="10ade-138">Automatikus skálázási beállításokat a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="10ade-138">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="10ade-139">Biztonsági mentés ütemezése beállítások vannak nem klónozható</span><span class="sxs-lookup"><span data-stu-id="10ade-139">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="10ade-140">Vannak nem klónozható a VNET beállításait</span><span class="sxs-lookup"><span data-stu-id="10ade-140">VNET settings are not cloned</span></span>
* <span data-ttu-id="10ade-141">App Insights nem automatikusan be vannak állítva a célként megadott webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="10ade-141">App Insights are not automatically set up on the destination web app</span></span>
* <span data-ttu-id="10ade-142">Egyszerű hitelesítési beállítások a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="10ade-142">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="10ade-143">A kudu bővítmény nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="10ade-143">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="10ade-144">Tipp szabályok nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="10ade-144">TiP rules are not cloned</span></span>
* <span data-ttu-id="10ade-145">Adatbázis-tartalom nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="10ade-145">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="10ade-146">Referencia</span><span class="sxs-lookup"><span data-stu-id="10ade-146">References</span></span>
* [<span data-ttu-id="10ade-147">Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="10ade-147">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="10ade-148">Webes alkalmazás Klónozás Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="10ade-148">Web App Cloning using Azure Portal</span></span>](app-service-web-app-cloning-portal.md)
* [<span data-ttu-id="10ade-149">Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="10ade-149">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="10ade-150">Az Azure Traffic Manager előzetes Azure Resource Manager támogatása</span><span class="sxs-lookup"><span data-stu-id="10ade-150">Azure Resource Manager support for Azure Traffic Manager Preview</span></span>](../traffic-manager/traffic-manager-powershell-arm.md)
* [<span data-ttu-id="10ade-151">Az App Service Environment bemutatása</span><span class="sxs-lookup"><span data-stu-id="10ade-151">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="10ade-152">Az Azure PowerShell használata az Azure Resource Managerrel</span><span class="sxs-lookup"><span data-stu-id="10ade-152">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

