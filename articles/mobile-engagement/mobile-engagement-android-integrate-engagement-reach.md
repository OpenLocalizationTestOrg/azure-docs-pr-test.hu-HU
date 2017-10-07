---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 4ab6143771bdc0758a548abb529d6bde98fc0e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-android"></a><span data-ttu-id="af661-103">Hogyan tooIntegrate Engagement Reach Android rendszeren</span><span class="sxs-lookup"><span data-stu-id="af661-103">How tooIntegrate Engagement Reach on Android</span></span>
> [!IMPORTANT]
> <span data-ttu-id="af661-104">Hogyan tooIntegrate Engagement az Android-dokumentum Ez az útmutató követése előtt hello ismertetett hello integrációs eljárást kell követnie.</span><span class="sxs-lookup"><span data-stu-id="af661-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> 

## <a name="standard-integration"></a><span data-ttu-id="af661-105">Standard integráció</span><span class="sxs-lookup"><span data-stu-id="af661-105">Standard integration</span></span>

<span data-ttu-id="af661-106">Másolja a projekt SDK hello Reach erőforrás fájlokat:</span><span class="sxs-lookup"><span data-stu-id="af661-106">Copy Reach resource files from hello SDK in your project :</span></span>

* <span data-ttu-id="af661-107">Hello fájlok másolása hello `res/layout` mappa küldi hello SDK be hello `res/layout` az alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="af661-107">Copy hello files from hello `res/layout` folder delivered with hello SDK into hello `res/layout` folder of your application.</span></span>
* <span data-ttu-id="af661-108">Hello fájlok másolása hello `res/drawable` mappa küldi hello SDK be hello `res/drawable` az alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="af661-108">Copy hello files from hello `res/drawable` folder delivered with hello SDK into hello `res/drawable` folder of your application.</span></span>

