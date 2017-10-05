---
title: "Windows univerzális alkalmazások Engagement SDK-integráció"
description: "Az Azure Mobile Engagement integrálása univerzális Windows-alkalmazások"
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
ms.openlocfilehash: 898160814304fa8ec65622056a77ca9d4caf2c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a><span data-ttu-id="4e656-103">Windows univerzális alkalmazások Engagement SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="4e656-103">Windows Universal Apps Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e656-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="4e656-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="4e656-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="4e656-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="4e656-106">iOS</span><span class="sxs-lookup"><span data-stu-id="4e656-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="4e656-107">Android</span><span class="sxs-lookup"><span data-stu-id="4e656-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="4e656-108">Ez az eljárás ismerteti a legegyszerűbben úgy Engagement elemzés és Monitorozási funkciók az univerzális Windows-alkalmazás aktiválása.</span><span class="sxs-lookup"><span data-stu-id="4e656-108">This procedure describes the simplest way to activate Engagement's Analytics and Monitoring functions in your Windows Universal application.</span></span>

<span data-ttu-id="4e656-109">Az alábbi lépéseket kell aktiválni a jelentés az összes statisztikai adatok felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals kiszámításához szükséges naplók elegendő.</span><span class="sxs-lookup"><span data-stu-id="4e656-109">The following steps are enough to activate the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="4e656-110">A jelentés más hasonló eseményeket, hibákat és feladatok statisztika kiszámításához szükséges naplók az Engagement API segítségével manuálisan hajtható végre (lásd: [a speciális a Mobile Engagement az univerzális Windows-alkalmazás szerinti címkézését API használatával](mobile-engagement-windows-store-use-engagement-api.md) óta ezek a statisztikákat a függő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4e656-110">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API (see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="4e656-111">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="4e656-111">Supported versions</span></span>
<span data-ttu-id="4e656-112">A Mobile Engagement SDK Windows Universal alkalmazásokkal való csak integrálhatók a Windows-futtatókörnyezet és célzó univerzális Windows Platform-alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="4e656-112">The Mobile Engagement SDK for Windows Universal Apps can only be integrated into Windows Runtime and Universal Windows Platform applications targeting :</span></span>

* <span data-ttu-id="4e656-113">Windows 8</span><span class="sxs-lookup"><span data-stu-id="4e656-113">Windows 8</span></span>
* <span data-ttu-id="4e656-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="4e656-114">Windows 8.1</span></span>
* <span data-ttu-id="4e656-115">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="4e656-115">Windows Phone 8.1</span></span>
* <span data-ttu-id="4e656-116">Windows 10 (asztali és mobil családok)</span><span class="sxs-lookup"><span data-stu-id="4e656-116">Windows 10 (desktop and mobile families)</span></span>

> [!NOTE]
> <span data-ttu-id="4e656-117">Ha Windows Phone Silverlight céloz meg, akkor tekintse meg a [Windows Phone Silverlight-integráció eljárás](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="4e656-117">If you are targeting Windows Phone Silverlight then refer to the [Windows Phone Silverlight integration procedure](mobile-engagement-windows-phone-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a><span data-ttu-id="4e656-118">A Mobile Engagement univerzális alkalmazások SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="4e656-118">Install the Mobile Engagement Universal Apps SDK</span></span>
### <a name="all-platforms"></a><span data-ttu-id="4e656-119">Összes platform</span><span class="sxs-lookup"><span data-stu-id="4e656-119">All platforms</span></span>
<span data-ttu-id="4e656-120">A Mobile Engagement SDK az univerzális Windows-alkalmazás neve Nuget-csomagként érhető el *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="4e656-120">The Mobile Engagement SDK for Windows Universal App is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="4e656-121">Telepítheti azt a Visual Studio Nuget-Csomagkezelőjét.</span><span class="sxs-lookup"><span data-stu-id="4e656-121">You can install it from the Visual Studio Nuget Package Manager.</span></span>

### <a name="windows-8x-and-windows-phone-81"></a><span data-ttu-id="4e656-122">A Windows 8.x és Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="4e656-122">Windows 8.x and Windows Phone 8.1</span></span>
<span data-ttu-id="4e656-123">NuGet automatikusan telepíti az SDK-erőforrások a `Resources` mappa a projekt gyökerében.</span><span class="sxs-lookup"><span data-stu-id="4e656-123">NuGet automatically deploys the SDK resources in the `Resources` folder at the root of your application project.</span></span>

### <a name="windows-10-universal-windows-platform-applications"></a><span data-ttu-id="4e656-124">Windows 10 univerzális Windows Platform-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="4e656-124">Windows 10 Universal Windows Platform applications</span></span>
<span data-ttu-id="4e656-125">NuGet nem automatikus központi telepítése még az UWP-alkalmazás az SDK-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="4e656-125">NuGet does not automatically deploy the SDK resources in your UWP application yet.</span></span> <span data-ttu-id="4e656-126">Meg kell azt manuálisan mindaddig, amíg az erőforrások telepítése a NuGet bevezetik:</span><span class="sxs-lookup"><span data-stu-id="4e656-126">You have to do it manually until resources deployment is reintroduced in NuGet:</span></span>

1. <span data-ttu-id="4e656-127">Nyissa meg a Fájlkezelőben.</span><span class="sxs-lookup"><span data-stu-id="4e656-127">Open your File Explorer.</span></span>
2. <span data-ttu-id="4e656-128">Nyissa meg azt a következő (**x.x.x** telepíti Engagement verziója): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*</span><span class="sxs-lookup"><span data-stu-id="4e656-128">Navigate to the following location (**x.x.x** is the version of Engagement you are installing): *%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*</span></span>
3. <span data-ttu-id="4e656-129">Húzza a **erőforrások** a Fájlkezelőben a Visual Studio projekt gyökerében mappát.</span><span class="sxs-lookup"><span data-stu-id="4e656-129">Drag and drop the **Resources** folder from the file explorer to the root of your project in Visual Studio.</span></span>
4. <span data-ttu-id="4e656-130">A Visual Studióban válassza ki a projektet, és aktiválja a **megjelenítése minden fájl** a ikon a **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="4e656-130">In Visual Studio select your project and activate the **Show All files** icon on top of the **Solution Explorer**.</span></span>
5. <span data-ttu-id="4e656-131">Egyes fájlok nem szerepelnek a projektet.</span><span class="sxs-lookup"><span data-stu-id="4e656-131">Some files are not included in the project.</span></span> <span data-ttu-id="4e656-132">Importálandó őket egyszerre kattintson a jobb gombbal a **erőforrások** mappa, **zárja ki a projekt** egy másikra a kattintson a jobb gombbal, majd a **erőforrások** mappa, **szerepeljenek projekt** újból felvenni az egész mappa.</span><span class="sxs-lookup"><span data-stu-id="4e656-132">To import them at once right click on the **Resources** folder, **Exclude from project** then another right click on the **Resources** folder, **Include in project** to re-include the whole folder.</span></span> <span data-ttu-id="4e656-133">Az összes fájlt a **erőforrások** mappa mostantól beletartoznak a projekthez.</span><span class="sxs-lookup"><span data-stu-id="4e656-133">All files from the **Resources** folder are now included in your project.</span></span>

## <a name="add-the-capabilities"></a><span data-ttu-id="4e656-134">A képességek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4e656-134">Add the capabilities</span></span>
<span data-ttu-id="4e656-135">Az Engagement SDK-t kell bizonyos funkciók, a Windows SDK a megfelelő működéshez.</span><span class="sxs-lookup"><span data-stu-id="4e656-135">The Engagement SDK needs some capabilities of the Windows SDK in order to work properly.</span></span>

<span data-ttu-id="4e656-136">Nyissa meg a `Package.appxmanifest` fájlt, és ne feledje, hogy a következő lehetőségei vannak deklarálva:</span><span class="sxs-lookup"><span data-stu-id="4e656-136">Open your `Package.appxmanifest` file and be sure that the following capabilities are declared:</span></span>

* `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a><span data-ttu-id="4e656-137">Az Engagement SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="4e656-137">Initialize the Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="4e656-138">Bevonási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4e656-138">Engagement configuration</span></span>
<span data-ttu-id="4e656-139">A bevonási konfigurációs központosított a `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="4e656-139">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="4e656-140">Adja meg, hogy a fájl szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="4e656-140">Edit this file to specify:</span></span>

* <span data-ttu-id="4e656-141">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="4e656-141">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="4e656-142">Ha azt szeretné, ehelyett meg futásidőben, hívása előtt az Engagement ügynök inicializálása a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="4e656-142">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

<span data-ttu-id="4e656-143">A kapcsolati karakterlánc az alkalmazás a klasszikus Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4e656-143">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="4e656-144">Bevonási inicializálása</span><span class="sxs-lookup"><span data-stu-id="4e656-144">Engagement initialization</span></span>
<span data-ttu-id="4e656-145">Amikor létrehoz egy új projektet egy `App.xaml.cs` fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="4e656-145">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="4e656-146">Ez az osztály örökli `Application` és számos fontos metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4e656-146">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="4e656-147">Azt az Engagement SDK inicializálása is használható.</span><span class="sxs-lookup"><span data-stu-id="4e656-147">It will also be used to initialize the Engagement SDK.</span></span>

<span data-ttu-id="4e656-148">Módosítsa a `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="4e656-148">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="4e656-149">Adja hozzá a `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="4e656-149">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="4e656-150">Adja meg az Engagement inicializálására egyszer minden hívásoknál megosztásához metódus:</span><span class="sxs-lookup"><span data-stu-id="4e656-150">Define a method to share the Engagement initialization once for all calls:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* <span data-ttu-id="4e656-151">Hívás `InitEngagement` a a `OnLaunched` módszert:</span><span class="sxs-lookup"><span data-stu-id="4e656-151">Call `InitEngagement` in the `OnLaunched` method:</span></span>
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* <span data-ttu-id="4e656-152">Az alkalmazás indításakor egy egyéni séma, egy másik alkalmazás vagy a parancssor használatával, majd a `OnActivated` metódust.</span><span class="sxs-lookup"><span data-stu-id="4e656-152">When your application is launched using a custom scheme, another application or the command line then the `OnActivated` method is called.</span></span> <span data-ttu-id="4e656-153">Is kell kezdeményezni az Engagement SDK-t, az alkalmazás aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="4e656-153">You also need to initiate the Engagement SDK when your app is activated.</span></span> <span data-ttu-id="4e656-154">Ehhez az szükséges, bírálja felül `OnActivated` módszert:</span><span class="sxs-lookup"><span data-stu-id="4e656-154">To do so, override `OnActivated` method:</span></span>
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> <span data-ttu-id="4e656-155">Kifejezetten nem ajánlott, hogy az alkalmazás egy másik helyen adja hozzá az Engagement inicializálására.</span><span class="sxs-lookup"><span data-stu-id="4e656-155">We strongly discourage you to add the Engagement initialization in another place of your application.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="4e656-156">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="4e656-156">Basic reporting</span></span>
### <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="4e656-157">Ajánlott módszer: túlterhelés a `Page` osztályok</span><span class="sxs-lookup"><span data-stu-id="4e656-157">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="4e656-158">A jelentés minden, a felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a műszaki statisztika számítási Engagement által igényelt naplók aktiválásához egyszerűen végezheti el az összes a `Page` alosztályokat örökli a `EngagementPage` osztályok.</span><span class="sxs-lookup"><span data-stu-id="4e656-158">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `Page` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="4e656-159">Íme egy példa bemutatja, hogyan ehhez az alkalmazás egy lap.</span><span class="sxs-lookup"><span data-stu-id="4e656-159">Here is an example of how to do this for a page of your application.</span></span> <span data-ttu-id="4e656-160">Ezt megteheti az ugyanaz az alkalmazás összes lapja esetében.</span><span class="sxs-lookup"><span data-stu-id="4e656-160">You can do the same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="4e656-161">C# forrásfájl</span><span class="sxs-lookup"><span data-stu-id="4e656-161">C# Source file</span></span>
<span data-ttu-id="4e656-162">A lap módosítása `.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="4e656-162">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="4e656-163">Adja hozzá a `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="4e656-163">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="4e656-164">Cserélje le `Page` rendelkező `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="4e656-164">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="4e656-165">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4e656-165">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="4e656-166">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4e656-166">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="4e656-167">Ha az oldala felülírja az `OnNavigatedTo` metódust, hívja meg a következőt: `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="4e656-167">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="4e656-168">Ellenkező esetben a tevékenységről nem készül jelentés (az `EngagementPage` meghívja a `StartActivity` tevékenységet az `OnNavigatedTo` metódusán belül).</span><span class="sxs-lookup"><span data-stu-id="4e656-168">Otherwise,  the activity will not be reported (the `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="4e656-169">XAML-fájl</span><span class="sxs-lookup"><span data-stu-id="4e656-169">XAML file</span></span>
<span data-ttu-id="4e656-170">A lap módosítása `.xaml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="4e656-170">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="4e656-171">Adja hozzá a következőt a névtér-deklarációkhoz:</span><span class="sxs-lookup"><span data-stu-id="4e656-171">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="4e656-172">Cserélje le `Page` rendelkező `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="4e656-172">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="4e656-173">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4e656-173">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="4e656-174">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4e656-174">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a><span data-ttu-id="4e656-175">Bírálja felül az alapértelmezett viselkedése</span><span class="sxs-lookup"><span data-stu-id="4e656-175">Override the default behaviour</span></span>
<span data-ttu-id="4e656-176">Alapértelmezés szerint az osztály nevét a lap a tevékenység nevét, nem extra akkor számít.</span><span class="sxs-lookup"><span data-stu-id="4e656-176">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="4e656-177">Ha az osztály a "Lap" utótagot használ, Engagement is törlődik.</span><span class="sxs-lookup"><span data-stu-id="4e656-177">If the class uses the "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="4e656-178">Ha azt szeretné, felülbírálhatja az alapértelmezett viselkedést, a neve, vegyük fel ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="4e656-178">If you want to override the default behaviour for the name, simply add this to your code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="4e656-179">Ha szeretne jelentést készíteni a tevékenységi néhány további informations, adhat hozzá ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="4e656-179">If you want to report some extra informations with your activity, you can add this to your code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="4e656-180">Ezek a módszerek nevezzük belül a `OnNavigatedTo` metódus a lap.</span><span class="sxs-lookup"><span data-stu-id="4e656-180">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="4e656-181">Másodlagos módszer: hívja `StartActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="4e656-181">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="4e656-182">Ha nem, vagy nem szeretné, hogy túlterhelés a `Page` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="4e656-182">If you cannot or do not want to overload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="4e656-183">Ajánlott hívására `StartActivity` belül a `OnNavigatedTo` metódus a lap.</span><span class="sxs-lookup"><span data-stu-id="4e656-183">We recommend to call `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="4e656-184">Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="4e656-184">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="4e656-185">A Windows Universal SDK automatikusan meghívja a `EndActivity` módszer az alkalmazás bezárása után.</span><span class="sxs-lookup"><span data-stu-id="4e656-185">The Windows Universal SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="4e656-186">Így **magas** hívására ajánlott a `StartActivity` módszer, amikor a felhasználó tevékenységét módosításához, és **soha** hívja a `EndActivity` metódus, ez a módszer kiszolgálónak küldi az Engagement, amely aktuális felhasználó rendelkezik kilép az alkalmazásból, a rendszer minden alkalmazásnaplók hatással van.</span><span class="sxs-lookup"><span data-stu-id="4e656-186">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method, this method sends to Engagement server that current user has leave the application, this will impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="4e656-187">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="4e656-187">Advanced reporting</span></span>
<span data-ttu-id="4e656-188">A jelentés adott alkalmazásesemények, a hibák és a feladatok, érdemes lehet ehhez használhatja a többi módszerek található a `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="4e656-188">Optionally, you may want to report application specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="4e656-189">A bevonási API lehetővé teszi, hogy Engagement speciális funkciók használatára.</span><span class="sxs-lookup"><span data-stu-id="4e656-189">The Engagement API allows to use all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="4e656-190">További információkért lásd: [a speciális a Mobile Engagement az univerzális Windows-alkalmazás szerinti címkézését API használatával](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="4e656-190">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="4e656-191">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4e656-191">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="4e656-192">Automatikus összeomlási jelentések letiltása</span><span class="sxs-lookup"><span data-stu-id="4e656-192">Disable automatic crash reporting</span></span>
<span data-ttu-id="4e656-193">A jelentéskészítési funkció bekapcsolása automatikus összeomlási letilthatja.</span><span class="sxs-lookup"><span data-stu-id="4e656-193">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="4e656-194">Ezután amikor nem kezelt kivétel lép fel, Engagement nem semmit.</span><span class="sxs-lookup"><span data-stu-id="4e656-194">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="4e656-195">Ha le szeretné letiltani ezt a funkciót, vegye figyelembe, hogy ha egy nem kezelt összeomlási történik, az alkalmazásban, Engagement nem küld a összeomlási **és** nem zárja be a munkamenet és feladatok.</span><span class="sxs-lookup"><span data-stu-id="4e656-195">If you plan to disable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send the crash **AND** will not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="4e656-196">Automatikus összeomlási jelentések letiltásához testreszabása a attól függően, hogy a módját, deklarált, akkor a konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="4e656-196">To disable automatic crash reporting, just customize your configuration depending on the way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="4e656-197">A `EngagementConfiguration.xml` fájl</span><span class="sxs-lookup"><span data-stu-id="4e656-197">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="4e656-198">Jelentés összeomlási beállítása `false` közötti `<reportCrash>` és `</reportCrash>` címkék.</span><span class="sxs-lookup"><span data-stu-id="4e656-198">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="4e656-199">A `EngagementConfiguration` futási időben objektum</span><span class="sxs-lookup"><span data-stu-id="4e656-199">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="4e656-200">A jelentés összeomlási értéke HAMIS, a EngagementConfiguration objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="4e656-200">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="4e656-201">Nagy sebességű átvitel</span><span class="sxs-lookup"><span data-stu-id="4e656-201">Burst mode</span></span>
<span data-ttu-id="4e656-202">Alapértelmezés szerint a bevonási service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="4e656-202">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="4e656-203">Ha az alkalmazás naplók nagyon gyakran, érdemes meghajtóin a naplókat, és jelentést őket egyszerre egy rendszeres időközönként alap (Ez a "kapacitásnövelés mód" nevezzük).</span><span class="sxs-lookup"><span data-stu-id="4e656-203">If your application reports logs very frequently, it is better to buffer the logs and to report them all at once on a regular time base (this is called the “burst mode”).</span></span>

<span data-ttu-id="4e656-204">Ehhez a metódus meghívására:</span><span class="sxs-lookup"><span data-stu-id="4e656-204">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="4e656-205">Az argumentum értéke a **ezredmásodperc**.</span><span class="sxs-lookup"><span data-stu-id="4e656-205">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="4e656-206">Bármikor Ha a valós idejű naplózási újraaktiválni kívánt, csak hívja a módszer minden paraméter nélkül, vagy a 0 érték.</span><span class="sxs-lookup"><span data-stu-id="4e656-206">At any time, if you want to reactivate the real-time logging, just call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="4e656-207">A kapacitásnövelés módja kissé az eszközakkumulátor élettartamának növelhető, de hatással van a bevonási figyelő: minden munkamenetek és feladatok időtartama a rendszer kerekíti a kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="4e656-207">The burst mode slightly increase the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration will be rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="4e656-208">Javasoljuk, hogy egy mint 30000 (30s) kapacitásnövelés küszöbértékkel.</span><span class="sxs-lookup"><span data-stu-id="4e656-208">It is recommended to use a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="4e656-209">Akkor érdemes figyelembe vennie, amelyet a naplók 300 elemet korlátozódnak.</span><span class="sxs-lookup"><span data-stu-id="4e656-209">You have to be aware that saved logs are limited to 300 items.</span></span> <span data-ttu-id="4e656-210">Ha a küldő túl hosszú néhány naplók elveszhet.</span><span class="sxs-lookup"><span data-stu-id="4e656-210">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="4e656-211">A kapacitásnövelés küszöbértéke kisebb, mint 1s időszakra nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="4e656-211">The burst threshold cannot be configured to a period lesser than 1s.</span></span> <span data-ttu-id="4e656-212">Ha erre, az SDK jelennek meg a hiba: a nyomkövetés, és automatikusan visszaállítja az alapértelmezett érték, azaz 0s.</span><span class="sxs-lookup"><span data-stu-id="4e656-212">If you try to do so, the SDK will show a trace with the error and will automatically reset to the default value, i.e., 0s.</span></span> <span data-ttu-id="4e656-213">Ez akkor indul el, az SDK jelentheti a naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="4e656-213">This will trigger the SDK to report the logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

