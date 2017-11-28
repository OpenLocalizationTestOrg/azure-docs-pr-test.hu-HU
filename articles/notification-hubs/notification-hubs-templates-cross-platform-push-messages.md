---
title: aaaTemplates
description: "Ez a témakör ismerteti a sablonok az Azure notification hubs használatával."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 0149f0c7473e5a4b952905bc8217582b58db2a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="templates"></a><span data-ttu-id="5bb60-103">Sablonok</span><span class="sxs-lookup"><span data-stu-id="5bb60-103">Templates</span></span>
## <a name="overview"></a><span data-ttu-id="5bb60-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5bb60-104">Overview</span></span>
<span data-ttu-id="5bb60-105">Sablonok egy ügyfél alkalmazás toospecify hello formátummal tooreceive szeretnének hello értesítések engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5bb60-105">Templates enable a client application toospecify hello exact format of hello notifications it wants tooreceive.</span></span> <span data-ttu-id="5bb60-106">Sablonokkal, az alkalmazások számos különböző előnnyel jár, beleértve a következőket hello is megvalósítható:</span><span class="sxs-lookup"><span data-stu-id="5bb60-106">Using templates, an app can realize several different benefits, including hello following :</span></span>

* <span data-ttu-id="5bb60-107">A platform-független háttér</span><span class="sxs-lookup"><span data-stu-id="5bb60-107">A platform-agnostic backend</span></span>
* <span data-ttu-id="5bb60-108">Személyre szabott értesítések</span><span class="sxs-lookup"><span data-stu-id="5bb60-108">Personalized notifications</span></span>
* <span data-ttu-id="5bb60-109">Ügyfélverzió függetlenség</span><span class="sxs-lookup"><span data-stu-id="5bb60-109">Client-version independence</span></span>
* <span data-ttu-id="5bb60-110">Egyszerű honosítása</span><span class="sxs-lookup"><span data-stu-id="5bb60-110">Easy localization</span></span>

<span data-ttu-id="5bb60-111">Ez a szakasz két részletes példákat hogyan toouse sablonok toosend platform-független értesítések célcsoport-kezelési az eszközök különböző platformokon és toopersonalize szórási tooeach eszközt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5bb60-111">This section provides two in-depth examples of how toouse templates toosend platform-agnostic notifications targeting all your devices across platforms, and toopersonalize broadcast notification tooeach device.</span></span>

## <a name="using-templates-cross-platform"></a><span data-ttu-id="5bb60-112">Sablonok platformfüggetlen használatával</span><span class="sxs-lookup"><span data-stu-id="5bb60-112">Using templates cross-platform</span></span>
<span data-ttu-id="5bb60-113">hello normál módon toosend leküldéses értesítések toosend minden értesítés esetén, amely toobe küldi el, egy adott hasznos tooplatform értesítési szolgáltatások, (WNS, APNS).</span><span class="sxs-lookup"><span data-stu-id="5bb60-113">hello standard way toosend push notifications is toosend, for each notification that is toobe sent, a specific payload tooplatform notification services (WNS, APNS).</span></span> <span data-ttu-id="5bb60-114">Például egy riasztási tooAPNS toosend hello payload egy Json-objektum, a következő képernyő hello:</span><span class="sxs-lookup"><span data-stu-id="5bb60-114">For example, toosend an alert tooAPNS, hello payload is a Json object of hello following form:</span></span>

    {"aps": {"alert" : "Hello!" }}

<span data-ttu-id="5bb60-115">a Windows Áruházbeli alkalmazások hasonló bejelentési üzenet toosend, hello XML-adattartalmat értéke a következő:</span><span class="sxs-lookup"><span data-stu-id="5bb60-115">toosend a similar toast message on a Windows Store application, hello XML payload is as follows:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

<span data-ttu-id="5bb60-116">Hasonló hasznos adat található MPNS (Windows Phone) és a GCM (Android) platformokhoz hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="5bb60-116">You can create similar payloads for MPNS (Windows Phone) and GCM (Android) platforms.</span></span>

