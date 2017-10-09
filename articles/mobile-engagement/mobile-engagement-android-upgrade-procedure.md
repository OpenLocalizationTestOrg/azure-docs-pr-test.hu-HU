---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: df5c82812fe0a242eaa5df8c906030237215b7eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="360a0-103">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="360a0-103">Upgrade procedures</span></span>
<span data-ttu-id="360a0-104">Ha Ön már rendelkezik integrált egy régebbi verzióját az SDK az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.</span><span class="sxs-lookup"><span data-stu-id="360a0-104">If you already have integrated an older version of our SDK into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="360a0-105">Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK a különböző verzióiban.</span><span class="sxs-lookup"><span data-stu-id="360a0-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="360a0-106">Például ha telepít át 1.4.0 hello kövesse toofirst rendelkezik too1.6.0 "1.4.0 a too1.5.0" eljárást akkor hello "1.5.0 a too1.6.0" eljárás.</span><span class="sxs-lookup"><span data-stu-id="360a0-106">For example if you migrate from 1.4.0 too1.6.0 you have toofirst follow hello "from 1.4.0 too1.5.0" procedure then hello "from 1.5.0 too1.6.0" procedure.</span></span>

<span data-ttu-id="360a0-107">Bármilyen hello verzióra frissít, hogy tooreplace hello `mobile-engagement-VERSION.jar` a hello újat.</span><span class="sxs-lookup"><span data-stu-id="360a0-107">Whatever hello version you upgrade from, you have tooreplace hello `mobile-engagement-VERSION.jar` with hello new one.</span></span>

## <a name="from-420-too421"></a><span data-ttu-id="360a0-108">A 4.2.0 too4.2.1</span><span class="sxs-lookup"><span data-stu-id="360a0-108">From 4.2.0 too4.2.1</span></span>
<span data-ttu-id="360a0-109">Ez a lépés ténylegesen végezhető hello SDK bármely verzióját, hogy a biztonság fokozása Reach tevékenységek integrálásakor.</span><span class="sxs-lookup"><span data-stu-id="360a0-109">This step can actually be done on any version of hello SDK, its a security improvement when you integrate Reach activities.</span></span>

<span data-ttu-id="360a0-110">Most adja hozzá `exported="false"` tooall Reach tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="360a0-110">You should now add `exported="false"` tooall Reach activities.</span></span>

