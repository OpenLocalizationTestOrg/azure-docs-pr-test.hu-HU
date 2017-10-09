---
title: "aaaAzure Mobile Engagement felhasználói felület - a Reach-kampány"
description: "Laern hogyan toocreate és használata az Azure Mobile Engagement leküldéses értesítéses kampányokkal kezelése"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2fe124a2-a86f-4136-81ba-a9d298ec798a
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 825e550ace63a34d1a90b10fa976a61eb15a6d04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-push-notification-campaigns"></a>Hogyan toocreate és leküldéses értesítéses kampányokkal kezelése
Hello hello felhasználói felület toocreate új leküldéses kampány Reach szakasza toosend leküldéses értesítés szükséges összes hello információk megadásával összetett képlet használható. hello-beállítások a leküldéses kampány változnak némileg hello négy típusok: közlemények, szavazások, Adatleküldések és Csempék (csak Windows Phone).

### <a name="option-applies-to"></a>A beállítás a következőkre vonatkozik:
* Nyelv: Az összes (közlemények, szavazások, Adatleküldések, Mozaik)
* A kampány: Összes (közlemények, szavazások, Adatleküldések, Mozaik)
* Értesítés: Közlemények, szavazások
* Tartalom: Az egyes kampány egyedi
* A célközönség: Összes (közlemények, szavazások, Adatleküldések, Mozaik)
* Időkereten belül: közlemények, szavazások, Csempéket
* Teszt: Az összes (közlemények, szavazások, Adatleküldések, Mozaik)

![Reach-Campaign1][20]

## <a name="languages"></a>Nyelvek
Hello nyelvek legördülő menü toosend a leküldéses toodevices toouse különböző nyelveken beállított egy másik verziója is használhatja. Alapértelmezés szerint minden eszköz azonos leküldéses, függetlenül attól, milyen nyelven be lettek állítva toouse hello fog kapni. Az eszköz set tooa különböző nyelvű felhasználók kapnak hello leküldéses hello alapértelmezett nyelvi verzióját. Hello leküldéses kampány beállítások lehetővé teszik toospecify alternatív tartalmát az egyes hello további nyelveket választ. 

![Reach-Campaign2][21]

### <a name="language-differences-apply-to"></a>Nyelvi különbségek vonatkoznak:
* Nyelvek: Egyedi nyelvek lehet kiválasztani hozzáadása toohello az alapértelmezett nyelven
* Kampány: Az összes nyelven azonos
* Értesítés: Az egyes nyelvekhez egyedi továbbá toohello alapértelmezett nyelv
* Tartalom: Az egyes nyelvekhez egyedi továbbá toohello alapértelmezett nyelv
* A célközönséget: Előfordulhat, hogy egy külön nyelvi feltételek alapján úgy szűri
* Időkereten belül: ugyanazt az összes nyelven
* Teszt: Küldhet tooeach nyelvi egyszerre

### <a name="supported-languages"></a>Támogatott nyelvek:
* Arab (ar) 
* Bolgár (bg) 
* Katalán (ca) 
* Kínai (zh) 
* Horvát (hr) 
* Czech (CS) 
* Dán (da) 
* Holland (Hollandia) 
* Angol (en) 
* Finn (fi) 
* Francia (fr) 
* Német (de) 
* Görög (el) 
* Héber (ő) 
* Hindi (nagy) 
* Magyar (hu) 
* Indonéziai (id) 
* Olasz () 
* Japán (ja) 
* Koreai (ko) 
* Lett (lv) 
* Litván (lt) 
* Maláj (macrolanguage) (ms) 
* Norvég Bokmål (NetBIOS) 
* Lengyel (pl) 
* Portugál (pt) 
* Román (ro) 
* Orosz (ru) 
* Szerb (sr) 
* Szlovák (sk) 
* Szlovén (SA) 
* Spanyol (es) 
* Svéd (sv) 
* Tagalog (tl) 
* Thai (CS) 
* Török (m) 
* Ukrán (uk) 
* Vietnami (VI.) 

## <a name="campaign"></a>Kampány
Használható hello kampány szakasz tooset hello nevét és kategóriáját a kampány, valamint tooignore hello célközönség szakasz a leküldéses kampány megtervezése, és inkább küldeni a kampány keresztül hello Reach API (és egyes elemek hello alacsony szintű leküldéses API). Kategóriák használható egy egyéni értesítési sablon toocontrol az alkalmazásbeli értesítésekben előre meghatározott beállítások alapján. A meglévő "kategóriák listája" hello Reach API használatával kaphat.

