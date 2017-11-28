---
title: "Univerzális speciális jelentéskészítés a MobileApps Engagement aaaWindows"
description: "Hogyan tooIntegrate Azure Mobile Engagement az univerzális Windows-alkalmazások"
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
ms.openlocfilehash: 20968f238ef7ae9dc0b8bb6dac3fb8bdb9bc3a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-hello-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="548ae-103">Az univerzális alkalmazások Engagement SDK a Windows hello speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="548ae-103">Advanced Reporting with hello Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="548ae-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="548ae-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-reporting.md)
> * [<span data-ttu-id="548ae-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="548ae-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="548ae-106">iOS</span><span class="sxs-lookup"><span data-stu-id="548ae-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="548ae-107">Android</span><span class="sxs-lookup"><span data-stu-id="548ae-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="548ae-108">Ez a témakör ismerteti az univerzális Windows-alkalmazás további jelentéskészítési forgatókönyvekhez.</span><span class="sxs-lookup"><span data-stu-id="548ae-108">This topic describes additional reporting scenarios in your Windows Universal application.</span></span> <span data-ttu-id="548ae-109">Ilyen például, beállítások, amelyek segítségével létrehozott hello tooapply toohello app [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="548ae-109">These scenarios include options that you can choose tooapply toohello app created in hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="548ae-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="548ae-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

<span data-ttu-id="548ae-111">Az oktatóanyag elindítása előtt először végezze hello [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) oktatóanyag, amely szándékosan közvetlen és egyszerű.</span><span class="sxs-lookup"><span data-stu-id="548ae-111">Before starting this tutorial, you must first complete hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial, which is deliberately direct and simple.</span></span> <span data-ttu-id="548ae-112">Ez az oktatóanyag ismerteti a további beállítások közül választhat.</span><span class="sxs-lookup"><span data-stu-id="548ae-112">This tutorial covers additional options you can choose from.</span></span>

## <a name="specifying-engagement-configuration-at-runtime"></a><span data-ttu-id="548ae-113">Futásidőben engagement konfiguráció megadása</span><span class="sxs-lookup"><span data-stu-id="548ae-113">Specifying engagement configuration at runtime</span></span>
<span data-ttu-id="548ae-114">hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt, amely ahol hello megadott [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="548ae-114">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project, which is where it was specified in hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) topic.</span></span>

<span data-ttu-id="548ae-115">De azt is megadhatja, hogy futásidőben: a következő metódus hello Engagement ügynök inicializálása előtt hello hívása:</span><span class="sxs-lookup"><span data-stu-id="548ae-115">But you can also specify it at runtime: you can call hello following method before hello Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="548ae-116">Ajánlott módszer: túlterhelés a `Page` osztályok</span><span class="sxs-lookup"><span data-stu-id="548ae-116">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="548ae-117">minden tooactivate hello reporting összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, ellenőrizze a `Page` alosztályokat örökölhet hello `EngagementPage` osztályok.</span><span class="sxs-lookup"><span data-stu-id="548ae-117">tooactivate hello reporting of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, make all your `Page` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="548ae-118">Itt látható egy példa egy lap az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="548ae-118">Here is an example for a page of your application.</span></span> <span data-ttu-id="548ae-119">Mindent hello ugyanaz az alkalmazás összes lapja esetében.</span><span class="sxs-lookup"><span data-stu-id="548ae-119">You can do hello same thing for all pages of your application.</span></span>

### <a name="c-source-file"></a><span data-ttu-id="548ae-120">C# forrásfájl</span><span class="sxs-lookup"><span data-stu-id="548ae-120">C# Source file</span></span>
<span data-ttu-id="548ae-121">A lap módosítása `.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="548ae-121">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="548ae-122">Adja hozzá a tooyour `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="548ae-122">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="548ae-123">Cserélje le `Page` rendelkező `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="548ae-123">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="548ae-124">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="548ae-124">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="548ae-125">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="548ae-125">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="548ae-126">Ha az oldala felülírja hello `OnNavigatedTo` módszer, lehet, hogy toocall `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="548ae-126">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="548ae-127">Ellenkező esetben hello tevékenység nem kell jelenteni (hello `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` metódus).</span><span class="sxs-lookup"><span data-stu-id="548ae-127">Otherwise, hello activity is not be reported (hello `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

### <a name="xaml-file"></a><span data-ttu-id="548ae-128">XAML-fájl</span><span class="sxs-lookup"><span data-stu-id="548ae-128">XAML file</span></span>
<span data-ttu-id="548ae-129">A lap módosítása `.xaml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="548ae-129">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="548ae-130">Tooyour névtér-deklarációk hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="548ae-130">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="548ae-131">Cserélje le `Page` rendelkező `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="548ae-131">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="548ae-132">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="548ae-132">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="548ae-133">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="548ae-133">**With Engagement:**</span></span>

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-hello-default-behaviour"></a><span data-ttu-id="548ae-134">Bírálja felül a hello alapértelmezett viselkedése</span><span class="sxs-lookup"><span data-stu-id="548ae-134">Override hello default behaviour</span></span>
<span data-ttu-id="548ae-135">Alapértelmezés szerint hello osztálynév hello lap hello tevékenység nevét, nem extra akkor számít.</span><span class="sxs-lookup"><span data-stu-id="548ae-135">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="548ae-136">Ha hello osztály hello "Lap" utótagot használ, az Engagement eltávolítja azt.</span><span class="sxs-lookup"><span data-stu-id="548ae-136">If hello class uses hello "Page" suffix, Engagement removes it.</span></span>

<span data-ttu-id="548ae-137">toooverride hello alapértelmezett viselkedése hello nevét, adja hozzá ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="548ae-137">toooverride hello default behavior for hello name, add this code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="548ae-138">tooreport további információt a tevékenységet, adja hozzá ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="548ae-138">tooreport extra information with your activity, add this code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="548ae-139">Ezek a módszerek is meghívhatók hello `OnNavigatedTo` módszer a lap.</span><span class="sxs-lookup"><span data-stu-id="548ae-139">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="548ae-140">Másodlagos módszer: hívja `StartActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="548ae-140">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="548ae-141">Ha nem, vagy nem toooverload a `Page` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="548ae-141">If you cannot or do not want toooverload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="548ae-142">Azt javasoljuk, hogy hívása `StartActivity` belül a `OnNavigatedTo` metódus a lap.</span><span class="sxs-lookup"><span data-stu-id="548ae-142">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="548ae-143">Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="548ae-143">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="548ae-144">Univerzális Windows SDK hello automatikusan meghívja a hello `EndActivity` metódus hello alkalmazás bezárásakor.</span><span class="sxs-lookup"><span data-stu-id="548ae-144">hello Windows Universal SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="548ae-145">Így **magas** toocall hello ajánlott `StartActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl**soha** hívás hello `EndActivity` metódust.</span><span class="sxs-lookup"><span data-stu-id="548ae-145">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method.</span></span> <span data-ttu-id="548ae-146">Ez a módszer értesíti hello Engagement server hello jelenlegi felhasználó elhagyta hello alkalmazás, amely hatással van-e az összes alkalmazás naplóiban.</span><span class="sxs-lookup"><span data-stu-id="548ae-146">This method notifies hello Engagement server that hello current user has left hello application, which will impact all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="548ae-147">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="548ae-147">Advanced reporting</span></span>
<span data-ttu-id="548ae-148">Szükség esetén érdemes lehet tooreport alkalmazásspecifikus eseményeket, hibákat és feladatokat, toodo így, használjon más módszereket található hello hello `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="548ae-148">Optionally, you may want tooreport application-specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="548ae-149">hello Engagement API lehetővé teszi, hogy minden bevonási speciális képességek használatát.</span><span class="sxs-lookup"><span data-stu-id="548ae-149">hello Engagement API allows use of all Engagement's advanced capabilities.</span></span>

<span data-ttu-id="548ae-150">További információkért lásd: [hogyan toouse hello speciális a Mobile Engagement az univerzális Windows-alkalmazás API címkézés](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="548ae-150">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

