---
title: "Windows Phone Silverlight SDK frissítési eljárások"
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
ms.openlocfilehash: f87f65788075c7f4067e77946e1bcbc8f3709317
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a><span data-ttu-id="0faec-103">Windows Phone Silverlight SDK frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="0faec-103">Windows Phone Silverlight SDK Upgrade Procedures</span></span>
<span data-ttu-id="0faec-104">Ha Ön már rendelkezik integrált egy régebbi verzióját az SDK az alkalmazásba, hogy az SDK-val történő frissítése során, vegye figyelembe a következő szempontokat.</span><span class="sxs-lookup"><span data-stu-id="0faec-104">If you already have integrated an older version of our SDK into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="0faec-105">Előfordulhat, hogy kell eljárnia számos eljárást, ha nem fogadta az SDK a különböző verzióiban.</span><span class="sxs-lookup"><span data-stu-id="0faec-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="0faec-106">Például ha áttelepít 0.10.1 0.11.0 először hajtsa végre a "from a 0.10.1 0.9.0-s" eljárást kell majd a "from a 0.11.0 0.10.1" eljárást.</span><span class="sxs-lookup"><span data-stu-id="0faec-106">For example if you migrate from 0.10.1 to 0.11.0 you have to first follow the "from 0.9.0 to 0.10.1" procedure then the "from 0.10.1 to 0.11.0" procedure.</span></span>

## <a name="from-200-to-330"></a><span data-ttu-id="0faec-107">A 2.0.0 3.3.0 számára</span><span class="sxs-lookup"><span data-stu-id="0faec-107">From 2.0.0 to 3.3.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="0faec-108">Vizsgálati naplók</span><span class="sxs-lookup"><span data-stu-id="0faec-108">Test logs</span></span>
<span data-ttu-id="0faec-109">Az SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve.</span><span class="sxs-lookup"><span data-stu-id="0faec-109">Console logs produced by the SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="0faec-110">Ez testreszabásához frissítse a tulajdonságát `EngagementAgent.Instance.TestLogEnabled` egy érhetők el az érték a `EngagementTestLogLevel` számbavételi, például:</span><span class="sxs-lookup"><span data-stu-id="0faec-110">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-to-200"></a><span data-ttu-id="0faec-111">A 1.1.1 2.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="0faec-111">From 1.1.1 to 2.0.0</span></span>
<span data-ttu-id="0faec-112">A következő ismerteti, hogyan telepíthetők át az SDK-integráció az alkalmazásba az Azure Mobile Engagement technológiával Capptain SAS által kínált Capptain szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0faec-112">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="0faec-113">Capptain és a Mobile Engagement nem ugyanazok a szolgáltatások és az alábbi eljárás csak emel ki, hogyan telepítheti át az ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0faec-113">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="0faec-114">Az SDK-t az alkalmazás áttelepítése rendszer nem telepíti át az adatok a Capptain kiszolgálókról a Mobile Engagement-kiszolgálókon</span><span class="sxs-lookup"><span data-stu-id="0faec-114">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="0faec-115">Ha egy korábbi verziójáról telepít, tekintse meg a Capptain webhely 1.1.1 először át, akkor alkalmazza az alábbi eljárás</span><span class="sxs-lookup"><span data-stu-id="0faec-115">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.1.1 first then apply the following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="0faec-116">Nuget-csomag</span><span class="sxs-lookup"><span data-stu-id="0faec-116">Nuget package</span></span>
<span data-ttu-id="0faec-117">Cserélje le **Capptain.WindowsPhone** által **MicrosoftAzure.MobileEngagement** Nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="0faec-117">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="0faec-118">Mobilmarketing alkalmazása</span><span class="sxs-lookup"><span data-stu-id="0faec-118">Applying Mobile Engagement</span></span>
<span data-ttu-id="0faec-119">Az SDK-t használ a kifejezés `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="0faec-119">The SDK uses the term `Engagement`.</span></span> <span data-ttu-id="0faec-120">Frissítenie kell a módosítás megfelelő a projekthez.</span><span class="sxs-lookup"><span data-stu-id="0faec-120">You need to update your project to match this change.</span></span>

<span data-ttu-id="0faec-121">El kell távolítania a jelenlegi Capptain nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="0faec-121">You need to uninstall your current Capptain nuget package.</span></span> <span data-ttu-id="0faec-122">Vegye figyelembe, hogy eltávolítja-e Capptain erőforrások mappában összes módosítást.</span><span class="sxs-lookup"><span data-stu-id="0faec-122">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="0faec-123">Ha meg szeretné tartani ezeket a fájlokat készítsen egy másolatot kapnak.</span><span class="sxs-lookup"><span data-stu-id="0faec-123">If you want to keep those files then make a copy of them.</span></span>

<span data-ttu-id="0faec-124">Ezt követően a Microsoft Azure Engagement új nuget-csomag telepítése a projekten.</span><span class="sxs-lookup"><span data-stu-id="0faec-124">After that, install the new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="0faec-125">Megtalálhatja közvetlenül a [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span><span class="sxs-lookup"><span data-stu-id="0faec-125">You can find it directly on [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span></span> <span data-ttu-id="0faec-126">Ez a művelet lecseréli a bevonási által használt összes erőforrások fájlokat, és az új Engagement DLL ad hozzá a projekt hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="0faec-126">This action replaces all resources files used by Engagement and adds the new Engagement DLL to your project References.</span></span>

<span data-ttu-id="0faec-127">A projekt hivatkozásait tisztítása Capptain DLL hivatkozások törlésével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0faec-127">You have to clean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="0faec-128">Ha ez nem teszi meg, Capptain verziója ütközésbe fognak kerülni, és hiba történik.</span><span class="sxs-lookup"><span data-stu-id="0faec-128">If you do not make this, the version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="0faec-129">Ha testre szabott Capptain erőforrásokat, a régi fájlok tartalmát, és illessze be őket az új Engagement fájlok.</span><span class="sxs-lookup"><span data-stu-id="0faec-129">If you have customized Capptain resources, copy your old files content and paste them in the new Engagement files.</span></span> <span data-ttu-id="0faec-130">Vegye figyelembe, hogy az xaml-és cs is frissítenie kell őket.</span><span class="sxs-lookup"><span data-stu-id="0faec-130">Please note that both xaml and cs files have to be updated.</span></span>

<span data-ttu-id="0faec-131">Ha ezek lépéseket csak akkor kell cserélje le az új Engagement hivatkozást Capptain hivatkozásai.</span><span class="sxs-lookup"><span data-stu-id="0faec-131">When those steps are completed you only have to replace old Capptain references by the new Engagement references.</span></span>

1. <span data-ttu-id="0faec-132">Összes Capptain névteret kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="0faec-132">All Capptain namespaces have to be updated.</span></span>
   
    <span data-ttu-id="0faec-133">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="0faec-133">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="0faec-134">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="0faec-134">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="0faec-135">Minden Capptain osztály, amely tartalmazza a "Capptain" tartalmaznia kell "Engagement".</span><span class="sxs-lookup"><span data-stu-id="0faec-135">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="0faec-136">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="0faec-136">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="0faec-137">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="0faec-137">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="0faec-138">Az xaml-fájlokkal Capptain névtér és attribútumok is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0faec-138">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="0faec-139">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="0faec-139">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="0faec-140">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="0faec-140">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="0faec-141">Az egyéb erőforrások például Capptain képek vegye figyelembe, hogy azok is átnevezték használható az "Engagement".</span><span class="sxs-lookup"><span data-stu-id="0faec-141">For the other resources like Capptain pictures, please note that they also have been renamed to use "Engagement".</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="0faec-142">Alkalmazásazonosító / SDK kulcs</span><span class="sxs-lookup"><span data-stu-id="0faec-142">Application ID / SDK Key</span></span>
<span data-ttu-id="0faec-143">Bevonási egy kapcsolati karakterláncot használ.</span><span class="sxs-lookup"><span data-stu-id="0faec-143">Engagement uses a connection string.</span></span> <span data-ttu-id="0faec-144">Nincs a Mobile Engagement egy alkalmazás és az SDK kulcs adható meg, csak akkor adjon meg egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0faec-144">You don't have to specify an application ID and an SDK key with Mobile Engagement, you only have to specify a connection string.</span></span> <span data-ttu-id="0faec-145">Állíthat be azt a EngagementConfiguration fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0faec-145">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="0faec-146">A bevonási konfigurációs állítható be a `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="0faec-146">The Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="0faec-147">Adja meg, hogy a fájl szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="0faec-147">Edit this file to specify:</span></span>

