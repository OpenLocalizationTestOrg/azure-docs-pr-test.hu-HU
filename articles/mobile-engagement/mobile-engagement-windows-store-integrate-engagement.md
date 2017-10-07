---
title: "aaaWindows univerzális alkalmazások Engagement SDK-integráció"
description: "Hogyan tooIntegrate Azure Mobile Engagement az univerzális Windows-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 18543798099c233dbe55cc387ba0216e369c8596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a><span data-ttu-id="50f12-103">Windows univerzális alkalmazások Engagement SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="50f12-103">Windows Universal Apps Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="50f12-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="50f12-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="50f12-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="50f12-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="50f12-106">iOS</span><span class="sxs-lookup"><span data-stu-id="50f12-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="50f12-107">Android</span><span class="sxs-lookup"><span data-stu-id="50f12-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="50f12-108">Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Engagement elemzés és Monitorozási funkciók az univerzális Windows-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="50f12-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your Windows Universal application.</span></span>

<span data-ttu-id="50f12-109">a lépéseket követve hello a naplók elegendő tooactivate hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok.</span><span class="sxs-lookup"><span data-stu-id="50f12-109">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="50f12-110">hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális a Mobile Engagement az univerzális Windows-alkalmazás API címkézés](mobile-engagement-windows-store-use-engagement-api.md) óta Ezek a statisztikákat a függő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="50f12-110">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="50f12-111">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="50f12-111">Supported versions</span></span>
<span data-ttu-id="50f12-112">Mobile Engagement SDK Windows Universal alkalmazásokkal való hello csak integrálhatók a Windows-futtatókörnyezet és célzó univerzális Windows Platform-alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="50f12-112">hello Mobile Engagement SDK for Windows Universal Apps can only be integrated into Windows Runtime and Universal Windows Platform applications targeting :</span></span>

* <span data-ttu-id="50f12-113">Windows 8</span><span class="sxs-lookup"><span data-stu-id="50f12-113">Windows 8</span></span>
* <span data-ttu-id="50f12-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="50f12-114">Windows 8.1</span></span>
* <span data-ttu-id="50f12-115">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="50f12-115">Windows Phone 8.1</span></span>
* <span data-ttu-id="50f12-116">Windows 10 (asztali és mobil családok)</span><span class="sxs-lookup"><span data-stu-id="50f12-116">Windows 10 (desktop and mobile families)</span></span>