<span data-ttu-id="5bb60-117">Ez a követelmény kényszeríti hello app háttér tooproduce különböző hasznos adat található minden egyes platformhoz, és hatékonyan miatt hello háttér felelős hello bemutató réteg hello alkalmazás részét.</span><span class="sxs-lookup"><span data-stu-id="5bb60-117">This requirement forces hello app backend tooproduce different payloads for each platform, and effectively makes hello backend responsible for part of hello presentation layer of hello app.</span></span> <span data-ttu-id="5bb60-118">Egyes okok közé tartoznak a honosítás és grafikus elrendezések (különösen a Windows Áruházbeli alkalmazások, amelyek tartalmazzák a csempék különféle típusú értesítések).</span><span class="sxs-lookup"><span data-stu-id="5bb60-118">Some concerns include localization and graphical layouts (especially for Windows Store apps that include notifications for various types of tiles).</span></span>

<span data-ttu-id="5bb60-119">hello Notification Hubs sablon szolgáltatás lehetővé teszi, hogy egy ügyfél app toocreate különleges regisztrációk, sablon regisztrációk, amelyek közé tartozik, továbbá toohello beállítás címkék, egy sablon neve.</span><span class="sxs-lookup"><span data-stu-id="5bb60-119">hello Notification Hubs template feature enables a client app toocreate special registrations, called template registrations, which include, in addition toohello set of tags, a template.</span></span> <span data-ttu-id="5bb60-120">hello Notification Hubs sablon szolgáltatás egy alkalmazás tooassociate ügyféleszközök sablonok lehetővé teszi, hogy telepítések (ajánlott) vagy a regisztráció dolgozik.</span><span class="sxs-lookup"><span data-stu-id="5bb60-120">hello Notification Hubs template feature enables a client app tooassociate devices with templates whether you are working with Installations (preferred) or Registrations.</span></span> <span data-ttu-id="5bb60-121">Megadott tartalom példák megelőző hello, hello csak platformfüggetlen információk is hello tényleges riasztási üzenete (Hello!).</span><span class="sxs-lookup"><span data-stu-id="5bb60-121">Given hello preceding payload examples, hello only platform-independent information is hello actual alert message (Hello!).</span></span> <span data-ttu-id="5bb60-122">A sablon olyan hogyan tooformat a platformfüggetlen üzenet az adott ügyfél-alkalmazás regisztrációja hello az értesítési központ hello vonatkozó utasításokat.</span><span class="sxs-lookup"><span data-stu-id="5bb60-122">A template is a set of instructions for hello Notification Hub on how tooformat a platform-independent message for hello registration of that specific client app.</span></span> <span data-ttu-id="5bb60-123">A fenti példa hello, üdvözlőüzenetére platform független ugyanazon a tulajdonságon: **message = Hello!**.</span><span class="sxs-lookup"><span data-stu-id="5bb60-123">In hello preceding example, hello platform independent message is a single property: **message = Hello!**.</span></span>

<span data-ttu-id="5bb60-124">a következő kép hello fent folyamat hello mutatja be:</span><span class="sxs-lookup"><span data-stu-id="5bb60-124">hello following picture illustrates hello above process:</span></span>

![](./media/notification-hubs-templates/notification-hubs-hello.png)

<span data-ttu-id="5bb60-125">hello sablont hello iOS app ügyfélregisztráció a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="5bb60-125">hello template for hello iOS client app registration is as follows:</span></span>

    {"aps": {"alert": "$(message)"}}

<span data-ttu-id="5bb60-126">hello megfelelő sablon hello Windows Store ügyfél alkalmazás van:</span><span class="sxs-lookup"><span data-stu-id="5bb60-126">hello corresponding template for hello Windows Store client app is:</span></span>

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

<span data-ttu-id="5bb60-127">Figyelje meg, hogy a tényleges üzenet hello hello kifejezés $(üzenet) van helyére.</span><span class="sxs-lookup"><span data-stu-id="5bb60-127">Notice that hello actual message is substituted for hello expression $(message).</span></span> <span data-ttu-id="5bb60-128">Ebben a kifejezésben hello értesítési központ arra utasítja, amikor adott regisztrációs, toobuild hello közös érték a kapcsolókat és azt követő üzenetet küld egy üzenet toothis.</span><span class="sxs-lookup"><span data-stu-id="5bb60-128">This expression instructs hello Notification Hub, whenever it sends a message toothis particular registration, toobuild a message that follows it and switches in hello common value.</span></span>

