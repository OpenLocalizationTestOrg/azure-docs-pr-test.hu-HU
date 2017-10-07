---
title: aaaTemplates
description: "Ez a témakör ismerteti a sablonok az Azure notification hubs használatával."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 0149f0c7473e5a4b952905bc8217582b58db2a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="templates"></a>Sablonok
## <a name="overview"></a>Áttekintés
Sablonok egy ügyfél alkalmazás toospecify hello formátummal tooreceive szeretnének hello értesítések engedélyezéséhez. Sablonokkal, az alkalmazások számos különböző előnnyel jár, beleértve a következőket hello is megvalósítható:

* A platform-független háttér
* Személyre szabott értesítések
* Ügyfélverzió függetlenség
* Egyszerű honosítása

Ez a szakasz két részletes példákat hogyan toouse sablonok toosend platform-független értesítések célcsoport-kezelési az eszközök különböző platformokon és toopersonalize szórási tooeach eszközt tartalmaz.

## <a name="using-templates-cross-platform"></a>Sablonok platformfüggetlen használatával
hello normál módon toosend leküldéses értesítések toosend minden értesítés esetén, amely toobe küldi el, egy adott hasznos tooplatform értesítési szolgáltatások, (WNS, APNS). Például egy riasztási tooAPNS toosend hello payload egy Json-objektum, a következő képernyő hello:

    {"aps": {"alert" : "Hello!" }}

a Windows Áruházbeli alkalmazások hasonló bejelentési üzenet toosend, hello XML-adattartalmat értéke a következő:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

Hasonló hasznos adat található MPNS (Windows Phone) és a GCM (Android) platformokhoz hozhat létre.

Ez a követelmény kényszeríti hello app háttér tooproduce különböző hasznos adat található minden egyes platformhoz, és hatékonyan miatt hello háttér felelős hello bemutató réteg hello alkalmazás részét. Egyes okok közé tartoznak a honosítás és grafikus elrendezések (különösen a Windows Áruházbeli alkalmazások, amelyek tartalmazzák a csempék különféle típusú értesítések).

hello Notification Hubs sablon szolgáltatás lehetővé teszi, hogy egy ügyfél app toocreate különleges regisztrációk, sablon regisztrációk, amelyek közé tartozik, továbbá toohello beállítás címkék, egy sablon neve. hello Notification Hubs sablon szolgáltatás egy alkalmazás tooassociate ügyféleszközök sablonok lehetővé teszi, hogy telepítések (ajánlott) vagy a regisztráció dolgozik. Megadott tartalom példák megelőző hello, hello csak platformfüggetlen információk is hello tényleges riasztási üzenete (Hello!). A sablon olyan hogyan tooformat a platformfüggetlen üzenet az adott ügyfél-alkalmazás regisztrációja hello az értesítési központ hello vonatkozó utasításokat. A fenti példa hello, üdvözlőüzenetére platform független ugyanazon a tulajdonságon: **message = Hello!**.

a következő kép hello fent folyamat hello mutatja be:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

hello sablont hello iOS app ügyfélregisztráció a következőképpen történik:

    {"aps": {"alert": "$(message)"}}

hello megfelelő sablon hello Windows Store ügyfél alkalmazás van:

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

Figyelje meg, hogy a tényleges üzenet hello hello kifejezés $(üzenet) van helyére. Ebben a kifejezésben hello értesítési központ arra utasítja, amikor adott regisztrációs, toobuild hello közös érték a kapcsolókat és azt követő üzenetet küld egy üzenet toothis.

Ha telepítési modell dolgozik, hello telepítési "sablon" billentyű rendelkezik több sablonok JSON. Ha alkalmazásregisztrációs modell dolgozik, hello ügyfélalkalmazás hozhat létre több regisztrációk rendelés toouse több sablon; például egy sablon figyelmeztetések és a csempe frissítések sablonját. Ügyfélalkalmazások adjunk natív regisztrációk (regisztráció nélküli sablonnal) és a sablon regisztráció.

Értesítési központ hello minden sablon egy értesítést küld, figyelembe véve, hogy tartoznak toohello nélkül ügyfél ugyanazt az alkalmazást. Ez a viselkedés használt tootranslate platformfüggetlen értesítések több értesítések be lehet. Például hello azonos platform független üzenet toohello értesítési központ zökkenőmentesen fordítható egy bejelentési értesítés és egy csempe frissítése erről tájékoztatva hello háttér toobe nélkül. Vegye figyelembe, hogy néhány platformon (például iOS) összeomlás több értesítések toohello ugyanarra az eszközre, ha elküldi őket egy rövid időn belül.

