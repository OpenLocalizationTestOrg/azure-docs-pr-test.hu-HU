---
title: "Windows Phone Silverlight Engagement SDK-integráció"
description: "Windows Phone Silverlight-alkalmazásokhoz az Azure Mobile Engagement integrálása"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 29b18aecff783cebf617995e2a19f16f0b68b51b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a><span data-ttu-id="1c9cc-103">Windows Phone Silverlight Engagement SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="1c9cc-103">Windows Phone Silverlight Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1c9cc-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="1c9cc-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="1c9cc-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="1c9cc-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="1c9cc-106">iOS</span><span class="sxs-lookup"><span data-stu-id="1c9cc-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="1c9cc-107">Android</span><span class="sxs-lookup"><span data-stu-id="1c9cc-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="1c9cc-108">Ez az eljárás ismerteti az Azure Mobile Engagement elemzés és Monitorozási funkciók a Windows Phone Silverlight alkalmazás aktiválása legegyszerűbb módja.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-108">This procedure describes the simplest way to activate Azure Mobile Engagement's Analytics and Monitoring functions in your Windows Phone Silverlight application.</span></span>

<span data-ttu-id="1c9cc-109">Az alábbi lépéseket kell aktiválni a jelentés az összes statisztikai adatok felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals kiszámításához szükséges naplók elegendő.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-109">The following steps are enough to activate the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="1c9cc-110">A jelentés más hasonló eseményeket, hibákat és feladatok statisztika kiszámításához szükséges naplók az Engagement API segítségével manuálisan hajtható végre (lásd: [használata a speciális API szerinti címkézését a Windows Phone Silverlight-alkalmazást a Mobile Engagement](mobile-engagement-windows-phone-use-engagement-api.md) alatt) mivel ezek a statisztikákat a függő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-110">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API (see [How to use the advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md) below) since these statistics are application-dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="1c9cc-111">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="1c9cc-111">Supported versions</span></span>
<span data-ttu-id="1c9cc-112">A Mobile Engagement SDK a Windows Silverlight csak integrálhatók célcsoport-kezelési alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-112">The Mobile Engagement SDK for Windows Silverlight can only be integrated into applications targeting :</span></span>

* <span data-ttu-id="1c9cc-113">Windows Phone 8.0</span><span class="sxs-lookup"><span data-stu-id="1c9cc-113">Windows Phone 8.0</span></span>
* <span data-ttu-id="1c9cc-114">Windows Phone 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="1c9cc-114">Windows Phone 8.1 Silverlight</span></span>

