---
title: "aaaHow tooUse Twilio hang-és SMS (Python) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. Kódminták Python."
services: 
documentationcenter: python
author: devinrader
manager: twilio
editor: 
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 1f16a107d95c94589b8d61a0971c5b62d639c571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-python"></a><span data-ttu-id="6c7a5-104">Hogyan tooUse Twilio hang-és Python SMS képességei</span><span class="sxs-lookup"><span data-stu-id="6c7a5-104">How tooUse Twilio for Voice and SMS Capabilities in Python</span></span>
<span data-ttu-id="6c7a5-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="6c7a5-106">a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="6c7a5-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="6c7a5-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="6c7a5-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="6c7a5-109">Twilio hello jövőbeli üzleti kommunikáció működtetéséhez, amely lehetővé teszi a fejlesztők tooembed hang, a VoIP, és alkalmazásokba üzenetküldési.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="6c7a5-110">Ezek virtualizálása hello Twilio kommunikációs API-platformon keresztül, az ilyen felhőalapú, a globális környezetben szükséges összes infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="6c7a5-111">Olyan egyszerű toobuild és méretezhető.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="6c7a5-112">Élvezze a rugalmasságot Önnel fizetési-, nyissa meg árképzési, és igénybe vehesse az felhő megbízhatóság.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="6c7a5-113">**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span>
<span data-ttu-id="6c7a5-114">**Twilio SMS** lehetővé teszi, hogy az alkalmazás toosend és a szöveges üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-114">**Twilio SMS** enables your application toosend and receive text messages.</span></span>
<span data-ttu-id="6c7a5-115">**Twilio-ügyfél** lehetővé teszi a toomake VoIP hívást a telefon, a táblagép vagy a böngésző és WebRTC támogatja.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="6c7a5-116"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="6c7a5-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="6c7a5-117">Az Azure-ügyfelek egy [a különleges ajánlat] [ special_offer] 10 $ Twilio-kredit a Twilio-fiók frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-117">Azure customers receive a [special offer][special_offer] $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="6c7a5-118">A Twilio-jóváírási alkalmazott tooany Twilio-használati ($10 jóváírás egyenértékű toosending akár 1000 SMS-t vagy mentése too1000 fogadás bejövő hang perc, attól függően, hogy a telefon és üzenet vagy hívás cél hello helye) lehet.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="6c7a5-119">Ez beváltani [Twilio-jóváírási] [ special_offer] használatának első lépéseihez.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-119">Redeem this [Twilio credit][special_offer] and get started.</span></span>

<span data-ttu-id="6c7a5-120">Twilio egy olyan használatalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="6c7a5-121">Nincsenek nem beállításról díjakat, és bármikor bezárhatja a fiókját.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="6c7a5-122">További részletei: [Twilio árképzési][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="6c7a5-123"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="6c7a5-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="6c7a5-124">hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="6c7a5-125">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="6c7a5-126">Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="6c7a5-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="6c7a5-127"><a id="Verbs"></a>Twilio-műveletek</span><span class="sxs-lookup"><span data-stu-id="6c7a5-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="6c7a5-128">hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="6c7a5-129">hello az alábbiakban olvashat egy listát a Twilio-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-129">hello following is a list of Twilio verbs.</span></span> <span data-ttu-id="6c7a5-130">Ismerje meg, más műveletek és a képességek keresztül készül hello [Twilio Markup Language dokumentáció][twiml].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation][twiml].</span></span>

* <span data-ttu-id="6c7a5-131">**&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="6c7a5-132">**&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="6c7a5-133">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="6c7a5-134">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="6c7a5-135">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-135">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="6c7a5-136">**&lt;Várólista&gt;**: hello tooa várólista hívók hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-136">**&lt;Queue&gt;**: Add hello tooa queue of callers.</span></span>
* <span data-ttu-id="6c7a5-137">**&lt;Rekord&gt;**: hello hang hello hívó rögzíti és hello rögzítése tartalmazó fájl URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-137">**&lt;Record&gt;**: Records hello voice of hello caller and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="6c7a5-138">**&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-138">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="6c7a5-139">**&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio-szám nélkül számlázási meg.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-139">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you.</span></span>
* <span data-ttu-id="6c7a5-140">**&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-140">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="6c7a5-141">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-141">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="6c7a5-142"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="6c7a5-142"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="6c7a5-143">TwiML egy olyan XML-alapú utasítások alapján, amely tájékoztatja, hogy hogyan Twilio hello Twilio-műveletek tooprocess hívás vagy SMS.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-143">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="6c7a5-144">Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-144">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="6c7a5-145">Ha az alkalmazás hívások hello Twilio API, hello API paraméterek egyike hello URL-címet, amely hello TwiML választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-145">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="6c7a5-146">Fejlesztési célra Twilio által megadott URL-címek tooprovide hello TwiML válaszok az alkalmazások által használt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-146">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="6c7a5-147">Akkor is elhelyezheti a saját URL-címek tooproduce hello TwiML válaszokat, és egy másik lehetőség toouse hello `TwiMLResponse` objektum.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-147">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello `TwiMLResponse` object.</span></span>

