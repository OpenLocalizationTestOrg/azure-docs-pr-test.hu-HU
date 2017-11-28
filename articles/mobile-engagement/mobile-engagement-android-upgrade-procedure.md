---
title: "Az Azure Mobile Engagement Android SDK-integráció"
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
ms.openlocfilehash: 1f047f93fa8bc852b28c86e91d0c007a94fb4299
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="c3c9c-103">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="c3c9c-103">Upgrade procedures</span></span>
<span data-ttu-id="c3c9c-104">Ha Ön már rendelkezik integrált egy régebbi verzióját az SDK az alkalmazásba, hogy az SDK-val történő frissítése során, vegye figyelembe a következő szempontokat.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-104">If you already have integrated an older version of our SDK into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="c3c9c-105">Előfordulhat, hogy kell eljárnia számos eljárást, ha nem fogadta az SDK a különböző verzióiban.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="c3c9c-106">Például ha áttelepít 1.4.0 1.6.0 először hajtsa végre a "from a 1.5.0 1.4.0" eljárást kell majd a "from a 1.6.0 1.5.0" eljárást.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-106">For example if you migrate from 1.4.0 to 1.6.0 you have to first follow the "from 1.4.0 to 1.5.0" procedure then the "from 1.5.0 to 1.6.0" procedure.</span></span>

<span data-ttu-id="c3c9c-107">Frissít, függetlenül a verziót, hogy cserélje le a `mobile-engagement-VERSION.jar` az újjal.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-107">Whatever the version you upgrade from, you have to replace the `mobile-engagement-VERSION.jar` with the new one.</span></span>

## <a name="from-420-to-421"></a><span data-ttu-id="c3c9c-108">A 4.2.0 való 4.2.1.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-108">From 4.2.0 to 4.2.1</span></span>
<span data-ttu-id="c3c9c-109">Ez a lépés ténylegesen végezhető az SDK-t bármely verzióját, hogy a biztonság fokozása Reach tevékenységek integrálásakor.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-109">This step can actually be done on any version of the SDK, its a security improvement when you integrate Reach activities.</span></span>

<span data-ttu-id="c3c9c-110">Most adja hozzá `exported="false"` összes Reach tevékenységben.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-110">You should now add `exported="false"` to all Reach activities.</span></span>