> [!WARNING]
> Hello "Ignore célközönség leküldéssel fogják megkapni toousers keresztül hello API" beállítás használatakor a Reach-kampány hello "Kampány" szakaszban nem automatikusan küldi hello kampány, szüksége lesz a toosend azt manuálisan hello Reach API.

![Reach-Campaign3][22]

### <a name="option-applies-to"></a>A beállítás a következőkre vonatkozik:
* Name: összes
* Kategória: Közlemények, szavazások
* Hagyja figyelmen kívül a célközönséget, leküldéssel fogják megkapni keresztül hello API toousers: összes

## <a name="notification"></a>Értesítés
Hello értesítési szakasz tooset alapbeállítások használata a leküldéses többek között: hello leküldéses, üdvözlőüzenetére, az alkalmazás lemezkép címe hello, vagy ha mellőzhető. Sok értesítési beállítások adott toohello platform, az eszköz. Kiválaszthatja, hogy a leküldéssel fogják megkapni "alkalmazás" vagy "alkalmazás" verzióról vagy mindkettőt. (Ne feledje, hogy a felhasználók is "részt" vagy "lemondja" a "kívüli alkalmazás" leküldéses értesítések: hello operációs rendszer szintű az eszközökön, és Azure Mobile Engagement sem fog tudni toooverride kell ezt a beállítást. Továbbá ne feledje, hogy hello Reach API "alkalmazásban" kezeli, és "out alkalmazás" leküldéses értesítések. hello leküldéses API lehet "app megszüntetésekor" túl leküldéses értesítések használt toohandle.) Leküldéses értesítések képek vagy HTML-tartalmakat, beleértve az alkalmazásban (Android SDK 2.1.0 vagy újabb, szükség leképezési kategóriák) alkalmazás vagy tooanother helyének kívül kapcsolásához mélyhivatkozással szabhatja testre. Hello ikon vagy iOS-jelvény módosíthatja, és küldje el a szöveget vagy webes tartalmat (html tartalom helyének URL-címe hivatkozás tooanother belül vagy kívül hello app előugró). Android-eszközök ring ellenőrizze vagy hello leküldéses együtt mozog is. (Ne feledje, hogy Ön lesz kell hello megfelelő, az Android SDK engedélyek manifest fájl tooring, vagy egy eszköz mozog.) Jelenleg nincs iparági szabványos az Android "összkép" méretek, óta méretek eltérőek minden olyan eszközre, de a szinte bármilyen képernyőméret használható 400 x 100 képek.

### <a name="delivery-types"></a>Kézbesítési típusok:
* Csak az alkalmazáson kívül: hello értesítési kézbesíti a rendszer, amikor hello felhasználói hello alkalmazás nem használja.
* hello kívüli alkalmazás csak értesítés egy tanúsítványra van szükség az Apple vagy a Google (APNS vagy a GCM-tanúsítvánnyal).
* Alkalmazáson belüli csak: hello értesítés jelenik meg, csak ha hello alkalmazás futása.
* hello értesítési hello Capptain kézbesítési rendszer tooreach hello felhasználót használja. Hello elrendezés/megjelenítését a leküldéses teljes mértékben testreszabható.
* Bármikor: Ez a beállítás biztosítja, hogy küldjön egy értesítést vagy hello alkalmazás fut-e.

![Reach-Campaign4][23]

### <a name="option-applies-to"></a>A beállítás a következőkre vonatkozik:
* Értesítés: Közlemények, szavazások

## <a name="content"></a>Tartalom
Hello tartalomszakasz toomodify hello tartalma a közlemények, szavazások, Adatleküldések és Csempék (csak Windows Phone) is használhatja. hello tartalom leküldéses kampányokra érték kampány adott toohello típusát. 

### <a name="see-also"></a>Lásd még:
* [Felhasználói felületének dokumentációja – Reach - tartalom leküldéses][Link 29]

![Reach-Campaign5][24]

