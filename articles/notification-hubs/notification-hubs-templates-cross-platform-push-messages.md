---
title: Sablonok
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
ms.openlocfilehash: 1ca24a4bf08ecdbe1c1e47a931613144309a04a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="templates"></a><span data-ttu-id="0102a-103">Sablonok</span><span class="sxs-lookup"><span data-stu-id="0102a-103">Templates</span></span>
## <a name="overview"></a><span data-ttu-id="0102a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0102a-104">Overview</span></span>
<span data-ttu-id="0102a-105">Sablonok engedélyezéséhez adja meg az értesítéseket, azt kéri a formátummal ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0102a-105">Templates enable a client application to specify the exact format of the notifications it wants to receive.</span></span> <span data-ttu-id="0102a-106">Sablonok használatával, az alkalmazások is valósíthat meg több különböző előnyöket, többek között a következők:</span><span class="sxs-lookup"><span data-stu-id="0102a-106">Using templates, an app can realize several different benefits, including the following :</span></span>

* <span data-ttu-id="0102a-107">A platform-független háttér</span><span class="sxs-lookup"><span data-stu-id="0102a-107">A platform-agnostic backend</span></span>
* <span data-ttu-id="0102a-108">Személyre szabott értesítések</span><span class="sxs-lookup"><span data-stu-id="0102a-108">Personalized notifications</span></span>
* <span data-ttu-id="0102a-109">Ügyfélverzió függetlenség</span><span class="sxs-lookup"><span data-stu-id="0102a-109">Client-version independence</span></span>
* <span data-ttu-id="0102a-110">Egyszerű honosítása</span><span class="sxs-lookup"><span data-stu-id="0102a-110">Easy localization</span></span>

<span data-ttu-id="0102a-111">Ez a témakör két részletes példákat a célcsoport-kezelési az eszközök különböző platformokon platform-független értesítések küldéséhez, és személyre szabása minden eszközre szórási értesítési sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="0102a-111">This section provides two in-depth examples of how to use templates to send platform-agnostic notifications targeting all your devices across platforms, and to personalize broadcast notification to each device.</span></span>

## <a name="using-templates-cross-platform"></a><span data-ttu-id="0102a-112">Sablonok platformfüggetlen használatával</span><span class="sxs-lookup"><span data-stu-id="0102a-112">Using templates cross-platform</span></span>
<span data-ttu-id="0102a-113">A szabványos leküldéses értesítések küldéséhez módja küldi, minden értesítés, amely küldött, a megadott platform értesítéseket kezelő szolgáltatása (WNS, APNS) a hasznos adatok között.</span><span class="sxs-lookup"><span data-stu-id="0102a-113">The standard way to send push notifications is to send, for each notification that is to be sent, a specific payload to platform notification services (WNS, APNS).</span></span> <span data-ttu-id="0102a-114">Például APNS riasztást küld, a tartalom nem egy Json-objektum a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="0102a-114">For example, to send an alert to APNS, the payload is a Json object of the following form:</span></span>

    {"aps": {"alert" : "Hello!" }}

<span data-ttu-id="0102a-115">Hasonló bejelentési üzenet küldése egy Windows Áruházbeli alkalmazásra, az XML-forgalma a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="0102a-115">To send a similar toast message on a Windows Store application, the XML payload is as follows:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

<span data-ttu-id="0102a-116">Hasonló hasznos adat található MPNS (Windows Phone) és a GCM (Android) platformokhoz hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0102a-116">You can create similar payloads for MPNS (Windows Phone) and GCM (Android) platforms.</span></span>

<span data-ttu-id="0102a-117">Ez a követelmény kényszeríti a háttéralkalmazás létrehozásához minden egyes platform különböző hasznos adat található, és hatékonyan miatt a háttérrendszer felelős a bemutató réteg az alkalmazás a része.</span><span class="sxs-lookup"><span data-stu-id="0102a-117">This requirement forces the app backend to produce different payloads for each platform, and effectively makes the backend responsible for part of the presentation layer of the app.</span></span> <span data-ttu-id="0102a-118">Egyes okok közé tartoznak a honosítás és grafikus elrendezések (különösen a Windows Áruházbeli alkalmazások, amelyek tartalmazzák a csempék különféle típusú értesítések).</span><span class="sxs-lookup"><span data-stu-id="0102a-118">Some concerns include localization and graphical layouts (especially for Windows Store apps that include notifications for various types of tiles).</span></span>

