---
title: "aaaAzure Mobile Engagement hibaelhárítási útmutatója - SDK"
description: "Az Azure Mobile Engagement SDK-integráció problémák elhárítása"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: de265cf1-2f88-43ef-8616-156ada5be7b6
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1c082b81d898f4bdb47b8efe6cfbacfd83fe9279
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a><span data-ttu-id="1aab2-103">Az SDK-integráció problémák hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="1aab2-103">Troubleshooting guide for SDK integration issues</span></span>
<span data-ttu-id="1aab2-104">Az alábbiakban hello lehetséges problémák merülhetnek fel az, hogy hogyan integrálja az Azure Mobile Engagement az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1aab2-104">hello following are possible issues you may encounter with how Azure Mobile Engagement integrates into your application.</span></span>

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a><span data-ttu-id="1aab2-105">Az alkalmazás egy másik olyan területen meghibásodása felfedezett SDK</span><span class="sxs-lookup"><span data-stu-id="1aab2-105">SDK issues discovered by a failure in another area of your application</span></span>
### <a name="issue"></a><span data-ttu-id="1aab2-106">Probléma</span><span class="sxs-lookup"><span data-stu-id="1aab2-106">Issue</span></span>
* <span data-ttu-id="1aab2-107">Felhasználói felületi adatok gyűjtése sikertelen (elemzés, figyelés, Szegmentálás vagy irányítópultok).</span><span class="sxs-lookup"><span data-stu-id="1aab2-107">UI data collection failure (in Analytics, Monitoring, Segmentation, or Dashboards).</span></span>
* <span data-ttu-id="1aab2-108">Hibák Push (leküldéses értesítések nem működnek a app alkalmazást, vagy mindkettőt).</span><span class="sxs-lookup"><span data-stu-id="1aab2-108">Push Failures (Pushes don't work in app, out of app, or both).</span></span>
* <span data-ttu-id="1aab2-109">Speciális funkció hibák (követési, földrajzi hely meghatározásának vagy adott leküldéses értesítések nem működnek platform).</span><span class="sxs-lookup"><span data-stu-id="1aab2-109">Advanced Feature Failures (Tracking, Geolocation, or platform specific Pushes don’t work).</span></span>
* <span data-ttu-id="1aab2-110">Sikertelen API (API-k sikertelen gyakran hibaüzenet nélkül csendes).</span><span class="sxs-lookup"><span data-stu-id="1aab2-110">API Failures (APIs fail often silently without error messages).</span></span>
* <span data-ttu-id="1aab2-111">A Szolgáltatáshibák (Azure Mobile Engagement egyike sem működik, az alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="1aab2-111">Service Failures (none of Azure Mobile Engagement works for your application).</span></span>

### <a name="causes"></a><span data-ttu-id="1aab2-112">Okok</span><span class="sxs-lookup"><span data-stu-id="1aab2-112">Causes</span></span>
* <span data-ttu-id="1aab2-113">A legtöbb során felmerülő kérdések hello Azure Mobile Engagement SDK feloldva toobe úgy fog történni az alkalmazásban (például a felhasználói felület gyűjtemény hiba, leküldéses hiba, speciális szolgáltatás hibája, Alkalmazásprogramozási felületében, alkalmazás-összeomlásokat, vagy nyilvánvaló szolgáltatás hibája kimaradásáról).</span><span class="sxs-lookup"><span data-stu-id="1aab2-113">Most issues that need toobe resolved with hello Azure Mobile Engagement SDK will be discovered by a failure in your application (such as a UI data collection failure, push failure, advanced feature failure, API failure, Application crashes, or apparent service outage).</span></span>  
* <span data-ttu-id="1aab2-114">Ha egy adott szolgáltatással az Azure Mobile Engagement az alkalmazás előtt soha nem működött, szüksége lesz a toocomplete hello integráció.</span><span class="sxs-lookup"><span data-stu-id="1aab2-114">If a particular feature of Azure Mobile Engagement has never worked in your app before, you will need toocomplete hello integration.</span></span> 
* <span data-ttu-id="1aab2-115">Ha az Azure Mobile Engagement egy adott szolgáltatással dolgozott, illetve leállt, szükség lehet a tooupgrade toohello legfrissebb verziója hello Azure Mobile Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="1aab2-115">If a particular feature of Azure Mobile Engagement was working and stopped, you may need tooupgrade toohello last version with hello Azure Mobile Engagement SDK.</span></span> <span data-ttu-id="1aab2-116">Ne feledje, hogy nincs-e az Azure Mobile Engagement SDK hello Azure Mobile Engagement (Android, iOS, Windows és Windows Phone) által támogatott platformokon egy másik verziója.</span><span class="sxs-lookup"><span data-stu-id="1aab2-116">Remember that there is a different version of hello Azure Mobile Engagement SDK for each platform supported by Azure Mobile Engagement (Android, iOS, Windows, and Windows Phone).</span></span>

#### <a name="sdk-integration"></a><span data-ttu-id="1aab2-117">SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="1aab2-117">SDK Integration</span></span>
* <span data-ttu-id="1aab2-118">Az Azure Mobile Engagement SDK-ban (elemzés) nincs megfelelően integrálva.</span><span class="sxs-lookup"><span data-stu-id="1aab2-118">Azure Mobile Engagement not correctly integrated in SDK (Analytics).</span></span>
* <span data-ttu-id="1aab2-119">Reach SDK (az alkalmazás és kívüli alkalmazás leküldéses értesítések) nincs megfelelően integrálva.</span><span class="sxs-lookup"><span data-stu-id="1aab2-119">Reach not correctly integrated in SDK (In App and Out of App Pushes).</span></span>
* <span data-ttu-id="1aab2-120">A tanúsítvány lejárt vagy nem megfelelő termék vs. Fejlesztői (csak iOS esetén).</span><span class="sxs-lookup"><span data-stu-id="1aab2-120">Certificate expired or incorrect PROD vs. DEV (iOS only).</span></span>
* <span data-ttu-id="1aab2-121">GCM vagy ADM nem megfelelően az SDK integrált (Android csak - szolgáltatás adott leküldéses értesítések).</span><span class="sxs-lookup"><span data-stu-id="1aab2-121">GCM or ADM not correctly integrated in SDK (Android only - Service Specific Pushes).</span></span>
* <span data-ttu-id="1aab2-122">Nyomon követése nem megfelelően integrált SDK (telepítés tároló nyomon követése).</span><span class="sxs-lookup"><span data-stu-id="1aab2-122">Tracking not correctly integrated in SDK (Install store tracking).</span></span>
* <span data-ttu-id="1aab2-123">Lusta helyét vagy GPS helyét nem megfelelően integrált SDK (célcsoportkezelést által földrajzi helyhez).</span><span class="sxs-lookup"><span data-stu-id="1aab2-123">Lazy Location or GPS Location not correctly integrated in SDK (Targeting by geo-location).</span></span>

<span data-ttu-id="1aab2-124">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="1aab2-124">**See also:**</span></span>

* <span data-ttu-id="1aab2-125">[SDK-dokumentáció - integráció útmutatók][Link 5]</span><span class="sxs-lookup"><span data-stu-id="1aab2-125">[SDK Documentation - Integration Guides][Link 5]</span></span> 
* <span data-ttu-id="1aab2-126">[Hibaelhárítási útmutató - leküldéses][Link 23]</span><span class="sxs-lookup"><span data-stu-id="1aab2-126">[Troubleshooting Guide - Push][Link 23]</span></span>

#### <a name="sdk-upgrade"></a><span data-ttu-id="1aab2-127">SDK frissítése</span><span class="sxs-lookup"><span data-stu-id="1aab2-127">SDK Upgrade</span></span>
* <span data-ttu-id="1aab2-128">Kell tooupgrade SDK tooresolve problémái hello SDK (hello az eszköz operációs rendszere verziója gyakran kapcsolódó toonewer) régebbi verzióit.</span><span class="sxs-lookup"><span data-stu-id="1aab2-128">Need tooupgrade SDK tooresolve issues with older versions of hello SDK (often related toonewer versions of hello device OS).</span></span>
* <span data-ttu-id="1aab2-129">Az alkalmazás összes korábbi verziójának eltávolítása az eszközről, és telepítse újra az alkalmazás legújabb verzióját hello, hello regisztrálja újra az Eszközazonosítót az hello Azure Mobile Engagement felhasználói felület tooconfirm, hogy az eszköz által használt hello az alkalmazás legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="1aab2-129">Uninstall all previous versions of your app from your device and reinstall hello newest version of your app, hello re-register your Device ID from hello Azure Mobile Engagement UI tooconfirm that your device is using hello newest version of your app.</span></span>

<span data-ttu-id="1aab2-130">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="1aab2-130">**See also:**</span></span>

* [<span data-ttu-id="1aab2-131">SDK-dokumentáció – kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1aab2-131">SDK Documentation - Release Notes</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [<span data-ttu-id="1aab2-132">SDK-dokumentáció - frissítési útmutatók</span><span class="sxs-lookup"><span data-stu-id="1aab2-132">SDK Documentation - Upgrade Guides</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a><span data-ttu-id="1aab2-133">Egyéb SDK</span><span class="sxs-lookup"><span data-stu-id="1aab2-133">SDK Other</span></span>
* <span data-ttu-id="1aab2-134">Az Application Manifest "AndroidManifest.xml" hibákat okozhat az Azure Mobile Engagement toowork (csak Android esetén).</span><span class="sxs-lookup"><span data-stu-id="1aab2-134">Errors in Application Manifest "AndroidManifest.xml" can cause Azure Mobile Engagement not toowork (Android only).</span></span>
* <span data-ttu-id="1aab2-135">Általános hiba az SDK-integráció és API-használati tooconfuse hello SDK kulcs, és az API-kulcs hello.</span><span class="sxs-lookup"><span data-stu-id="1aab2-135">A common issue with SDK integration and API usage is tooconfuse hello SDK Key and hello API Key.</span></span>

<span data-ttu-id="1aab2-136">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="1aab2-136">**See also:**</span></span>

* <span data-ttu-id="1aab2-137">[Alapfogalmak - szószedet][Link 6]</span><span class="sxs-lookup"><span data-stu-id="1aab2-137">[Concepts - Glossary][Link 6]</span></span>

## <a name="advanced-coding-issues"></a><span data-ttu-id="1aab2-138">Speciális problémák kódolása</span><span class="sxs-lookup"><span data-stu-id="1aab2-138">Advanced coding issues</span></span>
### <a name="issue"></a><span data-ttu-id="1aab2-139">Probléma</span><span class="sxs-lookup"><span data-stu-id="1aab2-139">Issue</span></span>
* <span data-ttu-id="1aab2-140">Adott platformkódot nem közvetlenül kapcsolódó tooAzure a Mobile Engagement az iOS, Android és Windows Phone problémákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="1aab2-140">Platform specific code not directly related tooAzure Mobile Engagement can cause issues on iOS, Android, and Windows Phone.</span></span>

### <a name="causes"></a><span data-ttu-id="1aab2-141">Okok</span><span class="sxs-lookup"><span data-stu-id="1aab2-141">Causes</span></span>
* <span data-ttu-id="1aab2-142">Számos speciális az Azure Mobile Engagement kódolási problémák miatt nem megfelelően megírt platform által megadott kódja nem közvetlenül kapcsolódó tooAzure a Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="1aab2-142">Many advanced coding issues with Azure Mobile Engagement are caused by improperly written platform specific code not directly related tooAzure Mobile Engagement.</span></span> <span data-ttu-id="1aab2-143">Szüksége lesz tooconsult dokumentáció adott toohello platform fejleszt a továbbá tooAzure a Mobile Engagement dokumentáció (Android, iOS, webalkalmazás, Windows és Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="1aab2-143">You will need tooconsult documentation specific toohello platform you are developing for in addition tooAzure Mobile Engagement documentation (Android, iOS, Web, Windows, and Windows Phone).</span></span>
* <span data-ttu-id="1aab2-144">Nem megfelelően állítja be "kategóriák", megakadályozza, hogy a linking helyről értesítési tooanother belül vagy kívül hello alkalmazás (csak Android esetén).</span><span class="sxs-lookup"><span data-stu-id="1aab2-144">Not correctly configuring "categories", prevents linking from a notification tooanother location either inside or outside of hello app (Android only).</span></span> 
* <span data-ttu-id="1aab2-145">Nincs beállítva, az "UIKit.framework" túl "választható" a kódban iOS "Szimbólum nem található a hiba" mutat be és/vagy a régebbi iOS-eszközök (csak iOS) összeomlik.</span><span class="sxs-lookup"><span data-stu-id="1aab2-145">Not setting "UIKit.framework" too"optional" in your iOS code, shows a "Symbol not found error" and/or crashes on older iOS devices (iOS only).</span></span>
* <span data-ttu-id="1aab2-146">Lejárt tanúsítványok, vagy fejlesztői hello vagy hello cert termék verziója nem megfelelően használja, okok leküldéses kapcsolatos problémákat (csak iOS esetén).</span><span class="sxs-lookup"><span data-stu-id="1aab2-146">Expired certificates or not correctly using hello DEV or Prod version of hello cert, causes push issues (iOS only).</span></span>
* <span data-ttu-id="1aab2-147">Nincsenek bizonyos korlátozások rejlő tooa platform, amely az Azure Mobile Engagement vezérlésére nem képes (ilyen például a system center hello működéséről az alkalmazásból leküldéses értesítések Android és iOS).</span><span class="sxs-lookup"><span data-stu-id="1aab2-147">There are some limitations inherent tooa platform that Azure Mobile Engagement can't control (like how hello system center works for out of app pushes in Android and iOS).</span></span>
* <span data-ttu-id="1aab2-148">Az Azure Mobile Engagement közzéteszi hello belső csomagok által használt Azure Mobile Engagement az iOS és Android hivatkozás teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="1aab2-148">Azure Mobile Engagement publishes a full list of hello internal packages used by Azure Mobile Engagement for iOS and Android for reference.</span></span> <span data-ttu-id="1aab2-149">Ne feledje, hogy az Azure Mobile Engagement néhány szolgáltatása adott toohello platform (Android, iOS, webalkalmazás, Windows és Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="1aab2-149">Keep in mind that some features of Azure Mobile Engagement are specific toohello platform (Android, iOS, Web, Windows, and Windows Phone).</span></span>

### <a name="see-also"></a><span data-ttu-id="1aab2-150">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1aab2-150">See also</span></span>
* <span data-ttu-id="1aab2-151">[Hibaelhárítási útmutató - leküldéses][Link 23]</span><span class="sxs-lookup"><span data-stu-id="1aab2-151">[Troubleshooting Guide - Push][Link 23]</span></span> 
* <span data-ttu-id="1aab2-152">[SDK-dokumentáció – kibocsátási megjegyzések][Link 5]</span><span class="sxs-lookup"><span data-stu-id="1aab2-152">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="1aab2-153">[SDK-dokumentáció - frissítési útmutatók][Link 5]</span><span class="sxs-lookup"><span data-stu-id="1aab2-153">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="application-crashes"></a><span data-ttu-id="1aab2-154">Alkalmazás-összeomlásokat</span><span class="sxs-lookup"><span data-stu-id="1aab2-154">Application crashes</span></span>
### <a name="issue"></a><span data-ttu-id="1aab2-155">Probléma</span><span class="sxs-lookup"><span data-stu-id="1aab2-155">Issue</span></span>
* <span data-ttu-id="1aab2-156">Az alkalmazás összeomlik hello végfelhasználói eszközön.</span><span class="sxs-lookup"><span data-stu-id="1aab2-156">Your application crashes on hello end users' device.</span></span>

### <a name="causes"></a><span data-ttu-id="1aab2-157">Okok</span><span class="sxs-lookup"><span data-stu-id="1aab2-157">Causes</span></span>
* <span data-ttu-id="1aab2-158">Összeomlási adatok megtekinthetők a hello *Analytics felhasználói felület* vagy hello *Analytics API*</span><span class="sxs-lookup"><span data-stu-id="1aab2-158">Crash information can be viewed in hello *Analytics UI* or hello *Analytics API*</span></span>
* <span data-ttu-id="1aab2-159">A vizsgálati eszköz Eszközazonosító hello hajtanak végre bizonyos hello található, ami miatt a kérelem toocrash egy végfelhasználó toohelp az azonos művelet azonosítására a összeomlási hello okát.</span><span class="sxs-lookup"><span data-stu-id="1aab2-159">You can find hello Device ID of your test device and take hello same action that caused your application toocrash for an end user toohelp identify hello cause of your crash.</span></span>
* <span data-ttu-id="1aab2-160">Azure Mobile Engagement SDK hello ismert problémái következtében az alkalmazások toocrash néha megoldó toohello hello SDK legújabb verziójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="1aab2-160">Known issues with hello Azure Mobile Engagement SDK that cause applications toocrash are sometimes resolved by upgrading toohello latest version of hello SDK.</span></span> <span data-ttu-id="1aab2-161">Győződjön meg arról, hogy toocheck hello kibocsátási megjegyzések a platformra vonatkozó összeomlások vizsgálatakor.</span><span class="sxs-lookup"><span data-stu-id="1aab2-161">Make sure toocheck hello release notes about your platform when investigating crashes.</span></span>

### <a name="see-also"></a><span data-ttu-id="1aab2-162">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1aab2-162">See also</span></span>
* <span data-ttu-id="1aab2-163">[SDK-dokumentáció – kibocsátási megjegyzések][Link 5]</span><span class="sxs-lookup"><span data-stu-id="1aab2-163">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="1aab2-164">[SDK-dokumentáció - frissítési útmutatók][Link 5]</span><span class="sxs-lookup"><span data-stu-id="1aab2-164">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="app-store-upload-failures"></a><span data-ttu-id="1aab2-165">Alkalmazás-áruház sikertelen feltöltés</span><span class="sxs-lookup"><span data-stu-id="1aab2-165">App store upload failures</span></span>
### <a name="issue"></a><span data-ttu-id="1aab2-166">Probléma</span><span class="sxs-lookup"><span data-stu-id="1aab2-166">Issue</span></span>
* <span data-ttu-id="1aab2-167">Kapcsolatos hibák toouploading hello legújabb verzióját az alkalmazás tooApple, Google vagy hello Windows alkalmazás-áruházból.</span><span class="sxs-lookup"><span data-stu-id="1aab2-167">Errors related toouploading hello latest version of your app tooApple, Google, or hello Windows App store.</span></span>

### <a name="causes"></a><span data-ttu-id="1aab2-168">Okok</span><span class="sxs-lookup"><span data-stu-id="1aab2-168">Causes</span></span>
* <span data-ttu-id="1aab2-169">Alkalmazás tárolja néha letiltja az alkalmazások bizonyos engedélyezett szolgáltatásokkal (pl. hello Apple Store megakadályozza, hogy hello felhasználása idfv fogja elvégezni lehetővé hello áruházban található alkalmazásokra és hello GooglePlay tároló megakadályozza, hogy az alkalmazással kapcsolatos adatok alkalmazások közötti megosztásának hello).</span><span class="sxs-lookup"><span data-stu-id="1aab2-169">App stores sometimes block apps with certain features enabled (e.g. hello Apple Store prevents hello use of IDFV in apps in hello store and hello GooglePlay store prevents hello sharing of application information between apps).</span></span> 
* <span data-ttu-id="1aab2-170">Győződjön meg arról, hogy ellenőrizze hello kibocsátási megjegyzések a platform és az hello SDK jelenlegi verzióját, ha az alkalmazás-áruházra toohello feltöltése nehézséget.</span><span class="sxs-lookup"><span data-stu-id="1aab2-170">Make sure that you check hello release notes about your platform and current version of hello SDK if you have difficulty uploading an app toohello store.</span></span>

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

