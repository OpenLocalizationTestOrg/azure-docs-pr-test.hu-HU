---
title: "aaaHow tooUse Twilio hang-és SMS (Ruby) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. Kódminták Ruby."
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
ms.openlocfilehash: aca5ccb4663ff03c9c1f39c848469415f06dfb45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-ruby"></a><span data-ttu-id="48a34-104">Hogyan tooUse Twilio hang-és Ruby SMS képességei</span><span class="sxs-lookup"><span data-stu-id="48a34-104">How tooUse Twilio for Voice and SMS Capabilities in Ruby</span></span>
<span data-ttu-id="48a34-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="48a34-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="48a34-106">a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="48a34-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="48a34-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="48a34-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="48a34-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="48a34-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="48a34-109">Twilio egy telefonos webszolgáltatás API, amely lehetővé teszi, hogy a meglévő webes nyelv és képességek toobuild hang- és SMS-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="48a34-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills toobuild voice and SMS applications.</span></span> <span data-ttu-id="48a34-110">Twilio egy olyan külső szolgáltatás (nem az Azure szolgáltatásai és nem Microsoft-termékek).</span><span class="sxs-lookup"><span data-stu-id="48a34-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="48a34-111">**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="48a34-111">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="48a34-112">**Twilio SMS** lehetővé teszi, hogy az alkalmazások toomake és az SMS-üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="48a34-112">**Twilio SMS** allows your applications toomake and receive SMS messages.</span></span> <span data-ttu-id="48a34-113">**Twilio-ügyfél** lehetővé teszi az alkalmazások tooenable hang kommunikációt meglévő hálózati kapcsolatok használata esetén, beleértve a mobil kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="48a34-113">**Twilio Client** allows your applications tooenable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="48a34-114"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="48a34-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="48a34-115">Információk a díjszabásról Twilio érhető el: [Twilio árképzési][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="48a34-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="48a34-116">Az Azure-ügyfelek egy [a különleges ajánlat][special_offer]: 1000 szövegek szabad követel vagy bejövő perc 1000.</span><span class="sxs-lookup"><span data-stu-id="48a34-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="48a34-117">Ez feliratkozott toosign ajánlat vagy további információért, látogasson el a [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="48a34-117">toosign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>  

## <span data-ttu-id="48a34-118"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="48a34-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="48a34-119">hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="48a34-119">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="48a34-120">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="48a34-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

### <span data-ttu-id="48a34-121"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="48a34-121"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="48a34-122">TwiML egy olyan XML-alapú utasításokat, amelyek tájékoztatják, hogy hogyan Twilio tooprocess hívás vagy SMS.</span><span class="sxs-lookup"><span data-stu-id="48a34-122">TwiML is a set of XML-based instructions that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="48a34-123">Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="48a34-123">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="48a34-124">Minden TwiML dokumentum rendelkezik `<Response>` , mint a legfelső szintű elem.</span><span class="sxs-lookup"><span data-stu-id="48a34-124">All TwiML documents have `<Response>` as their root element.</span></span> <span data-ttu-id="48a34-125">Ott Twilio-műveletek toodefine hello viselkedést az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="48a34-125">From there, you use Twilio Verbs toodefine hello behavior of your application.</span></span>

### <span data-ttu-id="48a34-126"><a id="Verbs"></a>TwiML műveletek</span><span class="sxs-lookup"><span data-stu-id="48a34-126"><a id="Verbs"></a>TwiML Verbs</span></span>
<span data-ttu-id="48a34-127">Twilio-műveletek a következők: XML-címkék, amelyek megadják Twilio mi túl**tegye**.</span><span class="sxs-lookup"><span data-stu-id="48a34-127">Twilio Verbs are XML tags that tell Twilio what too**do**.</span></span> <span data-ttu-id="48a34-128">Például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.</span><span class="sxs-lookup"><span data-stu-id="48a34-128">For example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span> 

<span data-ttu-id="48a34-129">hello az alábbiakban olvashat egy listát a Twilio-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="48a34-129">hello following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="48a34-130">**&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="48a34-130">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="48a34-131">**&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.</span><span class="sxs-lookup"><span data-stu-id="48a34-131">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="48a34-132">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="48a34-132">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="48a34-133">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="48a34-133">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="48a34-134">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="48a34-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="48a34-135">**&lt;Rekord&gt;**: hello hívó hang rögzíti és hello rögzítése tartalmazó fájl URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="48a34-135">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="48a34-136">**&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="48a34-136">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="48a34-137">**&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio szám anélkül, hogy számlázási</span><span class="sxs-lookup"><span data-stu-id="48a34-137">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="48a34-138">**&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.</span><span class="sxs-lookup"><span data-stu-id="48a34-138">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="48a34-139">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="48a34-139">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

<span data-ttu-id="48a34-140">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="48a34-140">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="48a34-141">Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="48a34-141">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="48a34-142"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="48a34-142"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="48a34-143">Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="48a34-143">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="48a34-144">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="48a34-144">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="48a34-145">Amikor regisztrál egy Twilio-fiókot, később is lesz egy ingyenes telefonszámot az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="48a34-145">When you sign up for a Twilio account, you'll get a free phone number for your application.</span></span> <span data-ttu-id="48a34-146">Egy fiók SID azonosítója és egy hitelesítési jogkivonatot is kap.</span><span class="sxs-lookup"><span data-stu-id="48a34-146">You'll also receive an account SID and an auth token.</span></span> <span data-ttu-id="48a34-147">Egyaránt lesz szükséges toomake Twilio API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="48a34-147">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="48a34-148">nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága.</span><span class="sxs-lookup"><span data-stu-id="48a34-148">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="48a34-149">A fiók SID azonosítója és a hitelesítési jogkivonat is megtekinthető, hello [Twilio fióklapját][twilio_account], a mezők feliratú hello **fiók SID** és **hitelesítési TOKEN** , illetve.</span><span class="sxs-lookup"><span data-stu-id="48a34-149">Your account SID and auth token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

### <span data-ttu-id="48a34-150"><a id="VerifyPhoneNumbers"></a>Ellenőrizze a telefonszámok</span><span class="sxs-lookup"><span data-stu-id="48a34-150"><a id="VerifyPhoneNumbers"></a>Verify Phone Numbers</span></span>
<span data-ttu-id="48a34-151">Lehetősége van által Twilio hozzáadása toohello szám számok szabályozhatja (azaz a mobiltelefon vagy otthoni telefonszámot) számára az alkalmazások is ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="48a34-151">In addition toohello number you are given by Twilio, you can also verify numbers that you control (i.e. your cell phone or home phone number) for use in your applications.</span></span> 

<span data-ttu-id="48a34-152">Információ tooverify egy telefonszám, lásd: [kezelése számok][verify_phone].</span><span class="sxs-lookup"><span data-stu-id="48a34-152">For information on how tooverify a phone number, see [Manage Numbers][verify_phone].</span></span>

## <span data-ttu-id="48a34-153"><a id="create_app"></a>Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="48a34-153"><a id="create_app"></a>Create a Ruby Application</span></span>
<span data-ttu-id="48a34-154">Egy Ruby alkalmazás, amely hello Twilio szolgáltatást használ, és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más Ruby alkalmazás hello Twilio szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="48a34-154">A Ruby application that uses hello Twilio service and is running in Azure is no different than any other Ruby application that uses hello Twilio service.</span></span> <span data-ttu-id="48a34-155">Amíg Twilio-szolgáltatások RESTful, és a Ruby többféleképpen hívható, ez a cikk összpontosítanak hogyan toouse Twilio szolgáltatásokhoz [Twilio segítő kódtára a Rubyhoz][twilio_ruby].</span><span class="sxs-lookup"><span data-stu-id="48a34-155">While Twilio services are RESTful and can be called from Ruby in several ways, this article will focus on how toouse Twilio services with [Twilio helper library for Ruby][twilio_ruby].</span></span>

<span data-ttu-id="48a34-156">Első, [beállításról egy új Azure Linux virtuális gép] [ azure_vm_setup] tooact új Ruby webes alkalmazás gazdagépként.</span><span class="sxs-lookup"><span data-stu-id="48a34-156">First, [set-up a new Azure Linux VM][azure_vm_setup] tooact as a host for your new Ruby web application.</span></span> <span data-ttu-id="48a34-157">Ugorja át hello használata esetén egy sínek alkalmazáshoz, csak a telepítés hello VM hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="48a34-157">Ignore hello steps involving hello creation of a Rails app, just set-up hello VM.</span></span> <span data-ttu-id="48a34-158">Ellenőrizze, hogy egy külső port a 80-as és egy belső portjával 5000 hozzon létre egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="48a34-158">Make sure you create an Endpoint with an external port of 80 and an internal port of 5000.</span></span>

<span data-ttu-id="48a34-159">Hello a következő példa a fogjuk használni [Sinatra][sinatra], egy nagyon egyszerű webes keretrendszer, a Ruby.</span><span class="sxs-lookup"><span data-stu-id="48a34-159">In hello examples below, we will be using [Sinatra][sinatra], a very simple web framework for Ruby.</span></span> <span data-ttu-id="48a34-160">De alapértékekkel használható hello Twilio segédkódtárba helyezni a Rubyhoz bármely más webes keretrendszer, többek között a Ruby sínen.</span><span class="sxs-lookup"><span data-stu-id="48a34-160">But you can certainly use hello Twilio helper library for Ruby with any other web framework, including Ruby on Rails.</span></span>

<span data-ttu-id="48a34-161">SSH-ból az új virtuális gép, és hozzon létre egy könyvtárat, az új alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="48a34-161">SSH into your new VM and create a directory for your new app.</span></span> <span data-ttu-id="48a34-162">Adott könyvtárán belül hozzon létre egy Gemfile, és másolja hello kód bele a következő nevű fájlt:</span><span class="sxs-lookup"><span data-stu-id="48a34-162">Inside that directory, create a file called Gemfile and copy hello following code into it:</span></span>

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

<span data-ttu-id="48a34-163">Futtassa parancssorból hello `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="48a34-163">On hello command line run `bundle install`.</span></span> <span data-ttu-id="48a34-164">Ez a hello függőségeket telepíti.</span><span class="sxs-lookup"><span data-stu-id="48a34-164">This will install hello dependencies above.</span></span> <span data-ttu-id="48a34-165">Ezután hozzon létre egy nevű fájlt `web.rb`.</span><span class="sxs-lookup"><span data-stu-id="48a34-165">Next create a file called `web.rb`.</span></span> <span data-ttu-id="48a34-166">Ez lesz, ahol a webalkalmazás hello kód él.</span><span class="sxs-lookup"><span data-stu-id="48a34-166">This will be where hello code for your web app lives.</span></span> <span data-ttu-id="48a34-167">Illessze be a kódot bele a következő hello:</span><span class="sxs-lookup"><span data-stu-id="48a34-167">Paste hello following code into it:</span></span>

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

<span data-ttu-id="48a34-168">Ezen a ponton érdemes futtatni tudja hello hello parancs `ruby web.rb -p 5000`.</span><span class="sxs-lookup"><span data-stu-id="48a34-168">At this point you should be able hello run hello command `ruby web.rb -p 5000`.</span></span> <span data-ttu-id="48a34-169">Ez fogja léptetéses-legfeljebb 5000-es port kis webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="48a34-169">This will spin-up a small web server on port 5000.</span></span> <span data-ttu-id="48a34-170">El tudja toobrowse toothis alkalmazást a böngészőben hello URL-cím felkeresésével beállításról az Azure virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="48a34-170">You should be able toobrowse toothis app in your browser by visiting hello URL you set-up for your Azure VM.</span></span> <span data-ttu-id="48a34-171">A webalkalmazás hello böngészőben érheti el, ha készen áll a toostart Twilio-alkalmazás elkészítése az Ön.</span><span class="sxs-lookup"><span data-stu-id="48a34-171">Once you can reach your web app in hello browser, you're ready toostart building a Twilio app.</span></span>

## <span data-ttu-id="48a34-172"><a id="configure_app"></a>Alkalmazás tooUse Twilio konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48a34-172"><a id="configure_app"></a>Configure Your Application tooUse Twilio</span></span>
<span data-ttu-id="48a34-173">A web app toouse hello Twilio könyvtár frissítése úgy állíthatja be a `Gemfile` tooinclude ezt a sort:</span><span class="sxs-lookup"><span data-stu-id="48a34-173">You can configure your web app toouse hello Twilio library by updating your `Gemfile` tooinclude this line:</span></span>

    gem 'twilio-ruby'

<span data-ttu-id="48a34-174">Hello parancssorban futtassa `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="48a34-174">On hello command line, run `bundle install`.</span></span> <span data-ttu-id="48a34-175">Most nyissa meg a `web.rb` és a sor hello bal felső, így:</span><span class="sxs-lookup"><span data-stu-id="48a34-175">Now open `web.rb` and including this line at hello top:</span></span>

    require 'twilio-ruby'

<span data-ttu-id="48a34-176">Most, hogy most már minden beállítva toouse hello Twilio segédkódtárba helyezni a Rubyhoz a web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="48a34-176">You're now all set toouse hello Twilio helper library for Ruby in your web app.</span></span>

## <span data-ttu-id="48a34-177"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="48a34-177"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="48a34-178">a következő hello bemutatja, hogyan egy kimenő toomake hívja.</span><span class="sxs-lookup"><span data-stu-id="48a34-178">hello following shows how toomake an outgoing call.</span></span> <span data-ttu-id="48a34-179">Főbb fogalmait tartalmazza a REST API-hívások Ruby toomake hello Twilio segédkódtárba helyezni használja, és TwiML megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="48a34-179">Key concepts include using hello Twilio helper library for Ruby toomake REST API calls and rendering TwiML.</span></span> <span data-ttu-id="48a34-180">Helyettesítse a saját értékeit hello **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszámot a Twilio fiók előzetes toorunning hello kódot.</span><span class="sxs-lookup"><span data-stu-id="48a34-180">Substitute your values for hello **From** and **To** phone numbers, and ensure that you verify hello **From** phone number for your Twilio account prior toorunning hello code.</span></span>

<span data-ttu-id="48a34-181">Ez a funkció hozzáadása túl`web.md`:</span><span class="sxs-lookup"><span data-stu-id="48a34-181">Add this function too`web.md`:</span></span>

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # hello number of hello phone initiating hello hello call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # hello number of hello phone receiving call.
    too= "NNNNNNNNNNN";

    # Use hello Twilio-provided site for hello TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create hello call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make hello call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

<span data-ttu-id="48a34-182">Ha Ön nyílt felfelé `http://yourdomain.cloudapp.net/make_call` egy böngészőben, amely elindítja a hello hívás toohello Twilio API toomake hello telefonhívás.</span><span class="sxs-lookup"><span data-stu-id="48a34-182">If you open-up `http://yourdomain.cloudapp.net/make_call` in a browser, that will trigger hello call toohello Twilio API toomake hello phone call.</span></span> <span data-ttu-id="48a34-183">az első két paraméterek hello `client.account.calls.create` magától értetődő viszonylag: hello számú hello hívás `from` és hello számú hello hívás `to`.</span><span class="sxs-lookup"><span data-stu-id="48a34-183">hello first two parameters in `client.account.calls.create` are fairly self-explanatory: hello number hello call is `from` and hello number hello call is `to`.</span></span> 

<span data-ttu-id="48a34-184">hello a harmadik paraméter (`url`), hogy a Twilio hello hívás csatlakoztatása után kérelmek milyen toodo tooget utasításokat hello URL-cím.</span><span class="sxs-lookup"><span data-stu-id="48a34-184">hello third parameter (`url`) is hello URL that Twilio requests tooget instructions on what toodo once hello call is connected.</span></span> <span data-ttu-id="48a34-185">Ebben az esetben azt URL-címet a telepítés (`http://yourdomain.cloudapp.net`), amely egy egyszerű TwiML dokumentumot ad vissza, és használja a hello `<Say>` néhány szöveg-beszéd átalakítás, és mondja ki fogadó "Hello Monkey" toohello személy hello művelet toodo hívja.</span><span class="sxs-lookup"><span data-stu-id="48a34-185">In this case we set-up a URL (`http://yourdomain.cloudapp.net`) that returns a simple TwiML document and uses hello `<Say>` verb toodo some text-to-speech and say "Hello Monkey" toohello person recieving hello call.</span></span>

## <span data-ttu-id="48a34-186"><a id="howto_recieve_sms"></a>Útmutató: az SMS-üzenet fogadása</span><span class="sxs-lookup"><span data-stu-id="48a34-186"><a id="howto_recieve_sms"></a>How to: Recieve an SMS message</span></span>
<span data-ttu-id="48a34-187">Az előző példában hello azt kezdeményezett egy **kimenő** telefonhívás.</span><span class="sxs-lookup"><span data-stu-id="48a34-187">In hello previous example we initiated an **outgoing** phone call.</span></span> <span data-ttu-id="48a34-188">Ez alkalommal, most használja az, hogy a Twilio-előfizetési tooprocess során megadott telefonszám hello egy **bejövő** SMS-üzenet.</span><span class="sxs-lookup"><span data-stu-id="48a34-188">This time, let's use hello phone number that Twilio gave us during sign-up tooprocess an **incoming** SMS message.</span></span>

<span data-ttu-id="48a34-189">Első, a bejelentkezés tooyour [Twilio-irányítópult][twilio_account].</span><span class="sxs-lookup"><span data-stu-id="48a34-189">First, log-in tooyour [Twilio dashboard][twilio_account].</span></span> <span data-ttu-id="48a34-190">Kattintson a "Numbers" hello felső NAV, majd a hello Twilio-szám lett megadva.</span><span class="sxs-lookup"><span data-stu-id="48a34-190">Click on "Numbers" in hello top nav and then click on hello Twilio number you were provided.</span></span> <span data-ttu-id="48a34-191">Látni fogja, két URL-címeket, amelyeket konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="48a34-191">You'll see two URLs that you can configure.</span></span> <span data-ttu-id="48a34-192">Egy hang kérelem URL-CÍMÉT és az SMS kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="48a34-192">A Voice Request URL and an SMS Request URL.</span></span> <span data-ttu-id="48a34-193">Ezek hello URL-címek készített Twilio hívásokat, amikor egy telefonhívást vagy SMS küldött tooyour számát.</span><span class="sxs-lookup"><span data-stu-id="48a34-193">These are hello URLs that Twilio calls whenever a phone call is made or an SMS is sent tooyour number.</span></span> <span data-ttu-id="48a34-194">hello URL-címek "webes hurkok" is nevezik.</span><span class="sxs-lookup"><span data-stu-id="48a34-194">hello URLs are also known as "web hooks".</span></span>

<span data-ttu-id="48a34-195">Szeretnénk tooprocess bejövő SMS-t, így érdemes hello URL-címe túl frissítése`http://yourdomain.cloudapp.net/sms_url`.</span><span class="sxs-lookup"><span data-stu-id="48a34-195">We would like tooprocess incoming SMS messages, so let's update hello URL too`http://yourdomain.cloudapp.net/sms_url`.</span></span> <span data-ttu-id="48a34-196">Lépjen tovább, és kattintson a módosítások mentése hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="48a34-196">Go ahead and click Save Changes at hello bottom of hello page.</span></span> <span data-ttu-id="48a34-197">Most, újból `web.rb` tegyük a programot az alkalmazás toohandle ezt:</span><span class="sxs-lookup"><span data-stu-id="48a34-197">Now, back in `web.rb` let's program our application toohandle this:</span></span>

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for hello ping! Twilio and Azure rock!</Message>
       </Response>"
    end

<span data-ttu-id="48a34-198">Hello módosítás után győződjön meg arról, hogy toore indítási a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="48a34-198">After making hello change, make sure toore-start your web app.</span></span> <span data-ttu-id="48a34-199">Most vegye ki a telefonjára, és küldjön egy SMS-tooyour Twilio-szám.</span><span class="sxs-lookup"><span data-stu-id="48a34-199">Now, take out your phone and send an SMS tooyour Twilio number.</span></span> <span data-ttu-id="48a34-200">Azonnal kapja meg az SMS-válasz, amely szerint "Hey, hello ping Köszönjük!</span><span class="sxs-lookup"><span data-stu-id="48a34-200">You should promptly get an SMS response that says "Hey, thanks for hello ping!</span></span> <span data-ttu-id="48a34-201">Twilio és az Azure rock! ".</span><span class="sxs-lookup"><span data-stu-id="48a34-201">Twilio and Azure rock!".</span></span>

## <span data-ttu-id="48a34-202"><a id="additional_services"></a>Útmutató: további Twilio-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="48a34-202"><a id="additional_services"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="48a34-203">Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="48a34-203">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="48a34-204">Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="48a34-204">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

### <span data-ttu-id="48a34-205"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48a34-205"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="48a34-206">Most, hogy megismerte a Twilio szolgáltatáshoz hello hello alapjait, kövesse a további hivatkozások toolearn:</span><span class="sxs-lookup"><span data-stu-id="48a34-206">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="48a34-207">[Twilio biztonsági irányelvek][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="48a34-207">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="48a34-208">[Twilio HowTos és példakódot][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="48a34-208">[Twilio HowTos and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="48a34-209">[Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="48a34-209">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="48a34-210">[A Githubon Twilio][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="48a34-210">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="48a34-211">[Forduljon a támogatási tooTwilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="48a34-211">[Talk tooTwilio Support][twilio_support]</span></span>

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
