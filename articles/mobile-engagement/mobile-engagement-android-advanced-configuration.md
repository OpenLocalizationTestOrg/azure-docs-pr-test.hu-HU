---
title: "Azure Mobile Engagement Android SDK aaaAdvanced konfigurációja"
description: "Speciális konfigurációs beállítások, beleértve az Azure Mobile Engagement Android SDK-val Android Manifest hello hello ismerteti"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 757abf362021fd018f444cae6305524623e77062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="16b8f-103">Az Azure Mobile Engagement Android SDK speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="16b8f-103">Advanced configuration for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="16b8f-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="16b8f-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="16b8f-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="16b8f-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="16b8f-106">iOS</span><span class="sxs-lookup"><span data-stu-id="16b8f-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="16b8f-107">Android</span><span class="sxs-lookup"><span data-stu-id="16b8f-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
>
>

<span data-ttu-id="16b8f-108">Ez az eljárás ismerteti, hogyan tooconfigure Azure Mobile Engagement Android-alkalmazások különböző konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="16b8f-108">This procedure describes how tooconfigure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16b8f-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="16b8f-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a><span data-ttu-id="16b8f-110">Engedélykövetelmények</span><span class="sxs-lookup"><span data-stu-id="16b8f-110">Permission Requirements</span></span>
<span data-ttu-id="16b8f-111">Néhány beállításhoz konkrét engedélyeket, amelyek az itt felsorolt hivatkozást, és beágyazott hello adott funkciójában szükségesek.</span><span class="sxs-lookup"><span data-stu-id="16b8f-111">Some options require specific permissions, all of which are listed here for reference, and in-line in hello specific feature.</span></span> <span data-ttu-id="16b8f-112">Ezen engedélyek toohello AndroidManifest.xml a projekt hozzáadása előtt vagy után hello `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="16b8f-112">Add these permissions toohello AndroidManifest.xml of your project immediately before or after hello `<application>` tag.</span></span>

<span data-ttu-id="16b8f-113">hello engedély kód toolook hello következő, ahol hello megfelelő engedélyekkel az alábbi hello táblából kitöltése szükséges.</span><span class="sxs-lookup"><span data-stu-id="16b8f-113">hello permission code needs toolook like hello following, where you fill in hello appropriate permission from hello table that follows.</span></span>

    <uses-permission android:name="android.permission.[specific permission]"/>


| <span data-ttu-id="16b8f-114">Engedély</span><span class="sxs-lookup"><span data-stu-id="16b8f-114">Permission</span></span> | <span data-ttu-id="16b8f-115">Használata esetén</span><span class="sxs-lookup"><span data-stu-id="16b8f-115">When used</span></span> |
| --- | --- |
| <span data-ttu-id="16b8f-116">INTERNET</span><span class="sxs-lookup"><span data-stu-id="16b8f-116">INTERNET</span></span> |<span data-ttu-id="16b8f-117">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="16b8f-117">Required.</span></span> <span data-ttu-id="16b8f-118">Alapszintű jelentéskészítéshez</span><span class="sxs-lookup"><span data-stu-id="16b8f-118">For basic reporting</span></span> |
| <span data-ttu-id="16b8f-119">ACCESS_NETWORK_STATE</span><span class="sxs-lookup"><span data-stu-id="16b8f-119">ACCESS_NETWORK_STATE</span></span> |<span data-ttu-id="16b8f-120">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="16b8f-120">Required.</span></span> <span data-ttu-id="16b8f-121">Alapszintű jelentéskészítéshez</span><span class="sxs-lookup"><span data-stu-id="16b8f-121">For basic reporting</span></span> |
| <span data-ttu-id="16b8f-122">RECEIVE_BOOT_COMPLETED</span><span class="sxs-lookup"><span data-stu-id="16b8f-122">RECEIVE_BOOT_COMPLETED</span></span> |<span data-ttu-id="16b8f-123">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="16b8f-123">Required.</span></span> <span data-ttu-id="16b8f-124">tooshow mentése hello értesítések center eszköz újraindítását követően.</span><span class="sxs-lookup"><span data-stu-id="16b8f-124">tooshow up hello notifications center after device reboot</span></span> |
| <span data-ttu-id="16b8f-125">WAKE_LOCK</span><span class="sxs-lookup"><span data-stu-id="16b8f-125">WAKE_LOCK</span></span> |<span data-ttu-id="16b8f-126">Ajánlott.</span><span class="sxs-lookup"><span data-stu-id="16b8f-126">Recommended.</span></span> <span data-ttu-id="16b8f-127">Lehetővé teszi, hogy Wi-Fi használata esetén, vagy ha a képernyőn a rendszer nem végez adatgyűjtést</span><span class="sxs-lookup"><span data-stu-id="16b8f-127">Enables collecting data when using WiFi or when screen is off</span></span> |
| <span data-ttu-id="16b8f-128">MOZOG</span><span class="sxs-lookup"><span data-stu-id="16b8f-128">VIBRATE</span></span> |<span data-ttu-id="16b8f-129">Választható.</span><span class="sxs-lookup"><span data-stu-id="16b8f-129">Optional.</span></span> <span data-ttu-id="16b8f-130">Lehetővé teszi, hogy vibráció értesítések fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="16b8f-130">Enables vibration when notifications are received</span></span> |
| <span data-ttu-id="16b8f-131">DOWNLOAD_WITHOUT_NOTIFICATION</span><span class="sxs-lookup"><span data-stu-id="16b8f-131">DOWNLOAD_WITHOUT_NOTIFICATION</span></span> |<span data-ttu-id="16b8f-132">Választható.</span><span class="sxs-lookup"><span data-stu-id="16b8f-132">Optional.</span></span> <span data-ttu-id="16b8f-133">Lehetővé teszi, hogy Android nagy vonalakban tekinti értesítés</span><span class="sxs-lookup"><span data-stu-id="16b8f-133">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="16b8f-134">WRITE_EXTERNAL_STORAGE</span><span class="sxs-lookup"><span data-stu-id="16b8f-134">WRITE_EXTERNAL_STORAGE</span></span> |<span data-ttu-id="16b8f-135">Választható.</span><span class="sxs-lookup"><span data-stu-id="16b8f-135">Optional.</span></span> <span data-ttu-id="16b8f-136">Lehetővé teszi, hogy Android nagy vonalakban tekinti értesítés</span><span class="sxs-lookup"><span data-stu-id="16b8f-136">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="16b8f-137">ACCESS_COARSE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="16b8f-137">ACCESS_COARSE_LOCATION</span></span> |<span data-ttu-id="16b8f-138">Választható.</span><span class="sxs-lookup"><span data-stu-id="16b8f-138">Optional.</span></span> <span data-ttu-id="16b8f-139">Lehetővé teszi a valós idejű hely jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="16b8f-139">Enables Real-time location reporting</span></span> |
| <span data-ttu-id="16b8f-140">ACCESS_FINE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="16b8f-140">ACCESS_FINE_LOCATION</span></span> |<span data-ttu-id="16b8f-141">Választható.</span><span class="sxs-lookup"><span data-stu-id="16b8f-141">Optional.</span></span> <span data-ttu-id="16b8f-142">Lehetővé teszi, hogy helyét GPS-alapú jelentés</span><span class="sxs-lookup"><span data-stu-id="16b8f-142">Enables GPS-based location reporting</span></span> |

<span data-ttu-id="16b8f-143">Kezdődően az Android M [futás közben bizonyos engedélyek felügyelt](mobile-engagement-android-location-reporting.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="16b8f-143">Starting with Android M, [some permissions are managed at run time](mobile-engagement-android-location-reporting.md#android-m-permissions).</span></span>

<span data-ttu-id="16b8f-144">Ha már használja a ``ACCESS_FINE_LOCATION``, akkor nem szükséges tooalso használja ``ACCESS_COARSE_LOCATION``.</span><span class="sxs-lookup"><span data-stu-id="16b8f-144">If you are already using ``ACCESS_FINE_LOCATION``, then you don't need tooalso use ``ACCESS_COARSE_LOCATION``.</span></span>

## <a name="android-manifest-configuration-options"></a><span data-ttu-id="16b8f-145">Android konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="16b8f-145">Android Manifest configuration options</span></span>
### <a name="crash-report"></a><span data-ttu-id="16b8f-146">Összeomlási jelentést</span><span class="sxs-lookup"><span data-stu-id="16b8f-146">Crash report</span></span>
<span data-ttu-id="16b8f-147">toodisable összeomlási jelentések, adja hozzá ezt a kódot közötti hello `<application>` és `</application>` címkék:</span><span class="sxs-lookup"><span data-stu-id="16b8f-147">toodisable crash reports, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="16b8f-148">Küszöbérték-kapacitásnövelés</span><span class="sxs-lookup"><span data-stu-id="16b8f-148">Burst threshold</span></span>
<span data-ttu-id="16b8f-149">Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="16b8f-149">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="16b8f-150">A jelentés alkalmazásnaplók gyakran változnak, esetén jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (más néven "nagy sebességű átvitel").</span><span class="sxs-lookup"><span data-stu-id="16b8f-150">If your application report logs vary frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (called "burst mode").</span></span> <span data-ttu-id="16b8f-151">toodo Igen, adja hozzá ezt a kódot hello közötti `<application>` és `</application>` címkék:</span><span class="sxs-lookup"><span data-stu-id="16b8f-151">toodo so, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="16b8f-152">Kapacitásnövelés mód némileg növeli a hello eszközakkumulátor élettartamának, azonban hatással van hello Engagement figyelő: minden munkamenetek és feladatok időtartama vannak kerekítve toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="16b8f-152">Burst mode slightly increases hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration are rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="16b8f-153">A kapacitásnövelés küszöbértéknek legfeljebb 30000 (30s) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="16b8f-153">Your burst threshold should be no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="16b8f-154">Munkamenet időkorlátja</span><span class="sxs-lookup"><span data-stu-id="16b8f-154">Session timeout</span></span>
 <span data-ttu-id="16b8f-155">Egy tevékenység által lenyomásával hello fejezheti **Home** vagy **vissza** kulcs beállítása hello üresjárati telefonos vagy egy másik alkalmazásban átugorja.</span><span class="sxs-lookup"><span data-stu-id="16b8f-155">You can end an activity by pressing hello **Home** or **Back** key, by setting hello phone idle or by jumping into another application.</span></span> <span data-ttu-id="16b8f-156">Alapértelmezés szerint a munkamenet véget ér 10 másodperc után utolsó tevékenysége hello végét.</span><span class="sxs-lookup"><span data-stu-id="16b8f-156">By default, a session is ended ten seconds after hello end of its last activity.</span></span> <span data-ttu-id="16b8f-157">Ezzel elkerülhető, hogy a munkamenet valószínűségét minden alkalommal hello felhasználó kilép, és adja vissza toohello alkalmazás gyorsan, amely is hiba akkor fordulhat elő, amikor a felhasználó hello szerzi be a képen egy értesítés, stb. Érdemes lehet toomodify ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="16b8f-157">This avoids a session split each time hello user exits and returns toohello application quickly, which can happen when hello user picks up an image, checks a notification, etc. You may want toomodify this parameter.</span></span> <span data-ttu-id="16b8f-158">toodo Igen, adja hozzá ezt a kódot hello közötti `<application>` és `</application>` címkék:</span><span class="sxs-lookup"><span data-stu-id="16b8f-158">toodo so, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="16b8f-159">Naplózási jelentések letiltásához</span><span class="sxs-lookup"><span data-stu-id="16b8f-159">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="16b8f-160">Metódushívások használatával</span><span class="sxs-lookup"><span data-stu-id="16b8f-160">Using a method call</span></span>
<span data-ttu-id="16b8f-161">Ha azt szeretné, hogy a naplók küldése Engagement toostop, hívása:</span><span class="sxs-lookup"><span data-stu-id="16b8f-161">If you want Engagement toostop sending logs, you can call:</span></span>

    EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="16b8f-162">Ez a hívás az állandó: egy megosztott beállításokat tartalmazó fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="16b8f-162">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="16b8f-163">Ha Engagement aktív Ez a függvény hívásakor, hello szolgáltatás toostop egy percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="16b8f-163">If Engagement is active when you call this function, it may take one minute for hello service toostop.</span></span> <span data-ttu-id="16b8f-164">Azonban ez nem indítsa el a hello szolgáltatást minden hello hello alkalmazás következő indításakor.</span><span class="sxs-lookup"><span data-stu-id="16b8f-164">However it won't launch hello service at all hello next time you launch hello application.</span></span>

<span data-ttu-id="16b8f-165">Engedélyezheti újra reporting működik az azonos hello meghívásával napló `true`.</span><span class="sxs-lookup"><span data-stu-id="16b8f-165">You can enable log reporting again by calling hello same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="16b8f-166">Integráció saját`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="16b8f-166">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="16b8f-167">Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="16b8f-167">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="16b8f-168">A beállítások fájlt (hello kívánt mód) Engagement toouse konfigurálhatja a hello `AndroidManifest.xml` fájlt `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="16b8f-168">You can configure Engagement toouse your preferences file (with hello desired mode) in hello `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="16b8f-169">Hello `engagement:agent:settings:name` kulcsa használt toodefine hello hello közös beállításokat fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="16b8f-169">hello `engagement:agent:settings:name` key is used toodefine hello name of hello shared preferences file.</span></span>
* <span data-ttu-id="16b8f-170">Hello `engagement:agent:settings:mode` kulcs hello közös beállításokat fájl használt toodefine hello módban.</span><span class="sxs-lookup"><span data-stu-id="16b8f-170">hello `engagement:agent:settings:mode` key is used toodefine hello mode of hello shared preferences file.</span></span> <span data-ttu-id="16b8f-171">Használjon hello azonos és a mód a `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="16b8f-171">Use hello same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="16b8f-172">hello módban kell átadni egy számot: használatakor a konstans jelzők kombinációja a kódban, ellenőrizze a hello teljes értékét.</span><span class="sxs-lookup"><span data-stu-id="16b8f-172">hello mode must be passed as a number: if you are using a combination of constant flags in your code, check hello total value.</span></span>

<span data-ttu-id="16b8f-173">Bevonási mindig használja hello `engagement:key` belül hello beállításokat tartalmazó fájlt ezzel a beállítással kezelésére szolgáló logikai kulcs.</span><span class="sxs-lookup"><span data-stu-id="16b8f-173">Engagement always uses hello `engagement:key` boolean key within hello preferences file for managing this setting.</span></span>

<span data-ttu-id="16b8f-174">Példa következő hello `AndroidManifest.xml` jeleníti meg az alapértelmezett értékek hello:</span><span class="sxs-lookup"><span data-stu-id="16b8f-174">hello following example of `AndroidManifest.xml` shows hello default values:</span></span>

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

<span data-ttu-id="16b8f-175">Ezután hozzáadhatja a `CheckBoxPreference` a preferencia elrendezésben például hello egyet a következő:</span><span class="sxs-lookup"><span data-stu-id="16b8f-175">Then you can add a `CheckBoxPreference` in your preference layout like hello following one:</span></span>

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