<span data-ttu-id="c3c9c-111">Reach tevékenységek most példához hasonló a `AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-111">Reach activities should now look like this on your `AndroidManifest.xml`:</span></span>

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

## <a name="from-400-to-410"></a><span data-ttu-id="c3c9c-112">A 4.0.0 4.1.0 számára</span><span class="sxs-lookup"><span data-stu-id="c3c9c-112">From 4.0.0 to 4.1.0</span></span>
<span data-ttu-id="c3c9c-113">Az SDK most leíró új engedély modellben Android m-alapú.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-113">The SDK now handle new permission model from Android M.</span></span>

<span data-ttu-id="c3c9c-114">Ha hely használatát, vagy olvassa el nagy vonalakban tekinti értesítések [ebben a szakaszban](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="c3c9c-114">If you use location features or big picture notifications please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>

<span data-ttu-id="c3c9c-115">Az új engedély modell mellett mostantól támogatjuk konfigurálása hely szolgáltatások futási időben.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-115">In addition to the new permission model, we now support configuring location features at runtime.</span></span>
<span data-ttu-id="c3c9c-116">Dolgozunk továbbra is kompatibilis helyhez jegyzék paraméterekkel rendelkező, de most már elavult.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-116">We are still compatible with the manifest parameters for location but it's now deprecated.</span></span> <span data-ttu-id="c3c9c-117">Futásidejű konfigurálását használatához távolítsa el a következő szakaszok a ``AndroidManifest.xml``:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-117">To use runtime configuration, remove the following sections from your ``AndroidManifest.xml``:</span></span>

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

<span data-ttu-id="c3c9c-118">olvassa el és [a frissíti, az eljárás](mobile-engagement-android-integrate-engagement.md#location-reporting) használja helyette a futásidejű konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-118">and read [this updated procedure](mobile-engagement-android-integrate-engagement.md#location-reporting) to use runtime configuration instead.</span></span>

## <a name="from-300-to-400"></a><span data-ttu-id="c3c9c-119">A 3.0.0 4.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="c3c9c-119">From 3.0.0 to 4.0.0</span></span>
### <a name="native-push"></a><span data-ttu-id="c3c9c-120">Natív leküldéssel</span><span class="sxs-lookup"><span data-stu-id="c3c9c-120">Native push</span></span>
<span data-ttu-id="c3c9c-121">Natív leküldéses (GCM/ADM) most is szolgál az alkalmazáson belüli értesítések, konfigurálnia kell a hitelesítő adatokat a natív leküldéses kampány bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-121">Native push (GCM/ADM) is now also used for in app notifications so you must configure the native push credentials for any type of push campaign.</span></span>

<span data-ttu-id="c3c9c-122">Ha nem már kövesse [ezzel az eljárással](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span><span class="sxs-lookup"><span data-stu-id="c3c9c-122">If not already done please follow [this procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="c3c9c-123">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="c3c9c-123">AndroidManifest.xml</span></span>
<span data-ttu-id="c3c9c-124">Reach integrációs módosítottuk ``AndroidManifest.xml``.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-124">Reach integration has been modified in ``AndroidManifest.xml``.</span></span>

<span data-ttu-id="c3c9c-125">Cserélje le ezt:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-125">Replace this:</span></span>

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

<span data-ttu-id="c3c9c-126">–</span><span class="sxs-lookup"><span data-stu-id="c3c9c-126">By</span></span>

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

<span data-ttu-id="c3c9c-127">Már létezik valószínűleg betöltése képernyő kattintva (a szöveges vagy webes tartalom) bejelentés vagy szavazásként.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-127">There is possibly a loading screen now when you click on an announcement (with text/web content) or a poll.</span></span>
<span data-ttu-id="c3c9c-128">Fel kell vennie ezeket a kampányok 4.0.0 munkát ez:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-128">You have to add this for those campaigns to work in 4.0.0:</span></span>

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a><span data-ttu-id="c3c9c-129">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="c3c9c-129">Resources</span></span>
<span data-ttu-id="c3c9c-130">Az új beágyazása `res/layout/engagement_loading.xml` fájlt a projektben.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-130">Embed the new `res/layout/engagement_loading.xml` file into your project.</span></span>

## <a name="from-240-to-300"></a><span data-ttu-id="c3c9c-131">A 2.4.0 3.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="c3c9c-131">From 2.4.0 to 3.0.0</span></span>
<span data-ttu-id="c3c9c-132">A következő ismerteti, hogyan telepíthetők át az SDK-integráció az alkalmazásba az Azure Mobile Engagement technológiával Capptain SAS által kínált Capptain szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-132">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> <span data-ttu-id="c3c9c-133">Ha egy korábbi verziójáról telepít, tekintse át a Capptain webhely 2.4.0 először át, és ezután alkalmazza az alábbi eljárást.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-133">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 2.4.0 first and then apply the following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c3c9c-134">Capptain és a Mobile Engagement nem ugyanazok a szolgáltatások, és az alábbi eljárás csak emel ki, hogyan telepítheti át az ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-134">Capptain and Mobile Engagement are not the same services, and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="c3c9c-135">Az SDK-t az alkalmazás áttelepítése rendszer nem telepíti át az adatok a Capptain kiszolgálókról a Mobile Engagement-kiszolgálókra.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-135">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers.</span></span>
> 
> 

### <a name="jar-file"></a><span data-ttu-id="c3c9c-136">JAR-fájlra</span><span class="sxs-lookup"><span data-stu-id="c3c9c-136">JAR file</span></span>
<span data-ttu-id="c3c9c-137">Cserélje le `capptain.jar` által `mobile-engagement-VERSION.jar` a a `libs` mappát.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-137">Replace `capptain.jar` by `mobile-engagement-VERSION.jar` in your `libs` folder.</span></span>

### <a name="resource-files"></a><span data-ttu-id="c3c9c-138">Erőforrás-fájlok</span><span class="sxs-lookup"><span data-stu-id="c3c9c-138">Resource files</span></span>
<span data-ttu-id="c3c9c-139">A Microsoft által biztosított minden erőforrásfájl (előtagjaként `capptain_`) helyébe a újakat kell (előtagként `engagement_`).</span><span class="sxs-lookup"><span data-stu-id="c3c9c-139">Every resource file that we provided (prefixed by `capptain_`) has to be replaced by the new ones (prefixed with `engagement_`).</span></span>

<span data-ttu-id="c3c9c-140">Ha testreszabta azokat a fájlokat, rendelkezik-e alkalmazza újra az új fájlok, a testreszabást **az erőforrás-fájlokban szereplő összes azonosító is át lett nevezve**.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-140">If you customized those files, you have to re-apply your customization on the new files, **all the identifiers in the resource files have also been renamed**.</span></span>

### <a name="application-id"></a><span data-ttu-id="c3c9c-141">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="c3c9c-141">Application ID</span></span>
<span data-ttu-id="c3c9c-142">Most Engagement használ egy kapcsolati karakterlánc konfigurálásához, az SDK-azonosítókat, például az alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-142">Now Engagement uses a connection string to configure the SDK identifiers such as the application identifier.</span></span>

<span data-ttu-id="c3c9c-143">Szükség van `EngagementAgent.init` módszer a indítója tevékenységi ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-143">You have to use `EngagementAgent.init` method in your launcher activity like this:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="c3c9c-144">A kapcsolati karakterlánc az alkalmazás Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-144">The connection string for your application is displayed on Azure Portal.</span></span>

<span data-ttu-id="c3c9c-145">Távolítsa el az összes hívása `CapptainAgent.configure` , `EngagementAgent.init` Ez a módszer váltja fel.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-145">Please remove any call to `CapptainAgent.configure` as `EngagementAgent.init` replaces that method.</span></span>

<span data-ttu-id="c3c9c-146">A `appId` már nem konfigurálható segítségével `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-146">The `appId` can no longer be configured using `AndroidManifest.xml`.</span></span>

