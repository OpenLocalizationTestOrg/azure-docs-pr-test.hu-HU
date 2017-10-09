---
title: "aaaAzure Mobile Engagement felhasználói felület - a Reach-kampány"
description: "Laern hogyan toocreate és használata az Azure Mobile Engagement leküldéses értesítéses kampányokkal kezelése"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2fe124a2-a86f-4136-81ba-a9d298ec798a
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 825e550ace63a34d1a90b10fa976a61eb15a6d04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-push-notification-campaigns"></a><span data-ttu-id="f9edc-103">Hogyan toocreate és leküldéses értesítéses kampányokkal kezelése</span><span class="sxs-lookup"><span data-stu-id="f9edc-103">How toocreate and manage push notification campaigns</span></span>
<span data-ttu-id="f9edc-104">Hello hello felhasználói felület toocreate új leküldéses kampány Reach szakasza toosend leküldéses értesítés szükséges összes hello információk megadásával összetett képlet használható.</span><span class="sxs-lookup"><span data-stu-id="f9edc-104">You can use hello Reach section of hello UI toocreate a new Push campaign with a complex formula by providing all hello information you need toosend a push notification.</span></span> <span data-ttu-id="f9edc-105">hello-beállítások a leküldéses kampány változnak némileg hello négy típusok: közlemények, szavazások, Adatleküldések és Csempék (csak Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="f9edc-105">hello options of a Push campaign vary slightly depending on hello four campaign types: Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span>

### <a name="option-applies-to"></a><span data-ttu-id="f9edc-106">A beállítás a következőkre vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="f9edc-106">Option Applies to:</span></span>
* <span data-ttu-id="f9edc-107">Nyelv: Az összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-107">Languages:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="f9edc-108">A kampány: Összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-108">Campaign:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="f9edc-109">Értesítés: Közlemények, szavazások</span><span class="sxs-lookup"><span data-stu-id="f9edc-109">Notification:     Announcements, Polls</span></span>
* <span data-ttu-id="f9edc-110">Tartalom: Az egyes kampány egyedi</span><span class="sxs-lookup"><span data-stu-id="f9edc-110">Content:    Unique for each campaign type</span></span>
* <span data-ttu-id="f9edc-111">A célközönség: Összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-111">Audience:     All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="f9edc-112">Időkereten belül: közlemények, szavazások, Csempéket</span><span class="sxs-lookup"><span data-stu-id="f9edc-112">Time frame:     Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="f9edc-113">Teszt: Az összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-113">Test:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>

![Reach-Campaign1][20]

## <a name="languages"></a><span data-ttu-id="f9edc-115">Nyelvek</span><span class="sxs-lookup"><span data-stu-id="f9edc-115">Languages</span></span>
<span data-ttu-id="f9edc-116">Hello nyelvek legördülő menü toosend a leküldéses toodevices toouse különböző nyelveken beállított egy másik verziója is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f9edc-116">You can use hello Languages drop-down menu toosend a different version of your Push toodevices that are set toouse different languages.</span></span> <span data-ttu-id="f9edc-117">Alapértelmezés szerint minden eszköz azonos leküldéses, függetlenül attól, milyen nyelven be lettek állítva toouse hello fog kapni.</span><span class="sxs-lookup"><span data-stu-id="f9edc-117">By default, all devices will receive hello same Push regardless of what language they are set toouse.</span></span> <span data-ttu-id="f9edc-118">Az eszköz set tooa különböző nyelvű felhasználók kapnak hello leküldéses hello alapértelmezett nyelvi verzióját.</span><span class="sxs-lookup"><span data-stu-id="f9edc-118">Users with their device set tooa different language will receive hello Default Language version of hello Push.</span></span> <span data-ttu-id="f9edc-119">Hello leküldéses kampány beállítások lehetővé teszik toospecify alternatív tartalmát az egyes hello további nyelveket választ.</span><span class="sxs-lookup"><span data-stu-id="f9edc-119">Many of hello push campaign options allow you toospecify alternate content for each of hello additional languages you select.</span></span> 

![Reach-Campaign2][21]

### <a name="language-differences-apply-to"></a><span data-ttu-id="f9edc-121">Nyelvi különbségek vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="f9edc-121">Language differences apply to:</span></span>
* <span data-ttu-id="f9edc-122">Nyelvek: Egyedi nyelvek lehet kiválasztani hozzáadása toohello az alapértelmezett nyelven</span><span class="sxs-lookup"><span data-stu-id="f9edc-122">Languages:    Unique languages may be selected in addition toohello default language</span></span>
* <span data-ttu-id="f9edc-123">Kampány: Az összes nyelven azonos</span><span class="sxs-lookup"><span data-stu-id="f9edc-123">Campaign:    Same for all languages</span></span>
* <span data-ttu-id="f9edc-124">Értesítés: Az egyes nyelvekhez egyedi továbbá toohello alapértelmezett nyelv</span><span class="sxs-lookup"><span data-stu-id="f9edc-124">Notification:    Unique for each language in addition toohello default language</span></span>
* <span data-ttu-id="f9edc-125">Tartalom: Az egyes nyelvekhez egyedi továbbá toohello alapértelmezett nyelv</span><span class="sxs-lookup"><span data-stu-id="f9edc-125">Content:    Unique for each language in addition toohello default language</span></span>
* <span data-ttu-id="f9edc-126">A célközönséget: Előfordulhat, hogy egy külön nyelvi feltételek alapján úgy szűri</span><span class="sxs-lookup"><span data-stu-id="f9edc-126">Audience:     May be filtered by a separate language criterion</span></span>
* <span data-ttu-id="f9edc-127">Időkereten belül: ugyanazt az összes nyelven</span><span class="sxs-lookup"><span data-stu-id="f9edc-127">Time frame:     Same for all languages</span></span>
* <span data-ttu-id="f9edc-128">Teszt: Küldhet tooeach nyelvi egyszerre</span><span class="sxs-lookup"><span data-stu-id="f9edc-128">Test:    May be sent tooeach language at a time</span></span>

### <a name="supported-languages"></a><span data-ttu-id="f9edc-129">Támogatott nyelvek:</span><span class="sxs-lookup"><span data-stu-id="f9edc-129">Supported Languages:</span></span>
* <span data-ttu-id="f9edc-130">Arab (ar)</span><span class="sxs-lookup"><span data-stu-id="f9edc-130">Arabic (ar)</span></span> 
* <span data-ttu-id="f9edc-131">Bolgár (bg)</span><span class="sxs-lookup"><span data-stu-id="f9edc-131">Bulgarian (bg)</span></span> 
* <span data-ttu-id="f9edc-132">Katalán (ca)</span><span class="sxs-lookup"><span data-stu-id="f9edc-132">Catalan (ca)</span></span> 
* <span data-ttu-id="f9edc-133">Kínai (zh)</span><span class="sxs-lookup"><span data-stu-id="f9edc-133">Chinese (zh)</span></span> 
* <span data-ttu-id="f9edc-134">Horvát (hr)</span><span class="sxs-lookup"><span data-stu-id="f9edc-134">Croatian (hr)</span></span> 
* <span data-ttu-id="f9edc-135">Czech (CS)</span><span class="sxs-lookup"><span data-stu-id="f9edc-135">Czech (cs)</span></span> 
* <span data-ttu-id="f9edc-136">Dán (da)</span><span class="sxs-lookup"><span data-stu-id="f9edc-136">Danish (da)</span></span> 
* <span data-ttu-id="f9edc-137">Holland (Hollandia)</span><span class="sxs-lookup"><span data-stu-id="f9edc-137">Dutch (nl)</span></span> 
* <span data-ttu-id="f9edc-138">Angol (en)</span><span class="sxs-lookup"><span data-stu-id="f9edc-138">English (en)</span></span> 
* <span data-ttu-id="f9edc-139">Finn (fi)</span><span class="sxs-lookup"><span data-stu-id="f9edc-139">Finnish (fi)</span></span> 
* <span data-ttu-id="f9edc-140">Francia (fr)</span><span class="sxs-lookup"><span data-stu-id="f9edc-140">French (fr)</span></span> 
* <span data-ttu-id="f9edc-141">Német (de)</span><span class="sxs-lookup"><span data-stu-id="f9edc-141">German (de)</span></span> 
* <span data-ttu-id="f9edc-142">Görög (el)</span><span class="sxs-lookup"><span data-stu-id="f9edc-142">Greek (el)</span></span> 
* <span data-ttu-id="f9edc-143">Héber (ő)</span><span class="sxs-lookup"><span data-stu-id="f9edc-143">Hebrew (he)</span></span> 
* <span data-ttu-id="f9edc-144">Hindi (nagy)</span><span class="sxs-lookup"><span data-stu-id="f9edc-144">Hindi (hi)</span></span> 
* <span data-ttu-id="f9edc-145">Magyar (hu)</span><span class="sxs-lookup"><span data-stu-id="f9edc-145">Hungarian (hu)</span></span> 
* <span data-ttu-id="f9edc-146">Indonéziai (id)</span><span class="sxs-lookup"><span data-stu-id="f9edc-146">Indonesian (id)</span></span> 
* <span data-ttu-id="f9edc-147">Olasz ()</span><span class="sxs-lookup"><span data-stu-id="f9edc-147">Italian (it)</span></span> 
* <span data-ttu-id="f9edc-148">Japán (ja)</span><span class="sxs-lookup"><span data-stu-id="f9edc-148">Japanese (ja)</span></span> 
* <span data-ttu-id="f9edc-149">Koreai (ko)</span><span class="sxs-lookup"><span data-stu-id="f9edc-149">Korean (ko)</span></span> 
* <span data-ttu-id="f9edc-150">Lett (lv)</span><span class="sxs-lookup"><span data-stu-id="f9edc-150">Latvian (lv)</span></span> 
* <span data-ttu-id="f9edc-151">Litván (lt)</span><span class="sxs-lookup"><span data-stu-id="f9edc-151">Lithuanian (lt)</span></span> 
* <span data-ttu-id="f9edc-152">Maláj (macrolanguage) (ms)</span><span class="sxs-lookup"><span data-stu-id="f9edc-152">Malay (macrolanguage) (ms)</span></span> 
* <span data-ttu-id="f9edc-153">Norvég Bokmål (NetBIOS)</span><span class="sxs-lookup"><span data-stu-id="f9edc-153">Norwegian Bokmål (nb)</span></span> 
* <span data-ttu-id="f9edc-154">Lengyel (pl)</span><span class="sxs-lookup"><span data-stu-id="f9edc-154">Polish (pl)</span></span> 
* <span data-ttu-id="f9edc-155">Portugál (pt)</span><span class="sxs-lookup"><span data-stu-id="f9edc-155">Portuguese (pt)</span></span> 
* <span data-ttu-id="f9edc-156">Román (ro)</span><span class="sxs-lookup"><span data-stu-id="f9edc-156">Romanian (ro)</span></span> 
* <span data-ttu-id="f9edc-157">Orosz (ru)</span><span class="sxs-lookup"><span data-stu-id="f9edc-157">Russian (ru)</span></span> 
* <span data-ttu-id="f9edc-158">Szerb (sr)</span><span class="sxs-lookup"><span data-stu-id="f9edc-158">Serbian (sr)</span></span> 
* <span data-ttu-id="f9edc-159">Szlovák (sk)</span><span class="sxs-lookup"><span data-stu-id="f9edc-159">Slovak (sk)</span></span> 
* <span data-ttu-id="f9edc-160">Szlovén (SA)</span><span class="sxs-lookup"><span data-stu-id="f9edc-160">Slovenian (sl)</span></span> 
* <span data-ttu-id="f9edc-161">Spanyol (es)</span><span class="sxs-lookup"><span data-stu-id="f9edc-161">Spanish (es)</span></span> 
* <span data-ttu-id="f9edc-162">Svéd (sv)</span><span class="sxs-lookup"><span data-stu-id="f9edc-162">Swedish (sv)</span></span> 
* <span data-ttu-id="f9edc-163">Tagalog (tl)</span><span class="sxs-lookup"><span data-stu-id="f9edc-163">Tagalog (tl)</span></span> 
* <span data-ttu-id="f9edc-164">Thai (CS)</span><span class="sxs-lookup"><span data-stu-id="f9edc-164">Thai (th)</span></span> 
* <span data-ttu-id="f9edc-165">Török (m)</span><span class="sxs-lookup"><span data-stu-id="f9edc-165">Turkish (tr)</span></span> 
* <span data-ttu-id="f9edc-166">Ukrán (uk)</span><span class="sxs-lookup"><span data-stu-id="f9edc-166">Ukrainian (uk)</span></span> 
* <span data-ttu-id="f9edc-167">Vietnami (VI.)</span><span class="sxs-lookup"><span data-stu-id="f9edc-167">Vietnamese (vi)</span></span> 

## <a name="campaign"></a><span data-ttu-id="f9edc-168">Kampány</span><span class="sxs-lookup"><span data-stu-id="f9edc-168">Campaign</span></span>
<span data-ttu-id="f9edc-169">Használható hello kampány szakasz tooset hello nevét és kategóriáját a kampány, valamint tooignore hello célközönség szakasz a leküldéses kampány megtervezése, és inkább küldeni a kampány keresztül hello Reach API (és egyes elemek hello alacsony szintű leküldéses API).</span><span class="sxs-lookup"><span data-stu-id="f9edc-169">You can use hello Campaign section tooset hello name and category of your campaign as well as if you plan tooignore hello audience section of a Push campaign and send this campaign via hello Reach API (and some elements with hello low level Push API) instead.</span></span> <span data-ttu-id="f9edc-170">Kategóriák használható egy egyéni értesítési sablon toocontrol az alkalmazásbeli értesítésekben előre meghatározott beállítások alapján.</span><span class="sxs-lookup"><span data-stu-id="f9edc-170">Categories can be used with a custom notification template toocontrol in-app notifications based on predefined settings.</span></span> <span data-ttu-id="f9edc-171">A meglévő "kategóriák listája" hello Reach API használatával kaphat.</span><span class="sxs-lookup"><span data-stu-id="f9edc-171">You can get a list of your existing “Categories” via hello Reach API.</span></span>

> [!WARNING]
> <span data-ttu-id="f9edc-172">Hello "Ignore célközönség leküldéssel fogják megkapni toousers keresztül hello API" beállítás használatakor a Reach-kampány hello "Kampány" szakaszban nem automatikusan küldi hello kampány, szüksége lesz a toosend azt manuálisan hello Reach API.</span><span class="sxs-lookup"><span data-stu-id="f9edc-172">If you use hello "Ignore Audience, push will be sent toousers via hello API" option in hello "Campaign" section of a Reach campaign, hello campaign will NOT automatically send, you will need toosend it manually via hello Reach API.</span></span>

![Reach-Campaign3][22]

### <a name="option-applies-to"></a><span data-ttu-id="f9edc-174">A beállítás a következőkre vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="f9edc-174">Option Applies to:</span></span>
* <span data-ttu-id="f9edc-175">Name: összes</span><span class="sxs-lookup"><span data-stu-id="f9edc-175">Name:    All</span></span>
* <span data-ttu-id="f9edc-176">Kategória: Közlemények, szavazások</span><span class="sxs-lookup"><span data-stu-id="f9edc-176">Category:    Announcements, Polls</span></span>
* <span data-ttu-id="f9edc-177">Hagyja figyelmen kívül a célközönséget, leküldéssel fogják megkapni keresztül hello API toousers: összes</span><span class="sxs-lookup"><span data-stu-id="f9edc-177">Ignore Audience, push will be sent toousers via hello API:    All</span></span>

## <a name="notification"></a><span data-ttu-id="f9edc-178">Értesítés</span><span class="sxs-lookup"><span data-stu-id="f9edc-178">Notification</span></span>
<span data-ttu-id="f9edc-179">Hello értesítési szakasz tooset alapbeállítások használata a leküldéses többek között: hello leküldéses, üdvözlőüzenetére, az alkalmazás lemezkép címe hello, vagy ha mellőzhető.</span><span class="sxs-lookup"><span data-stu-id="f9edc-179">You can use hello Notification section tooset basic settings for your push including: hello title of hello Push, hello message, an in-app image, or if it is dismissible.</span></span> <span data-ttu-id="f9edc-180">Sok értesítési beállítások adott toohello platform, az eszköz.</span><span class="sxs-lookup"><span data-stu-id="f9edc-180">Many notification settings are specific toohello platform of your device.</span></span> <span data-ttu-id="f9edc-181">Kiválaszthatja, hogy a leküldéssel fogják megkapni "alkalmazás" vagy "alkalmazás" verzióról vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="f9edc-181">You can select whether your push will be sent "in app" or "out of app" or both.</span></span> <span data-ttu-id="f9edc-182">(Ne feledje, hogy a felhasználók is "részt" vagy "lemondja" a "kívüli alkalmazás" leküldéses értesítések: hello operációs rendszer szintű az eszközökön, és Azure Mobile Engagement sem fog tudni toooverride kell ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="f9edc-182">(Remember that users can "opt-in" or "opt-out" of "out of app" Pushes at hello Operating System level on their devices, and Azure Mobile Engagement will not be able toooverride this setting.</span></span> <span data-ttu-id="f9edc-183">Továbbá ne feledje, hogy hello Reach API "alkalmazásban" kezeli, és "out alkalmazás" leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="f9edc-183">Also remember that hello Reach API handles "in app" and "out of app" Pushes.</span></span> <span data-ttu-id="f9edc-184">hello leküldéses API lehet "app megszüntetésekor" túl leküldéses értesítések használt toohandle.) Leküldéses értesítések képek vagy HTML-tartalmakat, beleértve az alkalmazásban (Android SDK 2.1.0 vagy újabb, szükség leképezési kategóriák) alkalmazás vagy tooanother helyének kívül kapcsolásához mélyhivatkozással szabhatja testre.</span><span class="sxs-lookup"><span data-stu-id="f9edc-184">hello Push API can be used toohandle "out of app" pushes too.) Pushes can be customized with pictures or HTML content, including deep links for linking outside of your App or tooanother location in your App (Android SDK 2.1.0 or later intent categories required).</span></span> <span data-ttu-id="f9edc-185">Hello ikon vagy iOS-jelvény módosíthatja, és küldje el a szöveget vagy webes tartalmat (html tartalom helyének URL-címe hivatkozás tooanother belül vagy kívül hello app előugró).</span><span class="sxs-lookup"><span data-stu-id="f9edc-185">You can change hello icon or iOS badge, and send either text or web content (a popup with html content, URL link tooanother location either inside or outside of hello app).</span></span> <span data-ttu-id="f9edc-186">Android-eszközök ring ellenőrizze vagy hello leküldéses együtt mozog is.</span><span class="sxs-lookup"><span data-stu-id="f9edc-186">You can also make Android devices ring or vibrate with hello Push.</span></span> <span data-ttu-id="f9edc-187">(Ne feledje, hogy Ön lesz kell hello megfelelő, az Android SDK engedélyek manifest fájl tooring, vagy egy eszköz mozog.) Jelenleg nincs iparági szabványos az Android "összkép" méretek, óta méretek eltérőek minden olyan eszközre, de a szinte bármilyen képernyőméret használható 400 x 100 képek.</span><span class="sxs-lookup"><span data-stu-id="f9edc-187">(Remember that you will need hello correct SDK permissions in your Android manifest file tooring or vibrate a device.) There is currently no industry standard for Android "Big Picture" sizes, since screen sizes are different on every device, but 400x100 pictures work on almost any screen size.</span></span>

### <a name="delivery-types"></a><span data-ttu-id="f9edc-188">Kézbesítési típusok:</span><span class="sxs-lookup"><span data-stu-id="f9edc-188">Delivery Types:</span></span>
* <span data-ttu-id="f9edc-189">Csak az alkalmazáson kívül: hello értesítési kézbesíti a rendszer, amikor hello felhasználói hello alkalmazás nem használja.</span><span class="sxs-lookup"><span data-stu-id="f9edc-189">Out of app only: hello notification will be delivered when hello user does not use hello application.</span></span>
* <span data-ttu-id="f9edc-190">hello kívüli alkalmazás csak értesítés egy tanúsítványra van szükség az Apple vagy a Google (APNS vagy a GCM-tanúsítvánnyal).</span><span class="sxs-lookup"><span data-stu-id="f9edc-190">hello out of app only notification requires a certificate from Apple or Google (APNS or GCM certificate).</span></span>
* <span data-ttu-id="f9edc-191">Alkalmazáson belüli csak: hello értesítés jelenik meg, csak ha hello alkalmazás futása.</span><span class="sxs-lookup"><span data-stu-id="f9edc-191">In-app only: hello notification appears only when hello application is running.</span></span>
* <span data-ttu-id="f9edc-192">hello értesítési hello Capptain kézbesítési rendszer tooreach hello felhasználót használja.</span><span class="sxs-lookup"><span data-stu-id="f9edc-192">hello notification uses hello Capptain delivery system tooreach hello user.</span></span> <span data-ttu-id="f9edc-193">Hello elrendezés/megjelenítését a leküldéses teljes mértékben testreszabható.</span><span class="sxs-lookup"><span data-stu-id="f9edc-193">You can fully customize hello visual layout/display of your push.</span></span>
* <span data-ttu-id="f9edc-194">Bármikor: Ez a beállítás biztosítja, hogy küldjön egy értesítést vagy hello alkalmazás fut-e.</span><span class="sxs-lookup"><span data-stu-id="f9edc-194">Anytime: This option ensures that you send a notification either hello application is running or not.</span></span>

![Reach-Campaign4][23]

### <a name="option-applies-to"></a><span data-ttu-id="f9edc-196">A beállítás a következőkre vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="f9edc-196">Option Applies to:</span></span>
* <span data-ttu-id="f9edc-197">Értesítés: Közlemények, szavazások</span><span class="sxs-lookup"><span data-stu-id="f9edc-197">Notification:     Announcements, Polls</span></span>

## <a name="content"></a><span data-ttu-id="f9edc-198">Tartalom</span><span class="sxs-lookup"><span data-stu-id="f9edc-198">Content</span></span>
<span data-ttu-id="f9edc-199">Hello tartalomszakasz toomodify hello tartalma a közlemények, szavazások, Adatleküldések és Csempék (csak Windows Phone) is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f9edc-199">You can use hello Content section toomodify hello content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="f9edc-200">hello tartalom leküldéses kampányokra érték kampány adott toohello típusát.</span><span class="sxs-lookup"><span data-stu-id="f9edc-200">hello Content setting of Push campaigns is specific toohello type of campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="f9edc-201">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f9edc-201">See also</span></span>
* <span data-ttu-id="f9edc-202">[Felhasználói felületének dokumentációja – Reach - tartalom leküldéses][Link 29]</span><span class="sxs-lookup"><span data-stu-id="f9edc-202">[UI Documentation - Reach - Push Content][Link 29]</span></span>

![Reach-Campaign5][24]

## <a name="audience"></a><span data-ttu-id="f9edc-204">Célközönség</span><span class="sxs-lookup"><span data-stu-id="f9edc-204">Audience</span></span>
<span data-ttu-id="f9edc-205">Hello célközönség szakasz toodefine elemek toolimit szabványos listáját is használhat, a kampány vagy -korlátok, a kampány testreszabott feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="f9edc-205">You can use hello Audience section toodefine a standard list of items toolimit your campaign or limits your campaign based on customized criteria.</span></span> <span data-ttu-id="f9edc-206">hello tooLimit beállítások szabványos készletét a célközönséget lehetővé teszi toopush tooeither új vagy régi felhasználók vagy csak a natív leküldéses felhasználók.</span><span class="sxs-lookup"><span data-stu-id="f9edc-206">hello standard set of options tooLimit your Audience allows you toopush tooeither new or old users or native push users only.</span></span> <span data-ttu-id="f9edc-207">Hello leküldéses kapják kvóta toolimit hello számos állíthat be.</span><span class="sxs-lookup"><span data-stu-id="f9edc-207">You can also set a quota toolimit hello number of users who receive hello push.</span></span> <span data-ttu-id="f9edc-208">Manuálisan módosíthatja a kampány Mitől szűrt tooinclude hello kifejezése egy vagy több feltétel tootarget felhasználók.</span><span class="sxs-lookup"><span data-stu-id="f9edc-208">You can manually Edit hello expression for how your campaign is filtered tooinclude one or more criterion tootarget users.</span></span> <span data-ttu-id="f9edc-209">Írja be a célközönség kifejezés manuálisan.</span><span class="sxs-lookup"><span data-stu-id="f9edc-209">You can manually type an audience expression.</span></span> <span data-ttu-id="f9edc-210">Ilyen kifejezésben egyértelműen meg kell határoznia hello kapcsolat feltételek között.</span><span class="sxs-lookup"><span data-stu-id="f9edc-210">Such an expression must explicitly define hello relation between criteria.</span></span> <span data-ttu-id="f9edc-211">Egy olyan feltételt, amely nagybetűvel kezdődhet, és nem tartalmazhat szóközt azonosítót írja le.</span><span class="sxs-lookup"><span data-stu-id="f9edc-211">A criterion is described by an identifier that must start with a capital letter and cannot contain spaces.</span></span> <span data-ttu-id="f9edc-212">hello kapcsolat hello feltételek között annak leírását használja "és", "vagy", "nem" operátor, valamint a "(",")".</span><span class="sxs-lookup"><span data-stu-id="f9edc-212">hello relation between hello criteria can be described using 'and', 'or', 'not' operators as well as '(', ')'.</span></span> <span data-ttu-id="f9edc-213">Példa: "Criterion1 vagy (Criterion1 és nem Criterion2)".</span><span class="sxs-lookup"><span data-stu-id="f9edc-213">Example: "Criterion1 or (Criterion1 and not Criterion2)".</span></span>

> [!NOTE]
> <span data-ttu-id="f9edc-214">A kampányok szereplő nagy célközönség, hello kiszolgálóoldali célcsoportkezelés vizsgálatot lassú lehet, különösen akkor, ha úgy próbálja toostart több kampányokat hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="f9edc-214">With a large audience included in campaigns, hello server side targeting scan can be slow, especially if you attempt toostart multiple campaigns at hello same time.</span></span>

* <span data-ttu-id="f9edc-215">Ha lehetséges csak start egy kampány egyszerre.</span><span class="sxs-lookup"><span data-stu-id="f9edc-215">If possible, only start one campaign at a time.</span></span>
* <span data-ttu-id="f9edc-216">A hello legfeljebb csak kezdő négy kampányok egyszerre.</span><span class="sxs-lookup"><span data-stu-id="f9edc-216">At hello most, only start four campaigns at a time.</span></span>
* <span data-ttu-id="f9edc-217">Csak tooyour az aktív felhasználók leküldéses (jelölőnégyzet "megszólítása csak olyan felhasználók, akik elérhetők natív leküldéssel" és "Csak az aktív felhasználók megszólítása"), hogy csak a felhasználók, akik továbbra is hello az alkalmazás telepítve van, és használja a beolvasott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="f9edc-217">Push only tooyour active users (checkbox "Engage only users who can be reached using Native Push" and "Engage only active users") so that only your users who still have hello app installed and use it will need toobe scanned.</span></span>
  <span data-ttu-id="f9edc-218">A célközönség van definiálva, miután hello használhatja ki, hány felhasználó fog kapni a leküldéses gomb toofind szimulálása.</span><span class="sxs-lookup"><span data-stu-id="f9edc-218">Once your audience is defined, you can use hello simulate button toofind out how many users will receive this Push.</span></span> <span data-ttu-id="f9edc-219">Ez fogja számítási ismert felhasználók által a célközönség (egy véletlenszerű felhasználói minta alapján becsült érték) potenciálisan megcélzott hello száma.</span><span class="sxs-lookup"><span data-stu-id="f9edc-219">This will compute hello number of known users potentially targeted by this audience (this is an estimate based on a random sample of users).</span></span> <span data-ttu-id="f9edc-220">Vegye figyelembe, hogy azok a felhasználók, akik eltávolították hello alkalmazást is részei a célközönségnek, de nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="f9edc-220">Be aware that users who have uninstalled hello application are also part of this audience, but cannot be reached.</span></span>

### <a name="see-also"></a><span data-ttu-id="f9edc-221">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f9edc-221">See also</span></span>
* <span data-ttu-id="f9edc-222">[Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]</span><span class="sxs-lookup"><span data-stu-id="f9edc-222">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

![Reach-Campaign6][25]

### <a name="edit-expression"></a><span data-ttu-id="f9edc-224">Szerkesztés</span><span class="sxs-lookup"><span data-stu-id="f9edc-224">Edit expression</span></span>
![Reach-Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a><span data-ttu-id="f9edc-226">A korlát a célközönség-beállítás:</span><span class="sxs-lookup"><span data-stu-id="f9edc-226">Limit your audience option applies to:</span></span>
* <span data-ttu-id="f9edc-227">Csak egy adott felhasználók alcsoportjának megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-227">Engage only a subset of users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="f9edc-228">Csak a régi felhasználók megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-228">Engage only old users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="f9edc-229">Csak az új felhasználók megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-229">Engage only new users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="f9edc-230">Csak a tétlen felhasználók megszólítása: közlemények, szavazások, Csempéket</span><span class="sxs-lookup"><span data-stu-id="f9edc-230">Engage only idle users:    Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="f9edc-231">Csak az aktív felhasználók megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)</span><span class="sxs-lookup"><span data-stu-id="f9edc-231">Engage only active users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="f9edc-232">Csak olyan felhasználók, akik elérhetők natív leküldéssel megszólítása: közlemények, szavazások</span><span class="sxs-lookup"><span data-stu-id="f9edc-232">Engage only users who can be reached using Native Push:     Announcements, Polls</span></span>

## <a name="time-frame"></a><span data-ttu-id="f9edc-233">Időkereten belül</span><span class="sxs-lookup"><span data-stu-id="f9edc-233">Time Frame</span></span>
<span data-ttu-id="f9edc-234">Hello időkereten belül szakasz tooset hello leküldéssel fogják megkapni, vagy hagyhatja hello időkereten belül üres toostart hello kampány azonnal használható.</span><span class="sxs-lookup"><span data-stu-id="f9edc-234">You can use hello Time Frame section tooset when hello push will be sent or you can leave hello time frame blank toostart hello campaign immediately.</span></span> <span data-ttu-id="f9edc-235">Ne feledje, hogy hello végfelhasználók időzóna használatával lehet, hogy Kezdőnap hello kampány egy korábbi, mint a végfelhasználók Ázsiában kapjon, és leküldéses értesítések, amíg az összes időzóna hello időkereten belül hello world egyezés állítsa be a kampány kis kötegeinek küldése.</span><span class="sxs-lookup"><span data-stu-id="f9edc-235">Remember that using hello end-users' time zone may start hello campaign a day earlier than you expect for your end-users in Asia and send small batches of pushes at a time until all time zones in hello world match hello time frame set for your campaign.</span></span> <span data-ttu-id="f9edc-236">Hello végfelhasználók számára időzóna használatával is lelassíthatják a kampányok mert toorequest hello idő hello telefonról hello leküldéses elindítása előtt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f9edc-236">Using hello end users' time zone can also cause delays in campaigns since it has toorequest hello time from hello phone before starting hello push.</span></span>

> [!NOTE]
> <span data-ttu-id="f9edc-237">Nélkül záródátumot gyorsítótárazásával kampányokra leküldéses értesítések helyileg és továbbra is megjelenítésükhöz követően manuálisan teljes kampányok.</span><span class="sxs-lookup"><span data-stu-id="f9edc-237">Campaigns without an end date can cache pushes locally and still display them after you manually complete campaigns.</span></span> <span data-ttu-id="f9edc-238">tooavoid ezt a viselkedést, a specifikus kampányok befejezési idő.</span><span class="sxs-lookup"><span data-stu-id="f9edc-238">tooavoid this behavior, specific an end time for campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="f9edc-239">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f9edc-239">See also</span></span>
* <span data-ttu-id="f9edc-240">[A reach - hogyan Tos – ütemezése][Link 3]</span><span class="sxs-lookup"><span data-stu-id="f9edc-240">[Reach - How Tos – Scheduling][Link 3]</span></span> 

![Reach-Campaign8][27]

### <a name="settings-apply-to"></a><span data-ttu-id="f9edc-242">Beállítások érvényesek:</span><span class="sxs-lookup"><span data-stu-id="f9edc-242">Settings Apply to:</span></span>
* <span data-ttu-id="f9edc-243">Időkereten belül: közlemények, szavazások, Csempéket</span><span class="sxs-lookup"><span data-stu-id="f9edc-243">Time frame:     Announcements, Polls, Tiles</span></span>

## <a name="test"></a><span data-ttu-id="f9edc-244">Tesztelés</span><span class="sxs-lookup"><span data-stu-id="f9edc-244">Test</span></span>
<span data-ttu-id="f9edc-245">Használhat hello teszt szakasz toosend leküldéses tooyour saját vizsgálati eszköz hello kampány mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9edc-245">You can use hello Test section toosend this push tooyour own test device before saving hello campaign.</span></span> <span data-ttu-id="f9edc-246">Ha konfigurálta a kampány bármilyen egyéni nyelveit, tesztelheti hello leküldéses mindkét nyelven.</span><span class="sxs-lookup"><span data-stu-id="f9edc-246">If you have configured any custom languages for this campaign, you can test hello push in each language.</span></span> <span data-ttu-id="f9edc-247">Egy vizsgálati eszköz "Saját fiók" beállítása.</span><span class="sxs-lookup"><span data-stu-id="f9edc-247">You can setup a test device from “My Account”.</span></span>

> [!NOTE]
> <span data-ttu-id="f9edc-248">Nem a rendszer adatokat naplózza hello gomb használatakor kiszolgálóoldali túl "teszt" leküldéses értesítések, adatokat csak naplózza a valódi leküldéses kampányokra.</span><span class="sxs-lookup"><span data-stu-id="f9edc-248">No server side data is logged when you use hello button too"test" pushes, data is only logged for real push campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="f9edc-249">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f9edc-249">See also</span></span>
* <span data-ttu-id="f9edc-250">[Felhasználói felület dokumentáció - fiókomat][Link 14]</span><span class="sxs-lookup"><span data-stu-id="f9edc-250">[UI Documentation - My Account][Link 14]</span></span>

![Reach-Campaign9][28]

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

