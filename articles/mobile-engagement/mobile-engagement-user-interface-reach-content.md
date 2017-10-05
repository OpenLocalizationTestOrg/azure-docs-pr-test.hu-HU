---
title: "Az Azure Mobile Engagement felhasználói felület - a Reach-tartalom"
description: "Az Azure Mobile Engagement leküldéses értesítéses kampányokkal különböző típusú egyedi tartalom kezelése"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: add64f06-43c9-475c-8722-51cd00bb844b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3741a43b74af5846e95e42d8a7b533621e780f2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a><span data-ttu-id="77c5d-103">A különböző típusú leküldéses értesítéses kampányokkal egyedi tartalmának kezelése</span><span class="sxs-lookup"><span data-stu-id="77c5d-103">How to manage the unique content of the different types of push notification campaigns</span></span>
<span data-ttu-id="77c5d-104">Egy új reach-kampány tartalomszakasz segítségével módosíthatja a közlemények, szavazások, Adatleküldések és Csempék (csak Windows Phone) tartalmát.</span><span class="sxs-lookup"><span data-stu-id="77c5d-104">You can use the Content section of a new reach campaign to modify the content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="77c5d-105">A tartalom leküldéses kampányokra lehet kampány típusának.</span><span class="sxs-lookup"><span data-stu-id="77c5d-105">The content setting of Push campaigns is specific to the type of campaign.</span></span> 

### <a name="content-types"></a><span data-ttu-id="77c5d-106">Tartalom típusa:</span><span class="sxs-lookup"><span data-stu-id="77c5d-106">Content types:</span></span>
* <span data-ttu-id="77c5d-107">Bejelentések</span><span class="sxs-lookup"><span data-stu-id="77c5d-107">Announcements</span></span>
* <span data-ttu-id="77c5d-108">Szavazások</span><span class="sxs-lookup"><span data-stu-id="77c5d-108">Polls</span></span>
* <span data-ttu-id="77c5d-109">Adatleküldések</span><span class="sxs-lookup"><span data-stu-id="77c5d-109">Data pushes</span></span>
* <span data-ttu-id="77c5d-110">Csempék (csak Windows Phone)</span><span class="sxs-lookup"><span data-stu-id="77c5d-110">Tiles (Windows Phone Only)</span></span>

