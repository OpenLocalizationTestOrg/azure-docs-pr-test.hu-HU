---
title: "aaaPublish alkalmazások tooindividual felhasználók egy Azure RemoteApp-gyűjteményben (előzetes verzió) |} Microsoft Docs"
description: "Ismerje meg, hogyan közzéteheti alkalmazások tooindividual felhasználók, ahelyett, hogy attól függően, hogy csoportok, az Azure Remoteappban."
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
ms.openlocfilehash: 87b435552ddbfc2c6d03aeb01d95a44985e71f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-tooindividual-users-in-an-azure-remoteapp-collection-preview"></a><span data-ttu-id="f14c5-103">Alkalmazások tooindividual felhasználók közzététele egy Azure RemoteApp-gyűjteményben (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="f14c5-103">Publish applications tooindividual users in an Azure RemoteApp collection (Preview)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f14c5-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="f14c5-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f14c5-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f14c5-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="f14c5-106">Ez a cikk azt ismerteti, hogyan toopublish alkalmazások tooindividual felhasználók egy Azure RemoteApp-gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="f14c5-106">This article explains how toopublish applications tooindividual users in an Azure RemoteApp collection.</span></span> <span data-ttu-id="f14c5-107">Ez az új funkció az Azure Remoteappban, jelenleg magán előnézetben és rendelkezésre álló csak tooselect korai támogatókat tesztelési célra.</span><span class="sxs-lookup"><span data-stu-id="f14c5-107">This is new functionality in Azure RemoteApp, currently in private preview and available only tooselect early adopters for evaluation purposes.</span></span>

<span data-ttu-id="f14c5-108">Eredetileg a Azure remoteappban csak egy módja volt az alkalmazás-közzététel: hello rendszergazda hello lemezképből alkalmazásokat tehetett közzé, és azok lenne látható tooall felhasználók hello gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="f14c5-108">Originally Azure RemoteApp enabled only one way of publishing applications: hello administrator would publish apps from hello image and they would be visible tooall users in hello collection.</span></span>

<span data-ttu-id="f14c5-109">Egy általános forgatókönyv számos alkalmazás egyetlen rendszerképet, a rendelés tooreduce felügyeleti költségek egy gyűjtemény üzembe helyezése tooinclude.</span><span class="sxs-lookup"><span data-stu-id="f14c5-109">A common scenario is tooinclude many applications in a single image and deploy one collection in order tooreduce management costs.</span></span> <span data-ttu-id="f14c5-110">Gyakran nem minden alkalmazás olyan megfelelő tooall felhasználók – a rendszergazdák inkább toopublish alkalmazások tooindividual felhasználókat, hogy ne lássák a szükségtelen alkalmazásokat az alkalmazásfolyamukban.</span><span class="sxs-lookup"><span data-stu-id="f14c5-110">Oftentimes not all applications are relevant tooall users – administrators would prefer toopublish apps tooindividual users so they don’t see unnecessary applications in their application feed.</span></span>

<span data-ttu-id="f14c5-111">Ez mostantól lehetséges az Azure RemoteAppban – jelenleg korlátozott előzetes verzióként.</span><span class="sxs-lookup"><span data-stu-id="f14c5-111">This is now possible in Azure RemoteApp – currently as a limited preview feature.</span></span> <span data-ttu-id="f14c5-112">Íme hello új funkció rövid összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="f14c5-112">Here is a brief summary of hello new functionality:</span></span>

1. <span data-ttu-id="f14c5-113">Egy gyűjtemény két módba állítható be:</span><span class="sxs-lookup"><span data-stu-id="f14c5-113">A collection can be set into one of two modes:</span></span>
   
   * <span data-ttu-id="f14c5-114">hello eredeti gyűjtemény módja, ha egy gyűjtemény minden felhasználója láthatja az összes közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f14c5-114">hello original collection mode, where all users in a collection can see all published applications.</span></span> <span data-ttu-id="f14c5-115">Ez az alapértelmezett mód hello.</span><span class="sxs-lookup"><span data-stu-id="f14c5-115">This is hello default mode.</span></span>
   * <span data-ttu-id="f14c5-116">hello új alkalmazás módja, ha csak látnak explicit módon hozzárendelt toothem alkalmazások</span><span class="sxs-lookup"><span data-stu-id="f14c5-116">hello new application mode, where users only see applications that have been explicitly assigned toothem</span></span>
2. <span data-ttu-id="f14c5-117">Hello pillanatban hello alkalmazás mód csak akkor engedélyezhető, Azure RemoteApp PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="f14c5-117">At hello moment hello application mode can only be enabled using Azure RemoteApp PowerShell cmdlets.</span></span>
   
   * <span data-ttu-id="f14c5-118">Ha tooapplication üzemmód beállítása, hello gyűjtemény felhasználó-hozzárendelése nem kezelhető hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="f14c5-118">When set tooapplication mode, user assignment in hello collection cannot be managed through hello Azure portal.</span></span> <span data-ttu-id="f14c5-119">Felhasználó-hozzárendelés PowerShell-parancsmagok segítségével kezelt toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f14c5-119">User assignment has toobe managed through PowerShell cmdlets.</span></span>
