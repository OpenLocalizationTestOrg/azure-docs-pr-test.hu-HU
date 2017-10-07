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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Hogyan tooUse Twilio hang-és Ruby SMS képességei
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás. a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor. A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [lépések](#NextSteps) szakasz.

## <a id="WhatIs"></a>Mi az, hogy a Twilio?
Twilio egy telefonos webszolgáltatás API, amely lehetővé teszi, hogy a meglévő webes nyelv és képességek toobuild hang- és SMS-alkalmazások. Twilio egy olyan külső szolgáltatás (nem az Azure szolgáltatásai és nem Microsoft-termékek).

**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja. **Twilio SMS** lehetővé teszi, hogy az alkalmazások toomake és az SMS-üzeneteket fogadni. **Twilio-ügyfél** lehetővé teszi az alkalmazások tooenable hang kommunikációt meglévő hálózati kapcsolatok használata esetén, beleértve a mobil kapcsolatok.

## <a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal
Információk a díjszabásról Twilio érhető el: [Twilio árképzési][twilio_pricing]. Az Azure-ügyfelek egy [a különleges ajánlat][special_offer]: 1000 szövegek szabad követel vagy bejövő perc 1000. Ez feliratkozott toosign ajánlat vagy további információért, látogasson el a [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Alapfogalmak
hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára. Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML egy olyan XML-alapú utasításokat, amelyek tájékoztatják, hogy hogyan Twilio tooprocess hívás vagy SMS.

Tegyük fel, a következő TwiML hello hello szöveg volna átalakítása **Hello World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Minden TwiML dokumentum rendelkezik `<Response>` , mint a legfelső szintű elem. Ott Twilio-műveletek toodefine hello viselkedést az alkalmazás használja.

### <a id="Verbs"></a>TwiML műveletek
Twilio-műveletek a következők: XML-címkék, amelyek megadják Twilio mi túl**tegye**. Például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet. 

hello az alábbiakban olvashat egy listát a Twilio-műveleteket.

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

Twilio műveletek, az attribútumokat és TwiML kapcsolatos további információkért lásd: [TwiML][twiml]. Hello Twilio API kapcsolatos további információkért lásd: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio-fiók létrehozása
Amikor készen áll a tooget Twilio-fiókja, Regisztráljon a [próbálja Twilio][try_twilio]. Egy ingyenes fiókot kezdődnie, és később frissítse a fiókját.

Amikor regisztrál egy Twilio-fiókot, később is lesz egy ingyenes telefonszámot az alkalmazáshoz. Egy fiók SID azonosítója és egy hitelesítési jogkivonatot is kap. Egyaránt lesz szükséges toomake Twilio API-hívásokat. nem engedélyezett tooprevent tooyour fiók eléréséhez, a hitelesítési jogkivonat biztonsága. A fiók SID azonosítója és a hitelesítési jogkivonat is megtekinthető, hello [Twilio fióklapját][twilio_account], a mezők feliratú hello **fiók SID** és **hitelesítési TOKEN** , illetve.

### <a id="VerifyPhoneNumbers"></a>Ellenőrizze a telefonszámok
Lehetősége van által Twilio hozzáadása toohello szám számok szabályozhatja (azaz a mobiltelefon vagy otthoni telefonszámot) számára az alkalmazások is ellenőrizheti. 

Információ tooverify egy telefonszám, lásd: [kezelése számok][verify_phone].

## <a id="create_app"></a>Ruby-alkalmazás létrehozása
Egy Ruby alkalmazás, amely hello Twilio szolgáltatást használ, és az Azure-ban fut. ugyanolyan helyzetet teremt, mint bármely más Ruby alkalmazás hello Twilio szolgáltatást használ. Amíg Twilio-szolgáltatások RESTful, és a Ruby többféleképpen hívható, ez a cikk összpontosítanak hogyan toouse Twilio szolgáltatásokhoz [Twilio segítő kódtára a Rubyhoz][twilio_ruby].

Első, [beállításról egy új Azure Linux virtuális gép] [ azure_vm_setup] tooact új Ruby webes alkalmazás gazdagépként. Ugorja át hello használata esetén egy sínek alkalmazáshoz, csak a telepítés hello VM hello létrehozását. Ellenőrizze, hogy egy külső port a 80-as és egy belső portjával 5000 hozzon létre egy végpontot.

Hello a következő példa a fogjuk használni [Sinatra][sinatra], egy nagyon egyszerű webes keretrendszer, a Ruby. De alapértékekkel használható hello Twilio segédkódtárba helyezni a Rubyhoz bármely más webes keretrendszer, többek között a Ruby sínen.

SSH-ból az új virtuális gép, és hozzon létre egy könyvtárat, az új alkalmazás. Adott könyvtárán belül hozzon létre egy Gemfile, és másolja hello kód bele a következő nevű fájlt:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Futtassa parancssorból hello `bundle install`. Ez a hello függőségeket telepíti. Ezután hozzon létre egy nevű fájlt `web.rb`. Ez lesz, ahol a webalkalmazás hello kód él. Illessze be a kódot bele a következő hello:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Ezen a ponton érdemes futtatni tudja hello hello parancs `ruby web.rb -p 5000`. Ez fogja léptetéses-legfeljebb 5000-es port kis webkiszolgáló. El tudja toobrowse toothis alkalmazást a böngészőben hello URL-cím felkeresésével beállításról az Azure virtuális gép számára. A webalkalmazás hello böngészőben érheti el, ha készen áll a toostart Twilio-alkalmazás elkészítése az Ön.

## <a id="configure_app"></a>Alkalmazás tooUse Twilio konfigurálása
A web app toouse hello Twilio könyvtár frissítése úgy állíthatja be a `Gemfile` tooinclude ezt a sort:

    gem 'twilio-ruby'

Hello parancssorban futtassa `bundle install`. Most nyissa meg a `web.rb` és a sor hello bal felső, így:

    require 'twilio-ruby'

Most, hogy most már minden beállítva toouse hello Twilio segédkódtárba helyezni a Rubyhoz a web app alkalmazásban.

## <a id="howto_make_call"></a>Hogyan: végezhet
a következő hello bemutatja, hogyan egy kimenő toomake hívja. Főbb fogalmait tartalmazza a REST API-hívások Ruby toomake hello Twilio segédkódtárba helyezni használja, és TwiML megjelenítése. Helyettesítse a saját értékeit hello **a** és **való** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszámot a Twilio fiók előzetes toorunning hello kódot.

Ez a funkció hozzáadása túl`web.md`:

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

Ha Ön nyílt felfelé `http://yourdomain.cloudapp.net/make_call` egy böngészőben, amely elindítja a hello hívás toohello Twilio API toomake hello telefonhívás. az első két paraméterek hello `client.account.calls.create` magától értetődő viszonylag: hello számú hello hívás `from` és hello számú hello hívás `to`. 

hello a harmadik paraméter (`url`), hogy a Twilio hello hívás csatlakoztatása után kérelmek milyen toodo tooget utasításokat hello URL-cím. Ebben az esetben azt URL-címet a telepítés (`http://yourdomain.cloudapp.net`), amely egy egyszerű TwiML dokumentumot ad vissza, és használja a hello `<Say>` néhány szöveg-beszéd átalakítás, és mondja ki fogadó "Hello Monkey" toohello személy hello művelet toodo hívja.

## <a id="howto_recieve_sms"></a>Útmutató: az SMS-üzenet fogadása
Az előző példában hello azt kezdeményezett egy **kimenő** telefonhívás. Ez alkalommal, most használja az, hogy a Twilio-előfizetési tooprocess során megadott telefonszám hello egy **bejövő** SMS-üzenet.

Első, a bejelentkezés tooyour [Twilio-irányítópult][twilio_account]. Kattintson a "Numbers" hello felső NAV, majd a hello Twilio-szám lett megadva. Látni fogja, két URL-címeket, amelyeket konfigurálhat. Egy hang kérelem URL-CÍMÉT és az SMS kérelem URL-CÍMÉT. Ezek hello URL-címek készített Twilio hívásokat, amikor egy telefonhívást vagy SMS küldött tooyour számát. hello URL-címek "webes hurkok" is nevezik.

Szeretnénk tooprocess bejövő SMS-t, így érdemes hello URL-címe túl frissítése`http://yourdomain.cloudapp.net/sms_url`. Lépjen tovább, és kattintson a módosítások mentése hello lap hello alján. Most, újból `web.rb` tegyük a programot az alkalmazás toohandle ezt:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for hello ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Hello módosítás után győződjön meg arról, hogy toore indítási a webes alkalmazást. Most vegye ki a telefonjára, és küldjön egy SMS-tooyour Twilio-szám. Azonnal kapja meg az SMS-válasz, amely szerint "Hey, hello ping Köszönjük! Twilio és az Azure rock! ".

## <a id="additional_services"></a>Útmutató: további Twilio-szolgáltatásokkal
Továbbá toohello példák Twilio kínál webes API-k, amely itt megjelenik tooleverage további Twilio-funkciót használhatja az Azure-alkalmazás. Teljes további információkért lásd: hello [Twilio API-JÁNAK dokumentációja][twilio_api_documentation].

### <a id="NextSteps"></a>Következő lépések
Most, hogy megismerte a Twilio szolgáltatáshoz hello hello alapjait, kövesse a további hivatkozások toolearn:

* [Twilio biztonsági irányelvek][twilio_security_guidelines]
* [Twilio HowTos és példakódot][twilio_howtos]
* [Twilio gyors üzembe helyezési oktatóanyag][twilio_quickstarts] 
* [A Githubon Twilio][twilio_on_github]
* [Forduljon a támogatási tooTwilio][twilio_support]

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