<span data-ttu-id="5bb60-129">Ha telepítési modell dolgozik, hello telepítési "sablon" billentyű rendelkezik több sablonok JSON.</span><span class="sxs-lookup"><span data-stu-id="5bb60-129">If you are working with Installation model, hello installation “templates” key holds a JSON of multiple templates.</span></span> <span data-ttu-id="5bb60-130">Ha alkalmazásregisztrációs modell dolgozik, hello ügyfélalkalmazás hozhat létre több regisztrációk rendelés toouse több sablon; például egy sablon figyelmeztetések és a csempe frissítések sablonját.</span><span class="sxs-lookup"><span data-stu-id="5bb60-130">If you are working with Registration model, hello client application can create multiple registrations in order toouse multiple templates; for example, a template for alert messages and a template for tile updates.</span></span> <span data-ttu-id="5bb60-131">Ügyfélalkalmazások adjunk natív regisztrációk (regisztráció nélküli sablonnal) és a sablon regisztráció.</span><span class="sxs-lookup"><span data-stu-id="5bb60-131">Client applications can also mix native registrations (registrations with no template) and template registrations.</span></span>

<span data-ttu-id="5bb60-132">Értesítési központ hello minden sablon egy értesítést küld, figyelembe véve, hogy tartoznak toohello nélkül ügyfél ugyanazt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5bb60-132">hello Notification Hub sends one notification for each template without considering whether they belong toohello same client app.</span></span> <span data-ttu-id="5bb60-133">Ez a viselkedés használt tootranslate platformfüggetlen értesítések több értesítések be lehet.</span><span class="sxs-lookup"><span data-stu-id="5bb60-133">This behavior can be used tootranslate platform-independent notifications into more notifications.</span></span> <span data-ttu-id="5bb60-134">Például hello azonos platform független üzenet toohello értesítési központ zökkenőmentesen fordítható egy bejelentési értesítés és egy csempe frissítése erről tájékoztatva hello háttér toobe nélkül.</span><span class="sxs-lookup"><span data-stu-id="5bb60-134">For example, hello same platform independent message toohello Notification Hub can be seamlessly translated in a toast alert and a tile update, without requiring hello backend toobe aware of it.</span></span> <span data-ttu-id="5bb60-135">Vegye figyelembe, hogy néhány platformon (például iOS) összeomlás több értesítések toohello ugyanarra az eszközre, ha elküldi őket egy rövid időn belül.</span><span class="sxs-lookup"><span data-stu-id="5bb60-135">Note that some platforms (for example, iOS) might collapse multiple notifications toohello same device if they are sent in a short period of time.</span></span>

## <a name="using-templates-for-personalization"></a><span data-ttu-id="5bb60-136">Sablonok használata személyre szabása</span><span class="sxs-lookup"><span data-stu-id="5bb60-136">Using templates for personalization</span></span>
<span data-ttu-id="5bb60-137">Egy másik előnye toousing sablonok hello képességét toouse az értesítések a Notification Hubs tooperform regisztrációs személyre szabása.</span><span class="sxs-lookup"><span data-stu-id="5bb60-137">Another advantage toousing templates is hello ability toouse Notification Hubs tooperform per-registration personalization of notifications.</span></span> <span data-ttu-id="5bb60-138">Vegyük példaként a időjárás-alkalmazást, amely egy csempe, amely hello időjárási feltételek megjeleníti az adott helyen.</span><span class="sxs-lookup"><span data-stu-id="5bb60-138">For example, consider a weather app that displays a tile with hello weather conditions at a specific location.</span></span> <span data-ttu-id="5bb60-139">A felhasználó Celsius vagy Fahrenheit fok, és egyetlen vagy öt nap előrejelzés választhat.</span><span class="sxs-lookup"><span data-stu-id="5bb60-139">A user can choose between Celsius or Fahrenheit degrees, and a single or five-day forecast.</span></span> <span data-ttu-id="5bb60-140">Sablonokkal, minden ügyfél az alkalmazástelepítés regisztrálhatnak hello formátuma (1 napos Celsius, 1 napos Fahrenheit, 5 nap Celsius, 5 nap Fahrenheit), és minden hello adatait tartalmazó egyetlen üzenet szükséges toofill azokat küldeni hello háttér sablonok (például egy öt nap az előrejelzések Celsius és Fahrenheit fokban megadva).</span><span class="sxs-lookup"><span data-stu-id="5bb60-140">Using templates, each client app installation can register for hello format required (1-day Celsius, 1-day Fahrenheit, 5-days Celsius, 5-days Fahrenheit), and have hello backend send a single message that contains all hello information required toofill those templates (for example, a five-day forecast with Celsius and Fahrenheit degrees).</span></span>