3. <span data-ttu-id="f14c5-120">A felhasználók csak látni fogják hello alkalmazások közzétett közvetlenül toothem.</span><span class="sxs-lookup"><span data-stu-id="f14c5-120">Users will only see hello applications published directly toothem.</span></span> <span data-ttu-id="f14c5-121">Azonban továbbra is lehet az egy felhasználói toolaunch hello kép lévő többi alkalmazást hello közvetlenül hello operációs rendszerben férnek hozzájuk.</span><span class="sxs-lookup"><span data-stu-id="f14c5-121">However, it may still be possible for a user toolaunch hello other applications available on hello image by accessing them directly in hello operating system.</span></span>
   
   * <span data-ttu-id="f14c5-122">Ez a szolgáltatás nem biztosít az alkalmazások; biztonságos zárolását Ez csak a láthatóságot korlátozza az alkalmazásfolyamban hello.</span><span class="sxs-lookup"><span data-stu-id="f14c5-122">This feature does not provide a secure lockdown of applications; it only limits visibility in hello application feed.</span></span>
   * <span data-ttu-id="f14c5-123">Ha tooisolate felhasználókat egyes alkalmazásoktól, szüksége lesz toouse különálló gyűjteményeket, amelyek.</span><span class="sxs-lookup"><span data-stu-id="f14c5-123">If you need tooisolate users from applications, you will need toouse separate collections for that.</span></span>

## <a name="how-tooget-azure-remoteapp-powershell-cmdlets"></a><span data-ttu-id="f14c5-124">Hogyan tooget Azure RemoteApp PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="f14c5-124">How tooget Azure RemoteApp PowerShell cmdlets</span></span>
<span data-ttu-id="f14c5-125">tootry hello előzetes verzió új funkcióinak, szüksége lesz a toouse Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="f14c5-125">tootry hello new preview functionality, you will need toouse Azure PowerShell cmdlets.</span></span> <span data-ttu-id="f14c5-126">Jelenleg nem lehetséges toouse hello Azure felügyeleti portál tooenable hello új alkalmazás-közzétételi mód.</span><span class="sxs-lookup"><span data-stu-id="f14c5-126">It is currently not possible toouse hello Azure Management portal tooenable hello new application publishing mode.</span></span>

<span data-ttu-id="f14c5-127">Először ellenőrizze, hogy rendelkezik-e hello [Azure PowerShell modul](/powershell/azure/overview) telepítve.</span><span class="sxs-lookup"><span data-stu-id="f14c5-127">First, make sure you have hello [Azure PowerShell module](/powershell/azure/overview) installed.</span></span>

<span data-ttu-id="f14c5-128">Majd indítsa el a felügyeleti és a következő parancsmag futtatási hello hello PowerShell-konzolon:</span><span class="sxs-lookup"><span data-stu-id="f14c5-128">Then launch hello PowerShell console in administrator mode and run hello following cmdlet:</span></span>

        Add-AzureAccount

<span data-ttu-id="f14c5-129">A rendszer felkéri az Azure felhasználói nevének jelszavának megadására.</span><span class="sxs-lookup"><span data-stu-id="f14c5-129">It will prompt you for your Azure user name and password.</span></span> <span data-ttu-id="f14c5-130">Miután bejelentkezett, szemben az Azure-előfizetések fogja tudni toorun Azure RemoteApp-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="f14c5-130">Once signed in, you will be able toorun Azure RemoteApp cmdlets against your Azure subscriptions.</span></span>

## <a name="how-toocheck-which-mode-a-collection-is-in"></a><span data-ttu-id="f14c5-131">Hogyan a egy gyűjtemény üzemmódjának toocheck</span><span class="sxs-lookup"><span data-stu-id="f14c5-131">How toocheck which mode a collection is in</span></span>
<span data-ttu-id="f14c5-132">Futtassa a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="f14c5-132">Run hello following cmdlet:</span></span>

        Get-AzureRemoteAppCollection <collectionName>

![Hello fizetési mód ellenőrzése](./media/remoteapp-perapp/araacllelvel.png)

<span data-ttu-id="f14c5-134">hello AclLevel tulajdonság hello a következő értékeket veheti fel:</span><span class="sxs-lookup"><span data-stu-id="f14c5-134">hello AclLevel property can have hello following values:</span></span>

