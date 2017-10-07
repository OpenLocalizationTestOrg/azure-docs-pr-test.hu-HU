---
title: "Mobile Engagement felhasználói felület – aaaAzure elérni feltétel"
description: "Ismerje meg, hogyan toouse célcsoport-kezelési feltételek toosend leküldéses kampányokra tooa részhalmaz Azure Mobile Engagementet használó felhasználók"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: a4ed03a0-55b1-4dd8-b0bd-c475005afb66
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d956add1b7edc1d49451596019c5a4dec098d724
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-targeting-criteria-toosend-push-campaigns-tooa-select-subset-of-your-users"></a><span data-ttu-id="d1760-103">Hogyan toouse célcsoport-kezelési feltételek toosend leküldéses kampányokra tooa válassza ki a felhasználók egy részhalmazát</span><span class="sxs-lookup"><span data-stu-id="d1760-103">How toouse targeting criteria toosend push campaigns tooa select subset of your users</span></span>
<span data-ttu-id="d1760-104">A célközönség célzó hello "Új feltételek" gombra a megadott feltételek egyike hello leghatékonyabb jellemzői Azure Mobile Engagement, hogy megfelelő küldünk segít leküldéses értesítéseket, hogy hello ügyfelek válaszol-e a Mindenki levélszeméttel tooinstead.</span><span class="sxs-lookup"><span data-stu-id="d1760-104">Targeting your audience by specific criteria with hello "New Criteria" button is one of hello most powerful concepts in Azure Mobile Engagement that helps you send relevant push notifications that hello customers will respond tooinstead of Spamming everyone.</span></span> <span data-ttu-id="d1760-105">Korlátozza a célközönséget, a szabványos feltételek alapján, és leküldéses értesítések toodetermine szimulálása hányan hello értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="d1760-105">You can limit your audience based on standard criteria and simulate pushes toodetermine how many people will receive hello notification.</span></span>

<span data-ttu-id="d1760-106">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="d1760-106">**See also:**</span></span>

* <span data-ttu-id="d1760-107">[Leküldéses kampány új UI - a Reach - dokumentáció][Link 27]</span><span class="sxs-lookup"><span data-stu-id="d1760-107">[UI Documentation - Reach - New Push Campaign][Link 27]</span></span>