## <a name="using-templates-for-personalization"></a>Sablonok használata személyre szabása
Egy másik előnye toousing sablonok hello képességét toouse az értesítések a Notification Hubs tooperform regisztrációs személyre szabása. Vegyük példaként a időjárás-alkalmazást, amely egy csempe, amely hello időjárási feltételek megjeleníti az adott helyen. A felhasználó Celsius vagy Fahrenheit fok, és egyetlen vagy öt nap előrejelzés választhat. Sablonokkal, minden ügyfél az alkalmazástelepítés regisztrálhatnak hello formátuma (1 napos Celsius, 1 napos Fahrenheit, 5 nap Celsius, 5 nap Fahrenheit), és minden hello adatait tartalmazó egyetlen üzenet szükséges toofill azokat küldeni hello háttér sablonok (például egy öt nap az előrejelzések Celsius és Fahrenheit fokban megadva).

hello sablon hello egy egynapos Celsius az előrejelzések hőmérsékletek értéke a következő:

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

hello üzenet toohello értesítési központ tartalmaz minden hello következő tulajdonságai:

<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>

Ez a minta segítségével hello háttér csak egyetlen üzenetet küld anélkül, hogy az adott beállításoknál toostore hello az alkalmazásaik felhasználóit. a következő kép hello ebben a forgatókönyvben mutatja be:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-tooregister-templates"></a>Hogyan tooregister sablonok
tooregister sablonoknak hello telepítési modell (ajánlott) vagy hello alkalmazásregisztrációs modell, lásd: [regisztrációs felügyeleti](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Sablon kifejezés nyelve
Sablonok olyan, korlátozott tooXML vagy JSON-dokumentum formázását. Emellett csak elhelyezheti kifejezések adott helyek; például csomópont attribútumok vagy XML-értékek JSON karakterlánc-tulajdonságok értékeit.

a következő táblázat azt mutatja be hello nyelvi sablonok engedélyezett hello:

| kifejezés | Leírás |
| --- | --- |
| $(prop) |Hivatkozási tooan esemény tulajdonság hello megadott névvel. A tulajdonságnevek nem nagybetűk között. Ebben a kifejezésben hello tulajdonság szöveges értéket vagy üres karakterlánc oldja fel, ha hello tulajdonság nem található. |
| $(prop, n) |Mivel fent, de hello szöveg explicit módon levágva n karakterek, például $(cím, 20) levágja hello cím tulajdonság hello tartalmát: 20 karakter. |
| . (prop, n) |Fent, de hello szöveget a három pont van utótaggal, amelyhez hozzá van kapcsolva. hello teljes mérete hello levágva karakterlánc, és n karakternél nem hosszabb hello utótag. . (cím, 20) és a "Ez az hello címsora" eredményeinek tulajdonsága bemeneti **Ez a cím hello...** |
| %(prop) |Hasonló too$(name) azzal a különbséggel, hogy hello kimeneti URI-kódolású. |
| #(prop) |A JSON-sablonokban használt (például az iOS és Android sablonok).<br><br>A funkció akkor működik pontosan hello ugyanaz, mint a korábban megadott, kivéve ha szerepel a JSON-sablonokat (például Apple-sablonok) $(prop). Ebben az esetben, ha ez a funkció nem körülvett "{","}" (például "myJsonProperty':"#(név)"), és a Javascript formátumban, például RegExp szolgáltatást tooa szám értékel: (0 &#124; (&#91; 1 – 9 &#93; &#91; 0-9 & #93 ;*))(\. &#91; 0-9 &#93; +)? ((az e &#124; E) (+ &#124;-)? &#91; 0-9 &#93; +)?, majd hello kimeneti JSON egy számot.<br><br>Például "jelvény:"#(név)"válik"jelvények": 40 (és nem"40"). |
| "text" vagy "text" |A szövegkonstans. Literálok egyetlen vagy dupla idézőjelek közé tetszőleges szöveget tartalmaznak. |
| Kif1 + Kif2 |hello összefűzési operátor való csatlakozás egyetlen karakterlánccá egyesít két kifejezésen. |

hello kifejezések űrlapok megelőző hello bármelyike lehet.

Kapott használatakor hello teljes kifejezést kell körülvett az {}. Például {$(prop) + '-' + $(prop2)}. |

Például hello most nem egy érvényes XML-sablont:

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


Amint azt fent összefűzése használatakor kifejezések kell csomagolni, kapcsos zárójelek közé. Példa:

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