* <span data-ttu-id="f14c5-135">Gyűjtemény: hello eredeti közzétételi mód.</span><span class="sxs-lookup"><span data-stu-id="f14c5-135">Collection: hello original publishing mode.</span></span> <span data-ttu-id="f14c5-136">Az összes felhasználó láthat minden közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f14c5-136">All users see all published apps.</span></span>
* <span data-ttu-id="f14c5-137">Alkalmazás: hello új közzétételi mód.</span><span class="sxs-lookup"><span data-stu-id="f14c5-137">Application: hello new publishing mode.</span></span> <span data-ttu-id="f14c5-138">Csak a hello alkalmazásokat közzétenni közvetlenül toothem a felhasználók láthatják.</span><span class="sxs-lookup"><span data-stu-id="f14c5-138">Users see only hello apps published directly toothem.</span></span>

## <a name="how-tooswitch-tooapplication-publishing-mode"></a><span data-ttu-id="f14c5-139">Hogyan tooswitch tooapplication közzétételi mód</span><span class="sxs-lookup"><span data-stu-id="f14c5-139">How tooswitch tooapplication publishing mode</span></span>
<span data-ttu-id="f14c5-140">Futtassa a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="f14c5-140">Run hello following cmdlet:</span></span>

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

<span data-ttu-id="f14c5-141">A rendszer megőrzi az alkalmazások közzétételének állapotát: kezdetben az összes felhasználó látni fog minden eredetileg közzétett alkalmazást hello.</span><span class="sxs-lookup"><span data-stu-id="f14c5-141">Application publishing state will be preserved: initially all users will see all of hello original published apps.</span></span>

## <a name="how-toolist-users-who-can-see-a-specific-application"></a><span data-ttu-id="f14c5-142">Hogyan toolist felhasználók, akik láthatnak egy adott alkalmazást</span><span class="sxs-lookup"><span data-stu-id="f14c5-142">How toolist users who can see a specific application</span></span>
<span data-ttu-id="f14c5-143">Futtassa a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="f14c5-143">Run hello following cmdlet:</span></span>

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

<span data-ttu-id="f14c5-144">A parancs megjeleníti az összes felhasználó számára látható hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f14c5-144">This lists all users who can see hello application.</span></span>

<span data-ttu-id="f14c5-145">Megjegyzés: A látható hello alkalmazások aliasait (hello fenti szintaxisban "app alias" elnevezésű) futtassa a Get-AzureRemoteAppProgram - CollectionName <collectionName>.</span><span class="sxs-lookup"><span data-stu-id="f14c5-145">Note: You can see hello application aliases (called "app alias" in hello syntax above) by running Get-AzureRemoteAppProgram -CollectionName <collectionName>.</span></span>

## <a name="how-tooassign-an-application-tooa-user"></a><span data-ttu-id="f14c5-146">Hogyan tooassign tooa Alkalmazásfelhasználó</span><span class="sxs-lookup"><span data-stu-id="f14c5-146">How tooassign an application tooa user</span></span>
<span data-ttu-id="f14c5-147">Futtassa a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="f14c5-147">Run hello following cmdlet:</span></span>

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

<span data-ttu-id="f14c5-148">hello felhasználói jelenik meg hello alkalmazás hello Azure RemoteApp-ügyfelet, és képes tooconnect tooit lesz.</span><span class="sxs-lookup"><span data-stu-id="f14c5-148">hello user will now see hello application in hello Azure RemoteApp client and will be able tooconnect tooit.</span></span>

## <a name="how-tooremove-an-application-from-a-user"></a><span data-ttu-id="f14c5-149">Hogyan tooremove az alkalmazás a felhasználó</span><span class="sxs-lookup"><span data-stu-id="f14c5-149">How tooremove an application from a user</span></span>
<span data-ttu-id="f14c5-150">Futtassa a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="f14c5-150">Run hello following cmdlet:</span></span>

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a><span data-ttu-id="f14c5-151">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="f14c5-151">Providing feedback</span></span>
<span data-ttu-id="f14c5-152">Nagyra értékeljük visszajelzését és javaslatait az előzetes verzióként elérhető szolgáltatással kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f14c5-152">We appreciate your feedback and suggestions regarding this preview feature.</span></span> <span data-ttu-id="f14c5-153">Töltse ki az hello [felmérés](http://www.instant.ly/s/FDdrb) arról, mit gondol toolet.</span><span class="sxs-lookup"><span data-stu-id="f14c5-153">Please fill out hello [survey](http://www.instant.ly/s/FDdrb) toolet us know what you think.</span></span>

## <a name="havent-had-a-chance-tootry-hello-preview-feature"></a><span data-ttu-id="f14c5-154">Nem volt alkalommal tootry hello előzetes verziójú funkciók?</span><span class="sxs-lookup"><span data-stu-id="f14c5-154">Haven't had a chance tootry hello preview feature?</span></span>
<span data-ttu-id="f14c5-155">Ha nem használta a hello preview még, akkor használja ezt [felmérés](http://www.instant.ly/s/AY83p) toorequest hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="f14c5-155">If you have not participated in hello preview yet, please use this [survey](http://www.instant.ly/s/AY83p) toorequest access.</span></span>

