---
title: "Hang-és SMS (Java) Twilio használata |} Microsoft Docs"
description: "Útmutató a telefonhívás, és a Twilio API szolgáltatás SMS üzenet küldése az Azure-on. A Kódminták Java nyelven."
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
ms.openlocfilehash: 5a1b2ffa160a31b639605242b651dc8d14e7a01b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a><span data-ttu-id="4e47d-104">Hang-és SMS képességei Java Twilio használata</span><span class="sxs-lookup"><span data-stu-id="4e47d-104">How to Use Twilio for Voice and SMS Capabilities in Java</span></span>
<span data-ttu-id="4e47d-105">Ez az útmutató bemutatja, hogyan Azure a Twilio API szolgáltatás közös programozási feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="4e47d-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="4e47d-106">A tárgyalt forgatókönyvekben szerepel, így a telefonhívás, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="4e47d-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="4e47d-107">A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: a [lépések](#NextSteps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="4e47d-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="4e47d-108"><a id="WhatIs"></a>Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="4e47d-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="4e47d-109">Twilio egy telefonos webszolgáltatás API, amely lehetővé teszi, hogy a meglévő webes nyelv és képességeik felhasználásával hang- és SMS alkalmazások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4e47d-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills to build voice and SMS applications.</span></span> <span data-ttu-id="4e47d-110">Twilio egy olyan külső szolgáltatás (nem az Azure szolgáltatásai és nem Microsoft-termékek).</span><span class="sxs-lookup"><span data-stu-id="4e47d-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="4e47d-111">**Twilio hang** lehetővé teszi, hogy az alkalmazások és a telefonhívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="4e47d-111">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="4e47d-112">**Twilio SMS** lehetővé teszi, hogy az alkalmazások és az SMS-üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="4e47d-112">**Twilio SMS** allows your applications to make and receive SMS messages.</span></span> <span data-ttu-id="4e47d-113">**Twilio-ügyfél** lehetővé teszi, hogy a meglévő internetes kapcsolattal, többek között a mobil kapcsolatok hang kommunikáció engedélyezése az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4e47d-113">**Twilio Client** allows your applications to enable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="4e47d-114"><a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal</span><span class="sxs-lookup"><span data-stu-id="4e47d-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="4e47d-115">Információk a díjszabásról Twilio érhető el: [Twilio árképzési][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="4e47d-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="4e47d-116">Az Azure-ügyfelek egy [a különleges ajánlat][special_offer]: 1000 szövegek szabad követel vagy bejövő perc 1000.</span><span class="sxs-lookup"><span data-stu-id="4e47d-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="4e47d-117">Iratkozzon fel a szolgáltatásokat, vagy további információért látogasson el [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="4e47d-117">To sign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>

## <span data-ttu-id="4e47d-118"><a id="Concepts"></a>Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="4e47d-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="4e47d-119">A Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="4e47d-119">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="4e47d-120">Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="4e47d-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="4e47d-121">A Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="4e47d-121">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="4e47d-122"><a id="Verbs"></a>Twilio-műveletek</span><span class="sxs-lookup"><span data-stu-id="4e47d-122"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="4e47d-123">Az API lehetővé teszi, hogy a Twilio használja műveletek; például a  **&lt;szóbeli&gt;**  parancs utasítja a Twilio hallhatóan hívás az üzenet kézbesítését.</span><span class="sxs-lookup"><span data-stu-id="4e47d-123">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="4e47d-124">A Twilio-műveletek listáját a következő:</span><span class="sxs-lookup"><span data-stu-id="4e47d-124">The following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="4e47d-125">**&lt;Telefonos kapcsolat&gt;**: a hívó kapcsolódik egy másik telefonon.</span><span class="sxs-lookup"><span data-stu-id="4e47d-125">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="4e47d-126">**&lt;Gyűjtsön&gt;**: adta meg a telefon billentyűzetén számjegyek gyűjti.</span><span class="sxs-lookup"><span data-stu-id="4e47d-126">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="4e47d-127">**&lt;Vonalbontás&gt;**: hívás véget ér.</span><span class="sxs-lookup"><span data-stu-id="4e47d-127">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="4e47d-128">**&lt;Lejátszási&gt;**: hangfájl lejátszása.</span><span class="sxs-lookup"><span data-stu-id="4e47d-128">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="4e47d-129">**&lt;Várólista&gt;**: vegye fel a hívók várólistába.</span><span class="sxs-lookup"><span data-stu-id="4e47d-129">**&lt;Queue&gt;**: Add the to a queue of callers.</span></span>
* <span data-ttu-id="4e47d-130">**&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.</span><span class="sxs-lookup"><span data-stu-id="4e47d-130">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="4e47d-131">**&lt;Rekord&gt;**: a hívó hang rögzíti, és a felvétel tartalmazó fájl URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4e47d-131">**&lt;Record&gt;**: Records the caller's voice and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="4e47d-132">**&lt;Átirányítási&gt;**: hívás vagy SMS irányítását átviszi a TwiML egy másik URL-címen.</span><span class="sxs-lookup"><span data-stu-id="4e47d-132">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="4e47d-133">**&lt;Elutasítása&gt;**: a Twilio-szám egy bejövő hívás elutasítása a nélkül, számlázási.</span><span class="sxs-lookup"><span data-stu-id="4e47d-133">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you.</span></span>
* <span data-ttu-id="4e47d-134">**&lt;Tegyük fel például&gt;**: konvertálja szöveg-beszéd átalakítás hívás készült.</span><span class="sxs-lookup"><span data-stu-id="4e47d-134">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="4e47d-135">**&lt;SMS&gt;**: SMS üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="4e47d-135">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="4e47d-136"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="4e47d-136"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="4e47d-137">TwiML olyan XML-alapú utasítások a Twilio-műveletek, amely tájékoztatja a Twilio hogyan lehet feldolgozni a hívást vagy SMS alapján.</span><span class="sxs-lookup"><span data-stu-id="4e47d-137">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="4e47d-138">Tegyük fel, a következő TwiML a szöveg volna átalakítása **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="4e47d-138">As an example, the following TwiML would convert the text **Hello World!**</span></span> <span data-ttu-id="4e47d-139">a beszédfelismerés.</span><span class="sxs-lookup"><span data-stu-id="4e47d-139">to speech.</span></span>

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="4e47d-140">Ha egy alkalmazás meghívja a Twilio API-t, az API paraméterek egyike az URL-címet, a TwiML választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4e47d-140">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="4e47d-141">Fejlesztési célra a Twilio által megadott URL-címek segítségével az alkalmazások által használt TwiML visszajelzést.</span><span class="sxs-lookup"><span data-stu-id="4e47d-141">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="4e47d-142">Akkor is elhelyezheti a TwiML válaszok létrehozásához a saját URL-címeket, és egy másik lehetőség az **TwiMLResponse** objektum.</span><span class="sxs-lookup"><span data-stu-id="4e47d-142">You could also host your own URLs to produce the TwiML responses, and another option is to use the **TwiMLResponse** object.</span></span>

<span data-ttu-id="4e47d-143">Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="4e47d-143">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="4e47d-144">A Twilio API-val kapcsolatos további információkért lásd: [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="4e47d-144">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="4e47d-145"><a id="CreateAccount"></a>Twilio-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e47d-145"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="4e47d-146">Amikor készen áll arra, hogy a Twilio-fiókot, feliratkozhat [próbálja Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="4e47d-146">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="4e47d-147">Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.</span><span class="sxs-lookup"><span data-stu-id="4e47d-147">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="4e47d-148">Amikor regisztrál egy Twilio-fiókja, kapni fog egy Fiókazonosító és egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="4e47d-148">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="4e47d-149">Is szükség lesz Twilio API-hívások indítása.</span><span class="sxs-lookup"><span data-stu-id="4e47d-149">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="4e47d-150">Jogosulatlan hozzáférés elkerülése érdekében a fiókját, hogy a hitelesítési jogkivonat biztonságának megőrzése.</span><span class="sxs-lookup"><span data-stu-id="4e47d-150">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="4e47d-151">A Fiókazonosítót és a hitelesítési jogkivonat megtekinthető a rendszer a [Twilio-konzol][twilio_console], a mezőket, címkével **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.</span><span class="sxs-lookup"><span data-stu-id="4e47d-151">Your account ID and authentication token are viewable at the [Twilio Console][twilio_console], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="4e47d-152"><a id="create_app"></a>Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e47d-152"><a id="create_app"></a>Create a Java Application</span></span>
1. <span data-ttu-id="4e47d-153">Szerezze be a Twilio-JAR, és adja hozzá a Java build elérési útját és a WAR-telepítési szerelvény.</span><span class="sxs-lookup"><span data-stu-id="4e47d-153">Obtain the Twilio JAR and add it to your Java build path and your WAR deployment assembly.</span></span> <span data-ttu-id="4e47d-154">A [https://github.com/twilio/twilio-java][twilio_java], töltse le a GitHub-adatforrások és létrehozhat saját JAR, vagy egy előre elkészített JAR (vagy anélkül függőségek) letöltése.</span><span class="sxs-lookup"><span data-stu-id="4e47d-154">At [https://github.com/twilio/twilio-java][twilio_java], you can download the GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
2. <span data-ttu-id="4e47d-155">Győződjön meg arról a JDK **cacerts** keystore az MD5 ujjlenyomat 67:CB:9 D Equifax biztonságos hitelesítésszolgáltató tanúsítványát tartalmazza: (a sorozatszám 35:DE:F4:CF és az SHA1 C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 ujjlenyomat D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="4e47d-155">Ensure your JDK's **cacerts** keystore contains the Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (the serial number is 35:DE:F4:CF and the SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="4e47d-156">Ez az tanúsítvány hitelesítésszolgáltatói tanúsítványa a [https://api.twilio.com] [ twilio_api_service] szolgáltatást, amelyet a Twilio API-k használatakor nevezik.</span><span class="sxs-lookup"><span data-stu-id="4e47d-156">This is the certificate authority (CA) certificate for the [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="4e47d-157">További információ biztosítása a JDK **cacerts** keystore tartalmaz a megfelelő CA-tanúsítvány című [tanúsítvány hozzáadása a Java Hitelesítésszolgáltatói tanúsítványok tárolójába][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="4e47d-157">For information about ensuring your JDK's **cacerts** keystore contains the correct CA certificate, see [Adding a Certificate to the Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="4e47d-158">Részletes utasítások a Twilio ügyféloldali kódtára a Javához készült érhetők el [hogyan végezheti el a telefonhívás használatával Twilio Azure Java-alkalmazásokban][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="4e47d-158">Detailed instructions for using the Twilio client library for Java are available at [How to Make a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="4e47d-159"><a id="configure_app"></a>Állítsa be az alkalmazását Twilio-könyvtárak használatára</span><span class="sxs-lookup"><span data-stu-id="4e47d-159"><a id="configure_app"></a>Configure Your Application to Use Twilio Libraries</span></span>
<span data-ttu-id="4e47d-160">A kód is hozzáadhat **importálása** utasításokat a felső részen a Twilio-csomagokat vagy az alkalmazásban használni kívánt osztályok számára a forrásfájlok.</span><span class="sxs-lookup"><span data-stu-id="4e47d-160">Within your code, you can add **import** statements at the top of your source files for the Twilio packages or classes you want to use in your application.</span></span>

<span data-ttu-id="4e47d-161">Java forrásfájljait tároló:</span><span class="sxs-lookup"><span data-stu-id="4e47d-161">For Java source files:</span></span>

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

<span data-ttu-id="4e47d-162">Java kiszolgáló lap (JSP) forrásfájljait tároló:</span><span class="sxs-lookup"><span data-stu-id="4e47d-162">For Java Server Page (JSP) source files:</span></span>

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
<span data-ttu-id="4e47d-163">Attól függően, hogy mely Twilio-csomagokat vagy osztályok szeretné használni, a **importálása** utasítások eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="4e47d-163">Depending on which Twilio packages or classes you want to use, your **import** statements may be different.</span></span>

## <span data-ttu-id="4e47d-164"><a id="howto_make_call"></a>Hogyan: végezhet</span><span class="sxs-lookup"><span data-stu-id="4e47d-164"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="4e47d-165">A következő bemutatja, hogyan végezheti el egy kimenő hívás használatával a **hívás** osztály.</span><span class="sxs-lookup"><span data-stu-id="4e47d-165">The following shows how to make an outgoing call using the **Call** class.</span></span> <span data-ttu-id="4e47d-166">Ezt a kódot a Twilio által biztosított hely is használja a Twilio Markup Language (TwiML) válasz vissza.</span><span class="sxs-lookup"><span data-stu-id="4e47d-166">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="4e47d-167">Helyettesítse a saját értékeit a **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a **a** telefonszám a Twilio-fiók a kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="4e47d-167">Substitute your values for the **from** and **to** phone numbers, and ensure that you verify the **from** phone number for your Twilio account prior to running the code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("http://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="4e47d-168">További információ a továbbított paraméterek a **Call.creator** módszer, lásd: [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="4e47d-168">For more information about the parameters passed in to the **Call.creator** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="4e47d-169">Ahogy azt korábban említettük, ez a kód Twilio által biztosított hely használatával a TwiML választ küldi vissza.</span><span class="sxs-lookup"><span data-stu-id="4e47d-169">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="4e47d-170">Helyette a saját webhely segítségével adja meg a TwiML válasz; További információkért lásd: [TwiML válaszok adja meg az Azure Java-alkalmazások hogyan](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="4e47d-170">You could instead use your own site to provide the TwiML response; for more information, see [How to Provide TwiML Responses in a Java Application on Azure](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="4e47d-171"><a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="4e47d-171"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="4e47d-172">A következő bemutatja, hogyan használ egy SMS üzenetet küldeni a **üzenet** osztály.</span><span class="sxs-lookup"><span data-stu-id="4e47d-172">The following shows how to send an SMS message using the **Message** class.</span></span> <span data-ttu-id="4e47d-173">A **a** szám **4155992671**, SMS-üzenetek küldéséhez a próba fiókok Twilio biztosítja.</span><span class="sxs-lookup"><span data-stu-id="4e47d-173">The **from** number, **4155992671**, is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="4e47d-174">A **való** számát a Twilio-fiókját a kód futtatása előtt ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="4e47d-174">The **to** number must be verified for your Twilio account prior to running the code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare To and From numbers and the Body of the SMS message
    PhoneNumber to = new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, To and Body values
    // then send the SMS message by calling the create() method
    Message sms = Message.creator(to, from, body).create();
```

<span data-ttu-id="4e47d-175">További információ a továbbított paraméterek a **Message.creator** módszer, lásd: [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span><span class="sxs-lookup"><span data-stu-id="4e47d-175">For more information about the parameters passed in to the **Message.creator** method, see [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span></span>

## <span data-ttu-id="4e47d-176"><a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok</span><span class="sxs-lookup"><span data-stu-id="4e47d-176"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="4e47d-177">Ha az alkalmazás kezdeményezi a Twilio API hívása például keresztül a **CallCreator.create** metódus, Twilio küldi a kérést TwiML választ várt URL-címet.</span><span class="sxs-lookup"><span data-stu-id="4e47d-177">When your application initiates a call to the Twilio API, for example via the **CallCreator.create** method, Twilio will send your request to a URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="4e47d-178">A fenti példában a Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="4e47d-178">The example above uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="4e47d-179">(Közben TwiML Web Services való használatra tervezték, megtekintheti a TwiML a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="4e47d-179">(While TwiML is designed for use by Web services, you can view the TwiML in your browser.</span></span> <span data-ttu-id="4e47d-180">Kattintson például [http://twimlets.com/message] [ twimlet_message_url] egy üres megjelenítéséhez  **&lt;válasz&gt;**  elem; másik példaként, kattintson a [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] megtekintéséhez egy  **&lt;válasz&gt;**  elem, amely tartalmazza a  **&lt;szóbeli &gt;**  elem.)</span><span class="sxs-lookup"><span data-stu-id="4e47d-180">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty **&lt;Response&gt;** element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21][twimlet_message_url_hello_world] to see a **&lt;Response&gt;** element that contains a **&lt;Say&gt;** element.)</span></span>

<span data-ttu-id="4e47d-181">Ahelyett, hogy a Twilio által megadott URL-címen, létrehozhat saját URL-cím hely, amely HTTP-válaszokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4e47d-181">Instead of relying on the Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="4e47d-182">A hely bármilyen nyelven HTTP-válaszok; visszaadó hozhat létre Ez a témakör azt feltételezi, hogy lesz a JSP lap URL-címet futtató.</span><span class="sxs-lookup"><span data-stu-id="4e47d-182">You can create the site in any language that returns HTTP responses; this topic assumes you'll be hosting the URL in a JSP page.</span></span>

<span data-ttu-id="4e47d-183">A következő JSP lap eredményez, amely szerint TwiML választ **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="4e47d-183">The following JSP page results in a TwiML response that says **Hello World!**</span></span> <span data-ttu-id="4e47d-184">a hívás.</span><span class="sxs-lookup"><span data-stu-id="4e47d-184">on the call.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="4e47d-185">A következő JSP lap egy TwiML választ, amely szerint a szöveget, több szünet van, és információk szerint a Twilio API-verzió és az Azure szerepkörnév eredményez.</span><span class="sxs-lookup"><span data-stu-id="4e47d-185">The following JSP page results in a TwiML response that says some text, has several pauses, and says information about the Twilio API version and the Azure role name.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

<span data-ttu-id="4e47d-186">A **ApiVersion** paraméter érhető el a Twilio hang kérések (SMS kérelmek nem).</span><span class="sxs-lookup"><span data-stu-id="4e47d-186">The **ApiVersion** parameter is available in Twilio voice requests (not SMS requests).</span></span> <span data-ttu-id="4e47d-187">A rendelkezésre álló kérelemben szereplő paraméterek Twilio hang-és SMS-kérelmeket, olvassa el <https://www.twilio.com/docs/api/twiml/twilio_request> és <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, illetve.</span><span class="sxs-lookup"><span data-stu-id="4e47d-187">To see the available request parameters for Twilio voice and SMS requests, see <https://www.twilio.com/docs/api/twiml/twilio_request> and <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectively.</span></span> <span data-ttu-id="4e47d-188">A **RoleName** környezeti változó is rendelkezésre áll egy Azure-telepítés részeként.</span><span class="sxs-lookup"><span data-stu-id="4e47d-188">The **RoleName** environment variable is available as part of an Azure deployment.</span></span> <span data-ttu-id="4e47d-189">(Ha hozzá szeretne adni az egyéni környezeti változók, így azok sikerült felvenni a **System.getenv**, lásd a témakör a környezeti változók [vegyes szerepkör konfigurációs beállításai] [ misc_role_config_settings].)</span><span class="sxs-lookup"><span data-stu-id="4e47d-189">(If you want to add custom environment variables so they could be picked up from **System.getenv**, see the environment variables section at [Miscellaneous Role Configuration Settings][misc_role_config_settings].)</span></span>

<span data-ttu-id="4e47d-190">Miután a TwiML visszajelzést beállítva JSP oldal, használja a JSP lap URL-átadott URL-CÍMÉT a **Call.creator** metódust.</span><span class="sxs-lookup"><span data-stu-id="4e47d-190">Once you have your JSP page set up to provide TwiML responses, use the URL of the JSP page as the URL passed into the **Call.creator** method.</span></span> <span data-ttu-id="4e47d-191">Például ha egy webes alkalmazás nevű központi telepítése egy Azure MyTwiML üzemeltetett szolgáltatás, és a JSP-oldal neve mytwiml.jsp, az URL-cím adható át **Call.creator** látható a következő módon:</span><span class="sxs-lookup"><span data-stu-id="4e47d-191">For example, if you have a Web application named MyTwiML deployed to an Azure hosted service, and the name of the JSP page is mytwiml.jsp, the URL can be passed to **Call.creator** as shown in the following:</span></span>

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="4e47d-192">Keresztül van lehetősége, hogy TwiML válaszol a **VoiceResponse** osztályt, amely megtalálható a **com.twilio.twiml** csomag.</span><span class="sxs-lookup"><span data-stu-id="4e47d-192">Another option for responding with TwiML is via the **VoiceResponse** class, which is available in the **com.twilio.twiml** package.</span></span>

<span data-ttu-id="4e47d-193">Az Azure-ban Java Twilio használatával kapcsolatos további információkért lásd: [hogyan végezheti el a telefonhívás használatával Twilio Azure Java-alkalmazásokban][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="4e47d-193">For additional information about using Twilio in Azure with Java, see [How to Make a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="4e47d-194"><a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="4e47d-194"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="4e47d-195">Az itt bemutatott példák Twilio lehetőséget biztosít webes API-k segítségével kihasználhatja az Azure alkalmazásról további Twilio-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="4e47d-195">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="4e47d-196">Teljes részletekért lásd: a [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="4e47d-196">For full details, see the [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="4e47d-197"><a id="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4e47d-197"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="4e47d-198">Most, hogy megismerte a Twilio szolgáltatáshoz alapjait, az alábbi hivatkozásokból további:</span><span class="sxs-lookup"><span data-stu-id="4e47d-198">Now that you've learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="4e47d-199">[Twilio biztonsági irányelvek][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="4e47d-199">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="4e47d-200">[Twilio útmutató és példakódot][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="4e47d-200">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="4e47d-201">[Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="4e47d-201">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="4e47d-202">[A Githubon Twilio][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="4e47d-202">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="4e47d-203">[Kérdezze meg a Twilio-támogatás][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="4e47d-203">[Talk to Twilio Support][twilio_support]</span></span>

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