## <a name="audience-criteria-can-include"></a><span data-ttu-id="d1760-108">Célközönségre vonatkozó feltételeket az alábbiakból állhat:</span><span class="sxs-lookup"><span data-stu-id="d1760-108">Audience criteria can include:</span></span>
* <span data-ttu-id="d1760-109">** Technicals: ** alapján hello célba azonos technikai információkat láthatja a hello elemzés és -figyelő szakasz.</span><span class="sxs-lookup"><span data-stu-id="d1760-109">**Technicals: ** You can target based on hello same technical information you can see in hello Analytics and Monitor sections.</span></span> <span data-ttu-id="d1760-110">**További információ:** [felhasználói felületének dokumentációja – Analytics][Link 15], [felhasználói felületének dokumentációja - figyelő][Link 16]</span><span class="sxs-lookup"><span data-stu-id="d1760-110">**See also:** [UI Documentation - Analytics][Link 15],  [UI Documentation - Monitor][Link 16]</span></span>
* <span data-ttu-id="d1760-111">**Hely:** alkalmazásokat, amelyek Geokerítéseket "a hely jelentéskészítési valós idejű" használata földrajzi helyhez egy feltételek tootarget hello GPS helye a közönség használhatja.</span><span class="sxs-lookup"><span data-stu-id="d1760-111">**Location:** Applications that use "Real time location reporting" with Geo-Fencing can use Geo-Location as a criteria tootarget an audience from hello GPS location.</span></span> <span data-ttu-id="d1760-112">"Lusta hely jelentéskészítési" hívás is használt tootarget hello mobiltelefon helyről közönség ("a hely jelentéskészítési valós idejű" és "Lusta terület hely jelentéskészítési" aktiválni kell az hello SDK).</span><span class="sxs-lookup"><span data-stu-id="d1760-112">"Lazy Area Location Reporting" call also be used tootarget an audience from hello cell phone location ("Real time location reporting" and "Lazy Area Location Reporting" must be activated from hello SDK).</span></span> <span data-ttu-id="d1760-113">**További információ:** [SDK-dokumentáció - iOS - integráció][Link 5], [SDK-dokumentáció - Android - integráció][Link 5]</span><span class="sxs-lookup"><span data-stu-id="d1760-113">**See also:** [SDK Documentation - iOS -  Integration][Link 5], [SDK Documentation - Android -  Integration][Link 5]</span></span>
* <span data-ttu-id="d1760-114">**Reach-visszajelzés:** célba a célközönséget, a korábbi reach értesítések keresztül közlemények, szavazások és Adatleküldések reach visszajelzései visszajelzései alapján.</span><span class="sxs-lookup"><span data-stu-id="d1760-114">**Reach Feedback:** You can target your audience based on their feedback from previous reach notifications through reach feedback from Announcements, Polls, and Data Pushes.</span></span> <span data-ttu-id="d1760-115">Ez lehetővé teszi a célközönséget, miután két vagy három, mint amennyi kampányok sikerült hello először toobetter cél.</span><span class="sxs-lookup"><span data-stu-id="d1760-115">This enables you toobetter target your audience after two or three reach campaigns than you could hello first time.</span></span> <span data-ttu-id="d1760-116">Azt is használható, felhasználók, akik már úgy, hogy a kampány tooNOT a hasonló tartalmú értesítést kapott toofilter küldésének toousers, akik már érkezett az előző adott kampányra.</span><span class="sxs-lookup"><span data-stu-id="d1760-116">It can also be used toofilter out users who already received a notification with similar content, by setting a campaign tooNOT be sent toousers who already received a specific previous campaign.</span></span> <span data-ttu-id="d1760-117">Akkor is kizárhat a felhasználók számára tartalmazza egy adott kampányt, amely a még aktív az új leküldéses értesítések fogadása.</span><span class="sxs-lookup"><span data-stu-id="d1760-117">You can even exclude users who are included a specific campaign that is still active from receiving new Pushes.</span></span> <span data-ttu-id="d1760-118">**További információ:** [UI - a Reach - dokumentáció leküldéses tartalom][Link 29]</span><span class="sxs-lookup"><span data-stu-id="d1760-118">**See also:** [UI Documentation -  Reach - Push Content][Link 29]</span></span>
* <span data-ttu-id="d1760-119">**A telepítéskövetési:** nyomon követheti a felhasználók az alkalmazást futtató alapulnak.</span><span class="sxs-lookup"><span data-stu-id="d1760-119">**Install Tracking:** You can track information based on where your users installed your App.</span></span> <span data-ttu-id="d1760-120">**További információ:** [felhasználói felületének dokumentációja – beállítások][Link 20]</span><span class="sxs-lookup"><span data-stu-id="d1760-120">**See also:** [UI Documentation -  Settings][Link 20]</span></span>
* <span data-ttu-id="d1760-121">**Felhasználói profil:** akkor is a célként megadott általános jogú felhasználói adatok alapján, és cél létrehozott hello egyéni alkalmazásadatok alapján is.</span><span class="sxs-lookup"><span data-stu-id="d1760-121">**User Profile:** You can target based on standard user information and you can target based on hello custom app info that you have created.</span></span> <span data-ttu-id="d1760-122">Ez magában foglalja a jelenleg bejelentkezett felhasználók és a felhasználók megválaszolta adott kérdések hello alkalmazás maga helyett csak hogyan azok válaszolt tooprevious kampányok tooset kérték Önök őket.</span><span class="sxs-lookup"><span data-stu-id="d1760-122">This includes users who are currently logged in and users that have answered specific questions you have asked them tooset in hello app itself instead of just how they have responded tooprevious campaigns.</span></span> <span data-ttu-id="d1760-123">Minden alkalmazás adatához definiálva az alkalmazás megjelennek a listában.</span><span class="sxs-lookup"><span data-stu-id="d1760-123">All of your App Info's defined for your app show up on this list.</span></span>
* <span data-ttu-id="d1760-124">Szegmensek: Is alapján létrehozott szegmensek cél több feltételt tartalmazó felhasználói viselkedés alapján.</span><span class="sxs-lookup"><span data-stu-id="d1760-124">Segments: You can also target based on segments that you have created based on specific user behavior containing multiple criteria.</span></span> <span data-ttu-id="d1760-125">A szegmenseket, az alkalmazás definiált összes jelenik meg, a listában.</span><span class="sxs-lookup"><span data-stu-id="d1760-125">All of your segments defined for your app show up on this list.</span></span> <span data-ttu-id="d1760-126">**További információ:** [felhasználói felületének dokumentációja – szegmensek][Link 18]</span><span class="sxs-lookup"><span data-stu-id="d1760-126">**See also:** [UI Documentation -  Segments][Link 18]</span></span>
* <span data-ttu-id="d1760-127">**Alkalmazásadat:** egyéni App-Info címkék "Beállítások" tootrack felhasználói viselkedés hozható létre.</span><span class="sxs-lookup"><span data-stu-id="d1760-127">**App Info:** Custom App Info Tags can be created from “Settings” tootrack user behavior.</span></span> <span data-ttu-id="d1760-128">**További információ:** [felhasználói felületének dokumentációja – beállítások][Link 20]</span><span class="sxs-lookup"><span data-stu-id="d1760-128">**See also:** [UI Documentation -  Settings][Link 20]</span></span>

