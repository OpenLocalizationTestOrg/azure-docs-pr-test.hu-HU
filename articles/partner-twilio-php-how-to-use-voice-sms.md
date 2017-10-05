---
title: "Hang-és SMS (PHP) Twilio használata |} Microsoft Docs"
description: "Útmutató a telefonhívás, és a Twilio API szolgáltatás SMS üzenet küldése az Azure-on. Kódminták PHP."
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
ms.openlocfilehash: bd50eac7390e8639f77894689388e6926cdb619c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a><span data-ttu-id="568e5-104">Hang-és a PHP SMS lehetővé tevő szolgáltatásaival Twilio használata</span><span class="sxs-lookup"><span data-stu-id="568e5-104">How to Use Twilio for Voice and SMS Capabilities in PHP</span></span>
<span data-ttu-id="568e5-105">Ez az útmutató bemutatja, hogyan Azure a Twilio API szolgáltatás közös programozási feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="568e5-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="568e5-106">A tárgyalt forgatókönyvekben szerepel, így a telefonhívás, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="568e5-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="568e5-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: a [lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="568e5-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="568e5-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="568e5-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="568e5-109">Twilio a jövőbeli üzleti kommunikáció, így a fejlesztők hang, a VoIP és a üzenetküldési alkalmazásokba történő beágyazásához van működtetéséhez.</span><span class="sxs-lookup"><span data-stu-id="568e5-109">Twilio is powering the future of business communications, enabling developers to embed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="568e5-110">Ezek virtualizálása keresztül a Twilio API kommunikációs platform, az ilyen felhőalapú, a globális környezetben szükséges összes infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="568e5-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through the Twilio communications API platform.</span></span> <span data-ttu-id="568e5-111">Alkalmazások olyan egyszerű hozhat létre és méretezhető.</span><span class="sxs-lookup"><span data-stu-id="568e5-111">Applications are simple to build and scalable.</span></span> <span data-ttu-id="568e5-112">Élvezze a rugalmasságot Önnel fizetési-, nyissa meg árképzési, és igénybe vehesse az felhő megbízhatóság.</span><span class="sxs-lookup"><span data-stu-id="568e5-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="568e5-113">**Twilio hang** lehetővé teszi, hogy az alkalmazások és a telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="568e5-113">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="568e5-114">**Twilio SMS** lehetővé teszi, hogy az alkalmazás szöveges üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="568e5-114">**Twilio SMS** enables your application to send and receive text messages.</span></span> <span data-ttu-id="568e5-115">**Twilio-ügyfél** lehetővé teszi a VoIP hívást a telefon, a táblagép vagy a böngésző és WebRTC támogatja.</span><span class="sxs-lookup"><span data-stu-id="568e5-115">**Twilio Client** allows you to make VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="568e5-116"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="568e5-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="568e5-117">Az Azure-ügyfelek egy [a különleges ajánlat](http://www.twilio.com/azure): udvarias $10 Twilio-kredit a Twilio-fiók frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="568e5-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="568e5-118">A Twilio-jóváírási alkalmazhatók minden Twilio-használati (akár 1000 SMS-üzenetek küldésekor vagy fogadásakor 1000 bejövő hang perc, attól függően, hogy a telefon és üzenet vagy hívás cél helye egyenértékűek $10 követel).</span><span class="sxs-lookup"><span data-stu-id="568e5-118">This Twilio Credit can be applied to any Twilio usage ($10 credit equivalent to sending as many as 1,000 SMS messages or receiving up to 1000 inbound Voice minutes, depending on the location of your phone number and message or call destination).</span></span> <span data-ttu-id="568e5-119">A Twilio-jóváírási beváltani és első lépések: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="568e5-119">Redeem this Twilio credit and get started at: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="568e5-120">Twilio egy olyan használatalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="568e5-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="568e5-121">Nincsenek nem beállításról díjakat, és bármikor bezárhatja a fiókját.</span><span class="sxs-lookup"><span data-stu-id="568e5-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="568e5-122">További részletei: [Twilio árképzési][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="568e5-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="568e5-123"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="568e5-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="568e5-124">A Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="568e5-124">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="568e5-125">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="568e5-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="568e5-126">A Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="568e5-126">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="568e5-127"><a id="Verbs"></a>Twilio-műveletek</span><span class="sxs-lookup"><span data-stu-id="568e5-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="568e5-128">Az API lehetővé teszi, hogy a Twilio használja műveletek; például a  **&lt;szóbeli&gt;**  parancs utasítja a Twilio hallhatóan hívás az üzenet kézbesítését.</span><span class="sxs-lookup"><span data-stu-id="568e5-128">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="568e5-129">A Twilio-műveletek listáját a következő:</span><span class="sxs-lookup"><span data-stu-id="568e5-129">The following is a list of Twilio verbs.</span></span> <span data-ttu-id="568e5-130">További információk az egyéb műveletek és képességek keresztül [Twilio Markup Language dokumentáció](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="568e5-130">Learn about the other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="568e5-131">**&lt;Telefonos kapcsolat&gt;**: a hívó kapcsolódik egy másik telefonon.</span><span class="sxs-lookup"><span data-stu-id="568e5-131">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="568e5-132">**&lt;Gyűjtsön&gt;**: adta meg a telefon billentyűzetén számjegyek gyűjti.</span><span class="sxs-lookup"><span data-stu-id="568e5-132">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="568e5-133">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="568e5-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="568e5-134">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="568e5-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="568e5-135">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="568e5-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="568e5-136">**&lt;Rekord&gt;**: a hívó hang rögzíti, és a felvétel tartalmazó fájl URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="568e5-136">**&lt;Record&gt;**: Records the caller's voice and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="568e5-137">**&lt;Átirányítási&gt;**: hívás vagy SMS irányítását átviszi a TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="568e5-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="568e5-138">**&lt;Elutasítása&gt;**: a Twilio-szám egy bejövő hívás elutasítása a nélkül, számlázási</span><span class="sxs-lookup"><span data-stu-id="568e5-138">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you</span></span>
* <span data-ttu-id="568e5-139">**&lt;Tegyük fel például&gt;**: konvertálja szöveg-beszéd átalakítás hívás készült.</span><span class="sxs-lookup"><span data-stu-id="568e5-139">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="568e5-140">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="568e5-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="568e5-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="568e5-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="568e5-142">TwiML olyan XML-alapú utasítások a Twilio-műveletek, amely tájékoztatja a Twilio hogyan lehet feldolgozni a hívást vagy SMS alapján.</span><span class="sxs-lookup"><span data-stu-id="568e5-142">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="568e5-143">Tegyük fel, a következő TwiML a szöveg volna átalakítása **Hello World** beszéddé.</span><span class="sxs-lookup"><span data-stu-id="568e5-143">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="568e5-144">Ha egy alkalmazás meghívja a Twilio API-t, az API paraméterek egyike az URL-címet, a TwiML választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="568e5-144">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="568e5-145">Fejlesztési célra a Twilio által megadott URL-címek segítségével az alkalmazások által használt TwiML visszajelzést.</span><span class="sxs-lookup"><span data-stu-id="568e5-145">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="568e5-146">Akkor is elhelyezheti a TwiML válaszok létrehozásához a saját URL-címeket, és egy másik lehetőség az **TwiMLResponse** objektum.</span><span class="sxs-lookup"><span data-stu-id="568e5-146">You could also host your own URLs to produce the TwiML responses, and another option is to use the **TwiMLResponse** object.</span></span>

<span data-ttu-id="568e5-147">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="568e5-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="568e5-148">A Twilio API-val kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="568e5-148">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="568e5-149"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="568e5-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="568e5-150">Amikor készen áll arra, hogy a Twilio-fiókot, feliratkozhat [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="568e5-150">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="568e5-151">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="568e5-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="568e5-152">Amikor regisztrál egy Twilio-fiókja, kapni fog egy Fiókazonosító és egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="568e5-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="568e5-153">Is szükség lesz Twilio API-hívások indítása.</span><span class="sxs-lookup"><span data-stu-id="568e5-153">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="568e5-154">Jogosulatlan hozzáférés elkerülése érdekében a fiókját, hogy a hitelesítési jogkivonat biztonságának megőrzése.</span><span class="sxs-lookup"><span data-stu-id="568e5-154">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="568e5-155">A Fiókazonosítót és a hitelesítési jogkivonat megtekinthető a rendszer a [Twilio fióklapját][twilio_account], a mezőket, címkével **fiók SID** és **hitelesítési TOKEN**, illetve.</span><span class="sxs-lookup"><span data-stu-id="568e5-155">Your account ID and authentication token are viewable at the [Twilio account page][twilio_account], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="568e5-156"><a id="create_app"></a>PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="568e5-156"><a id="create_app"></a>Create a PHP Application</span></span>
<span data-ttu-id="568e5-157">A PHP-alkalmazások, hogy a Twilio-szolgáltatás és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más PHP-alkalmazás, hogy a Twilio-szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="568e5-157">A PHP application that uses the Twilio service and is running in Azure is no different than any other PHP application that uses the Twilio service.</span></span> <span data-ttu-id="568e5-158">Twilio-szolgáltatások REST-alapú, és a php-ből többféle módon is nevezik, ez a cikk a Twilio-szolgáltatások használatával összpontosítanak [Twilio-könyvtár a PHP a Githubról][twilio_php].</span><span class="sxs-lookup"><span data-stu-id="568e5-158">While Twilio services are REST-based and can be called from PHP in several ways, this article will focus on how to use Twilio services with [Twilio library for PHP from GitHub][twilio_php].</span></span> <span data-ttu-id="568e5-159">Php-hez tartozó a Twilio-könyvtár használatával kapcsolatos további információkért lásd: [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="568e5-159">For more information about using the Twilio library for PHP, see [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="568e5-160">Részletes útmutatást létrehozása és telepítése az Azure-bA Twilio/PHP-alkalmazások érhetők el [hogyan végezheti el a telefonhívás használatával Twilio a PHP-alkalmazások Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="568e5-160">Detailed instructions for building and deploying a Twilio/PHP application to Azure are available at [How to Make a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="568e5-161"><a id="configure_app"></a>Állítsa be az alkalmazását Twilio-könyvtárak használatára</span><span class="sxs-lookup"><span data-stu-id="568e5-161"><a id="configure_app"></a>Configure Your Application to Use Twilio Libraries</span></span>
<span data-ttu-id="568e5-162">Az alkalmazás a Twilio könyvtár használata php-hez tartozó két módon konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="568e5-162">You can configure your application to use the Twilio library for PHP in two ways:</span></span>

1. <span data-ttu-id="568e5-163">Töltse le a Githubról PHP a Twilio-könyvtár ([https://github.com/twilio/twilio-php][twilio_php]), és adja hozzá a **szolgáltatások** könyvtárhoz, hogy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="568e5-163">Download the Twilio library for PHP from GitHub ([https://github.com/twilio/twilio-php][twilio_php]) and add the **Services** directory to your application.</span></span>
   
    <span data-ttu-id="568e5-164">-VAGY-</span><span class="sxs-lookup"><span data-stu-id="568e5-164">-OR-</span></span>
2. <span data-ttu-id="568e5-165">Telepítse a PHP a Twilio-könyvtár KÖRTEFÁK csomag.</span><span class="sxs-lookup"><span data-stu-id="568e5-165">Install the Twilio library for PHP as a PEAR package.</span></span> <span data-ttu-id="568e5-166">Az alábbi parancsokkal telepíthető:</span><span class="sxs-lookup"><span data-stu-id="568e5-166">It can be installed with the following commands:</span></span>
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

<span data-ttu-id="568e5-167">Miután telepítette a Twilio-könyvtár php, amelyet ezután felvehet egy **require_once** nyilatkozat tetején található a PHP fájlok a könyvtárban hivatkozni:</span><span class="sxs-lookup"><span data-stu-id="568e5-167">Once you have installed the Twilio library for PHP, you can then add a **require_once** statement at the top of your PHP files to reference the library:</span></span>

        require_once 'Services/Twilio.php';

<span data-ttu-id="568e5-168">További információkért lásd: [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="568e5-168">For more information, see [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="568e5-169"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="568e5-169"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="568e5-170">A következő bemutatja, hogyan végezheti el egy kimenő hívás használatával a **Services_Twilio** osztály.</span><span class="sxs-lookup"><span data-stu-id="568e5-170">The following shows how to make an outgoing call using the **Services_Twilio** class.</span></span> <span data-ttu-id="568e5-171">Ezt a kódot a Twilio által biztosított hely is használja a Twilio Markup Language (TwiML) válasz vissza.</span><span class="sxs-lookup"><span data-stu-id="568e5-171">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="568e5-172">Helyettesítse a saját értékeit a **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a **a** telefonszám a Twilio-fiók a kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="568e5-172">Substitute your values for the **From** and **To** phone numbers, and ensure that you verify the **From** phone number for your Twilio account prior to running the code.</span></span>

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
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

<span data-ttu-id="568e5-173">Ahogy azt korábban említettük, ez a kód Twilio által biztosított hely használatával a TwiML választ küldi vissza.</span><span class="sxs-lookup"><span data-stu-id="568e5-173">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="568e5-174">Helyette a saját webhely segítségével adja meg a TwiML válasz; További információkért lásd: [hogyan TwiML válaszok adja meg a saját webhelyről](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="568e5-174">You could instead use your own site to provide the TwiML response; for more information, see [How to Provide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

* <span data-ttu-id="568e5-175">**Megjegyzés:**: SSL-tanúsítvány érvényesítési hibák elhárításához lásd: [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span><span class="sxs-lookup"><span data-stu-id="568e5-175">**Note**: To troubleshoot SSL certificate validation errors, see [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span></span> 

## <span data-ttu-id="568e5-176"><a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="568e5-176"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="568e5-177">A következő bemutatja, hogyan használ egy SMS üzenetet küldeni a **Services_Twilio** osztály.</span><span class="sxs-lookup"><span data-stu-id="568e5-177">The following shows how to send an SMS message using the **Services_Twilio** class.</span></span> <span data-ttu-id="568e5-178">A **a** száma az SMS-üzenetek küldéséhez a próba fiókok Twilio biztosítja.</span><span class="sxs-lookup"><span data-stu-id="568e5-178">The **From** number is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="568e5-179">A **való** számát a Twilio-fiókját a kód futtatása előtt ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="568e5-179">The **To** number must be verified for your Twilio account prior to running the code.</span></span>

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <span data-ttu-id="568e5-180"><a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok</span><span class="sxs-lookup"><span data-stu-id="568e5-180"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="568e5-181">Amikor az alkalmazás a Twilio API hívása kezdeményez, Twilio TwiML választ várt URL-címet a kérést küld.</span><span class="sxs-lookup"><span data-stu-id="568e5-181">When your application initiates a call to the Twilio API, Twilio will send your request to a URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="568e5-182">A fenti példában a Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="568e5-182">The example above uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="568e5-183">(Közben TwiML a Twilio végzi, megtekintheti az informatikai a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="568e5-183">(While TwiML is designed for use by Twilio, you can view the it in your browser.</span></span> <span data-ttu-id="568e5-184">Kattintson például [http://twimlets.com/message] [ twimlet_message_url] egy üres megjelenítéséhez `<Response>` elem; másik példaként, kattintson a [http://twimlets.com/message? % 5B0 üzenet: %5 D = Hello % 20World] [ twimlet_message_url_hello_world] megtekintéséhez egy `<Response>` elem, amely tartalmazza a `<Say>` elem.)</span><span class="sxs-lookup"><span data-stu-id="568e5-184">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] to see a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="568e5-185">Ahelyett, hogy a Twilio által megadott URL-címen, létrehozhat saját webhelyén, amely HTTP-válaszokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="568e5-185">Instead of relying on the Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="568e5-186">A hely bármilyen nyelven, amely visszaadja az XML-válaszok; hozhat létre Ez a témakör azt feltételezi, hogy fogja használni a PHP a TwiML létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="568e5-186">You can create the site in any language that returns XML responses; this topic assumes you'll be using PHP to create the TwiML.</span></span>

<span data-ttu-id="568e5-187">A következő lap a PHP eredményez, amely szerint TwiML választ **Hello World** hívásakor.</span><span class="sxs-lookup"><span data-stu-id="568e5-187">The following PHP page results in a TwiML response that says **Hello World** on the call.</span></span>

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

<span data-ttu-id="568e5-188">Ahogy látja, a fenti példa, TwiML a rendszer a választ csak az XML-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="568e5-188">As you can see from the example above, the TwiML response is simply an XML document.</span></span> <span data-ttu-id="568e5-189">A PHP Twilio-könyvtárban osztályokkal rendelkezik, amelyek az Ön TwiML hoz létre.</span><span class="sxs-lookup"><span data-stu-id="568e5-189">The Twilio library for PHP contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="568e5-190">Az alábbi példa hozza létre a megfelelő választ, ahogy fent látható, de használja a **szolgáltatások\_Twilio\_Twiml** osztály a PHP Twilio könyvtárában:</span><span class="sxs-lookup"><span data-stu-id="568e5-190">The example below produces the equivalent response as shown above, but uses the **Services\_Twilio\_Twiml** class in the Twilio library for PHP:</span></span>

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

<span data-ttu-id="568e5-191">TwiML kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="568e5-191">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span> 

<span data-ttu-id="568e5-192">Miután a TwiML visszajelzést beállítva PHP oldal, használja a PHP lap URL-átadott URL-CÍMÉT a `Services_Twilio->account->calls->create` metódust.</span><span class="sxs-lookup"><span data-stu-id="568e5-192">Once you have your PHP page set up to provide TwiML responses, use the URL of the PHP page as the URL passed into the  `Services_Twilio->account->calls->create`  method.</span></span> <span data-ttu-id="568e5-193">Ha egy webes alkalmazás neve például **MyTwiML** telepített egy Azure üzemeltetett szolgáltatás, és a PHP-oldal neve **mytwiml.php**, az URL-cím adható át **Services_Twilio -> fiók -> hívások -> hozzon létre** a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="568e5-193">For example, if you have a Web application named **MyTwiML** deployed to an Azure hosted service, and the name of the PHP page is **mytwiml.php**, the URL can be passed to  **Services_Twilio->account->calls->create**  as shown in the following example:</span></span>

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
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

<span data-ttu-id="568e5-194">Az Azure-ban PHP Twilio használatával kapcsolatos további információkért lásd: [hogyan végezheti el a telefonhívás használatával Twilio a PHP-alkalmazások Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="568e5-194">For additional information about using Twilio in Azure with PHP, see [How to Make a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="568e5-195"><a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="568e5-195"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="568e5-196">Az itt bemutatott példák Twilio lehetőséget biztosít webes API-k segítségével kihasználhatja az Azure alkalmazásról további Twilio-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="568e5-196">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="568e5-197">Teljes részletekért lásd: a [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="568e5-197">For full details, see the [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="568e5-198"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="568e5-198"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="568e5-199">Most, hogy megismerte a Twilio szolgáltatáshoz alapjait, az alábbi hivatkozásokból további:</span><span class="sxs-lookup"><span data-stu-id="568e5-199">Now that you've learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="568e5-200">[Twilio biztonsági irányelvek][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="568e5-200">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="568e5-201">[Twilio útmutató és példakódot][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="568e5-201">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="568e5-202">[Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="568e5-202">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="568e5-203">[A Githubon Twilio][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="568e5-203">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="568e5-204">[Kérdezze meg a Twilio-támogatás][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="568e5-204">[Talk to Twilio Support][twilio_support]</span></span>

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
