---
title: "Hang-és SMS (Ruby) Twilio használata |} Microsoft Docs"
description: "Útmutató a telefonhívás, és a Twilio API szolgáltatás SMS üzenet küldése az Azure-on. Kódminták Ruby."
services: 
documentationcenter: ruby
author: devinrader
manager: twilio
editor: 
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 69e50e7fe0e1f302e96c309878b8dea6869dff4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a><span data-ttu-id="c3006-104">Hang-és SMS képességei Ruby Twilio használata</span><span class="sxs-lookup"><span data-stu-id="c3006-104">How to Use Twilio for Voice and SMS Capabilities in Ruby</span></span>
<span data-ttu-id="c3006-105">Ez az útmutató bemutatja, hogyan Azure a Twilio API szolgáltatás közös programozási feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="c3006-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="c3006-106">A tárgyalt forgatókönyvekben szerepel, így a telefonhívás, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="c3006-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="c3006-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: a [lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c3006-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="c3006-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="c3006-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="c3006-109">Twilio egy telefonos webszolgáltatás API, amely lehetővé teszi, hogy a meglévő webes nyelv és képességeik felhasználásával hang- és SMS alkalmazások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c3006-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills to build voice and SMS applications.</span></span> <span data-ttu-id="c3006-110">Twilio egy olyan külső szolgáltatás (nem az Azure szolgáltatásai és nem Microsoft-termékek).</span><span class="sxs-lookup"><span data-stu-id="c3006-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="c3006-111">**Twilio hang** lehetővé teszi, hogy az alkalmazások és a telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="c3006-111">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="c3006-112">**Twilio SMS** lehetővé teszi, hogy az alkalmazások és az SMS-üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="c3006-112">**Twilio SMS** allows your applications to make and receive SMS messages.</span></span> <span data-ttu-id="c3006-113">**Twilio-ügyfél** lehetővé teszi, hogy a meglévő internetes kapcsolattal, többek között a mobil kapcsolatok hang kommunikáció engedélyezése az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c3006-113">**Twilio Client** allows your applications to enable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="c3006-114"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="c3006-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="c3006-115">Információk a díjszabásról Twilio érhető el: [Twilio árképzési][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="c3006-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="c3006-116">Az Azure-ügyfelek egy [a különleges ajánlat][special_offer]: 1000 szövegek szabad követel vagy bejövő perc 1000.</span><span class="sxs-lookup"><span data-stu-id="c3006-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="c3006-117">Iratkozzon fel a szolgáltatásokat, vagy további információért látogasson el [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="c3006-117">To sign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>  

## <span data-ttu-id="c3006-118"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="c3006-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="c3006-119">A Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="c3006-119">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="c3006-120">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="c3006-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

### <span data-ttu-id="c3006-121"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="c3006-121"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="c3006-122">TwiML olyan XML-alapú utasításokat, amelyek tájékoztatják a Twilio hogyan lehet feldolgozni a hívást vagy SMS készlete.</span><span class="sxs-lookup"><span data-stu-id="c3006-122">TwiML is a set of XML-based instructions that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="c3006-123">Tegyük fel, a következő TwiML a szöveg volna átalakítása **Hello World** beszéddé.</span><span class="sxs-lookup"><span data-stu-id="c3006-123">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="c3006-124">Minden TwiML dokumentum rendelkezik `<Response>` , mint a legfelső szintű elem.</span><span class="sxs-lookup"><span data-stu-id="c3006-124">All TwiML documents have `<Response>` as their root element.</span></span> <span data-ttu-id="c3006-125">Ott segítségével Twilio-műveletek az alkalmazás viselkedésének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="c3006-125">From there, you use Twilio Verbs to define the behavior of your application.</span></span>

### <span data-ttu-id="c3006-126"><a id="Verbs"></a>TwiML műveletek</span><span class="sxs-lookup"><span data-stu-id="c3006-126"><a id="Verbs"></a>TwiML Verbs</span></span>
<span data-ttu-id="c3006-127">Twilio-műveletek a következők: XML-címkék, hogy a Twilio arról, mit kell **tegye**.</span><span class="sxs-lookup"><span data-stu-id="c3006-127">Twilio Verbs are XML tags that tell Twilio what to **do**.</span></span> <span data-ttu-id="c3006-128">Például a  **&lt;szóbeli&gt;**  parancs utasítja a Twilio hallhatóan hívás az üzenet kézbesítését.</span><span class="sxs-lookup"><span data-stu-id="c3006-128">For example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span> 

<span data-ttu-id="c3006-129">A Twilio-műveletek listáját a következő:</span><span class="sxs-lookup"><span data-stu-id="c3006-129">The following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="c3006-130">**&lt;Telefonos kapcsolat&gt;**: a hívó kapcsolódik egy másik telefonon.</span><span class="sxs-lookup"><span data-stu-id="c3006-130">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="c3006-131">**&lt;Gyűjtsön&gt;**: adta meg a telefon billentyűzetén számjegyek gyűjti.</span><span class="sxs-lookup"><span data-stu-id="c3006-131">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="c3006-132">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="c3006-132">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="c3006-133">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="c3006-133">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="c3006-134">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="c3006-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="c3006-135">**&lt;Rekord&gt;**: a hívó hang rögzíti, és a felvétel tartalmazó fájl URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c3006-135">**&lt;Record&gt;**: Records the caller's voice and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="c3006-136">**&lt;Átirányítási&gt;**: hívás vagy SMS irányítását átviszi a TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="c3006-136">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="c3006-137">**&lt;Elutasítása&gt;**: a Twilio-szám egy bejövő hívás elutasítása a nélkül, számlázási</span><span class="sxs-lookup"><span data-stu-id="c3006-137">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you</span></span>
* <span data-ttu-id="c3006-138">**&lt;Tegyük fel például&gt;**: konvertálja szöveg-beszéd átalakítás hívás készült.</span><span class="sxs-lookup"><span data-stu-id="c3006-138">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="c3006-139">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="c3006-139">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

<span data-ttu-id="c3006-140">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="c3006-140">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="c3006-141">A Twilio API-val kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="c3006-141">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="c3006-142"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3006-142"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="c3006-143">Amikor készen áll arra, hogy a Twilio-fiókot, feliratkozhat [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="c3006-143">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="c3006-144">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="c3006-144">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="c3006-145">Amikor regisztrál egy Twilio-fiókot, később is lesz egy ingyenes telefonszámot az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c3006-145">When you sign up for a Twilio account, you'll get a free phone number for your application.</span></span> <span data-ttu-id="c3006-146">Egy fiók SID azonosítója és egy hitelesítési jogkivonatot is kap.</span><span class="sxs-lookup"><span data-stu-id="c3006-146">You'll also receive an account SID and an auth token.</span></span> <span data-ttu-id="c3006-147">Is szükség lesz Twilio API-hívások indítása.</span><span class="sxs-lookup"><span data-stu-id="c3006-147">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="c3006-148">Jogosulatlan hozzáférés elkerülése érdekében a fiókját, hogy a hitelesítési jogkivonat biztonságának megőrzése.</span><span class="sxs-lookup"><span data-stu-id="c3006-148">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="c3006-149">A fiók SID azonosítója és a hitelesítési jogkivonat is látható, a [Twilio fióklapját][twilio_account], a mezőket, címkével **fiók SID** és **hitelesítési JOGKIVONAT**, kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="c3006-149">Your account SID and auth token are viewable at the [Twilio account page][twilio_account], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

### <span data-ttu-id="c3006-150"><a id="VerifyPhoneNumbers"></a>Ellenőrizze a telefonszámok</span><span class="sxs-lookup"><span data-stu-id="c3006-150"><a id="VerifyPhoneNumbers"></a>Verify Phone Numbers</span></span>
<span data-ttu-id="c3006-151">A szám kívül lehetősége van által Twilio, azt is ellenőrizheti, hogy legyen az alkalmazások által vezérelt (azaz a mobiltelefon vagy otthoni telefonszámot) számok.</span><span class="sxs-lookup"><span data-stu-id="c3006-151">In addition to the number you are given by Twilio, you can also verify numbers that you control (i.e. your cell phone or home phone number) for use in your applications.</span></span> 

<span data-ttu-id="c3006-152">Hogyan lehet ellenőrizni a telefonszám további információkért lásd: [kezelése számok][verify_phone].</span><span class="sxs-lookup"><span data-stu-id="c3006-152">For information on how to verify a phone number, see [Manage Numbers][verify_phone].</span></span>

## <span data-ttu-id="c3006-153"><a id="create_app"></a>Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3006-153"><a id="create_app"></a>Create a Ruby Application</span></span>
<span data-ttu-id="c3006-154">Egy Ruby alkalmazás, amely a Twilio-szolgáltatás és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más Ruby használó alkalmazások a Twilio szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c3006-154">A Ruby application that uses the Twilio service and is running in Azure is no different than any other Ruby application that uses the Twilio service.</span></span> <span data-ttu-id="c3006-155">Amíg Twilio-szolgáltatások RESTful, és a Ruby többféleképpen hívható, ez a cikk a Twilio-szolgáltatások használatával összpontosítanak [Twilio segítő kódtára a Rubyhoz][twilio_ruby].</span><span class="sxs-lookup"><span data-stu-id="c3006-155">While Twilio services are RESTful and can be called from Ruby in several ways, this article will focus on how to use Twilio services with [Twilio helper library for Ruby][twilio_ruby].</span></span>

<span data-ttu-id="c3006-156">Első, [beállításról egy új Azure Linux virtuális gép] [ azure_vm_setup] új Ruby webes alkalmazás állomás nevében járhasson el.</span><span class="sxs-lookup"><span data-stu-id="c3006-156">First, [set-up a new Azure Linux VM][azure_vm_setup] to act as a host for your new Ruby web application.</span></span> <span data-ttu-id="c3006-157">Hagyja figyelmen kívül használata esetén egy sínek alkalmazáshoz, csak beállításról a virtuális gép létrehozásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="c3006-157">Ignore the steps involving the creation of a Rails app, just set-up the VM.</span></span> <span data-ttu-id="c3006-158">Ellenőrizze, hogy egy külső port a 80-as és egy belső portjával 5000 hozzon létre egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="c3006-158">Make sure you create an Endpoint with an external port of 80 and an internal port of 5000.</span></span>

<span data-ttu-id="c3006-159">Az alábbi példákban fogjuk használni [Sinatra][sinatra], egy nagyon egyszerű webes keretrendszer, a Ruby.</span><span class="sxs-lookup"><span data-stu-id="c3006-159">In the examples below, we will be using [Sinatra][sinatra], a very simple web framework for Ruby.</span></span> <span data-ttu-id="c3006-160">De alapértékekkel használata a Twilio-segédkódtárba helyezni a Rubyhoz bármely más webes keretrendszer, többek között a Ruby sínen.</span><span class="sxs-lookup"><span data-stu-id="c3006-160">But you can certainly use the Twilio helper library for Ruby with any other web framework, including Ruby on Rails.</span></span>

<span data-ttu-id="c3006-161">SSH-ból az új virtuális gép, és hozzon létre egy könyvtárat, az új alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c3006-161">SSH into your new VM and create a directory for your new app.</span></span> <span data-ttu-id="c3006-162">Hozzon létre egy Gemfile nevű fájlt, és a következő kódot bemásolhatja belül könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="c3006-162">Inside that directory, create a file called Gemfile and copy the following code into it:</span></span>

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

<span data-ttu-id="c3006-163">A parancssorban futtassa `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="c3006-163">On the command line run `bundle install`.</span></span> <span data-ttu-id="c3006-164">Ez telepíti az alábbi függőségeket.</span><span class="sxs-lookup"><span data-stu-id="c3006-164">This will install the dependencies above.</span></span> <span data-ttu-id="c3006-165">Ezután hozzon létre egy nevű fájlt `web.rb`.</span><span class="sxs-lookup"><span data-stu-id="c3006-165">Next create a file called `web.rb`.</span></span> <span data-ttu-id="c3006-166">Ez lesz, ahol a kódot a webalkalmazás él.</span><span class="sxs-lookup"><span data-stu-id="c3006-166">This will be where the code for your web app lives.</span></span> <span data-ttu-id="c3006-167">Az alábbi kód beillesztése azt:</span><span class="sxs-lookup"><span data-stu-id="c3006-167">Paste the following code into it:</span></span>

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

<span data-ttu-id="c3006-168">Ekkor meg kell majd futtassa a parancsot `ruby web.rb -p 5000`.</span><span class="sxs-lookup"><span data-stu-id="c3006-168">At this point you should be able the run the command `ruby web.rb -p 5000`.</span></span> <span data-ttu-id="c3006-169">Ez fogja léptetéses-legfeljebb 5000-es port kis webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c3006-169">This will spin-up a small web server on port 5000.</span></span> <span data-ttu-id="c3006-170">Meg kell tudni keresse ki ezt az alkalmazást a böngészőben az URL-cím felkeresésével meg beállításról az Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c3006-170">You should be able to browse to this app in your browser by visiting the URL you set-up for your Azure VM.</span></span> <span data-ttu-id="c3006-171">A webalkalmazás a böngészőben érheti el, ha készen áll a Twilio-alkalmazások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c3006-171">Once you can reach your web app in the browser, you're ready to start building a Twilio app.</span></span>

## <span data-ttu-id="c3006-172"><a id="configure_app"></a>Állítsa be az alkalmazását Twilio használata</span><span class="sxs-lookup"><span data-stu-id="c3006-172"><a id="configure_app"></a>Configure Your Application to Use Twilio</span></span>
<span data-ttu-id="c3006-173">Beállíthatja, hogy a Twilio-könyvtár használata frissítésével a webalkalmazás a `Gemfile` ezt a sort tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="c3006-173">You can configure your web app to use the Twilio library by updating your `Gemfile` to include this line:</span></span>

    gem 'twilio-ruby'

<span data-ttu-id="c3006-174">A parancssorban futtassa `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="c3006-174">On the command line, run `bundle install`.</span></span> <span data-ttu-id="c3006-175">Most nyissa meg a `web.rb` és a sor tetején, így:</span><span class="sxs-lookup"><span data-stu-id="c3006-175">Now open `web.rb` and including this line at the top:</span></span>

    require 'twilio-ruby'

<span data-ttu-id="c3006-176">Most már készen is szeretné használni a Twilio-segédkódtárba helyezni a Ruby a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c3006-176">You're now all set to use the Twilio helper library for Ruby in your web app.</span></span>

## <span data-ttu-id="c3006-177"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="c3006-177"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="c3006-178">A következő bemutatja, hogyan végezhet.</span><span class="sxs-lookup"><span data-stu-id="c3006-178">The following shows how to make an outgoing call.</span></span> <span data-ttu-id="c3006-179">Alapfogalmak közé tartozik a REST API-hívások indítása a Twilio segítő kódtára a Rubyhoz segítségével, és TwiML megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="c3006-179">Key concepts include using the Twilio helper library for Ruby to make REST API calls and rendering TwiML.</span></span> <span data-ttu-id="c3006-180">Helyettesítse a saját értékeit a **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a **a** telefonszám a Twilio-fiók a kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="c3006-180">Substitute your values for the **From** and **To** phone numbers, and ensure that you verify the **From** phone number for your Twilio account prior to running the code.</span></span>

<span data-ttu-id="c3006-181">Adja hozzá ezt a funkciót `web.md`:</span><span class="sxs-lookup"><span data-stu-id="c3006-181">Add this function to `web.md`:</span></span>

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

<span data-ttu-id="c3006-182">Ha Ön nyílt felfelé `http://yourdomain.cloudapp.net/make_call` egy böngészőben, amely akkor indul el, a telefonhívás a Twilio API hívása.</span><span class="sxs-lookup"><span data-stu-id="c3006-182">If you open-up `http://yourdomain.cloudapp.net/make_call` in a browser, that will trigger the call to the Twilio API to make the phone call.</span></span> <span data-ttu-id="c3006-183">Az első két paraméterek `client.account.calls.create` magától értetődő viszonylag: száma a hívás nem `from` és szám a hívás nem `to`.</span><span class="sxs-lookup"><span data-stu-id="c3006-183">The first two parameters in `client.account.calls.create` are fairly self-explanatory: the number the call is `from` and the number the call is `to`.</span></span> 

<span data-ttu-id="c3006-184">A harmadik paraméter (`url`) az URL-cím, amelyet Twilio utasításokat hozzáférhet Mi a teendő, ha a csatlakozás igényel.</span><span class="sxs-lookup"><span data-stu-id="c3006-184">The third parameter (`url`) is the URL that Twilio requests to get instructions on what to do once the call is connected.</span></span> <span data-ttu-id="c3006-185">Ebben az esetben azt URL-címet a telepítés (`http://yourdomain.cloudapp.net`), amely egy egyszerű TwiML dokumentumot ad vissza, és használja a `<Say>` hajtsa végre az egyes szöveg-beszéd átalakítás, és mondja ki a "Hello, Monkey" annak a személynek a hívás fogadása.</span><span class="sxs-lookup"><span data-stu-id="c3006-185">In this case we set-up a URL (`http://yourdomain.cloudapp.net`) that returns a simple TwiML document and uses the `<Say>` verb to do some text-to-speech and say "Hello Monkey" to the person recieving the call.</span></span>

## <span data-ttu-id="c3006-186"><a id="howto_recieve_sms"></a>Útmutató: az SMS-üzenet fogadása</span><span class="sxs-lookup"><span data-stu-id="c3006-186"><a id="howto_recieve_sms"></a>How to: Recieve an SMS message</span></span>
<span data-ttu-id="c3006-187">Az előző példában a Microsoft által kezdeményezett egy **kimenő** telefonhívás.</span><span class="sxs-lookup"><span data-stu-id="c3006-187">In the previous example we initiated an **outgoing** phone call.</span></span> <span data-ttu-id="c3006-188">Ez alkalommal, most használja a telefonszámot, amelyet a Twilio megadott során regisztrációs folyamat egy **bejövő** SMS-üzenet.</span><span class="sxs-lookup"><span data-stu-id="c3006-188">This time, let's use the phone number that Twilio gave us during sign-up to process an **incoming** SMS message.</span></span>

<span data-ttu-id="c3006-189">Első, jelentkezzen be a [Twilio-irányítópult][twilio_account].</span><span class="sxs-lookup"><span data-stu-id="c3006-189">First, log-in to your [Twilio dashboard][twilio_account].</span></span> <span data-ttu-id="c3006-190">Kattintson a "Numbers" a felső navigációs, és kattintson a Twilio számára lett megadva.</span><span class="sxs-lookup"><span data-stu-id="c3006-190">Click on "Numbers" in the top nav and then click on the Twilio number you were provided.</span></span> <span data-ttu-id="c3006-191">Látni fogja, két URL-címeket, amelyeket konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="c3006-191">You'll see two URLs that you can configure.</span></span> <span data-ttu-id="c3006-192">Egy hang kérelem URL-CÍMÉT és az SMS kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c3006-192">A Voice Request URL and an SMS Request URL.</span></span> <span data-ttu-id="c3006-193">Ezek a Twilio meghívja a phone kezdeményezték, és az SMS a rendszer elküldi a használni kívánt URL-címek.</span><span class="sxs-lookup"><span data-stu-id="c3006-193">These are the URLs that Twilio calls whenever a phone call is made or an SMS is sent to your number.</span></span> <span data-ttu-id="c3006-194">Az URL-címek "webes hurkok" is nevezik.</span><span class="sxs-lookup"><span data-stu-id="c3006-194">The URLs are also known as "web hooks".</span></span>

<span data-ttu-id="c3006-195">Bejövő SMS-üzenetek feldolgozásához, ezért frissíti az URL-CÍMÉT kell `http://yourdomain.cloudapp.net/sms_url`.</span><span class="sxs-lookup"><span data-stu-id="c3006-195">We would like to process incoming SMS messages, so let's update the URL to `http://yourdomain.cloudapp.net/sms_url`.</span></span> <span data-ttu-id="c3006-196">Lépjen tovább, és kattintson a módosítások mentése a lap alján.</span><span class="sxs-lookup"><span data-stu-id="c3006-196">Go ahead and click Save Changes at the bottom of the page.</span></span> <span data-ttu-id="c3006-197">Most, újból `web.rb` ennek kezelése az alkalmazás most program:</span><span class="sxs-lookup"><span data-stu-id="c3006-197">Now, back in `web.rb` let's program our application to handle this:</span></span>

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

<span data-ttu-id="c3006-198">A módosítás után győződjön meg arról, hogy indítsa újra a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c3006-198">After making the change, make sure to re-start your web app.</span></span> <span data-ttu-id="c3006-199">Most vegye ki a telefonjára, és a Twilio-szám SMS küldése.</span><span class="sxs-lookup"><span data-stu-id="c3006-199">Now, take out your phone and send an SMS to your Twilio number.</span></span> <span data-ttu-id="c3006-200">Azonnal kapja meg az SMS-válasz, amely szerint "Hey, a ping Köszönjük!</span><span class="sxs-lookup"><span data-stu-id="c3006-200">You should promptly get an SMS response that says "Hey, thanks for the ping!</span></span> <span data-ttu-id="c3006-201">Twilio és az Azure rock! ".</span><span class="sxs-lookup"><span data-stu-id="c3006-201">Twilio and Azure rock!".</span></span>

## <span data-ttu-id="c3006-202"><a id="additional_services"></a>Útmutató: további Twilio-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="c3006-202"><a id="additional_services"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="c3006-203">Az itt bemutatott példák Twilio lehetőséget biztosít webes API-k segítségével kihasználhatja az Azure alkalmazásról további Twilio-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="c3006-203">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="c3006-204">Teljes részletekért lásd: a [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="c3006-204">For full details, see the [Twilio API documentation][twilio_api_documentation].</span></span>

### <span data-ttu-id="c3006-205"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3006-205"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="c3006-206">Most, hogy megismerte a Twilio szolgáltatáshoz alapjait, az alábbi hivatkozásokból további:</span><span class="sxs-lookup"><span data-stu-id="c3006-206">Now that you've learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="c3006-207">[Twilio biztonsági irányelvek][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="c3006-207">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="c3006-208">[Twilio HowTos és példakódot][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="c3006-208">[Twilio HowTos and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="c3006-209">[Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="c3006-209">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="c3006-210">[A Githubon Twilio][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="c3006-210">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="c3006-211">[Kérdezze meg a Twilio-támogatás][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="c3006-211">[Talk to Twilio Support][twilio_support]</span></span>

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
