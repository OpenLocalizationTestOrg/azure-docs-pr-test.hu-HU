---
title: "Az Azure Mobile Engagement Android SDK-integráció"
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
ms.openlocfilehash: 35bd92e52b7a02f58620a03156902f9f91be57ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-engagement-on-android"></a><span data-ttu-id="4810e-103">Hogyan integrálható az Engagement Android rendszeren</span><span class="sxs-lookup"><span data-stu-id="4810e-103">How to Integrate Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4810e-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="4810e-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="4810e-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="4810e-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="4810e-106">iOS</span><span class="sxs-lookup"><span data-stu-id="4810e-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="4810e-107">Android</span><span class="sxs-lookup"><span data-stu-id="4810e-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="4810e-108">Ez az eljárás ismerteti a legegyszerűbben úgy Engagement elemzés és Monitorozási funkciók az Android alkalmazás aktiválása.</span><span class="sxs-lookup"><span data-stu-id="4810e-108">This procedure describes the simplest way to activate Engagement's Analytics and Monitoring functions in your Android application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4810e-109">A minimális Android SDK API-szintet kell lennie, 10 vagy újabb rendszer (Android 2.3.3 vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="4810e-109">Your minimum Android SDK API level must be 10 or higher (Android 2.3.3 or higher).</span></span>
> 
> 