> [!NOTE]
> <span data-ttu-id="1c9cc-115">Ha a Windows Phone 8.1 (nem Silverlight) céloz meg, majd tekintse meg a [univerzális Windows-integrációs eljárás](mobile-engagement-windows-store-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="1c9cc-115">If you are targeting Windows Phone 8.1 (non-Silverlight) then refer to the [Windows Universal integration procedure](mobile-engagement-windows-store-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-the-mobile-engagement-silverlight-sdk"></a><span data-ttu-id="1c9cc-116">A Mobile Engagement Silverlight SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="1c9cc-116">Install the Mobile Engagement Silverlight SDK</span></span>
<span data-ttu-id="1c9cc-117">A Mobile Engagement SDK a Windows Silverlight nevű Nuget-csomagként érhető el *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-117">The Mobile Engagement SDK for Windows Silverlight is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="1c9cc-118">Telepítheti azt a Visual Studio Nuget-Csomagkezelőjét.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-118">You can install it from the Visual Studio Nuget Package Manager.</span></span> 

## <a name="add-the-capabilities"></a><span data-ttu-id="1c9cc-119">A képességek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1c9cc-119">Add the capabilities</span></span>
<span data-ttu-id="1c9cc-120">Az Engagement SDK-t kell bizonyos funkciók, a Windows Phone Silverlight SDK a megfelelő működéshez.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-120">The Engagement SDK needs some capabilities of the Windows Phone Silverlight SDK in order to work properly.</span></span>

<span data-ttu-id="1c9cc-121">Nyissa meg a `WMAppManifest.xml` fájlt, és ne feledje, hogy az alábbi képességeket deklarálva van a `Capabilities` panel:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-121">Open your `WMAppManifest.xml` file and be sure that the following capabilities are declared in the `Capabilities` panel:</span></span>

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-the-engagement-sdk"></a><span data-ttu-id="1c9cc-122">Az Engagement SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="1c9cc-122">Initialize the Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="1c9cc-123">Bevonási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="1c9cc-123">Engagement configuration</span></span>
<span data-ttu-id="1c9cc-124">A bevonási konfigurációs központosított a `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-124">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="1c9cc-125">Adja meg, hogy a fájl szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-125">Edit this file to specify :</span></span>

* <span data-ttu-id="1c9cc-126">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-126">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="1c9cc-127">Ha azt szeretné, ehelyett meg futásidőben, hívása előtt az Engagement ügynök inicializálása a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-127">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="1c9cc-128">A kapcsolati karakterlánc az alkalmazás a klasszikus Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-128">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="1c9cc-129">Bevonási inicializálása</span><span class="sxs-lookup"><span data-stu-id="1c9cc-129">Engagement initialization</span></span>
<span data-ttu-id="1c9cc-130">Amikor létrehoz egy új projektet egy `App.xaml.cs` fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-130">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="1c9cc-131">Ez az osztály örökli `Application` és számos fontos metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-131">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="1c9cc-132">Azt az Engagement SDK inicializálása is használható.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-132">It will also be used to initialize the Engagement SDK.</span></span>

<span data-ttu-id="1c9cc-133">Módosítsa a `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-133">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="1c9cc-134">Adja hozzá a `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-134">Add to your `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="1c9cc-135">Helyezze be `EngagementAgent.Instance.Init` a a `Application_Launching` módszert:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-135">Insert `EngagementAgent.Instance.Init` in the `Application_Launching` method :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* <span data-ttu-id="1c9cc-136">Helyezze be `EngagementAgent.Instance.OnActivated` a a `Application_Activated` módszert:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-136">Insert `EngagementAgent.Instance.OnActivated` in the `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> <span data-ttu-id="1c9cc-137">Kifejezetten nem ajánlott, hogy az alkalmazás egy másik helyen adja hozzá az Engagement inicializálására.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-137">We strongly discourage you to add the Engagement initialization in another place of your application.</span></span> <span data-ttu-id="1c9cc-138">Azonban figyelembe, hogy a `EngagementAgent.Instance.Init` metódus egy dedikált szálon, nem pedig a felhasználói felület szálán futtatja.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-138">However, be aware that the `EngagementAgent.Instance.Init` method runs on a dedicated thread, and not on the UI thread.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="1c9cc-139">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="1c9cc-139">Basic reporting</span></span>
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a><span data-ttu-id="1c9cc-140">Ajánlott módszer: túlterhelés a `PhoneApplicationPage` osztályok</span><span class="sxs-lookup"><span data-stu-id="1c9cc-140">Recommended method : overload your `PhoneApplicationPage` classes</span></span>
<span data-ttu-id="1c9cc-141">A jelentés minden, a felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a műszaki statisztika számítási Engagement által igényelt naplók aktiválásához egyszerűen végezheti el az összes a `PhoneApplicationPage` alosztályokat örökli a `EngagementPage` osztályok.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-141">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `PhoneApplicationPage` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="1c9cc-142">Íme egy példa bemutatja, hogyan ehhez az alkalmazás egy lap.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-142">Here is an example of how to do this for a page of your application.</span></span> <span data-ttu-id="1c9cc-143">Ezt megteheti az ugyanaz az alkalmazás összes lapja esetében.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-143">You can do the same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="1c9cc-144">C# forrásfájl</span><span class="sxs-lookup"><span data-stu-id="1c9cc-144">C# Source file</span></span>
<span data-ttu-id="1c9cc-145">A lap módosítása `.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-145">Modify your page `.xaml.cs` file :</span></span>

* <span data-ttu-id="1c9cc-146">Adja hozzá a `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-146">Add to your `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="1c9cc-147">Cserélje le `PhoneApplicationPage` rendelkező `EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="1c9cc-147">Replace `PhoneApplicationPage` with `EngagementPage` :</span></span>

<span data-ttu-id="1c9cc-148">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1c9cc-148">**Without Engagement:**</span></span>

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

<span data-ttu-id="1c9cc-149">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1c9cc-149">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> <span data-ttu-id="1c9cc-150">Ha az oldala örökli a `OnNavigatedTo` módszer, ügyeljen arra, hogy lehetővé teszik a `base.OnNavigatedTo(e)` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-150">If your page inherits from the `OnNavigatedTo` method, be careful to let the `base.OnNavigatedTo(e)` call.</span></span> <span data-ttu-id="1c9cc-151">Ellenkező esetben a tevékenység nem fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-151">Otherwise, the activity will not be reported.</span></span> <span data-ttu-id="1c9cc-152">Valójában a `EngagementPage` hívja a következő `StartActivity` belül a `OnNavigatedTo` metódust.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-152">Indeed, the `EngagementPage` is calling `StartActivity` inside the `OnNavigatedTo` method.</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="1c9cc-153">XAML-fájl</span><span class="sxs-lookup"><span data-stu-id="1c9cc-153">XAML file</span></span>
<span data-ttu-id="1c9cc-154">A lap módosítása `.xaml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-154">Modify your page `.xaml` file :</span></span>

* <span data-ttu-id="1c9cc-155">A névtér-deklarációk hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-155">Add to your namespaces declarations :</span></span>
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* <span data-ttu-id="1c9cc-156">Cserélje le `phone:PhoneApplicationPage` rendelkező `engagement:EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="1c9cc-156">Replace `phone:PhoneApplicationPage` with `engagement:EngagementPage` :</span></span>

<span data-ttu-id="1c9cc-157">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1c9cc-157">**Without Engagement:**</span></span>

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

<span data-ttu-id="1c9cc-158">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1c9cc-158">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a><span data-ttu-id="1c9cc-159">Az alapértelmezett viselkedés felülbírálásához</span><span class="sxs-lookup"><span data-stu-id="1c9cc-159">Override the default behavior</span></span>
<span data-ttu-id="1c9cc-160">Alapértelmezés szerint az osztály nevét a lap a tevékenység nevét, nem extra akkor számít.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-160">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="1c9cc-161">Ha az osztály a "Lap" utótagot használ, Engagement is törlődik.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-161">If the class uses the "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="1c9cc-162">Ha azt szeretné, felülbírálhatja az alapértelmezett viselkedést, a neve, vegyük fel ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-162">If you want to override the default behavior for the name, simply add this to your code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

<span data-ttu-id="1c9cc-163">Ha szeretne jelentést készíteni néhány további információt a tevékenységet, adhat hozzá ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-163">If you want to report some extra information with your activity, you can add this to your code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

<span data-ttu-id="1c9cc-164">Ezek a módszerek nevezzük belül a `OnNavigatedTo` metódus a lap.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-164">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="1c9cc-165">Másodlagos módszer: hívja `StartActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="1c9cc-165">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="1c9cc-166">Ha nem, vagy nem szeretné, hogy túlterhelés a `PhoneApplicationPage` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-166">If you cannot or do not want to overload your `PhoneApplicationPage` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="1c9cc-167">Azt javasoljuk, hogy hívása `StartActivity` belül a `OnNavigatedTo` a PhoneApplicationPage metódusában.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-167">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your PhoneApplicationPage.</span></span>

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> <span data-ttu-id="1c9cc-168">Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-168">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="1c9cc-169">Az SDK automatikusan meghívja a `EndActivity` módszer az alkalmazás bezárása után.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-169">The SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="1c9cc-170">Így **magas** hívására ajánlott a `StartActivity` módszer, amikor a felhasználó tevékenységét módosításához, és **soha** hívja a `EndActivity` metódus.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-170">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method.</span></span> <span data-ttu-id="1c9cc-171">Ez a módszer egy üzenetet küld a Engagement kiszolgálónak, hogy az aktuális felhasználó elhagyta az alkalmazást, és ez hatással van az összes alkalmazás naplóiban.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-171">This method sends a message to the Engagement server that the current user has left the application and this impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="1c9cc-172">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="1c9cc-172">Advanced reporting</span></span>
<span data-ttu-id="1c9cc-173">A jelentés adott alkalmazásesemények, a hibák és a feladatok, érdemes lehet ehhez használhatja a többi módszerek található a `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-173">Optionally, you may want to report application specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="1c9cc-174">A bevonási API lehetővé teszi, hogy Engagement speciális funkciók használatára.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-174">The Engagement API allows to use all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="1c9cc-175">További információkért lásd: [használata a speciális API szerinti címkézését a Windows Phone Silverlight-alkalmazást a Mobile Engagement](mobile-engagement-windows-phone-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="1c9cc-175">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="1c9cc-176">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="1c9cc-176">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="1c9cc-177">Automatikus összeomlási jelentések letiltása</span><span class="sxs-lookup"><span data-stu-id="1c9cc-177">Disable automatic crash reporting</span></span>
<span data-ttu-id="1c9cc-178">A jelentéskészítési funkció bekapcsolása automatikus összeomlási letilthatja.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-178">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="1c9cc-179">Ezután amikor nem kezelt kivétel lép fel, Engagement nem semmit.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-179">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="1c9cc-180">Ha le szeretné letiltani ezt a funkciót, vegye figyelembe, hogy ha egy nem kezelt összeomlási történik, az alkalmazásban, Engagement nem küld a összeomlási **és** azt a munkamenetet, és a feladatok nem zárható be.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-180">If you plan to disable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send the crash **AND** it will not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="1c9cc-181">Automatikus összeomlási jelentések letiltásához testreszabása a attól függően, hogy a módját, deklarált, akkor a konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-181">To disable automatic crash reporting, just customize your configuration depending on the way you declared it :</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="1c9cc-182">A `EngagementConfiguration.xml` fájl</span><span class="sxs-lookup"><span data-stu-id="1c9cc-182">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="1c9cc-183">Jelentés összeomlási beállítása `false` közötti `<reportCrash>` és `</reportCrash>` címkék.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-183">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="1c9cc-184">A `EngagementConfiguration` futási időben objektum</span><span class="sxs-lookup"><span data-stu-id="1c9cc-184">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="1c9cc-185">A jelentés összeomlási értéke HAMIS, a EngagementConfiguration objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-185">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="1c9cc-186">Nagy sebességű átvitel</span><span class="sxs-lookup"><span data-stu-id="1c9cc-186">Burst mode</span></span>
<span data-ttu-id="1c9cc-187">Alapértelmezés szerint a bevonási service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-187">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="1c9cc-188">Ha az alkalmazás naplók nagyon gyakran, érdemes meghajtóin a naplókat, és jelentést őket egyszerre egy rendszeres időközönként alap (Ez a "kapacitásnövelés mód" nevezzük).</span><span class="sxs-lookup"><span data-stu-id="1c9cc-188">If your application reports logs very frequently, it is better to buffer the logs and to report them all at once on a regular time base (this is called the “burst mode”).</span></span>

<span data-ttu-id="1c9cc-189">Ehhez a metódus meghívására:</span><span class="sxs-lookup"><span data-stu-id="1c9cc-189">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="1c9cc-190">Az argumentum értéke a **ezredmásodperc**.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-190">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="1c9cc-191">Bármikor Ha a valós idejű naplózási újraaktiválni kívánt, csak hívja a módszer minden paraméter nélkül, vagy a 0 érték.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-191">At any time, if you want to reactivate the real-time logging, just call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="1c9cc-192">A kapacitásnövelés módja kissé az eszközakkumulátor élettartamának növelhető, de hatással van a bevonási figyelő: minden munkamenetek és feladatok időtartama a rendszer kerekíti a kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="1c9cc-192">The burst mode slightly increase the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration will be rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="1c9cc-193">Javasoljuk, hogy egy mint 30000 (30s) kapacitásnövelés küszöbértékkel.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-193">It is recommended to use a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="1c9cc-194">Akkor érdemes figyelembe vennie, amelyet a naplók 300 elemet korlátozódnak.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-194">You have to be aware that saved logs are limited to 300 items.</span></span> <span data-ttu-id="1c9cc-195">Ha a küldő túl hosszú néhány naplók elveszhet.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-195">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="1c9cc-196">A kapacitásnövelés küszöbértéket nem állítható be egy időszak kisebb mint egy második.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-196">The burst threshold cannot be configured to a period lesser than one second.</span></span> <span data-ttu-id="1c9cc-197">Ha ehhez az SDK jelennek meg a hiba, és automatikusan visszaáll az alapértelmezett érték lesz a nyomkövetési Ez azt jelenti, hogy nulla másodperc.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-197">If you try to do so, the SDK will show a trace with the error and will automatically reset to the default value, that is, zero seconds.</span></span> <span data-ttu-id="1c9cc-198">Ez akkor indul el, az SDK jelentheti a naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="1c9cc-198">This will trigger the SDK to report the logs in real-time.</span></span>
> 
> 

