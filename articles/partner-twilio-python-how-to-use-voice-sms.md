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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-python"></a>Hogyan tooUse Twilio hang-és Python SMS képességei
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás. a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor. A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.

## <a id="WhatIs"></a>Mi az, hogy a Twilio?
Twilio hello jövőbeli üzleti kommunikáció működtetéséhez, amely lehetővé teszi a fejlesztők tooembed hang, a VoIP, és alkalmazásokba üzenetküldési. Ezek virtualizálása hello Twilio kommunikációs API-platformon keresztül, az ilyen felhőalapú, a globális környezetben szükséges összes infrastruktúrát. Olyan egyszerű toobuild és méretezhető. Élvezze a rugalmasságot Önnel fizetési-, nyissa meg árképzési, és igénybe vehesse az felhő megbízhatóság.

**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja.
**Twilio SMS** lehetővé teszi, hogy az alkalmazás toosend és a szöveges üzeneteket.
**Twilio-ügyfél** lehetővé teszi a toomake VoIP hívást a telefon, a táblagép vagy a böngésző és WebRTC támogatja.

## <a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal
Az Azure-ügyfelek egy [a különleges ajánlat] [ special_offer] 10 $ Twilio-kredit a Twilio-fiók frissítésekor. A Twilio-jóváírási alkalmazott tooany Twilio-használati ($10 jóváírás egyenértékű toosending akár 1000 SMS-t vagy mentése too1000 fogadás bejövő hang perc, attól függően, hogy a telefon és üzenet vagy hívás cél hello helye) lehet. Ez beváltani [Twilio-jóváírási] [ special_offer] használatának első lépéseihez.

Twilio egy olyan használatalapú szolgáltatás. Nincsenek nem beállításról díjakat, és bármikor bezárhatja a fiókját. További részletei: [Twilio árképzési][twilio_pricing].

## <a id="Concepts"></a>Alapfogalmak
hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára. Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].

Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-műveletek
hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.

hello az alábbiakban olvashat egy listát a Twilio-műveleteket. Ismerje meg, más műveletek és a képességek keresztül készül hello [Twilio Markup Language dokumentáció][twiml].

* **&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.
* **&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.
* **&lt;Vonalbontás&gt;**: hívás véget ér.
* **&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.
* **&lt;Lejátszási&gt;**: hangfájl lejátszása.
* **&lt;Várólista&gt;**: hello tooa várólista hívók hozzáadása.
* **&lt;Rekord&gt;**: hello hang hello hívó rögzíti és hello rögzítése tartalmazó fájl URL-címet adja vissza.
* **&lt;Átirányítási&gt;**: átirányítja a hívást vagy SMS toohello TwiML egy másik URL-címen.
* **&lt;Elutasítása&gt;**: elutasítások egy bejövő hívás tooyour Twilio-szám nélkül számlázási meg.
* **&lt;Tegyük fel például&gt;**: konvertálja szöveg toospeech hívás készült.
* **&lt;SMS&gt;**: SMS üzenetet küld.

### <a id="TwiML"></a>TwiML
TwiML egy olyan XML-alapú utasítások alapján, amely tájékoztatja, hogy hogyan Twilio hello Twilio-műveletek tooprocess hívás vagy SMS.

Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Ha az alkalmazás hívások hello Twilio API, hello API paraméterek egyike hello URL-címet, amely hello TwiML választ ad vissza. Fejlesztési célra Twilio által megadott URL-címek tooprovide hello TwiML válaszok az alkalmazások által használt is használhatja. Akkor is elhelyezheti a saját URL-címek tooproduce hello TwiML válaszokat, és egy másik lehetőség toouse hello `TwiMLResponse` objektum.

Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml]. Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio-fiók létrehozása
Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio]. Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.

Amikor regisztrál egy Twilio-fiókja, kap egy fiók SID azonosítója és egy hitelesítési jogkivonatot. Egyaránt lesz szükséges toomake Twilio API-hívásokat. nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága. A fiók SID azonosítója és a hitelesítési jogkivonat hello megtekinthető [Twilio-konzol][twilio_console], a mezők feliratú hello **fiók SID** és **hitelesítési JOGKIVONAT**, illetve.

## <a id="create_app"></a>Python-alkalmazás létrehozása
Egy Python-alkalmazás, amely hello Twilio szolgáltatást használ, és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más Python alkalmazás hello Twilio szolgáltatást használ. Twilio-szolgáltatások REST-alapú, és a Python többféleképpen hívható, ez a cikk összpontosítanak hogyan toouse Twilio szolgáltatásokhoz [Twilio-kódtára a Pythonhoz a Githubról][twilio_python]. Python hello Twilio-könyvtár használatával kapcsolatos további információkért lásd: [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].

Első, a [telepítés egy új Azure Linux virtuális gép] [azure_vm_setup] tooact új Python webes alkalmazás gazdagépként. Miután hello virtuális gép fut, szükség van egy tooexpose az alkalmazás egy nyilvános portot az alábbiakban.

### <a name="add-an-incoming-rule"></a>Bejövő szabály felvétele
  1. Nyissa meg toohello [hálózati biztonsági csoport] [azure_nsg] lap.
  2. Válassza ki a hálózati biztonsági csoport, amely megfelel a virtuális gép hello.
  3. Adja hozzá és **kimenő szabály** a **80-as port**. Lehet, hogy tooallow bejövő bármilyen címről.

