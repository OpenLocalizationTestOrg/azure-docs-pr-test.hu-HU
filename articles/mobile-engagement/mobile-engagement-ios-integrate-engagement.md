---
title: "a Mobile Engagement iOS SDK-integráció aaaAzure |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 947ea44b-00c1-450f-9a3b-74437954dc56
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 66ce34efabede7d882caa8a91431a8df71e4fb59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-ios"></a><span data-ttu-id="daf0f-103">Hogyan tooIntegrate Engagement IOS rendszerű eszközökön</span><span class="sxs-lookup"><span data-stu-id="daf0f-103">How tooIntegrate Engagement on iOS</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="daf0f-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="daf0f-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="daf0f-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="daf0f-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="daf0f-106">iOS</span><span class="sxs-lookup"><span data-stu-id="daf0f-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="daf0f-107">Android</span><span class="sxs-lookup"><span data-stu-id="daf0f-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
>
>

<span data-ttu-id="daf0f-108">Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Engagement elemzés és Monitorozási funkciók az iOS-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="daf0f-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your iOS application.</span></span>

<span data-ttu-id="daf0f-109">hello Engagement SDK iOS7 + és Xcode 8 + van szükség: az alkalmazás központi telepítés célját hello verziójúnak kell lennie iOS 7-es.</span><span class="sxs-lookup"><span data-stu-id="daf0f-109">hello Engagement SDK requires iOS7+ and Xcode 8+: hello deployment target of your application must be at least iOS 7.</span></span>

