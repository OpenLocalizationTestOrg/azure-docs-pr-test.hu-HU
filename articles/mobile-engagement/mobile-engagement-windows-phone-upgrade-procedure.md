---
title: "aaaWindows Phone Silverlight SDK frissítési eljárások"
description: "Windows Phone Silverlight SDK frissítési eljárásokat és Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 87130026-9759-4659-9184-788a3627a165
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d72e7b8a59ef2c0a95b22efbf1e5257271399ddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a><span data-ttu-id="a2ef0-103">Windows Phone Silverlight SDK frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="a2ef0-103">Windows Phone Silverlight SDK Upgrade Procedures</span></span>
<span data-ttu-id="a2ef0-104">Ha Ön már rendelkezik integrált egy régebbi verzióját az SDK az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-104">If you already have integrated an older version of our SDK into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="a2ef0-105">Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK a különböző verzióiban.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="a2ef0-106">Például ha telepít át 0.10.1 hello kövesse toofirst rendelkezik too0.11.0 "0.9.0-s a too0.10.1" eljárást akkor hello "0.10.1 a too0.11.0" eljárás.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-106">For example if you migrate from 0.10.1 too0.11.0 you have toofirst follow hello "from 0.9.0 too0.10.1" procedure then hello "from 0.10.1 too0.11.0" procedure.</span></span>

## <a name="from-200-too330"></a><span data-ttu-id="a2ef0-107">A 2.0.0 too3.3.0</span><span class="sxs-lookup"><span data-stu-id="a2ef0-107">From 2.0.0 too3.3.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="a2ef0-108">Vizsgálati naplók</span><span class="sxs-lookup"><span data-stu-id="a2ef0-108">Test logs</span></span>
<span data-ttu-id="a2ef0-109">Hello SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-109">Console logs produced by hello SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="a2ef0-110">toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:</span><span class="sxs-lookup"><span data-stu-id="a2ef0-110">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-too200"></a><span data-ttu-id="a2ef0-111">1.1.1 a too2.0.0</span><span class="sxs-lookup"><span data-stu-id="a2ef0-111">From 1.1.1 too2.0.0</span></span>
<span data-ttu-id="a2ef0-112">hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-112">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="a2ef0-113">Capptain és a Mobile Engagement nem hello azonos szolgáltatások és hello alábbi eljárás csak emel ki, hogyan toomigrate hello ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-113">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="a2ef0-114">Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement kiszolgálókról</span><span class="sxs-lookup"><span data-stu-id="a2ef0-114">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="a2ef0-115">Ha egy korábbi verziójáról telepít, adjon hello Capptain webhely toomigrate too1.1.1 először tekintse át, akkor alkalmazza a következő eljárás hello</span><span class="sxs-lookup"><span data-stu-id="a2ef0-115">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.1.1 first then apply hello following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="a2ef0-116">Nuget-csomag</span><span class="sxs-lookup"><span data-stu-id="a2ef0-116">Nuget package</span></span>
<span data-ttu-id="a2ef0-117">Cserélje le **Capptain.WindowsPhone** által **MicrosoftAzure.MobileEngagement** Nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-117">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="a2ef0-118">Mobilmarketing alkalmazása</span><span class="sxs-lookup"><span data-stu-id="a2ef0-118">Applying Mobile Engagement</span></span>
<span data-ttu-id="a2ef0-119">hello SDK hello kifejezést használja `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-119">hello SDK uses hello term `Engagement`.</span></span> <span data-ttu-id="a2ef0-120">A projekt toomatch kell tooupdate ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-120">You need tooupdate your project toomatch this change.</span></span>

<span data-ttu-id="a2ef0-121">Az aktuális Capptain nuget-csomagot kell toouninstall.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-121">You need toouninstall your current Capptain nuget package.</span></span> <span data-ttu-id="a2ef0-122">Vegye figyelembe, hogy eltávolítja-e Capptain erőforrások mappában összes módosítást.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-122">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="a2ef0-123">Ha azt szeretné, hogy tookeep azokat a fájlokat készítsen egy másolatot kapnak.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-123">If you want tookeep those files then make a copy of them.</span></span>

<span data-ttu-id="a2ef0-124">Ezt követően telepítse a projekt hello új Microsoft Azure Engagement nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-124">After that, install hello new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="a2ef0-125">Megtalálhatja közvetlenül a [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span><span class="sxs-lookup"><span data-stu-id="a2ef0-125">You can find it directly on [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span></span> <span data-ttu-id="a2ef0-126">Ez a művelet cserél összes erőforrás fájlok Engagement használják, és hozzáadja a hello új Engagement DLL tooyour projektre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-126">This action replaces all resources files used by Engagement and adds hello new Engagement DLL tooyour project References.</span></span>

<span data-ttu-id="a2ef0-127">Hogy tooclean a projekt hivatkozásait Capptain DLL hivatkozások törlésével.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-127">You have tooclean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="a2ef0-128">Ha ez nem teszi meg, Capptain hello verziója ütközésbe fognak kerülni, és hiba történik.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-128">If you do not make this, hello version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="a2ef0-129">Ha testreszabott Capptain erőforrások, a régi fájlok tartalmát, és illessze be őket hello új Engagement fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-129">If you have customized Capptain resources, copy your old files content and paste them in hello new Engagement files.</span></span> <span data-ttu-id="a2ef0-130">Vegye figyelembe, hogy xaml-és cs is rendelkezik-e a frissített toobe.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-130">Please note that both xaml and cs files have toobe updated.</span></span>

<span data-ttu-id="a2ef0-131">Ha ezek lépéseket csak akkor kell Capptain hivatkozásai tooreplace hello új Engagement hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-131">When those steps are completed you only have tooreplace old Capptain references by hello new Engagement references.</span></span>

1. <span data-ttu-id="a2ef0-132">Összes Capptain névtér toobe frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-132">All Capptain namespaces have toobe updated.</span></span>
   
    <span data-ttu-id="a2ef0-133">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="a2ef0-133">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="a2ef0-134">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="a2ef0-134">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="a2ef0-135">Minden Capptain osztály, amely tartalmazza a "Capptain" tartalmaznia kell "Engagement".</span><span class="sxs-lookup"><span data-stu-id="a2ef0-135">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="a2ef0-136">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="a2ef0-136">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="a2ef0-137">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="a2ef0-137">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="a2ef0-138">Az xaml-fájlokkal Capptain névtér és attribútumok is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-138">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="a2ef0-139">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="a2ef0-139">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="a2ef0-140">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="a2ef0-140">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="a2ef0-141">Hello a további erőforrások, például a Capptain képek, ne feledje, hogy azok is történt átnevezett toouse "Engagement".</span><span class="sxs-lookup"><span data-stu-id="a2ef0-141">For hello other resources like Capptain pictures, please note that they also have been renamed toouse "Engagement".</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="a2ef0-142">Alkalmazásazonosító / SDK kulcs</span><span class="sxs-lookup"><span data-stu-id="a2ef0-142">Application ID / SDK Key</span></span>
<span data-ttu-id="a2ef0-143">Bevonási egy kapcsolati karakterláncot használ.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-143">Engagement uses a connection string.</span></span> <span data-ttu-id="a2ef0-144">Egy alkalmazás Azonosítót és egy Mobile Engagement SDK kulcs toospecify nincs, így nem toospecify egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-144">You don't have toospecify an application ID and an SDK key with Mobile Engagement, you only have toospecify a connection string.</span></span> <span data-ttu-id="a2ef0-145">Állíthat be azt a EngagementConfiguration fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-145">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="a2ef0-146">hello Engagement konfigurációs állítható be a `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-146">hello Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="a2ef0-147">A fájl toospecify szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="a2ef0-147">Edit this file toospecify:</span></span>