## <a name="audience"></a>Célközönség
Hello célközönség szakasz toodefine elemek toolimit szabványos listáját is használhat, a kampány vagy -korlátok, a kampány testreszabott feltétel alapján. hello tooLimit beállítások szabványos készletét a célközönséget lehetővé teszi toopush tooeither új vagy régi felhasználók vagy csak a natív leküldéses felhasználók. Hello leküldéses kapják kvóta toolimit hello számos állíthat be. Manuálisan módosíthatja a kampány Mitől szűrt tooinclude hello kifejezése egy vagy több feltétel tootarget felhasználók. Írja be a célközönség kifejezés manuálisan. Ilyen kifejezésben egyértelműen meg kell határoznia hello kapcsolat feltételek között. Egy olyan feltételt, amely nagybetűvel kezdődhet, és nem tartalmazhat szóközt azonosítót írja le. hello kapcsolat hello feltételek között annak leírását használja "és", "vagy", "nem" operátor, valamint a "(",")". Példa: "Criterion1 vagy (Criterion1 és nem Criterion2)".

> [!NOTE]
> A kampányok szereplő nagy célközönség, hello kiszolgálóoldali célcsoportkezelés vizsgálatot lassú lehet, különösen akkor, ha úgy próbálja toostart több kampányokat hello azonos idő.

* Ha lehetséges csak start egy kampány egyszerre.
* A hello legfeljebb csak kezdő négy kampányok egyszerre.
* Csak tooyour az aktív felhasználók leküldéses (jelölőnégyzet "megszólítása csak olyan felhasználók, akik elérhetők natív leküldéssel" és "Csak az aktív felhasználók megszólítása"), hogy csak a felhasználók, akik továbbra is hello az alkalmazás telepítve van, és használja a beolvasott toobe kell.
  A célközönség van definiálva, miután hello használhatja ki, hány felhasználó fog kapni a leküldéses gomb toofind szimulálása. Ez fogja számítási ismert felhasználók által a célközönség (egy véletlenszerű felhasználói minta alapján becsült érték) potenciálisan megcélzott hello száma. Vegye figyelembe, hogy azok a felhasználók, akik eltávolították hello alkalmazást is részei a célközönségnek, de nem érhető el.

### <a name="see-also"></a>Lásd még:
* [Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]

![Reach-Campaign6][25]

### <a name="edit-expression"></a>Szerkesztés
![Reach-Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a>A korlát a célközönség-beállítás:
* Csak egy adott felhasználók alcsoportjának megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)
* Csak a régi felhasználók megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)
* Csak az új felhasználók megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)
* Csak a tétlen felhasználók megszólítása: közlemények, szavazások, Csempéket
* Csak az aktív felhasználók megszólítása: összes (közlemények, szavazások, Adatleküldések, Mozaik)
* Csak olyan felhasználók, akik elérhetők natív leküldéssel megszólítása: közlemények, szavazások

## <a name="time-frame"></a>Időkereten belül
Hello időkereten belül szakasz tooset hello leküldéssel fogják megkapni, vagy hagyhatja hello időkereten belül üres toostart hello kampány azonnal használható. Ne feledje, hogy hello végfelhasználók időzóna használatával lehet, hogy Kezdőnap hello kampány egy korábbi, mint a végfelhasználók Ázsiában kapjon, és leküldéses értesítések, amíg az összes időzóna hello időkereten belül hello world egyezés állítsa be a kampány kis kötegeinek küldése. Hello végfelhasználók számára időzóna használatával is lelassíthatják a kampányok mert toorequest hello idő hello telefonról hello leküldéses elindítása előtt rendelkezik.

> [!NOTE]
> Nélkül záródátumot gyorsítótárazásával kampányokra leküldéses értesítések helyileg és továbbra is megjelenítésükhöz követően manuálisan teljes kampányok. tooavoid ezt a viselkedést, a specifikus kampányok befejezési idő.

### <a name="see-also"></a>Lásd még:
* [A reach - hogyan Tos – ütemezése][Link 3] 

![Reach-Campaign8][27]

### <a name="settings-apply-to"></a>Beállítások érvényesek:
* Időkereten belül: közlemények, szavazások, Csempéket

## <a name="test"></a>Tesztelés
Használhat hello teszt szakasz toosend leküldéses tooyour saját vizsgálati eszköz hello kampány mentéséhez. Ha konfigurálta a kampány bármilyen egyéni nyelveit, tesztelheti hello leküldéses mindkét nyelven. Egy vizsgálati eszköz "Saját fiók" beállítása.

> [!NOTE]
> Nem a rendszer adatokat naplózza hello gomb használatakor kiszolgálóoldali túl "teszt" leküldéses értesítések, adatokat csak naplózza a valódi leküldéses kampányokra.

### <a name="see-also"></a>Lásd még:
* [Felhasználói felület dokumentáció - fiókomat][Link 14]

![Reach-Campaign9][28]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

