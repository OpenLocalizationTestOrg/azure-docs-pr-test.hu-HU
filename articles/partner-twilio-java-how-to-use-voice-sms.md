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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-java"></a>Hogyan tooUse Twilio hang-és Java SMS képességei
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás. a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor. A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.

## <a id="WhatIs"></a>Mi az, hogy a Twilio?
Twilio egy telefonos webszolgáltatás API, amely lehetővé teszi, hogy a meglévő webes nyelv és képességek toobuild hang- és SMS-alkalmazások. Twilio egy olyan külső szolgáltatás (nem az Azure szolgáltatásai és nem Microsoft-termékek).

**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja. **Twilio SMS** lehetővé teszi, hogy az alkalmazások toomake és az SMS-üzeneteket fogadni. **Twilio-ügyfél** lehetővé teszi az alkalmazások tooenable hang kommunikációt meglévő hálózati kapcsolatok használata esetén, beleértve a mobil kapcsolatok.

## <a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal
Információk a díjszabásról Twilio érhető el: [Twilio árképzési][twilio_pricing]. Az Azure-ügyfelek egy [a különleges ajánlat][special_offer]: 1000 szövegek szabad követel vagy bejövő perc 1000. Ez feliratkozott toosign ajánlat vagy további információért, látogasson el a [http://ahoy.twilio.com/azure][special_offer].

## <a id="Concepts"></a>Alapfogalmak
hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára. Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].

Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-műveletek
hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.

hello az alábbiakban olvashat egy listát a Twilio-műveleteket.

* **&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.
* **&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.
* **&lt;Vonalbontás&gt;**: hívás véget ér.
* **&lt;Lejátszási&gt;**: hangfájl lejátszása.
* **&lt;Várólista&gt;**: hello tooa várólista hívók hozzáadása.
* **&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.
* **&lt;Rekord&gt;**: hello hívó hang rögzíti és hello rögzítése tartalmazó fájl URL-címet adja vissza.
* **&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.
* **&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio-szám nélkül számlázási meg.
* **&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.
* **&lt;SMS&gt;**: SMS üzenetet küld.

### <a id="TwiML"></a>TwiML
TwiML egy olyan XML-alapú utasítások alapján, amely tájékoztatja, hogy hogyan Twilio hello Twilio-műveletek tooprocess hívás vagy SMS.

Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World!** toospeech.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

Ha az alkalmazás hívások hello Twilio API, hello API paraméterek egyike hello URL-címet, amely hello TwiML választ ad vissza. Fejlesztési célra Twilio által megadott URL-címek tooprovide hello TwiML válaszok az alkalmazások által használt is használhatja. Akkor is elhelyezheti a saját URL-címek tooproduce hello TwiML válaszokat, és egy másik lehetőség toouse hello **TwiMLResponse** objektum.

Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml]. Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio-fiók létrehozása
Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio]. Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.

Amikor regisztrál egy Twilio-fiókja, kapni fog egy Fiókazonosító és egy hitelesítési jogkivonatot. Egyaránt lesz szükséges toomake Twilio API-hívásokat. nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága. A Fiókazonosítót és a hitelesítési jogkivonat megtekinthető a hello [Twilio-konzol][twilio_console], a mezők feliratú hello **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.

