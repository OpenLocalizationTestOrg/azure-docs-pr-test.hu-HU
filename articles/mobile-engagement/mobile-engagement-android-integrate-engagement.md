---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4f79936ea0fa6102023dec2b4682032a4a81fa9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-android"></a><span data-ttu-id="84c31-103">Hogyan tooIntegrate Engagement Android rendszeren</span><span class="sxs-lookup"><span data-stu-id="84c31-103">How tooIntegrate Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="84c31-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="84c31-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="84c31-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="84c31-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="84c31-106">iOS</span><span class="sxs-lookup"><span data-stu-id="84c31-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="84c31-107">Android</span><span class="sxs-lookup"><span data-stu-id="84c31-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="84c31-108">Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Engagement elemzés és figyelési funkciók az Android-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="84c31-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your Android application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84c31-109">A minimális Android SDK API-szintet kell lennie, 10 vagy újabb rendszer (Android 2.3.3 vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="84c31-109">Your minimum Android SDK API level must be 10 or higher (Android 2.3.3 or higher).</span></span>
> 
> 

<span data-ttu-id="84c31-110">a lépéseket követve hello a naplók elegendő tooactivates hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok.</span><span class="sxs-lookup"><span data-stu-id="84c31-110">hello following steps are enough tooactivates hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="84c31-111">hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális a Mobile Engagement az Android API szerinti címkézését](mobile-engagement-android-use-engagement-api.md) óta ezek a statisztikákat a függő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84c31-111">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Android](mobile-engagement-android-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-hello-engagement-sdk-and-service-into-your-android-project"></a><span data-ttu-id="84c31-112">Hello Engagement SDK-t és a szolgáltatás beágyazása az Android-projekt</span><span class="sxs-lookup"><span data-stu-id="84c31-112">Embed hello Engagement SDK and service into your Android project</span></span>
<span data-ttu-id="84c31-113">Letöltési hello Android SDK [Itt](https://aka.ms/vq9mfn) beolvasása `mobile-engagement-VERSION.jar` és hello is elhelyezheti `libs` az Android-projekt mappájából (Ha még nem létezik, a hello függvénytárak mappa létrehozása).</span><span class="sxs-lookup"><span data-stu-id="84c31-113">Download hello Android SDK from [here](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` and put them into hello `libs` folder of your Android project (create hello libs folder if it does not exist yet).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84c31-114">Ha ProGuard az alkalmazáscsomag, kell tookeep bizonyos osztályokat.</span><span class="sxs-lookup"><span data-stu-id="84c31-114">If you build your application package with ProGuard, you need tookeep some classes.</span></span> <span data-ttu-id="84c31-115">A következő konfigurációs részlet hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="84c31-115">You can use hello following configuration snippet:</span></span>
> 
> <span data-ttu-id="84c31-116">-nyilvános osztály tartsa * android.os.IInterface bővíti-osztály com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {megtartása</span><span class="sxs-lookup"><span data-stu-id="84c31-116">-keep public class * extends android.os.IInterface -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {</span></span>
> 
> <span data-ttu-id="84c31-117"><methods>; }</span><span class="sxs-lookup"><span data-stu-id="84c31-117"><methods>; }</span></span>
> 
> 

<span data-ttu-id="84c31-118">A bevonási kapcsolati karakterlánc megadásához hívó hello metódus hello indítója tevékenység a következő:</span><span class="sxs-lookup"><span data-stu-id="84c31-118">Specify your Engagement connection string by calling hello following method in hello launcher activity:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="84c31-119">hello kapcsolati karakterlánc az alkalmazás Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="84c31-119">hello connection string for your application is displayed on Azure Portal.</span></span>

* <span data-ttu-id="84c31-120">Ha hiányzik, adja hozzá az alábbi Android engedélyek hello (előtt hello `<application>` címke):</span><span class="sxs-lookup"><span data-stu-id="84c31-120">If missing, add hello following Android permissions (before hello `<application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* <span data-ttu-id="84c31-121">Adja hozzá a következő szakasz hello (közötti hello `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="84c31-121">Add hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* <span data-ttu-id="84c31-122">Változás `<Your application name>` által az alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="84c31-122">Change `<Your application name>` by hello name of your application.</span></span>

> [!TIP]
> <span data-ttu-id="84c31-123">Hello `android:label` attribútum lehetővé teszi a toochoose hello neve hello Engagement szolgáltatás toohello végfelhasználók hello "Futó szolgáltatások" képernyőjén telefont fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="84c31-123">hello `android:label` attribute allows you toochoose hello name of hello Engagement service as it will appear toohello end-users in hello "Running services" screen of their phone.</span></span> <span data-ttu-id="84c31-124">Az ajánlott tooset ezt az attribútumot túl`"<Your application name>Service"` (pl. `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="84c31-124">It is recommended tooset this attribute too`"<Your application name>Service"` (e.g. `"AcmeFunGameService"`).</span></span>
> 
> 

<span data-ttu-id="84c31-125">Adja meg hello `android:process` attribútum biztosítja, hogy hello Engagement szolgáltatás folyamatban (Engagement futtató ugyanezt a folyamatot, az alkalmazás biztosítják a fő/felhasználói felület szálán potenciálisan kevésbé rugalmas hello) fog futni.</span><span class="sxs-lookup"><span data-stu-id="84c31-125">Specifying hello `android:process` attribute ensures that hello Engagement service will run in its own process (running Engagement in hello same process as your application will make your main/UI thread potentially less responsive).</span></span>

> [!NOTE]
> <span data-ttu-id="84c31-126">A kód helyez `Application.onCreate()` és más alkalmazás visszahívások fog futni az alkalmazás folyamatok, többek között a hello Engagement szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="84c31-126">Any code you place in `Application.onCreate()` and other application callbacks will be run for all your application's processes, including hello Engagement service.</span></span> <span data-ttu-id="84c31-127">(Például nem szükséges memória-kiosztásokat és szálak hello bevonási folyamat, ismétlődő szórási fogadók vagy szolgáltatások) nemkívánatos hatásai lehetnek.</span><span class="sxs-lookup"><span data-stu-id="84c31-127">It may have unwanted side effects (like unneeded memory allocations and threads in hello Engagement's process, duplicate broadcast receivers or services).</span></span>
> 
> 

<span data-ttu-id="84c31-128">Ha akkor bírálja felül `Application.onCreate()`, a következő kódrészletet a hello elején ajánlott tooadd hello a `Application.onCreate()` függvény:</span><span class="sxs-lookup"><span data-stu-id="84c31-128">If you override `Application.onCreate()`, it's recommended tooadd hello following code snippet at hello beginning of your `Application.onCreate()` function:</span></span>

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

<span data-ttu-id="84c31-129">Mindent ugyanaz a hello `Application.onTerminate()`, `Application.onLowMemory()` és `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="84c31-129">You can do hello same thing for `Application.onTerminate()`, `Application.onLowMemory()` and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="84c31-130">Emellett kibővítheti `EngagementApplication` helyett kiterjesztése `Application`: hello visszahívási `Application.onCreate()` hello folyamat ellenőrzése és hívások `Application.onApplicationProcessCreate()` csak ha hello aktuális folyamat nem hello egy üzemeltetési hello Engagement szolgáltatás, hello ugyanazok a szabályok vonatkoznak a hello más visszahívások.</span><span class="sxs-lookup"><span data-stu-id="84c31-130">You can also extend `EngagementApplication` instead of extending `Application`: hello callback `Application.onCreate()` does hello process check and calls `Application.onApplicationProcessCreate()` only if hello current process is not hello one hosting hello Engagement service, hello same rules apply for hello other callbacks.</span></span>

## <a name="basic-reporting"></a><span data-ttu-id="84c31-131">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="84c31-131">Basic reporting</span></span>
### <a name="recommended-method-overload-your-activity-classes"></a><span data-ttu-id="84c31-132">Ajánlott módszer: túlterhelés a `Activity` osztályok</span><span class="sxs-lookup"><span data-stu-id="84c31-132">Recommended method: overload your `Activity` classes</span></span>
<span data-ttu-id="84c31-133">Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, csak rendelkezik az összes toomake a `*Activity` alosztályokat örökölhet hello megfelelő `Engagement*Activity` osztályok (pl. Ha kiterjeszti az örökölt tevékenység `ListActivity`, ellenőrizze az általa bővített `EngagementListActivity`).</span><span class="sxs-lookup"><span data-stu-id="84c31-133">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you just have toomake all your `*Activity` sub-classes inherit from hello corresponding `Engagement*Activity` classes (e.g. if your legacy activity extends `ListActivity`, make it extends `EngagementListActivity`).</span></span>

<span data-ttu-id="84c31-134">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="84c31-134">**Without Engagement :**</span></span>

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

<span data-ttu-id="84c31-135">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="84c31-135">**With Engagement :**</span></span>

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> <span data-ttu-id="84c31-136">Használata esetén `EngagementListActivity` vagy `EngagementExpandableListActivity`, győződjön meg arról, hogy semmilyen hívásban túl`requestWindowFeature(...);` túl a hello hívása előtt végrehajtott`super.onCreate(...);`, ellenkező esetben a összeomlási történik.</span><span class="sxs-lookup"><span data-stu-id="84c31-136">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call too`requestWindowFeature(...);` is made before hello call too`super.onCreate(...);`, otherwise a crash will occur.</span></span>
> 
> 

<span data-ttu-id="84c31-137">Ezeket az osztályokat az hello található `src` mappa, és a projektben másolja őket.</span><span class="sxs-lookup"><span data-stu-id="84c31-137">You can find these classes in hello `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="84c31-138">hello osztályokat is az hello **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="84c31-138">hello classes are also in hello **JavaDoc**.</span></span>

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="84c31-139">Másodlagos módszer: hívja `startActivity()` és `endActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="84c31-139">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="84c31-140">Ha nem, vagy nem toooverload a `Activity` osztályok, ehelyett elindításához és a tevékenységek befejezése meghívásával `EngagementAgent`tartozó módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="84c31-140">If you cannot or do not want toooverload your `Activity` classes, you can instead start and end your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84c31-141">Android SDK hello soha nem hívja a hello `endActivity()` metódust, akkor is, ha hello alkalmazás bezárása (Android, az alkalmazások valójában soha nem Lezáródnak).</span><span class="sxs-lookup"><span data-stu-id="84c31-141">hello Android SDK never calls hello `endActivity()` method, even when hello application is closed (on Android, applications are actually never closed).</span></span> <span data-ttu-id="84c31-142">Így *magas* toocall hello ajánlott `startActivity()` hello metódust `onResume` a visszahívási *minden* tevékenységek, és hello `endActivity()` hello metódus `onPause()` a visszahívási *összes* a tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="84c31-142">Thus, it is *HIGHLY* recommended toocall hello `startActivity()` method in hello `onResume` callback of *ALL* your activities, and hello `endActivity()` method in hello `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="84c31-143">Ez a hello csak úgy toobe meg arról, hogy a munkamenetek szivárgását nem lehet.</span><span class="sxs-lookup"><span data-stu-id="84c31-143">This is hello only way toobe sure that sessions will not be leaked.</span></span> <span data-ttu-id="84c31-144">Egy munkamenet kiszivárgott, ha hello Engagement szolgáltatás soha nem leválasztja a hello Engagement háttérrendszerből (mivel hello szolgáltatás továbbra is csatlakoztatva marad mindaddig, amíg függőben egy munkamenet).</span><span class="sxs-lookup"><span data-stu-id="84c31-144">If a session is leaked, hello Engagement service will never disconnect from hello Engagement backend (since hello service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="84c31-145">Például:</span><span class="sxs-lookup"><span data-stu-id="84c31-145">Here is an example:</span></span>

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

<span data-ttu-id="84c31-146">A Példa nagyon hasonló toohello `EngagementActivity` osztály és annak változataihoz, amelynek forráskód megadott hello `src` mappát.</span><span class="sxs-lookup"><span data-stu-id="84c31-146">This example very similiar toohello `EngagementActivity` class and its variants, whose source code is provided in hello `src` folder.</span></span>

## <a name="test"></a><span data-ttu-id="84c31-147">Tesztelés</span><span class="sxs-lookup"><span data-stu-id="84c31-147">Test</span></span>
<span data-ttu-id="84c31-148">Most ellenőrizze, hogy az integráció a mobilalkalmazás emulátor vagy eszköz a és ellenőrzése, hogy a munkamenet hello figyelő lapon történő regisztrálást.</span><span class="sxs-lookup"><span data-stu-id="84c31-148">Now please verify your integration by running your mobile app in an emulator or device and verifying that it registers a session on hello Monitor tab.</span></span>

<span data-ttu-id="84c31-149">hello következő szakaszokban opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="84c31-149">hello next sections are optional.</span></span>

## <a name="location-reporting"></a><span data-ttu-id="84c31-150">Helyi jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="84c31-150">Location reporting</span></span>
<span data-ttu-id="84c31-151">Ha azt szeretné, hogy a helyek toobe jelentett, szüksége tooadd konfigurációs néhány sornyi (közötti hello `<application>` és `</application>` címkék).</span><span class="sxs-lookup"><span data-stu-id="84c31-151">If you want locations toobe reported, you need tooadd a few lines of configuration (between hello `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="84c31-152">Helymeghatározást</span><span class="sxs-lookup"><span data-stu-id="84c31-152">Lazy area location reporting</span></span>
<span data-ttu-id="84c31-153">Helymeghatározást lehetővé teszi, hogy tooreport hello ország, valamint régió és Helység társított toodevices.</span><span class="sxs-lookup"><span data-stu-id="84c31-153">Lazy area location reporting allows tooreport hello country, region and locality associated toodevices.</span></span> <span data-ttu-id="84c31-154">A hely jelentéskészítési típusú csak használ a hálózati helyek (cella azonosító vagy Wi-Fi alapján).</span><span class="sxs-lookup"><span data-stu-id="84c31-154">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="84c31-155">hello eszköz terület munkamenetenként legfeljebb egyszer jelenti.</span><span class="sxs-lookup"><span data-stu-id="84c31-155">hello device area is reported at most once per session.</span></span> <span data-ttu-id="84c31-156">hello GPS a rendszer nem használja, és ezáltal az ilyen típusú hely jelentés nagyon kevés (nem toosay nem) hello akkumulátor hatása.</span><span class="sxs-lookup"><span data-stu-id="84c31-156">hello GPS is never used, and thus this type of location report has very few (not toosay no) impact on hello battery.</span></span>

<span data-ttu-id="84c31-157">Jelentett területek a következők felhasználók, munkamenetek, kapcsolódó események és hibák földrajzi statisztikája használt toocompute.</span><span class="sxs-lookup"><span data-stu-id="84c31-157">Reported areas are used toocompute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="84c31-158">Ezek használhatók a Reach-kampányokat feltételként.</span><span class="sxs-lookup"><span data-stu-id="84c31-158">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="84c31-159">tooenable Lusta terület helye reporting hello konfigurációs korábban az itt ismertetett eljárás segítségével is elvégezhető:</span><span class="sxs-lookup"><span data-stu-id="84c31-159">tooenable lazy area location reporting, you can do it by using hello configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="84c31-160">Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="84c31-160">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="84c31-161">Tovább használhatja vagy ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84c31-161">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="84c31-162">Valós idejű hely jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="84c31-162">Real time location reporting</span></span>
<span data-ttu-id="84c31-163">Valós idejű hely jelentéskészítési lehetővé teszi, hogy tooreport hello szélességi és hosszúsági társított toodevices.</span><span class="sxs-lookup"><span data-stu-id="84c31-163">Real time location reporting allows tooreport hello latitude and longitude associated toodevices.</span></span> <span data-ttu-id="84c31-164">Alapértelmezés szerint a hely jelentéskészítési csak használja a hálózati helyek (cella azonosító vagy Wi-Fi alapján), és hello reporting csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="84c31-164">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and hello reporting is only active when hello application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="84c31-165">Valós idejű helyek *nem* toocompute statisztika használt.</span><span class="sxs-lookup"><span data-stu-id="84c31-165">Real time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="84c31-166">A csak célja a geokerítések valós idejű tooallow hello használata \<Reach-célközönségre-geokerítések\> Reach-kampányokat a feltételnek.</span><span class="sxs-lookup"><span data-stu-id="84c31-166">Their only purpose is tooallow hello use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="84c31-167">tooenable valós idejű helye reporting hello konfigurációs korábban az itt ismertetett eljárás segítségével is elvégezhető:</span><span class="sxs-lookup"><span data-stu-id="84c31-167">tooenable real time location reporting, you can do it by using hello configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="84c31-168">Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="84c31-168">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="84c31-169">Tovább használhatja vagy ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84c31-169">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

#### <a name="gps-based-reporting"></a><span data-ttu-id="84c31-170">GPS-alapú jelentés</span><span class="sxs-lookup"><span data-stu-id="84c31-170">GPS based reporting</span></span>
<span data-ttu-id="84c31-171">Alapértelmezés szerint valós idejű helyét jelentő csak funkció alapú hálózati helyek.</span><span class="sxs-lookup"><span data-stu-id="84c31-171">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="84c31-172">tooenable hello GPS alapuló (amelyek pontos sokkal) helyek, hello konfigurációs objektum használja:</span><span class="sxs-lookup"><span data-stu-id="84c31-172">tooenable hello use of GPS based locations (which are far more precise), use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="84c31-173">Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="84c31-173">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="84c31-174">Háttér-jelentés</span><span class="sxs-lookup"><span data-stu-id="84c31-174">Background reporting</span></span>
<span data-ttu-id="84c31-175">Alapértelmezés szerint valós idejű hely jelentéskészítési az csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="84c31-175">By default, real time location reporting is only active when hello application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="84c31-176">a háttérben, használjon hello konfigurációs objektum is reporting tooenable hello:</span><span class="sxs-lookup"><span data-stu-id="84c31-176">tooenable hello reporting also in background, use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="84c31-177">Hello alkalmazás fut a háttérben, amikor csak alapú hálózati helyek küld jelentést, még akkor is, ha engedélyezte a hello GPS.</span><span class="sxs-lookup"><span data-stu-id="84c31-177">When hello application runs in background, only network based locations are reported, even if you enabled hello GPS.</span></span>
> 
> 

<span data-ttu-id="84c31-178">hello háttér hely jelentés le lesz állítva, ha hello felhasználó az eszköz újraindul, az automatikus újraindítás rendszerindítás toomake is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="84c31-178">hello background location report will be stopped if hello user reboots its device, you can add this toomake it automatically restart at boot time:</span></span>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

<span data-ttu-id="84c31-179">Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="84c31-179">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a><span data-ttu-id="84c31-180">Android M-engedélyek</span><span class="sxs-lookup"><span data-stu-id="84c31-180">Android M permissions</span></span>
<span data-ttu-id="84c31-181">Az Android M-től kezdődően egyes engedélyek futásidőben felügyelt, és felhasználói jóváhagyás szükséges.</span><span class="sxs-lookup"><span data-stu-id="84c31-181">Starting with Android M, some permissions are managed at runtime and needs user approval.</span></span>

<span data-ttu-id="84c31-182">hello futásidejű engedélyek ki lesz kapcsolva a új alkalmazások alapértelmezés szerint ha Android API-szintet 23 célozhat meg.</span><span class="sxs-lookup"><span data-stu-id="84c31-182">hello runtime permissions will be turned off by default for new app installations if you target Android API level 23.</span></span> <span data-ttu-id="84c31-183">Ellenkező esetben azt lesz kapcsolva alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="84c31-183">Otherwise it will be turned on by default.</span></span>

<span data-ttu-id="84c31-184">hello felhasználói is engedélyezi/letiltja ezeket az engedélyeket hello eszköz beállítások menüjében.</span><span class="sxs-lookup"><span data-stu-id="84c31-184">hello user can enable/disable those permissions from hello device settings menu.</span></span> <span data-ttu-id="84c31-185">Engedélyek rendszer menüből kikapcsolása használhatatlanná teszi a háttérfolyamatot hello alkalmazás, ez a rendszer viselkedését, és nincs hatással van a háttérben képességét tooreceive leküldéses.</span><span class="sxs-lookup"><span data-stu-id="84c31-185">Turning off permissions from system menu kills background processes of hello application, this is a system behavior and has no impact on ability tooreceive push in background.</span></span>

<span data-ttu-id="84c31-186">A Mobile Engagement hello környezetében futásidőben jóváhagyást igénylő hello engedélyek a következők:</span><span class="sxs-lookup"><span data-stu-id="84c31-186">In hello context of Mobile Engagement, hello permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* <span data-ttu-id="84c31-187">`WRITE_EXTERNAL_STORAGE`(csak akkor, ha ez egy 23 Android API-szintet célzó)</span><span class="sxs-lookup"><span data-stu-id="84c31-187">`WRITE_EXTERNAL_STORAGE` (only when targeting Android API level 23 for this one)</span></span>

<span data-ttu-id="84c31-188">hello külső tárhelyen csak Reach nagy vonalakban tekinti a szolgáltatás használatos.</span><span class="sxs-lookup"><span data-stu-id="84c31-188">hello external storage is used only for Reach big picture feature.</span></span> <span data-ttu-id="84c31-189">Ha talál kérni a felhasználókat, az engedély toobe zavaró, eltávolíthatja azt ha azt csak a Mobile Engagement, de a nagy vonalakban tekinti funkció letiltása hello költségekkel.</span><span class="sxs-lookup"><span data-stu-id="84c31-189">If you find asking users this permission toobe disruptive, you can remove it if you used it only for Mobile Engagement but at hello cost of disabling big picture feature.</span></span>

<span data-ttu-id="84c31-190">Hello hely szolgáltatások egy szabványos rendszer párbeszédpanelen engedélyek toouser igényeljen.</span><span class="sxs-lookup"><span data-stu-id="84c31-190">For hello location features, you should request permissions toouser using a standard system dialog.</span></span> <span data-ttu-id="84c31-191">Hello felhasználói jóvá, ha szüksége van-e tootell ``EngagementAgent`` tootake (egyéb hello módosítás felül lesz feldolgozott hello következő hello felhasználó elindítja hello alkalmazása) valós idejű figyelembe módosítani.</span><span class="sxs-lookup"><span data-stu-id="84c31-191">If hello user approves, you need tootell ``EngagementAgent`` tootake that change into account in real time (otherwise hello change will be processed hello next time hello user launches hello application).</span></span>

<span data-ttu-id="84c31-192">Íme egy kódot minta toouse toorequest Alkalmazásengedélyek és előre hello eredmény tevékenységet, ha pozitív túl``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="84c31-192">Here is a code sample toouse in an activity of your application toorequest permissions and forward hello result if positive too``EngagementAgent``:</span></span>

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want tookeep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a><span data-ttu-id="84c31-193">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="84c31-193">Advanced reporting</span></span>
<span data-ttu-id="84c31-194">Nem kötelező, ha tooreport adott eseményeket, hibákat és feladatok, kell toouse hello Engagement API keresztül hello hello módszerek `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="84c31-194">Optionally, if you want tooreport application specific events, errors and jobs, you need toouse hello Engagement API through hello methods of hello `EngagementAgent` class.</span></span> <span data-ttu-id="84c31-195">Ez az osztály egy objektumának lehet hívó hello által retreived `EngagementAgent.getInstance()` statikus metódus.</span><span class="sxs-lookup"><span data-stu-id="84c31-195">An object of this class can be retreived by calling hello `EngagementAgent.getInstance()` static method.</span></span>

<span data-ttu-id="84c31-196">hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók és részleteit a hello hogyan tooUse az Engagement API-t az Android (valamint hello hello műszaki dokumentációját az `EngagementAgent` osztály).</span><span class="sxs-lookup"><span data-stu-id="84c31-196">hello Engagement API allows toouse all of Engagement's advanced capabilities and is detailed in hello How tooUse the Engagement API on Android (as well as in hello technical documentation of hello `EngagementAgent` class).</span></span>

## <a name="advanced-configuration-in-androidmanifestxml"></a><span data-ttu-id="84c31-197">Speciális konfigurációs (az AndroidManifest.xml)</span><span class="sxs-lookup"><span data-stu-id="84c31-197">Advanced configuration (in AndroidManifest.xml)</span></span>
### <a name="wake-locks"></a><span data-ttu-id="84c31-198">Ébresztési zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="84c31-198">Wake locks</span></span>
<span data-ttu-id="84c31-199">Ha azt szeretné, hogy statisztika küldött Wi-Fi használata esetén valós időben, vagy le van tiltva üdvözlő képernyőt toobe, adja hozzá a következő opcionális engedély hello:</span><span class="sxs-lookup"><span data-stu-id="84c31-199">If you want toobe sure that statistics are sent in real time when using Wifi or when hello screen is off, add hello following optional permission:</span></span>

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a><span data-ttu-id="84c31-200">Összeomlási jelentést</span><span class="sxs-lookup"><span data-stu-id="84c31-200">Crash report</span></span>
<span data-ttu-id="84c31-201">Ha azt szeretné, hogy toodisable összeomlási jelentések, adja hozzá ezt a (közötti hello `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="84c31-201">If you want toodisable crash reports, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="84c31-202">Küszöbérték-kapacitásnövelés</span><span class="sxs-lookup"><span data-stu-id="84c31-202">Burst threshold</span></span>
<span data-ttu-id="84c31-203">Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="84c31-203">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="84c31-204">Ha az alkalmazás naplók nagyon gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (Ez a lehetőség hello "kapacitásnövelés mód").</span><span class="sxs-lookup"><span data-stu-id="84c31-204">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello "burst mode").</span></span> <span data-ttu-id="84c31-205">toodo Igen, adja hozzá ezt a (közötti hello `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="84c31-205">toodo so, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="84c31-206">hello kapacitásnövelés mód némileg növeli hello akkumulátor élettartama hello Engagement figyelő azonban hatással van: az összes munkamenetek és feladatok is lesznek kerekített toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="84c31-206">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="84c31-207">Az ajánlott toouse egy mint 30000 (30s) kapacitásnövelés küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="84c31-207">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="84c31-208">Munkamenet időkorlátja</span><span class="sxs-lookup"><span data-stu-id="84c31-208">Session timeout</span></span>
<span data-ttu-id="84c31-209">Alapértelmezés szerint a munkamenet befejeződött a 10 egység az utolsó tevékenység (amely általában hello lenyomásával Home esetén, vagy a kulcs, biztonsági beállítás hello üresjárati telefonos vagy egy másik alkalmazásban átugorja) hello vége után.</span><span class="sxs-lookup"><span data-stu-id="84c31-209">By default, a session is ended 10s after hello end of its last activity (which usually occurs by pressing hello Home or Back key, by setting hello phone idle or by jumping into another application).</span></span> <span data-ttu-id="84c31-210">Ez a tooavoid munkamenet valószínűségét minden alkalommal hello felhasználói lépjen ki, és térjen vissza a toohello alkalmazás nagyon gyorsan (amely a hiba akkor fordulhat elő ő átvételéhez egy kép, amikor ellenőrizheti egy értesítés, stb.).</span><span class="sxs-lookup"><span data-stu-id="84c31-210">This is tooavoid a session split each time hello user exit and return toohello application very quickly (which can happen when he pick up a image, check a notification, etc.).</span></span> <span data-ttu-id="84c31-211">Érdemes lehet toomodify ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="84c31-211">You may want toomodify this parameter.</span></span> <span data-ttu-id="84c31-212">toodo Igen, adja hozzá ezt a (közötti hello `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="84c31-212">toodo so, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="84c31-213">Naplózási jelentések letiltásához</span><span class="sxs-lookup"><span data-stu-id="84c31-213">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="84c31-214">Metódushívások használatával</span><span class="sxs-lookup"><span data-stu-id="84c31-214">Using a method call</span></span>
<span data-ttu-id="84c31-215">Ha azt szeretné, hogy a naplók küldése Engagement toostop, hívása:</span><span class="sxs-lookup"><span data-stu-id="84c31-215">If you want Engagement toostop sending logs, you can call:</span></span>

            EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="84c31-216">Ez a hívás az állandó: egy megosztott beállításokat tartalmazó fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="84c31-216">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="84c31-217">Ha Engagement aktív Ez a függvény hívásakor, 1 perces hello szolgáltatás toostop is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="84c31-217">If Engagement is active when you call this function, it may take 1 minute for hello service toostop.</span></span> <span data-ttu-id="84c31-218">Azonban ez nem indítsa el a hello szolgáltatást minden hello hello alkalmazás következő indításakor.</span><span class="sxs-lookup"><span data-stu-id="84c31-218">However it won't launch hello service at all hello next time you launch hello application.</span></span>

<span data-ttu-id="84c31-219">Engedélyezheti újra reporting működik az azonos hello meghívásával napló `true`.</span><span class="sxs-lookup"><span data-stu-id="84c31-219">You can enable log reporting again by calling hello same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="84c31-220">Integráció saját`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="84c31-220">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="84c31-221">Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="84c31-221">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="84c31-222">A beállítások fájlt (hello kívánt mód) Engagement toouse konfigurálhatja a hello `AndroidManifest.xml` fájlt `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="84c31-222">You can configure Engagement toouse your preferences file (with hello desired mode) in hello `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="84c31-223">Hello `engagement:agent:settings:name` kulcsa használt toodefine hello hello közös beállításokat fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="84c31-223">hello `engagement:agent:settings:name` key is used toodefine hello name of hello shared preferences file.</span></span>
* <span data-ttu-id="84c31-224">Hello `engagement:agent:settings:mode` kulcsa használt toodefine hello mód hello közös beállításokat fájl, hello használja ugyanazt a módot és a a `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="84c31-224">hello `engagement:agent:settings:mode` key is used toodefine hello mode of hello shared preferences file, you should use hello same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="84c31-225">hello módban kell átadni egy számot: használatakor a konstans jelzők kombinációja a kódban, ellenőrizze a hello teljes értékét.</span><span class="sxs-lookup"><span data-stu-id="84c31-225">hello mode must be passed as a number: if you are using a combination of constant flags in your code, check hello total value.</span></span>

<span data-ttu-id="84c31-226">Bevonási mindig használjon hello `engagement:key` belül hello beállításokat tartalmazó fájlt ezzel a beállítással kezelésére szolgáló logikai kulcs.</span><span class="sxs-lookup"><span data-stu-id="84c31-226">Engagement always use hello `engagement:key` boolean key within hello preferences file for managing this setting.</span></span>

<span data-ttu-id="84c31-227">Példa következő hello `AndroidManifest.xml` jeleníti meg az alapértelmezett értékek hello:</span><span class="sxs-lookup"><span data-stu-id="84c31-227">hello following example of `AndroidManifest.xml` shows hello default values:</span></span>

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

<span data-ttu-id="84c31-228">Ezután hozzáadhatja a `CheckBoxPreference` a preferencia elrendezésben például hello egyet a következő:</span><span class="sxs-lookup"><span data-stu-id="84c31-228">Then you can add a `CheckBoxPreference` in your preference layout like hello following one:</span></span>

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