<span data-ttu-id="0102a-119">A Notification Hubs sablon funkció lehetővé teszi, hogy egy ügyfélalkalmazás különleges regisztrációk nevű sablon regisztrációk, többek között, a címkék, készlete mellett egy sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0102a-119">The Notification Hubs template feature enables a client app to create special registrations, called template registrations, which include, in addition to the set of tags, a template.</span></span> <span data-ttu-id="0102a-120">A Notification Hubs sablon funkció lehetővé teszi, hogy egy ügyfélalkalmazás eszközök társítandó sablonok, hogy telepítések (ajánlott) vagy a regisztráció dolgozik.</span><span class="sxs-lookup"><span data-stu-id="0102a-120">The Notification Hubs template feature enables a client app to associate devices with templates whether you are working with Installations (preferred) or Registrations.</span></span> <span data-ttu-id="0102a-121">A fenti példákban hasznos megadott, csak a platformfüggetlen információ a tényleges riasztási üzenete (Hello!).</span><span class="sxs-lookup"><span data-stu-id="0102a-121">Given the preceding payload examples, the only platform-independent information is the actual alert message (Hello!).</span></span> <span data-ttu-id="0102a-122">A sablon olyan utasítások az adott ügyfél-alkalmazás regisztrációjának platformfüggetlen üzenetet formázásának módja az értesítési központ.</span><span class="sxs-lookup"><span data-stu-id="0102a-122">A template is a set of instructions for the Notification Hub on how to format a platform-independent message for the registration of that specific client app.</span></span> <span data-ttu-id="0102a-123">Az előző példában a platform független üzenet egyetlen tulajdonság: **message = Hello!**.</span><span class="sxs-lookup"><span data-stu-id="0102a-123">In the preceding example, the platform independent message is a single property: **message = Hello!**.</span></span>

<span data-ttu-id="0102a-124">A következő képen a fenti folyamatát mutatja be:</span><span class="sxs-lookup"><span data-stu-id="0102a-124">The following picture illustrates the above process:</span></span>

![](./media/notification-hubs-templates/notification-hubs-hello.png)

<span data-ttu-id="0102a-125">A sablon az iOS alkalmazás-regisztráció a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="0102a-125">The template for the iOS client app registration is as follows:</span></span>

    {"aps": {"alert": "$(message)"}}

<span data-ttu-id="0102a-126">A megfelelő sablon a Windows áruház-ügyfélalkalmazás van:</span><span class="sxs-lookup"><span data-stu-id="0102a-126">The corresponding template for the Windows Store client app is:</span></span>

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

<span data-ttu-id="0102a-127">Figyelje meg, hogy a tényleges üzenet van helyére a kifejezés $(üzenet).</span><span class="sxs-lookup"><span data-stu-id="0102a-127">Notice that the actual message is substituted for the expression $(message).</span></span> <span data-ttu-id="0102a-128">Ebben a kifejezésben az értesítési központ arra utasítja, amikor egy üzenetet küld az adott regisztrációs hozhat létre, és a közös érték kapcsolókat a következő üzenetet.</span><span class="sxs-lookup"><span data-stu-id="0102a-128">This expression instructs the Notification Hub, whenever it sends a message to this particular registration, to build a message that follows it and switches in the common value.</span></span>