* <span data-ttu-id="0faec-148">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="0faec-148">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="0faec-149">Ha azt szeretné, ehelyett meg futásidőben, hívása előtt az Engagement ügynök inicializálása a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="0faec-149">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="0faec-150">A kapcsolati karakterlánc az alkalmazás a klasszikus Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0faec-150">The connection string for your application is displayed in the Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="0faec-151">Elemek kiszolgálónév-változás</span><span class="sxs-lookup"><span data-stu-id="0faec-151">Items name change</span></span>
<span data-ttu-id="0faec-152">Minden elem nevű *capptain* megnevezett *engagement*.</span><span class="sxs-lookup"><span data-stu-id="0faec-152">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="0faec-153">Ehhez hasonlóan az *Capptain* való *Engagement*.</span><span class="sxs-lookup"><span data-stu-id="0faec-153">Similarly for *Capptain* to *Engagement*.</span></span>

<span data-ttu-id="0faec-154">Példák a gyakran használt Capptain elemeket:</span><span class="sxs-lookup"><span data-stu-id="0faec-154">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="0faec-155">Most nevű EngagementConfiguration CapptainConfiguration</span><span class="sxs-lookup"><span data-stu-id="0faec-155">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="0faec-156">Most nevű EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="0faec-156">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="0faec-157">Most nevű EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="0faec-157">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="0faec-158">Most nevű EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="0faec-158">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="0faec-159">Most nevű GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="0faec-159">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="0faec-160">Vegye figyelembe, hogy az Átnevezés is hatással van a többszörösen definiált metódusok.</span><span class="sxs-lookup"><span data-stu-id="0faec-160">Note that rename also affects overridden methods.</span></span>

