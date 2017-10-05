---
title: "Alkalmazások közzététele egyéni felhasználók számára egy Azure RemoteApp-gyűjteményben (előzetes) | Microsoft Docs"
description: "Ismerje meg, hogyan tehet közzé alkalmazásokat egyéni felhasználók számára a csoportok használata helyett az Azure RemoteAppban."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
ms.openlocfilehash: c94ffffdec3e46ed08d941ee58dcf17b432e1dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a><span data-ttu-id="11aaf-103">Alkalmazások közzététele egyéni felhasználók számára egy Azure RemoteApp-gyűjteményben (előzetes)</span><span class="sxs-lookup"><span data-stu-id="11aaf-103">Publish applications to individual users in an Azure RemoteApp collection (Preview)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="11aaf-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="11aaf-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="11aaf-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="11aaf-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="11aaf-106">Ez a cikk ismerteti, hogyan tehet közzé alkalmazásokat egyéni felhasználók számára egy Azure RemoteApp-gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="11aaf-106">This article explains how to publish applications to individual users in an Azure RemoteApp collection.</span></span> <span data-ttu-id="11aaf-107">Ez az Azure RemoteApp egy új funkciója, amely jelenleg „privát előzetes verzióként”, csak egyes korai támogatók számára érhető el, értékelési célokra.</span><span class="sxs-lookup"><span data-stu-id="11aaf-107">This is new functionality in Azure RemoteApp, currently in private preview and available only to select early adopters for evaluation purposes.</span></span>

<span data-ttu-id="11aaf-108">Az Azure RemoteAppban eredetileg csak egy módja volt az alkalmazások „közzétételének”: a rendszergazda alkalmazásokat tehetett közzé a rendszerképről, amelyek a gyűjteményben lévő összes felhasználó számára láthatók voltak.</span><span class="sxs-lookup"><span data-stu-id="11aaf-108">Originally Azure RemoteApp enabled only one way of publishing applications: the administrator would publish apps from the image and they would be visible to all users in the collection.</span></span>

<span data-ttu-id="11aaf-109">Egy általános forgatókönyv számos alkalmazás egyetlen rendszerképbe való belefoglalása, és egy gyűjtemény üzembe helyezése a felügyeleti költségek csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="11aaf-109">A common scenario is to include many applications in a single image and deploy one collection in order to reduce management costs.</span></span> <span data-ttu-id="11aaf-110">Gyakran nem minden felhasználónak van szüksége minden alkalmazásra, így a rendszergazdák inkább az egyéni felhasználók számára szeretnének alkalmazásokat közzétenni, hogy ne lássák a szükségtelen alkalmazásokat az alkalmazásfolyamukban.</span><span class="sxs-lookup"><span data-stu-id="11aaf-110">Oftentimes not all applications are relevant to all users – administrators would prefer to publish apps to individual users so they don’t see unnecessary applications in their application feed.</span></span>

<span data-ttu-id="11aaf-111">Ez mostantól lehetséges az Azure RemoteAppban – jelenleg korlátozott előzetes verzióként.</span><span class="sxs-lookup"><span data-stu-id="11aaf-111">This is now possible in Azure RemoteApp – currently as a limited preview feature.</span></span> <span data-ttu-id="11aaf-112">Az új funkció rövid összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="11aaf-112">Here is a brief summary of the new functionality:</span></span>

1. <span data-ttu-id="11aaf-113">Egy gyűjtemény két módba állítható be:</span><span class="sxs-lookup"><span data-stu-id="11aaf-113">A collection can be set into one of two modes:</span></span>
   
   * <span data-ttu-id="11aaf-114">az eredeti „gyűjtemény módba”, amelyben egy gyűjtemény minden felhasználója láthatja az összes közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="11aaf-114">the original collection mode, where all users in a collection can see all published applications.</span></span> <span data-ttu-id="11aaf-115">Ez az alapértelmezett mód.</span><span class="sxs-lookup"><span data-stu-id="11aaf-115">This is the default mode.</span></span>
   * <span data-ttu-id="11aaf-116">az új „alkalmazás módba”, amelyben a felhasználók csak a kifejezetten hozzájuk rendelt alkalmazásokat láthatják.</span><span class="sxs-lookup"><span data-stu-id="11aaf-116">the new application mode, where users only see applications that have been explicitly assigned to them</span></span>