<span data-ttu-id="0102a-129">Ha telepítési modell dolgozik, a telepítés "sablon" billentyű rendelkezik több sablonok JSON.</span><span class="sxs-lookup"><span data-stu-id="0102a-129">If you are working with Installation model, the installation “templates” key holds a JSON of multiple templates.</span></span> <span data-ttu-id="0102a-130">Ha alkalmazásregisztrációs modell dolgozik, az ügyfélalkalmazás több regisztrációk hozhat létre ahhoz, hogy több sablon; például egy sablon figyelmeztetések és a csempe frissítések sablonját.</span><span class="sxs-lookup"><span data-stu-id="0102a-130">If you are working with Registration model, the client application can create multiple registrations in order to use multiple templates; for example, a template for alert messages and a template for tile updates.</span></span> <span data-ttu-id="0102a-131">Ügyfélalkalmazások adjunk natív regisztrációk (regisztráció nélküli sablonnal) és a sablon regisztráció.</span><span class="sxs-lookup"><span data-stu-id="0102a-131">Client applications can also mix native registrations (registrations with no template) and template registrations.</span></span>

<span data-ttu-id="0102a-132">Az értesítési központ minden sablon egy értesítést küld, figyelembe véve, hogy tartoznak-e az azonos ügyfélalkalmazás nélkül.</span><span class="sxs-lookup"><span data-stu-id="0102a-132">The Notification Hub sends one notification for each template without considering whether they belong to the same client app.</span></span> <span data-ttu-id="0102a-133">Ez a viselkedés platformfüggetlen értesítések be további értesítések lefordítani használható.</span><span class="sxs-lookup"><span data-stu-id="0102a-133">This behavior can be used to translate platform-independent notifications into more notifications.</span></span> <span data-ttu-id="0102a-134">Például az értesítési központba ugyanazon platform független üzenet zökkenőmentesen fordítható egy bejelentési értesítés és egy csempe frissítése érdemes figyelembe vennie, hogy a háttér nélkül.</span><span class="sxs-lookup"><span data-stu-id="0102a-134">For example, the same platform independent message to the Notification Hub can be seamlessly translated in a toast alert and a tile update, without requiring the backend to be aware of it.</span></span> <span data-ttu-id="0102a-135">Vegye figyelembe, hogy néhány platformon (például iOS) összeomlás több értesítés is érkezett ugyanarra az eszközre, ha elküldi őket egy rövid időn belül.</span><span class="sxs-lookup"><span data-stu-id="0102a-135">Note that some platforms (for example, iOS) might collapse multiple notifications to the same device if they are sent in a short period of time.</span></span>

## <a name="using-templates-for-personalization"></a><span data-ttu-id="0102a-136">Sablonok használata személyre szabása</span><span class="sxs-lookup"><span data-stu-id="0102a-136">Using templates for personalization</span></span>
<span data-ttu-id="0102a-137">Egy másik sablonokkal előnye, hogy a nem tudják használni a Notification Hubs regisztrációs megszemélyesítési értesítések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="0102a-137">Another advantage to using templates is the ability to use Notification Hubs to perform per-registration personalization of notifications.</span></span> <span data-ttu-id="0102a-138">Vegyük példaként a időjárás-alkalmazást, amely egy csempe, amely a időjárási feltételek megjeleníti egy adott helyen.</span><span class="sxs-lookup"><span data-stu-id="0102a-138">For example, consider a weather app that displays a tile with the weather conditions at a specific location.</span></span> <span data-ttu-id="0102a-139">A felhasználó Celsius vagy Fahrenheit fok, és egyetlen vagy öt nap előrejelzés választhat.</span><span class="sxs-lookup"><span data-stu-id="0102a-139">A user can choose between Celsius or Fahrenheit degrees, and a single or five-day forecast.</span></span> <span data-ttu-id="0102a-140">Sablonokkal, minden ügyfél az alkalmazástelepítés regisztrálhatja a kötelező formátum (1 napos Celsius, 1 napos Fahrenheit, 5 nap Celsius, 5-nap Fahrenheit), és a backend kötelező kitölteni, ezeket a sablonokat az összes információt tartalmazó egyetlen üzenet küldése (például egy öt nap az előrejelzések Celsius és Fahrenheit fokban megadva).</span><span class="sxs-lookup"><span data-stu-id="0102a-140">Using templates, each client app installation can register for the format required (1-day Celsius, 1-day Fahrenheit, 5-days Celsius, 5-days Fahrenheit), and have the backend send a single message that contains all the information required to fill those templates (for example, a five-day forecast with Celsius and Fahrenheit degrees).</span></span>