### <a name="set-hello-dns-name-label"></a>DNS-névcímke hello beállítása
  1. Nyissa meg toohello [hello nyilvános IP-címet adott meg] [azure_ips] lap.
  2. Válassza ki a hello nyilvános IP-címet, hogy correspends a virtuális géppel.
  3. Set hello **DNS-névcímke** a hello **konfigurációs** szakasz. A jelen példában hello esetben fog megjelenni ehhez hasonló *a tartománycímke*. centralus.cloudapp.azure.com

Ha le is tudja keresztül telepítheti a virtuális gép SSH toohello tooconnect hello webes keretrendszer az Ön által választott (hello két legtöbb közismert a Python kívánt [Flask](http://flask.pocoo.org/) és [Django](https://www.djangoproject.com)). Csak a hello futtatásával telepíthető valamelyiken `pip install` parancsot.

Ne feledje, hogy csak a 80-as porton konfigurált hello virtuális gép tooallow forgalmat. Ezért győződjön meg arról, hogy tooconfigure hello alkalmazás toouse ezt a portot.

## <a id="configure_app"></a>Alkalmazás tooUse Twilio szalagtárak konfigurálása
Az alkalmazás toouse hello Twilio-könyvtár Python kétféleképpen konfigurálható:

* Telepítse a Python hello Twilio könyvtár Pip csomag. A következő parancsok hello telepíthető:
   
        $ pip install twilio

    -VAGY-

* Töltse le a Githubról Python hello Twilio-könyvtár ([https://github.com/twilio/twilio-python][twilio_python]), a telepítés:

        $ python setup.py install

Miután telepítette a hello Twilio-könyvtár a Python, a következőket teheti majd `import` azt a Python-fájlokban:

        import twilio

További információkért lásd: [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Hogyan: végezhet
a következő hello bemutatja, hogyan egy kimenő toomake hívja. Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ. Helyettesítse a saját értékeit hello **from_number** és **to_number** telefonszámokat, és győződjön meg arról, hogy ellenőrizte, hogy hello **from_number** telefonszám Twilio-fiókja Mielőtt hello kód futtatása.

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

Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ. Ehelyett használhat saját hely tooprovide hello TwiML válasz; További információkért lásd: [hogyan tooProvide TwiML válaszok a saját webhelyről](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése
hello következő bemutatja, hogyan egy SMS üzenetet használatával toosend hello `TwilioRestClient` osztály. Hello **from_number** szám Twilio biztosítja a próbaverzió fiókok toosend SMS-t. Hello **to_number** szám ellenőrizni kell a Twilio-fiókjához hello kód futtatása előtt.

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

## <a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok
Ha az alkalmazás egy hívás toohello Twilio API, Twilio elküldi a kérelem tooa URL-CÍMÉT, amely tooreturn TwiML választ várt. hello a fenti példában hello Twilio-megadott URL-címet [http://twimlets.com/message][twimlet_message_url]. (TwiML által Twilio használatra szolgál, míg tekintheti a böngészőben. Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres `<Response>` elem; másik példaként, kattintson a [http://twimlets.com/message? % 5B0 üzenet %5 D Hello % 20World =] [ twimlet_message_url_hello_world] toosee egy `<Response>` elem, amely tartalmazza egy `<Say>` elem.)

Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját webhelyén, amely HTTP-válaszokat ad vissza. Hello helyet létrehozhat bármilyen nyelven, amely visszaadja az XML-válaszok; Ez a témakör azt feltételezi, hogy a Python toocreate hello TwiML fog használni.

hello alábbi példák kimeneteként TwiML választ, amely szerint **Hello World** hello hívásakor.

A Flask:

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

A django alkalmazással:

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

Ahogy látja, a fenti példa hello, hello TwiML válasz egyszerűen egy XML-dokumentumot. hello Twilio kódtára a Pythonhoz TwiML hoz létre az Ön osztályokat tartalmazza. hello az alábbi példa hello egyenértékű válasz eredményez, ahogy fent látható, de használ hello `twiml` hello Twilio könyvtárban Python modul:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

TwiML kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/twiml][twiml_reference].

Miután a Python-alkalmazás beállítása tooprovide TwiML válaszok, használható hello alkalmazás URL-címe hello hello hello átadott URL-cím `client.calls.create` metódust. Ha egy webes alkalmazás neve például **MyTwiML** telepített tooan Azure üzemeltetett szolgáltatásban futó, használja az URL-cím webhook, ahogy az alábbi példa hello:

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

## <a id="AdditionalServices"></a>Útmutató: további Twilio-szolgáltatásokkal
Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás. Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api].

## <a id="NextSteps"></a>Következő lépések
Most, hogy a Twilio szolgáltatáshoz hello hello alapjait megtanulta, kövesse a további hivatkozások toolearn:

* [Twilio biztonsági irányelvek][twilio_security_guidelines]
* [Útmutató a Twilio-útmutatók és példakódot][twilio_howtos]
* [Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts]
* [A Githubon Twilio][twilio_on_github]
* [Forduljon a támogatási tooTwilio][twilio_support]

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
