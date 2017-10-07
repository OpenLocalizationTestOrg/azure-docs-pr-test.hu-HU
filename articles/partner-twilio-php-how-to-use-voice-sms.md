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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-php"></a>Hogyan tooUse Twilio hang-és a PHP SMS képességei
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás. a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor. A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.

## <a id="WhatIs"></a>Mi az, hogy a Twilio?
Twilio hello jövőbeli üzleti kommunikáció működtetéséhez, amely lehetővé teszi a fejlesztők tooembed hang, a VoIP, és alkalmazásokba üzenetküldési. Ezek virtualizálása hello Twilio kommunikációs API-platformon keresztül, az ilyen felhőalapú, a globális környezetben szükséges összes infrastruktúrát. Olyan egyszerű toobuild és méretezhető. Élvezze a rugalmasságot Önnel fizetési-, nyissa meg árképzési, és igénybe vehesse az felhő megbízhatóság.

**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja. **Twilio SMS** lehetővé teszi, hogy az alkalmazás toosend és a szöveges üzeneteket. **Twilio-ügyfél** lehetővé teszi a toomake VoIP hívást a telefon, a táblagép vagy a böngésző és WebRTC támogatja.

## <a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal
Az Azure-ügyfelek egy [a különleges ajánlat](http://www.twilio.com/azure): udvarias $10 Twilio-kredit a Twilio-fiók frissítésekor. A Twilio-jóváírási alkalmazott tooany Twilio-használati ($10 jóváírás egyenértékű toosending akár 1000 SMS-t vagy mentése too1000 fogadás bejövő hang perc, attól függően, hogy a telefon és üzenet vagy hívás cél hello helye) lehet. A Twilio-jóváírási beváltani és első lépések: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio egy olyan használatalapú szolgáltatás. Nincsenek nem beállításról díjakat, és bármikor bezárhatja a fiókját. További részletei: [Twilio árképzési][twilio_pricing].

## <a id="Concepts"></a>Alapfogalmak
hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára. Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].

Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-műveletek
hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.

hello az alábbiakban olvashat egy listát a Twilio-műveleteket. Ismerje meg, más műveletek és a képességek keresztül készül hello [Twilio Markup Language dokumentáció](http://www.twilio.com/docs/api/twiml).

* **&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.
* **&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.
* **&lt;Vonalbontás&gt;**: hívás véget ér.
* **&lt;Lejátszási&gt;**: hangfájl lejátszása.
* **&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.
* **&lt;Rekord&gt;**: hello hívó hang rögzíti és hello rögzítése tartalmazó fájl URL-címet adja vissza.
* **&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.
* **&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio szám anélkül, hogy számlázási
* **&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.
* **&lt;SMS&gt;**: SMS üzenetet küld.

### <a id="TwiML"></a>TwiML
TwiML egy olyan XML-alapú utasítások alapján, amely tájékoztatja, hogy hogyan Twilio hello Twilio-műveletek tooprocess hívás vagy SMS.

Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Ha az alkalmazás hívások hello Twilio API, hello API paraméterek egyike hello URL-címet, amely hello TwiML választ ad vissza. Fejlesztési célra Twilio által megadott URL-címek tooprovide hello TwiML válaszok az alkalmazások által használt is használhatja. Akkor is elhelyezheti a saját URL-címek tooproduce hello TwiML válaszokat, és egy másik lehetőség toouse hello **TwiMLResponse** objektum.

Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml]. Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio-fiók létrehozása
Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio]. Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.

Amikor regisztrál egy Twilio-fiókja, kapni fog egy Fiókazonosító és egy hitelesítési jogkivonatot. Egyaránt lesz szükséges toomake Twilio API-hívásokat. nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága. A Fiókazonosítót és a hitelesítési jogkivonat megtekinthető a hello [Twilio lapra][twilio_account], a mezők feliratú hello **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.