<span data-ttu-id="4810e-110">Az alábbi lépéseket kell elegendő a aktiválja a jelentés az összes statisztikai adatok felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals kiszámításához szükséges naplók.</span><span class="sxs-lookup"><span data-stu-id="4810e-110">The following steps are enough to activates the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="4810e-111">A jelentés más hasonló eseményeket, hibákat és feladatok statisztika kiszámításához szükséges naplók az Engagement API segítségével manuálisan hajtható végre (lásd: [használata a speciális a Mobile Engagement az Android API szerinti címkézését](mobile-engagement-android-use-engagement-api.md) mivel ezek a statisztikák a függő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4810e-111">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API (see [How to use the advanced Mobile Engagement tagging API in your Android](mobile-engagement-android-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a><span data-ttu-id="4810e-112">Az Engagement SDK-t és a szolgáltatás beágyazása az Android-projekt</span><span class="sxs-lookup"><span data-stu-id="4810e-112">Embed the Engagement SDK and service into your Android project</span></span>
<span data-ttu-id="4810e-113">Az Android SDK letöltése [Itt](https://aka.ms/vq9mfn) beolvasása `mobile-engagement-VERSION.jar` , és be azokat a `libs` az Android-projekt mappájából (hozza létre a függvénytárak mappát, ha még nem létezik).</span><span class="sxs-lookup"><span data-stu-id="4810e-113">Download the Android SDK from [here](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` and put them into the `libs` folder of your Android project (create the libs folder if it does not exist yet).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4810e-114">Ha a ProGuard alkalmazáscsomag, szeretne rögzíteni bizonyos osztályokat.</span><span class="sxs-lookup"><span data-stu-id="4810e-114">If you build your application package with ProGuard, you need to keep some classes.</span></span> <span data-ttu-id="4810e-115">A következő konfigurációs részlet használhatja:</span><span class="sxs-lookup"><span data-stu-id="4810e-115">You can use the following configuration snippet:</span></span>
> 
> <span data-ttu-id="4810e-116">-nyilvános osztály tartsa * android.os.IInterface bővíti-osztály com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {megtartása</span><span class="sxs-lookup"><span data-stu-id="4810e-116">-keep public class * extends android.os.IInterface -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {</span></span>
> 
> <span data-ttu-id="4810e-117"><methods>; }</span><span class="sxs-lookup"><span data-stu-id="4810e-117"><methods>; }</span></span>
> 
> 

<span data-ttu-id="4810e-118">Adja meg a bevonási kapcsolati karakterlánc a következő metódus meghívásával indítója tevékenységben:</span><span class="sxs-lookup"><span data-stu-id="4810e-118">Specify your Engagement connection string by calling the following method in the launcher activity:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="4810e-119">A kapcsolati karakterlánc az alkalmazás Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4810e-119">The connection string for your application is displayed on Azure Portal.</span></span>

* <span data-ttu-id="4810e-120">Ha hiányoznak, adja meg a következő Android engedélyeket (előtt a `<application>` címke):</span><span class="sxs-lookup"><span data-stu-id="4810e-120">If missing, add the following Android permissions (before the `<application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* <span data-ttu-id="4810e-121">Adja hozzá a következő szakaszt (között a `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="4810e-121">Add the following section (between the `<application>` and `</application>` tags):</span></span>
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* <span data-ttu-id="4810e-122">Változás `<Your application name>` az alkalmazás nevével.</span><span class="sxs-lookup"><span data-stu-id="4810e-122">Change `<Your application name>` by the name of your application.</span></span>

> [!TIP]
> <span data-ttu-id="4810e-123">A `android:label` attribútum lehetővé teszi, hogy a szolgáltatás neve a bevonási kiválaszthatja azt a telefont "Szolgáltatásokat futtató" képernyőjén a végfelhasználók számára megjelenik.</span><span class="sxs-lookup"><span data-stu-id="4810e-123">The `android:label` attribute allows you to choose the name of the Engagement service as it will appear to the end-users in the "Running services" screen of their phone.</span></span> <span data-ttu-id="4810e-124">Javasoljuk, hogy ez az attribútum beállítása `"<Your application name>Service"` (pl. `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="4810e-124">It is recommended to set this attribute to `"<Your application name>Service"` (e.g. `"AcmeFunGameService"`).</span></span>
> 
> 

<span data-ttu-id="4810e-125">Adja meg a `android:process` attribútum biztosítja, hogy a bevonási szolgáltatás a saját fájlleírókból (Engagement ugyanabban a folyamatban, az alkalmazás biztosítják a fő/felhasználói felület szálán potenciálisan kevésbé rugalmas) fog futni.</span><span class="sxs-lookup"><span data-stu-id="4810e-125">Specifying the `android:process` attribute ensures that the Engagement service will run in its own process (running Engagement in the same process as your application will make your main/UI thread potentially less responsive).</span></span>

> [!NOTE]
> <span data-ttu-id="4810e-126">A kód helyez `Application.onCreate()` és más alkalmazás visszahívások fog futni az alkalmazás folyamatok, többek között a bevonási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4810e-126">Any code you place in `Application.onCreate()` and other application callbacks will be run for all your application's processes, including the Engagement service.</span></span> <span data-ttu-id="4810e-127">(Például nem szükséges memória-kiosztásokat és a bevonási folyamat, ismétlődő szórási fogadók vagy szolgáltatások) nemkívánatos hatásai lehetnek.</span><span class="sxs-lookup"><span data-stu-id="4810e-127">It may have unwanted side effects (like unneeded memory allocations and threads in the Engagement's process, duplicate broadcast receivers or services).</span></span>
> 
> 

<span data-ttu-id="4810e-128">Felülbírálásakor `Application.onCreate()`, a következő kódrészletet hozzáadása elején javasoljuk a `Application.onCreate()` függvény:</span><span class="sxs-lookup"><span data-stu-id="4810e-128">If you override `Application.onCreate()`, it's recommended to add the following code snippet at the beginning of your `Application.onCreate()` function:</span></span>

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

<span data-ttu-id="4810e-129">Ugyanezt megteheti `Application.onTerminate()`, `Application.onLowMemory()` és `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="4810e-129">You can do the same thing for `Application.onTerminate()`, `Application.onLowMemory()` and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="4810e-130">Emellett kibővítheti `EngagementApplication` helyett kiterjesztése `Application`: a visszahívás `Application.onCreate()` a folyamat ellenőrzés és hívások `Application.onApplicationProcessCreate()` csak ha az aktuális folyamat nem azt az Engagement szolgáltatást tartalmazó, ugyanazok a szabályok vonatkoznak a más visszahívások.</span><span class="sxs-lookup"><span data-stu-id="4810e-130">You can also extend `EngagementApplication` instead of extending `Application`: the callback `Application.onCreate()` does the process check and calls `Application.onApplicationProcessCreate()` only if the current process is not the one hosting the Engagement service, the same rules apply for the other callbacks.</span></span>

## <a name="basic-reporting"></a><span data-ttu-id="4810e-131">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="4810e-131">Basic reporting</span></span>
### <a name="recommended-method-overload-your-activity-classes"></a><span data-ttu-id="4810e-132">Ajánlott módszer: túlterhelés a `Activity` osztályok</span><span class="sxs-lookup"><span data-stu-id="4810e-132">Recommended method: overload your `Activity` classes</span></span>
<span data-ttu-id="4810e-133">Ahhoz, hogy aktiválja a jelentés minden, a felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a műszaki statisztika számítási Engagement által igényelt naplók, akkor csak kell végeznie az összes a `*Activity` alosztályokat öröklik a megfelelő `Engagement*Activity` (pl. osztályok Ha kiterjeszti az örökölt tevékenység `ListActivity`, ellenőrizze az általa bővített `EngagementListActivity`).</span><span class="sxs-lookup"><span data-stu-id="4810e-133">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you just have to make all your `*Activity` sub-classes inherit from the corresponding `Engagement*Activity` classes (e.g. if your legacy activity extends `ListActivity`, make it extends `EngagementListActivity`).</span></span>

<span data-ttu-id="4810e-134">**Nélkül Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4810e-134">**Without Engagement :**</span></span>

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

<span data-ttu-id="4810e-135">**Az Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4810e-135">**With Engagement :**</span></span>

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
> <span data-ttu-id="4810e-136">Használata esetén `EngagementListActivity` vagy `EngagementExpandableListActivity`, győződjön meg arról, hogy semmilyen hívásban való `requestWindowFeature(...);` hívása előtt történik `super.onCreate(...);`, ellenkező esetben a összeomlási történik.</span><span class="sxs-lookup"><span data-stu-id="4810e-136">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call to `requestWindowFeature(...);` is made before the call to `super.onCreate(...);`, otherwise a crash will occur.</span></span>
> 
> 

<span data-ttu-id="4810e-137">Ezeket az osztályokat is megtalálhatja a `src` mappa, és a projektben másolja őket.</span><span class="sxs-lookup"><span data-stu-id="4810e-137">You can find these classes in the `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="4810e-138">Az osztályok szerepelnek a **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="4810e-138">The classes are also in the **JavaDoc**.</span></span>

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="4810e-139">Másodlagos módszer: hívja `startActivity()` és `endActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="4810e-139">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="4810e-140">Ha nem, vagy nem szeretné, hogy túlterhelés a `Activity` osztályok, ehelyett elindításához és a tevékenységek befejezése meghívásával `EngagementAgent`tartozó módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="4810e-140">If you cannot or do not want to overload your `Activity` classes, you can instead start and end your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4810e-141">Az Android SDK soha nem hívja a `endActivity()` metódust, akkor is, ha az alkalmazás bezárása (Android, az alkalmazások valójában soha nem Lezáródnak).</span><span class="sxs-lookup"><span data-stu-id="4810e-141">The Android SDK never calls the `endActivity()` method, even when the application is closed (on Android, applications are actually never closed).</span></span> <span data-ttu-id="4810e-142">Tehát, *magas* hívására ajánlott a `startActivity()` metódust a `onResume` a visszahívási *összes* a tevékenységeket, és a `endActivity()` metódus a a `onPause()` a visszahívási *összes* a tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="4810e-142">Thus, it is *HIGHLY* recommended to call the `startActivity()` method in the `onResume` callback of *ALL* your activities, and the `endActivity()` method in the `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="4810e-143">Ez az az egyetlen lehetőség, hogy a munkamenetek szivárgását nem lehet.</span><span class="sxs-lookup"><span data-stu-id="4810e-143">This is the only way to be sure that sessions will not be leaked.</span></span> <span data-ttu-id="4810e-144">Munkamenet kiszivárgott, ha a bevonási szolgáltatás soha nem leválasztja az Engagement háttérrendszeréhez a (mivel a szolgáltatás továbbra is csatlakoztatva marad mindaddig, amíg függőben egy munkamenet).</span><span class="sxs-lookup"><span data-stu-id="4810e-144">If a session is leaked, the Engagement service will never disconnect from the Engagement backend (since the service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="4810e-145">Például:</span><span class="sxs-lookup"><span data-stu-id="4810e-145">Here is an example:</span></span>

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

<span data-ttu-id="4810e-146">Ez a Példa nagyon hasonlít a `EngagementActivity` osztály és annak változataihoz, amelynek forráskódban szerepel a `src` mappát.</span><span class="sxs-lookup"><span data-stu-id="4810e-146">This example very similiar to the `EngagementActivity` class and its variants, whose source code is provided in the `src` folder.</span></span>

## <a name="test"></a><span data-ttu-id="4810e-147">Tesztelés</span><span class="sxs-lookup"><span data-stu-id="4810e-147">Test</span></span>
<span data-ttu-id="4810e-148">Most ellenőrizze, hogy az integráció a mobilalkalmazás emulátor vagy eszköz a és ellenőrzése, hogy a munkamenet a Monitor lapon történő regisztrálást.</span><span class="sxs-lookup"><span data-stu-id="4810e-148">Now please verify your integration by running your mobile app in an emulator or device and verifying that it registers a session on the Monitor tab.</span></span>

<span data-ttu-id="4810e-149">A következő szakaszokban opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="4810e-149">The next sections are optional.</span></span>

## <a name="location-reporting"></a><span data-ttu-id="4810e-150">Helyi jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="4810e-150">Location reporting</span></span>
<span data-ttu-id="4810e-151">Ha azt szeretné, hogy a helyek jelentendő, kell hozzáadnia a konfigurációs néhány sornyi (között a `<application>` és `</application>` címkék).</span><span class="sxs-lookup"><span data-stu-id="4810e-151">If you want locations to be reported, you need to add a few lines of configuration (between the `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="4810e-152">Helymeghatározást</span><span class="sxs-lookup"><span data-stu-id="4810e-152">Lazy area location reporting</span></span>
<span data-ttu-id="4810e-153">Helymeghatározást lehetővé teszi, hogy az ország, valamint régió és Helység szerint társított eszközök jelentését.</span><span class="sxs-lookup"><span data-stu-id="4810e-153">Lazy area location reporting allows to report the country, region and locality associated to devices.</span></span> <span data-ttu-id="4810e-154">A hely jelentéskészítési típusú csak használ a hálózati helyek (cella azonosító vagy Wi-Fi alapján).</span><span class="sxs-lookup"><span data-stu-id="4810e-154">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="4810e-155">Az eszköz területen munkamenetenként legfeljebb egyszer jelenti.</span><span class="sxs-lookup"><span data-stu-id="4810e-155">The device area is reported at most once per session.</span></span> <span data-ttu-id="4810e-156">A rendszer nem használja a GPS, és ezáltal helye a jelentés az ilyen típusú (nem a fel nem) nagyon kevés az akkumulátor hatással.</span><span class="sxs-lookup"><span data-stu-id="4810e-156">The GPS is never used, and thus this type of location report has very few (not to say no) impact on the battery.</span></span>

<span data-ttu-id="4810e-157">Jelentett területek segítségével számítási felhasználók, munkamenetek, kapcsolódó események és hibák földrajzi statisztikája.</span><span class="sxs-lookup"><span data-stu-id="4810e-157">Reported areas are used to compute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="4810e-158">Ezek használhatók a Reach-kampányokat feltételként.</span><span class="sxs-lookup"><span data-stu-id="4810e-158">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="4810e-159">Helymeghatározást engedélyezéséhez ezt megteheti a korábban az itt ismertetett eljárás konfigurációval:</span><span class="sxs-lookup"><span data-stu-id="4810e-159">To enable lazy area location reporting, you can do it by using the configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="4810e-160">Szükség vegye fel a következő engedélyt, ha hiányoznak:</span><span class="sxs-lookup"><span data-stu-id="4810e-160">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="4810e-161">Tovább használhatja vagy ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4810e-161">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="4810e-162">Valós idejű hely jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="4810e-162">Real time location reporting</span></span>
<span data-ttu-id="4810e-163">Valós idejű hely jelentéskészítési lehetővé teszi, hogy a szélességi és hosszúsági társított eszközök jelentését.</span><span class="sxs-lookup"><span data-stu-id="4810e-163">Real time location reporting allows to report the latitude and longitude associated to devices.</span></span> <span data-ttu-id="4810e-164">Alapértelmezés szerint a hely jelentéskészítési csak használja a hálózati helyek (cella azonosító vagy Wi-Fi alapján), és a jelentéskészítési csak aktív futtatásakor az alkalmazás az előtérben (azaz egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="4810e-164">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and the reporting is only active when the application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="4810e-165">Valós idejű helyek *nem* statisztika kiszámítására használt.</span><span class="sxs-lookup"><span data-stu-id="4810e-165">Real time locations are *NOT* used to compute statistics.</span></span> <span data-ttu-id="4810e-166">Az egyetlen célja, hogy a geokerítések valós idejű használatának engedélyezése \<Reach-célközönségre-geokerítések\> Reach-kampányokat a feltételnek.</span><span class="sxs-lookup"><span data-stu-id="4810e-166">Their only purpose is to allow the use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="4810e-167">Ahhoz, hogy a reporting valós idejű helyét, ezt megteheti a korábban az itt ismertetett eljárás konfigurációval:</span><span class="sxs-lookup"><span data-stu-id="4810e-167">To enable real time location reporting, you can do it by using the configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="4810e-168">Szükség vegye fel a következő engedélyt, ha hiányoznak:</span><span class="sxs-lookup"><span data-stu-id="4810e-168">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="4810e-169">Tovább használhatja vagy ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4810e-169">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

#### <a name="gps-based-reporting"></a><span data-ttu-id="4810e-170">GPS-alapú jelentés</span><span class="sxs-lookup"><span data-stu-id="4810e-170">GPS based reporting</span></span>
<span data-ttu-id="4810e-171">Alapértelmezés szerint valós idejű helyét jelentő csak funkció alapú hálózati helyek.</span><span class="sxs-lookup"><span data-stu-id="4810e-171">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="4810e-172">Alapú GPS-helyek (amelyek pontos sokkal) használatának engedélyezése a konfigurációs objektum használja:</span><span class="sxs-lookup"><span data-stu-id="4810e-172">To enable the use of GPS based locations (which are far more precise), use the configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="4810e-173">Szükség vegye fel a következő engedélyt, ha hiányoznak:</span><span class="sxs-lookup"><span data-stu-id="4810e-173">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="4810e-174">Háttér-jelentés</span><span class="sxs-lookup"><span data-stu-id="4810e-174">Background reporting</span></span>
<span data-ttu-id="4810e-175">Alapértelmezés szerint valós idejű hely jelentéskészítési akkor csak aktív, ha az alkalmazás futtatása az előtérben (azaz egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="4810e-175">By default, real time location reporting is only active when the application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="4810e-176">A jelentések engedélyezéséhez is háttérben a konfigurációs objektum használja:</span><span class="sxs-lookup"><span data-stu-id="4810e-176">To enable the reporting also in background, use the configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="4810e-177">Az alkalmazás futtatásakor a háttérben, csak a hálózati helyek alapú jelenti, még akkor is, ha engedélyezte a GPS.</span><span class="sxs-lookup"><span data-stu-id="4810e-177">When the application runs in background, only network based locations are reported, even if you enabled the GPS.</span></span>
> 
> 

<span data-ttu-id="4810e-178">A háttérben hely jelentés le lesz állítva, ha a felhasználó az eszköz újraindul, felvétele abba, hogy a rendszer indításakor automatikusan újraindul:</span><span class="sxs-lookup"><span data-stu-id="4810e-178">The background location report will be stopped if the user reboots its device, you can add this to make it automatically restart at boot time:</span></span>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

<span data-ttu-id="4810e-179">Szükség vegye fel a következő engedélyt, ha hiányoznak:</span><span class="sxs-lookup"><span data-stu-id="4810e-179">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a><span data-ttu-id="4810e-180">Android M-engedélyek</span><span class="sxs-lookup"><span data-stu-id="4810e-180">Android M permissions</span></span>
<span data-ttu-id="4810e-181">Az Android M-től kezdődően egyes engedélyek futásidőben felügyelt, és felhasználói jóváhagyás szükséges.</span><span class="sxs-lookup"><span data-stu-id="4810e-181">Starting with Android M, some permissions are managed at runtime and needs user approval.</span></span>

<span data-ttu-id="4810e-182">A futásidejű engedélyek ki lesz kapcsolva új alkalmazástelepítések alapértelmezés szerint ha Android API-szintet 23 célozhat meg.</span><span class="sxs-lookup"><span data-stu-id="4810e-182">The runtime permissions will be turned off by default for new app installations if you target Android API level 23.</span></span> <span data-ttu-id="4810e-183">Ellenkező esetben azt lesz kapcsolva alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="4810e-183">Otherwise it will be turned on by default.</span></span>

<span data-ttu-id="4810e-184">A felhasználó is engedélyezi/letiltja ezeket az engedélyeket az eszköz beállítások menüjében.</span><span class="sxs-lookup"><span data-stu-id="4810e-184">The user can enable/disable those permissions from the device settings menu.</span></span> <span data-ttu-id="4810e-185">Engedélyek rendszer menüből kikapcsolása használhatatlanná teszi az alkalmazás háttérfolyamatot, ez a rendszer viselkedését, és nincs hatással van a háttérben leküldéses fogadni.</span><span class="sxs-lookup"><span data-stu-id="4810e-185">Turning off permissions from system menu kills background processes of the application, this is a system behavior and has no impact on ability to receive push in background.</span></span>

<span data-ttu-id="4810e-186">A Mobile Engagement keretében a futási időben jóváhagyás szükséges engedélyeket a következők:</span><span class="sxs-lookup"><span data-stu-id="4810e-186">In the context of Mobile Engagement, the permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* <span data-ttu-id="4810e-187">`WRITE_EXTERNAL_STORAGE`(csak akkor, ha ez egy 23 Android API-szintet célzó)</span><span class="sxs-lookup"><span data-stu-id="4810e-187">`WRITE_EXTERNAL_STORAGE` (only when targeting Android API level 23 for this one)</span></span>

<span data-ttu-id="4810e-188">A külső tárolóeszközről csak Reach nagy vonalakban tekinti a szolgáltatás használatos.</span><span class="sxs-lookup"><span data-stu-id="4810e-188">The external storage is used only for Reach big picture feature.</span></span> <span data-ttu-id="4810e-189">Ha talál kérni a felhasználókat, amely zavart, eltávolíthatja, ha azt csak a Mobile Engagement de nagy vonalakban tekinti funkció letiltása.</span><span class="sxs-lookup"><span data-stu-id="4810e-189">If you find asking users this permission to be disruptive, you can remove it if you used it only for Mobile Engagement but at the cost of disabling big picture feature.</span></span>

<span data-ttu-id="4810e-190">Hely szolgáltatásainak igényeljen egy szabványos rendszer párbeszédpanelen felhasználói jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="4810e-190">For the location features, you should request permissions to user using a standard system dialog.</span></span> <span data-ttu-id="4810e-191">Ha a felhasználónak jóvá, kell-e adnia ``EngagementAgent`` figyelembe, hogy módosítás életbe valós időben (ellenkező esetben a módosítás dolgozza fel a rendszer a következő alkalommal a felhasználó elindítja az alkalmazást).</span><span class="sxs-lookup"><span data-stu-id="4810e-191">If the user approves, you need to tell ``EngagementAgent`` to take that change into account in real time (otherwise the change will be processed the next time the user launches the application).</span></span>

<span data-ttu-id="4810e-192">Ez a kódminta használata egy tevékenységben, az alkalmazás engedélyek kéréséhez és továbbítsa az eredmény, ha pozitív ``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="4810e-192">Here is a code sample to use in an activity of your application to request permissions and forward the result if positive to ``EngagementAgent``:</span></span>

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
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a><span data-ttu-id="4810e-193">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="4810e-193">Advanced reporting</span></span>
<span data-ttu-id="4810e-194">Szükség esetén szeretne jelentést készíteni az adott alkalmazásesemények, a hibák és a feladatok, ha szeretné használni az Engagement API-t a a módszerrel a `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="4810e-194">Optionally, if you want to report application specific events, errors and jobs, you need to use the Engagement API through the methods of the `EngagementAgent` class.</span></span> <span data-ttu-id="4810e-195">Ez az osztály egy objektumának lehet retreived meghívásával a `EngagementAgent.getInstance()` statikus metódus.</span><span class="sxs-lookup"><span data-stu-id="4810e-195">An object of this class can be retreived by calling the `EngagementAgent.getInstance()` static method.</span></span>

<span data-ttu-id="4810e-196">A bevonási API lehetővé teszi, hogy Engagement speciális funkciók használatára, és az útmutató részletesen az Engagement API-t használni az Android (valamint a műszaki dokumentációját az a `EngagementAgent` osztály).</span><span class="sxs-lookup"><span data-stu-id="4810e-196">The Engagement API allows to use all of Engagement's advanced capabilities and is detailed in the How to Use the Engagement API on Android (as well as in the technical documentation of the `EngagementAgent` class).</span></span>

## <a name="advanced-configuration-in-androidmanifestxml"></a><span data-ttu-id="4810e-197">Speciális konfigurációs (az AndroidManifest.xml)</span><span class="sxs-lookup"><span data-stu-id="4810e-197">Advanced configuration (in AndroidManifest.xml)</span></span>
### <a name="wake-locks"></a><span data-ttu-id="4810e-198">Ébresztési zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="4810e-198">Wake locks</span></span>
<span data-ttu-id="4810e-199">Ha azt szeretné, és ellenőrizze, hogy statisztika küldött Wi-Fi használata esetén valós időben, vagy le van tiltva a képernyő, adja meg a következő opcionális engedély:</span><span class="sxs-lookup"><span data-stu-id="4810e-199">If you want to be sure that statistics are sent in real time when using Wifi or when the screen is off, add the following optional permission:</span></span>

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a><span data-ttu-id="4810e-200">Összeomlási jelentést</span><span class="sxs-lookup"><span data-stu-id="4810e-200">Crash report</span></span>
<span data-ttu-id="4810e-201">Ha le szeretné tiltani a összeomlási jelentések, adja hozzá ezt a (között a `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="4810e-201">If you want to disable crash reports, add this (between the `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="4810e-202">Küszöbérték-kapacitásnövelés</span><span class="sxs-lookup"><span data-stu-id="4810e-202">Burst threshold</span></span>
<span data-ttu-id="4810e-203">Alapértelmezés szerint a bevonási service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="4810e-203">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="4810e-204">Ha az alkalmazás naplók nagyon gyakran, érdemes meghajtóin a naplókat, és jelentést őket egyszerre egy rendszeres időközönként alap (Ez a "kapacitásnövelés mód" nevezzük).</span><span class="sxs-lookup"><span data-stu-id="4810e-204">If your application reports logs very frequently, it is better to buffer the logs and to report them all at once on a regular time base (this is called the "burst mode").</span></span> <span data-ttu-id="4810e-205">Ehhez adja hozzá ezt a (között a `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="4810e-205">To do so, add this (between the `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="4810e-206">A kapacitásnövelés módja kissé az eszközakkumulátor élettartamának növelhető, de hatással van a bevonási figyelő: minden munkamenetek és feladatok időtartama a rendszer kerekíti a kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="4810e-206">The burst mode slightly increase the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration will be rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="4810e-207">Javasoljuk, hogy egy mint 30000 (30s) kapacitásnövelés küszöbértékkel.</span><span class="sxs-lookup"><span data-stu-id="4810e-207">It is recommended to use a burst threshold no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="4810e-208">Munkamenet időkorlátja</span><span class="sxs-lookup"><span data-stu-id="4810e-208">Session timeout</span></span>
<span data-ttu-id="4810e-209">Alapértelmezés szerint a munkamenet befejeződött a 10 egység az utolsó tevékenység (amely általában akkor fordul elő, a Home vagy biztonsági billentyű lenyomásával, úgy, hogy a telefon üresjárati vagy egy másik alkalmazásban átugorja) vége után.</span><span class="sxs-lookup"><span data-stu-id="4810e-209">By default, a session is ended 10s after the end of its last activity (which usually occurs by pressing the Home or Back key, by setting the phone idle or by jumping into another application).</span></span> <span data-ttu-id="4810e-210">Ez a munkamenet valószínűségét minden egyes alkalommal a felhasználó kilépési, és térjen vissza az alkalmazás nagyon gyorsan (amely a hiba akkor fordulhat elő ő átvételéhez egy kép, amikor ellenőrizheti egy értesítés, stb.) elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="4810e-210">This is to avoid a session split each time the user exit and return to the application very quickly (which can happen when he pick up a image, check a notification, etc.).</span></span> <span data-ttu-id="4810e-211">Érdemes lehet módosíthatják ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="4810e-211">You may want to modify this parameter.</span></span> <span data-ttu-id="4810e-212">Ehhez adja hozzá ezt a (között a `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="4810e-212">To do so, add this (between the `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="4810e-213">Naplózási jelentések letiltásához</span><span class="sxs-lookup"><span data-stu-id="4810e-213">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="4810e-214">Metódushívások használatával</span><span class="sxs-lookup"><span data-stu-id="4810e-214">Using a method call</span></span>
<span data-ttu-id="4810e-215">Ha azt szeretné, hogy leállítja a naplók küldése Engagement, hívása:</span><span class="sxs-lookup"><span data-stu-id="4810e-215">If you want Engagement to stop sending logs, you can call:</span></span>

            EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="4810e-216">Ez a hívás az állandó: egy megosztott beállításokat tartalmazó fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="4810e-216">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="4810e-217">Ha Engagement aktív Ez a függvény hívásakor, 1 perc, a szolgáltatás leállítása eltarthat.</span><span class="sxs-lookup"><span data-stu-id="4810e-217">If Engagement is active when you call this function, it may take 1 minute for the service to stop.</span></span> <span data-ttu-id="4810e-218">Azonban ez nem indítsa el a szolgáltatás minden legközelebb indítani az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4810e-218">However it won't launch the service at all the next time you launch the application.</span></span>

<span data-ttu-id="4810e-219">Engedélyezheti a reporting újra működik meghívásával napló `true`.</span><span class="sxs-lookup"><span data-stu-id="4810e-219">You can enable log reporting again by calling the same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="4810e-220">Integráció saját`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="4810e-220">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="4810e-221">Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="4810e-221">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="4810e-222">Konfigurálhatja a bevonási a beállításokat tartalmazó fájlt (a kívánt mód) a használatára a `AndroidManifest.xml` fájlt `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="4810e-222">You can configure Engagement to use your preferences file (with the desired mode) in the `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="4810e-223">A `engagement:agent:settings:name` kulcs használatával a közös beállításokat fájl nevének meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="4810e-223">The `engagement:agent:settings:name` key is used to define the name of the shared preferences file.</span></span>
* <span data-ttu-id="4810e-224">A `engagement:agent:settings:mode` kulcs használatával a közös beállításokat fájl módját határozza meg, mint a azonos módot kell használnia a `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="4810e-224">The `engagement:agent:settings:mode` key is used to define the mode of the shared preferences file, you should use the same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="4810e-225">Egy számot a módot kell átadni: használatakor a konstans jelzők kombinációja a kódban, ellenőrizze a teljes értékét.</span><span class="sxs-lookup"><span data-stu-id="4810e-225">The mode must be passed as a number: if you are using a combination of constant flags in your code, check the total value.</span></span>

<span data-ttu-id="4810e-226">Bevonási mindig használja a `engagement:key` logikai kulcs ezzel a beállítással kezelésére szolgáló beállítások fájlon belül.</span><span class="sxs-lookup"><span data-stu-id="4810e-226">Engagement always use the `engagement:key` boolean key within the preferences file for managing this setting.</span></span>

<span data-ttu-id="4810e-227">A következő példa `AndroidManifest.xml` megjeleníti az alapértelmezett értékeket:</span><span class="sxs-lookup"><span data-stu-id="4810e-227">The following example of `AndroidManifest.xml` shows the default values:</span></span>

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

<span data-ttu-id="4810e-228">Ezután hozzáadhatja a `CheckBoxPreference` a preferencia elrendezésben hasonló a következő:</span><span class="sxs-lookup"><span data-stu-id="4810e-228">Then you can add a `CheckBoxPreference` in your preference layout like the following one:</span></span>

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
