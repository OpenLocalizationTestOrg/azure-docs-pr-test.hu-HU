---
title: "aaaHow tooUse Twilio hang-és SMS (Java) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. A Kódminták Java nyelven."
services: 
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: f3508965-5527-4255-9d51-5d5f926f4d43
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: a186e2c8e73ced928bd0dec348971034f10ba82c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-java"></a><span data-ttu-id="c4b27-104">Hogyan tooUse Twilio hang-és Java SMS képességei</span><span class="sxs-lookup"><span data-stu-id="c4b27-104">How tooUse Twilio for Voice and SMS Capabilities in Java</span></span>
<span data-ttu-id="c4b27-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c4b27-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="c4b27-106">a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="c4b27-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="c4b27-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c4b27-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="c4b27-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="c4b27-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="c4b27-109">Twilio egy telefonos webszolgáltatás API, amely lehetővé teszi, hogy a meglévő webes nyelv és képességek toobuild hang- és SMS-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c4b27-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills toobuild voice and SMS applications.</span></span> <span data-ttu-id="c4b27-110">Twilio egy olyan külső szolgáltatás (nem az Azure szolgáltatásai és nem Microsoft-termékek).</span><span class="sxs-lookup"><span data-stu-id="c4b27-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="c4b27-111">**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="c4b27-111">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="c4b27-112">**Twilio SMS** lehetővé teszi, hogy az alkalmazások toomake és az SMS-üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="c4b27-112">**Twilio SMS** allows your applications toomake and receive SMS messages.</span></span> <span data-ttu-id="c4b27-113">**Twilio-ügyfél** lehetővé teszi az alkalmazások tooenable hang kommunikációt meglévő hálózati kapcsolatok használata esetén, beleértve a mobil kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="c4b27-113">**Twilio Client** allows your applications tooenable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="c4b27-114"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="c4b27-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="c4b27-115">Információk a díjszabásról Twilio érhető el: [Twilio árképzési][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="c4b27-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="c4b27-116">Az Azure-ügyfelek egy [a különleges ajánlat][special_offer]: 1000 szövegek szabad követel vagy bejövő perc 1000.</span><span class="sxs-lookup"><span data-stu-id="c4b27-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="c4b27-117">Ez feliratkozott toosign ajánlat vagy további információért, látogasson el a [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="c4b27-117">toosign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>