> [!NOTE]
> <span data-ttu-id="daf0f-110">Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="daf0f-110">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="daf0f-111">Nincs egy ismert hiba hello Reach-modul korábbi verzióhoz iOS 10 eszközök lásd futtatott [reach-modul integrációs hello](mobile-engagement-ios-integrate-engagement-reach.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="daf0f-111">There is a known bug on hello Reach module of this previous version while running on iOS 10 devices see [hello reach module integration](mobile-engagement-ios-integrate-engagement-reach.md) for more details.</span></span> <span data-ttu-id="daf0f-112">Ha toouse hello SDK v3.2.4 válassza ki, akkor csak a hello kihagyása `UserNotifications.framework` hello következő lépésben importálni.</span><span class="sxs-lookup"><span data-stu-id="daf0f-112">If you choose toouse hello SDK v3.2.4 then just skip hello `UserNotifications.framework` import in hello next step.</span></span>
>
>

<span data-ttu-id="daf0f-113">a lépéseket követve hello a naplók elegendő tooactivate hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok.</span><span class="sxs-lookup"><span data-stu-id="daf0f-113">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="daf0f-114">hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális a Mobile Engagement az iOS-alkalmazás szerinti címkézését API](mobile-engagement-ios-use-engagement-api.md) óta statisztikák alkalmazás függenek.</span><span class="sxs-lookup"><span data-stu-id="daf0f-114">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API  (see [How toouse hello advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-hello-engagement-sdk-into-your-ios-project"></a><span data-ttu-id="daf0f-115">Az iOS-projekthez az Engagement SDK hello beágyazása</span><span class="sxs-lookup"><span data-stu-id="daf0f-115">Embed hello Engagement SDK into your iOS project</span></span>
* <span data-ttu-id="daf0f-116">Hello iOS SDK letöltése a [Itt](http://aka.ms/qk2rnj).</span><span class="sxs-lookup"><span data-stu-id="daf0f-116">Download hello iOS SDK from [here](http://aka.ms/qk2rnj).</span></span>
* <span data-ttu-id="daf0f-117">Hozzáadás hello Engagement SDK tooyour iOS-projekt: az Xcode-ban kattintson a jobb gombbal a projekt, és válassza ki **"fájlok túl hozzáadása..."** , és válassza a hello `EngagementSDK` mappa.</span><span class="sxs-lookup"><span data-stu-id="daf0f-117">Add hello Engagement SDK tooyour iOS project: in Xcode, right click on your project and select **"Add files too..."** and choose hello `EngagementSDK` folder.</span></span>
* <span data-ttu-id="daf0f-118">Bevonási szükséges további keretrendszerek toowork: hello project Explorer, nyissa meg a projekt ablaktáblán, és válassza ki a megfelelő cél hello.</span><span class="sxs-lookup"><span data-stu-id="daf0f-118">Engagement requires additional frameworks toowork: in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="daf0f-119">Ezután nyissa meg a hello **"Összeállítási fázisok"** lapon és a hello **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszereket:</span><span class="sxs-lookup"><span data-stu-id="daf0f-119">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add these frameworks:</span></span>

  * <span data-ttu-id="daf0f-120">`UserNotifications.framework`-készlet hello hivatkozásokat`Optional`</span><span class="sxs-lookup"><span data-stu-id="daf0f-120">`UserNotifications.framework` - set hello link as `Optional`</span></span>
  * <span data-ttu-id="daf0f-121">`AdSupport.framework`-készlet hello hivatkozásokat`Optional`</span><span class="sxs-lookup"><span data-stu-id="daf0f-121">`AdSupport.framework` - set hello link as `Optional`</span></span>
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> <span data-ttu-id="daf0f-122">hello AdSupport keretrendszer távolítható el.</span><span class="sxs-lookup"><span data-stu-id="daf0f-122">hello AdSupport framework can be removed.</span></span> <span data-ttu-id="daf0f-123">Bevonási kell a keretrendszer toocollect hello idfa-JÁT.</span><span class="sxs-lookup"><span data-stu-id="daf0f-123">Engagement needs this framework toocollect hello IDFA.</span></span> <span data-ttu-id="daf0f-124">Azonban le kell tiltani IDFA-gyűjtést \<ios-sdk-engagement-idfa-ját\> toocomply hello új Apple házirenddel kapcsolatos ezt.</span><span class="sxs-lookup"><span data-stu-id="daf0f-124">However, IDFA collection can be disabled \<ios-sdk-engagement-idfa\> toocomply with hello new Apple policy regarding this ID.</span></span>
>
>

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="daf0f-125">Hello Engagement SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="daf0f-125">Initialize hello Engagement SDK</span></span>
<span data-ttu-id="daf0f-126">Az alkalmazás delegáltjának kell toomodify:</span><span class="sxs-lookup"><span data-stu-id="daf0f-126">You need toomodify your Application Delegate:</span></span>

* <span data-ttu-id="daf0f-127">A fájlt a hello tetején hello Engagement ügynök importálása:</span><span class="sxs-lookup"><span data-stu-id="daf0f-127">At hello top of your implementation file, import hello Engagement agent:</span></span>

      [...]
      #import "EngagementAgent.h"
* <span data-ttu-id="daf0f-128">Engagement inicializálására belül hello metódus "**applicationDidFinishLaunching:**"vagy"**alkalmazás: didFinishLaunchingWithOptions:**":</span><span class="sxs-lookup"><span data-stu-id="daf0f-128">Initialize Engagement inside hello method '**applicationDidFinishLaunching:**' or '**application:didFinishLaunchingWithOptions:**':</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a><span data-ttu-id="daf0f-129">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="daf0f-129">Basic reporting</span></span>
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a><span data-ttu-id="daf0f-130">Ajánlott módszer: túlterhelés a `UIViewController` osztályok</span><span class="sxs-lookup"><span data-stu-id="daf0f-130">Recommended method: overload your `UIViewController` classes</span></span>
<span data-ttu-id="daf0f-131">Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, egyszerűen végezheti el az összes a `UIViewController` alosztályokat örökölhet hello `EngagementViewController` osztályok (ugyanaz a szabály a `UITableViewController`  - \> `EngagementTableViewController`).</span><span class="sxs-lookup"><span data-stu-id="daf0f-131">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `UIViewController` sub-classes inherit from hello `EngagementViewController` classes (same rule for `UITableViewController` -\> `EngagementTableViewController`).</span></span>

<span data-ttu-id="daf0f-132">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="daf0f-132">**Without Engagement :**</span></span>

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

<span data-ttu-id="daf0f-133">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="daf0f-133">**With Engagement :**</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="daf0f-134">Másodlagos módszer: hívja `startActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="daf0f-134">Alternate method: call `startActivity()` manually</span></span>
<span data-ttu-id="daf0f-135">Ha nem, vagy nem toooverload a `UIViewController` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent`tartozó módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="daf0f-135">If you cannot or do not want toooverload your `UIViewController` classes, you can instead start your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="daf0f-136">hello iOS SDK automatikusan meghívja a hello `endActivity()` metódus hello alkalmazás bezárásakor.</span><span class="sxs-lookup"><span data-stu-id="daf0f-136">hello iOS SDK automatically calls hello `endActivity()` method when hello application is closed.</span></span> <span data-ttu-id="daf0f-137">Így *magas* toocall hello ajánlott `startActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl*soha* hívás hello `endActivity` metódus kényszeríti a metódus hívása óta hello aktuális munkamenet toobe véget ért.</span><span class="sxs-lookup"><span data-stu-id="daf0f-137">Thus, it is *HIGHLY* recommended toocall hello `startActivity` method whenever hello activity of hello user change, and too*NEVER* call hello `endActivity` method, since calling this method forces hello current session toobe ended.</span></span>
>
>

## <a name="location-reporting"></a><span data-ttu-id="daf0f-138">Helyi jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="daf0f-138">Location reporting</span></span>
<span data-ttu-id="daf0f-139">Szolgáltatási feltételek Apple nem teszik lehetővé az alkalmazások toouse hely csak a statisztika célra nyomon követése.</span><span class="sxs-lookup"><span data-stu-id="daf0f-139">Apple terms of service do not allow applications toouse location tracking for statistics purpose only.</span></span> <span data-ttu-id="daf0f-140">Ebből kifolyólag javasoljuk tooenable hely jelentések csak akkor, ha az alkalmazás is használhatnak hello más okból nyomon követése.</span><span class="sxs-lookup"><span data-stu-id="daf0f-140">Thus, it is recommended tooenable location reports only if your application also use hello location tracking for another reason.</span></span>

<span data-ttu-id="daf0f-141">Az iOS 8-től kezdődően meg kell adnia egy hogyan alkalmazása használja-e helyszolgáltatás úgy, hogy egy karakterláncot hello kulcs leírását [NSLocationWhenInUseUsageDescription] vagy [NSLocationAlwaysUsageDescription]az alkalmazás Info.plist fájljában.</span><span class="sxs-lookup"><span data-stu-id="daf0f-141">Starting with iOS 8, you must provide a description for how your app uses location services by setting a string for hello key [NSLocationWhenInUseUsageDescription] or [NSLocationAlwaysUsageDescription] in your app's Info.plist file.</span></span> <span data-ttu-id="daf0f-142">Ha azt szeretné, hogy az Engagement hello háttérben tooreport helyét, adja hozzá a hello kulcs NSLocationAlwaysUsageDescription.</span><span class="sxs-lookup"><span data-stu-id="daf0f-142">If you want tooreport location in hello background with Engagement, add hello key NSLocationAlwaysUsageDescription.</span></span> <span data-ttu-id="daf0f-143">Minden más esetben hello kulcsot NSLocationWhenInUseUsageDescription hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="daf0f-143">In all other cases, add hello key NSLocationWhenInUseUsageDescription.</span></span> <span data-ttu-id="daf0f-144">Ne feledje, hogy NSLocationAlwaysAndWhenInUseUsageDescription és a NSLocationWhenInUseUsageDescription tooreport háttér hely IOS 11.</span><span class="sxs-lookup"><span data-stu-id="daf0f-144">Note that you need both NSLocationAlwaysAndWhenInUseUsageDescription and NSLocationWhenInUseUsageDescription tooreport background location on iOS 11.</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="daf0f-145">Helymeghatározást</span><span class="sxs-lookup"><span data-stu-id="daf0f-145">Lazy area location reporting</span></span>
<span data-ttu-id="daf0f-146">Helymeghatározást lehetővé teszi, hogy tooreport hello ország, valamint régió és Helység társított toodevices.</span><span class="sxs-lookup"><span data-stu-id="daf0f-146">Lazy area location reporting allows tooreport hello country, region and locality associated toodevices.</span></span> <span data-ttu-id="daf0f-147">A hely jelentéskészítési típusú csak használ a hálózati helyek (cella azonosító vagy Wi-Fi alapján).</span><span class="sxs-lookup"><span data-stu-id="daf0f-147">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="daf0f-148">hello eszköz terület munkamenetenként legfeljebb egyszer jelenti.</span><span class="sxs-lookup"><span data-stu-id="daf0f-148">hello device area is reported at most once per session.</span></span> <span data-ttu-id="daf0f-149">hello GPS a rendszer nem használja, és ezáltal az ilyen típusú hely jelentés nagyon kevés (nem toosay nem) hello akkumulátor hatása.</span><span class="sxs-lookup"><span data-stu-id="daf0f-149">hello GPS is never used, and thus this type of location report has very few (not toosay no) impact on hello battery.</span></span>

<span data-ttu-id="daf0f-150">Jelentett területek a következők felhasználók, munkamenetek, kapcsolódó események és hibák földrajzi statisztikája használt toocompute.</span><span class="sxs-lookup"><span data-stu-id="daf0f-150">Reported areas are used toocompute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="daf0f-151">Ezek használhatók a Reach-kampányokat feltételként.</span><span class="sxs-lookup"><span data-stu-id="daf0f-151">They can also be used as criterion in Reach campaigns.</span></span> <span data-ttu-id="daf0f-152">utolsó ismert lehet lekérdezése Köszönjük toohello jelentett terület hello [Device API-val].</span><span class="sxs-lookup"><span data-stu-id="daf0f-152">hello last known area reported for a device can be retrieved thanks toohello [Device API].</span></span>

<span data-ttu-id="daf0f-153">tooenable Lusta terület helye reporting, adja hozzá a sor hello Engagement ügynök inicializálása után a következő hello:</span><span class="sxs-lookup"><span data-stu-id="daf0f-153">tooenable lazy area location reporting, add hello following line after initializing hello Engagement agent:</span></span>

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a><span data-ttu-id="daf0f-154">Valós idejű hely jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="daf0f-154">Real time location reporting</span></span>
<span data-ttu-id="daf0f-155">Valós idejű hely jelentéskészítési lehetővé teszi, hogy tooreport hello szélességi és hosszúsági társított toodevices.</span><span class="sxs-lookup"><span data-stu-id="daf0f-155">Real time location reporting allows tooreport hello latitude and longitude associated toodevices.</span></span> <span data-ttu-id="daf0f-156">Alapértelmezés szerint a hely jelentéskészítési csak használja a hálózati helyek (cella azonosító vagy Wi-Fi alapján), és hello reporting csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="daf0f-156">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and hello reporting is only active when hello application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="daf0f-157">Valós idejű helyek *nem* toocompute statisztika használt.</span><span class="sxs-lookup"><span data-stu-id="daf0f-157">Real time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="daf0f-158">A csak célja a geokerítések valós idejű tooallow hello használata \<Reach-célközönségre-geokerítések\> Reach-kampányokat a feltételnek.</span><span class="sxs-lookup"><span data-stu-id="daf0f-158">Their only purpose is tooallow hello use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="daf0f-159">tooenable valós idejű hely jelentéskészítési, adja hozzá a sor hello Engagement ügynök inicializálása után a következő hello:</span><span class="sxs-lookup"><span data-stu-id="daf0f-159">tooenable real time location reporting, add hello following line after initializing hello Engagement agent:</span></span>

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a><span data-ttu-id="daf0f-160">GPS-alapú jelentés</span><span class="sxs-lookup"><span data-stu-id="daf0f-160">GPS based reporting</span></span>
<span data-ttu-id="daf0f-161">Alapértelmezés szerint valós idejű helyét jelentő csak funkció alapú hálózati helyek.</span><span class="sxs-lookup"><span data-stu-id="daf0f-161">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="daf0f-162">tooenable hello GPS alapuló (amelyek pontos sokkal) helyét, adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="daf0f-162">tooenable hello use of GPS based locations (which are far more precise), add:</span></span>

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a><span data-ttu-id="daf0f-163">Háttér-jelentés</span><span class="sxs-lookup"><span data-stu-id="daf0f-163">Background reporting</span></span>
<span data-ttu-id="daf0f-164">Alapértelmezés szerint valós idejű hely jelentéskészítési az csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="daf0f-164">By default, real time location reporting is only active when hello application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="daf0f-165">Adja hozzá a háttérben, is reporting tooenable hello:</span><span class="sxs-lookup"><span data-stu-id="daf0f-165">tooenable hello reporting also in background, add:</span></span>

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> <span data-ttu-id="daf0f-166">Hello alkalmazás fut a háttérben, amikor csak alapú hálózati helyek küld jelentést, még akkor is, ha engedélyezte a hello GPS.</span><span class="sxs-lookup"><span data-stu-id="daf0f-166">When hello application runs in background, only network based locations are reported, even if you enabled hello GPS.</span></span>
>
>

<span data-ttu-id="daf0f-167">Ez a függvény végrehajtása fel fogja hívni [startMonitoringSignificantLocationChanges] amikor az alkalmazás a hello háttér állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="daf0f-167">Implementation of this function will call [startMonitoringSignificantLocationChanges] when your application goes into hello background.</span></span> <span data-ttu-id="daf0f-168">Vegye figyelembe, hogy azt fogja automatikusan indítsa újra az alkalmazás hello háttér Ha új helyet esemény érkezik.</span><span class="sxs-lookup"><span data-stu-id="daf0f-168">Be aware that it will automatically relaunch your application into hello background if a new location event arrives.</span></span>

## <a name="advanced-reporting"></a><span data-ttu-id="daf0f-169">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="daf0f-169">Advanced reporting</span></span>
<span data-ttu-id="daf0f-170">Nem kötelező, ha tooreport adott eseményeket, hibákat és feladatok, kell toouse hello Engagement API keresztül hello hello módszerek `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="daf0f-170">Optionally, if you want tooreport application specific events, errors and jobs, you need toouse hello Engagement API through hello methods of hello `EngagementAgent` class.</span></span> <span data-ttu-id="daf0f-171">Ez az osztály egy objektumának lekérhetők hívó hello `[EngagementAgent shared]` statikus metódus.</span><span class="sxs-lookup"><span data-stu-id="daf0f-171">An object of this class can be retrieved by calling hello `[EngagementAgent shared]` static method.</span></span>

<span data-ttu-id="daf0f-172">hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók és részleteit a hello hogyan tooUse az Engagement API IOS (valamint hello hello műszaki dokumentációját az `EngagementAgent` osztály).</span><span class="sxs-lookup"><span data-stu-id="daf0f-172">hello Engagement API allows toouse all of Engagement's advanced capabilities and is detailed in hello How tooUse the Engagement API on iOS (as well as in hello technical documentation of hello `EngagementAgent` class).</span></span>

## <a name="disable-idfa-collection"></a><span data-ttu-id="daf0f-173">Tiltsa le a IDFA-gyűjtést</span><span class="sxs-lookup"><span data-stu-id="daf0f-173">Disable IDFA collection</span></span>
<span data-ttu-id="daf0f-174">Alapértelmezés szerint a bevonási használandó hello [idfa-JÁT] toouniquely felhasználó azonosításának.</span><span class="sxs-lookup"><span data-stu-id="daf0f-174">By default, Engagement will use hello [IDFA] toouniquely identify a user.</span></span> <span data-ttu-id="daf0f-175">De ha máshol hirdetési hello alkalmazás nem használja, akkor előfordulhat, hogy elutasítja hello App Store felülvizsgálati folyamat.</span><span class="sxs-lookup"><span data-stu-id="daf0f-175">But if you’re not using advertising elsewhere in hello app, you might be rejected by hello App Store review process.</span></span> <span data-ttu-id="daf0f-176">IDFA-gyűjtést letiltható hello előfeldolgozó makró hozzáadásával `ENGAGEMENT_DISABLE_IDFA` a pch fájlban (vagy a hello `Build Settings` az alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="daf0f-176">IDFA collection can be disabled by adding hello preprocessor macro `ENGAGEMENT_DISABLE_IDFA` in your pch file (or in hello `Build Settings` of your application).</span></span> <span data-ttu-id="daf0f-177">Ezzel biztosíthatja, hogy nincs nem túl`ASIdentifierManager`, `advertisingIdentifier` vagy `isAdvertisingTrackingEnabled` hello alkalmazás építés.</span><span class="sxs-lookup"><span data-stu-id="daf0f-177">This will ensure that there is no references too`ASIdentifierManager`, `advertisingIdentifier` or `isAdvertisingTrackingEnabled` in hello application build.</span></span>

<span data-ttu-id="daf0f-178">Integráció a hello **prefix.pch** fájlt:</span><span class="sxs-lookup"><span data-stu-id="daf0f-178">Integration in hello **prefix.pch** file:</span></span>

    #define ENGAGEMENT_DISABLE_IDFA
    ...

<span data-ttu-id="daf0f-179">Ellenőrizheti, hogy hello IDFA-gyűjtést megfelelően letiltotta az alkalmazás hello Engagement vizsgálati naplók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="daf0f-179">You can verify that hello IDFA collection is properly disabled in your application by checking hello Engagement test logs.</span></span> <span data-ttu-id="daf0f-180">Lásd: Integráció tesztelése hello\<ios-sdk-engagement-teszt-idfa-ját\> dokumentációja további információkat.</span><span class="sxs-lookup"><span data-stu-id="daf0f-180">See hello Integration Test\<ios-sdk-engagement-test-idfa\> documentation for further information.</span></span>

## <a name="disable-log-reporting"></a><span data-ttu-id="daf0f-181">Naplózási jelentések letiltásához</span><span class="sxs-lookup"><span data-stu-id="daf0f-181">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="daf0f-182">Metódushívások használatával</span><span class="sxs-lookup"><span data-stu-id="daf0f-182">Using a method call</span></span>
<span data-ttu-id="daf0f-183">Ha azt szeretné, hogy a naplók küldése Engagement toostop, hívása:</span><span class="sxs-lookup"><span data-stu-id="daf0f-183">If you want Engagement toostop sending logs, you can call:</span></span>

    [[EngagementAgent shared] setEnabled:NO];

<span data-ttu-id="daf0f-184">Ez a hívás az állandó: használ `NSUserDefaults` toostore hello információkat.</span><span class="sxs-lookup"><span data-stu-id="daf0f-184">This call is persistent: it uses `NSUserDefaults` toostore hello information.</span></span>

<span data-ttu-id="daf0f-185">Engedélyezheti újra reporting működik az azonos hello meghívásával napló `YES`.</span><span class="sxs-lookup"><span data-stu-id="daf0f-185">You can enable log reporting again by calling hello same function with `YES`.</span></span>

### <a name="integration-in-your-settings-bundle"></a><span data-ttu-id="daf0f-186">A beállítások nyalábban integráció</span><span class="sxs-lookup"><span data-stu-id="daf0f-186">Integration in your settings bundle</span></span>
<span data-ttu-id="daf0f-187">Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `Settings.bundle` fájlt.</span><span class="sxs-lookup"><span data-stu-id="daf0f-187">Instead of calling this function, you can also integrate this setting directly in your existing `Settings.bundle` file.</span></span> <span data-ttu-id="daf0f-188">karakterlánc hello `engagement_agent_enabled` kell használni egy hello preferencia azonosítót, és társított tooa váltása kapcsoló kell lennie (`PSToggleSwitchSpecifier`).</span><span class="sxs-lookup"><span data-stu-id="daf0f-188">hello string `engagement_agent_enabled` must be used as a hello preference identifier and it must be associated tooa toggle switch(`PSToggleSwitchSpecifier`).</span></span>

<span data-ttu-id="daf0f-189">Példa következő hello `Settings.bundle` bemutatja, hogyan tooimplement azt:</span><span class="sxs-lookup"><span data-stu-id="daf0f-189">hello following example of `Settings.bundle` shows how tooimplement it:</span></span>

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Device API-val]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[idfa-JÁT]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
