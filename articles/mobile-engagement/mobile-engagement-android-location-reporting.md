---
title: "aaaLocation jelentéskészítést az Azure Mobile Engagement Android SDK"
description: "Ismerteti, hogyan jelentéskészítést az Azure Mobile Engagement Android SDK tooconfigure helye"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: c2cb097df2a77bee2d56ffe9509dc116548db408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="dee40-103">Az Azure Mobile Engagement Android SDK Reporting helye</span><span class="sxs-lookup"><span data-stu-id="dee40-103">Location Reporting for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dee40-104">Android</span><span class="sxs-lookup"><span data-stu-id="dee40-104">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="dee40-105">Ez a témakör ismerteti, hogyan jelentéskészítést az Android-alkalmazás az toodo helyét.</span><span class="sxs-lookup"><span data-stu-id="dee40-105">This topic describes how toodo location reporting for your Android application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dee40-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dee40-106">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a><span data-ttu-id="dee40-107">Helyi jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="dee40-107">Location reporting</span></span>
<span data-ttu-id="dee40-108">Ha azt szeretné, hogy a helyek toobe jelentett, szüksége tooadd konfigurációs néhány sornyi (közötti hello `<application>` és `</application>` címkék).</span><span class="sxs-lookup"><span data-stu-id="dee40-108">If you want locations toobe reported, you need tooadd a few lines of configuration (between hello `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="dee40-109">Helymeghatározást</span><span class="sxs-lookup"><span data-stu-id="dee40-109">Lazy area location reporting</span></span>
<span data-ttu-id="dee40-110">Helymeghatározást lehetővé teszi, hogy jelentéskészítési hello ország, régió és Helység szerint társított eszközök.</span><span class="sxs-lookup"><span data-stu-id="dee40-110">Lazy area location reporting enables reporting hello country, region, and locality associated with devices.</span></span> <span data-ttu-id="dee40-111">A hely jelentéskészítési típusú csak használ a hálózati helyek (cella azonosító vagy Wi-Fi alapján).</span><span class="sxs-lookup"><span data-stu-id="dee40-111">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="dee40-112">hello eszköz terület munkamenetenként legfeljebb egyszer jelenti.</span><span class="sxs-lookup"><span data-stu-id="dee40-112">hello device area is reported at most once per session.</span></span> <span data-ttu-id="dee40-113">hello GPS soha nem szolgál, és így ez a típus helye jelentés alacsony hatással van hello akkumulátor.</span><span class="sxs-lookup"><span data-stu-id="dee40-113">hello GPS is never used, and thus this type of location report has low impact on hello battery.</span></span>

<span data-ttu-id="dee40-114">Jelentett területek a következők felhasználók munkamenetek, események és hibák földrajzi statisztikája használt toocompute.</span><span class="sxs-lookup"><span data-stu-id="dee40-114">Reported areas are used toocompute geographic statistics about users, sessions, events, and errors.</span></span> <span data-ttu-id="dee40-115">Ezek használhatók a Reach-kampányokat feltételként.</span><span class="sxs-lookup"><span data-stu-id="dee40-115">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="dee40-116">Engedélyezi a korábban az itt ismertetett eljárás hello konfiguráció reporting Lusta terület helye:</span><span class="sxs-lookup"><span data-stu-id="dee40-116">You enable lazy area location reporting by using hello configuration previously mentioned in this procedure:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="dee40-117">Meg kell a helyre vonatkozó engedély toospecify is.</span><span class="sxs-lookup"><span data-stu-id="dee40-117">You also need toospecify a location permission.</span></span> <span data-ttu-id="dee40-118">Ezt a kódot használja ``COARSE`` engedély:</span><span class="sxs-lookup"><span data-stu-id="dee40-118">This code uses ``COARSE`` permission:</span></span>

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="dee40-119">Ha az alkalmazás számára szükséges, akkor használhatja ``ACCESS_FINE_LOCATION`` helyette.</span><span class="sxs-lookup"><span data-stu-id="dee40-119">If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="dee40-120">Valós idejű hely jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="dee40-120">Real-time location reporting</span></span>
<span data-ttu-id="dee40-121">Valós idejű hely jelentéskészítési lehetővé teszi, hogy jelentéskészítési hello szélességi és hosszúsági társított eszközök.</span><span class="sxs-lookup"><span data-stu-id="dee40-121">Real-time location reporting enables reporting hello latitude and longitude associated with devices.</span></span> <span data-ttu-id="dee40-122">Alapértelmezés szerint a hely jelentéskészítési típusú hálózati helyek, cella azonosító vagy Wi-Fi alapú csak használja.</span><span class="sxs-lookup"><span data-stu-id="dee40-122">By default, this type of location reporting only uses network locations, based on Cell ID or WIFI.</span></span> <span data-ttu-id="dee40-123">hello reporting csak aktív alkalmazás futásakor, hello előtérben (például egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="dee40-123">hello reporting is only active when hello application runs in foreground (for example, during a session).</span></span>

<span data-ttu-id="dee40-124">Valós idejű helyek *nem* toocompute statisztika használt.</span><span class="sxs-lookup"><span data-stu-id="dee40-124">Real-time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="dee40-125">A csak célja a geokerítések valós idejű tooallow hello használata \<Reach-célközönségre-geokerítések\> Reach-kampányokat a feltételnek.</span><span class="sxs-lookup"><span data-stu-id="dee40-125">Their only purpose is tooallow hello use of real-time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="dee40-126">tooenable valós idejű hely jelentéskészítési, adja hozzá a sort a kód toowhere hello Engagement kapcsolati karakterlánc beállítása hello indítója tevékenység.</span><span class="sxs-lookup"><span data-stu-id="dee40-126">tooenable real-time location reporting, add a line of code toowhere you set hello Engagement connection string in hello launcher activity.</span></span> <span data-ttu-id="dee40-127">hello eredménye a következőhöz hasonló, hello:</span><span class="sxs-lookup"><span data-stu-id="dee40-127">hello result looks like hello following:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need toospecify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a><span data-ttu-id="dee40-128">GPS-alapú jelentés</span><span class="sxs-lookup"><span data-stu-id="dee40-128">GPS based reporting</span></span>
<span data-ttu-id="dee40-129">Alapértelmezés szerint a valós idejű hely jelentéskészítési csak használja hálózati helyek.</span><span class="sxs-lookup"><span data-stu-id="dee40-129">By default, real-time location reporting only uses network-based locations.</span></span> <span data-ttu-id="dee40-130">tooenable hello használata GPS-alapú helyeket, amelyek jobban pontos, hello konfigurációs objektum használja:</span><span class="sxs-lookup"><span data-stu-id="dee40-130">tooenable hello use of GPS-based locations, which are far more precise, use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="dee40-131">Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="dee40-131">You also need tooadd hello following permission if missing:</span></span>

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="dee40-132">Háttér-jelentés</span><span class="sxs-lookup"><span data-stu-id="dee40-132">Background reporting</span></span>
<span data-ttu-id="dee40-133">Alapértelmezés szerint a valós idejű hely jelentéskészítési csak aktív alkalmazás futásakor, hello előtérben (például egy munkamenetben).</span><span class="sxs-lookup"><span data-stu-id="dee40-133">By default, real-time location reporting is only active when hello application runs in foreground (for example, during a session).</span></span> <span data-ttu-id="dee40-134">a háttérben, is reporting tooenable hello a konfigurációs objektum használja:</span><span class="sxs-lookup"><span data-stu-id="dee40-134">tooenable hello reporting also in background, use this configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="dee40-135">Hello alkalmazás fut a háttérben, amikor csak a hálózati helyek küld jelentést, még akkor is, ha engedélyezte a hello GPS.</span><span class="sxs-lookup"><span data-stu-id="dee40-135">When hello application runs in background, only network-based locations are reported, even if you enabled hello GPS.</span></span>
> 
> 

<span data-ttu-id="dee40-136">Ha hello felhasználó az eszköz újraindul, hello háttér hely jelentés le van állítva.</span><span class="sxs-lookup"><span data-stu-id="dee40-136">If hello user reboots their device, hello background location report is stopped.</span></span> <span data-ttu-id="dee40-137">toomake automatikusan újraindítja a rendszerindítás, adja hozzá ezt a kódot.</span><span class="sxs-lookup"><span data-stu-id="dee40-137">toomake it automatically restart at boot time, add this code.</span></span>

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

<span data-ttu-id="dee40-138">Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="dee40-138">You also need tooadd hello following permission if missing:</span></span>

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a><span data-ttu-id="dee40-139">Android M-engedélyek</span><span class="sxs-lookup"><span data-stu-id="dee40-139">Android M permissions</span></span>
<span data-ttu-id="dee40-140">Az Android M-től kezdődően egyes engedélyek futásidőben felügyelt, és felhasználói jóváhagyás szükséges.</span><span class="sxs-lookup"><span data-stu-id="dee40-140">Starting with Android M, some permissions are managed at runtime and need user approval.</span></span>

<span data-ttu-id="dee40-141">Ha a cél Android API-szintet 23, hello futásidejű engedélyek ki vannak kapcsolva alapértelmezés szerint új alkalmazástelepítések.</span><span class="sxs-lookup"><span data-stu-id="dee40-141">If you target Android API level 23, hello runtime permissions are turned off by default for new app installations.</span></span> <span data-ttu-id="dee40-142">Ellenkező esetben ezek alapértelmezés szerint vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="dee40-142">Otherwise they are turned on by default.</span></span>

<span data-ttu-id="dee40-143">Akkor is engedélyezi/letiltja ezeket az engedélyeket hello eszköz beállítások menüjében.</span><span class="sxs-lookup"><span data-stu-id="dee40-143">You can enable/disable those permissions from hello device settings menu.</span></span> <span data-ttu-id="dee40-144">Hello háttérfolyamatot hello alkalmazás, amely a rendszer viselkedését, és nincs hatással van a háttérben képességét tooreceive leküldéses hello rendszer menü engedélyek kikapcsolása használhatatlanná teszi.</span><span class="sxs-lookup"><span data-stu-id="dee40-144">Turning off permissions from hello system menu kills hello background processes of hello application, which is a system behavior, and has no impact on ability tooreceive push in background.</span></span>

<span data-ttu-id="dee40-145">A Mobile Engagement-hely jelentési hello környezetében futásidőben jóváhagyást igénylő hello engedélyek a következők:</span><span class="sxs-lookup"><span data-stu-id="dee40-145">In hello context of Mobile Engagement location reporting, hello permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

<span data-ttu-id="dee40-146">Egy szabványos rendszer párbeszédpanelen hello felhasználói engedélyek kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="dee40-146">Request permissions from hello user using a standard system dialog.</span></span> <span data-ttu-id="dee40-147">Ha hello felhasználói jóvá, mondja el ``EngagementAgent`` tootake, amely valós idejű figyelembe változik.</span><span class="sxs-lookup"><span data-stu-id="dee40-147">If hello user approves, tell ``EngagementAgent`` tootake that change into account in real-time.</span></span> <span data-ttu-id="dee40-148">Ellenkező esetben a hello módosítása feldolgozott hello következő hello felhasználó elindítja hello alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="dee40-148">Otherwise hello change is processed hello next time hello user launches hello application.</span></span>

<span data-ttu-id="dee40-149">Íme egy kódot minta toouse toorequest Alkalmazásengedélyek és előre hello eredmény tevékenységet, ha pozitív túl``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="dee40-149">Here is a code sample toouse in an activity of your application toorequest permissions and forward hello result if positive too``EngagementAgent``:</span></span>

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
         * Request location permission, but this doesn't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
