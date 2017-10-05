---
title: "Az Azure Mobile Engagement Android SDK speciális konfiguráció"
description: "Ismerteti, beleértve az Azure Mobile Engagement Android SDK-val Android Manifest speciális konfigurációs beállítás"
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
ms.openlocfilehash: 0301f71c76872714aa1bf727a6c21dd7a63db036
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="def22-103">Az Azure Mobile Engagement Android SDK speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="def22-103">Advanced configuration for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="def22-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="def22-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="def22-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="def22-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="def22-106">iOS</span><span class="sxs-lookup"><span data-stu-id="def22-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="def22-107">Android</span><span class="sxs-lookup"><span data-stu-id="def22-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
>
>

<span data-ttu-id="def22-108">Ez az eljárás ismerteti az Azure Mobile Engagement Android-alkalmazások beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="def22-108">This procedure describes how to configure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="def22-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="def22-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a><span data-ttu-id="def22-110">Engedélykövetelmények</span><span class="sxs-lookup"><span data-stu-id="def22-110">Permission Requirements</span></span>
<span data-ttu-id="def22-111">Néhány beállításhoz konkrét engedélyeket, amelyek az itt felsorolt hivatkozást, és beágyazott adott funkciójában szükségesek.</span><span class="sxs-lookup"><span data-stu-id="def22-111">Some options require specific permissions, all of which are listed here for reference, and in-line in the specific feature.</span></span> <span data-ttu-id="def22-112">Ezek az engedélyek hozzáadása a projekt AndroidManifest.xml azonnal előtt vagy után a `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="def22-112">Add these permissions to the AndroidManifest.xml of your project immediately before or after the `<application>` tag.</span></span>

<span data-ttu-id="def22-113">Az engedély kódot kell tűnik a következőt, ahol ki a megfelelő engedélyekkel az alábbi táblázat a.</span><span class="sxs-lookup"><span data-stu-id="def22-113">The permission code needs to look like the following, where you fill in the appropriate permission from the table that follows.</span></span>

    <uses-permission android:name="android.permission.[specific permission]"/>


| <span data-ttu-id="def22-114">Engedély</span><span class="sxs-lookup"><span data-stu-id="def22-114">Permission</span></span> | <span data-ttu-id="def22-115">Használata esetén</span><span class="sxs-lookup"><span data-stu-id="def22-115">When used</span></span> |
| --- | --- |
| <span data-ttu-id="def22-116">INTERNET</span><span class="sxs-lookup"><span data-stu-id="def22-116">INTERNET</span></span> |<span data-ttu-id="def22-117">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="def22-117">Required.</span></span> <span data-ttu-id="def22-118">Alapszintű jelentéskészítéshez</span><span class="sxs-lookup"><span data-stu-id="def22-118">For basic reporting</span></span> |
| <span data-ttu-id="def22-119">ACCESS_NETWORK_STATE</span><span class="sxs-lookup"><span data-stu-id="def22-119">ACCESS_NETWORK_STATE</span></span> |<span data-ttu-id="def22-120">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="def22-120">Required.</span></span> <span data-ttu-id="def22-121">Alapszintű jelentéskészítéshez</span><span class="sxs-lookup"><span data-stu-id="def22-121">For basic reporting</span></span> |
| <span data-ttu-id="def22-122">RECEIVE_BOOT_COMPLETED</span><span class="sxs-lookup"><span data-stu-id="def22-122">RECEIVE_BOOT_COMPLETED</span></span> |<span data-ttu-id="def22-123">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="def22-123">Required.</span></span> <span data-ttu-id="def22-124">Az értesítések központ megjelennek az eszköz újraindítását követően.</span><span class="sxs-lookup"><span data-stu-id="def22-124">To show up the notifications center after device reboot</span></span> |
| <span data-ttu-id="def22-125">WAKE_LOCK</span><span class="sxs-lookup"><span data-stu-id="def22-125">WAKE_LOCK</span></span> |<span data-ttu-id="def22-126">Ajánlott.</span><span class="sxs-lookup"><span data-stu-id="def22-126">Recommended.</span></span> <span data-ttu-id="def22-127">Lehetővé teszi, hogy Wi-Fi használata esetén, vagy ha a képernyőn a rendszer nem végez adatgyűjtést</span><span class="sxs-lookup"><span data-stu-id="def22-127">Enables collecting data when using WiFi or when screen is off</span></span> |
| <span data-ttu-id="def22-128">MOZOG</span><span class="sxs-lookup"><span data-stu-id="def22-128">VIBRATE</span></span> |<span data-ttu-id="def22-129">Választható.</span><span class="sxs-lookup"><span data-stu-id="def22-129">Optional.</span></span> <span data-ttu-id="def22-130">Lehetővé teszi, hogy vibráció értesítések fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="def22-130">Enables vibration when notifications are received</span></span> |
| <span data-ttu-id="def22-131">DOWNLOAD_WITHOUT_NOTIFICATION</span><span class="sxs-lookup"><span data-stu-id="def22-131">DOWNLOAD_WITHOUT_NOTIFICATION</span></span> |<span data-ttu-id="def22-132">Választható.</span><span class="sxs-lookup"><span data-stu-id="def22-132">Optional.</span></span> <span data-ttu-id="def22-133">Lehetővé teszi, hogy Android nagy vonalakban tekinti értesítés</span><span class="sxs-lookup"><span data-stu-id="def22-133">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="def22-134">WRITE_EXTERNAL_STORAGE</span><span class="sxs-lookup"><span data-stu-id="def22-134">WRITE_EXTERNAL_STORAGE</span></span> |<span data-ttu-id="def22-135">Választható.</span><span class="sxs-lookup"><span data-stu-id="def22-135">Optional.</span></span> <span data-ttu-id="def22-136">Lehetővé teszi, hogy Android nagy vonalakban tekinti értesítés</span><span class="sxs-lookup"><span data-stu-id="def22-136">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="def22-137">ACCESS_COARSE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="def22-137">ACCESS_COARSE_LOCATION</span></span> |<span data-ttu-id="def22-138">Választható.</span><span class="sxs-lookup"><span data-stu-id="def22-138">Optional.</span></span> <span data-ttu-id="def22-139">Lehetővé teszi a valós idejű hely jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="def22-139">Enables Real-time location reporting</span></span> |
| <span data-ttu-id="def22-140">ACCESS_FINE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="def22-140">ACCESS_FINE_LOCATION</span></span> |<span data-ttu-id="def22-141">Választható.</span><span class="sxs-lookup"><span data-stu-id="def22-141">Optional.</span></span> <span data-ttu-id="def22-142">Lehetővé teszi, hogy helyét GPS-alapú jelentés</span><span class="sxs-lookup"><span data-stu-id="def22-142">Enables GPS-based location reporting</span></span> |

<span data-ttu-id="def22-143">Kezdődően az Android M [futás közben bizonyos engedélyek felügyelt](mobile-engagement-android-location-reporting.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="def22-143">Starting with Android M, [some permissions are managed at run time](mobile-engagement-android-location-reporting.md#android-m-permissions).</span></span>

<span data-ttu-id="def22-144">Ha már használja a ``ACCESS_FINE_LOCATION``, akkor nem kell is ``ACCESS_COARSE_LOCATION``.</span><span class="sxs-lookup"><span data-stu-id="def22-144">If you are already using ``ACCESS_FINE_LOCATION``, then you don't need to also use ``ACCESS_COARSE_LOCATION``.</span></span>

## <a name="android-manifest-configuration-options"></a><span data-ttu-id="def22-145">Android konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="def22-145">Android Manifest configuration options</span></span>
### <a name="crash-report"></a><span data-ttu-id="def22-146">Összeomlási jelentést</span><span class="sxs-lookup"><span data-stu-id="def22-146">Crash report</span></span>
<span data-ttu-id="def22-147">Összeomlási jelentések letiltásához adja hozzá ezt a kódot között a `<application>` és `</application>` címkék:</span><span class="sxs-lookup"><span data-stu-id="def22-147">To disable crash reports, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="def22-148">Küszöbérték-kapacitásnövelés</span><span class="sxs-lookup"><span data-stu-id="def22-148">Burst threshold</span></span>
<span data-ttu-id="def22-149">Alapértelmezés szerint a bevonási service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="def22-149">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="def22-150">Ha a jelentés alkalmazásnaplók változnak gyakran, érdemes meghajtóin a naplókat, és hogy jelentse őket egyszerre egy alap rendszeres időközönként a (más néven "nagy sebességű átvitel").</span><span class="sxs-lookup"><span data-stu-id="def22-150">If your application report logs vary frequently, it is better to buffer the logs and to report them all at once on a regular time base (called "burst mode").</span></span> <span data-ttu-id="def22-151">Ehhez adja hozzá ezt a kódot között a `<application>` és `</application>` címkék:</span><span class="sxs-lookup"><span data-stu-id="def22-151">To do so, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="def22-152">Kapacitásnövelés mód némileg növeli az eszközakkumulátor élettartamának, de hatással van a bevonási figyelő: minden munkamenetek és feladatok időtartama a kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a kapacitásnövelés küszöbértéket nem láthatják) vannak kerekítve.</span><span class="sxs-lookup"><span data-stu-id="def22-152">Burst mode slightly increases the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration are rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="def22-153">A kapacitásnövelés küszöbértéknek legfeljebb 30000 (30s) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="def22-153">Your burst threshold should be no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="def22-154">Munkamenet időkorlátja</span><span class="sxs-lookup"><span data-stu-id="def22-154">Session timeout</span></span>
 <span data-ttu-id="def22-155">Egy tevékenység fejezheti billentyűkombináció lenyomásával a **Home** vagy **vissza** kulcs, vagy egy másik alkalmazásban átugorja úgy, hogy a telefon üresjárati.</span><span class="sxs-lookup"><span data-stu-id="def22-155">You can end an activity by pressing the **Home** or **Back** key, by setting the phone idle or by jumping into another application.</span></span> <span data-ttu-id="def22-156">Alapértelmezés szerint a munkamenet véget ér 10 másodperc után utolsó tevékenysége végén.</span><span class="sxs-lookup"><span data-stu-id="def22-156">By default, a session is ended ten seconds after the end of its last activity.</span></span> <span data-ttu-id="def22-157">Ezzel elkerülhető a munkamenet valószínűségét minden alkalommal, amikor a felhasználó kilép, és visszaadja az alkalmazásnak gyorsan, amely képes hiba akkor fordulhat elő, amikor a felhasználó szerzi be a képen egy értesítés, stb. Érdemes lehet módosíthatják ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="def22-157">This avoids a session split each time the user exits and returns to the application quickly, which can happen when the user picks up an image, checks a notification, etc. You may want to modify this parameter.</span></span> <span data-ttu-id="def22-158">Ehhez adja hozzá ezt a kódot között a `<application>` és `</application>` címkék:</span><span class="sxs-lookup"><span data-stu-id="def22-158">To do so, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="def22-159">Naplózási jelentések letiltásához</span><span class="sxs-lookup"><span data-stu-id="def22-159">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="def22-160">Metódushívások használatával</span><span class="sxs-lookup"><span data-stu-id="def22-160">Using a method call</span></span>
<span data-ttu-id="def22-161">Ha azt szeretné, hogy leállítja a naplók küldése Engagement, hívása:</span><span class="sxs-lookup"><span data-stu-id="def22-161">If you want Engagement to stop sending logs, you can call:</span></span>

    EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="def22-162">Ez a hívás az állandó: egy megosztott beállításokat tartalmazó fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="def22-162">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="def22-163">Ha Engagement aktív Ez a függvény hívásakor, a szolgáltatás leállítása egy percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="def22-163">If Engagement is active when you call this function, it may take one minute for the service to stop.</span></span> <span data-ttu-id="def22-164">Azonban ez nem indítsa el a szolgáltatás minden legközelebb indítani az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="def22-164">However it won't launch the service at all the next time you launch the application.</span></span>

<span data-ttu-id="def22-165">Engedélyezheti a reporting újra működik meghívásával napló `true`.</span><span class="sxs-lookup"><span data-stu-id="def22-165">You can enable log reporting again by calling the same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="def22-166">Integráció saját`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="def22-166">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="def22-167">Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="def22-167">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="def22-168">Konfigurálhatja a bevonási a beállításokat tartalmazó fájlt (a kívánt mód) a használatára a `AndroidManifest.xml` fájlt `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="def22-168">You can configure Engagement to use your preferences file (with the desired mode) in the `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="def22-169">A `engagement:agent:settings:name` kulcs használatával a közös beállításokat fájl nevének meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="def22-169">The `engagement:agent:settings:name` key is used to define the name of the shared preferences file.</span></span>
* <span data-ttu-id="def22-170">A `engagement:agent:settings:mode` kulcs használatával a közös beállításokat fájl módjának meghatározása.</span><span class="sxs-lookup"><span data-stu-id="def22-170">The `engagement:agent:settings:mode` key is used to define the mode of the shared preferences file.</span></span> <span data-ttu-id="def22-171">Az azonos módot, és a használja a `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="def22-171">Use the same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="def22-172">Egy számot a módot kell átadni: használatakor a konstans jelzők kombinációja a kódban, ellenőrizze a teljes értékét.</span><span class="sxs-lookup"><span data-stu-id="def22-172">The mode must be passed as a number: if you are using a combination of constant flags in your code, check the total value.</span></span>

<span data-ttu-id="def22-173">Bevonási mindig használja a `engagement:key` logikai kulcs ezzel a beállítással kezelésére szolgáló beállítások fájlon belül.</span><span class="sxs-lookup"><span data-stu-id="def22-173">Engagement always uses the `engagement:key` boolean key within the preferences file for managing this setting.</span></span>

<span data-ttu-id="def22-174">A következő példa `AndroidManifest.xml` megjeleníti az alapértelmezett értékeket:</span><span class="sxs-lookup"><span data-stu-id="def22-174">The following example of `AndroidManifest.xml` shows the default values:</span></span>

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

<span data-ttu-id="def22-175">Ezután hozzáadhatja a `CheckBoxPreference` a preferencia elrendezésben hasonló a következő:</span><span class="sxs-lookup"><span data-stu-id="def22-175">Then you can add a `CheckBoxPreference` in your preference layout like the following one:</span></span>

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
