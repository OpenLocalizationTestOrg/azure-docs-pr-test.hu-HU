---
title: "aaaHow tooUse Twilio hang-és SMS (.NET) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. Kódminták .NET."
services: 
documentationcenter: .net
author: devinrader
manager: twilio
editor: 
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: f568da87ef15e9f540fee9674de31e983d4acb6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-from-azure"></a>Hogyan toouse Twilio hang-és SMS-képességek az Azure-ból
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello Twilio API az Azure-szolgáltatás. a tárgyalt hello forgatókönyvekben szerepel, így telefonhívást, és egy rövid üzenetet szolgáltatás (SMS) üzenet küldésekor. A Twilio- és hang- és SMS használata az alkalmazások további információkért lásd: hello [további lépések](#NextSteps) szakasz.

## <a id="WhatIs"></a>Mi az, hogy a Twilio?
Twilio hello jövőbeli üzleti kommunikáció működtetéséhez, amely lehetővé teszi a fejlesztők tooembed hang, a VoIP, és alkalmazásokba üzenetküldési. Ezek virtualizálása hello Twilio kommunikációs API-platformon keresztül, az ilyen felhőalapú, a globális környezetben szükséges összes infrastruktúrát. Olyan egyszerű toobuild és méretezhető. Élvezze a rugalmasságot az árképzés használatalapú fizetés,-felhő megbízhatóság igénybe.

**Twilio hang** lehetővé teszi, hogy az alkalmazások toomake és telefonhívásokat fogadja. **Twilio SMS** lehetővé teszi, hogy az alkalmazások toosend és az SMS-üzeneteket fogadni. **Twilio-ügyfél** lehetővé teszi a toomake VoIP hívást a telefon, a táblagép vagy a böngésző és WebRTC támogatja.

## <a id="Pricing"></a>Twilio árak és a különleges ajánlatokkal
Az Azure-ügyfelek egy [a különleges ajánlat](http://www.twilio.com/azure): udvarias $10 Twilio-kredit a Twilio-fiók frissítésekor. A Twilio-jóváírási alkalmazott tooany Twilio-használati ($10 jóváírás egyenértékű toosending akár 1000 SMS-t vagy mentése too1000 fogadás bejövő hang perc, attól függően, hogy a telefon és üzenet vagy hívás cél hello helye) lehet. A Twilio-jóváírási beváltani és első lépések [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio egy olyan használatalapú szolgáltatás. Nincsenek nem beállításról díjakat, és bármikor bezárhatja a fiókját. További részletei: [Twilio árképzési](http://www.twilio.com/voice/pricing).

## <a id="Concepts"></a>Alapfogalmak
hello Twilio API egy RESTful API, hang- és SMS-funkciókat biztosít az alkalmazások számára. Ügyféloldali kódtáraknál érhetők el több nyelven is; az útmutató, [Twilio API függvénytárai][twilio_libraries].

Hello Twilio API fő szempontjait Twilio-műveletek és Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-műveletek
hello API lehetővé teszi, hogy a Twilio használja műveletek; például hello  **&lt;szóbeli&gt;**  parancs utasítja Twilio tooaudibly kézbesítése hívás üzenet.

hello az alábbiakban olvashat egy listát a Twilio-műveleteket.  Ismerje meg, más műveletek és a képességek keresztül készül hello [Twilio Markup Language dokumentáció](http://www.twilio.com/docs/api/twiml).

* **&lt;Telefonos kapcsolat&gt;**: hello hívó tooanother phone csatlakozik.
* **&lt;Gyűjtsön&gt;**: hello telefon billentyűzetén beírt számjegyeket gyűjti.
* **&lt;Vonalbontás&gt;**: hívás véget ér.
* **&lt;Lejátszási&gt;**: hangfájl lejátszása.
* **&lt;Felfüggesztés&gt;**: Csendes megvárja-e a megadott számú másodpercnél tovább.
* **&lt;Rekord&gt;**: hello hívó hang rögzíti, és egy hello rögzítése tartalmazó fájl URL-CÍMÉT adja vissza.
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

## <a id="create_app"></a>Az Azure-alkalmazások létrehozása
Az Azure-alkalmazások, amelyen a Twilio-engedélyezett alkalmazás nem eltér a többi Azure alkalmazás. Hello Twilio .NET könyvtár hozzáadása és konfigurálása hello szerepkör toouse hello Twilio .NET-kódtárakra.
A kezdeti Azure-projekt létrehozása információkért lásd: [Azure-projekt létrehozása a Visual Studio][vs_project].

## <a id="configure_app"></a>Alkalmazás toouse Twilio szalagtárak konfigurálása
Twilio biztosít, hogy a Twilio tooprovide egyszerű és egyszerű módon toointeract hello Twilio REST API-t és a Twilio-ügyfél különböző aspektusainak burkolása .NET segítő szalagtárak toogenerate TwiML válaszokat.

Twilio öt szalagtárak biztosít a .NET-fejlesztők számára:
Részletes ismertetés|Leírás
---|---
Twilio.API|hello Twilio Alapkönyvtár tördelve hello Twilio REST API-t egy rövid .NET könyvtárban. A könyvtár a .NET, a Silverlight, a Windows Phone 7 és érhető el.
Twilio.TwiML|A .NET rövid módon toogenerate TwiML markup biztosít.
Twilio.MVC|ASP.NET MVC használó fejlesztők ezt a szalagtárat magában foglalja a TwilioController TwiML ActionResult és kérelem ellenőrző attribútuma.
Twilio.WebMatrix|A fejlesztők a Microsoft szabad WebMatrix fejlesztési eszköz segítségével a könyvtárban Razor szintaxis segítő különböző Twilio-műveletekhez.
Twilio.Client.Capability|Hello funkció token generátor hello Twilio ügyfél JavaScript SDK való használatra tartalmazza.

Vegye figyelembe, hogy a kódtárakat .NET 3.5-ös, a Silverlight 4 vagy a Windows Phone 7 vagy újabb szükséges.

a jelen útmutatóban hello minták hello Twilio.API könyvtár használatára.

hello szalagtárak lehet [hello NuGet package manager bővítmény segítségével telepített](http://www.twilio.com/docs/csharp/install) érhető el a Visual Studio 2010 too2015 fel.  hello forráskód üzemelteti [GitHub][twilio_github_repo], mely tartalmazza a teljes dokumentációt hello szalagtárak tartalmazó Wiki.

Alapértelmezés szerint a Microsoft Visual Studio 2010 NuGet 1.2-es verzióját telepíti. Hello Twilio-tárak telepítéséhez az 1.6-os NuGet vagy újabb verzióra. Információ a telepítésekor vagy frissítésekor NuGet: [http://nuget.org/][nuget].

> [!NOTE]
> tooinstall hello legújabb verziójának NuGet, előbb el kell távolítania hello betöltött verzió hello Visual Studio-kiterjesztés kezelőjének használatával. toodo Igen, futtatnia kell a Visual Studio rendszergazdaként. Ellenkező esetben hello Eltávolítás gomb le van tiltva.
>
>

### <a id="use_nuget"></a>tooadd hello Twilio szalagtárak tooyour Visual Studio-projekt:
1. Nyissa meg a megoldást a Visual Studióban.
2. Kattintson a jobb gombbal **hivatkozások**.
3. Kattintson a **NuGet-csomagok kezelése...**
4. Kattintson a **Online**.
5. Hello online a keresőmezőbe írja be a *twilio*.
6. Kattintson a **telepítése** hello Twilio-csomag.

## <a id="howto_make_call"></a>Hogyan: végezhet
hello következő bemutatja, hogyan egy kimenő toomake hívás, hello használatával **CallResource** osztály. Ezt a kódot is használ egy Twilio által biztosított hely tooreturn hello Twilio Markup Language (TwiML) választ. Helyettesítse a saját értékeit hello **való** és **a** telefonszámokat, és győződjön meg arról, hogy ellenőrizze a hello **a** telefonszám Twilio-fiókja hello kód futtatása előtt.

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use hello Twilio-provided site for hello TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set hello call From, To, and URL values toouse for hello call.
    // This sample uses hello sandbox number provided by
    // Twilio toomake hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

Az átadott toohello hello paraméterekkel kapcsolatos további információért **CallResource.Create** módszer, lásd: [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Ahogy azt korábban említettük, a kódot használja a Twilio által biztosított hely tooreturn hello TwiML választ. Ehelyett használhat saját hely tooprovide hello TwiML választ. További információkért lásd: [hogyan: Adjon meg TwiML válaszok a saját webhelyén](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Útmutató: az SMS-üzenet küldése
hello következő képernyőfelvétel egy SMS üzenetet használatával toosend hello hogyan **MessageResource** osztály. Hello **a** szám Twilio biztosítja a próbaverzió fiókok toosend SMS-t. Hello **való** szám ellenőrizni kell a Twilio-fiókjához hello kód futtatása előtt.

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    try
    {
        // Send an SMS message.
        var message = MessageResource.Create(
            to: new PhoneNumber("+12069419717"),
            from: new PhoneNumber("+14155992671"),
            body: "This is my SMS message.");
    }
    catch (TwilioException ex)
    {
        // An exception occurred making hello REST call
        Console.WriteLine(ex.Message);
    }

## <a id="howto_provide_twiml_responses"></a>Hogyan: Adja meg a saját webhelyén TwiML válaszok
Ha az alkalmazás kezdeményezi a Twilio API - hívás toohello például keresztül hello **CallResource.Create** metódus - Twilio a kérelem tooan URL-CÍMÉT, amely várt tooreturn TwiML választ küld. hello példát [hogyan: végezhet](#howto_make_call) használ hello Twilio-megadott URL-címet [http://twimlets.com/message] [ twimlet_message_url] tooreturn hello válasz.

> [!NOTE]
> Amíg TwiML web Services való használatra tervezték, hello TwiML tekintheti meg a böngészőben. Kattintson például [http://twimlets.com/message] [ twimlet_message_url] toosee egy üres &lt;válasz&gt; elem; másik példaként, kattintson a [http://twimlets.com/message ? Üzenet % 5B0 %5 D Hello % 20World =](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee egy &lt;válasz&gt; elem, amely tartalmazza egy &lt;szóbeli&gt; elem.
>
>

Ahelyett, hogy a hello Twilio-megadott URL-címet, létrehozhat saját URL-cím hely, amely HTTP-válaszokat ad vissza. Hello webhely HTTP-válaszok visszaadó eltérő nyelvű hozhat létre. Ez a témakör azt feltételezi, hogy lesz hello URL-CÍMÉT egy ASP.NET általános kezelő üzemeltetési.

ASP.NET-kezelő a következő hello létrehozza TwiML választ, amely szerint **Hello World** hello hívásakor.

    using System.Text;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {
            public void ProcessRequest(HttpContext context)
            {
                const string twiMLResponse =
                    "<Response><Say>Hello World.</Say></Response>";
                
                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.ContentEncoding = Encoding.UTF8;
                context.Response.Write(twiMLResponse);
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }
    
Ahogy látja, a fenti példa hello, hello TwiML válasz egyszerűen egy XML-dokumentumot. hello Twilio.TwiML könyvtárban osztályokkal rendelkezik, amelyek az Ön TwiML hoz létre. hello az alábbi példa hello egyenértékű válasz eredményez, ahogy fent látható, de használ hello **VoiceResponse** osztály.

    using System.Web;
    using Twilio.TwiML;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                var twiml = new VoiceResponse();
                twiml.Say("Hello World.");

                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.Write(twiml.ToString());
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

TwiML kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).

Egy módon tooprovide TwiML válaszok beállítása után átadhatók adott URL-cím toohello **CallResource.Create** metódust. Például, ha rendelkezik telepített MyTwiML tooan Azure-felhőszolgáltatásban nevű webalkalmazást, és az ASP.NET-kezelő hello nevét mytwiml.ashx, hello URL-cím átadhatók túl**CallResource.Create** látható módon a következő kód hello Példa:

    // This sample uses hello sandbox number provided by Twilio toomake hello call.
    // Place hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

Azure és az ASP.NET Twilio használatával kapcsolatos további információkért lásd: [hogyan toomake egy telefonhívási Twilio használata Azure webes szerepkörrel rendelkező][howto_phonecall_dotnet].

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: http://twimlets.com/message

[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[vs_project]:http://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:http://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