<span data-ttu-id="af661-109">Szerkessze a `AndroidManifest.xml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="af661-109">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="af661-110">Adja hozzá a következő szakasz hello (közötti hello `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="af661-110">Add hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
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
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
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
* <span data-ttu-id="af661-111">Az engedély tooreplay rendszerértesítéseket, amely nem rendszerindításkor van szüksége (más módon lemezen megmarad, de nem fognak többé megjelenni valóban telepítette, tooinclude ez).</span><span class="sxs-lookup"><span data-stu-id="af661-111">You need this permission tooreplay system notifications that were not clicked at boot (otherwise they will be kept on disk but won't be displayed anymore, you really have tooinclude this).</span></span>
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* <span data-ttu-id="af661-112">Adjon meg egy ikon másolásával és szerkesztése a következő szakasz hello (az alkalmazás és a rendszer megfelelően) értesítésekhez használatos (közötti hello `<application>` és `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="af661-112">Specify an icon used for notifications (both in app and system ones) by copying and editing hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> <span data-ttu-id="af661-113">Ez a szakasz **kötelező** Ha a kíván használni a rendszer értesítéseket Reach-kampányokat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="af661-113">This section is **mandatory** if you plan on using system notifications when creating Reach campaigns.</span></span> <span data-ttu-id="af661-114">Android megakadályozza, hogy a rendszer értesítéseket ikonok nélkül látható.</span><span class="sxs-lookup"><span data-stu-id="af661-114">Android prevents system notifications without icons from being shown.</span></span> <span data-ttu-id="af661-115">Ezért ez a szakasz kihagyása esetén a végfelhasználók számára nem lesz képes tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="af661-115">So if you omit this section, your end users will not be able tooreceive them.</span></span>
> 
> 

* <span data-ttu-id="af661-116">Rendszerértesítéseket használatával nagy vonalakban tekinti kampányok hoz létre, ha szüksége van-e a következő engedélyek tooadd hello (után hello `</application>` címke) Ha hiányoznak:</span><span class="sxs-lookup"><span data-stu-id="af661-116">If you create campaigns with system notifications using big picture, you need tooadd hello following permissions (after hello `</application>` tag) if missing:</span></span>
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * <span data-ttu-id="af661-117">Az Android m-alapú, és ha az alkalmazás célozza 23 vagy újabb, Android API-szintet ``WRITE_EXTERNAL_STORAGE`` engedély a felhasználó jóváhagyást igényel.</span><span class="sxs-lookup"><span data-stu-id="af661-117">On Android M and if your application targets Android API level 23 or greater, ``WRITE_EXTERNAL_STORAGE`` permission requires user approval.</span></span> <span data-ttu-id="af661-118">Kérjük, olvassa el [ebben a szakaszban](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="af661-118">Please read [this section](mobile-engagement-android-integrate-engagement.md#android-m-permissions).</span></span>
* <span data-ttu-id="af661-119">A rendszer értesítések azt is megadhatja a hello érhetik el kampány hello eszköz kell gyűrű és/vagy a mozog.</span><span class="sxs-lookup"><span data-stu-id="af661-119">For system notifications you can also specify in hello Reach campaign if hello device should ring and/or vibrate.</span></span> <span data-ttu-id="af661-120">Az toowork, rendelkezik arra megtörténjen a deklarálása a következő engedély hello toomake (után hello `</application>` címke):</span><span class="sxs-lookup"><span data-stu-id="af661-120">For it toowork, you have toomake sure you declared hello following permission (after hello `</application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  <span data-ttu-id="af661-121">Ez az engedély nélkül Android megakadályozza, hogy a rendszer értesítéseket megjelenítésének Ha bejelölte hello ring vagy hello mozog hello Reach-kampány manager beállítást.</span><span class="sxs-lookup"><span data-stu-id="af661-121">Without this permission, Android prevents system notifications from being shown if you checked hello ring or hello vibrate option in hello Reach Campaign manager.</span></span>

## <a name="native-push"></a><span data-ttu-id="af661-122">Natív leküldéssel</span><span class="sxs-lookup"><span data-stu-id="af661-122">Native Push</span></span>
<span data-ttu-id="af661-123">Most, hogy konfigurálta a Reach-modul, tooconfigure natív leküldéses toobe képes tooreceive hello kampányok hello eszközön kell.</span><span class="sxs-lookup"><span data-stu-id="af661-123">Now that you configured Reach module, you need tooconfigure native push toobe able tooreceive hello campaigns on hello device.</span></span>

<span data-ttu-id="af661-124">Az Android két szolgáltatás támogatott:</span><span class="sxs-lookup"><span data-stu-id="af661-124">We support two services on Android:</span></span>

* <span data-ttu-id="af661-125">Google Play-eszközök: használata [Google Cloud Messaging] által következő hello [hogyan tooIntegrate az Engagement GCM útmutató](mobile-engagement-android-gcm-integrate.md) útmutató.</span><span class="sxs-lookup"><span data-stu-id="af661-125">Google Play devices: Use [Google Cloud Messaging] by following hello [How tooIntegrate GCM with Engagement guide](mobile-engagement-android-gcm-integrate.md) guide.</span></span>
* <span data-ttu-id="af661-126">Amazon eszközök: használata [Amazon Device Messaging] által következő hello [hogyan tooIntegrate az Engagement ADM útmutató](mobile-engagement-android-adm-integrate.md) útmutató.</span><span class="sxs-lookup"><span data-stu-id="af661-126">Amazon devices: Use [Amazon Device Messaging] by following hello [How tooIntegrate ADM with Engagement guide](mobile-engagement-android-adm-integrate.md) guide.</span></span>

<span data-ttu-id="af661-127">Ha azt szeretné, hogy tootarget Amazon és a Google Play eszközök, a lehetséges toohave fejlesztési 1 AndroidManifest.xml/APK tartalmát.</span><span class="sxs-lookup"><span data-stu-id="af661-127">If you want tootarget both Amazon and Google Play devices, its possible toohave everything inside 1 AndroidManifest.xml/APK for development.</span></span> <span data-ttu-id="af661-128">De tooAmazon terjesztésekor, azok előfordulhat, hogy elutasítja az alkalmazás, ha találnak GCM-kódot.</span><span class="sxs-lookup"><span data-stu-id="af661-128">But when submitting tooAmazon, they may reject your application if they find GCM code.</span></span>

<span data-ttu-id="af661-129">Több APKs ebben az esetben kell használnia.</span><span class="sxs-lookup"><span data-stu-id="af661-129">You should use multiple APKs in that case.</span></span>

<span data-ttu-id="af661-130">**Az alkalmazás most már készen áll a tooreceive és megjelenítési elérni kampányok van!**</span><span class="sxs-lookup"><span data-stu-id="af661-130">**Your application is now ready tooreceive and display reach campaigns!**</span></span>

## <a name="how-toohandle-data-push"></a><span data-ttu-id="af661-131">Toohandle adatok leküldés</span><span class="sxs-lookup"><span data-stu-id="af661-131">How toohandle data push</span></span>
### <a name="integration"></a><span data-ttu-id="af661-132">Integráció</span><span class="sxs-lookup"><span data-stu-id="af661-132">Integration</span></span>
<span data-ttu-id="af661-133">Ha az alkalmazás toobe képes tooreceive Reach adatok leküldéses értesítések, toocreate alosztályának kell `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` és hello vonatkozó hivatkozást `AndroidManifest.xml` fájl (közötti hello `<application>` és/vagy `</application>` címkék):</span><span class="sxs-lookup"><span data-stu-id="af661-133">If you want your application toobe able tooreceive Reach data pushes, you have toocreate a sub-class of `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` and reference it in hello `AndroidManifest.xml` file (between hello `<application>` and/or `</application>` tags):</span></span>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

<span data-ttu-id="af661-134">Akkor lehet felülbírálni hello `onDataPushStringReceived` és `onDataPushBase64Received` visszahívások.</span><span class="sxs-lookup"><span data-stu-id="af661-134">Then you can override hello `onDataPushStringReceived` and `onDataPushBase64Received` callbacks.</span></span> <span data-ttu-id="af661-135">Például:</span><span class="sxs-lookup"><span data-stu-id="af661-135">Here is an example:</span></span>

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a><span data-ttu-id="af661-136">Kategória</span><span class="sxs-lookup"><span data-stu-id="af661-136">Category</span></span>
<span data-ttu-id="af661-137">hello kategória paraméter nem kötelező, amikor adatokat leküldéses kampány létrehozása, és lehetővé teszi, hogy toofilter adatok leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="af661-137">hello category parameter is optional when you create a Data Push campaign and allows you toofilter data pushes.</span></span> <span data-ttu-id="af661-138">Ez akkor hasznos, ha több szórási fogadók adatleküldések, különböző típusú kezelése, vagy ha azt szeretné, hogy toopush különböző a `Base64` adatok és a kívánt tooidentify típusuk őket elemzése előtt.</span><span class="sxs-lookup"><span data-stu-id="af661-138">This is useful if you have several broadcast receivers handling different types of data pushes, or if you want toopush different kinds of `Base64` data and want tooidentify their type before parsing them.</span></span>

### <a name="callbacks-return-parameter"></a><span data-ttu-id="af661-139">A visszahívások visszatérési paraméter</span><span class="sxs-lookup"><span data-stu-id="af661-139">Callbacks' return parameter</span></span>
<span data-ttu-id="af661-140">Az alábbiakban néhány irányelvek tooproperly leíró hello paraméternek `onDataPushStringReceived` és `onDataPushBase64Received`:</span><span class="sxs-lookup"><span data-stu-id="af661-140">Here are some guidelines tooproperly handle hello return parameter of `onDataPushStringReceived` and `onDataPushBase64Received`:</span></span>

* <span data-ttu-id="af661-141">Visszaadja-e a szórásos receiver `null` hello visszahívási, ha egy adatok toohandle leküldés nem ismeri a.</span><span class="sxs-lookup"><span data-stu-id="af661-141">A broadcast receiver should return `null` in hello callback if it does not know how toohandle a data push.</span></span> <span data-ttu-id="af661-142">Hello kategória toodetermine kell használnia, hogy a szórásos receiver hello adatleküldés kezelje-e.</span><span class="sxs-lookup"><span data-stu-id="af661-142">You should use hello category toodetermine whether your broadcast receiver should handle hello data push or not.</span></span>
* <span data-ttu-id="af661-143">Hello szórásos receiver valamelyikét kell visszaadnia `true` a hello visszahívási, ha elfogadja a hello adatleküldés.</span><span class="sxs-lookup"><span data-stu-id="af661-143">One of hello broadcast receiver should return `true` in hello callback if it accepts hello data push.</span></span>
* <span data-ttu-id="af661-144">Hello szórásos receiver valamelyikét kell visszaadnia `false` a hello visszahívási Ha hello adatleküldés felismeri, de bármilyen oknál fogva figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="af661-144">One of hello broadcast receiver should return `false` in hello callback if it recognizes hello data push, but discards it for whatever reason.</span></span> <span data-ttu-id="af661-145">Például vissza `false` érvénytelen hello fogadott adatok esetén.</span><span class="sxs-lookup"><span data-stu-id="af661-145">For example, return `false` when hello received data is invalid.</span></span>
* <span data-ttu-id="af661-146">Ha egy szórási fogadó beolvasása `true` során egy másik egyikét adja vissza `false` hello ugyanazt az adatleküldés, hello viselkedés van definiálva, akkor soha ne tegye, hogy.</span><span class="sxs-lookup"><span data-stu-id="af661-146">If one broadcast receiver returns `true` while another one returns `false` for hello same data push, hello behavior is undefined, you should never do that.</span></span>

<span data-ttu-id="af661-147">hello visszatérési típusa csak hello Reach statisztika szolgál:</span><span class="sxs-lookup"><span data-stu-id="af661-147">hello return type is used only for hello Reach statistics:</span></span>

* <span data-ttu-id="af661-148">`Replied`értéke akkor növekszik, ha az egyik hello szórási fogadók adott vissza, vagy `true` vagy `false`.</span><span class="sxs-lookup"><span data-stu-id="af661-148">`Replied` is incremented if one of hello broadcast receivers returned either `true` or `false`.</span></span>
* <span data-ttu-id="af661-149">`Actioned`értéke akkor nő, csak akkor, ha egy hello szórási visszaadott fogadók `true`.</span><span class="sxs-lookup"><span data-stu-id="af661-149">`Actioned` is incremented only if one of hello broadcast receivers returned `true`.</span></span>

## <a name="how-toocustomize-campaigns"></a><span data-ttu-id="af661-150">Hogyan toocustomize kampányok</span><span class="sxs-lookup"><span data-stu-id="af661-150">How toocustomize campaigns</span></span>
<span data-ttu-id="af661-151">toocustomize kampányok, módosíthatja a Reach SDK hello hello elrendezése.</span><span class="sxs-lookup"><span data-stu-id="af661-151">toocustomize campaigns, you can modify hello layouts provided in hello Reach SDK.</span></span>

<span data-ttu-id="af661-152">Kell hello elrendezések használt összes hello azonosítónak tároljuk és hello nézettípusokat hello azonosítót, különösen a lemezkép és szöveges nézeteket használó róluk.</span><span class="sxs-lookup"><span data-stu-id="af661-152">You should keep all hello identifiers used in hello layouts and keep hello types of hello views that use an identifier, especially for text views and image views.</span></span> <span data-ttu-id="af661-153">Egyes nézetek csak használt toohide vagy szemléltet, ezért a típus módosítható.</span><span class="sxs-lookup"><span data-stu-id="af661-153">Some views are just used toohide or show areas so their type may be changed.</span></span> <span data-ttu-id="af661-154">Tekintse meg a hello forráskódját, ha azt tervezi, toochange hello típusú nézet a megadott hello elrendezés.</span><span class="sxs-lookup"><span data-stu-id="af661-154">Please check hello source code if you intend toochange hello type of a view in hello provided layouts.</span></span>

### <a name="notifications"></a><span data-ttu-id="af661-155">Értesítések</span><span class="sxs-lookup"><span data-stu-id="af661-155">Notifications</span></span>
<span data-ttu-id="af661-156">Az értesítések két típusa van: az elrendezés fájlokat használó rendszer és az alkalmazáson belüli értesítések.</span><span class="sxs-lookup"><span data-stu-id="af661-156">There are two types of notifications: system and in-app notifications which use different layout files.</span></span>

#### <a name="system-notifications"></a><span data-ttu-id="af661-157">Rendszer-értesítések</span><span class="sxs-lookup"><span data-stu-id="af661-157">System notifications</span></span>
<span data-ttu-id="af661-158">toouse hello kell toocustomize rendszerértesítéseket **kategóriák**.</span><span class="sxs-lookup"><span data-stu-id="af661-158">toocustomize system notifications you need toouse hello **categories**.</span></span> <span data-ttu-id="af661-159">Túl is ugorhat[kategóriák](#categories).</span><span class="sxs-lookup"><span data-stu-id="af661-159">You can jump too[Categories](#categories).</span></span>

#### <a name="in-app-notifications"></a><span data-ttu-id="af661-160">Alkalmazáson belüli értesítések</span><span class="sxs-lookup"><span data-stu-id="af661-160">In-app notifications</span></span>
<span data-ttu-id="af661-161">Alapértelmezés szerint egy alkalmazásbeli értesítés egy nézet, amellyel dinamikusan hozzáadott toohello aktuális tevékenység felhasználói felület Köszönjük toohello Android módszer `addContentView()`.</span><span class="sxs-lookup"><span data-stu-id="af661-161">By default, an in-app notification is a view that is dynamically added toohello current activity user interface thanks toohello Android method `addContentView()`.</span></span> <span data-ttu-id="af661-162">Ez a lehetőség egy értesítési területre.</span><span class="sxs-lookup"><span data-stu-id="af661-162">This is called a notification overlay.</span></span> <span data-ttu-id="af661-163">Értesítési átfedések nincsenek rendkívül gyors integrációs, mert nem igényelnek, toomodify bármely elrendezés az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="af661-163">Notification overlays are great for a fast integration because they do not require you toomodify any layout in your application.</span></span>

<span data-ttu-id="af661-164">az értesítési átfedések toomodify hello megjelenését, egyszerűen módosíthatja hello fájlt `engagement_notification_area.xml` tooyour kell.</span><span class="sxs-lookup"><span data-stu-id="af661-164">toomodify hello look of your notification overlays, you can simply modify hello file `engagement_notification_area.xml` tooyour needs.</span></span>

> [!NOTE]
> <span data-ttu-id="af661-165">hello fájl `engagement_notification_overlay.xml` hello tartalmazó használt toocreate egy értesítési területre hello fájl tartalmaz `engagement_notification_area.xml`.</span><span class="sxs-lookup"><span data-stu-id="af661-165">hello file `engagement_notification_overlay.xml` is hello one that is used toocreate a notification overlay, it includes hello file `engagement_notification_area.xml`.</span></span> <span data-ttu-id="af661-166">Testre is szabhatja az toosuit az igényeinek, (például hello értesítési területen belül hello átfedő elhelyezéséhez esetében).</span><span class="sxs-lookup"><span data-stu-id="af661-166">You can also customize it toosuit your needs (such as for positioning hello notification area within hello overlay).</span></span>
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a><span data-ttu-id="af661-167">Egy tevékenység elrendezés részeként értesítési elrendezés tartalmazza</span><span class="sxs-lookup"><span data-stu-id="af661-167">Include notification layout as part of an activity layout</span></span>
<span data-ttu-id="af661-168">Átfedések ideális az egy gyors integrációt, de lehet kényelmetlen vagy mellékhatásokkal bizonyos esetekben.</span><span class="sxs-lookup"><span data-stu-id="af661-168">Overlays are great for a fast integration but can be inconvenient or have side effects in special cases.</span></span> <span data-ttu-id="af661-169">hello átfedő rendszer testre szabható tevékenység szinten, így könnyen tooprevent hatásai különleges tevékenységekhez.</span><span class="sxs-lookup"><span data-stu-id="af661-169">hello overlay system can be customized at an activity level, making it easy tooprevent side effects for special activities.</span></span>

<span data-ttu-id="af661-170">Dönthet úgy is tooinclude az értesítési elrendezés a a meglévő elrendezés Köszönjük toohello Android **tartalmaznak** utasítást.</span><span class="sxs-lookup"><span data-stu-id="af661-170">You can decide tooinclude our notification layout in your existing layout thanks toohello Android **include** statement.</span></span> <span data-ttu-id="af661-171">hello az alábbiakban látható egy példa egy módosított `ListActivity` elrendezést tartalmazó csak egy `ListView`.</span><span class="sxs-lookup"><span data-stu-id="af661-171">hello following is an example of a modified `ListActivity` layout containing just a `ListView`.</span></span>

<span data-ttu-id="af661-172">**Mielőtt Engagement-integráció:**</span><span class="sxs-lookup"><span data-stu-id="af661-172">**Before Engagement integration :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

<span data-ttu-id="af661-173">**Miután Engagement-integráció:**</span><span class="sxs-lookup"><span data-stu-id="af661-173">**After Engagement integration :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

<span data-ttu-id="af661-174">Ebben a példában hozzáadott egy szülőtároló mert hello eredeti elrendezés egy listanézeti hello felső szintű eleme.</span><span class="sxs-lookup"><span data-stu-id="af661-174">In this example we added a parent container since hello original layout used a list view as hello top level element.</span></span> <span data-ttu-id="af661-175">Is hozzáadtunk `android:layout_weight="1"` toobe képes tooadd egy nézet egy listanézeti alatt konfigurált `android:layout_height="fill_parent"`.</span><span class="sxs-lookup"><span data-stu-id="af661-175">We also added `android:layout_weight="1"` toobe able tooadd a view below a list view configured with `android:layout_height="fill_parent"`.</span></span>

<span data-ttu-id="af661-176">hello Engagement Reach SDK automatikusan észleli, hogy hello értesítési elrendezés ezt a tevékenységet tartalmazza, és nem adja hozzá egy átmeneti területre, ehhez a tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="af661-176">hello Engagement Reach SDK automatically detects that hello notification layout is included in this activity and will not add an overlay for this activity.</span></span>

> [!TIP]
> <span data-ttu-id="af661-177">Az alkalmazás egy ListActivity használja, ha egy látható Reach területre elkerülheti reakciójú tooclicked elemek hello listanézetben többé.</span><span class="sxs-lookup"><span data-stu-id="af661-177">If you use a ListActivity in your application, a visible Reach overlay will prevent you from reacting tooclicked items in hello list view anymore.</span></span> <span data-ttu-id="af661-178">Ez az egy ismert probléma.</span><span class="sxs-lookup"><span data-stu-id="af661-178">This is a known issue.</span></span> <span data-ttu-id="af661-179">toowork a probléma megoldásához javasoljuk, hogy tooembed hello értesítési elrendezés saját lista tevékenység elrendezésben, például a hello korábbi minta.</span><span class="sxs-lookup"><span data-stu-id="af661-179">toowork around this problem we suggest you tooembed hello notification layout in your own list activity layout like in hello previous sample.</span></span>
> 
> 

##### <a name="disabling-application-notification-per-activity"></a><span data-ttu-id="af661-180">/ Tevékenység alkalmazás értesítés letiltása</span><span class="sxs-lookup"><span data-stu-id="af661-180">Disabling application notification per activity</span></span>
<span data-ttu-id="af661-181">Ha nem szeretné, hogy hello átfedő toobe adnak tooyour tevékenység, és ha nincs hello értesítési elrendezés saját elrendezésben, letilthatja a hello felirat a tevékenységnek hello `AndroidManifest.xml` hozzáadásával egy `meta-data` szakasz hasonlóan a hello következő Példa:</span><span class="sxs-lookup"><span data-stu-id="af661-181">If you don't want hello overlay toobe added tooyour activity, and if you don't include hello notification layout in your own layout, you can disable hello overlay for this activity in hello `AndroidManifest.xml` by adding a `meta-data` section like in hello following example:</span></span>

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <span data-ttu-id="af661-182"><a name="categories"></a>Kategóriák</span><span class="sxs-lookup"><span data-stu-id="af661-182"><a name="categories"></a> Categories</span></span>
<span data-ttu-id="af661-183">Megadott elrendezések hello módosításakor az értesítések hello megjelenésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="af661-183">When you modify hello provided layouts, you modify hello look of all your notifications.</span></span> <span data-ttu-id="af661-184">Kategóriák engedélyezése toodefine, különböző megcélzott értesítések (valószínűleg viselkedések) keresi.</span><span class="sxs-lookup"><span data-stu-id="af661-184">Categories allow you toodefine various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="af661-185">A kategória a Reach-kampány létrehozásakor adható meg.</span><span class="sxs-lookup"><span data-stu-id="af661-185">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="af661-186">Ne feledje, hogy kategóriák is segítségével testre szabható mutató hirdetmények és szavazások, a dokumentum későbbi szakaszában ismertetett.</span><span class="sxs-lookup"><span data-stu-id="af661-186">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="af661-187">az értesítések kategória leíró tooregister, kell tooadd hívás hello alkalmazás inicializálásakor.</span><span class="sxs-lookup"><span data-stu-id="af661-187">tooregister a category handler for your notifications, you need tooadd a call when hello application is initialized.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="af661-188">Olvassa el a hello figyelmeztetést jelenít meg hello android: folyamat attribútum \<android-sdk-engagement-folyamat\> a hello hogyan tooIntegrate Engagement Android témakör a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="af661-188">Please read hello warning about hello android:process attribute \<android-sdk-engagement-process\> in hello How tooIntegrate Engagement on Android topic before proceeding.</span></span>
> 
> 

<span data-ttu-id="af661-189">hello alábbi példa azt feltételezi, hogy Ön hello előző figyelmeztetés vonatkozik, és alosztályának `EngagementApplication`:</span><span class="sxs-lookup"><span data-stu-id="af661-189">hello following example assumes you acknowledged hello previous warning and use a sub-class of `EngagementApplication`:</span></span>

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

<span data-ttu-id="af661-190">Hello `MyNotifier` objektum hello kategória értesítéskezelő hello végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="af661-190">hello `MyNotifier` object is hello implementation of hello notification category handler.</span></span> <span data-ttu-id="af661-191">Hello, vagy egy megvalósított `EngagementNotifier` illesztőfelület vagy hello alapértelmezett végrehajtásának sub osztály: `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="af661-191">It is either an implementation of hello `EngagementNotifier` interface or a sub class of hello default implementation: `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="af661-192">Vegye figyelembe, hogy ugyanazon bejelentő hello kezelni tud a különböző kategóriákba, ilyen regisztrálhatja:</span><span class="sxs-lookup"><span data-stu-id="af661-192">Note that hello same notifier can handle several categories, you can register them like this:</span></span>

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

<span data-ttu-id="af661-193">tooreplace hello alapértelmezett kategória implementációja regisztrálhatja a megvalósítás, például a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="af661-193">tooreplace hello default category implementation, you can register your implementation like in hello following example:</span></span>

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

<span data-ttu-id="af661-194">a kezelő használt hello aktuális kategória felülbírálhatja a legtöbb módszer paraméterként átadott `EngagementDefaultNotifier`.</span><span class="sxs-lookup"><span data-stu-id="af661-194">hello current category used in a handler is passed as a parameter in most methods you can override in `EngagementDefaultNotifier`.</span></span>

<span data-ttu-id="af661-195">Átadva akár egy `String` paraméter vagy közvetetten egy `EngagementReachContent` objektum, amely rendelkezik egy `getCategory()` metódus.</span><span class="sxs-lookup"><span data-stu-id="af661-195">It is passed either as a `String` parameter or indirectly in a `EngagementReachContent` object which has a `getCategory()` method.</span></span>

<span data-ttu-id="af661-196">Módosíthatja a legtöbb hello értesítési létrehozási folyamata módszerek az újradefiniálás `EngagementDefaultNotifier`, amely speciális testreszabás érzi, hogy az ingyenes tootake hello műszaki dokumentáció és hello forráskód meg.</span><span class="sxs-lookup"><span data-stu-id="af661-196">You can change most of hello notification creation process by redefining methods on `EngagementDefaultNotifier`, for more advanced customization feel free tootake a look at hello technical documentation and at hello source code.</span></span>

##### <a name="in-app-notifications"></a><span data-ttu-id="af661-197">Alkalmazáson belüli értesítések</span><span class="sxs-lookup"><span data-stu-id="af661-197">In-app notifications</span></span>
<span data-ttu-id="af661-198">Ha csak toouse alternatív elrendezés az egy adott konkrét kategóriával, mint például a következő hello Megvalósíthat ez:</span><span class="sxs-lookup"><span data-stu-id="af661-198">If you just want toouse alternate layouts for a specific category, you can implement this as in hello following example:</span></span>

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

<span data-ttu-id="af661-199">**Példa `my_notification_overlay.xml` :**</span><span class="sxs-lookup"><span data-stu-id="af661-199">**Example of `my_notification_overlay.xml` :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

<span data-ttu-id="af661-200">Ahogy látja, hello átfedő nézet azonosító hello szabványos egy másik.</span><span class="sxs-lookup"><span data-stu-id="af661-200">As you can see, hello overlay view identifier is different than hello standard one.</span></span> <span data-ttu-id="af661-201">Fontos, hogy minden elrendezés átfedések egy egyedi azonosítót használjon.</span><span class="sxs-lookup"><span data-stu-id="af661-201">It is important that each layout use a unique identifier for overlays.</span></span>

<span data-ttu-id="af661-202">**Példa `my_notification_area.xml` :**</span><span class="sxs-lookup"><span data-stu-id="af661-202">**Example of `my_notification_area.xml` :**</span></span>

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

<span data-ttu-id="af661-203">Ahogy látja, hello értesítési terület nézet azonosító hello szabványos egy másik.</span><span class="sxs-lookup"><span data-stu-id="af661-203">As you can see, hello notification area view identifier is different than hello standard one.</span></span> <span data-ttu-id="af661-204">Fontos, hogy minden elrendezés egy egyedi azonosítót használ, az értesítési terület.</span><span class="sxs-lookup"><span data-stu-id="af661-204">It is important that each layout uses a unique identifier for notification areas.</span></span>

<span data-ttu-id="af661-205">Egyszerű példa a kategória lehetővé teszi az alkalmazás (vagy alkalmazásbeli) értesítések, üdvözlő képernyőt hello tetején jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="af661-205">This simple example of category makes application (or in-app) notifications displayed at hello top of hello screen.</span></span> <span data-ttu-id="af661-206">A Microsoft hello hello értesítési területen maga használt szabványos azonosítókat nem változott.</span><span class="sxs-lookup"><span data-stu-id="af661-206">We did not change hello standard identifiers used in hello notification area itself.</span></span>

<span data-ttu-id="af661-207">Ha azt szeretné, hogy tooredefine hello toochange `EngagementDefaultNotifier.prepareInAppArea` metódust.</span><span class="sxs-lookup"><span data-stu-id="af661-207">If you want toochange that, you have tooredefine hello `EngagementDefaultNotifier.prepareInAppArea` method.</span></span> <span data-ttu-id="af661-208">Ajánlott toolook hello műszaki dokumentáció és annak forráskódját hello `EngagementNotifier` és `EngagementDefaultNotifier` Ha azt szeretné, a szint speciális.</span><span class="sxs-lookup"><span data-stu-id="af661-208">It's recommended toolook at hello technical documentation and at hello source code of `EngagementNotifier` and `EngagementDefaultNotifier` if you want this level of advanced customization.</span></span>

##### <a name="system-notifications"></a><span data-ttu-id="af661-209">Rendszer-értesítések</span><span class="sxs-lookup"><span data-stu-id="af661-209">System notifications</span></span>
<span data-ttu-id="af661-210">Kiterjesztésével `EngagementDefaultNotifier`, ha szeretné felülbírálni az `onNotificationPrepared` tooalter hello értesítési hello alapértelmezett megvalósítás előkészített.</span><span class="sxs-lookup"><span data-stu-id="af661-210">By extending `EngagementDefaultNotifier`, you can override `onNotificationPrepared` tooalter hello notification that was prepared by hello default implementation.</span></span>

<span data-ttu-id="af661-211">Példa:</span><span class="sxs-lookup"><span data-stu-id="af661-211">For example:</span></span>

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

<span data-ttu-id="af661-212">Ez a példa jeleníti meg a folyamatban lévő esemény hello "folyamatban" kategóriát használatakor tartalom rendszerértesítőként teszi.</span><span class="sxs-lookup"><span data-stu-id="af661-212">This example makes a system notification for a content being displayed as an ongoing event when hello "ongoing" category is used.</span></span>

<span data-ttu-id="af661-213">Ha azt szeretné, hogy toobuild hello `Notification` objektum teljesen új, lépjen vissza `false` toohello metódus és hívás `notify` saját kezűleg a hello `NotificationManager`.</span><span class="sxs-lookup"><span data-stu-id="af661-213">If you want toobuild hello `Notification` object from scratch, you can return `false` toohello method and call `notify` yourself on hello `NotificationManager`.</span></span> <span data-ttu-id="af661-214">Ebben az esetben fontos, hogy maradjon egy `contentIntent`, egy `deleteIntent` és által használt értesítési azonosítójában hello `EngagementReachReceiver`.</span><span class="sxs-lookup"><span data-stu-id="af661-214">In that case it's important that you keep a `contentIntent`, a `deleteIntent` and hello notification identifier used by `EngagementReachReceiver`.</span></span>

<span data-ttu-id="af661-215">Íme egy végrehajtás megfelelő példát:</span><span class="sxs-lookup"><span data-stu-id="af661-215">Here is a correct example of such an implementation:</span></span>

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice hello call tooget hello right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a><span data-ttu-id="af661-216">Csak értesítést tartalmazó hirdetmények</span><span class="sxs-lookup"><span data-stu-id="af661-216">Notification only announcements</span></span>
<span data-ttu-id="af661-217">hello hello vezetése kattintson a csak közlemény testre szabható felülbírálásával értesítést `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` előkészített toomodify hello `Intent`.</span><span class="sxs-lookup"><span data-stu-id="af661-217">hello management of hello click on a notification only announcement can be customized by overriding `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` toomodify hello prepared `Intent`.</span></span> <span data-ttu-id="af661-218">Ezzel a módszerrel teszi tootune hello jelzők könnyen.</span><span class="sxs-lookup"><span data-stu-id="af661-218">Using this method allows you tootune hello flags easily.</span></span>

<span data-ttu-id="af661-219">Például tooadd hello `SINGLE_TOP` jelző:</span><span class="sxs-lookup"><span data-stu-id="af661-219">For example tooadd hello `SINGLE_TOP` flag:</span></span>

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

<span data-ttu-id="af661-220">A hagyományos Engagement felhasználók számára vegye figyelembe, hogy rendszerüzenetek nélkül művelet URL-CÍMÉT most elindítja hello alkalmazást ha háttérben, így ez a metódus meghívható a bejelentés nélkül műveleti URL-cím.</span><span class="sxs-lookup"><span data-stu-id="af661-220">For legacy Engagement users, please note that system notifications without action URL now launches hello application if it was in background, so this method can be called with an announcement without action URL.</span></span> <span data-ttu-id="af661-221">Vegye figyelembe, hogy hello leképezés testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="af661-221">You should consider that when customizing hello intent.</span></span>

<span data-ttu-id="af661-222">Is megvalósíthatja `EngagementNotifier.executeNotifAnnouncementAction` teljesen.</span><span class="sxs-lookup"><span data-stu-id="af661-222">You can also implement `EngagementNotifier.executeNotifAnnouncementAction` from scratch.</span></span>

##### <a name="notification-life-cycle"></a><span data-ttu-id="af661-223">Értesítési életciklusa</span><span class="sxs-lookup"><span data-stu-id="af661-223">Notification life cycle</span></span>
<span data-ttu-id="af661-224">Hello alapértelmezett kategóriát használatakor a hello egyes életciklus metódusok nevezik `EngagementReachInteractiveContent` objektum tooreport statisztikák és a frissítés hello kampány állapota:</span><span class="sxs-lookup"><span data-stu-id="af661-224">When using hello default category, some life cycle methods are called on hello `EngagementReachInteractiveContent` object tooreport statistics and update hello campaign state:</span></span>

* <span data-ttu-id="af661-225">Hello értesítés jelenik meg az alkalmazás vagy hello állapotsor be, amikor hello `displayNotification` módszer neve (amely statisztikáit) által `EngagementReachAgent` Ha `handleNotification` adja vissza `true`.</span><span class="sxs-lookup"><span data-stu-id="af661-225">When hello notification is displayed in application or put in hello status bar, hello `displayNotification` method is called (which reports statistics) by `EngagementReachAgent` if `handleNotification` returns `true`.</span></span>
* <span data-ttu-id="af661-226">Ha hello értesítési eltűnése hello `exitNotification` metódus lehívásra kerül, statisztika jelez, és tovább kampányok most már tudja feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="af661-226">If hello notification is dismissed, hello `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="af661-227">Ha hello értesítési kattint, `actionNotification` van nevű statisztika jelez, és a kapcsolódó hello leképezés lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="af661-227">If hello notification is clicked, `actionNotification` is called, statistic is reported and hello associated intent is launched.</span></span>

<span data-ttu-id="af661-228">Ha a végrehajtásának `EngagementNotifier` Mellőzés hello alapértelmezett viselkedés telepítette, toocall életciklus módszerekhez önállóan.</span><span class="sxs-lookup"><span data-stu-id="af661-228">If your implementation of `EngagementNotifier` bypasses hello default behavior, you have toocall these life cycle methods by yourself.</span></span> <span data-ttu-id="af661-229">hello a következő példák bemutatják, egyes esetekben, ahol hello alapértelmezett viselkedés elmarad:</span><span class="sxs-lookup"><span data-stu-id="af661-229">hello following examples illustrate some cases where hello default behavior is bypassed:</span></span>

* <span data-ttu-id="af661-230">Nem terjeszti ki `EngagementDefaultNotifier`, pl. megvalósította a teljesen új kategória kezelését.</span><span class="sxs-lookup"><span data-stu-id="af661-230">You don't extend `EngagementDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="af661-231">A rendszer értesítések, üdvözlő overrode `onNotificationPrepared` és módosított `contentIntent` vagy `deleteIntent` a hello `Notification` objektum.</span><span class="sxs-lookup"><span data-stu-id="af661-231">For system notifications, you overrode hello `onNotificationPrepared` and you modified `contentIntent` or `deleteIntent` in hello `Notification` object.</span></span>
* <span data-ttu-id="af661-232">Az alkalmazásbeli értesítések overrode `prepareInAppArea`, nem lehet rövidebb meg arról, hogy toomap `actionNotification` tooone a U.I vezérlők.</span><span class="sxs-lookup"><span data-stu-id="af661-232">For in-app notifications, you overrode `prepareInAppArea`, be sure toomap at least `actionNotification` tooone of your U.I controls.</span></span>

> [!NOTE]
> <span data-ttu-id="af661-233">Ha `handleNotification` jelez kivétel, hello tartalom törlése és `dropContent` nevezik.</span><span class="sxs-lookup"><span data-stu-id="af661-233">If `handleNotification` throws an exception, hello content is deleted and `dropContent` is called.</span></span> <span data-ttu-id="af661-234">Ez a statisztika jelez, és tovább kampányok most már tudja feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="af661-234">This is reported in statistics and next campaigns can now be processed.</span></span>
> 
> 

### <a name="announcements-and-polls"></a><span data-ttu-id="af661-235">Hirdetmények és szavazások esetén</span><span class="sxs-lookup"><span data-stu-id="af661-235">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="af661-236">Elrendezés</span><span class="sxs-lookup"><span data-stu-id="af661-236">Layouts</span></span>
<span data-ttu-id="af661-237">Módosíthatja a hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` és `engagement_poll.xml` fájlok toocustomize szöveg közlemények, webes mutató hirdetmények és szavazások esetén.</span><span class="sxs-lookup"><span data-stu-id="af661-237">You can modify hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` and `engagement_poll.xml` files toocustomize text announcements, web announcements and polls.</span></span>

<span data-ttu-id="af661-238">Ezeket a fájlokat két közös elrendezések hello Címterület és hello gomb terület megosztani.</span><span class="sxs-lookup"><span data-stu-id="af661-238">These files share two common layouts for hello title area and hello button area.</span></span> <span data-ttu-id="af661-239">hello elrendezését hello cím `engagement_content_title.xml` és felhasználási hello hello háttér városról rajzolható fájl.</span><span class="sxs-lookup"><span data-stu-id="af661-239">hello layout for hello title is `engagement_content_title.xml` and uses hello eponymous drawable file for hello background.</span></span> <span data-ttu-id="af661-240">hello hello akciógombok és kilépési gombok elrendezése van `engagement_button_bar.xml` és felhasználási hello hello háttér városról rajzolható fájl.</span><span class="sxs-lookup"><span data-stu-id="af661-240">hello layout for hello action and exit buttons is `engagement_button_bar.xml` and uses hello eponymous drawable file for hello background.</span></span>

<span data-ttu-id="af661-241">A szavazás, hello kérdés elrendezés és a választási lehetőségek dinamikusan vannak növelve többször hello segítségével `engagement_question.xml` elrendezésfájlt hello kérdések és hello `engagement_choice.xml` fájl hello lehetőségekért.</span><span class="sxs-lookup"><span data-stu-id="af661-241">In a poll, hello question layout and their choices are dynamically inflated using several times hello `engagement_question.xml` layout file for hello questions and hello `engagement_choice.xml` file for hello choices.</span></span>

#### <a name="categories"></a><span data-ttu-id="af661-242">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="af661-242">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="af661-243">Alternatív elrendezések</span><span class="sxs-lookup"><span data-stu-id="af661-243">Alternate layouts</span></span>
<span data-ttu-id="af661-244">Értesítések, például a hello kampány kategória lehet használt toohave alternatív elrendezések a hirdetmények és szavazások esetén.</span><span class="sxs-lookup"><span data-stu-id="af661-244">Like notifications, hello campaign's category can be used toohave alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="af661-245">Például egy kategóriát a szöveg közlemény toocreate, kiterjesztheti `EngagementTextAnnouncementActivity` és hivatkozni rá hello `AndroidManifest.xml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="af661-245">For example, toocreate a category for a text announcement, you can extend `EngagementTextAnnouncementActivity` and reference it hello `AndroidManifest.xml` file:</span></span>

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

<span data-ttu-id="af661-246">Vegye figyelembe a hello leképezés hello kategóriát szűrővel toomake hello különbség hello alapértelmezett közlemény tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="af661-246">Note that hello category in hello intent filter is used toomake hello difference with hello default announcement activity.</span></span>

<span data-ttu-id="af661-247">hello Reach SDK egy adott konkrét kategóriával hello leképezési rendszer tooresolve hello tevékenységre használ, és azt visszavált hello alapértelmezett kategórián Ha hello feloldása nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="af661-247">hello Reach SDK uses hello intent system tooresolve hello right activity for a specific category and it falls back on hello default category if hello resolution failed.</span></span>

<span data-ttu-id="af661-248">Akkor el kell tooimplement `MyCustomTextAnnouncementActivity`, ha csak szeretné, hogy toochange hello elrendezés (, de tartsa hello azonos nézet azonosítók), csak rendelkezik toodefine hello osztály, például a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="af661-248">Then you have tooimplement `MyCustomTextAnnouncementActivity`, if you just want toochange hello layout (but keep hello same view identifiers), you just have toodefine hello class like in hello following example:</span></span>

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class toouse R.layout.my_text_announcement
              }
            }

<span data-ttu-id="af661-249">tooreplace hello alapértelmezett kategória szöveg hirdetmények, egyszerűen cserélje le `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` a megvalósítás.</span><span class="sxs-lookup"><span data-stu-id="af661-249">tooreplace hello default category of text announcements, simply replace `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` by your implementation.</span></span>

<span data-ttu-id="af661-250">Webes mutató hirdetmények és szavazások hasonló módon szabhatja testre.</span><span class="sxs-lookup"><span data-stu-id="af661-250">Web announcements and polls can be customized in a similar fashion.</span></span>

<span data-ttu-id="af661-251">Webes hirdetmények kiterjesztheti `EngagementWebAnnouncementActivity` és a tevékenységeket hello `AndroidManifest.xml` , például az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="af661-251">For web announcements you can extend `EngagementWebAnnouncementActivity` and declare your activity in hello `AndroidManifest.xml` like in hello following example:</span></span>

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in hello intent is hello data mime type -->
              </intent-filter>
            </activity>

<span data-ttu-id="af661-252">A lekérdezések kiterjesztheti `EngagementPollActivity` és a a hello `AndroidManifest.xml` , például az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="af661-252">For polls you can extend `EngagementPollActivity` and declare your in hello `AndroidManifest.xml` like in hello following example:</span></span>

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a><span data-ttu-id="af661-253">Teljesen új végrehajtása</span><span class="sxs-lookup"><span data-stu-id="af661-253">Implementation from scratch</span></span>
<span data-ttu-id="af661-254">Megvalósíthat kategóriák a közlemény (és a lekérdezési) tevékenységek hello egyik kiterjesztése nélkül `Engagement*Activity` osztályok által biztosított hello Reach SDK.</span><span class="sxs-lookup"><span data-stu-id="af661-254">You can implement categories for your announcement (and poll) activities without extending one of hello `Engagement*Activity` classes provided by hello Reach SDK.</span></span> <span data-ttu-id="af661-255">Ez akkor hasznos, például ha azt szeretné, toodefine nem használja, mint szabványos elrendezések hello azonos megtekinti hello elrendezés.</span><span class="sxs-lookup"><span data-stu-id="af661-255">This is useful for example if you want toodefine a layout that does not use hello same views as hello standard layouts.</span></span>

<span data-ttu-id="af661-256">Például a speciális értesítési testreszabáshoz ajánlott: hello forráskódját hello szabványos implementációjától toolook.</span><span class="sxs-lookup"><span data-stu-id="af661-256">Like for advanced notification customization, it is recommended toolook at hello source code of hello standard implementation.</span></span>

<span data-ttu-id="af661-257">Az alábbiakban néhány dolgot tookeep figyelembe vételével: Reach hello tevékenység egy adott biztonsági mentés (megfelelő toohello leképezési szűrő) fog elindulni, plusz egy kiegészítő paraméterrel, amely hello tartalmi azonosító.</span><span class="sxs-lookup"><span data-stu-id="af661-257">Here are some things tookeep in mind: Reach will launch hello activity with a specific intent (corresponding toohello intent filter) plus an extra parameter which is hello content identifier.</span></span>

<span data-ttu-id="af661-258">tooretrieve hello tartalom objektum mikor hello létrehozása kampány hello webhelyen, megadott hello mezőket tartalmazó teheti meg:</span><span class="sxs-lookup"><span data-stu-id="af661-258">tooretrieve hello content object which contain hello fields you specified when creating hello campaign on hello web site you can do this:</span></span>

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

<span data-ttu-id="af661-259">A statisztika, jelentse hello tartalom hello jelenik meg `onResume` esemény:</span><span class="sxs-lookup"><span data-stu-id="af661-259">For statistics, you should report hello content is displayed in hello `onResume` event:</span></span>

            @Override
            protected void onResume()
            {
             /* Mark hello content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

<span data-ttu-id="af661-260">Ezt követően ne feledje toocall vagy `actionContent(this)` vagy `exitContent(this)` hello tartalom objektum előtt hello tevékenység háttér állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="af661-260">Then, don't forget toocall either `actionContent(this)` or `exitContent(this)` on hello content object before hello activity goes into background.</span></span>

<span data-ttu-id="af661-261">Ha nem hívható meg vagy `actionContent` vagy `exitContent`, statisztika nem küldhető el (azaz nem elemzés hello kampány), és több ami még fontosabb – hello következő kampányok nem kap értesítést hello alkalmazás folyamatának újraindításáig.</span><span class="sxs-lookup"><span data-stu-id="af661-261">If you don't call either `actionContent` or `exitContent`, statistics won't be sent (i.e. no analytics on hello campaign) and more importantly, hello next campaigns will not be notified until hello application process is restarted.</span></span>

<span data-ttu-id="af661-262">Tájolás vagy egyéb konfigurációs módosítások végrehajtása is hello kód legbonyolultabb toodetermine ellenőrizze, hogy hello tevékenység háttér állapotba kerül, vagy nem, szabványos implementációjától teszi, hogy hello tartalom készként való kilépett hello felhasználó elhagyja hello tevékenység (vagy ha hello nyomja le `HOME` vagy `BACK`) kivéve, ha hello tájolás változik.</span><span class="sxs-lookup"><span data-stu-id="af661-262">Orientation or other configuration changes can make hello code tricky toodetermine whether hello activity goes into background or not, hello standard implementation makes sure hello content is reported as exited if hello user leaves hello activity (either by pressing `HOME` or `BACK`) but not if hello orientation changes.</span></span>

<span data-ttu-id="af661-263">Itt része hello érdekes hello végrehajtására:</span><span class="sxs-lookup"><span data-stu-id="af661-263">Here is hello interesting part of hello implementation:</span></span>

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have toocheck anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

<span data-ttu-id="af661-264">Ahogy látja, ha a hívott `actionContent(this)` hello tevékenység befejeződött, majd `exitContent(this)` anélkül, hogy bármilyen hatása biztonságosan hívható.</span><span class="sxs-lookup"><span data-stu-id="af661-264">As you can see, if you called `actionContent(this)` then finished hello activity, `exitContent(this)` can be safely called without having any effect.</span></span>

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