<span data-ttu-id="360a0-111">Reach tevékenységek most példához hasonló a `AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="360a0-111">Reach activities should now look like this on your `AndroidManifest.xml`:</span></span>

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

## <a name="from-400-too410"></a><span data-ttu-id="360a0-112">A 4.0.0 too4.1.0</span><span class="sxs-lookup"><span data-stu-id="360a0-112">From 4.0.0 too4.1.0</span></span>
<span data-ttu-id="360a0-113">hello SDK most leíró új engedély modell Android m-alapú.</span><span class="sxs-lookup"><span data-stu-id="360a0-113">hello SDK now handle new permission model from Android M.</span></span>

<span data-ttu-id="360a0-114">Ha hely használatát, vagy olvassa el nagy vonalakban tekinti értesítések [ebben a szakaszban](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="360a0-114">If you use location features or big picture notifications please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>

<span data-ttu-id="360a0-115">Ezenkívül toohello új engedély modell mostantól támogatjuk hely szolgáltatások konfigurálásának futásidőben.</span><span class="sxs-lookup"><span data-stu-id="360a0-115">In addition toohello new permission model, we now support configuring location features at runtime.</span></span>
<span data-ttu-id="360a0-116">Továbbra is kompatibilis helyhez hello jegyzék paraméterekkel dolgozunk, de most már elavult.</span><span class="sxs-lookup"><span data-stu-id="360a0-116">We are still compatible with hello manifest parameters for location but it's now deprecated.</span></span> <span data-ttu-id="360a0-117">remove hello következő szakaszában toouse futásidejű konfigurálását, a ``AndroidManifest.xml``:</span><span class="sxs-lookup"><span data-stu-id="360a0-117">toouse runtime configuration, remove hello following sections from your ``AndroidManifest.xml``:</span></span>

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

<span data-ttu-id="360a0-118">olvassa el és [a frissíti, az eljárás](mobile-engagement-android-integrate-engagement.md#location-reporting) toouse futásidejű konfigurálását helyette.</span><span class="sxs-lookup"><span data-stu-id="360a0-118">and read [this updated procedure](mobile-engagement-android-integrate-engagement.md#location-reporting) toouse runtime configuration instead.</span></span>

## <a name="from-300-too400"></a><span data-ttu-id="360a0-119">A 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="360a0-119">From 3.0.0 too4.0.0</span></span>
### <a name="native-push"></a><span data-ttu-id="360a0-120">Natív leküldéssel</span><span class="sxs-lookup"><span data-stu-id="360a0-120">Native push</span></span>
<span data-ttu-id="360a0-121">Natív leküldéses (GCM/ADM) most is szolgál az alkalmazáson belüli értesítések, konfigurálnia kell a hello hitelesítő adatokat a natív leküldéses kampány bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="360a0-121">Native push (GCM/ADM) is now also used for in app notifications so you must configure hello native push credentials for any type of push campaign.</span></span>

<span data-ttu-id="360a0-122">Ha nem már kövesse [ezzel az eljárással](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span><span class="sxs-lookup"><span data-stu-id="360a0-122">If not already done please follow [this procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="360a0-123">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="360a0-123">AndroidManifest.xml</span></span>
<span data-ttu-id="360a0-124">Reach integrációs módosítottuk ``AndroidManifest.xml``.</span><span class="sxs-lookup"><span data-stu-id="360a0-124">Reach integration has been modified in ``AndroidManifest.xml``.</span></span>

<span data-ttu-id="360a0-125">Cserélje le ezt:</span><span class="sxs-lookup"><span data-stu-id="360a0-125">Replace this:</span></span>

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

<span data-ttu-id="360a0-126">–</span><span class="sxs-lookup"><span data-stu-id="360a0-126">By</span></span>

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

<span data-ttu-id="360a0-127">Már létezik valószínűleg betöltése képernyő kattintva (a szöveges vagy webes tartalom) bejelentés vagy szavazásként.</span><span class="sxs-lookup"><span data-stu-id="360a0-127">There is possibly a loading screen now when you click on an announcement (with text/web content) or a poll.</span></span>
<span data-ttu-id="360a0-128">Rendelkezik tooadd ez ezeket a 4.0.0 kampányok toowork:</span><span class="sxs-lookup"><span data-stu-id="360a0-128">You have tooadd this for those campaigns toowork in 4.0.0:</span></span>

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a><span data-ttu-id="360a0-129">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="360a0-129">Resources</span></span>
<span data-ttu-id="360a0-130">Új hello beágyazása `res/layout/engagement_loading.xml` fájlt a projektben.</span><span class="sxs-lookup"><span data-stu-id="360a0-130">Embed hello new `res/layout/engagement_loading.xml` file into your project.</span></span>

## <a name="from-240-too300"></a><span data-ttu-id="360a0-131">A 2.4.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="360a0-131">From 2.4.0 too3.0.0</span></span>
<span data-ttu-id="360a0-132">hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="360a0-132">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> <span data-ttu-id="360a0-133">Ha egy korábbi verziójáról telepít, tekintse át először hello Capptain webhely toomigrate too2.4.0, és ezután alkalmazza az eljárást követő hello.</span><span class="sxs-lookup"><span data-stu-id="360a0-133">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too2.4.0 first and then apply hello following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="360a0-134">Capptain és a Mobile Engagement nem hello azonos szolgáltatások, és hello az alábbi eljárás csak Highlight hogyan toomigrate hello ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="360a0-134">Capptain and Mobile Engagement are not hello same services, and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="360a0-135">Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement-kiszolgálókról.</span><span class="sxs-lookup"><span data-stu-id="360a0-135">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers.</span></span>
> 
> 

### <a name="jar-file"></a><span data-ttu-id="360a0-136">JAR-fájlra</span><span class="sxs-lookup"><span data-stu-id="360a0-136">JAR file</span></span>
<span data-ttu-id="360a0-137">Cserélje le `capptain.jar` által `mobile-engagement-VERSION.jar` a a `libs` mappát.</span><span class="sxs-lookup"><span data-stu-id="360a0-137">Replace `capptain.jar` by `mobile-engagement-VERSION.jar` in your `libs` folder.</span></span>

### <a name="resource-files"></a><span data-ttu-id="360a0-138">Erőforrás-fájlok</span><span class="sxs-lookup"><span data-stu-id="360a0-138">Resource files</span></span>
<span data-ttu-id="360a0-139">A Microsoft által biztosított minden erőforrásfájl (által meghatározott `capptain_`) toobe helyettesítése hello újakat (előtagként `engagement_`).</span><span class="sxs-lookup"><span data-stu-id="360a0-139">Every resource file that we provided (prefixed by `capptain_`) has toobe replaced by hello new ones (prefixed with `engagement_`).</span></span>

<span data-ttu-id="360a0-140">Ha testreszabta azokat a fájlokat, rendelkezik-e toore-hello új fájlok, a testreszabást alkalmazni **hello erőforrás-fájlokban szereplő összes hello azonosítónak is át lett nevezve**.</span><span class="sxs-lookup"><span data-stu-id="360a0-140">If you customized those files, you have toore-apply your customization on hello new files, **all hello identifiers in hello resource files have also been renamed**.</span></span>

### <a name="application-id"></a><span data-ttu-id="360a0-141">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="360a0-141">Application ID</span></span>
<span data-ttu-id="360a0-142">Most Engagement használja a kapcsolati karakterlánc tooconfigure hello SDK azonosítók például hello alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="360a0-142">Now Engagement uses a connection string tooconfigure hello SDK identifiers such as hello application identifier.</span></span>

<span data-ttu-id="360a0-143">Toouse rendelkezik `EngagementAgent.init` módszer a indítója tevékenységi ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="360a0-143">You have toouse `EngagementAgent.init` method in your launcher activity like this:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="360a0-144">hello kapcsolati karakterlánc az alkalmazás Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="360a0-144">hello connection string for your application is displayed on Azure Portal.</span></span>

<span data-ttu-id="360a0-145">Távolítsa el az összes hívás túl`CapptainAgent.configure` , `EngagementAgent.init` Ez a módszer váltja fel.</span><span class="sxs-lookup"><span data-stu-id="360a0-145">Please remove any call too`CapptainAgent.configure` as `EngagementAgent.init` replaces that method.</span></span>

<span data-ttu-id="360a0-146">Hello `appId` már nem konfigurálható segítségével `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="360a0-146">hello `appId` can no longer be configured using `AndroidManifest.xml`.</span></span>