## <a id="create_app"></a>PHP-alkalmazás létrehozása
A PHP-alkalmazások, amelyek hello Twilio szolgáltatást használ, és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más PHP-alkalmazások hello Twilio szolgáltatást használ. Twilio-szolgáltatások REST-alapú, és a php-ből többféle módon is nevezik, ez a cikk összpontosítanak hogyan toouse Twilio szolgáltatásokhoz [Twilio-könyvtár a PHP a Githubról][twilio_php]. Php-hez tartozó hello Twilio-könyvtár használatával kapcsolatos további információkért lásd: [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Részletes útmutatást létrehozása és telepítése a Twilio/PHP-alkalmazás tooAzure érhetők el [hogyan tooMake a PHP-alkalmazások az Azure-on a telefonhívás használatával Twilio][howto_phonecall_php].

## <a id="configure_app"></a>Alkalmazás tooUse Twilio szalagtárak konfigurálása
Az alkalmazás toouse hello Twilio-könyvtár PHP kétféleképpen konfigurálható:

1. Töltse le a Githubról PHP hello Twilio-könyvtár ([https://github.com/twilio/twilio-php][twilio_php]), és adja hozzá a hello **szolgáltatások** directory tooyour alkalmazás.
   
    -VAGY-
2. Telepítse a PHP hello Twilio könyvtár KÖRTEFÁK csomag. A következő parancsok hello telepíthető:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Miután telepítette a hello Twilio könyvtár php, amelyet ezután felvehet egy **require_once** utasítás a PHP hello tetején fájlok tooreference hello könyvtár:

        require_once 'Services/Twilio.php';

További információkért lásd: [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Hogyan: végezhet
hello következő bemutatja, hogyan egy kimenő toomake hívás, hello használatával **Services_Twilio** osztály. Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ. Helyettesítse a saját értékeit hello **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszámot a Twilio fiók előzetes toorunning hello kódot.

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

Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ. Ehelyett használhat saját hely tooprovide hello TwiML válasz; További információkért lásd: [hogyan tooProvide TwiML válaszok a saját webhelyről](#howto_provide_twiml_responses).

* **Megjegyzés:**: tootroubleshoot SSL tanúsítvány érvényesítési hibákat, tekintse meg [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése
hello következő bemutatja, hogyan egy SMS üzenetet használatával toosend hello **Services_Twilio** osztály. Hello **a** szám Twilio biztosítja a próbaverzió fiókok toosend SMS-t. Hello **való** szám ellenőrizni kell a Twilio fiók előzetes toorunning hello kódot.

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

## <a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok
Ha az alkalmazás egy hívás toohello Twilio API, Twilio elküldi a kérelem tooa URL-CÍMÉT, amely tooreturn TwiML választ várt. hello a fenti példában hello Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url]. (Közben TwiML a Twilio végzi, megtekintheti a böngészőben hello azt. Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres `<Response>` elem; másik példaként, kattintson a [http://twimlets.com/message? % 5B0 üzenet %5 D Hello % 20World =] [ twimlet_message_url_hello_world] toosee egy `<Response>` elem, amely tartalmazza egy `<Say>` elem.)

Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját webhelyén, amely HTTP-válaszokat ad vissza. Hello helyet létrehozhat bármilyen nyelven, amely visszaadja az XML-válaszok; Ez a témakör azt feltételezi, hogy a PHP toocreate hello TwiML fogja használni.

a PHP lap eredmények TwiML választ, amely szerint a következő hello **Hello World** hello hívásakor.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Ahogy látja, a fenti példa hello, hello TwiML válasz egyszerűen egy XML-dokumentumot. hello Twilio php könyvtárban osztályokkal rendelkezik, amelyek az Ön TwiML hoz létre. az alábbi példa hello hello egyenértékű válasz eredményez, ahogy fent látható, de hello használ **szolgáltatások\_Twilio\_Twiml** osztály php hello Twilio könyvtárban:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

TwiML kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/twiml][twiml_reference]. 

Miután a tooprovide TwiML válaszok beállítása a PHP oldal, használja hello PHP lap URL-címe hello hello hello átadott URL-cím `Services_Twilio->account->calls->create` metódust. Ha egy webes alkalmazás neve például **MyTwiML** telepített tooan Azure üzemeltetett szolgáltatás, és hello neve hello PHP lap **mytwiml.php**, URL-címe túl átadhatók hello **Services_ Twilio -> fiók -> hívások -> hozzon létre** a hello a következő példában látható módon:

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

Az Azure-ban PHP Twilio használatával kapcsolatos további információkért lásd: [hogyan tooMake a PHP-alkalmazások az Azure-on a telefonhívás használatával Twilio][howto_phonecall_php].

## <a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal
Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás. Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].

## <a id="NextSteps"></a>Következő lépések
Most, hogy megismerte a Twilio szolgáltatáshoz hello hello alapjait, kövesse a további hivatkozások toolearn:

* [Twilio biztonsági irányelvek][twilio_security_guidelines]
* [Twilio útmutató és példakódot][twilio_howtos]
* [Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts] 
* [A Githubon Twilio][twilio_on_github]
* [Forduljon a támogatási tooTwilio][twilio_support]

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