> [!NOTE]
> <span data-ttu-id="50f12-117">Ha a Windows Phone Silverlight céloz meg, majd tekintse meg a toohello [Windows Phone Silverlight-integráció eljárás](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="50f12-117">If you are targeting Windows Phone Silverlight then refer toohello [Windows Phone Silverlight integration procedure](mobile-engagement-windows-phone-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-hello-mobile-engagement-universal-apps-sdk"></a><span data-ttu-id="50f12-118">Hello Mobile Engagement univerzális alkalmazások SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="50f12-118">Install hello Mobile Engagement Universal Apps SDK</span></span>
### <a name="all-platforms"></a><span data-ttu-id="50f12-119">Összes platform</span><span class="sxs-lookup"><span data-stu-id="50f12-119">All platforms</span></span>
<span data-ttu-id="50f12-120">Mobile Engagement SDK az univerzális Windows-alkalmazás hello nevű Nuget-csomagként érhető el *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="50f12-120">hello Mobile Engagement SDK for Windows Universal App is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="50f12-121">A Visual Studio Nuget-Csomagkezelő hello telepítheti.</span><span class="sxs-lookup"><span data-stu-id="50f12-121">You can install it from hello Visual Studio Nuget Package Manager.</span></span>

### <a name="windows-8x-and-windows-phone-81"></a><span data-ttu-id="50f12-122">A Windows 8.x és Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="50f12-122">Windows 8.x and Windows Phone 8.1</span></span>
<span data-ttu-id="50f12-123">NuGet automatikusan telepíti a hello SDK erőforrások hello `Resources` mappát a hello a projekt gyökerében.</span><span class="sxs-lookup"><span data-stu-id="50f12-123">NuGet automatically deploys hello SDK resources in hello `Resources` folder at hello root of your application project.</span></span>

### <a name="windows-10-universal-windows-platform-applications"></a><span data-ttu-id="50f12-124">Windows 10 univerzális Windows Platform-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="50f12-124">Windows 10 Universal Windows Platform applications</span></span>
<span data-ttu-id="50f12-125">NuGet nem automatikus központi telepítése még az UWP-alkalmazás hello SDK-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="50f12-125">NuGet does not automatically deploy hello SDK resources in your UWP application yet.</span></span> <span data-ttu-id="50f12-126">Az erőforrások telepítése amíg manuálisan újra bevezették NuGet toodo közül választhat:</span><span class="sxs-lookup"><span data-stu-id="50f12-126">You have toodo it manually until resources deployment is reintroduced in NuGet:</span></span>

1. <span data-ttu-id="50f12-127">Nyissa meg a Fájlkezelőben.</span><span class="sxs-lookup"><span data-stu-id="50f12-127">Open your File Explorer.</span></span>
2. <span data-ttu-id="50f12-128">Keresse meg a következő helyen toohello (**x.x.x** telepíti Engagement hello verziója): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*</span><span class="sxs-lookup"><span data-stu-id="50f12-128">Navigate toohello following location (**x.x.x** is hello version of Engagement you are installing): *%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*</span></span>
3. <span data-ttu-id="50f12-129">A fogd és vidd hello **erőforrások** hello fájl explorer toohello a projekt gyökerében a Visual Studio mappát.</span><span class="sxs-lookup"><span data-stu-id="50f12-129">Drag and drop hello **Resources** folder from hello file explorer toohello root of your project in Visual Studio.</span></span>
4. <span data-ttu-id="50f12-130">A Visual Studio válassza ki a projektet, és aktiválja a hello **megjelenítése minden fájl** ikonja felett hello **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="50f12-130">In Visual Studio select your project and activate hello **Show All files** icon on top of hello **Solution Explorer**.</span></span>
5. <span data-ttu-id="50f12-131">Egyes fájlok nem szerepelnek a hello projekt.</span><span class="sxs-lookup"><span data-stu-id="50f12-131">Some files are not included in hello project.</span></span> <span data-ttu-id="50f12-132">őket egyszerre kattintson jobb gombbal a hello tooimport **erőforrások** mappa, **zárja ki a projekt** majd egy másik kattintson jobb gombbal a hello **erőforrások** mappa, **belefoglalása a projekt** toore-hello egész mappa tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="50f12-132">tooimport them at once right click on hello **Resources** folder, **Exclude from project** then another right click on hello **Resources** folder, **Include in project** toore-include hello whole folder.</span></span> <span data-ttu-id="50f12-133">Minden fájl a hello **erőforrások** mappa mostantól beletartoznak a projekthez.</span><span class="sxs-lookup"><span data-stu-id="50f12-133">All files from hello **Resources** folder are now included in your project.</span></span>

## <a name="add-hello-capabilities"></a><span data-ttu-id="50f12-134">Hello képességek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="50f12-134">Add hello capabilities</span></span>
<span data-ttu-id="50f12-135">hello Engagement SDK megfelelően kell néhány hello Windows SDK-t a rendelés toowork képességeit.</span><span class="sxs-lookup"><span data-stu-id="50f12-135">hello Engagement SDK needs some capabilities of hello Windows SDK in order toowork properly.</span></span>

<span data-ttu-id="50f12-136">Nyissa meg a `Package.appxmanifest` fájlt, és ne feledje, hogy a következő képességeket hello deklarált:</span><span class="sxs-lookup"><span data-stu-id="50f12-136">Open your `Package.appxmanifest` file and be sure that hello following capabilities are declared:</span></span>

* `Internet (Client)`

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="50f12-137">Hello Engagement SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="50f12-137">Initialize hello Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="50f12-138">Bevonási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="50f12-138">Engagement configuration</span></span>
<span data-ttu-id="50f12-139">hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="50f12-139">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="50f12-140">A fájl toospecify szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="50f12-140">Edit this file toospecify:</span></span>

* <span data-ttu-id="50f12-141">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="50f12-141">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="50f12-142">Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:</span><span class="sxs-lookup"><span data-stu-id="50f12-142">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

<span data-ttu-id="50f12-143">hello kapcsolati karakterlánc az alkalmazás megjelenik a klasszikus Azure portál hello.</span><span class="sxs-lookup"><span data-stu-id="50f12-143">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="50f12-144">Bevonási inicializálása</span><span class="sxs-lookup"><span data-stu-id="50f12-144">Engagement initialization</span></span>
<span data-ttu-id="50f12-145">Amikor létrehoz egy új projektet egy `App.xaml.cs` fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="50f12-145">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="50f12-146">Ez az osztály örökli `Application` és számos fontos metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="50f12-146">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="50f12-147">Azt is használt tooinitialize hello Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="50f12-147">It will also be used tooinitialize hello Engagement SDK.</span></span>

<span data-ttu-id="50f12-148">Módosítsa a hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="50f12-148">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="50f12-149">Adja hozzá a tooyour `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="50f12-149">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="50f12-150">Adja meg a metódus tooshare hello Engagement inicializálására egyszer minden hívásoknál:</span><span class="sxs-lookup"><span data-stu-id="50f12-150">Define a method tooshare hello Engagement initialization once for all calls:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* <span data-ttu-id="50f12-151">Hívás `InitEngagement` a hello `OnLaunched` módszert:</span><span class="sxs-lookup"><span data-stu-id="50f12-151">Call `InitEngagement` in hello `OnLaunched` method:</span></span>
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* <span data-ttu-id="50f12-152">Az alkalmazás indításakor egyéni megjelenítve, egy másik alkalmazás vagy hello parancssori majd hello `OnActivated` metódust.</span><span class="sxs-lookup"><span data-stu-id="50f12-152">When your application is launched using a custom scheme, another application or hello command line then hello `OnActivated` method is called.</span></span> <span data-ttu-id="50f12-153">Az alkalmazás aktiválásakor is kell tooinitiate hello Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="50f12-153">You also need tooinitiate hello Engagement SDK when your app is activated.</span></span> <span data-ttu-id="50f12-154">tehát felülbírálása toodo `OnActivated` módszert:</span><span class="sxs-lookup"><span data-stu-id="50f12-154">toodo so, override `OnActivated` method:</span></span>
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> <span data-ttu-id="50f12-155">Kifejezetten nem ajánlott, tooadd hello Engagement inicializálására az alkalmazás egy másik helyen.</span><span class="sxs-lookup"><span data-stu-id="50f12-155">We strongly discourage you tooadd hello Engagement initialization in another place of your application.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="50f12-156">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="50f12-156">Basic reporting</span></span>
### <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="50f12-157">Ajánlott módszer: túlterhelés a `Page` osztályok</span><span class="sxs-lookup"><span data-stu-id="50f12-157">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="50f12-158">Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, egyszerűen végezheti el az összes a `Page` alosztályokat örökölhet hello `EngagementPage` osztályok.</span><span class="sxs-lookup"><span data-stu-id="50f12-158">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `Page` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="50f12-159">Íme egy példa bemutatja, hogyan toodo Ez az alkalmazás egy lap.</span><span class="sxs-lookup"><span data-stu-id="50f12-159">Here is an example of how toodo this for a page of your application.</span></span> <span data-ttu-id="50f12-160">Mindent hello ugyanaz az alkalmazás összes lapja esetében.</span><span class="sxs-lookup"><span data-stu-id="50f12-160">You can do hello same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="50f12-161">C# forrásfájl</span><span class="sxs-lookup"><span data-stu-id="50f12-161">C# Source file</span></span>
<span data-ttu-id="50f12-162">A lap módosítása `.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="50f12-162">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="50f12-163">Adja hozzá a tooyour `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="50f12-163">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="50f12-164">Cserélje le `Page` rendelkező `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="50f12-164">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="50f12-165">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="50f12-165">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="50f12-166">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="50f12-166">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="50f12-167">Ha az oldala felülírja hello `OnNavigatedTo` módszer, lehet, hogy toocall `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="50f12-167">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="50f12-168">Ellenkező esetben hello tevékenység nem készíthető jelentés (hello `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` metódus).</span><span class="sxs-lookup"><span data-stu-id="50f12-168">Otherwise,  hello activity will not be reported (hello `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="50f12-169">XAML-fájl</span><span class="sxs-lookup"><span data-stu-id="50f12-169">XAML file</span></span>
<span data-ttu-id="50f12-170">A lap módosítása `.xaml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="50f12-170">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="50f12-171">Tooyour névtér-deklarációk hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="50f12-171">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="50f12-172">Cserélje le `Page` rendelkező `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="50f12-172">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="50f12-173">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="50f12-173">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="50f12-174">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="50f12-174">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-hello-default-behaviour"></a><span data-ttu-id="50f12-175">Bírálja felül a hello alapértelmezett viselkedése</span><span class="sxs-lookup"><span data-stu-id="50f12-175">Override hello default behaviour</span></span>
<span data-ttu-id="50f12-176">Alapértelmezés szerint hello osztálynév hello lap hello tevékenység nevét, nem extra akkor számít.</span><span class="sxs-lookup"><span data-stu-id="50f12-176">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="50f12-177">Ha hello osztály hello "Lap" utótagot használ, Engagement is törlődik.</span><span class="sxs-lookup"><span data-stu-id="50f12-177">If hello class uses hello "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="50f12-178">Ha toooverride hello alapértelmezett viselkedése hello neveként, egyszerűen adja hozzá a tooyour kódot:</span><span class="sxs-lookup"><span data-stu-id="50f12-178">If you want toooverride hello default behaviour for hello name, simply add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="50f12-179">Ha azt szeretné tooreport néhány további informations a tevékenységet, a tooyour kódot is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="50f12-179">If you want tooreport some extra informations with your activity, you can add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="50f12-180">Ezek a módszerek is meghívhatók hello `OnNavigatedTo` módszer a lap.</span><span class="sxs-lookup"><span data-stu-id="50f12-180">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="50f12-181">Másodlagos módszer: hívja `StartActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="50f12-181">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="50f12-182">Ha nem, vagy nem toooverload a `Page` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="50f12-182">If you cannot or do not want toooverload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="50f12-183">Azt javasoljuk, hogy toocall `StartActivity` belül a `OnNavigatedTo` metódus a lap.</span><span class="sxs-lookup"><span data-stu-id="50f12-183">We recommend toocall `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="50f12-184">Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="50f12-184">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="50f12-185">Univerzális Windows SDK hello automatikusan meghívja a hello `EndActivity` metódus hello alkalmazás bezárásakor.</span><span class="sxs-lookup"><span data-stu-id="50f12-185">hello Windows Universal SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="50f12-186">Így **magas** toocall hello ajánlott `StartActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl**soha** hívás hello `EndActivity` módszer, ez a módszer küld tooEngagement kiszolgáló, hogy az aktuális felhasználó rendelkezik-e hagyja hello alkalmazás, a rendszer minden alkalmazásnaplók hatással van.</span><span class="sxs-lookup"><span data-stu-id="50f12-186">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method, this method sends tooEngagement server that current user has leave hello application, this will impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="50f12-187">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="50f12-187">Advanced reporting</span></span>
<span data-ttu-id="50f12-188">Szükség esetén érdemes lehet tooreport adott eseményeket, hibákat és feladatokat, toodo így, használjon más módszereket található hello hello `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="50f12-188">Optionally, you may want tooreport application specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="50f12-189">hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók.</span><span class="sxs-lookup"><span data-stu-id="50f12-189">hello Engagement API allows toouse all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="50f12-190">További információkért lásd: [hogyan toouse hello speciális a Mobile Engagement az univerzális Windows-alkalmazás API címkézés](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="50f12-190">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="50f12-191">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="50f12-191">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="50f12-192">Automatikus összeomlási jelentések letiltása</span><span class="sxs-lookup"><span data-stu-id="50f12-192">Disable automatic crash reporting</span></span>
<span data-ttu-id="50f12-193">Bármikor letilthatja hello automatikus összeomlási jelentéskészítési funkció bekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="50f12-193">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="50f12-194">Ezután amikor nem kezelt kivétel lép fel, Engagement nem semmit.</span><span class="sxs-lookup"><span data-stu-id="50f12-194">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="50f12-195">Ha azt tervezi, toodisable ezt a szolgáltatást, vegye figyelembe, hogy ha egy nem kezelt összeomlási történik, az alkalmazásban, Engagement nem küld hello összeomlási **és** hello munkamenet és feladatok nem zárható be.</span><span class="sxs-lookup"><span data-stu-id="50f12-195">If you plan toodisable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send hello crash **AND** will not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="50f12-196">toodisable automatikus összeomlási reporting, attól függően, hogy hello módja, deklarált, konfiguráció csak testreszabása:</span><span class="sxs-lookup"><span data-stu-id="50f12-196">toodisable automatic crash reporting, just customize your configuration depending on hello way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="50f12-197">A `EngagementConfiguration.xml` fájl</span><span class="sxs-lookup"><span data-stu-id="50f12-197">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="50f12-198">Állítsa be a jelentés túl összeomlik miattuk`false` közötti `<reportCrash>` és `</reportCrash>` címkék.</span><span class="sxs-lookup"><span data-stu-id="50f12-198">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="50f12-199">A `EngagementConfiguration` futási időben objektum</span><span class="sxs-lookup"><span data-stu-id="50f12-199">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="50f12-200">Állítsa be a jelentés összeomlási toofalse a EngagementConfiguration objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="50f12-200">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="50f12-201">Nagy sebességű átvitel</span><span class="sxs-lookup"><span data-stu-id="50f12-201">Burst mode</span></span>
<span data-ttu-id="50f12-202">Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="50f12-202">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="50f12-203">Ha az alkalmazás naplók nagyon gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (Ez a lehetőség hello "kapacitásnövelés mód").</span><span class="sxs-lookup"><span data-stu-id="50f12-203">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello “burst mode”).</span></span>

<span data-ttu-id="50f12-204">toodo tehát hello metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="50f12-204">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="50f12-205">hello argumentum értéke a **ezredmásodperc**.</span><span class="sxs-lookup"><span data-stu-id="50f12-205">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="50f12-206">Bármikor Ha azt szeretné, hogy tooreactivate hello valós idejű naplózási, csak hívja hello metódus bármely paraméter nélkül vagy hello 0 értékkel.</span><span class="sxs-lookup"><span data-stu-id="50f12-206">At any time, if you want tooreactivate hello real-time logging, just call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="50f12-207">hello kapacitásnövelés mód némileg növeli hello akkumulátor élettartama hello Engagement figyelő azonban hatással van: az összes munkamenetek és feladatok is lesznek kerekített toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="50f12-207">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="50f12-208">Az ajánlott toouse egy mint 30000 (30s) kapacitásnövelés küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="50f12-208">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="50f12-209">Vegye figyelembe a mentett naplókat még korlátozott too300 elemek toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="50f12-209">You have toobe aware that saved logs are limited too300 items.</span></span> <span data-ttu-id="50f12-210">Ha a küldő túl hosszú néhány naplók elveszhet.</span><span class="sxs-lookup"><span data-stu-id="50f12-210">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="50f12-211">hello kapacitásnövelés küszöbértéke kisebb, mint 1s tooa időszak nem lehet konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="50f12-211">hello burst threshold cannot be configured tooa period lesser than 1s.</span></span> <span data-ttu-id="50f12-212">Ha úgy próbálja toodo, hello SDK hello hiba: a nyomkövetési bemutatják és automatikusan toohello alapértelmezett értékét, azaz 0s alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="50f12-212">If you try toodo so, hello SDK will show a trace with hello error and will automatically reset toohello default value, i.e., 0s.</span></span> <span data-ttu-id="50f12-213">Ez akkor indul el, hello SDK tooreport hello naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="50f12-213">This will trigger hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