* <span data-ttu-id="a2ef0-148">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-148">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="a2ef0-149">Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:</span><span class="sxs-lookup"><span data-stu-id="a2ef0-149">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="a2ef0-150">hello kapcsolati karakterlánc az alkalmazás hello klasszikus Azure portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-150">hello connection string for your application is displayed in hello Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="a2ef0-151">Elemek kiszolgálónév-változás</span><span class="sxs-lookup"><span data-stu-id="a2ef0-151">Items name change</span></span>
<span data-ttu-id="a2ef0-152">Minden elem nevű *capptain* megnevezett *engagement*.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-152">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="a2ef0-153">Ehhez hasonlóan az *Capptain* túl*Engagement*.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-153">Similarly for *Capptain* too*Engagement*.</span></span>

<span data-ttu-id="a2ef0-154">Példák a gyakran használt Capptain elemeket:</span><span class="sxs-lookup"><span data-stu-id="a2ef0-154">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="a2ef0-155">Most nevű EngagementConfiguration CapptainConfiguration</span><span class="sxs-lookup"><span data-stu-id="a2ef0-155">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="a2ef0-156">Most nevű EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="a2ef0-156">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="a2ef0-157">Most nevű EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="a2ef0-157">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="a2ef0-158">Most nevű EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="a2ef0-158">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="a2ef0-159">Most nevű GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="a2ef0-159">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="a2ef0-160">Vegye figyelembe, hogy az Átnevezés is hatással van a többszörösen definiált metódusok.</span><span class="sxs-lookup"><span data-stu-id="a2ef0-160">Note that rename also affects overridden methods.</span></span>