## <a id="create_app"></a>Java-alkalmazás létrehozása
1. Szerezze be a Twilio-JAR hello, és vegye fel tooyour Java elérési út és a WAR-telepítési szerelvény hozhat létre. A [https://github.com/twilio/twilio-java][twilio_java], hello GitHub forrásból töltse le és létrehozhat saját JAR, vagy egy előre elkészített JAR (vagy anélkül függőségek) letöltése.
2. Győződjön meg arról a JDK **cacerts** keystore hello Equifax biztonságos hitelesítésszolgáltató tanúsítványát az MD5 ujjlenyomat 67:CB:9 D tartalmazza: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello sorozatszám 35:DE:F4:CF és hello SHA1 ujjlenyomat D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Ez az hello tanúsítvány hitelesítésszolgáltatói tanúsítvány hello [https://api.twilio.com] [ twilio_api_service] szolgáltatást, amelyet a Twilio API-k használatakor nevezik. További információ biztosítása a JDK **cacerts** keystore tartalmaz hello megfelelő CA-tanúsítvány című [hozzáadása egy tanúsítvány toohello Java hitelesítésszolgáltató tanúsítványtároló][add_ca_cert].

Részletes útmutatásért hello Twilio ügyféloldali kódtár Java érhetők el [hogyan tooMake Azure Java-alkalmazásokban a telefonhívás használatával Twilio][howto_phonecall_java].

## <a id="configure_app"></a>Alkalmazás tooUse Twilio szalagtárak konfigurálása
A kód is hozzáadhat **importálása** hello Twilio-csomagok, vagy osztályokat, akkor a forrásfájlok hello tetején utasítások szeretné, hogy az alkalmazás toouse.

Java forrásfájljait tároló:

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

Java kiszolgáló lap (JSP) forrásfájljait tároló:

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
Attól függően, hogy mely Twilio-csomagokat vagy osztályok kívánt toouse, a **importálása** utasítások eltérő lehet.

## <a id="howto_make_call"></a>Hogyan: végezhet
hello következő bemutatja, hogyan egy kimenő toomake hívás, hello használatával **hívás** osztály. Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ. Helyettesítse a saját értékeit hello **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszámot a Twilio fiók előzetes toorunning hello kódot.

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

Az átadott toohello hello paraméterekkel kapcsolatos további információért **Call.creator** módszer, lásd: [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ. Ehelyett használhat saját hely tooprovide hello TwiML válasz; További információkért lásd: [hogyan tooProvide TwiML válaszok az Azure Java-alkalmazások](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése
hello következő bemutatja, hogyan egy SMS üzenetet használatával toosend hello **üzenet** osztály. Hello **a** szám **4155992671**, a próbaverzió fiókok toosend SMS-üzenetek Twilio biztosítja. Hello **való** szám ellenőrizni kell a Twilio fiók előzetes toorunning hello kódot.

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

Az átadott toohello hello paraméterekkel kapcsolatos további információért **Message.creator** módszer, lásd: [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok
Ha az alkalmazás indít el egy hívás toohello Twilio API, például keresztül hello **CallCreator.create** metódus, Twilio küldi a kérelmet tooa URL-Címeket várt tooreturn TwiML választ. hello a fenti példában hello Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url]. (TwiML Web Services használatra szolgál, míg megtekintheti hello TwiML a böngészőben. Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres  **&lt;válasz&gt;**  elem; másik példaként, kattintson a [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] toosee egy  **&lt;válasz&gt;**  elem, amely tartalmazza a  **&lt;szóbeli &gt;**  elem.)

Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját URL-cím hely, amely HTTP-válaszokat ad vissza. Hello helyet létrehozhat bármilyen nyelven, amely visszaadja a HTTP-válaszok; Ez a témakör azt feltételezi, hogy lesz JSP lapon hello URL-címet futtató.

JSP lap eredmények TwiML választ, amely szerint a következő hello **Hello World!** hello hívásakor.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

JSP lap eredmények TwiML választ, amely szerint a szöveget, a következő hello több szünet rendelkezik, és hello Twilio API-verziót és hello Azure szerepkörnév kapcsolatos információk szerint.

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

Hello **ApiVersion** paraméter érhető el a Twilio hang kérések (SMS kérelmek nem). toosee hello elérhető kérelemben szereplő paraméterek Twilio hang-és SMS-kérelmeket, lásd: <https://www.twilio.com/docs/api/twiml/twilio_request> és <https://www.twilio.com/docs/api/twiml/sms/twilio_request >, illetve. Hello **RoleName** környezeti változó is rendelkezésre áll egy Azure-telepítés részeként. (Ha azt szeretné tooadd egyéni környezeti változókat, így azok sikerült felvenni a **System.getenv**, lásd: hello környezeti változók szakasz [vegyes szerepkör konfigurációs beállításai] [misc_role_config_settings].)

Miután a JSP lap tooprovide TwiML válaszok beállítása, használható hello JSP lap URL-címe hello hello átadott URL-cím hello **Call.creator** metódust. Például ha egy webes alkalmazás telepített MyTwiML tooan nevű Azure üzemeltetett szolgáltatás, és hello JSP lap hello nevét mytwiml.jsp, hello URL-címe túl átadhatók**Call.creator** hello következő látható:

```java
    // Declare tooand From numbers and hello URL of your JSP page
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

Hello keresztül van lehetősége, hogy válaszol TwiML **VoiceResponse** osztály, amely hello található **com.twilio.twiml** csomag.

Az Azure-ban Java Twilio használatával kapcsolatos további információkért lásd: [hogyan tooMake Azure Java-alkalmazásokban a telefonhívás használatával Twilio][howto_phonecall_java].

## <a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal
Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás. Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].

## <a id="NextSteps"></a>Következő lépések
Most, hogy megismerte a Twilio szolgáltatáshoz hello hello alapjait, kövesse a további hivatkozások toolearn:

* [Twilio biztonsági irányelvek][twilio_security_guidelines]
* [Twilio útmutató és példakódot][twilio_howtos]
* [Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]
* [A Githubon Twilio][twilio_on_github]
* [Forduljon a támogatási tooTwilio][twilio_support]

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