2. <span data-ttu-id="11aaf-117">Az alkalmazás mód jelenleg csak Azure RemoteApp PowerShell-parancsmagok használatával engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="11aaf-117">At the moment the application mode can only be enabled using Azure RemoteApp PowerShell cmdlets.</span></span>
   
   * <span data-ttu-id="11aaf-118">Amikor alkalmazás módba állítja be a gyűjteményt, a gyűjtemény felhasználó-hozzárendelése nem kezelhető az Azure Portalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="11aaf-118">When set to application mode, user assignment in the collection cannot be managed through the Azure portal.</span></span> <span data-ttu-id="11aaf-119">A felhasználó-hozzárendelés kezelését PowerShell-parancsmagokon keresztül kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="11aaf-119">User assignment has to be managed through PowerShell cmdlets.</span></span>
3. <span data-ttu-id="11aaf-120">A felhasználók ekkor csak a közvetlenül a számukra közzétett alkalmazásokat láthatják.</span><span class="sxs-lookup"><span data-stu-id="11aaf-120">Users will only see the applications published directly to them.</span></span> <span data-ttu-id="11aaf-121">Lehetséges azonban, hogy a felhasználók továbbra is elindíthatják a rendszerképen lévő többi alkalmazást, ha közvetlenül az operációs rendszerben férnek hozzájuk.</span><span class="sxs-lookup"><span data-stu-id="11aaf-121">However, it may still be possible for a user to launch the other applications available on the image by accessing them directly in the operating system.</span></span>
   
   * <span data-ttu-id="11aaf-122">Ez a szolgáltatás nem biztosítja az alkalmazások biztonságos zárolását; csak a láthatóságot korlátozza az alkalmazásfolyamban.</span><span class="sxs-lookup"><span data-stu-id="11aaf-122">This feature does not provide a secure lockdown of applications; it only limits visibility in the application feed.</span></span>
   * <span data-ttu-id="11aaf-123">Ha el kell különítenie felhasználókat egyes alkalmazásoktól, különálló gyűjteményeket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="11aaf-123">If you need to isolate users from applications, you will need to use separate collections for that.</span></span>

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a><span data-ttu-id="11aaf-124">Az Azure RemoteApp PowerShell-parancsmagok beszerzése</span><span class="sxs-lookup"><span data-stu-id="11aaf-124">How to get Azure RemoteApp PowerShell cmdlets</span></span>
<span data-ttu-id="11aaf-125">Az előzetes verzió új funkcióinak kipróbálásához Azure PowerShell-parancsmagokat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="11aaf-125">To try the new preview functionality, you will need to use Azure PowerShell cmdlets.</span></span> <span data-ttu-id="11aaf-126">Az Azure felügyeleti portáljával jelenleg nincs lehetőség az új alkalmazás-közzétételi mód engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="11aaf-126">It is currently not possible to use the Azure Management portal to enable the new application publishing mode.</span></span>

