---
title: "aaaWindows Phone Silverlight Engagement SDK-integráció"
description: "Hogyan tooIntegrate Azure Mobile Engagement Windows Phone Silverlight-alkalmazásokhoz"
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
ms.openlocfilehash: f65683a62e5256cea469a3a73d99ade4331cb6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a><span data-ttu-id="52f32-103">Windows Phone Silverlight Engagement SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="52f32-103">Windows Phone Silverlight Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="52f32-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="52f32-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="52f32-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="52f32-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="52f32-106">iOS</span><span class="sxs-lookup"><span data-stu-id="52f32-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="52f32-107">Android</span><span class="sxs-lookup"><span data-stu-id="52f32-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="52f32-108">Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Azure Mobile Engagement elemzés és Monitorozási funkciók a Windows Phone Silverlight alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="52f32-108">This procedure describes hello simplest way tooactivate Azure Mobile Engagement's Analytics and Monitoring functions in your Windows Phone Silverlight application.</span></span>

<span data-ttu-id="52f32-109">a lépéseket követve hello a naplók elegendő tooactivate hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok.</span><span class="sxs-lookup"><span data-stu-id="52f32-109">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="52f32-110">hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális API szerinti címkézését a Windows Phone Silverlight-alkalmazást a Mobile Engagement](mobile-engagement-windows-phone-use-engagement-api.md) az alábbi) mert a statisztikák függő alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="52f32-110">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md) below) since these statistics are application-dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="52f32-111">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="52f32-111">Supported versions</span></span>
<span data-ttu-id="52f32-112">Mobile Engagement SDK a Windows Silverlight hello csak integrálhatók célcsoport-kezelési alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="52f32-112">hello Mobile Engagement SDK for Windows Silverlight can only be integrated into applications targeting :</span></span>

* <span data-ttu-id="52f32-113">Windows Phone 8.0</span><span class="sxs-lookup"><span data-stu-id="52f32-113">Windows Phone 8.0</span></span>
* <span data-ttu-id="52f32-114">Windows Phone 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="52f32-114">Windows Phone 8.1 Silverlight</span></span>