## <a name="example"></a><span data-ttu-id="d1760-129">Példa:</span><span class="sxs-lookup"><span data-stu-id="d1760-129">Example:</span></span>
<span data-ttu-id="d1760-130">Ha azt szeretné, hogy toopush egy közlemény csak toohello alárendelt a felhasználók csoportja, amely egy alkalmazásbeli végeztek vásárolja meg a műveletet.</span><span class="sxs-lookup"><span data-stu-id="d1760-130">If you want toopush an announcement only toohello sub-set of your users that have performed an in-app purchase action.</span></span>

1. <span data-ttu-id="d1760-131">Nyissa meg tooyour alkalmazás beállításait tartalmazó lap, válassza ki a hello "Alkalmazásadatok" menü, jelölje be az "Új app-info"</span><span class="sxs-lookup"><span data-stu-id="d1760-131">Go tooyour application settings page, select hello "App info" menu and select "New app info"</span></span>
2. <span data-ttu-id="d1760-132">Egy új "inAppPurchase" nevű logikai alkalmazásadatok regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d1760-132">Register a new Boolean app info called "inAppPurchase"</span></span>
3. <span data-ttu-id="d1760-133">Az alkalmazás beállítása az alkalmazásadatok túl "true" Győződjön amikor hello felhasználó sikeresen hajt végre az alkalmazáson belüli vásárlás (a hello sendAppInfo ("inAppPurchase",...) függvény)</span><span class="sxs-lookup"><span data-stu-id="d1760-133">Make your application set this app info too"true" when hello user successfully performs an in-app purchase (by using hello sendAppInfo("inAppPurchase", ...) function)</span></span>
4. <span data-ttu-id="d1760-134">Ha nem szeretné toodo Ez az alkalmazásról, ezt megteheti a háttérrendszerből hello eszköz API használatával)</span><span class="sxs-lookup"><span data-stu-id="d1760-134">If you don't want toodo this from your application, you can do it from your backend by using hello device API)</span></span>
5. <span data-ttu-id="d1760-135">Ezután egyszerűen toocreate a hirdetmény azokat a nézeteket, korlátozza a célközönség toousers, amelynek "inAppPurchase" beállítása túl "true")</span><span class="sxs-lookup"><span data-stu-id="d1760-135">Then, you just need toocreate your announcement, with a criterion limiting your audience toousers having "inAppPurchase" set too"true")</span></span>