<span data-ttu-id="5bb60-141">hello sablon hello egy egynapos Celsius az előrejelzések hőmérsékletek értéke a következő:</span><span class="sxs-lookup"><span data-stu-id="5bb60-141">hello template for hello one-day forecast with Celsius temperatures is as follows:</span></span>

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

<span data-ttu-id="5bb60-142">hello üzenet toohello értesítési központ tartalmaz minden hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="5bb60-142">hello message sent toohello Notification Hub contains all hello following properties:</span></span>

<table border="1">

<tr><td><span data-ttu-id="5bb60-143">day1_image</span><span class="sxs-lookup"><span data-stu-id="5bb60-143">day1_image</span></span></td><td><span data-ttu-id="5bb60-144">day2_image</span><span class="sxs-lookup"><span data-stu-id="5bb60-144">day2_image</span></span></td><td><span data-ttu-id="5bb60-145">day3_image</span><span class="sxs-lookup"><span data-stu-id="5bb60-145">day3_image</span></span></td><td><span data-ttu-id="5bb60-146">day4_image</span><span class="sxs-lookup"><span data-stu-id="5bb60-146">day4_image</span></span></td><td><span data-ttu-id="5bb60-147">day5_image</span><span class="sxs-lookup"><span data-stu-id="5bb60-147">day5_image</span></span></td></tr>

<tr><td><span data-ttu-id="5bb60-148">day1_tempC</span><span class="sxs-lookup"><span data-stu-id="5bb60-148">day1_tempC</span></span></td><td><span data-ttu-id="5bb60-149">day2_tempC</span><span class="sxs-lookup"><span data-stu-id="5bb60-149">day2_tempC</span></span></td><td><span data-ttu-id="5bb60-150">day3_tempC</span><span class="sxs-lookup"><span data-stu-id="5bb60-150">day3_tempC</span></span></td><td><span data-ttu-id="5bb60-151">day4_tempC</span><span class="sxs-lookup"><span data-stu-id="5bb60-151">day4_tempC</span></span></td><td><span data-ttu-id="5bb60-152">day5_tempC</span><span class="sxs-lookup"><span data-stu-id="5bb60-152">day5_tempC</span></span></td></tr>

<tr><td><span data-ttu-id="5bb60-153">day1_tempF</span><span class="sxs-lookup"><span data-stu-id="5bb60-153">day1_tempF</span></span></td><td><span data-ttu-id="5bb60-154">day2_tempF</span><span class="sxs-lookup"><span data-stu-id="5bb60-154">day2_tempF</span></span></td><td><span data-ttu-id="5bb60-155">day3_tempF</span><span class="sxs-lookup"><span data-stu-id="5bb60-155">day3_tempF</span></span></td><td><span data-ttu-id="5bb60-156">day4_tempF</span><span class="sxs-lookup"><span data-stu-id="5bb60-156">day4_tempF</span></span></td><td><span data-ttu-id="5bb60-157">day5_tempF</span><span class="sxs-lookup"><span data-stu-id="5bb60-157">day5_tempF</span></span></td></tr>
</table><br/>