> [!NOTE]
> <span data-ttu-id="52f32-115">Ha a Windows Phone 8.1 (nem Silverlight) céloz meg, majd tekintse meg a toohello [univerzális Windows-integrációs eljárás](mobile-engagement-windows-store-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="52f32-115">If you are targeting Windows Phone 8.1 (non-Silverlight) then refer toohello [Windows Universal integration procedure](mobile-engagement-windows-store-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-hello-mobile-engagement-silverlight-sdk"></a><span data-ttu-id="52f32-116">Hello Mobile Engagement Silverlight SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="52f32-116">Install hello Mobile Engagement Silverlight SDK</span></span>
<span data-ttu-id="52f32-117">Mobile Engagement SDK a Windows Silverlight hello nevű Nuget-csomagként érhető el *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="52f32-117">hello Mobile Engagement SDK for Windows Silverlight is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="52f32-118">A Visual Studio Nuget-Csomagkezelő hello telepítheti.</span><span class="sxs-lookup"><span data-stu-id="52f32-118">You can install it from hello Visual Studio Nuget Package Manager.</span></span> 

## <a name="add-hello-capabilities"></a><span data-ttu-id="52f32-119">Hello képességek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="52f32-119">Add hello capabilities</span></span>
<span data-ttu-id="52f32-120">hello Engagement SDK megfelelően kell néhány rendelés toowork a Windows Phone Silverlight SDK hello képességeit.</span><span class="sxs-lookup"><span data-stu-id="52f32-120">hello Engagement SDK needs some capabilities of hello Windows Phone Silverlight SDK in order toowork properly.</span></span>

<span data-ttu-id="52f32-121">Nyissa meg a `WMAppManifest.xml` fájlt, és ne feledje, hogy a következő képességeket hello táblákon hello `Capabilities` panel:</span><span class="sxs-lookup"><span data-stu-id="52f32-121">Open your `WMAppManifest.xml` file and be sure that hello following capabilities are declared in hello `Capabilities` panel:</span></span>

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="52f32-122">Hello Engagement SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="52f32-122">Initialize hello Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="52f32-123">Bevonási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="52f32-123">Engagement configuration</span></span>
<span data-ttu-id="52f32-124">hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="52f32-124">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="52f32-125">A fájl toospecify szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="52f32-125">Edit this file toospecify :</span></span>

* <span data-ttu-id="52f32-126">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="52f32-126">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="52f32-127">Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:</span><span class="sxs-lookup"><span data-stu-id="52f32-127">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="52f32-128">hello kapcsolati karakterlánc az alkalmazás megjelenik a klasszikus Azure portál hello.</span><span class="sxs-lookup"><span data-stu-id="52f32-128">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="52f32-129">Bevonási inicializálása</span><span class="sxs-lookup"><span data-stu-id="52f32-129">Engagement initialization</span></span>
<span data-ttu-id="52f32-130">Amikor létrehoz egy új projektet egy `App.xaml.cs` fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="52f32-130">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="52f32-131">Ez az osztály örökli `Application` és számos fontos metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="52f32-131">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="52f32-132">Azt is használt tooinitialize hello Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="52f32-132">It will also be used tooinitialize hello Engagement SDK.</span></span>

<span data-ttu-id="52f32-133">Módosítsa a hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="52f32-133">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="52f32-134">Adja hozzá a tooyour `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="52f32-134">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="52f32-135">Helyezze be `EngagementAgent.Instance.Init` a hello `Application_Launching` módszert:</span><span class="sxs-lookup"><span data-stu-id="52f32-135">Insert `EngagementAgent.Instance.Init` in hello `Application_Launching` method :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* <span data-ttu-id="52f32-136">Helyezze be `EngagementAgent.Instance.OnActivated` a hello `Application_Activated` módszert:</span><span class="sxs-lookup"><span data-stu-id="52f32-136">Insert `EngagementAgent.Instance.OnActivated` in hello `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> <span data-ttu-id="52f32-137">Kifejezetten nem ajánlott, tooadd hello Engagement inicializálására az alkalmazás egy másik helyen.</span><span class="sxs-lookup"><span data-stu-id="52f32-137">We strongly discourage you tooadd hello Engagement initialization in another place of your application.</span></span> <span data-ttu-id="52f32-138">Azonban figyelembe, hogy hello `EngagementAgent.Instance.Init` metódus egy dedikált szálon, és nem a felhasználói felület szálán hello futtatja.</span><span class="sxs-lookup"><span data-stu-id="52f32-138">However, be aware that hello `EngagementAgent.Instance.Init` method runs on a dedicated thread, and not on hello UI thread.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="52f32-139">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="52f32-139">Basic reporting</span></span>
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a><span data-ttu-id="52f32-140">Ajánlott módszer: túlterhelés a `PhoneApplicationPage` osztályok</span><span class="sxs-lookup"><span data-stu-id="52f32-140">Recommended method : overload your `PhoneApplicationPage` classes</span></span>
<span data-ttu-id="52f32-141">Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, egyszerűen végezheti el az összes a `PhoneApplicationPage` alosztályokat örökölhet hello `EngagementPage` osztályok.</span><span class="sxs-lookup"><span data-stu-id="52f32-141">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `PhoneApplicationPage` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="52f32-142">Íme egy példa bemutatja, hogyan toodo Ez az alkalmazás egy lap.</span><span class="sxs-lookup"><span data-stu-id="52f32-142">Here is an example of how toodo this for a page of your application.</span></span> <span data-ttu-id="52f32-143">Mindent hello ugyanaz az alkalmazás összes lapja esetében.</span><span class="sxs-lookup"><span data-stu-id="52f32-143">You can do hello same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="52f32-144">C# forrásfájl</span><span class="sxs-lookup"><span data-stu-id="52f32-144">C# Source file</span></span>
<span data-ttu-id="52f32-145">A lap módosítása `.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="52f32-145">Modify your page `.xaml.cs` file :</span></span>

* <span data-ttu-id="52f32-146">Adja hozzá a tooyour `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="52f32-146">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="52f32-147">Cserélje le `PhoneApplicationPage` rendelkező `EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="52f32-147">Replace `PhoneApplicationPage` with `EngagementPage` :</span></span>

<span data-ttu-id="52f32-148">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="52f32-148">**Without Engagement:**</span></span>

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

<span data-ttu-id="52f32-149">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="52f32-149">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> <span data-ttu-id="52f32-150">Ha az oldala örököl hello `OnNavigatedTo` módszer, gondos toolet hello kell `base.OnNavigatedTo(e)` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="52f32-150">If your page inherits from hello `OnNavigatedTo` method, be careful toolet hello `base.OnNavigatedTo(e)` call.</span></span> <span data-ttu-id="52f32-151">Ellenkező esetben hello tevékenység nem fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="52f32-151">Otherwise, hello activity will not be reported.</span></span> <span data-ttu-id="52f32-152">Valóban hello `EngagementPage` hívja a következő `StartActivity` belül hello `OnNavigatedTo` metódust.</span><span class="sxs-lookup"><span data-stu-id="52f32-152">Indeed, hello `EngagementPage` is calling `StartActivity` inside hello `OnNavigatedTo` method.</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="52f32-153">XAML-fájl</span><span class="sxs-lookup"><span data-stu-id="52f32-153">XAML file</span></span>
<span data-ttu-id="52f32-154">A lap módosítása `.xaml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="52f32-154">Modify your page `.xaml` file :</span></span>

* <span data-ttu-id="52f32-155">Tooyour névtér-deklarációk hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="52f32-155">Add tooyour namespaces declarations :</span></span>
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* <span data-ttu-id="52f32-156">Cserélje le `phone:PhoneApplicationPage` rendelkező `engagement:EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="52f32-156">Replace `phone:PhoneApplicationPage` with `engagement:EngagementPage` :</span></span>

<span data-ttu-id="52f32-157">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="52f32-157">**Without Engagement:**</span></span>

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

<span data-ttu-id="52f32-158">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="52f32-158">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-hello-default-behavior"></a><span data-ttu-id="52f32-159">Hello alapértelmezett viselkedés felülbírálásához</span><span class="sxs-lookup"><span data-stu-id="52f32-159">Override hello default behavior</span></span>
<span data-ttu-id="52f32-160">Alapértelmezés szerint hello osztálynév hello lap hello tevékenység nevét, nem extra akkor számít.</span><span class="sxs-lookup"><span data-stu-id="52f32-160">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="52f32-161">Ha hello osztály hello "Lap" utótagot használ, Engagement is törlődik.</span><span class="sxs-lookup"><span data-stu-id="52f32-161">If hello class uses hello "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="52f32-162">Ha hello neve toooverride hello alapértelmezett viselkedését, egyszerűen adja hozzá a tooyour kódot:</span><span class="sxs-lookup"><span data-stu-id="52f32-162">If you want toooverride hello default behavior for hello name, simply add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

<span data-ttu-id="52f32-163">Ha tooreport néhány további információt szeretne a tevékenységet, a tooyour kódot is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="52f32-163">If you want tooreport some extra information with your activity, you can add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

<span data-ttu-id="52f32-164">Ezek a módszerek is meghívhatók hello `OnNavigatedTo` módszer a lap.</span><span class="sxs-lookup"><span data-stu-id="52f32-164">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="52f32-165">Másodlagos módszer: hívja `StartActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="52f32-165">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="52f32-166">Ha nem, vagy nem toooverload a `PhoneApplicationPage` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="52f32-166">If you cannot or do not want toooverload your `PhoneApplicationPage` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="52f32-167">Azt javasoljuk, hogy hívása `StartActivity` belül a `OnNavigatedTo` a PhoneApplicationPage metódusában.</span><span class="sxs-lookup"><span data-stu-id="52f32-167">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your PhoneApplicationPage.</span></span>

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> <span data-ttu-id="52f32-168">Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="52f32-168">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="52f32-169">hello SDK automatikusan meghívja a hello `EndActivity` metódus hello alkalmazás bezárásakor.</span><span class="sxs-lookup"><span data-stu-id="52f32-169">hello SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="52f32-170">Így **magas** toocall hello ajánlott `StartActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl**soha** hívás hello `EndActivity` metódust.</span><span class="sxs-lookup"><span data-stu-id="52f32-170">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method.</span></span> <span data-ttu-id="52f32-171">Ez a módszer küld message toohello Engagement kiszolgáló, hogy hello aktuális felhasználó kilépett hello alkalmazást, és ez hatással van az összes alkalmazás naplóiban.</span><span class="sxs-lookup"><span data-stu-id="52f32-171">This method sends a message toohello Engagement server that hello current user has left hello application and this impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="52f32-172">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="52f32-172">Advanced reporting</span></span>
<span data-ttu-id="52f32-173">Szükség esetén érdemes lehet tooreport adott eseményeket, hibákat és feladatokat, toodo így, használjon más módszereket található hello hello `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="52f32-173">Optionally, you may want tooreport application specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="52f32-174">hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók.</span><span class="sxs-lookup"><span data-stu-id="52f32-174">hello Engagement API allows toouse all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="52f32-175">További információkért lásd: [hogyan toouse hello speciális API szerinti címkézését a Windows Phone Silverlight-alkalmazást a Mobile Engagement](mobile-engagement-windows-phone-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="52f32-175">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="52f32-176">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="52f32-176">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="52f32-177">Automatikus összeomlási jelentések letiltása</span><span class="sxs-lookup"><span data-stu-id="52f32-177">Disable automatic crash reporting</span></span>
<span data-ttu-id="52f32-178">Bármikor letilthatja hello automatikus összeomlási jelentéskészítési funkció bekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="52f32-178">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="52f32-179">Ezután amikor nem kezelt kivétel lép fel, Engagement nem semmit.</span><span class="sxs-lookup"><span data-stu-id="52f32-179">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="52f32-180">Ha azt tervezi, toodisable ezt a szolgáltatást, vegye figyelembe, hogy ha egy nem kezelt összeomlási történik, az alkalmazásban, Engagement nem küld hello összeomlási **és** nem bezárul hello munkamenet és feladatok.</span><span class="sxs-lookup"><span data-stu-id="52f32-180">If you plan toodisable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send hello crash **AND** it will not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="52f32-181">toodisable automatikus összeomlási reporting, attól függően, hogy hello módja, deklarált, konfiguráció csak testreszabása:</span><span class="sxs-lookup"><span data-stu-id="52f32-181">toodisable automatic crash reporting, just customize your configuration depending on hello way you declared it :</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="52f32-182">A `EngagementConfiguration.xml` fájl</span><span class="sxs-lookup"><span data-stu-id="52f32-182">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="52f32-183">Állítsa be a jelentés túl összeomlik miattuk`false` közötti `<reportCrash>` és `</reportCrash>` címkék.</span><span class="sxs-lookup"><span data-stu-id="52f32-183">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="52f32-184">A `EngagementConfiguration` futási időben objektum</span><span class="sxs-lookup"><span data-stu-id="52f32-184">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="52f32-185">Állítsa be a jelentés összeomlási toofalse a EngagementConfiguration objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="52f32-185">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="52f32-186">Nagy sebességű átvitel</span><span class="sxs-lookup"><span data-stu-id="52f32-186">Burst mode</span></span>
<span data-ttu-id="52f32-187">Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="52f32-187">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="52f32-188">Ha az alkalmazás naplók nagyon gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (Ez a lehetőség hello "kapacitásnövelés mód").</span><span class="sxs-lookup"><span data-stu-id="52f32-188">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello “burst mode”).</span></span>

<span data-ttu-id="52f32-189">toodo tehát hello metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="52f32-189">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="52f32-190">hello argumentum értéke a **ezredmásodperc**.</span><span class="sxs-lookup"><span data-stu-id="52f32-190">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="52f32-191">Bármikor Ha azt szeretné, hogy tooreactivate hello valós idejű naplózási, csak hívja hello metódus bármely paraméter nélkül vagy hello 0 értékkel.</span><span class="sxs-lookup"><span data-stu-id="52f32-191">At any time, if you want tooreactivate hello real-time logging, just call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="52f32-192">hello kapacitásnövelés mód némileg növeli hello akkumulátor élettartama hello Engagement figyelő azonban hatással van: az összes munkamenetek és feladatok is lesznek kerekített toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="52f32-192">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="52f32-193">Az ajánlott toouse egy mint 30000 (30s) kapacitásnövelés küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="52f32-193">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="52f32-194">Vegye figyelembe a mentett naplókat még korlátozott too300 elemek toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="52f32-194">You have toobe aware that saved logs are limited too300 items.</span></span> <span data-ttu-id="52f32-195">Ha a küldő túl hosszú néhány naplók elveszhet.</span><span class="sxs-lookup"><span data-stu-id="52f32-195">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="52f32-196">hello kapacitásnövelés küszöbértéke nem lehet konfigurálni tooa időszak kevesebb mint egy második.</span><span class="sxs-lookup"><span data-stu-id="52f32-196">hello burst threshold cannot be configured tooa period lesser than one second.</span></span> <span data-ttu-id="52f32-197">Ha úgy próbálja toodo, hello SDK hello hiba: a nyomkövetési bemutatják és automatikusan alaphelyzetbe toohello alapértelmezett érték, ez azt jelenti, hogy nulla másodperc.</span><span class="sxs-lookup"><span data-stu-id="52f32-197">If you try toodo so, hello SDK will show a trace with hello error and will automatically reset toohello default value, that is, zero seconds.</span></span> <span data-ttu-id="52f32-198">Ez akkor indul el, hello SDK tooreport hello naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="52f32-198">This will trigger hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