> [!NOTE]
> <span data-ttu-id="d1760-136">Azure Mobile Engagement toogather információra a felhasználók eszközéről célzó eltérő app-info címkék feltétel alapján van szüksége, mielőtt hello leküldéses zajlik, és késést okozhat.</span><span class="sxs-lookup"><span data-stu-id="d1760-136">Targeting based on criteria other than app info tags requires Azure Mobile Engagement toogather information from your users' devices before hello push is sent and so can cause a delay.</span></span> <span data-ttu-id="d1760-137">Leküldéses értesítések összetett leküldéses konfigurációs beállítások (például a jelvényt frissítése) is késleltetheti-e.</span><span class="sxs-lookup"><span data-stu-id="d1760-137">Complex push configuration options (like updating badges) can also delay pushes.</span></span> <span data-ttu-id="d1760-138">Hello abszolút leggyorsabb ügyfélleküldéses módszer az Azure Mobile Engagement leküldéses API hello "egy hibaüzenetet" kampány a használata.</span><span class="sxs-lookup"><span data-stu-id="d1760-138">Using a "one shot" campaign from hello Push API is hello absolute fastest push method in Azure Mobile Engagement.</span></span> <span data-ttu-id="d1760-139">Hello következő leggyorsabb eljárás csak app-info címkék használatával leküldéses feltételként a Reach-kampány (vagy hello Reach API vagy a felhasználói felület hello) azért, mert az app-info címkék hello kiszolgálóoldali van tárolva.</span><span class="sxs-lookup"><span data-stu-id="d1760-139">Using only app info tags as push criteria for a Reach campaign (either from hello Reach API or hello UI) is hello next fastest method since app info tags are stored on hello server side.</span></span> <span data-ttu-id="d1760-140">Hello leginkább rugalmas, de a leghosszabb ügyfélleküldéses módszer más célcsoport-kezelési feltételek alapján a leküldéses kampány részeként azért, mert az Azure Mobile Engagement rendelkezik olyan eszközökkel, tooquery hello rendelés toosend hello kampány.</span><span class="sxs-lookup"><span data-stu-id="d1760-140">Using other targeting criteria for a push campaign is hello most flexible but slowest push method since Azure Mobile Engagement has tooquery hello devices in order toosend hello campaign.</span></span>

![Reach-Criterion1][29] 