<span data-ttu-id="6c7a5-148">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-148">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="6c7a5-149">Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-149">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="6c7a5-150"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c7a5-150"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="6c7a5-151">Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-151">When you are ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="6c7a5-152">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-152">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="6c7a5-153">Amikor regisztrál egy Twilio-fiókja, kap egy fiók SID azonosítója és egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-153">When you sign up for a Twilio account, you receive an account SID and an authentication token.</span></span> <span data-ttu-id="6c7a5-154">Egyaránt lesz szükséges toomake Twilio API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-154">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="6c7a5-155">nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-155">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="6c7a5-156">A fiók SID azonosítója és a hitelesítési jogkivonat hello megtekinthető [Twilio-konzol][twilio_console], a mezők feliratú hello **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-156">Your account SID and authentication token are viewable in hello [Twilio Console][twilio_console], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="6c7a5-157"><a id="create_app"></a>Python-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c7a5-157"><a id="create_app"></a>Create a Python Application</span></span>
<span data-ttu-id="6c7a5-158">Egy Python-alkalmazás, amely hello Twilio szolgáltatást használ, és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más Python alkalmazás hello Twilio szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-158">A Python application that uses hello Twilio service and is running in Azure is no different than any other Python application that uses hello Twilio service.</span></span> <span data-ttu-id="6c7a5-159">Twilio-szolgáltatások REST-alapú, és a Python többféleképpen hívható, ez a cikk összpontosítanak hogyan toouse Twilio szolgáltatásokhoz [Twilio-kódtára a Pythonhoz a Githubról][twilio_python].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-159">While Twilio services are REST-based and can be called from Python in several ways, this article will focus on how toouse Twilio services with [Twilio library for Python from GitHub][twilio_python].</span></span> <span data-ttu-id="6c7a5-160">Python hello Twilio-könyvtár használatával kapcsolatos további információkért lásd: [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-160">For more information about using hello Twilio library for Python, see [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="6c7a5-161">Első, a [telepítés egy új Azure Linux virtuális gép] [azure_vm_setup] tooact új Python webes alkalmazás gazdagépként.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-161">First, [set-up a new Azure Linux VM][azure_vm_setup] tooact as a host for your new Python web application.</span></span> <span data-ttu-id="6c7a5-162">Miután hello virtuális gép fut, szükség van egy tooexpose az alkalmazás egy nyilvános portot az alábbiakban.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-162">Once hello Virtual Machine is running, you will need tooexpose your application on a public port as described below.</span></span>

### <a name="add-an-incoming-rule"></a><span data-ttu-id="6c7a5-163">Bejövő szabály felvétele</span><span class="sxs-lookup"><span data-stu-id="6c7a5-163">Add An Incoming Rule</span></span>
  1. <span data-ttu-id="6c7a5-164">Nyissa meg toohello [hálózati biztonsági csoport] [azure_nsg] lap.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-164">Go toohello [Network Security Group][azure_nsg] page.</span></span>
  2. <span data-ttu-id="6c7a5-165">Válassza ki a hálózati biztonsági csoport, amely megfelel a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-165">Select hello Network Security Group that corresponds with your Virtual Machine.</span></span>
  3. <span data-ttu-id="6c7a5-166">Adja hozzá és **kimenő szabály** a **80-as port**.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-166">Add and **Outgoing Rule** for **port 80**.</span></span> <span data-ttu-id="6c7a5-167">Lehet, hogy tooallow bejövő bármilyen címről.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-167">Be sure tooallow incoming from any address.</span></span>

### <a name="set-hello-dns-name-label"></a><span data-ttu-id="6c7a5-168">DNS-névcímke hello beállítása</span><span class="sxs-lookup"><span data-stu-id="6c7a5-168">Set hello DNS Name Label</span></span>
  1. <span data-ttu-id="6c7a5-169">Nyissa meg toohello [hello nyilvános IP-címet adott meg] [azure_ips] lap.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-169">Go toohello [hello Public IP Adresses][azure_ips] page.</span></span>
  2. <span data-ttu-id="6c7a5-170">Válassza ki a hello nyilvános IP-címet, hogy correspends a virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-170">Select hello Public IP that correspends with your Virtual Machine.</span></span>
  3. <span data-ttu-id="6c7a5-171">Set hello **DNS-névcímke** a hello **konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-171">Set hello **DNS Name Label** in hello **Configuration** section.</span></span> <span data-ttu-id="6c7a5-172">A jelen példában hello esetben fog megjelenni ehhez hasonló *a tartománycímke*. centralus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="6c7a5-172">In hello case of this example it will look something like this *your-domain-label*.centralus.cloudapp.azure.com</span></span>

<span data-ttu-id="6c7a5-173">Ha le is tudja keresztül telepítheti a virtuális gép SSH toohello tooconnect hello webes keretrendszer az Ön által választott (hello két legtöbb közismert a Python kívánt [Flask](http://flask.pocoo.org/) és [Django](https://www.djangoproject.com)).</span><span class="sxs-lookup"><span data-stu-id="6c7a5-173">Once you are able tooconnect through SSH toohello Virtual Machine you can install hello Web Framework of your choice (hello two most well known in Python being [Flask](http://flask.pocoo.org/) and [Django](https://www.djangoproject.com)).</span></span> <span data-ttu-id="6c7a5-174">Csak a hello futtatásával telepíthető valamelyiken `pip install` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-174">You can install either of them just by running hello `pip install` command.</span></span>

<span data-ttu-id="6c7a5-175">Ne feledje, hogy csak a 80-as porton konfigurált hello virtuális gép tooallow forgalmat.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-175">Keep in mind that we configured hello Virtual Machine tooallow traffic only on port 80.</span></span> <span data-ttu-id="6c7a5-176">Ezért győződjön meg arról, hogy tooconfigure hello alkalmazás toouse ezt a portot.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-176">So make sure tooconfigure hello application toouse this port.</span></span>

## <span data-ttu-id="6c7a5-177"><a id="configure_app"></a>Alkalmazás tooUse Twilio szalagtárak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c7a5-177"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="6c7a5-178">Az alkalmazás toouse hello Twilio-könyvtár Python kétféleképpen konfigurálható:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-178">You can configure your application toouse hello Twilio library for Python in two ways:</span></span>

* <span data-ttu-id="6c7a5-179">Telepítse a Python hello Twilio könyvtár Pip csomag.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-179">Install hello Twilio library for Python as a Pip package.</span></span> <span data-ttu-id="6c7a5-180">A következő parancsok hello telepíthető:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-180">It can be installed with hello following commands:</span></span>
   
        $ pip install twilio

    <span data-ttu-id="6c7a5-181">-VAGY-</span><span class="sxs-lookup"><span data-stu-id="6c7a5-181">-OR-</span></span>

* <span data-ttu-id="6c7a5-182">Töltse le a Githubról Python hello Twilio-könyvtár ([https://github.com/twilio/twilio-python][twilio_python]), a telepítés:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-182">Download hello Twilio library for Python from GitHub ([https://github.com/twilio/twilio-python][twilio_python]) and install it like this:</span></span>

        $ python setup.py install

<span data-ttu-id="6c7a5-183">Miután telepítette a hello Twilio-könyvtár a Python, a következőket teheti majd `import` azt a Python-fájlokban:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-183">Once you have installed hello Twilio library for Python, you can then `import` it in your Python files:</span></span>

        import twilio

<span data-ttu-id="6c7a5-184">További információkért lásd: [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-184">For more information, see [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="6c7a5-185"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="6c7a5-185"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="6c7a5-186">a következő hello bemutatja, hogyan egy kimenő toomake hívja.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-186">hello following shows how toomake an outgoing call.</span></span> <span data-ttu-id="6c7a5-187">Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-187">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="6c7a5-188">Helyettesítse a saját értékeit hello **from_number** és **to_number** telefonszámokat, és győződjön meg arról, hogy ellenőrizte, hogy hello **from_number** telefonszám Twilio-fiókja Mielőtt hello kód futtatása.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-188">Substitute your values for hello **from_number** and **to_number** phone numbers, and ensure that you've verified hello **from_number** phone number for your Twilio account before running hello code.</span></span>

    from urllib.parse import urlencode

    # Import hello Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # hello number of hello phone initiating hello hello call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # hello number of hello phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use hello Twilio-provided site for hello TwiML response.
    url = "http://twimlets.com/message?"

    # hello phone message text.
    message = "Hello world."

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make hello call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

<span data-ttu-id="6c7a5-189">Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-189">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="6c7a5-190">Ehelyett használhat saját hely tooprovide hello TwiML válasz; További információkért lásd: [hogyan tooProvide TwiML válaszok a saját webhelyről](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="6c7a5-190">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="6c7a5-191"><a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="6c7a5-191"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="6c7a5-192">hello következő bemutatja, hogyan egy SMS üzenetet használatával toosend hello `TwilioRestClient` osztály.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-192">hello following shows how toosend an SMS message using hello `TwilioRestClient` class.</span></span> <span data-ttu-id="6c7a5-193">Hello **from_number** szám Twilio biztosítja a próbaverzió fiókok toosend SMS-t.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-193">hello **from_number** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="6c7a5-194">Hello **to_number** szám ellenőrizni kell a Twilio-fiókjához hello kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-194">hello **to_number** number must be verified for your Twilio account before running hello code.</span></span>

    # Import hello Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send hello SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <span data-ttu-id="6c7a5-195"><a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok</span><span class="sxs-lookup"><span data-stu-id="6c7a5-195"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="6c7a5-196">Ha az alkalmazás egy hívás toohello Twilio API, Twilio elküldi a kérelem tooa URL-CÍMÉT, amely tooreturn TwiML választ várt.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-196">When your application initiates a call toohello Twilio API, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="6c7a5-197">hello a fenti példában hello Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-197">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="6c7a5-198">(TwiML által Twilio használatra szolgál, míg tekintheti a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-198">(While TwiML is designed for use by Twilio, you can view it in your browser.</span></span> <span data-ttu-id="6c7a5-199">Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres `<Response>` elem; másik példaként, kattintson a [http://twimlets.com/message? % 5B0 üzenet %5 D Hello % 20World =] [ twimlet_message_url_hello_world] toosee egy `<Response>` elem, amely tartalmazza egy `<Say>` elem.)</span><span class="sxs-lookup"><span data-stu-id="6c7a5-199">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] toosee a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="6c7a5-200">Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját webhelyén, amely HTTP-válaszokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-200">Instead of relying on hello Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="6c7a5-201">Hello helyet létrehozhat bármilyen nyelven, amely visszaadja az XML-válaszok; Ez a témakör azt feltételezi, hogy a Python toocreate hello TwiML fog használni.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-201">You can create hello site in any language that returns XML responses; this topic assumes you will be using Python toocreate hello TwiML.</span></span>

<span data-ttu-id="6c7a5-202">hello alábbi példák kimeneteként TwiML választ, amely szerint **Hello World** hello hívásakor.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-202">hello following examples will output a TwiML response that says **Hello World** on hello call.</span></span>

<span data-ttu-id="6c7a5-203">A Flask:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-203">With Flask:</span></span>

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

<span data-ttu-id="6c7a5-204">A django alkalmazással:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-204">With Django:</span></span>

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

<span data-ttu-id="6c7a5-205">Ahogy látja, a fenti példa hello, hello TwiML válasz egyszerűen egy XML-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-205">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="6c7a5-206">hello Twilio kódtára a Pythonhoz TwiML hoz létre az Ön osztályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-206">hello Twilio library for Python contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="6c7a5-207">hello az alábbi példa hello egyenértékű válasz eredményez, ahogy fent látható, de használ hello `twiml` hello Twilio könyvtárban Python modul:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-207">hello example below produces hello equivalent response as shown above, but uses hello `twiml` module in hello Twilio library for Python:</span></span>

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

<span data-ttu-id="6c7a5-208">TwiML kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-208">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span>

<span data-ttu-id="6c7a5-209">Miután a Python-alkalmazás beállítása tooprovide TwiML válaszok, használható hello alkalmazás URL-címe hello hello hello átadott URL-cím `client.calls.create` metódust.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-209">Once you have your Python application set up tooprovide TwiML responses, use hello URL of hello application as hello URL passed into hello `client.calls.create`  method.</span></span> <span data-ttu-id="6c7a5-210">Ha egy webes alkalmazás neve például **MyTwiML** telepített tooan Azure üzemeltetett szolgáltatásban futó, használja az URL-cím webhook, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-210">For example, if you have a Web application named **MyTwiML** deployed tooan Azure hosted service, you can use its url as webhook as shown in hello following example:</span></span>

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make hello call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <span data-ttu-id="6c7a5-211"><a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="6c7a5-211"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="6c7a5-212">Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6c7a5-212">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="6c7a5-213">Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="6c7a5-213">For full details, see hello [Twilio API documentation][twilio_api].</span></span>

## <span data-ttu-id="6c7a5-214"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c7a5-214"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="6c7a5-215">Most, hogy a Twilio szolgáltatáshoz hello hello alapjait megtanulta, kövesse a további hivatkozások toolearn:</span><span class="sxs-lookup"><span data-stu-id="6c7a5-215">Now that you have learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="6c7a5-216">[Twilio biztonsági irányelvek][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="6c7a5-216">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="6c7a5-217">[Útmutató a Twilio-útmutatók és példakódot][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="6c7a5-217">[Twilio HowTo Guides and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="6c7a5-218">[Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="6c7a5-218">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="6c7a5-219">[A Githubon Twilio][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="6c7a5-219">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="6c7a5-220">[Forduljon a támogatási tooTwilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="6c7a5-220">[Talk tooTwilio Support][twilio_support]</span></span>

[special_offer]: http://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-python/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
