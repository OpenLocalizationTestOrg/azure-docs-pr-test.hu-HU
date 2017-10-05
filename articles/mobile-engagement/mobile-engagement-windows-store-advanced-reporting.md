---
title: "Jelentéskészítés a MobileApps Engagement speciális Windows Universal"
description: "Az Azure Mobile Engagement integrálása univerzális Windows-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ea5030bf-73ac-49b7-bc3e-c25fc10e945a
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: feac309db1ffce0945012e293bfc1df417aed876
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="3e466-103">Jelentéskészítés a a Windows univerzális alkalmazások Engagement SDK speciális</span><span class="sxs-lookup"><span data-stu-id="3e466-103">Advanced Reporting with the Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3e466-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="3e466-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-reporting.md)
> * [<span data-ttu-id="3e466-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="3e466-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="3e466-106">iOS</span><span class="sxs-lookup"><span data-stu-id="3e466-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="3e466-107">Android</span><span class="sxs-lookup"><span data-stu-id="3e466-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="3e466-108">Ez a témakör ismerteti az univerzális Windows-alkalmazás további jelentéskészítési forgatókönyvekhez.</span><span class="sxs-lookup"><span data-stu-id="3e466-108">This topic describes additional reporting scenarios in your Windows Universal application.</span></span> <span data-ttu-id="3e466-109">Ilyen például a létrehozott alkalmazás alkalmazandó választható lehetőségek az [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3e466-109">These scenarios include options that you can choose to apply to the app created in the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e466-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3e466-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

<span data-ttu-id="3e466-111">Az oktatóanyag elindítása előtt először végezze el a [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) oktatóanyag, amely szándékosan közvetlen és egyszerű.</span><span class="sxs-lookup"><span data-stu-id="3e466-111">Before starting this tutorial, you must first complete the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial, which is deliberately direct and simple.</span></span> <span data-ttu-id="3e466-112">Ez az oktatóanyag ismerteti a további beállítások közül választhat.</span><span class="sxs-lookup"><span data-stu-id="3e466-112">This tutorial covers additional options you can choose from.</span></span>

## <a name="specifying-engagement-configuration-at-runtime"></a><span data-ttu-id="3e466-113">Futásidőben engagement konfiguráció megadása</span><span class="sxs-lookup"><span data-stu-id="3e466-113">Specifying engagement configuration at runtime</span></span>
<span data-ttu-id="3e466-114">A bevonási konfigurációs központosított a `Resources\EngagementConfiguration.xml` fájlt a projekt, amely ahol lett megadva a a [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="3e466-114">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project, which is where it was specified in the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) topic.</span></span>

<span data-ttu-id="3e466-115">De azt is megadhatja, hogy futásidőben: hívása előtt az Engagement ügynök inicializálása a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="3e466-115">But you can also specify it at runtime: you can call the following method before the Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="3e466-116">Ajánlott módszer: túlterhelés a `Page` osztályok</span><span class="sxs-lookup"><span data-stu-id="3e466-116">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="3e466-117">A felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a műszaki statisztika számítási Engagement által igényelt összes napló jelentéskészítési aktiválásához végezze el az összes a `Page` alosztályokat örökli a `EngagementPage` osztályok.</span><span class="sxs-lookup"><span data-stu-id="3e466-117">To activate the reporting of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, make all your `Page` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="3e466-118">Itt látható egy példa egy lap az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3e466-118">Here is an example for a page of your application.</span></span> <span data-ttu-id="3e466-119">Ezt megteheti az ugyanaz az alkalmazás összes lapja esetében.</span><span class="sxs-lookup"><span data-stu-id="3e466-119">You can do the same thing for all pages of your application.</span></span>

### <a name="c-source-file"></a><span data-ttu-id="3e466-120">C# forrásfájl</span><span class="sxs-lookup"><span data-stu-id="3e466-120">C# Source file</span></span>
<span data-ttu-id="3e466-121">A lap módosítása `.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="3e466-121">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="3e466-122">Adja hozzá a `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="3e466-122">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="3e466-123">Cserélje le `Page` rendelkező `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="3e466-123">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="3e466-124">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="3e466-124">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="3e466-125">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="3e466-125">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="3e466-126">Ha az oldala felülírja az `OnNavigatedTo` metódust, hívja meg a következőt: `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="3e466-126">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="3e466-127">Ellenkező esetben a tevékenység nem kell jelenteni (a `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` metódus).</span><span class="sxs-lookup"><span data-stu-id="3e466-127">Otherwise, the activity is not be reported (the `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

### <a name="xaml-file"></a><span data-ttu-id="3e466-128">XAML-fájl</span><span class="sxs-lookup"><span data-stu-id="3e466-128">XAML file</span></span>
<span data-ttu-id="3e466-129">A lap módosítása `.xaml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="3e466-129">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="3e466-130">Adja hozzá a következőt a névtér-deklarációkhoz:</span><span class="sxs-lookup"><span data-stu-id="3e466-130">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="3e466-131">Cserélje le `Page` rendelkező `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="3e466-131">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="3e466-132">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="3e466-132">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="3e466-133">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="3e466-133">**With Engagement:**</span></span>

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-the-default-behaviour"></a><span data-ttu-id="3e466-134">Bírálja felül az alapértelmezett viselkedése</span><span class="sxs-lookup"><span data-stu-id="3e466-134">Override the default behaviour</span></span>
<span data-ttu-id="3e466-135">Alapértelmezés szerint az osztály nevét a lap a tevékenység nevét, nem extra akkor számít.</span><span class="sxs-lookup"><span data-stu-id="3e466-135">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="3e466-136">Ha az osztály a "Lap" utótagot használ, az Engagement eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="3e466-136">If the class uses the "Page" suffix, Engagement removes it.</span></span>

<span data-ttu-id="3e466-137">Felülbírálhatja az alapértelmezett viselkedést, a neve, vegye fel ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="3e466-137">To override the default behavior for the name, add this code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="3e466-138">További információ a tevékenységi jelentést, adja hozzá a kódot:</span><span class="sxs-lookup"><span data-stu-id="3e466-138">To report extra information with your activity, add this code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="3e466-139">Ezek a módszerek nevezzük belül a `OnNavigatedTo` metódus a lap.</span><span class="sxs-lookup"><span data-stu-id="3e466-139">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="3e466-140">Másodlagos módszer: hívja `StartActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="3e466-140">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="3e466-141">Ha nem, vagy nem szeretné, hogy túlterhelés a `Page` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="3e466-141">If you cannot or do not want to overload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="3e466-142">Azt javasoljuk, hogy hívása `StartActivity` belül a `OnNavigatedTo` metódus a lap.</span><span class="sxs-lookup"><span data-stu-id="3e466-142">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="3e466-143">Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e466-143">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="3e466-144">A Windows Universal SDK automatikusan meghívja a `EndActivity` módszer az alkalmazás bezárása után.</span><span class="sxs-lookup"><span data-stu-id="3e466-144">The Windows Universal SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="3e466-145">Így **magas** hívására ajánlott a `StartActivity` módszer, amikor a felhasználó tevékenységét módosításához, és **soha** hívja a `EndActivity` metódus.</span><span class="sxs-lookup"><span data-stu-id="3e466-145">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method.</span></span> <span data-ttu-id="3e466-146">Ez a módszer értesíti az Engagement-kiszolgálót, hogy az aktuális felhasználó elhagyta az alkalmazásról, így hatással van-e az összes alkalmazás naplóiban.</span><span class="sxs-lookup"><span data-stu-id="3e466-146">This method notifies the Engagement server that the current user has left the application, which will impact all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="3e466-147">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="3e466-147">Advanced reporting</span></span>
<span data-ttu-id="3e466-148">Szükség esetén előfordulhat, hogy szeretne jelentést készíteni, az alkalmazás-specifikus eseményeket, hibákat és feladatok, ehhez használja a többi metódusok találhatók a `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="3e466-148">Optionally, you may want to report application-specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="3e466-149">A bevonási API lehetővé teszi, hogy minden bevonási speciális képességek használatát.</span><span class="sxs-lookup"><span data-stu-id="3e466-149">The Engagement API allows use of all Engagement's advanced capabilities.</span></span>

<span data-ttu-id="3e466-150">További információkért lásd: [a speciális a Mobile Engagement az univerzális Windows-alkalmazás szerinti címkézését API használatával](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="3e466-150">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