## <span data-ttu-id="c4b27-118"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="c4b27-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="c4b27-119">hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="c4b27-119">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="c4b27-120">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="c4b27-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="c4b27-121">Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="c4b27-121">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="c4b27-122"><a id="Verbs"></a>Twilio-műveletek</span><span class="sxs-lookup"><span data-stu-id="c4b27-122"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="c4b27-123">hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.</span><span class="sxs-lookup"><span data-stu-id="c4b27-123">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="c4b27-124">hello az alábbiakban olvashat egy listát a Twilio-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="c4b27-124">hello following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="c4b27-125">**&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="c4b27-125">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="c4b27-126">**&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.</span><span class="sxs-lookup"><span data-stu-id="c4b27-126">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="c4b27-127">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="c4b27-127">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="c4b27-128">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="c4b27-128">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="c4b27-129">**&lt;Várólista&gt;**: hello tooa várólista hívók hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="c4b27-129">**&lt;Queue&gt;**: Add hello tooa queue of callers.</span></span>
* <span data-ttu-id="c4b27-130">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="c4b27-130">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="c4b27-131">**&lt;Rekord&gt;**: hello hívó hang rögzíti és hello rögzítése tartalmazó fájl URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c4b27-131">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="c4b27-132">**&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="c4b27-132">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="c4b27-133">**&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio-szám nélkül számlázási meg.</span><span class="sxs-lookup"><span data-stu-id="c4b27-133">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you.</span></span>
* <span data-ttu-id="c4b27-134">**&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.</span><span class="sxs-lookup"><span data-stu-id="c4b27-134">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="c4b27-135">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="c4b27-135">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="c4b27-136"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="c4b27-136"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="c4b27-137">TwiML egy olyan XML-alapú utasítások alapján, amely tájékoztatja, hogy hogyan Twilio hello Twilio-műveletek tooprocess hívás vagy SMS.</span><span class="sxs-lookup"><span data-stu-id="c4b27-137">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="c4b27-138">Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="c4b27-138">As an example, hello following TwiML would convert hello text **Hello World!**</span></span> <span data-ttu-id="c4b27-139">toospeech.</span><span class="sxs-lookup"><span data-stu-id="c4b27-139">toospeech.</span></span>

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="c4b27-140">Ha az alkalmazás hívások hello Twilio API, hello API paraméterek egyike hello URL-címet, amely hello TwiML választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c4b27-140">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="c4b27-141">Fejlesztési célra Twilio által megadott URL-címek tooprovide hello TwiML válaszok az alkalmazások által használt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c4b27-141">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="c4b27-142">Akkor is elhelyezheti a saját URL-címek tooproduce hello TwiML válaszokat, és egy másik lehetőség toouse hello **TwiMLResponse** objektum.</span><span class="sxs-lookup"><span data-stu-id="c4b27-142">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="c4b27-143">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="c4b27-143">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="c4b27-144">Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="c4b27-144">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="c4b27-145"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4b27-145"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="c4b27-146">Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="c4b27-146">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="c4b27-147">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="c4b27-147">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="c4b27-148">Amikor regisztrál egy Twilio-fiókja, kapni fog egy Fiókazonosító és egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="c4b27-148">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="c4b27-149">Egyaránt lesz szükséges toomake Twilio API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="c4b27-149">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="c4b27-150">nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága.</span><span class="sxs-lookup"><span data-stu-id="c4b27-150">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="c4b27-151">A Fiókazonosítót és a hitelesítési jogkivonat megtekinthető a hello [Twilio-konzol][twilio_console], a mezők feliratú hello **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.</span><span class="sxs-lookup"><span data-stu-id="c4b27-151">Your account ID and authentication token are viewable at hello [Twilio Console][twilio_console], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="c4b27-152"><a id="create_app"></a>Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4b27-152"><a id="create_app"></a>Create a Java Application</span></span>
1. <span data-ttu-id="c4b27-153">Szerezze be a Twilio-JAR hello, és vegye fel tooyour Java elérési út és a WAR-telepítési szerelvény hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="c4b27-153">Obtain hello Twilio JAR and add it tooyour Java build path and your WAR deployment assembly.</span></span> <span data-ttu-id="c4b27-154">A [https://github.com/twilio/twilio-java][twilio_java], hello GitHub forrásból töltse le és létrehozhat saját JAR, vagy egy előre elkészített JAR (vagy anélkül függőségek) letöltése.</span><span class="sxs-lookup"><span data-stu-id="c4b27-154">At [https://github.com/twilio/twilio-java][twilio_java], you can download hello GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
2. <span data-ttu-id="c4b27-155">Győződjön meg arról a JDK **cacerts** keystore hello Equifax biztonságos hitelesítésszolgáltató tanúsítványát az MD5 ujjlenyomat 67:CB:9 D tartalmazza: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello sorozatszám 35:DE:F4:CF és hello SHA1 ujjlenyomat D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="c4b27-155">Ensure your JDK's **cacerts** keystore contains hello Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serial number is 35:DE:F4:CF and hello SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="c4b27-156">Ez az hello tanúsítvány hitelesítésszolgáltatói tanúsítvány hello [https://api.twilio.com] [ twilio_api_service] szolgáltatást, amelyet a Twilio API-k használatakor nevezik.</span><span class="sxs-lookup"><span data-stu-id="c4b27-156">This is hello certificate authority (CA) certificate for hello [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="c4b27-157">További információ biztosítása a JDK **cacerts** keystore tartalmaz hello megfelelő CA-tanúsítvány című [hozzáadása egy tanúsítvány toohello Java hitelesítésszolgáltató tanúsítványtároló][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="c4b27-157">For information about ensuring your JDK's **cacerts** keystore contains hello correct CA certificate, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="c4b27-158">Részletes útmutatásért hello Twilio ügyféloldali kódtár Java érhetők el [hogyan tooMake Azure Java-alkalmazásokban a telefonhívás használatával Twilio][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="c4b27-158">Detailed instructions for using hello Twilio client library for Java are available at [How tooMake a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="c4b27-159"><a id="configure_app"></a>Alkalmazás tooUse Twilio szalagtárak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c4b27-159"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="c4b27-160">A kód is hozzáadhat **importálása** hello Twilio-csomagok, vagy osztályokat, akkor a forrásfájlok hello tetején utasítások szeretné, hogy az alkalmazás toouse.</span><span class="sxs-lookup"><span data-stu-id="c4b27-160">Within your code, you can add **import** statements at hello top of your source files for hello Twilio packages or classes you want toouse in your application.</span></span>

<span data-ttu-id="c4b27-161">Java forrásfájljait tároló:</span><span class="sxs-lookup"><span data-stu-id="c4b27-161">For Java source files:</span></span>

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

<span data-ttu-id="c4b27-162">Java kiszolgáló lap (JSP) forrásfájljait tároló:</span><span class="sxs-lookup"><span data-stu-id="c4b27-162">For Java Server Page (JSP) source files:</span></span>

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
<span data-ttu-id="c4b27-163">Attól függően, hogy mely Twilio-csomagokat vagy osztályok kívánt toouse, a **importálása** utasítások eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="c4b27-163">Depending on which Twilio packages or classes you want toouse, your **import** statements may be different.</span></span>

## <span data-ttu-id="c4b27-164"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="c4b27-164"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="c4b27-165">hello következő bemutatja, hogyan egy kimenő toomake hívás, hello használatával **hívás** osztály.</span><span class="sxs-lookup"><span data-stu-id="c4b27-165">hello following shows how toomake an outgoing call using hello **Call** class.</span></span> <span data-ttu-id="c4b27-166">Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ.</span><span class="sxs-lookup"><span data-stu-id="c4b27-166">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="c4b27-167">Helyettesítse a saját értékeit hello **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszámot a Twilio fiók előzetes toorunning hello kódot.</span><span class="sxs-lookup"><span data-stu-id="c4b27-167">Substitute your values for hello **from** and **to** phone numbers, and ensure that you verify hello **from** phone number for your Twilio account prior toorunning hello code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize hello Twilio client.
    Twilio.init(accountSID, authToken);

    // Use hello Twilio-provided site for hello TwiML response.
    URI uri = new URI("http://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare tooand From numbers
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="c4b27-168">Az átadott toohello hello paraméterekkel kapcsolatos további információért **Call.creator** módszer, lásd: [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="c4b27-168">For more information about hello parameters passed in toohello **Call.creator** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="c4b27-169">Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ.</span><span class="sxs-lookup"><span data-stu-id="c4b27-169">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="c4b27-170">Ehelyett használhat saját hely tooprovide hello TwiML válasz; További információkért lásd: [hogyan tooProvide TwiML válaszok az Azure Java-alkalmazások](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="c4b27-170">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses in a Java Application on Azure](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="c4b27-171"><a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="c4b27-171"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="c4b27-172">hello következő bemutatja, hogyan egy SMS üzenetet használatával toosend hello **üzenet** osztály.</span><span class="sxs-lookup"><span data-stu-id="c4b27-172">hello following shows how toosend an SMS message using hello **Message** class.</span></span> <span data-ttu-id="c4b27-173">Hello **a** szám **4155992671**, a próbaverzió fiókok toosend SMS-üzenetek Twilio biztosítja.</span><span class="sxs-lookup"><span data-stu-id="c4b27-173">hello **from** number, **4155992671**, is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="c4b27-174">Hello **való** szám ellenőrizni kell a Twilio fiók előzetes toorunning hello kódot.</span><span class="sxs-lookup"><span data-stu-id="c4b27-174">hello **to** number must be verified for your Twilio account prior toorunning hello code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize hello Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare tooand From numbers and hello Body of hello SMS message
    PhoneNumber too= new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, tooand Body values
    // then send hello SMS message by calling hello create() method
    Message sms = Message.creator(to, from, body).create();
```

<span data-ttu-id="c4b27-175">Az átadott toohello hello paraméterekkel kapcsolatos további információért **Message.creator** módszer, lásd: [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span><span class="sxs-lookup"><span data-stu-id="c4b27-175">For more information about hello parameters passed in toohello **Message.creator** method, see [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span></span>

## <span data-ttu-id="c4b27-176"><a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok</span><span class="sxs-lookup"><span data-stu-id="c4b27-176"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="c4b27-177">Ha az alkalmazás indít el egy hívás toohello Twilio API, például keresztül hello **CallCreator.create** metódus, Twilio küldi a kérelmet tooa URL-Címeket várt tooreturn TwiML választ.</span><span class="sxs-lookup"><span data-stu-id="c4b27-177">When your application initiates a call toohello Twilio API, for example via hello **CallCreator.create** method, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="c4b27-178">hello a fenti példában hello Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="c4b27-178">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="c4b27-179">(TwiML Web Services használatra szolgál, míg megtekintheti hello TwiML a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="c4b27-179">(While TwiML is designed for use by Web services, you can view hello TwiML in your browser.</span></span> <span data-ttu-id="c4b27-180">Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres  **&lt;válasz&gt;**  elem; másik példaként, kattintson a [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] toosee egy  **&lt;válasz&gt;**  elem, amely tartalmazza a  **&lt;szóbeli &gt;**  elem.)</span><span class="sxs-lookup"><span data-stu-id="c4b27-180">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty **&lt;Response&gt;** element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21][twimlet_message_url_hello_world] toosee a **&lt;Response&gt;** element that contains a **&lt;Say&gt;** element.)</span></span>

<span data-ttu-id="c4b27-181">Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját URL-cím hely, amely HTTP-válaszokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c4b27-181">Instead of relying on hello Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="c4b27-182">Hello helyet létrehozhat bármilyen nyelven, amely visszaadja a HTTP-válaszok; Ez a témakör azt feltételezi, hogy lesz JSP lapon hello URL-címet futtató.</span><span class="sxs-lookup"><span data-stu-id="c4b27-182">You can create hello site in any language that returns HTTP responses; this topic assumes you'll be hosting hello URL in a JSP page.</span></span>

<span data-ttu-id="c4b27-183">JSP lap eredmények TwiML választ, amely szerint a következő hello **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="c4b27-183">hello following JSP page results in a TwiML response that says **Hello World!**</span></span> <span data-ttu-id="c4b27-184">hello hívásakor.</span><span class="sxs-lookup"><span data-stu-id="c4b27-184">on hello call.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="c4b27-185">JSP lap eredmények TwiML választ, amely szerint a szöveget, a következő hello több szünet rendelkezik, és hello Twilio API-verziót és hello Azure szerepkörnév kapcsolatos információk szerint.</span><span class="sxs-lookup"><span data-stu-id="c4b27-185">hello following JSP page results in a TwiML response that says some text, has several pauses, and says information about hello Twilio API version and hello Azure role name.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>hello Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>hello Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

<span data-ttu-id="c4b27-186">Hello **ApiVersion** paraméter érhető el a Twilio hang kérések (SMS kérelmek nem).</span><span class="sxs-lookup"><span data-stu-id="c4b27-186">hello **ApiVersion** parameter is available in Twilio voice requests (not SMS requests).</span></span> <span data-ttu-id="c4b27-187">toosee hello elérhető kérelemben szereplő paraméterek Twilio hang-és SMS-kérelmeket, lásd: <https://www.twilio.com/docs/api/twiml/twilio_request> és <https://www.twilio.com/docs/api/twiml/sms/twilio_request >, illetve.</span><span class="sxs-lookup"><span data-stu-id="c4b27-187">toosee hello available request parameters for Twilio voice and SMS requests, see <https://www.twilio.com/docs/api/twiml/twilio_request> and <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectively.</span></span> <span data-ttu-id="c4b27-188">Hello **RoleName** környezeti változó is rendelkezésre áll egy Azure-telepítés részeként.</span><span class="sxs-lookup"><span data-stu-id="c4b27-188">hello **RoleName** environment variable is available as part of an Azure deployment.</span></span> <span data-ttu-id="c4b27-189">(Ha azt szeretné tooadd egyéni környezeti változókat, így azok sikerült felvenni a **System.getenv**, lásd: hello környezeti változók szakasz [vegyes szerepkör konfigurációs beállításai] [misc_role_config_settings].)</span><span class="sxs-lookup"><span data-stu-id="c4b27-189">(If you want tooadd custom environment variables so they could be picked up from **System.getenv**, see hello environment variables section at [Miscellaneous Role Configuration Settings][misc_role_config_settings].)</span></span>

<span data-ttu-id="c4b27-190">Miután a JSP lap tooprovide TwiML válaszok beállítása, használható hello JSP lap URL-címe hello hello átadott URL-cím hello **Call.creator** metódust.</span><span class="sxs-lookup"><span data-stu-id="c4b27-190">Once you have your JSP page set up tooprovide TwiML responses, use hello URL of hello JSP page as hello URL passed into hello **Call.creator** method.</span></span> <span data-ttu-id="c4b27-191">Például ha egy webes alkalmazás telepített MyTwiML tooan nevű Azure üzemeltetett szolgáltatás, és hello JSP lap hello nevét mytwiml.jsp, hello URL-címe túl átadhatók**Call.creator** hello következő látható:</span><span class="sxs-lookup"><span data-stu-id="c4b27-191">For example, if you have a Web application named MyTwiML deployed tooan Azure hosted service, and hello name of hello JSP page is mytwiml.jsp, hello URL can be passed too**Call.creator** as shown in hello following:</span></span>

```java
    // Declare tooand From numbers and hello URL of your JSP page
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="c4b27-192">Hello keresztül van lehetősége, hogy válaszol TwiML **VoiceResponse** osztály, amely hello található **com.twilio.twiml** csomag.</span><span class="sxs-lookup"><span data-stu-id="c4b27-192">Another option for responding with TwiML is via hello **VoiceResponse** class, which is available in hello **com.twilio.twiml** package.</span></span>

<span data-ttu-id="c4b27-193">Az Azure-ban Java Twilio használatával kapcsolatos további információkért lásd: [hogyan tooMake Azure Java-alkalmazásokban a telefonhívás használatával Twilio][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="c4b27-193">For additional information about using Twilio in Azure with Java, see [How tooMake a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="c4b27-194"><a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="c4b27-194"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="c4b27-195">Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c4b27-195">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="c4b27-196">Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="c4b27-196">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="c4b27-197"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4b27-197"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="c4b27-198">Most, hogy megismerte a Twilio szolgáltatáshoz hello hello alapjait, kövesse a további hivatkozások toolearn:</span><span class="sxs-lookup"><span data-stu-id="c4b27-198">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="c4b27-199">[Twilio biztonsági irányelvek][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="c4b27-199">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="c4b27-200">[Twilio útmutató és példakódot][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="c4b27-200">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="c4b27-201">[Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="c4b27-201">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="c4b27-202">[A Githubon Twilio][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="c4b27-202">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="c4b27-203">[Forduljon a támogatási tooTwilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="c4b27-203">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/docs
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