<span data-ttu-id="0102a-141">A sablon az egy egynapos előrejelzés Celsius hőmérsékletek nem az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0102a-141">The template for the one-day forecast with Celsius temperatures is as follows:</span></span>

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

<span data-ttu-id="0102a-142">Az értesítési központnak küldött üzenet tartalmazza a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="0102a-142">The message sent to the Notification Hub contains all the following properties:</span></span>

<table border="1">

<tr><td><span data-ttu-id="0102a-143">day1_image</span><span class="sxs-lookup"><span data-stu-id="0102a-143">day1_image</span></span></td><td><span data-ttu-id="0102a-144">day2_image</span><span class="sxs-lookup"><span data-stu-id="0102a-144">day2_image</span></span></td><td><span data-ttu-id="0102a-145">day3_image</span><span class="sxs-lookup"><span data-stu-id="0102a-145">day3_image</span></span></td><td><span data-ttu-id="0102a-146">day4_image</span><span class="sxs-lookup"><span data-stu-id="0102a-146">day4_image</span></span></td><td><span data-ttu-id="0102a-147">day5_image</span><span class="sxs-lookup"><span data-stu-id="0102a-147">day5_image</span></span></td></tr>

<tr><td><span data-ttu-id="0102a-148">day1_tempC</span><span class="sxs-lookup"><span data-stu-id="0102a-148">day1_tempC</span></span></td><td><span data-ttu-id="0102a-149">day2_tempC</span><span class="sxs-lookup"><span data-stu-id="0102a-149">day2_tempC</span></span></td><td><span data-ttu-id="0102a-150">day3_tempC</span><span class="sxs-lookup"><span data-stu-id="0102a-150">day3_tempC</span></span></td><td><span data-ttu-id="0102a-151">day4_tempC</span><span class="sxs-lookup"><span data-stu-id="0102a-151">day4_tempC</span></span></td><td><span data-ttu-id="0102a-152">day5_tempC</span><span class="sxs-lookup"><span data-stu-id="0102a-152">day5_tempC</span></span></td></tr>

<tr><td><span data-ttu-id="0102a-153">day1_tempF</span><span class="sxs-lookup"><span data-stu-id="0102a-153">day1_tempF</span></span></td><td><span data-ttu-id="0102a-154">day2_tempF</span><span class="sxs-lookup"><span data-stu-id="0102a-154">day2_tempF</span></span></td><td><span data-ttu-id="0102a-155">day3_tempF</span><span class="sxs-lookup"><span data-stu-id="0102a-155">day3_tempF</span></span></td><td><span data-ttu-id="0102a-156">day4_tempF</span><span class="sxs-lookup"><span data-stu-id="0102a-156">day4_tempF</span></span></td><td><span data-ttu-id="0102a-157">day5_tempF</span><span class="sxs-lookup"><span data-stu-id="0102a-157">day5_tempF</span></span></td></tr>
</table><br/>