<span data-ttu-id="c3c9c-147">Távolítsa el az ebben a szakaszban a `AndroidManifest.xml` Ha azt:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-147">Please remove this section from your `AndroidManifest.xml` if you have it:</span></span>

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a><span data-ttu-id="c3c9c-148">Java API</span><span class="sxs-lookup"><span data-stu-id="c3c9c-148">Java API</span></span>
<span data-ttu-id="c3c9c-149">Az SDK minden Java-osztály minden hívás tartalmaz, át kell nevezni; például `CapptainAgent.getInstance(this)` kell átnevezni `EngagementAgent.getInstance(this)`, `extends CapptainActivity` kell átnevezni `extends EngagementActivity` stb...</span><span class="sxs-lookup"><span data-stu-id="c3c9c-149">Every call to any Java class of our SDK has to be renamed; for example, `CapptainAgent.getInstance(this)` must be renamed `EngagementAgent.getInstance(this)`, `extends CapptainActivity` must be renamed `extends EngagementActivity` etc...</span></span>

<span data-ttu-id="c3c9c-150">Alapértelmezett ügynök preferencia fájlok lettek integrálva, ha a fájl alapértelmezett neve mostantól `engagement.agent` és a kulcs `engagement:agent`.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-150">If you were integrated with default agent preference files, the default file name is now `engagement.agent` and the key is `engagement:agent`.</span></span>

<span data-ttu-id="c3c9c-151">Webes közlemények létrehozásakor a Javascript kötő mostantól `engagementReachContent`.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-151">When creating web announcements, the Javascript binder is now `engagementReachContent`.</span></span>

### <a name="androidmanifestxml"></a><span data-ttu-id="c3c9c-152">AndroidManifest.xml</span><span class="sxs-lookup"><span data-stu-id="c3c9c-152">AndroidManifest.xml</span></span>
<span data-ttu-id="c3c9c-153">Hiba történt a sok módosítást, a szolgáltatás már nincs megosztva, és nagy mennyiségű fogadók nem exportálható többé.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-153">A lot of changes happened there, the service is not shared anymore, and a lot of receivers are not exportable anymore.</span></span>

<span data-ttu-id="c3c9c-154">A szolgáltatás deklaráció mostantól egyszerűbb; Távolítsa el a leképezési szűrő és az összes metaadatot azon belül, és adja hozzá `exportable=false`.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-154">The service declaration is now simpler; remove the intent filter and all meta-data inside it, and add `exportable=false`.</span></span>

<span data-ttu-id="c3c9c-155">Továbbá minden engagement használata átnevezi.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-155">Plus everything is renamed to use engagement.</span></span>