<span data-ttu-id="5bb60-158">Ez a minta segítségével hello háttér csak egyetlen üzenetet küld anélkül, hogy az adott beállításoknál toostore hello az alkalmazásaik felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="5bb60-158">By using this pattern, hello backend only sends a single message without having toostore specific personalization options for hello app users.</span></span> <span data-ttu-id="5bb60-159">a következő kép hello ebben a forgatókönyvben mutatja be:</span><span class="sxs-lookup"><span data-stu-id="5bb60-159">hello following picture illustrates this scenario:</span></span>

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-tooregister-templates"></a><span data-ttu-id="5bb60-160">Hogyan tooregister sablonok</span><span class="sxs-lookup"><span data-stu-id="5bb60-160">How tooregister templates</span></span>
<span data-ttu-id="5bb60-161">tooregister sablonoknak hello telepítési modell (ajánlott) vagy hello alkalmazásregisztrációs modell, lásd: [regisztrációs felügyeleti](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="5bb60-161">tooregister with templates using hello Installation model (preferred), or hello Registration model, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

## <a name="template-expression-language"></a><span data-ttu-id="5bb60-162">Sablon kifejezés nyelve</span><span class="sxs-lookup"><span data-stu-id="5bb60-162">Template expression language</span></span>
<span data-ttu-id="5bb60-163">Sablonok olyan, korlátozott tooXML vagy JSON-dokumentum formázását.</span><span class="sxs-lookup"><span data-stu-id="5bb60-163">Templates are limited tooXML or JSON document formats.</span></span> <span data-ttu-id="5bb60-164">Emellett csak elhelyezheti kifejezések adott helyek; például csomópont attribútumok vagy XML-értékek JSON karakterlánc-tulajdonságok értékeit.</span><span class="sxs-lookup"><span data-stu-id="5bb60-164">Also, you can only place expressions in particular places; for example, node attributes or values for XML, string property values for JSON.</span></span>

<span data-ttu-id="5bb60-165">a következő táblázat azt mutatja be hello nyelvi sablonok engedélyezett hello:</span><span class="sxs-lookup"><span data-stu-id="5bb60-165">hello following table shows hello language allowed in templates:</span></span>

| <span data-ttu-id="5bb60-166">kifejezés</span><span class="sxs-lookup"><span data-stu-id="5bb60-166">Expression</span></span> | <span data-ttu-id="5bb60-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="5bb60-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5bb60-168">$(prop)</span><span class="sxs-lookup"><span data-stu-id="5bb60-168">$(prop)</span></span> |<span data-ttu-id="5bb60-169">Hivatkozási tooan esemény tulajdonság hello megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="5bb60-169">Reference tooan event property with hello given name.</span></span> <span data-ttu-id="5bb60-170">A tulajdonságnevek nem nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="5bb60-170">Property names are not case-sensitive.</span></span> <span data-ttu-id="5bb60-171">Ebben a kifejezésben hello tulajdonság szöveges értéket vagy üres karakterlánc oldja fel, ha hello tulajdonság nem található.</span><span class="sxs-lookup"><span data-stu-id="5bb60-171">This expression resolves into hello property’s text value or into an empty string if hello property is not present.</span></span> |
| <span data-ttu-id="5bb60-172">$(prop, n)</span><span class="sxs-lookup"><span data-stu-id="5bb60-172">$(prop, n)</span></span> |<span data-ttu-id="5bb60-173">Mivel fent, de hello szöveg explicit módon levágva n karakterek, például $(cím, 20) levágja hello cím tulajdonság hello tartalmát: 20 karakter.</span><span class="sxs-lookup"><span data-stu-id="5bb60-173">As above, but hello text is explicitly clipped at n characters, for example $(title, 20) clips hello contents of hello title property at 20 characters.</span></span> |
| <span data-ttu-id="5bb60-174">. (prop, n)</span><span class="sxs-lookup"><span data-stu-id="5bb60-174">.(prop, n)</span></span> |<span data-ttu-id="5bb60-175">Fent, de hello szöveget a három pont van utótaggal, amelyhez hozzá van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="5bb60-175">As above, but hello text is suffixed with three dots as it is clipped.</span></span> <span data-ttu-id="5bb60-176">hello teljes mérete hello levágva karakterlánc, és n karakternél nem hosszabb hello utótag.</span><span class="sxs-lookup"><span data-stu-id="5bb60-176">hello total size of hello clipped string and hello suffix does not exceed n characters.</span></span> <span data-ttu-id="5bb60-177">. (cím, 20) és a "Ez az hello címsora" eredményeinek tulajdonsága bemeneti **Ez a cím hello...**</span><span class="sxs-lookup"><span data-stu-id="5bb60-177">.(title, 20) with an input property of “This is hello title line” results in **This is hello title...**</span></span> |
| <span data-ttu-id="5bb60-178">%(prop)</span><span class="sxs-lookup"><span data-stu-id="5bb60-178">%(prop)</span></span> |<span data-ttu-id="5bb60-179">Hasonló too$(name) azzal a különbséggel, hogy hello kimeneti URI-kódolású.</span><span class="sxs-lookup"><span data-stu-id="5bb60-179">Similar too$(name) except that hello output is URI-encoded.</span></span> |
| <span data-ttu-id="5bb60-180">#(prop)</span><span class="sxs-lookup"><span data-stu-id="5bb60-180">#(prop)</span></span> |<span data-ttu-id="5bb60-181">A JSON-sablonokban használt (például az iOS és Android sablonok).</span><span class="sxs-lookup"><span data-stu-id="5bb60-181">Used in JSON templates (for example, for iOS and Android templates).</span></span><br><br><span data-ttu-id="5bb60-182">A funkció akkor működik pontosan hello ugyanaz, mint a korábban megadott, kivéve ha szerepel a JSON-sablonokat (például Apple-sablonok) $(prop).</span><span class="sxs-lookup"><span data-stu-id="5bb60-182">This function works exactly hello same as $(prop) previously specified, except when used in JSON templates (for example, Apple templates).</span></span> <span data-ttu-id="5bb60-183">Ebben az esetben, ha ez a funkció nem körülvett "{","}" (például "myJsonProperty':"#(név)"), és a Javascript formátumban, például RegExp szolgáltatást tooa szám értékel: (0 &#124; (&#91; 1 – 9 &#93; &#91; 0-9 & #93 ;*))(\. &#91; 0-9 &#93; +)? ((az e &#124; E) (+ &#124;-)? &#91; 0-9 &#93; +)?, majd hello kimeneti JSON egy számot.</span><span class="sxs-lookup"><span data-stu-id="5bb60-183">In this case, if this function is not surrounded by “{‘,’}” (for example, ‘myJsonProperty’ : ‘#(name)’), and it evaluates tooa number in Javascript format, for example, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*))(\.&#91;0-9&#93;+)?((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, then hello output JSON is a number.</span></span><br><br><span data-ttu-id="5bb60-184">Például "jelvény:"#(név)"válik"jelvények": 40 (és nem"40").</span><span class="sxs-lookup"><span data-stu-id="5bb60-184">For example, ‘badge : ‘#(name)’ becomes ‘badge’ : 40 (and not ‘40‘).</span></span> |
| <span data-ttu-id="5bb60-185">"text" vagy "text"</span><span class="sxs-lookup"><span data-stu-id="5bb60-185">‘text’ or “text”</span></span> |<span data-ttu-id="5bb60-186">A szövegkonstans.</span><span class="sxs-lookup"><span data-stu-id="5bb60-186">A literal.</span></span> <span data-ttu-id="5bb60-187">Literálok egyetlen vagy dupla idézőjelek közé tetszőleges szöveget tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="5bb60-187">Literals contain arbitrary text enclosed in single or double quotes.</span></span> |
| <span data-ttu-id="5bb60-188">Kif1 + Kif2</span><span class="sxs-lookup"><span data-stu-id="5bb60-188">expr1 + expr2</span></span> |<span data-ttu-id="5bb60-189">hello összefűzési operátor való csatlakozás egyetlen karakterlánccá egyesít két kifejezésen.</span><span class="sxs-lookup"><span data-stu-id="5bb60-189">hello concatenation operator joining two expressions into a single string.</span></span> |

<span data-ttu-id="5bb60-190">hello kifejezések űrlapok megelőző hello bármelyike lehet.</span><span class="sxs-lookup"><span data-stu-id="5bb60-190">hello expressions can be any of hello preceding forms.</span></span>

<span data-ttu-id="5bb60-191">Kapott használatakor hello teljes kifejezést kell körülvett az {}.</span><span class="sxs-lookup"><span data-stu-id="5bb60-191">When using concatenation, hello entire expression must be surrounded with {}.</span></span> <span data-ttu-id="5bb60-192">Például {$(prop) + '-' + $(prop2)}.</span><span class="sxs-lookup"><span data-stu-id="5bb60-192">For example, {$(prop) + ‘ - ’ + $(prop2)}.</span></span> |

<span data-ttu-id="5bb60-193">Például hello most nem egy érvényes XML-sablont:</span><span class="sxs-lookup"><span data-stu-id="5bb60-193">For example, hello following is not a valid XML template:</span></span>

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


<span data-ttu-id="5bb60-194">Amint azt fent összefűzése használatakor kifejezések kell csomagolni, kapcsos zárójelek közé.</span><span class="sxs-lookup"><span data-stu-id="5bb60-194">As explained above, when using concatenation, expressions must be wrapped in curly brackets.</span></span> <span data-ttu-id="5bb60-195">Példa:</span><span class="sxs-lookup"><span data-stu-id="5bb60-195">For example:</span></span>

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

