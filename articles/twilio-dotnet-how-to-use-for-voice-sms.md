---
title: "aaaHow tooUse Twilio hang-és SMS (.NET) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. Kódminták .NET."
services: 
documentationcenter: .net
author: devinrader
manager: twilio
editor: 
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: f568da87ef15e9f540fee9674de31e983d4acb6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-from-azure"></a><span data-ttu-id="f9089-104">Hogyan toouse Twilio hang-és SMS-képességek az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="f9089-104">How toouse Twilio for voice and SMS capabilities from Azure</span></span>
<span data-ttu-id="f9089-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f9089-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="f9089-106">a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="f9089-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="f9089-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [további lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f9089-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next steps](#NextSteps) section.</span></span>

## <span data-ttu-id="f9089-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="f9089-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="f9089-109">Twilio hello jövőbeli üzleti kommunikáció működtetéséhez, amely lehetővé teszi a fejlesztők tooembed hang, a VoIP, és alkalmazásokba üzenetküldési.</span><span class="sxs-lookup"><span data-stu-id="f9089-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="f9089-110">Ezek virtualizálása hello Twilio kommunikációs API-platformon keresztül, az ilyen felhőalapú, a globális környezetben szükséges összes infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="f9089-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="f9089-111">Olyan egyszerű toobuild és méretezhető.</span><span class="sxs-lookup"><span data-stu-id="f9089-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="f9089-112">Élvezze a rugalmasságot az árképzés használatalapú fizetés,-felhő megbízhatóság igénybe.</span><span class="sxs-lookup"><span data-stu-id="f9089-112">Enjoy flexibility with pay-as-you-go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="f9089-113">**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="f9089-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="f9089-114">**Twilio SMS** lehetővé teszi, hogy az alkalmazások toosend és az SMS-üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="f9089-114">**Twilio SMS** enables your applications toosend and receive SMS messages.</span></span> <span data-ttu-id="f9089-115">**Twilio-ügyfél** lehetővé teszi a toomake VoIP hívást a telefon, a táblagép vagy a böngésző és WebRTC támogatja.</span><span class="sxs-lookup"><span data-stu-id="f9089-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="f9089-116"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="f9089-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="f9089-117">Az Azure-ügyfelek egy [a különleges ajánlat](http://www.twilio.com/azure): udvarias $10 Twilio-kredit a Twilio-fiók frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="f9089-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="f9089-118">A Twilio-jóváírási alkalmazott tooany Twilio-használati ($10 jóváírás egyenértékű toosending akár 1000 SMS-t vagy mentése too1000 fogadás bejövő hang perc, attól függően, hogy a telefon és üzenet vagy hívás cél hello helye) lehet.</span><span class="sxs-lookup"><span data-stu-id="f9089-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="f9089-119">A Twilio-jóváírási beváltani és első lépések [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="f9089-119">Redeem this Twilio credit and get started at [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="f9089-120">Twilio egy olyan használatalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f9089-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="f9089-121">Nincsenek nem beállításról díjakat, és bármikor bezárhatja a fiókját.</span><span class="sxs-lookup"><span data-stu-id="f9089-121">There are no set-up fees, and you can close your account at any time.</span></span> <span data-ttu-id="f9089-122">További részletei: [Twilio árképzési](http://www.twilio.com/voice/pricing).</span><span class="sxs-lookup"><span data-stu-id="f9089-122">You can find more details at [Twilio Pricing](http://www.twilio.com/voice/pricing).</span></span>