## <a name="criterion-options-apply-to"></a><span data-ttu-id="d1760-142">Feltétel beállításai vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="d1760-142">Criterion Options Apply to:</span></span>
* <span data-ttu-id="d1760-143">**Technicals**</span><span class="sxs-lookup"><span data-stu-id="d1760-143">**Technicals**</span></span>     
* <span data-ttu-id="d1760-144">Belső vezérlőprogram name: belső vezérlőprogram neve</span><span class="sxs-lookup"><span data-stu-id="d1760-144">Firmware name:    Firmware name</span></span>
* <span data-ttu-id="d1760-145">Belső vezérlőprogram-verziója: belső vezérlőprogram-verziója</span><span class="sxs-lookup"><span data-stu-id="d1760-145">Firmware version:    Firmware version</span></span>
* <span data-ttu-id="d1760-146">Eszköz típusa: eszközmodell</span><span class="sxs-lookup"><span data-stu-id="d1760-146">Device model:    Device model</span></span>
* <span data-ttu-id="d1760-147">Eszköz gyártója: eszköz gyártója</span><span class="sxs-lookup"><span data-stu-id="d1760-147">Device manufacturer:    Device manufacturer</span></span>
* <span data-ttu-id="d1760-148">Alkalmazás-verzió: alkalmazás verziója</span><span class="sxs-lookup"><span data-stu-id="d1760-148">Application version:    Application version</span></span>
* <span data-ttu-id="d1760-149">Portvívő név: szolgáltatónként neve nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-149">Carrier name:    Carrier name, undefined</span></span>
* <span data-ttu-id="d1760-150">Portvívő ország: szolgáltatónként ország, nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-150">Carrier country:    Carrier country, undefined</span></span>
* <span data-ttu-id="d1760-151">Hálózati típusa: hálózati típusa</span><span class="sxs-lookup"><span data-stu-id="d1760-151">Network type:    Network type</span></span>
* <span data-ttu-id="d1760-152">Területi beállítás: területi beállítás</span><span class="sxs-lookup"><span data-stu-id="d1760-152">Locale:    Locale</span></span>
* <span data-ttu-id="d1760-153">Képernyő méretéhez: képernyő méretéhez</span><span class="sxs-lookup"><span data-stu-id="d1760-153">Screen size:    Screen size</span></span>
* <span data-ttu-id="d1760-154">**Hely**</span><span class="sxs-lookup"><span data-stu-id="d1760-154">**Location**</span></span>      
* <span data-ttu-id="d1760-155">Utolsó ismert terület: ország, régió, helység szerint.</span><span class="sxs-lookup"><span data-stu-id="d1760-155">Last known area:    Country, Region, Locality</span></span>
* <span data-ttu-id="d1760-156">Valós idejű geokerítések: lista a Poi (név, műveletek), a körkörös POI (név, szélesség hosszúság, Radius a mérőszámok)</span><span class="sxs-lookup"><span data-stu-id="d1760-156">Real time geo-fencing:    List of POIs (Name, Actions), Circular POI (Name, Latitude, Longitude, Radius in meters)</span></span>
* <span data-ttu-id="d1760-157">**Visszajelzés elérni**</span><span class="sxs-lookup"><span data-stu-id="d1760-157">**Reach feedback**</span></span>     
* <span data-ttu-id="d1760-158">Bejelentés visszajelzés: közlemény, visszajelzés</span><span class="sxs-lookup"><span data-stu-id="d1760-158">Announcement feedback:    Announcement, feedback</span></span>
* <span data-ttu-id="d1760-159">Kérdezze le visszajelzését: lekérdezési, visszajelzés</span><span class="sxs-lookup"><span data-stu-id="d1760-159">Poll feedback:    Poll, feedback</span></span>
* <span data-ttu-id="d1760-160">Kérdezze le a válasz-visszajelzés: kérdezze le a válasz visszajelzést, a kérdés, a választott</span><span class="sxs-lookup"><span data-stu-id="d1760-160">Poll answer feedback:    Poll answer feedback, question, choice</span></span>
* <span data-ttu-id="d1760-161">Adatok leküldéses visszajelzés: az Adatleküldés, visszajelzés</span><span class="sxs-lookup"><span data-stu-id="d1760-161">Data Push feedback:    Data Push, feedback</span></span>
* <span data-ttu-id="d1760-162">**A telepítéskövetési**</span><span class="sxs-lookup"><span data-stu-id="d1760-162">**Install Tracking**</span></span>     
* <span data-ttu-id="d1760-163">Tároló: Tároló nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-163">Store:    Store, Undefined</span></span>
* <span data-ttu-id="d1760-164">Forrás: Forrás nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-164">Source:    Source, Undefined</span></span>
* <span data-ttu-id="d1760-165">**Felhasználói profil**</span><span class="sxs-lookup"><span data-stu-id="d1760-165">**User profile**</span></span>     
* <span data-ttu-id="d1760-166">Nemét: csatlakozó vagy nő, nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-166">Gender:    male or female, undefined</span></span>
* <span data-ttu-id="d1760-167">Dátum születése: kezelő, dátum, nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-167">Birth date:    operator, date, undefined</span></span>
* <span data-ttu-id="d1760-168">Részt: IGAZ vagy hamis, nem definiált</span><span class="sxs-lookup"><span data-stu-id="d1760-168">Opt-in:    true or false, undefined</span></span>
* <span data-ttu-id="d1760-169">**Alkalmazásadatok**</span><span class="sxs-lookup"><span data-stu-id="d1760-169">**App Info**</span></span>      
* <span data-ttu-id="d1760-170">Karakterlánc: Karakterlánc, nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-170">String:    String, undefined</span></span>
* <span data-ttu-id="d1760-171">: Operátor, dátuma nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-171">Date:    operator, date, undefined</span></span>
* <span data-ttu-id="d1760-172">Egész szám: operátor, számot, nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-172">Integer:    operator, number, undefined</span></span>
* <span data-ttu-id="d1760-173">Logikai érték: IGAZ vagy hamis, nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="d1760-173">Boolean:    true or false, undefined</span></span>
* <span data-ttu-id="d1760-174">**Szegmens**</span><span class="sxs-lookup"><span data-stu-id="d1760-174">**Segment**</span></span>    
* <span data-ttu-id="d1760-175">(A legördülő listából) szegmensek neve (felhasználók megcélzása, akik nem tartozik ebbe a szegmensbe) kizárási.</span><span class="sxs-lookup"><span data-stu-id="d1760-175">Name of Segments (from dropdown list), Exclusion (target users that are not a part of this segment).</span></span>

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
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

