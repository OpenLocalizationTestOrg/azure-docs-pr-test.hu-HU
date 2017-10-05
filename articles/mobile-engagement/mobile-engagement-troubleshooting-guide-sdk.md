---
title: "Az Azure Mobile Engagement hibaelhárítási útmutatója - SDK"
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
ms.openlocfilehash: 4d9d6165deb4bd0c65f1841aa7c457363a1f2865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a><span data-ttu-id="06556-103">Az SDK-integráció problémák hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="06556-103">Troubleshooting guide for SDK integration issues</span></span>
<span data-ttu-id="06556-104">A következőkben lehetséges problémák merülhetnek fel az, hogy hogyan integrálja az Azure Mobile Engagement az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="06556-104">The following are possible issues you may encounter with how Azure Mobile Engagement integrates into your application.</span></span>

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a><span data-ttu-id="06556-105">Az alkalmazás egy másik olyan területen meghibásodása felfedezett SDK</span><span class="sxs-lookup"><span data-stu-id="06556-105">SDK issues discovered by a failure in another area of your application</span></span>
### <a name="issue"></a><span data-ttu-id="06556-106">Probléma</span><span class="sxs-lookup"><span data-stu-id="06556-106">Issue</span></span>
* <span data-ttu-id="06556-107">Felhasználói felületi adatok gyűjtése sikertelen (elemzés, figyelés, Szegmentálás vagy irányítópultok).</span><span class="sxs-lookup"><span data-stu-id="06556-107">UI data collection failure (in Analytics, Monitoring, Segmentation, or Dashboards).</span></span>
* <span data-ttu-id="06556-108">Hibák Push (leküldéses értesítések nem működnek a app alkalmazást, vagy mindkettőt).</span><span class="sxs-lookup"><span data-stu-id="06556-108">Push Failures (Pushes don't work in app, out of app, or both).</span></span>
* <span data-ttu-id="06556-109">Speciális funkció hibák (követési, földrajzi hely meghatározásának vagy adott leküldéses értesítések nem működnek platform).</span><span class="sxs-lookup"><span data-stu-id="06556-109">Advanced Feature Failures (Tracking, Geolocation, or platform specific Pushes don’t work).</span></span>
* <span data-ttu-id="06556-110">Sikertelen API (API-k sikertelen gyakran hibaüzenet nélkül csendes).</span><span class="sxs-lookup"><span data-stu-id="06556-110">API Failures (APIs fail often silently without error messages).</span></span>
* <span data-ttu-id="06556-111">A Szolgáltatáshibák (Azure Mobile Engagement egyike sem működik, az alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="06556-111">Service Failures (none of Azure Mobile Engagement works for your application).</span></span>

### <a name="causes"></a><span data-ttu-id="06556-112">Okok</span><span class="sxs-lookup"><span data-stu-id="06556-112">Causes</span></span>
* <span data-ttu-id="06556-113">A legtöbb során felmerülő kérdések oldható fel az Azure Mobile Engagement SDK az alkalmazás (például a felhasználói felület gyűjtemény hiba, leküldéses hiba, speciális szolgáltatás hibája, Alkalmazásprogramozási felületében, alkalmazás-összeomlásokat, vagy nyilvánvaló szolgáltatáskimaradás) hibát fog eredményül adni.</span><span class="sxs-lookup"><span data-stu-id="06556-113">Most issues that need to be resolved with the Azure Mobile Engagement SDK will be discovered by a failure in your application (such as a UI data collection failure, push failure, advanced feature failure, API failure, Application crashes, or apparent service outage).</span></span>  
* <span data-ttu-id="06556-114">Ha egy adott szolgáltatással az Azure Mobile Engagement az alkalmazás előtt soha nem működött, szüksége lesz az integráció végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="06556-114">If a particular feature of Azure Mobile Engagement has never worked in your app before, you will need to complete the integration.</span></span> 
* <span data-ttu-id="06556-115">Ha az Azure Mobile Engagement egy adott szolgáltatással dolgozott, illetve leállt, szükség lehet az utolsó frissítés az Azure Mobile Engagement SDK verziójával.</span><span class="sxs-lookup"><span data-stu-id="06556-115">If a particular feature of Azure Mobile Engagement was working and stopped, you may need to upgrade to the last version with the Azure Mobile Engagement SDK.</span></span> <span data-ttu-id="06556-116">Ne feledje, hogy nincs-e az Azure Mobile Engagement SDK minden Azure Mobile Engagement (Android, iOS, Windows és Windows Phone) által támogatott platform egy másik verziója.</span><span class="sxs-lookup"><span data-stu-id="06556-116">Remember that there is a different version of the Azure Mobile Engagement SDK for each platform supported by Azure Mobile Engagement (Android, iOS, Windows, and Windows Phone).</span></span>

#### <a name="sdk-integration"></a><span data-ttu-id="06556-117">SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="06556-117">SDK Integration</span></span>
* <span data-ttu-id="06556-118">Az Azure Mobile Engagement SDK-ban (elemzés) nincs megfelelően integrálva.</span><span class="sxs-lookup"><span data-stu-id="06556-118">Azure Mobile Engagement not correctly integrated in SDK (Analytics).</span></span>
* <span data-ttu-id="06556-119">Reach SDK (az alkalmazás és kívüli alkalmazás leküldéses értesítések) nincs megfelelően integrálva.</span><span class="sxs-lookup"><span data-stu-id="06556-119">Reach not correctly integrated in SDK (In App and Out of App Pushes).</span></span>
* <span data-ttu-id="06556-120">A tanúsítvány lejárt vagy nem megfelelő termék vs. Fejlesztői (csak iOS esetén).</span><span class="sxs-lookup"><span data-stu-id="06556-120">Certificate expired or incorrect PROD vs. DEV (iOS only).</span></span>
* <span data-ttu-id="06556-121">GCM vagy ADM nem megfelelően az SDK integrált (Android csak - szolgáltatás adott leküldéses értesítések).</span><span class="sxs-lookup"><span data-stu-id="06556-121">GCM or ADM not correctly integrated in SDK (Android only - Service Specific Pushes).</span></span>
* <span data-ttu-id="06556-122">Nyomon követése nem megfelelően integrált SDK (telepítés tároló nyomon követése).</span><span class="sxs-lookup"><span data-stu-id="06556-122">Tracking not correctly integrated in SDK (Install store tracking).</span></span>
* <span data-ttu-id="06556-123">Lusta helyét vagy GPS helyét nem megfelelően integrált SDK (célcsoportkezelést által földrajzi helyhez).</span><span class="sxs-lookup"><span data-stu-id="06556-123">Lazy Location or GPS Location not correctly integrated in SDK (Targeting by geo-location).</span></span>

<span data-ttu-id="06556-124">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="06556-124">**See also:**</span></span>

* <span data-ttu-id="06556-125">[SDK-dokumentáció - integráció útmutatók][Link 5]</span><span class="sxs-lookup"><span data-stu-id="06556-125">[SDK Documentation - Integration Guides][Link 5]</span></span> 
* <span data-ttu-id="06556-126">[Hibaelhárítási útmutató - leküldéses][Link 23]</span><span class="sxs-lookup"><span data-stu-id="06556-126">[Troubleshooting Guide - Push][Link 23]</span></span>

#### <a name="sdk-upgrade"></a><span data-ttu-id="06556-127">SDK frissítése</span><span class="sxs-lookup"><span data-stu-id="06556-127">SDK Upgrade</span></span>
* <span data-ttu-id="06556-128">Az SDK-val (gyakran az eszköz operációs rendszer újabb verzióit kapcsolatos) a korábbi verzióival kapcsolatos problémák megoldásához SDK frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="06556-128">Need to upgrade SDK to resolve issues with older versions of the SDK (often related to newer versions of the device OS).</span></span>
* <span data-ttu-id="06556-129">Az alkalmazás összes korábbi verziójának eltávolítása az eszközről, és telepítse újra a legújabb verzióját, az alkalmazás a regisztrálja újra az Eszközazonosítót az Azure Mobile Engagement felhasználói felülete annak ellenőrzéséhez, hogy az eszköz az alkalmazás legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="06556-129">Uninstall all previous versions of your app from your device and reinstall the newest version of your app, the re-register your Device ID from the Azure Mobile Engagement UI to confirm that your device is using the newest version of your app.</span></span>

<span data-ttu-id="06556-130">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="06556-130">**See also:**</span></span>

* [<span data-ttu-id="06556-131">SDK-dokumentáció – kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="06556-131">SDK Documentation - Release Notes</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [<span data-ttu-id="06556-132">SDK-dokumentáció - frissítési útmutatók</span><span class="sxs-lookup"><span data-stu-id="06556-132">SDK Documentation - Upgrade Guides</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a><span data-ttu-id="06556-133">Egyéb SDK</span><span class="sxs-lookup"><span data-stu-id="06556-133">SDK Other</span></span>
* <span data-ttu-id="06556-134">Az Application Manifest "AndroidManifest.xml" hibákat okozhat, Azure Mobile Engagement nem működik (csak Android esetén).</span><span class="sxs-lookup"><span data-stu-id="06556-134">Errors in Application Manifest "AndroidManifest.xml" can cause Azure Mobile Engagement not to work (Android only).</span></span>
* <span data-ttu-id="06556-135">SDK-integrációval és API-használati egy gyakori probléma, hogy megzavarják a SDK és az API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="06556-135">A common issue with SDK integration and API usage is to confuse the SDK Key and the API Key.</span></span>

<span data-ttu-id="06556-136">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="06556-136">**See also:**</span></span>

* <span data-ttu-id="06556-137">[Alapfogalmak - szószedet][Link 6]</span><span class="sxs-lookup"><span data-stu-id="06556-137">[Concepts - Glossary][Link 6]</span></span>

## <a name="advanced-coding-issues"></a><span data-ttu-id="06556-138">Speciális problémák kódolása</span><span class="sxs-lookup"><span data-stu-id="06556-138">Advanced coding issues</span></span>
### <a name="issue"></a><span data-ttu-id="06556-139">Probléma</span><span class="sxs-lookup"><span data-stu-id="06556-139">Issue</span></span>
* <span data-ttu-id="06556-140">Platform adott kód, nem közvetlenül kapcsolódik az Azure Mobile Engagement az iOS, Android és Windows Phone hibákat is okozhat.</span><span class="sxs-lookup"><span data-stu-id="06556-140">Platform specific code not directly related to Azure Mobile Engagement can cause issues on iOS, Android, and Windows Phone.</span></span>

### <a name="causes"></a><span data-ttu-id="06556-141">Okok</span><span class="sxs-lookup"><span data-stu-id="06556-141">Causes</span></span>
* <span data-ttu-id="06556-142">Nem megfelelően megírt platform adott kód, nem közvetlenül kapcsolódik az Azure Mobile Engagement számos speciális az Azure Mobile Engagement kódolási probléma okozza.</span><span class="sxs-lookup"><span data-stu-id="06556-142">Many advanced coding issues with Azure Mobile Engagement are caused by improperly written platform specific code not directly related to Azure Mobile Engagement.</span></span> <span data-ttu-id="06556-143">Szüksége lesz a platformon, Azure Mobile Engagement dokumentáció (Android, iOS, webalkalmazás, Windows és Windows Phone) mellett a fejlesztői dokumentációból.</span><span class="sxs-lookup"><span data-stu-id="06556-143">You will need to consult documentation specific to the platform you are developing for in addition to Azure Mobile Engagement documentation (Android, iOS, Web, Windows, and Windows Phone).</span></span>
* <span data-ttu-id="06556-144">Nem megfelelően állítja be "kategóriák", megakadályozza, hogy egy értesítésből összekapcsolása egy másik helyen belül vagy kívül az alkalmazás (csak Android esetén).</span><span class="sxs-lookup"><span data-stu-id="06556-144">Not correctly configuring "categories", prevents linking from a notification to another location either inside or outside of the app (Android only).</span></span> 
* <span data-ttu-id="06556-145">Nem a beállítást "UIKit.framework" "választható", a kódban iOS "Szimbólum nem található a hiba" mutat be és/vagy a régebbi iOS-eszközök (csak iOS) összeomlik.</span><span class="sxs-lookup"><span data-stu-id="06556-145">Not setting "UIKit.framework" to "optional" in your iOS code, shows a "Symbol not found error" and/or crashes on older iOS devices (iOS only).</span></span>
* <span data-ttu-id="06556-146">Lejárt tanúsítványok, vagy nem megfelelően a cert okok fejlesztői vagy a termék verzióját használja leküldéses kapcsolatos problémákat (csak iOS esetén).</span><span class="sxs-lookup"><span data-stu-id="06556-146">Expired certificates or not correctly using the DEV or Prod version of the cert, causes push issues (iOS only).</span></span>
* <span data-ttu-id="06556-147">Bizonyos korlátozások is egy platform, amely az Azure Mobile Engagement vezérlésére nem képes (ilyen például a system center működéséről az alkalmazásból leküldéses értesítések Android és iOS) beépített szolgáltatásai.</span><span class="sxs-lookup"><span data-stu-id="06556-147">There are some limitations inherent to a platform that Azure Mobile Engagement can't control (like how the system center works for out of app pushes in Android and iOS).</span></span>
* <span data-ttu-id="06556-148">Az Azure Mobile Engagement teszi közzé a belső csomagok által használt Azure Mobile Engagement az iOS és Android hivatkozás teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="06556-148">Azure Mobile Engagement publishes a full list of the internal packages used by Azure Mobile Engagement for iOS and Android for reference.</span></span> <span data-ttu-id="06556-149">Ne feledje, hogy az Azure Mobile Engagement néhány szolgáltatása a platformon (Android, iOS, webalkalmazás, Windows és Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="06556-149">Keep in mind that some features of Azure Mobile Engagement are specific to the platform (Android, iOS, Web, Windows, and Windows Phone).</span></span>

### <a name="see-also"></a><span data-ttu-id="06556-150">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="06556-150">See also</span></span>
* <span data-ttu-id="06556-151">[Hibaelhárítási útmutató - leküldéses][Link 23]</span><span class="sxs-lookup"><span data-stu-id="06556-151">[Troubleshooting Guide - Push][Link 23]</span></span> 
* <span data-ttu-id="06556-152">[SDK-dokumentáció – kibocsátási megjegyzések][Link 5]</span><span class="sxs-lookup"><span data-stu-id="06556-152">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="06556-153">[SDK-dokumentáció - frissítési útmutatók][Link 5]</span><span class="sxs-lookup"><span data-stu-id="06556-153">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="application-crashes"></a><span data-ttu-id="06556-154">Alkalmazás-összeomlásokat</span><span class="sxs-lookup"><span data-stu-id="06556-154">Application crashes</span></span>
### <a name="issue"></a><span data-ttu-id="06556-155">Probléma</span><span class="sxs-lookup"><span data-stu-id="06556-155">Issue</span></span>
* <span data-ttu-id="06556-156">Az alkalmazás összeomlik a végfelhasználók eszközön.</span><span class="sxs-lookup"><span data-stu-id="06556-156">Your application crashes on the end users' device.</span></span>

### <a name="causes"></a><span data-ttu-id="06556-157">Okok</span><span class="sxs-lookup"><span data-stu-id="06556-157">Causes</span></span>
* <span data-ttu-id="06556-158">Összeomlási adatok az megtekinthetők a *Analytics felhasználói felület* vagy a *Analytics API*</span><span class="sxs-lookup"><span data-stu-id="06556-158">Crash information can be viewed in the *Analytics UI* or the *Analytics API*</span></span>
* <span data-ttu-id="06556-159">Keresse meg az Eszközazonosítót a vizsgálati eszköz, és hajtsa végre a azonos műveletet, ami miatt a végfelhasználó számára az összeomlások okának azonosításához összeomlását az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="06556-159">You can find the Device ID of your test device and take the same action that caused your application to crash for an end user to help identify the cause of your crash.</span></span>
* <span data-ttu-id="06556-160">Az Azure Mobile Engagement SDK ismert problémái miatt az alkalmazások összeomlási néha megoldó az SDK legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="06556-160">Known issues with the Azure Mobile Engagement SDK that cause applications to crash are sometimes resolved by upgrading to the latest version of the SDK.</span></span> <span data-ttu-id="06556-161">Ellenőrizze, hogy a kibocsátási megjegyzések a platformra vonatkozó összeomlások vizsgálatakor.</span><span class="sxs-lookup"><span data-stu-id="06556-161">Make sure to check the release notes about your platform when investigating crashes.</span></span>

### <a name="see-also"></a><span data-ttu-id="06556-162">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="06556-162">See also</span></span>
* <span data-ttu-id="06556-163">[SDK-dokumentáció – kibocsátási megjegyzések][Link 5]</span><span class="sxs-lookup"><span data-stu-id="06556-163">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="06556-164">[SDK-dokumentáció - frissítési útmutatók][Link 5]</span><span class="sxs-lookup"><span data-stu-id="06556-164">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="app-store-upload-failures"></a><span data-ttu-id="06556-165">Alkalmazás-áruház sikertelen feltöltés</span><span class="sxs-lookup"><span data-stu-id="06556-165">App store upload failures</span></span>
### <a name="issue"></a><span data-ttu-id="06556-166">Probléma</span><span class="sxs-lookup"><span data-stu-id="06556-166">Issue</span></span>
* <span data-ttu-id="06556-167">Az alkalmazás legújabb verzióját feltölteni az Apple, Google vagy a Windows App store kapcsolatos hibákat.</span><span class="sxs-lookup"><span data-stu-id="06556-167">Errors related to uploading the latest version of your app to Apple, Google, or the Windows App store.</span></span>

### <a name="causes"></a><span data-ttu-id="06556-168">Okok</span><span class="sxs-lookup"><span data-stu-id="06556-168">Causes</span></span>
* <span data-ttu-id="06556-169">Alkalmazás tárolja néha letiltja az alkalmazások az egyes engedélyezett funkciók (például az Apple Store áruházból megakadályozza, hogy az áruház alkalmazások idfv fogja elvégezni lehetővé használatát és a GooglePlay tároló megakadályozza, hogy az alkalmazással kapcsolatos adatok alkalmazások közötti megosztásának).</span><span class="sxs-lookup"><span data-stu-id="06556-169">App stores sometimes block apps with certain features enabled (e.g. the Apple Store prevents the use of IDFV in apps in the store and the GooglePlay store prevents the sharing of application information between apps).</span></span> 
* <span data-ttu-id="06556-170">Győződjön meg arról, hogy ellenőrizze a kibocsátási megjegyzések a platform és az SDK jelenlegi verzióját, ha olyan alkalmazást tölt fel az áruház nehézséget.</span><span class="sxs-lookup"><span data-stu-id="06556-170">Make sure that you check the release notes about your platform and current version of the SDK if you have difficulty uploading an app to the store.</span></span>

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