## <span data-ttu-id="f9089-123"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="f9089-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="f9089-124">hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="f9089-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="f9089-125">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="f9089-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="f9089-126">Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="f9089-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="f9089-127"><a id="Verbs"></a>Twilio-műveletek</span><span class="sxs-lookup"><span data-stu-id="f9089-127"><a id="Verbs"></a>Twilio verbs</span></span>
<span data-ttu-id="f9089-128">hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.</span><span class="sxs-lookup"><span data-stu-id="f9089-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="f9089-129">hello az alábbiakban olvashat egy listát a Twilio-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f9089-129">hello following is a list of Twilio verbs.</span></span>  <span data-ttu-id="f9089-130">Ismerje meg, más műveletek és a képességek keresztül készül hello [Twilio Markup Language dokumentáció](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="f9089-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="f9089-131">**&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="f9089-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="f9089-132">**&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.</span><span class="sxs-lookup"><span data-stu-id="f9089-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="f9089-133">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="f9089-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="f9089-134">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="f9089-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="f9089-135">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="f9089-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="f9089-136">**&lt;Rekord&gt;**: hello hívó hang rögzíti, és egy hello rögzítése tartalmazó fájl URL-CÍMÉT adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f9089-136">**&lt;Record&gt;**: Records hello caller's voice and returns an URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="f9089-137">**&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="f9089-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="f9089-138">**&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio szám anélkül, hogy számlázási</span><span class="sxs-lookup"><span data-stu-id="f9089-138">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="f9089-139">**&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.</span><span class="sxs-lookup"><span data-stu-id="f9089-139">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="f9089-140">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="f9089-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="f9089-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="f9089-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="f9089-142">TwiML egy olyan XML-alapú utasítások alapján, amely tájékoztatja, hogy hogyan Twilio hello Twilio-műveletek tooprocess hívás vagy SMS.</span><span class="sxs-lookup"><span data-stu-id="f9089-142">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="f9089-143">Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="f9089-143">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="f9089-144">Ha az alkalmazás hívások hello Twilio API, hello API paraméterek egyike hello URL-címet, amely hello TwiML választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f9089-144">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="f9089-145">Fejlesztési célra Twilio által megadott URL-címek tooprovide hello TwiML válaszok az alkalmazások által használt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f9089-145">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="f9089-146">Akkor is elhelyezheti a saját URL-címek tooproduce hello TwiML válaszokat, és egy másik lehetőség toouse hello **TwiMLResponse** objektum.</span><span class="sxs-lookup"><span data-stu-id="f9089-146">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="f9089-147">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="f9089-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="f9089-148">Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="f9089-148">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="f9089-149"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9089-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="f9089-150">Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="f9089-150">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="f9089-151">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="f9089-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="f9089-152">Amikor regisztrál egy Twilio-fiókja, kapni fog egy Fiókazonosító és egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="f9089-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="f9089-153">Egyaránt lesz szükséges toomake Twilio API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="f9089-153">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="f9089-154">nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága.</span><span class="sxs-lookup"><span data-stu-id="f9089-154">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="f9089-155">A Fiókazonosítót és a hitelesítési jogkivonat megtekinthető a hello [Twilio lapra][twilio_account], a mezők feliratú hello **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.</span><span class="sxs-lookup"><span data-stu-id="f9089-155">Your account ID and authentication token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="f9089-156"><a id="create_app"></a>Az Azure-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9089-156"><a id="create_app"></a>Create an Azure Application</span></span>
<span data-ttu-id="f9089-157">Az Azure-alkalmazások, amelyen a Twilio-engedélyezett alkalmazás nem eltér a többi Azure alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f9089-157">An Azure application that hosts a Twilio enabled application is no different from any other Azure application.</span></span> <span data-ttu-id="f9089-158">Hello Twilio .NET könyvtár hozzáadása és konfigurálása hello szerepkör toouse hello Twilio .NET-kódtárakra.</span><span class="sxs-lookup"><span data-stu-id="f9089-158">You add hello Twilio .NET library and configure hello role toouse hello Twilio .NET libraries.</span></span>
<span data-ttu-id="f9089-159">A kezdeti Azure-projekt létrehozása információkért lásd: [Azure-projekt létrehozása a Visual Studio][vs_project].</span><span class="sxs-lookup"><span data-stu-id="f9089-159">For information on creating an initial Azure project, see [Creating an Azure project with Visual Studio][vs_project].</span></span>

## <span data-ttu-id="f9089-160"><a id="configure_app"></a>Alkalmazás toouse Twilio szalagtárak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f9089-160"><a id="configure_app"></a>Configure Your Application toouse Twilio Libraries</span></span>
<span data-ttu-id="f9089-161">Twilio biztosít, hogy a Twilio tooprovide egyszerű és egyszerű módon toointeract hello Twilio REST API-t és a Twilio-ügyfél különböző aspektusainak burkolása .NET segítő szalagtárak toogenerate TwiML válaszokat.</span><span class="sxs-lookup"><span data-stu-id="f9089-161">Twilio provides a set of .NET helper libraries that wrap various aspects of Twilio tooprovide simple and easy ways toointeract with hello Twilio REST API and Twilio Client toogenerate TwiML responses.</span></span>

<span data-ttu-id="f9089-162">Twilio öt szalagtárak biztosít a .NET-fejlesztők számára:</span><span class="sxs-lookup"><span data-stu-id="f9089-162">Twilio provides five libraries for .NET developers:</span></span>
<span data-ttu-id="f9089-163">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="f9089-163">Library</span></span>|<span data-ttu-id="f9089-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="f9089-164">Description</span></span>
---|---
<span data-ttu-id="f9089-165">Twilio.API</span><span class="sxs-lookup"><span data-stu-id="f9089-165">Twilio.API</span></span>|<span data-ttu-id="f9089-166">hello Twilio Alapkönyvtár tördelve hello Twilio REST API-t egy rövid .NET könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="f9089-166">hello core Twilio library that wraps hello Twilio REST API in a friendly .NET library.</span></span> <span data-ttu-id="f9089-167">A könyvtár a .NET, a Silverlight, a Windows Phone 7 és érhető el.</span><span class="sxs-lookup"><span data-stu-id="f9089-167">This library is available for .NET, Silverlight, and Windows Phone 7.</span></span>
<span data-ttu-id="f9089-168">Twilio.TwiML</span><span class="sxs-lookup"><span data-stu-id="f9089-168">Twilio.TwiML</span></span>|<span data-ttu-id="f9089-169">A .NET rövid módon toogenerate TwiML markup biztosít.</span><span class="sxs-lookup"><span data-stu-id="f9089-169">Provides a .NET friendly way toogenerate TwiML markup.</span></span>
<span data-ttu-id="f9089-170">Twilio.MVC</span><span class="sxs-lookup"><span data-stu-id="f9089-170">Twilio.MVC</span></span>|<span data-ttu-id="f9089-171">ASP.NET MVC használó fejlesztők ezt a szalagtárat magában foglalja a TwilioController TwiML ActionResult és kérelem ellenőrző attribútuma.</span><span class="sxs-lookup"><span data-stu-id="f9089-171">For developers using ASP.NET MVC, this library includes a TwilioController, TwiML ActionResult, and request validation attribute.</span></span>
<span data-ttu-id="f9089-172">Twilio.WebMatrix</span><span class="sxs-lookup"><span data-stu-id="f9089-172">Twilio.WebMatrix</span></span>|<span data-ttu-id="f9089-173">A fejlesztők a Microsoft szabad WebMatrix fejlesztési eszköz segítségével a könyvtárban Razor szintaxis segítő különböző Twilio-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="f9089-173">For developers using Microsoft's free WebMatrix development tool, this library contains Razor syntax helpers for various Twilio actions.</span></span>
<span data-ttu-id="f9089-174">Twilio.Client.Capability</span><span class="sxs-lookup"><span data-stu-id="f9089-174">Twilio.Client.Capability</span></span>|<span data-ttu-id="f9089-175">Hello funkció token generátor hello Twilio ügyfél JavaScript SDK való használatra tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f9089-175">Contains hello Capability token generator for use with hello Twilio Client JavaScript SDK.</span></span>

<span data-ttu-id="f9089-176">Vegye figyelembe, hogy a kódtárakat .NET 3.5-ös, a Silverlight 4 vagy a Windows Phone 7 vagy újabb szükséges.</span><span class="sxs-lookup"><span data-stu-id="f9089-176">Note that all libraries require .NET 3.5, Silverlight 4, or Windows Phone 7 or later.</span></span>

<span data-ttu-id="f9089-177">a jelen útmutatóban hello minták hello Twilio.API könyvtár használatára.</span><span class="sxs-lookup"><span data-stu-id="f9089-177">hello samples provided in this guide use hello Twilio.API library.</span></span>

<span data-ttu-id="f9089-178">hello szalagtárak lehet [hello NuGet package manager bővítmény segítségével telepített](http://www.twilio.com/docs/csharp/install) érhető el a Visual Studio 2010 too2015 fel.</span><span class="sxs-lookup"><span data-stu-id="f9089-178">hello libraries can be [installed using hello NuGet package manager extension](http://www.twilio.com/docs/csharp/install) available for Visual Studio 2010 up too2015.</span></span>  <span data-ttu-id="f9089-179">hello forráskód üzemelteti [GitHub][twilio_github_repo], mely tartalmazza a teljes dokumentációt hello szalagtárak tartalmazó Wiki.</span><span class="sxs-lookup"><span data-stu-id="f9089-179">hello source code is hosted on [GitHub][twilio_github_repo], which includes a Wiki that contains complete documentation for using hello libraries.</span></span>

<span data-ttu-id="f9089-180">Alapértelmezés szerint a Microsoft Visual Studio 2010 NuGet 1.2-es verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="f9089-180">By default, Microsoft Visual Studio 2010 installs version 1.2 of NuGet.</span></span> <span data-ttu-id="f9089-181">Hello Twilio-tárak telepítéséhez az 1.6-os NuGet vagy újabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="f9089-181">Installing hello Twilio libraries requires version 1.6 of NuGet or higher.</span></span> <span data-ttu-id="f9089-182">Információ a telepítésekor vagy frissítésekor NuGet: [http://nuget.org/][nuget].</span><span class="sxs-lookup"><span data-stu-id="f9089-182">For information on installing or updating NuGet, see [http://nuget.org/][nuget].</span></span>

> [!NOTE]
> <span data-ttu-id="f9089-183">tooinstall hello legújabb verziójának NuGet, előbb el kell távolítania hello betöltött verzió hello Visual Studio-kiterjesztés kezelőjének használatával.</span><span class="sxs-lookup"><span data-stu-id="f9089-183">tooinstall hello latest version of NuGet, you must first uninstall hello loaded version using hello Visual Studio Extension Manager.</span></span> <span data-ttu-id="f9089-184">toodo Igen, futtatnia kell a Visual Studio rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f9089-184">toodo so, you must run Visual Studio as administrator.</span></span> <span data-ttu-id="f9089-185">Ellenkező esetben hello Eltávolítás gomb le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="f9089-185">Otherwise, hello Uninstall button is disabled.</span></span>
>
>

### <span data-ttu-id="f9089-186"><a id="use_nuget"></a>tooadd hello Twilio szalagtárak tooyour Visual Studio-projekt:</span><span class="sxs-lookup"><span data-stu-id="f9089-186"><a id="use_nuget"></a>tooadd hello Twilio libraries tooyour Visual Studio project:</span></span>
1. <span data-ttu-id="f9089-187">Nyissa meg a megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="f9089-187">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="f9089-188">Kattintson a jobb gombbal **hivatkozások**.</span><span class="sxs-lookup"><span data-stu-id="f9089-188">Right-click **References**.</span></span>
3. <span data-ttu-id="f9089-189">Kattintson a **NuGet-csomagok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="f9089-189">Click **Manage NuGet Packages...**</span></span>
4. <span data-ttu-id="f9089-190">Kattintson a **Online**.</span><span class="sxs-lookup"><span data-stu-id="f9089-190">Click **Online**.</span></span>
5. <span data-ttu-id="f9089-191">Hello online a keresőmezőbe írja be a *twilio*.</span><span class="sxs-lookup"><span data-stu-id="f9089-191">In hello search online box, type *twilio*.</span></span>
6. <span data-ttu-id="f9089-192">Kattintson a **telepítése** hello Twilio-csomag.</span><span class="sxs-lookup"><span data-stu-id="f9089-192">Click **Install** on hello Twilio package.</span></span>

## <span data-ttu-id="f9089-193"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="f9089-193"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="f9089-194">hello következő bemutatja, hogyan egy kimenő toomake hívás, hello használatával **CallResource** osztály.</span><span class="sxs-lookup"><span data-stu-id="f9089-194">hello following shows how toomake an outgoing call using hello **CallResource** class.</span></span> <span data-ttu-id="f9089-195">Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ.</span><span class="sxs-lookup"><span data-stu-id="f9089-195">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="f9089-196">Helyettesítse a saját értékeit hello **való** és **a** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszám Twilio-fiókja hello kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="f9089-196">Substitute your values for hello **to** and **from** phone numbers, and ensure that you verify hello **from** phone number for your Twilio account before running hello code.</span></span>

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use hello Twilio-provided site for hello TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set hello call From, To, and URL values toouse for hello call.
    // This sample uses hello sandbox number provided by
    // Twilio toomake hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

<span data-ttu-id="f9089-197">Az átadott toohello hello paraméterekkel kapcsolatos további információért **CallResource.Create** módszer, lásd: [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="f9089-197">For more information about hello parameters passed in toohello **CallResource.Create** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="f9089-198">Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ.</span><span class="sxs-lookup"><span data-stu-id="f9089-198">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="f9089-199">Ehelyett használhat saját hely tooprovide hello TwiML választ.</span><span class="sxs-lookup"><span data-stu-id="f9089-199">You could instead use your own site tooprovide hello TwiML response.</span></span> <span data-ttu-id="f9089-200">További információkért lásd: [hogyan: Adjon meg TwiML válaszok a saját webhelyén](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="f9089-200">For more information, see [How to: Provide TwiML responses from your own website](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="f9089-201"><a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="f9089-201"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="f9089-202">hello következő képernyőfelvétel egy SMS üzenetet használatával toosend hello hogyan **MessageResource** osztály.</span><span class="sxs-lookup"><span data-stu-id="f9089-202">hello following screenshot shows how toosend an SMS message using hello **MessageResource**  class.</span></span> <span data-ttu-id="f9089-203">Hello **a** szám Twilio biztosítja a próbaverzió fiókok toosend SMS-t.</span><span class="sxs-lookup"><span data-stu-id="f9089-203">hello **from** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="f9089-204">Hello **való** szám ellenőrizni kell a Twilio-fiókjához hello kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="f9089-204">hello **to** number must be verified for your Twilio account before you run hello code.</span></span>

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    try
    {
        // Send an SMS message.
        var message = MessageResource.Create(
            to: new PhoneNumber("+12069419717"),
            from: new PhoneNumber("+14155992671"),
            body: "This is my SMS message.");
    }
    catch (TwilioException ex)
    {
        // An exception occurred making hello REST call
        Console.WriteLine(ex.Message);
    }

## <span data-ttu-id="f9089-205"><a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok</span><span class="sxs-lookup"><span data-stu-id="f9089-205"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own website</span></span>
<span data-ttu-id="f9089-206">Ha az alkalmazás kezdeményezi a Twilio API - hívás toohello például keresztül hello **CallResource.Create** metódus - Twilio a kérelem tooan URL-CÍMÉT, amely várt tooreturn TwiML választ küld.</span><span class="sxs-lookup"><span data-stu-id="f9089-206">When your application initiates a call toohello Twilio API - for example, via hello **CallResource.Create** method - Twilio sends your request tooan URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="f9089-207">hello példát [hogyan: végezhet](#howto_make_call) használ hello Twilio-megadott URL-címet [http://twimlets.com/message] [ twimlet_message_url] tooreturn hello válasz.</span><span class="sxs-lookup"><span data-stu-id="f9089-207">hello example in [How to: Make an outgoing call](#howto_make_call) uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url] tooreturn hello response.</span></span>

> [!NOTE]
> <span data-ttu-id="f9089-208">Amíg TwiML web Services való használatra tervezték, hello TwiML tekintheti meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="f9089-208">While TwiML is designed for use by web services, you can view hello TwiML in your browser.</span></span> <span data-ttu-id="f9089-209">Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres &lt;válasz&gt; elem; másik példaként, kattintson a [http://twimlets.com/message ? Üzenet % 5B0 %5 D Hello % 20World =](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee egy &lt;válasz&gt; elem, amely tartalmazza egy &lt;szóbeli&gt; elem.</span><span class="sxs-lookup"><span data-stu-id="f9089-209">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty &lt;Response&gt; element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee a &lt;Response&gt; element that contains a &lt;Say&gt; element.</span></span>
>
>

<span data-ttu-id="f9089-210">Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját URL-cím hely, amely HTTP-válaszokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f9089-210">Instead of relying on hello Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="f9089-211">Hello webhely HTTP-válaszok visszaadó eltérő nyelvű hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="f9089-211">You can create hello site in any language that returns HTTP responses.</span></span> <span data-ttu-id="f9089-212">Ez a témakör azt feltételezi, hogy lesz hello URL-CÍMÉT egy ASP.NET általános kezelő üzemeltetési.</span><span class="sxs-lookup"><span data-stu-id="f9089-212">This topic assumes you'll be hosting hello URL from an ASP.NET generic handler.</span></span>

<span data-ttu-id="f9089-213">ASP.NET-kezelő a következő hello létrehozza TwiML választ, amely szerint **Hello World** hello hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f9089-213">hello following ASP.NET Handler crafts a TwiML response that says **Hello World** on hello call.</span></span>

    using System.Text;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {
            public void ProcessRequest(HttpContext context)
            {
                const string twiMLResponse =
                    "<Response><Say>Hello World.</Say></Response>";
                
                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.ContentEncoding = Encoding.UTF8;
                context.Response.Write(twiMLResponse);
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }
    
<span data-ttu-id="f9089-214">Ahogy látja, a fenti példa hello, hello TwiML válasz egyszerűen egy XML-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="f9089-214">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="f9089-215">hello Twilio.TwiML könyvtárban osztályokkal rendelkezik, amelyek az Ön TwiML hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f9089-215">hello Twilio.TwiML library contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="f9089-216">hello az alábbi példa hello egyenértékű válasz eredményez, ahogy fent látható, de használ hello **VoiceResponse** osztály.</span><span class="sxs-lookup"><span data-stu-id="f9089-216">hello example below produces hello equivalent response as shown above, but uses hello **VoiceResponse** class.</span></span>

    using System.Web;
    using Twilio.TwiML;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                var twiml = new VoiceResponse();
                twiml.Say("Hello World.");

                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.Write(twiml.ToString());
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

<span data-ttu-id="f9089-217">TwiML kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="f9089-217">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span></span>

<span data-ttu-id="f9089-218">Egy módon tooprovide TwiML válaszok beállítása után átadhatók adott URL-cím toohello **CallResource.Create** metódust.</span><span class="sxs-lookup"><span data-stu-id="f9089-218">Once you have set up a way tooprovide TwiML responses, you can pass that URL toohello **CallResource.Create** method.</span></span> <span data-ttu-id="f9089-219">Például, ha rendelkezik telepített MyTwiML tooan Azure-felhőszolgáltatásban nevű webalkalmazást, és az ASP.NET-kezelő hello nevét mytwiml.ashx, hello URL-cím átadhatók túl**CallResource.Create** látható módon a következő kód hello Példa:</span><span class="sxs-lookup"><span data-stu-id="f9089-219">For example, if you have a web application named MyTwiML deployed tooan Azure cloud service, and hello name of your ASP.NET Handler is mytwiml.ashx, hello URL can be passed too**CallResource.Create** as shown in hello following code sample:</span></span>

    // This sample uses hello sandbox number provided by Twilio toomake hello call.
    // Place hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

<span data-ttu-id="f9089-220">Azure és az ASP.NET Twilio használatával kapcsolatos további információkért lásd: [hogyan toomake egy telefonhívási Twilio használata Azure webes szerepkörrel rendelkező][howto_phonecall_dotnet].</span><span class="sxs-lookup"><span data-stu-id="f9089-220">For additional information about using Twilio on Azure with ASP.NET, see [How toomake a phone call using Twilio in a web role on Azure][howto_phonecall_dotnet].</span></span>

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: http://twimlets.com/message

[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[vs_project]:http://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:http://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