<span data-ttu-id="0102a-158">Ez a kialakítás használatával a háttér csak egyetlen üzenetet küld anélkül, hogy a felhasználók az adott testreszabási beállítások tárolására.</span><span class="sxs-lookup"><span data-stu-id="0102a-158">By using this pattern, the backend only sends a single message without having to store specific personalization options for the app users.</span></span> <span data-ttu-id="0102a-159">A következő kép bemutatja, ebben a forgatókönyvben:</span><span class="sxs-lookup"><span data-stu-id="0102a-159">The following picture illustrates this scenario:</span></span>

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a><span data-ttu-id="0102a-160">Hogyan kell regisztrálni a sablonok</span><span class="sxs-lookup"><span data-stu-id="0102a-160">How to register templates</span></span>
<span data-ttu-id="0102a-161">A telepítési modell (ajánlott), vagy az alkalmazásregisztrációs modell segítségével sablonok regisztrálni, lásd: [regisztrációs felügyeleti](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="0102a-161">To register with templates using the Installation model (preferred), or the Registration model, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

## <a name="template-expression-language"></a><span data-ttu-id="0102a-162">Sablon kifejezés nyelve</span><span class="sxs-lookup"><span data-stu-id="0102a-162">Template expression language</span></span>
<span data-ttu-id="0102a-163">XML- vagy JSON-dokumentum formátumok sablonok korlátozódnak.</span><span class="sxs-lookup"><span data-stu-id="0102a-163">Templates are limited to XML or JSON document formats.</span></span> <span data-ttu-id="0102a-164">Emellett csak elhelyezheti kifejezések adott helyek; például csomópont attribútumok vagy XML-értékek JSON karakterlánc-tulajdonságok értékeit.</span><span class="sxs-lookup"><span data-stu-id="0102a-164">Also, you can only place expressions in particular places; for example, node attributes or values for XML, string property values for JSON.</span></span>

<span data-ttu-id="0102a-165">Az alábbi táblázat a sablonok engedélyezett nyelv:</span><span class="sxs-lookup"><span data-stu-id="0102a-165">The following table shows the language allowed in templates:</span></span>

| <span data-ttu-id="0102a-166">kifejezés</span><span class="sxs-lookup"><span data-stu-id="0102a-166">Expression</span></span> | <span data-ttu-id="0102a-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="0102a-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0102a-168">$(prop)</span><span class="sxs-lookup"><span data-stu-id="0102a-168">$(prop)</span></span> |<span data-ttu-id="0102a-169">A megadott nevű egy eseménytulajdonság hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="0102a-169">Reference to an event property with the given name.</span></span> <span data-ttu-id="0102a-170">A tulajdonságnevek nem nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="0102a-170">Property names are not case-sensitive.</span></span> <span data-ttu-id="0102a-171">Ebben a kifejezésben oldja fel a tulajdonságérték szöveg vagy üres karakterlánc, ha a tulajdonság nem található.</span><span class="sxs-lookup"><span data-stu-id="0102a-171">This expression resolves into the property’s text value or into an empty string if the property is not present.</span></span> |
| <span data-ttu-id="0102a-172">$(prop, n)</span><span class="sxs-lookup"><span data-stu-id="0102a-172">$(prop, n)</span></span> |<span data-ttu-id="0102a-173">A fenti de a szöveg explicit módon levágva n karakterek, például $(cím, 20) levágja a cím tulajdonság tartalmának: 20 karakter.</span><span class="sxs-lookup"><span data-stu-id="0102a-173">As above, but the text is explicitly clipped at n characters, for example $(title, 20) clips the contents of the title property at 20 characters.</span></span> |
| <span data-ttu-id="0102a-174">. (prop, n)</span><span class="sxs-lookup"><span data-stu-id="0102a-174">.(prop, n)</span></span> |<span data-ttu-id="0102a-175">A fenti de három pont utótaggal a szöveg, amelyhez hozzá van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="0102a-175">As above, but the text is suffixed with three dots as it is clipped.</span></span> <span data-ttu-id="0102a-176">A lerövidített karakterlánc és az utótag teljes mérete nem haladja meg a n karakter.</span><span class="sxs-lookup"><span data-stu-id="0102a-176">The total size of the clipped string and the suffix does not exceed n characters.</span></span> <span data-ttu-id="0102a-177">. (cím, 20) és az "Ez a cím sor az" eredményeinek tulajdonsága bemeneti **címe...**</span><span class="sxs-lookup"><span data-stu-id="0102a-177">.(title, 20) with an input property of “This is the title line” results in **This is the title...**</span></span> |
| <span data-ttu-id="0102a-178">%(prop)</span><span class="sxs-lookup"><span data-stu-id="0102a-178">%(prop)</span></span> |<span data-ttu-id="0102a-179">Hasonló $(name) azzal a különbséggel, hogy a kimeneti URI-kódolású.</span><span class="sxs-lookup"><span data-stu-id="0102a-179">Similar to $(name) except that the output is URI-encoded.</span></span> |
| <span data-ttu-id="0102a-180">#(prop)</span><span class="sxs-lookup"><span data-stu-id="0102a-180">#(prop)</span></span> |<span data-ttu-id="0102a-181">A JSON-sablonokban használt (például az iOS és Android sablonok).</span><span class="sxs-lookup"><span data-stu-id="0102a-181">Used in JSON templates (for example, for iOS and Android templates).</span></span><br><br><span data-ttu-id="0102a-182">Ez a funkció ugyanúgy működik mint a korábban megadott, kivéve ha szerepel a JSON-sablonokat (például Apple-sablonok) $(prop).</span><span class="sxs-lookup"><span data-stu-id="0102a-182">This function works exactly the same as $(prop) previously specified, except when used in JSON templates (for example, Apple templates).</span></span> <span data-ttu-id="0102a-183">Ebben az esetben, ha ez a funkció nem körülvett "{","}" (például "myJsonProperty':"#(név)"), és az eredmény egy számot a Javascript-formátumban, például RegExp szolgáltatást: (0 &#124; (&#91; 1 – 9 &#93; &#91; 0-9 & #93 ;*))(\. &#91; 0-9 &#93; +)? ((az e &#124; E) (+ &#124;-)? &#91; 0-9 &#93; +)?, akkor a kimenet JSON egy számot.</span><span class="sxs-lookup"><span data-stu-id="0102a-183">In this case, if this function is not surrounded by “{‘,’}” (for example, ‘myJsonProperty’ : ‘#(name)’), and it evaluates to a number in Javascript format, for example, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*))(\.&#91;0-9&#93;+)?((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, then the output JSON is a number.</span></span><br><br><span data-ttu-id="0102a-184">Például "jelvény:"#(név)"válik"jelvények": 40 (és nem"40").</span><span class="sxs-lookup"><span data-stu-id="0102a-184">For example, ‘badge : ‘#(name)’ becomes ‘badge’ : 40 (and not ‘40‘).</span></span> |
| <span data-ttu-id="0102a-185">"text" vagy "text"</span><span class="sxs-lookup"><span data-stu-id="0102a-185">‘text’ or “text”</span></span> |<span data-ttu-id="0102a-186">A szövegkonstans.</span><span class="sxs-lookup"><span data-stu-id="0102a-186">A literal.</span></span> <span data-ttu-id="0102a-187">Literálok egyetlen vagy dupla idézőjelek közé tetszőleges szöveget tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="0102a-187">Literals contain arbitrary text enclosed in single or double quotes.</span></span> |
| <span data-ttu-id="0102a-188">Kif1 + Kif2</span><span class="sxs-lookup"><span data-stu-id="0102a-188">expr1 + expr2</span></span> |<span data-ttu-id="0102a-189">A csatlakozás egyetlen karakterlánccá egyesít két kifejezések összefűzése operátor.</span><span class="sxs-lookup"><span data-stu-id="0102a-189">The concatenation operator joining two expressions into a single string.</span></span> |

<span data-ttu-id="0102a-190">A kifejezés előző űrlapokat bármelyike lehet.</span><span class="sxs-lookup"><span data-stu-id="0102a-190">The expressions can be any of the preceding forms.</span></span>

<span data-ttu-id="0102a-191">Ha kapott, a teljes kifejezést kell körülvett az {}.</span><span class="sxs-lookup"><span data-stu-id="0102a-191">When using concatenation, the entire expression must be surrounded with {}.</span></span> <span data-ttu-id="0102a-192">Például {$(prop) + '-' + $(prop2)}.</span><span class="sxs-lookup"><span data-stu-id="0102a-192">For example, {$(prop) + ‘ - ’ + $(prop2)}.</span></span> |

<span data-ttu-id="0102a-193">Például a következő értéke nem egy érvényes XML-sablont:</span><span class="sxs-lookup"><span data-stu-id="0102a-193">For example, the following is not a valid XML template:</span></span>

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


<span data-ttu-id="0102a-194">Amint azt fent összefűzése használatakor kifejezések kell csomagolni, kapcsos zárójelek közé.</span><span class="sxs-lookup"><span data-stu-id="0102a-194">As explained above, when using concatenation, expressions must be wrapped in curly brackets.</span></span> <span data-ttu-id="0102a-195">Példa:</span><span class="sxs-lookup"><span data-stu-id="0102a-195">For example:</span></span>

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