<span data-ttu-id="c3c9c-156">Ez most néz ki:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-156">It now looks like:</span></span>

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

<span data-ttu-id="c3c9c-157">Ha szeretné engedélyezni a vizsgálati naplók, a metaadatok most már át lett helyezve a alkalmazás címkére, és át lett nevezve:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-157">When you want to enable test logs, the meta-data has now been moved to the application tag and has been renamed:</span></span>

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

<span data-ttu-id="c3c9c-158">Minden egyéb metaadatot csak át lett nevezve, a teljes listája (az állomásokon átnevezése csak az megfelelően használata):</span><span class="sxs-lookup"><span data-stu-id="c3c9c-158">All other meta-data have just been renamed, here is the full list (of course rename only the ones you use):</span></span>

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

<span data-ttu-id="c3c9c-159">Google Play áruházból és nyomon követését SmartAd el lett távolítva SDK csak el kell távolítania a helyettesítő nélkül:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-159">Google Play and SmartAd tracking has been removed from SDK you just have to remove this without replacement:</span></span>

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

<span data-ttu-id="c3c9c-160">A Reach-tevékenységek most deklarált ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-160">The Reach activities are now declared like this:</span></span>

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

<span data-ttu-id="c3c9c-161">Ha egyéni Reach tevékenységeket, csak módosítani szeretné a leképezési műveletek egyező vagy `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` vagy `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-161">If you have custom Reach activities, you need only to change the intent actions to match either `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` or `com.microsoft.azure.engagement.reach.intent.action.POLL`.</span></span>

<span data-ttu-id="c3c9c-162">A szórási fogadók át lett nevezve, valamint mostantól felvehet `exported=false`.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-162">The broadcast receivers have been renamed, plus we now add `exported=false`.</span></span> <span data-ttu-id="c3c9c-163">A fogadók (az állomásokon átnevezése csak az megfelelően használ) az új meghatározásával teljes listája itt található:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-163">Here is the full list of the receivers with the new specification, (of course rename only the ones you use):</span></span>

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

<span data-ttu-id="c3c9c-164">Követés fogadó eltávolították, ezért el kell távolítania az ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-164">Tracking receiver has been removed, so you have to remove this section:</span></span>

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

<span data-ttu-id="c3c9c-165">Vegye figyelembe, hogy a szórásos receiver megvalósítását deklarációjában **EngagementMessageReceiver** módosult a `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-165">Note that the declaration of your implementation of the broadcast receiver **EngagementMessageReceiver** has changed in the `AndroidManifest.xml`.</span></span> <span data-ttu-id="c3c9c-166">Ennek az az oka az API-t küldeni, és távolítsa el az tetszőleges XMPP-üzeneteket tetszőleges XMPP-entitások és az API-t az eszközök közötti üzeneteket küldjön és fogadjon el lettek távolítva.</span><span class="sxs-lookup"><span data-stu-id="c3c9c-166">This is because the API to send and remove arbitrary XMPP messages from arbitrary XMPP entities and the API to send and receive messages between devices have been removed.</span></span> <span data-ttu-id="c3c9c-167">Így is törölnie kell a következő visszahívások a **EngagementMessageReceiver** végrehajtására:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-167">Thus, you have also to delete the following callbacks from your **EngagementMessageReceiver** implementation :</span></span>

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

<span data-ttu-id="c3c9c-168">és</span><span class="sxs-lookup"><span data-stu-id="c3c9c-168">and</span></span>

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

<span data-ttu-id="c3c9c-169">Törölje a semmilyen hívásban **EngagementAgent** számára:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-169">then delete any call on **EngagementAgent** for :</span></span>

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

<span data-ttu-id="c3c9c-170">és</span><span class="sxs-lookup"><span data-stu-id="c3c9c-170">and</span></span>

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a><span data-ttu-id="c3c9c-171">Proguard</span><span class="sxs-lookup"><span data-stu-id="c3c9c-171">Proguard</span></span>
<span data-ttu-id="c3c9c-172">Proguard konfigurációs is negatív hatással lehet rebranding, például a szabályok most hasznosítani:</span><span class="sxs-lookup"><span data-stu-id="c3c9c-172">Proguard configuration can be impacted by rebranding, the rules are now looking like:</span></span>

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