<span data-ttu-id="360a0-147">Távolítsa el az ebben a szakaszban a `AndroidManifest.xml` Ha azt:</span><span class="sxs-lookup"><span data-stu-id="360a0-147">Please remove this section from your `AndroidManifest.xml` if you have it:</span></span>

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a><span data-ttu-id="360a0-148">Java API</span><span class="sxs-lookup"><span data-stu-id="360a0-148">Java API</span></span>
<span data-ttu-id="360a0-149">Minden hívás tooany az SDK Java-osztály rendelkezik toobe átnevezett; például `CapptainAgent.getInstance(this)` kell átnevezni `EngagementAgent.getInstance(this)`, `extends CapptainActivity` kell átnevezni `extends EngagementActivity` stb...</span><span class="sxs-lookup"><span data-stu-id="360a0-149">Every call tooany Java class of our SDK has toobe renamed; for example, `CapptainAgent.getInstance(this)` must be renamed `EngagementAgent.getInstance(this)`, `extends CapptainActivity` must be renamed `extends EngagementActivity` etc...</span></span>

<span data-ttu-id="360a0-150">Ha alapértelmezett ügynök preferencia fájlok lettek integrálva, hello alapértelmezett neve mostantól `engagement.agent` és hello kulcs `engagement:agent`.</span><span class="sxs-lookup"><span data-stu-id="360a0-150">If you were integrated with default agent preference files, hello default file name is now `engagement.agent` and hello key is `engagement:agent`.</span></span>

<span data-ttu-id="360a0-151">Webes közlemények létrehozásakor hello Javascript kötő mostantól `engagementReachContent`.</span><span class="sxs-lookup"><span data-stu-id="360a0-151">When creating web announcements, hello Javascript binder is now `engagementReachContent`.</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="360a0-152">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="360a0-152">AndroidManifest.xml</span></span>
<span data-ttu-id="360a0-153">Hiba történt a sok módosítást, hello szolgáltatást nem többé megosztva, és nagy mennyiségű fogadók nem exportálható többé.</span><span class="sxs-lookup"><span data-stu-id="360a0-153">A lot of changes happened there, hello service is not shared anymore, and a lot of receivers are not exportable anymore.</span></span>

<span data-ttu-id="360a0-154">hello szolgáltatás deklaráció mostantól egyszerűbb; Távolítsa el a hello leképezési szűrő és az összes metaadatot azon belül, és adja hozzá `exportable=false`.</span><span class="sxs-lookup"><span data-stu-id="360a0-154">hello service declaration is now simpler; remove hello intent filter and all meta-data inside it, and add `exportable=false`.</span></span>

<span data-ttu-id="360a0-155">Plusz átnevezett toouse engagement.</span><span class="sxs-lookup"><span data-stu-id="360a0-155">Plus everything is renamed toouse engagement.</span></span>

<span data-ttu-id="360a0-156">Ez most néz ki:</span><span class="sxs-lookup"><span data-stu-id="360a0-156">It now looks like:</span></span>

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