## <a name="content-of-announcements"></a><span data-ttu-id="77c5d-111">Közlemények tartalma</span><span class="sxs-lookup"><span data-stu-id="77c5d-111">Content of Announcements</span></span>
 ![Reach-Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a><span data-ttu-id="77c5d-113">Hirdetmény típusának kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="77c5d-113">Choose the type of your announcement:</span></span>
* <span data-ttu-id="77c5d-114">Csak értesítés: egy egyszerű standard szintű értesítési.</span><span class="sxs-lookup"><span data-stu-id="77c5d-114">Notification only: It is a simple standard notification.</span></span> <span data-ttu-id="77c5d-115">Azt jelenti, hogy ha a felhasználó kattint, további nézet nélkül jelenik meg, de csak a hozzá tartozó műveletet hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="77c5d-115">Meaning that if a user clicks on it, no additional view will appear, but only the action associated to it will occur.</span></span>
* <span data-ttu-id="77c5d-116">Szöveg közlemény: egy értesítés, amely kapcsolatba lép a felhasználó számára a szöveges nézet, tekintse meg a legyen.</span><span class="sxs-lookup"><span data-stu-id="77c5d-116">Text announcement: It is a notification that engages the user to have a look at a text view.</span></span>
* <span data-ttu-id="77c5d-117">Webes hirdetmény: egy értesítés, amely kapcsolatba lép a felhasználó számára, tekintse meg a következő a webes nézet.</span><span class="sxs-lookup"><span data-stu-id="77c5d-117">Web announcement: It is a notification that engages the user to have a look at a web view.</span></span>

### <a name="see-also"></a><span data-ttu-id="77c5d-118">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="77c5d-118">See also</span></span>
* <span data-ttu-id="77c5d-119">[A reach - hogyan Tos - közlemények][Link 3]</span><span class="sxs-lookup"><span data-stu-id="77c5d-119">[Reach - How Tos - Announcements][Link 3]</span></span> 

### <a name="about-web-view-announcements"></a><span data-ttu-id="77c5d-120">Vonatkozó webes nézetre mutató hirdetmények:</span><span class="sxs-lookup"><span data-stu-id="77c5d-120">About Web View Announcements:</span></span>
<span data-ttu-id="77c5d-121">Előforduló "{deviceid}" a HTML-kódot vagy JavaScript-kód itt automatikusan felülírja a közlemény megjelenítő eszköz azonosítója.</span><span class="sxs-lookup"><span data-stu-id="77c5d-121">Occurrences of the pattern "{deviceid}" in the HTML code or JavaScript code you provide here will be automatically replaced by the identifier of the device displaying the announcement.</span></span> <span data-ttu-id="77c5d-122">Ez az Azure Mobile Engagement-eszközazonosítók a üzemeltetett külső webszolgáltatásokból a beolvasandó egyszerűen.</span><span class="sxs-lookup"><span data-stu-id="77c5d-122">This is an easy way to retrieve Azure Mobile Engagement device identifiers in an external web service hosted on your back office.</span></span>
<span data-ttu-id="77c5d-123">Ha teljes képernyős webes nézetet szeretne létrehozni (az alapértelmezett akciógombok és kilépési gombok nélkül), a webes hirdetmény JavaScript-kódjában szereplő következő funkciókat használhatja:</span><span class="sxs-lookup"><span data-stu-id="77c5d-123">If you want to create a full screen web view (without the default Action and Exit buttons we provide) you can use the following functions from your web view announcement's JavaScript code:</span></span> 

* <span data-ttu-id="77c5d-124">a hirdetményművelet végrehajtása: ReachContent.actionContent()</span><span class="sxs-lookup"><span data-stu-id="77c5d-124">perform the announcement action: ReachContent.actionContent()</span></span>
* <span data-ttu-id="77c5d-125">Kilépés a hirdetményből: ReachContent.exitContent()</span><span class="sxs-lookup"><span data-stu-id="77c5d-125">exit from the announcement: ReachContent.exitContent()</span></span>

### <a name="choose-your-action"></a><span data-ttu-id="77c5d-126">Válassza ki a műveletet:</span><span class="sxs-lookup"><span data-stu-id="77c5d-126">Choose your Action:</span></span>
### <a name="about-action-urls"></a><span data-ttu-id="77c5d-127">Kapcsolatos művelet URL-címek:</span><span class="sxs-lookup"><span data-stu-id="77c5d-127">About Action URLs:</span></span>
<span data-ttu-id="77c5d-128">A megcélzott eszköz operációs rendszere által értelmezhető valamennyi URL-cím használható műveleti URL-címként.</span><span class="sxs-lookup"><span data-stu-id="77c5d-128">Any URL that can be interpreted by a targeted device's operating system can be used as an action URL.</span></span>
<span data-ttu-id="77c5d-129">Az alkalmazása által támogatott dedikált URL-címek (köztük azok, amelyek egy adott képernyőre irányítják a felhasználót) szintén használhatók műveleti URL-címként.</span><span class="sxs-lookup"><span data-stu-id="77c5d-129">Any dedicated URL that your application might support (e.g. to make users jump to a particular screen) can also be used as an action URL.</span></span>
<span data-ttu-id="77c5d-130">A {deviceid} minta összes előfordulásának automatikusan cseréli le a műveletet végrehajtó eszköz azonosítója.</span><span class="sxs-lookup"><span data-stu-id="77c5d-130">Each occurrence of the {deviceid} pattern is automatically replaced by the identifier of the device performing the action.</span></span> <span data-ttu-id="77c5d-131">Ez egyszerűen beolvashatók az Azure Mobile Engagement-eszközazonosítók a üzemeltetett külső webszolgáltatáson keresztül is használható.</span><span class="sxs-lookup"><span data-stu-id="77c5d-131">This can be used to easily retrieve Azure Mobile Engagement device identifiers via an external web service hosted on your back office.</span></span>

* <span data-ttu-id="77c5d-132">**Android + iOS műveletek**</span><span class="sxs-lookup"><span data-stu-id="77c5d-132">**Android + iOS actions**</span></span>
  * <span data-ttu-id="77c5d-133">Weblap megnyitása</span><span class="sxs-lookup"><span data-stu-id="77c5d-133">Open a web page</span></span>
  * <span data-ttu-id="77c5d-134">http://\[web-site-tartomány\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-134">http://\[web-site-domain\]</span></span> 
  * <span data-ttu-id="77c5d-135">Példa: http://www.azure.com</span><span class="sxs-lookup"><span data-stu-id="77c5d-135">Example:http://www.azure.com</span></span>
  * <span data-ttu-id="77c5d-136">E-mail küldése</span><span class="sxs-lookup"><span data-stu-id="77c5d-136">Send an e-mail</span></span>
  * <span data-ttu-id="77c5d-137">mailto:\[-címzett\]? tulajdonos =\[tulajdonos\]& body =\[üzenet\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-137">mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\]</span></span> 
  * <span data-ttu-id="77c5d-138">Example:mailto:foo@example.com? tulajdonos = a hónap % 20from % 20Azure % 20Mobile % 20Engagement! & body = jó % 20stuff!</span><span class="sxs-lookup"><span data-stu-id="77c5d-138">Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!</span></span>
  * <span data-ttu-id="77c5d-139">SMS küldése</span><span class="sxs-lookup"><span data-stu-id="77c5d-139">Send a SMS</span></span>
  * <span data-ttu-id="77c5d-140">SMS:\[-telefonszám\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-140">sms:\[phone-number\]</span></span> 
  * <span data-ttu-id="77c5d-141">Példa: sms:2125551212</span><span class="sxs-lookup"><span data-stu-id="77c5d-141">Example:sms:2125551212</span></span>
  * <span data-ttu-id="77c5d-142">Telefonszám tárcsázása</span><span class="sxs-lookup"><span data-stu-id="77c5d-142">Dial a phone number</span></span>
  * <span data-ttu-id="77c5d-143">Tel:\[-telefonszám\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-143">tel:\[phone-number\]</span></span> 
  * <span data-ttu-id="77c5d-144">Példa: tel:2125551212</span><span class="sxs-lookup"><span data-stu-id="77c5d-144">Example:tel:2125551212</span></span>
* <span data-ttu-id="77c5d-145">**Android csak műveletek**</span><span class="sxs-lookup"><span data-stu-id="77c5d-145">**Android only actions**</span></span>
  * <span data-ttu-id="77c5d-146">Alkalmazás letöltése a Play Áruházból</span><span class="sxs-lookup"><span data-stu-id="77c5d-146">Download an application on the Play Store</span></span>
  * <span data-ttu-id="77c5d-147">Market://details?ID=\[alkalmazáscsomag\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-147">market://details?id=\[app package\]</span></span> 
  * <span data-ttu-id="77c5d-148">Példa: market://details?id=com.microsoft.office.word</span><span class="sxs-lookup"><span data-stu-id="77c5d-148">Example:market://details?id=com.microsoft.office.word</span></span>
  * <span data-ttu-id="77c5d-149">Geolokációs keresés indítása</span><span class="sxs-lookup"><span data-stu-id="77c5d-149">Start a geo-localized search</span></span>
  * <span data-ttu-id="77c5d-150">GEO:0, 0? q =\[keresési lekérdezés\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-150">geo:0,0?q=\[search query\]</span></span> 
  * <span data-ttu-id="77c5d-151">Példa: geo:0, 0? q = starbucks, Párizsi</span><span class="sxs-lookup"><span data-stu-id="77c5d-151">Example:geo:0,0?q=starbucks,paris</span></span>
* <span data-ttu-id="77c5d-152">**csak iOS-műveletek**</span><span class="sxs-lookup"><span data-stu-id="77c5d-152">**iOS only actions**</span></span>
  * <span data-ttu-id="77c5d-153">Alkalmazás letöltése az App Store-ból</span><span class="sxs-lookup"><span data-stu-id="77c5d-153">Download an application on the App Store</span></span>
  * <span data-ttu-id="77c5d-154">http://iTunes.apple.com/ [Ország] /app/ [alkalmazás neve] /id [alkalmazás azonosítója]? mt = 8</span><span class="sxs-lookup"><span data-stu-id="77c5d-154">http://itunes.apple.com/[country]/app/[app name]/id[app id]?mt=8</span></span> 
  * <span data-ttu-id="77c5d-155">Példa: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8</span><span class="sxs-lookup"><span data-stu-id="77c5d-155">Example:http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8</span></span>
  * <span data-ttu-id="77c5d-156">Windows-műveletek</span><span class="sxs-lookup"><span data-stu-id="77c5d-156">Windows Actions</span></span>
  * <span data-ttu-id="77c5d-157">Weblap megnyitása</span><span class="sxs-lookup"><span data-stu-id="77c5d-157">Open a web page</span></span>
  * <span data-ttu-id="77c5d-158">http://\[web-site-tartomány\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-158">http://\[web-site-domain\]</span></span> 
  * <span data-ttu-id="77c5d-159">Példa: http://www.azure.com</span><span class="sxs-lookup"><span data-stu-id="77c5d-159">Example:http://www.azure.com</span></span>
  * <span data-ttu-id="77c5d-160">E-mail küldése</span><span class="sxs-lookup"><span data-stu-id="77c5d-160">Send an e-mail</span></span>
  * <span data-ttu-id="77c5d-161">mailto:\[-címzett\]? tulajdonos =\[tulajdonos\]& body =\[üzenet\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-161">mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\]</span></span> 
  * <span data-ttu-id="77c5d-162">Example:mailto:foo@example.com? tulajdonos = a hónap % 20from % 20Azure % 20Mobile % 20Engagement! & body = jó % 20stuff!</span><span class="sxs-lookup"><span data-stu-id="77c5d-162">Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!</span></span>
  * <span data-ttu-id="77c5d-163">SMS küldése (az Áruházból letölthető Skype alkalmazás szükséges hozzá)</span><span class="sxs-lookup"><span data-stu-id="77c5d-163">Send a SMS (Skype Store App required)</span></span>
  * <span data-ttu-id="77c5d-164">SMS:\[-telefonszám\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-164">sms:\[phone-number\]</span></span> 
  * <span data-ttu-id="77c5d-165">Példa: sms:2125551212</span><span class="sxs-lookup"><span data-stu-id="77c5d-165">Example:sms:2125551212</span></span>
  * <span data-ttu-id="77c5d-166">Telefonszám tárcsázása (az Áruházból letölthető Skype alkalmazás szükséges hozzá)</span><span class="sxs-lookup"><span data-stu-id="77c5d-166">Dial a phone number (Skype Store App required)</span></span>
  * <span data-ttu-id="77c5d-167">Tel:\[-telefonszám\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-167">tel:\[phone-number\]</span></span> 
  * <span data-ttu-id="77c5d-168">Példa: tel:2125551212</span><span class="sxs-lookup"><span data-stu-id="77c5d-168">Example:tel:2125551212</span></span>
  * <span data-ttu-id="77c5d-169">Alkalmazás letöltése a Play Áruházból</span><span class="sxs-lookup"><span data-stu-id="77c5d-169">Download an application on the Play Store</span></span>
  * <span data-ttu-id="77c5d-170">MS-windows-tároló: PDP? PFN =\[app Csomagazonosító\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-170">ms-windows-store:PDP?PFN=\[app package ID\]</span></span> 
  * <span data-ttu-id="77c5d-171">Példa: ms-windows-tároló: PDP? PFN 4d91298a-07cb-40fb-aecc-4cb5615d53c1 =</span><span class="sxs-lookup"><span data-stu-id="77c5d-171">Example:ms-windows-store:PDP?PFN=4d91298a-07cb-40fb-aecc-4cb5615d53c1</span></span>
  * <span data-ttu-id="77c5d-172">Keresés a Bing Térképek szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="77c5d-172">Start a bingmaps search</span></span>
  * <span data-ttu-id="77c5d-173">bingmaps:? q =\[keresési lekérdezés\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-173">bingmaps:?q=\[search query\]</span></span> 
  * <span data-ttu-id="77c5d-174">Példa: bingmaps:? q = starbucks, Párizsi</span><span class="sxs-lookup"><span data-stu-id="77c5d-174">Example:bingmaps:?q=starbucks,paris</span></span>
  * <span data-ttu-id="77c5d-175">Egyéni séma használata</span><span class="sxs-lookup"><span data-stu-id="77c5d-175">Use a custom scheme</span></span>
  * <span data-ttu-id="77c5d-176">\[egyéni séma\]://\[egyéni séma paraméterei\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-176">\[custom scheme\]://\[custom scheme params\]</span></span> 
  * <span data-ttu-id="77c5d-177">Példa: myCustomProtocol://myCustomParams</span><span class="sxs-lookup"><span data-stu-id="77c5d-177">Example:myCustomProtocol://myCustomParams</span></span>
  * <span data-ttu-id="77c5d-178">Csomagadatok (tárolóalkalmazás olvassa el a kiterjesztés kötelező) használata</span><span class="sxs-lookup"><span data-stu-id="77c5d-178">Use a package data (Store App for extension read required)</span></span>
  * <span data-ttu-id="77c5d-179">\[mappa\]\[adatok\].\[ bővítmény\]</span><span class="sxs-lookup"><span data-stu-id="77c5d-179">\[folder\]\[data\].\[extension\]</span></span> 
  * <span data-ttu-id="77c5d-180">Example:myfolderdata.txt</span><span class="sxs-lookup"><span data-stu-id="77c5d-180">Example:myfolderdata.txt</span></span>

### <a name="build-a-tracking-url"></a><span data-ttu-id="77c5d-181">A követési URL-cím összeállítása:</span><span class="sxs-lookup"><span data-stu-id="77c5d-181">Build a Tracking URL:</span></span>
* <span data-ttu-id="77c5d-182">A "Beállítások" című része a <UI Documentation> az utasítás felépítésével egy követési URL-címet, amely segítségével a felhasználók az egyéb alkalmazások letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="77c5d-182">See the “Settings” section of the <UI Documentation> for instruction on building a tracking URL that will allow users to download one of your other applications.</span></span>

### <a name="define-the-texts-of-your-announcement"></a><span data-ttu-id="77c5d-183">Hirdetmény szövegeinek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="77c5d-183">Define the texts of your announcement</span></span>
<span data-ttu-id="77c5d-184">Adja meg a cím, a tartalom és a hirdetmény szövegeinek kiválasztása gombra.</span><span class="sxs-lookup"><span data-stu-id="77c5d-184">Fill in the title, content, and button texts of your announcement.</span></span> <span data-ttu-id="77c5d-185">A felhasználók hogyan válaszolt a kampány reach visszajelzések alapján jövőbeli kampány közönség célba.</span><span class="sxs-lookup"><span data-stu-id="77c5d-185">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="77c5d-186">Célközönség kiválasztását, hogy a kampány lett csak leküldött, megválaszolt, műveletet kiváltó, illetve amelyekből kiléptek visszajelzést is alapulhat.</span><span class="sxs-lookup"><span data-stu-id="77c5d-186">Audience targeting can be based on the feedback of whether this campaign was just pushed, replied, actioned, or exited.</span></span>

### <a name="see-also"></a><span data-ttu-id="77c5d-187">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="77c5d-187">See also</span></span>
* <span data-ttu-id="77c5d-188">[Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]</span><span class="sxs-lookup"><span data-stu-id="77c5d-188">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-polls"></a><span data-ttu-id="77c5d-189">Szavazások tartalma</span><span class="sxs-lookup"><span data-stu-id="77c5d-189">Content of Polls</span></span>
![Reach-Content2][31] 

<span data-ttu-id="77c5d-191">Töltse ki a címét, leírását és hirdetmény szövegeinek kiválasztása gombra.</span><span class="sxs-lookup"><span data-stu-id="77c5d-191">Fill in the title, description, and button texts of your announcement.</span></span> <span data-ttu-id="77c5d-192">Adja hozzá a kérdések és a kérdésekre adott válaszokat lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="77c5d-192">Then, add questions and choices for the answers to your questions.</span></span>
<span data-ttu-id="77c5d-193">A felhasználók hogyan válaszolt a kampány reach visszajelzések alapján jövőbeli kampány közönség célba.</span><span class="sxs-lookup"><span data-stu-id="77c5d-193">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="77c5d-194">Célközönség-e a kampány lett csak leküldött, megválaszolt, műveletet kiváltó, illetve amelyekből kiléptek is alapulhat.</span><span class="sxs-lookup"><span data-stu-id="77c5d-194">Audience targeting can be based on whether this campaign was just pushed, replied, actioned, or exited.</span></span> <span data-ttu-id="77c5d-195">A lekérdezési válasz visszajelzést, ahol a kérdés és válasz választott használt feltételként is alapulhat célközönség kiválasztását.</span><span class="sxs-lookup"><span data-stu-id="77c5d-195">Audience targeting can also be based on Poll answer feedback, where the question and answer choice are used as criteria.</span></span>

### <a name="see-also"></a><span data-ttu-id="77c5d-196">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="77c5d-196">See also</span></span>
* <span data-ttu-id="77c5d-197">[Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]</span><span class="sxs-lookup"><span data-stu-id="77c5d-197">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-data-pushes"></a><span data-ttu-id="77c5d-198">Adatleküldések tartalma</span><span class="sxs-lookup"><span data-stu-id="77c5d-198">Content of Data Pushes</span></span>
![Reach-Content3][32] 

### <a name="choose-the-type-of-your-data"></a><span data-ttu-id="77c5d-200">Az adatok típusának kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="77c5d-200">Choose the type of your data:</span></span>
* <span data-ttu-id="77c5d-201">Szöveg</span><span class="sxs-lookup"><span data-stu-id="77c5d-201">Text</span></span>
* <span data-ttu-id="77c5d-202">Bináris adatok</span><span class="sxs-lookup"><span data-stu-id="77c5d-202">Binary data</span></span>
* <span data-ttu-id="77c5d-203">A Base64 adatok</span><span class="sxs-lookup"><span data-stu-id="77c5d-203">Base64 data</span></span>

### <a name="define-the-content-of-your-data"></a><span data-ttu-id="77c5d-204">Az adatok tartalmának meghatározása</span><span class="sxs-lookup"><span data-stu-id="77c5d-204">Define the content of your data</span></span>
* <span data-ttu-id="77c5d-205">Kijelölt szöveg adatokat küldeni, ha másolja és illessze be a "tartalom" jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="77c5d-205">If you selected to push text data, copy and paste the text into the "content" box.</span></span>
* <span data-ttu-id="77c5d-206">Ha bináris vagy base64 adatok leküldéses választotta, a "a fájl feltöltése" gomb segítségével feltölteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="77c5d-206">If you selected to push either binary or base64 data, use the "upload your file" button to upload your file.</span></span>
* <span data-ttu-id="77c5d-207">A felhasználók hogyan válaszolt a kampány reach visszajelzések alapján jövőbeli kampány közönség célba.</span><span class="sxs-lookup"><span data-stu-id="77c5d-207">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="77c5d-208">Célközönség-e a kampány lett csak leküldött, megválaszolt, műveletet kiváltó, illetve amelyekből kiléptek is alapulhat.</span><span class="sxs-lookup"><span data-stu-id="77c5d-208">Audience targeting can be based on whether this campaign was just pushed, replied, actioned, or exited.</span></span>

### <a name="see-also"></a><span data-ttu-id="77c5d-209">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="77c5d-209">See also</span></span>
* <span data-ttu-id="77c5d-210">[Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]</span><span class="sxs-lookup"><span data-stu-id="77c5d-210">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-tiles-windows-phone-only"></a><span data-ttu-id="77c5d-211">Tartalom csempék (csak Windows Phone)</span><span class="sxs-lookup"><span data-stu-id="77c5d-211">Content of Tiles (Windows Phone only)</span></span>
![Reach-Content4][33]

### <a name="define-the-content-of-your-tile"></a><span data-ttu-id="77c5d-213">A csempe tartalmának meghatározása</span><span class="sxs-lookup"><span data-stu-id="77c5d-213">Define the content of your tile</span></span>
<span data-ttu-id="77c5d-214">A csempe payload az alkalmazás a Windows Phone-eszközökön a csempén megjelenő szöveg.</span><span class="sxs-lookup"><span data-stu-id="77c5d-214">The tile payload is the text to be displayed in the tile of your app on Windows Phone devices.</span></span>
<span data-ttu-id="77c5d-215">Egy mozaik leküldéses a Windows Phone natív leküldéses a Microsoft leküldéses értesítési szolgáltatásának (MPNS) verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="77c5d-215">A tile push is the Microsoft Push Notification Service (MPNS) version of a native push for Windows Phone.</span></span> <span data-ttu-id="77c5d-216">A csempe leküldéses típus csak akkor leküldéses, amelynek nincs választ, és így jövőbeli kampányok célközönségét nem hozható létre, az eredmények a csempe leküldéses kampány.</span><span class="sxs-lookup"><span data-stu-id="77c5d-216">The tile push type is the only push type that does not have a response and so the audience of future campaigns can't be built on the results of a tile push campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="77c5d-217">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="77c5d-217">See also</span></span>
* <span data-ttu-id="77c5d-218">[API - a Reach API - dokumentáció natív leküldéssel][Link 4]</span><span class="sxs-lookup"><span data-stu-id="77c5d-218">[API Documentation - Reach API - Native Push][Link 4]</span></span>

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

