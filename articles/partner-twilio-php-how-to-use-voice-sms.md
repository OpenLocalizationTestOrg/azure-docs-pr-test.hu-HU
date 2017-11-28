---
title: "aaaHow tooUse Twilio hang-és SMS (PHP) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. Kódminták PHP."
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 9354df8de694826a0ff7ea92620ec4d7e5c2fd70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-php"></a><span data-ttu-id="0f8ba-104">Hogyan tooUse Twilio hang-és a PHP SMS képességei</span><span class="sxs-lookup"><span data-stu-id="0f8ba-104">How tooUse Twilio for Voice and SMS Capabilities in PHP</span></span>
<span data-ttu-id="0f8ba-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="0f8ba-106">a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="0f8ba-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="0f8ba-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="0f8ba-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="0f8ba-109">Twilio hello jövőbeli üzleti kommunikáció működtetéséhez, amely lehetővé teszi a fejlesztők tooembed hang, a VoIP, és alkalmazásokba üzenetküldési.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="0f8ba-110">Ezek virtualizálása hello Twilio kommunikációs API-platformon keresztül, az ilyen felhőalapú, a globális környezetben szükséges összes infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="0f8ba-111">Olyan egyszerű toobuild és méretezhető.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="0f8ba-112">Élvezze a rugalmasságot Önnel fizetési-, nyissa meg árképzési, és igénybe vehesse az felhő megbízhatóság.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="0f8ba-113">**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="0f8ba-114">**Twilio SMS** lehetővé teszi, hogy az alkalmazás toosend és a szöveges üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-114">**Twilio SMS** enables your application toosend and receive text messages.</span></span> <span data-ttu-id="0f8ba-115">**Twilio-ügyfél** lehetővé teszi a toomake VoIP hívást a telefon, a táblagép vagy a böngésző és WebRTC támogatja.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="0f8ba-116"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="0f8ba-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="0f8ba-117">Az Azure-ügyfelek egy [a különleges ajánlat](http://www.twilio.com/azure): udvarias $10 Twilio-kredit a Twilio-fiók frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="0f8ba-118">A Twilio-jóváírási alkalmazott tooany Twilio-használati ($10 jóváírás egyenértékű toosending akár 1000 SMS-t vagy mentése too1000 fogadás bejövő hang perc, attól függően, hogy a telefon és üzenet vagy hívás cél hello helye) lehet.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="0f8ba-119">A Twilio-jóváírási beváltani és első lépések: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="0f8ba-119">Redeem this Twilio credit and get started at: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="0f8ba-120">Twilio egy olyan használatalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="0f8ba-121">Nincsenek nem beállításról díjakat, és bármikor bezárhatja a fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="0f8ba-122">További részletei: [Twilio árképzési][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="0f8ba-123"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="0f8ba-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="0f8ba-124">hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="0f8ba-125">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="0f8ba-126">Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="0f8ba-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="0f8ba-127"><a id="Verbs"></a>Twilio-műveletek</span><span class="sxs-lookup"><span data-stu-id="0f8ba-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="0f8ba-128">hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="0f8ba-129">hello az alábbiakban olvashat egy listát a Twilio-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-129">hello following is a list of Twilio verbs.</span></span> <span data-ttu-id="0f8ba-130">Ismerje meg, más műveletek és a képességek keresztül készül hello [Twilio Markup Language dokumentáció](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="0f8ba-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="0f8ba-131">**&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="0f8ba-132">**&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="0f8ba-133">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="0f8ba-134">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="0f8ba-135">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="0f8ba-136">**&lt;Rekord&gt;**: hello hívó hang rögzíti és hello rögzítése tartalmazó fájl URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-136">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="0f8ba-137">**&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="0f8ba-138">**&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio szám anélkül, hogy számlázási</span><span class="sxs-lookup"><span data-stu-id="0f8ba-138">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="0f8ba-139">**&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-139">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="0f8ba-140">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="0f8ba-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="0f8ba-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="0f8ba-142">TwiML egy olyan XML-alapú utasítások alapján, amely tájékoztatja, hogy hogyan Twilio hello Twilio-műveletek tooprocess hívás vagy SMS.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-142">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="0f8ba-143">Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-143">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="0f8ba-144">Ha az alkalmazás hívások hello Twilio API, hello API paraméterek egyike hello URL-címet, amely hello TwiML választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-144">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="0f8ba-145">Fejlesztési célra Twilio által megadott URL-címek tooprovide hello TwiML válaszok az alkalmazások által használt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-145">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="0f8ba-146">Akkor is elhelyezheti a saját URL-címek tooproduce hello TwiML válaszokat, és egy másik lehetőség toouse hello **TwiMLResponse** objektum.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-146">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="0f8ba-147">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="0f8ba-148">Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-148">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="0f8ba-149"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f8ba-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="0f8ba-150">Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-150">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="0f8ba-151">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="0f8ba-152">Amikor regisztrál egy Twilio-fiókja, kapni fog egy Fiókazonosító és egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="0f8ba-153">Egyaránt lesz szükséges toomake Twilio API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-153">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="0f8ba-154">nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-154">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="0f8ba-155">A Fiókazonosítót és a hitelesítési jogkivonat megtekinthető a hello [Twilio lapra][twilio_account], a mezők feliratú hello **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-155">Your account ID and authentication token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="0f8ba-156"><a id="create_app"></a>PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f8ba-156"><a id="create_app"></a>Create a PHP Application</span></span>
<span data-ttu-id="0f8ba-157">A PHP-alkalmazások, amelyek hello Twilio szolgáltatást használ, és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más PHP-alkalmazások hello Twilio szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-157">A PHP application that uses hello Twilio service and is running in Azure is no different than any other PHP application that uses hello Twilio service.</span></span> <span data-ttu-id="0f8ba-158">Twilio-szolgáltatások REST-alapú, és a php-ből többféle módon is nevezik, ez a cikk összpontosítanak hogyan toouse Twilio szolgáltatásokhoz [Twilio-könyvtár a PHP a Githubról][twilio_php].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-158">While Twilio services are REST-based and can be called from PHP in several ways, this article will focus on how toouse Twilio services with [Twilio library for PHP from GitHub][twilio_php].</span></span> <span data-ttu-id="0f8ba-159">Php-hez tartozó hello Twilio-könyvtár használatával kapcsolatos további információkért lásd: [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-159">For more information about using hello Twilio library for PHP, see [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="0f8ba-160">Részletes útmutatást létrehozása és telepítése a Twilio/PHP-alkalmazás tooAzure érhetők el [hogyan tooMake a PHP-alkalmazások az Azure-on a telefonhívás használatával Twilio][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-160">Detailed instructions for building and deploying a Twilio/PHP application tooAzure are available at [How tooMake a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="0f8ba-161"><a id="configure_app"></a>Alkalmazás tooUse Twilio szalagtárak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0f8ba-161"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="0f8ba-162">Az alkalmazás toouse hello Twilio-könyvtár PHP kétféleképpen konfigurálható:</span><span class="sxs-lookup"><span data-stu-id="0f8ba-162">You can configure your application toouse hello Twilio library for PHP in two ways:</span></span>

1. <span data-ttu-id="0f8ba-163">Töltse le a Githubról PHP hello Twilio-könyvtár ([https://github.com/twilio/twilio-php][twilio_php]), és adja hozzá a hello **szolgáltatások** directory tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-163">Download hello Twilio library for PHP from GitHub ([https://github.com/twilio/twilio-php][twilio_php]) and add hello **Services** directory tooyour application.</span></span>
   
    <span data-ttu-id="0f8ba-164">-VAGY-</span><span class="sxs-lookup"><span data-stu-id="0f8ba-164">-OR-</span></span>
2. <span data-ttu-id="0f8ba-165">Telepítse a PHP hello Twilio könyvtár KÖRTEFÁK csomag.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-165">Install hello Twilio library for PHP as a PEAR package.</span></span> <span data-ttu-id="0f8ba-166">A következő parancsok hello telepíthető:</span><span class="sxs-lookup"><span data-stu-id="0f8ba-166">It can be installed with hello following commands:</span></span>
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

<span data-ttu-id="0f8ba-167">Miután telepítette a hello Twilio könyvtár php, amelyet ezután felvehet egy **require_once** utasítás a PHP hello tetején fájlok tooreference hello könyvtár:</span><span class="sxs-lookup"><span data-stu-id="0f8ba-167">Once you have installed hello Twilio library for PHP, you can then add a **require_once** statement at hello top of your PHP files tooreference hello library:</span></span>

        require_once 'Services/Twilio.php';

<span data-ttu-id="0f8ba-168">További információkért lásd: [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-168">For more information, see [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="0f8ba-169"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="0f8ba-169"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="0f8ba-170">hello következő bemutatja, hogyan egy kimenő toomake hívás, hello használatával **Services_Twilio** osztály.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-170">hello following shows how toomake an outgoing call using hello **Services_Twilio** class.</span></span> <span data-ttu-id="0f8ba-171">Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-171">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="0f8ba-172">Helyettesítse a saját értékeit hello **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszámot a Twilio fiók előzetes toorunning hello kódot.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-172">Substitute your values for hello **From** and **To** phone numbers, and ensure that you verify hello **From** phone number for your Twilio account prior toorunning hello code.</span></span>

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // hello number of hello phone initiating hello hello call.
    $from_number = "NNNNNNNNNNN";

    // hello number of hello phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use hello Twilio-provided site for hello TwiML response.
    $url = "http://twimlets.com/message";

    // hello phone message text.
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make hello call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

<span data-ttu-id="0f8ba-173">Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-173">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="0f8ba-174">Ehelyett használhat saját hely tooprovide hello TwiML válasz; További információkért lásd: [hogyan tooProvide TwiML válaszok a saját webhelyről](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="0f8ba-174">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

* <span data-ttu-id="0f8ba-175">**Megjegyzés:**: tootroubleshoot SSL tanúsítvány érvényesítési hibákat, tekintse meg [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span><span class="sxs-lookup"><span data-stu-id="0f8ba-175">**Note**: tootroubleshoot SSL certificate validation errors, see [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span></span> 

## <span data-ttu-id="0f8ba-176"><a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="0f8ba-176"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="0f8ba-177">hello következő bemutatja, hogyan egy SMS üzenetet használatával toosend hello **Services_Twilio** osztály.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-177">hello following shows how toosend an SMS message using hello **Services_Twilio** class.</span></span> <span data-ttu-id="0f8ba-178">Hello **a** szám Twilio biztosítja a próbaverzió fiókok toosend SMS-t.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-178">hello **From** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="0f8ba-179">Hello **való** szám ellenőrizni kell a Twilio fiók előzetes toorunning hello kódot.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-179">hello **To** number must be verified for your Twilio account prior toorunning hello code.</span></span>

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send hello SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <span data-ttu-id="0f8ba-180"><a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok</span><span class="sxs-lookup"><span data-stu-id="0f8ba-180"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="0f8ba-181">Ha az alkalmazás egy hívás toohello Twilio API, Twilio elküldi a kérelem tooa URL-CÍMÉT, amely tooreturn TwiML választ várt.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-181">When your application initiates a call toohello Twilio API, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="0f8ba-182">hello a fenti példában hello Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-182">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="0f8ba-183">(Közben TwiML a Twilio végzi, megtekintheti a böngészőben hello azt.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-183">(While TwiML is designed for use by Twilio, you can view hello it in your browser.</span></span> <span data-ttu-id="0f8ba-184">Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres `<Response>` elem; másik példaként, kattintson a [http://twimlets.com/message? % 5B0 üzenet %5 D Hello % 20World =] [ twimlet_message_url_hello_world] toosee egy `<Response>` elem, amely tartalmazza egy `<Say>` elem.)</span><span class="sxs-lookup"><span data-stu-id="0f8ba-184">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] toosee a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="0f8ba-185">Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját webhelyén, amely HTTP-válaszokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-185">Instead of relying on hello Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="0f8ba-186">Hello helyet létrehozhat bármilyen nyelven, amely visszaadja az XML-válaszok; Ez a témakör azt feltételezi, hogy a PHP toocreate hello TwiML fogja használni.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-186">You can create hello site in any language that returns XML responses; this topic assumes you'll be using PHP toocreate hello TwiML.</span></span>

<span data-ttu-id="0f8ba-187">a PHP lap eredmények TwiML választ, amely szerint a következő hello **Hello World** hello hívásakor.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-187">hello following PHP page results in a TwiML response that says **Hello World** on hello call.</span></span>

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

<span data-ttu-id="0f8ba-188">Ahogy látja, a fenti példa hello, hello TwiML válasz egyszerűen egy XML-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-188">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="0f8ba-189">hello Twilio php könyvtárban osztályokkal rendelkezik, amelyek az Ön TwiML hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-189">hello Twilio library for PHP contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="0f8ba-190">az alábbi példa hello hello egyenértékű válasz eredményez, ahogy fent látható, de hello használ **szolgáltatások\_Twilio\_Twiml** osztály php hello Twilio könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="0f8ba-190">hello example below produces hello equivalent response as shown above, but uses hello **Services\_Twilio\_Twiml** class in hello Twilio library for PHP:</span></span>

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

<span data-ttu-id="0f8ba-191">TwiML kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-191">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span> 

<span data-ttu-id="0f8ba-192">Miután a tooprovide TwiML válaszok beállítása a PHP oldal, használja hello PHP lap URL-címe hello hello hello átadott URL-cím `Services_Twilio->account->calls->create` metódust.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-192">Once you have your PHP page set up tooprovide TwiML responses, use hello URL of hello PHP page as hello URL passed into hello  `Services_Twilio->account->calls->create`  method.</span></span> <span data-ttu-id="0f8ba-193">Ha egy webes alkalmazás neve például **MyTwiML** telepített tooan Azure üzemeltetett szolgáltatás, és hello neve hello PHP lap **mytwiml.php**, URL-címe túl átadhatók hello **Services_ Twilio -> fiók -> hívások -> hozzon létre** a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="0f8ba-193">For example, if you have a Web application named **MyTwiML** deployed tooan Azure hosted service, and hello name of hello PHP page is **mytwiml.php**, hello URL can be passed too **Services_Twilio->account->calls->create**  as shown in hello following example:</span></span>

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // hello phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

<span data-ttu-id="0f8ba-194">Az Azure-ban PHP Twilio használatával kapcsolatos további információkért lásd: [hogyan tooMake a PHP-alkalmazások az Azure-on a telefonhívás használatával Twilio][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-194">For additional information about using Twilio in Azure with PHP, see [How tooMake a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="0f8ba-195"><a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="0f8ba-195"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="0f8ba-196">Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0f8ba-196">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="0f8ba-197">Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="0f8ba-197">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="0f8ba-198"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f8ba-198"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="0f8ba-199">Most, hogy megismerte a Twilio szolgáltatáshoz hello hello alapjait, kövesse a további hivatkozások toolearn:</span><span class="sxs-lookup"><span data-stu-id="0f8ba-199">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="0f8ba-200">[Twilio biztonsági irányelvek][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="0f8ba-200">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="0f8ba-201">[Twilio útmutató és példakódot][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="0f8ba-201">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="0f8ba-202">[Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="0f8ba-202">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="0f8ba-203">[A Githubon Twilio][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="0f8ba-203">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="0f8ba-204">[Forduljon a támogatási tooTwilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="0f8ba-204">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