<span data-ttu-id="360a0-157">Ha azt szeretné, hogy tooenable vizsgálati naplókat, hello metaadatok most már toohello alkalmazás címke áthelyezni, és át lett nevezve:</span><span class="sxs-lookup"><span data-stu-id="360a0-157">When you want tooenable test logs, hello meta-data has now been moved toohello application tag and has been renamed:</span></span>

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

<span data-ttu-id="360a0-158">Minden egyéb metaadatot csak át lett nevezve, hello teljes listája (természetesen átnevezése csak hello néhányat a meglévők közül használhat):</span><span class="sxs-lookup"><span data-stu-id="360a0-158">All other meta-data have just been renamed, here is hello full list (of course rename only hello ones you use):</span></span>

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

<span data-ttu-id="360a0-159">Google Play és SmartAd követési el lett távolítva az SDK csak rendelkezik tooremove ez helyettesítő nélkül:</span><span class="sxs-lookup"><span data-stu-id="360a0-159">Google Play and SmartAd tracking has been removed from SDK you just have tooremove this without replacement:</span></span>

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

<span data-ttu-id="360a0-160">hello Reach tevékenységek most deklarált ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="360a0-160">hello Reach activities are now declared like this:</span></span>

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

<span data-ttu-id="360a0-161">Ha egyéni Reach tevékenységeket, vagy van kell csak toochange hello leképezési műveletek toomatch `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` vagy `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span><span class="sxs-lookup"><span data-stu-id="360a0-161">If you have custom Reach activities, you need only toochange hello intent actions toomatch either `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` or `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span></span>

<span data-ttu-id="360a0-162">hello szórási fogadók átnevezték, valamint mostantól felvehet `exported=false`.</span><span class="sxs-lookup"><span data-stu-id="360a0-162">hello broadcast receivers have been renamed, plus we now add `exported=false`.</span></span> <span data-ttu-id="360a0-163">Hello hello fogadók hello új meghatározásával, (természetesen átnevezése csak hello néhányat a meglévők közül használhat) teljes listája itt található:</span><span class="sxs-lookup"><span data-stu-id="360a0-163">Here is hello full list of hello receivers with hello new specification, (of course rename only hello ones you use):</span></span>

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

<span data-ttu-id="360a0-164">Követés fogadó eltávolították, így ez a szakasz kell tooremove:</span><span class="sxs-lookup"><span data-stu-id="360a0-164">Tracking receiver has been removed, so you have tooremove this section:</span></span>

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

<span data-ttu-id="360a0-165">Vegye figyelembe, hogy hello megvalósítását hello deklarációjában szórási fogadó **EngagementMessageReceiver** hello változásai `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="360a0-165">Note that hello declaration of your implementation of hello broadcast receiver **EngagementMessageReceiver** has changed in hello `AndroidManifest.xml`.</span></span> <span data-ttu-id="360a0-166">Ez mert hello API toosend és eltávolítás tetszőleges XMPP a tetszőleges XMPP entitásokból üzenetek és API toosend hello és eszközök közötti üzeneteket fogadni, hogy eltávolították.</span><span class="sxs-lookup"><span data-stu-id="360a0-166">This is because hello API toosend and remove arbitrary XMPP messages from arbitrary XMPP entities and hello API toosend and receive messages between devices have been removed.</span></span> <span data-ttu-id="360a0-167">Ebből kifolyólag rendelkezik is a visszahívásokat a következő toodelete hello a **EngagementMessageReceiver** végrehajtására:</span><span class="sxs-lookup"><span data-stu-id="360a0-167">Thus, you have also toodelete hello following callbacks from your **EngagementMessageReceiver** implementation :</span></span>

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

<span data-ttu-id="360a0-168">és</span><span class="sxs-lookup"><span data-stu-id="360a0-168">and</span></span>

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

<span data-ttu-id="360a0-169">Törölje a semmilyen hívásban **EngagementAgent** számára:</span><span class="sxs-lookup"><span data-stu-id="360a0-169">then delete any call on **EngagementAgent** for :</span></span>

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

<span data-ttu-id="360a0-170">és</span><span class="sxs-lookup"><span data-stu-id="360a0-170">and</span></span>

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a><span data-ttu-id="360a0-171">Proguard</span><span class="sxs-lookup"><span data-stu-id="360a0-171">Proguard</span></span>
<span data-ttu-id="360a0-172">Proguard konfigurációs is negatív hatással lehet rebranding, szabályok most hasznosítani például hello:</span><span class="sxs-lookup"><span data-stu-id="360a0-172">Proguard configuration can be impacted by rebranding, hello rules are now looking like:</span></span>

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