<span data-ttu-id="11aaf-127">Először győződjön meg róla, hogy telepítve van az [Azure PowerShell modul](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="11aaf-127">First, make sure you have the [Azure PowerShell module](/powershell/azure/overview) installed.</span></span>

<span data-ttu-id="11aaf-128">Ezután indítsa el a PowerShell-konzolt rendszergazdai módban, és futtassa a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="11aaf-128">Then launch the PowerShell console in administrator mode and run the following cmdlet:</span></span>

        Add-AzureAccount

<span data-ttu-id="11aaf-129">A rendszer felkéri az Azure felhasználói nevének jelszavának megadására.</span><span class="sxs-lookup"><span data-stu-id="11aaf-129">It will prompt you for your Azure user name and password.</span></span> <span data-ttu-id="11aaf-130">Miután bejelentkezett, futtathatja az Azure RemoteApp-parancsmagokat az Azure-előfizetésén.</span><span class="sxs-lookup"><span data-stu-id="11aaf-130">Once signed in, you will be able to run Azure RemoteApp cmdlets against your Azure subscriptions.</span></span>

## <a name="how-to-check-which-mode-a-collection-is-in"></a><span data-ttu-id="11aaf-131">Egy gyűjtemény üzemmódjának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="11aaf-131">How to check which mode a collection is in</span></span>
<span data-ttu-id="11aaf-132">Futtassa a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="11aaf-132">Run the following cmdlet:</span></span>

        Get-AzureRemoteAppCollection <collectionName>

![Ellenőrizze a gyűjtemény használati módját](./media/remoteapp-perapp/araacllelvel.png)

<span data-ttu-id="11aaf-134">Az AclLevel tulajdonság a következő értékeket veheti fel:</span><span class="sxs-lookup"><span data-stu-id="11aaf-134">The AclLevel property can have the following values:</span></span>

* <span data-ttu-id="11aaf-135">Collection: az eredeti közzétételi mód.</span><span class="sxs-lookup"><span data-stu-id="11aaf-135">Collection: the original publishing mode.</span></span> <span data-ttu-id="11aaf-136">Az összes felhasználó láthat minden közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="11aaf-136">All users see all published apps.</span></span>
* <span data-ttu-id="11aaf-137">Alkalmazás: az új közzétételi mód.</span><span class="sxs-lookup"><span data-stu-id="11aaf-137">Application: the new publishing mode.</span></span> <span data-ttu-id="11aaf-138">A felhasználók csak a közvetlenül a számukra közzétett alkalmazásokat látják.</span><span class="sxs-lookup"><span data-stu-id="11aaf-138">Users see only the apps published directly to them.</span></span>

## <a name="how-to-switch-to-application-publishing-mode"></a><span data-ttu-id="11aaf-139">Váltás az alkalmazások közzétételi módja között</span><span class="sxs-lookup"><span data-stu-id="11aaf-139">How to switch to application publishing mode</span></span>
<span data-ttu-id="11aaf-140">Futtassa a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="11aaf-140">Run the following cmdlet:</span></span>

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

<span data-ttu-id="11aaf-141">A rendszer megőrzi az alkalmazások közzétételének állapotát: kezdetben az összes felhasználó látni fog minden eredetileg közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="11aaf-141">Application publishing state will be preserved: initially all users will see all of the original published apps.</span></span>

## <a name="how-to-list-users-who-can-see-a-specific-application"></a><span data-ttu-id="11aaf-142">Azon felhasználók listázása, akik láthatnak egy adott alkalmazást</span><span class="sxs-lookup"><span data-stu-id="11aaf-142">How to list users who can see a specific application</span></span>
<span data-ttu-id="11aaf-143">Futtassa a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="11aaf-143">Run the following cmdlet:</span></span>

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

<span data-ttu-id="11aaf-144">A parancs megjeleníti az összes felhasználót, aki láthatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="11aaf-144">This lists all users who can see the application.</span></span>

<span data-ttu-id="11aaf-145">Megjegyzés: Az alkalmazások aliasait (a fenti szintaxisban „app alias”) a Get-AzureRemoteAppProgram -CollectionName <collectionName> parancsmag futtatásával jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="11aaf-145">Note: You can see the application aliases (called "app alias" in the syntax above) by running Get-AzureRemoteAppProgram -CollectionName <collectionName>.</span></span>

## <a name="how-to-assign-an-application-to-a-user"></a><span data-ttu-id="11aaf-146">Alkalmazás hozzárendelése egy felhasználóhoz</span><span class="sxs-lookup"><span data-stu-id="11aaf-146">How to assign an application to a user</span></span>
<span data-ttu-id="11aaf-147">Futtassa a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="11aaf-147">Run the following cmdlet:</span></span>

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

<span data-ttu-id="11aaf-148">A felhasználó ekkor látni fogja az alkalmazást az Azure RemoteApp-ügyfélen, és csatlakozni tud hozzá.</span><span class="sxs-lookup"><span data-stu-id="11aaf-148">The user will now see the application in the Azure RemoteApp client and will be able to connect to it.</span></span>

## <a name="how-to-remove-an-application-from-a-user"></a><span data-ttu-id="11aaf-149">Alkalmazások eltávolítása egy felhasználótól</span><span class="sxs-lookup"><span data-stu-id="11aaf-149">How to remove an application from a user</span></span>
<span data-ttu-id="11aaf-150">Futtassa a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="11aaf-150">Run the following cmdlet:</span></span>

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a><span data-ttu-id="11aaf-151">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="11aaf-151">Providing feedback</span></span>
<span data-ttu-id="11aaf-152">Nagyra értékeljük visszajelzését és javaslatait az előzetes verzióként elérhető szolgáltatással kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="11aaf-152">We appreciate your feedback and suggestions regarding this preview feature.</span></span> <span data-ttu-id="11aaf-153">Kérjük, töltse ki a [felmérést](http://www.instant.ly/s/FDdrb), és ossza meg velünk a véleményét.</span><span class="sxs-lookup"><span data-stu-id="11aaf-153">Please fill out the [survey](http://www.instant.ly/s/FDdrb) to let us know what you think.</span></span>

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a><span data-ttu-id="11aaf-154">Nem volt lehetősége kipróbálni az előzetes verzióként elérhető szolgáltatást?</span><span class="sxs-lookup"><span data-stu-id="11aaf-154">Haven't had a chance to try the preview feature?</span></span>
<span data-ttu-id="11aaf-155">Ha még nem használta az előzetes verziót, töltse ki ezt a [felmérést](http://www.instant.ly/s/AY83p) a hozzáférés kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="11aaf-155">If you have not participated in the preview yet, please use this [survey](http://www.instant.ly/s/AY83p) to request access.</span></span>

