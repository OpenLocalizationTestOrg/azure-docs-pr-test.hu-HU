---
title: "Mobile Engagement felhasználói felület – aaaAzure elérni hogyan számára"
description: "Felhasználói felület és Azure Mobile Engagement áttekintése"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 30af87e6-c816-4cce-8609-6cbd3e83de14
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4b6dafd09d894214d4c386f5c6f157a77671606f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-using-and-managing-pushes-tooreach-out-tooyour-end-users"></a>Hogyan tooget használatának és a kimenő tooyour végfelhasználók tooreach kezelése leküldéses értesítések
Miután hello SDK teljesen integrálva van az alkalmazás, elkezdheti hello hello Reach szakaszának hello felhasználói felület tooPush értesítések toohello felhasználók az alkalmazás segítségével.  

## <a name="do-your-first-push-notification-campaign"></a>Hajtsa végre az első leküldéses Értesítéses kampány
* Győződjön meg arról, hogy a Reach integrálva van az alkalmazás a hello SDK. 
* Az alkalmazás kiválasztása

![First1][1]

* Nyissa meg toohello "Reach" szakaszban, és kattintson a "közlemény"

![First2][2]

* Hozzon létre egy új kampányt, és adjon neki nevet
  
![First3][3]

* Válassza ki, hogyan hello értesítési kézbesítési, mint alkalmazásbeli csak

![First4][4]

* Azt szeretné, hogy toopush hello üzenet létrehozása

![First5][5]

* A cím írhat hello értesítésre kattinthat (nem kötelező).
* Írás a leküldéses üzenet tartalma.
* Lemezkép feltöltése Vegye figyelembe, hogy a hello hello fájl mérete nem haladhatja meg a 32768 bájt.
* Hello képességét tooselect további beállítások is rendelkezik, de a jelen oktatóanyag hello felvennie a kijelölést, azt láthatja, hogy később.
* Csak az értesítésként hello tartalom típusának kiválasztása

![First6][6]

* A leküldéses kampány létrehozásához, és akkor jelenik meg a kampány listáján.

![First7][7]

## <a name="test-your-push-notification-campaign"></a>A leküldéses értesítési kampány tesztelése
![Test1][8]

* Regisztrálja az eszközt.
* Kattintson a hello jelölőnégyzetet az hello eszköz toopush szeretné.
* Kattintson a hello "Test" gomb toosend hello leküldéses toohello eszköz.

![Teszt2][9]

* Hello kampány aktiválása

![Teszt3][10]

* Most, hogy létrehozta a kampány egyszerűen tooactivate hello értesítési toobe a leküldött tooyour felhasználók.

## <a name="send-personalized-pushes"></a>Személyre szabott leküldéses értesítések küldése
* Ez a példa egy leküldéses, ahol meg egy egyéni visszatérítési kódja hello leküldéses értesítést hoz létre.

![Personalize1][11]

Személyre szabás működésével úgy lecserélésével a mutató által egy app-info címke a konfigurálnia kell a toomake, hello felhasználónak hello megfelelő app-info először definiálva van. Ez a példa hello megcélzott felhasználók rendelkeznek egy meghatározott rebate_code nevű alkalmazás info címke.
Ahogy fent látni hello leküldéses értesítési tartalom hello jelölő ${rebate_code}, amely jelzi, hogy a rendszer lecserélve a tényleges tartalom hello hello app-info címke toobe magában foglalja.

> [!WARNING]
> Ha hello app-info címke nincs definiálva a következő hello felhasználói, hello felhasználó nem kap hello leküldéses.

* eredménye

![Personalize2][12]

### <a name="you-can-further-personalize-hello-text-your-notification"></a>További szabhatja hello szöveg az értesítés
![Personalize3][13]

* Hello értesítési, többek között a következőket hello címe
* És üdvözlőüzenetére hello tartalmát.
* Hirdetmény (szöveges vagy webes nézetben) hello típusának kiválasztása

![Personalize4][14]

### <a name="hello-body-of-an-announcement-may-also-be-personalized-with"></a>bejelentés hello törzsét előfordulhat, hogy is az általunk:
* hello műveleti URL-cím, azt szeretné, hogy kezdőlapján toocustomize hello
* hello cím
* hello hello üzenet törzsében.

## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>A leküldéses értesítési megkülönböztetéséhez (a vagy alkalmazásból)
* Válassza ki a hello típusú értesítést fog leküldéses, válassza ki az alkalmazást, toohello "Reach" szakasz lépjen, vagy a leküldéses kampány létrehozásához, és nyissa meg toohello "Értesítések" szakasz.
* Kattintson a "szállítási mód" hello szeretné.
* Kattintson a hello "Korlátozása tevékenységek" jelölőnégyzetet, ha azt szeretné, hogy hello értesítési adott tevékenységei (képernyői) történik.

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Csak az alkalmazáson kívül" szállítási mód
![Differentiate2][16]

"Csak az alkalmazáson kívül" kézbesítési módot biztosít a leküldéses értesítést, ha hello alkalmazás le van zárva. Ezt az hello szabványos leküldéses értesítést.
"Csak az alkalmazáson kívül" kiválasztásakor kell már megadott hello a tanúsítványokat a hello platform, amely az alkalmazás van építve (APNS vagy GCM).

### <a name="see-also"></a>Lásd még:
* [Apple Push Notification szolgáltatás – tanúsítványok](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google Cloud Messaging – tanúsítvány](http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"az alkalmazáson belüli csak" szállítási mód
![Differentiate3][17]

"Alkalmazásbeli csak" szállítási mód leküldéses értesítéseket biztosít, hello alkalmazás futtatásakor.
Ehhez az értesítéshez nem kell toogo hello APNS keresztül és a GCM-rendszer.
Hello alkalmazásbeli kézbesítési rendszer tooreach használhatja a végfelhasználók számára.
Teljes körű hello értesítési testreszabása, és döntse el, a tevékenységet (képernyő) hello értesítés jelenik meg.

### <a name="anytime-delivery-mode"></a>"Bármikor" szállítási mód
Kiválaszthatja az "Bármikor" szállítási módban, biztosítja, hogy a végfelhasználó használni tudja, hogy hello tooreach alkalmazás fut-e.
"Bármikor" kiválasztásakor kell már megadott hello platform, amely az alkalmazás (APNS vagy GCM) épít hello tanúsítványokat. 

## <a name="schedule-a-push-campaign"></a>A leküldéses kampány ütemezése
### <a name="plan-toostart-a-campaign"></a>A kampány tooStart megtervezése
![Shedule1][18]

Hello március 21, és rendelkezik egy bejelentési toomake tervezett a hello március 22nd éjfélkor. Hello felület toodo leküldéses elé toostay nincs! Megtervezheti, hogy előzetesen hello pontos perc a rendszer értesítést küld.

* UN-ellenőrzési hello "None" jelölőnégyzetet, és válassza ki a kezdési idő 
* Válassza ki a hello dátum és hello időtartamát toostart hello leküldéses kampány.

### <a name="plan-tooend-a-campaign"></a>A kampány tooend megtervezése
![Shedule2][19]

Azt szeretné, a kampány toostop hello a 25-éig március du. 3:00, de Ön tudja róla, hogy toodo nem lehet azt.
Hello felület toopush elé toostay nincs! Megtervezheti, hogy előzetesen hello pontos perc a kampány befejeződik.

* Kattintson a hello "None" jelölőnégyzet jelölését, vagy válasszon egy befejezési időpontja
* Válassza ki a hello dátum és hello időtartamát toofinish hello leküldéses kampány.

### <a name="end-a-campaign-manually"></a>A kampány manuálisan végén
![Shedule3][20]

Alapértelmezés szerint hello "None"-jelölőnégyzeteket.
Ez azt jelenti, hogy hello kampány megkezdődik, amint hello aktiválnia eléri szakaszban, majd ha leáll a hello end eléri a szakasz.

> [!NOTE]
> A záró dátum nélküli létrehozott kampányok hello leküldéses hello eszköz helyileg tárolja, és szerepel, hogy hello következő megnyitásakor hello alkalmazás akkor is, ha hello kampány manuálisan véget ér.

## <a name="enhance-a-push-notification-with-a-text-view"></a>A szöveges nézet a leküldéses értesítés javítása
### <a name="what-is-a-text-view"></a>Mi az a szöveg nézetet?
![TextView1][21]

A szöveges nézet olyan szöveges tartalommal előugró ablak. Az előugró ablak hello végfelhasználói kattintott hello leküldéses értesítés jelenik meg.
A szöveges nézet lehetővé teszi a toopresent tartalom tooyour végfelhasználói. Ez egyben hello lehetőség toopresent egy hívás tooaction például Ugrás tooa lap az alkalmazás átirányítása tooa tároló, a weblap megnyitásakor e-mail, küldés, kezdve egy földrajzi helyét keresési stb...

### <a name="example-text-view"></a>Példa: Szöveges nézet
* A leküldéses értesítési kampány létrehozásához hello "Elérni" szakaszban, és nevezze el a kampány

![TextView2][22]

* Írása, amely megjelenik majd üdvözlőüzenetére hello értesítést.
* Válassza ki a "text" tartalomtípusa közlemény hello

![TextView3][23]

> [!NOTE]
> Amikor a szöveges nézet, azt mindig tartalmaz egy értesítési először. 

* Adja meg a hello szöveg (kiválasztását követően hello szöveg közlemény tartalmát, hello alterület jelenik meg, így toodefine hello szöveg toobe jelenik meg.)

![TextView4][24]

* Az írási üdvözlőüzenetére hello tetején megjelenő hello cím.
* Hello fő hello szöveg nézet tartalmának írása.
* Írjon hello tartalmat, amely megjelenik majd hello akciógombra kattinthat (a Akciógomb lehetővé teszi a hello alkalmazás toomake egy adott művelet, például egy hello alkalmazás tooan App Store-ból vagy bármilyen típusú adatforrások biztosíthat átirányítása lap megnyitása).
* Írási hello tartalom, amely megjelenik majd a Kilépés gombra hello (hello Kilépés gombra kattintva, hello szöveges nézet eltűnik.)
* A leküldéses értesítési kampány létrehozásához, és hello kampány lista megjelenik.

![TextView5][25]

* A leküldéses értesítési kampány toosend hello szöveg nézet tooyour felhasználók aktiválása.

![TextView6][26]

* eredménye

![TextView7][27]

* hello felhasználó megkapja hello értesítést, és kattintson rá.
* egy előugró lehetővé tevő hello felhasználói toointeract vele hello szöveges nézet jelenik meg.

## <a name="enhance-a-push-notification-with-a-web-view"></a>A webes nézet a leküldéses értesítés javítása
### <a name="what-is-a-web-view"></a>Mi az a webes nézet?
![WebView1][28]

A webes nézet egy előugró ablak webtartalmakkal együtt. Az előugró ablak akkor jelenik meg, amikor hello végfelhasználói hello leküldéses értesítési kattintott.
A webes nézet lehetővé teszi a toohave több interakciót tesz hello végfelhasználói.
Ez egyben hello lehetőség toopresent egy hívás tooaction, például az átirányítási tooApp tároló, a weblap megnyitásakor e-mail, küldés, kezdve egy földrajzi helyét keresési stb...

### <a name="example-web-view"></a>Példa: Webes nézet
* A leküldéses kampány létrehozásához hello "Elérni" szakaszban, és nevezze el a kampányt.

![WebView2][29]

* Írása, amely megjelenik majd üdvözlőüzenetére hello értesítést.
* Jelölje ki "web" hello közlemény tartalom típusa

![WebView3][30]

### <a name="about-announcement-types"></a>Kapcsolatos bejelentés típusok:
* Csak értesítés: egy egyszerű standard szintű értesítési. Ami azt jelenti, hogy ha a felhasználó kattint, további nézet nélkül jelenik meg, de csak hello művelet társított tooit történik.
* Szöveg közlemény: egy értesítés, amely kapcsolatba lép a hello felhasználói toohave egy pillantást a szöveges nézet.
* Webes hirdetmény: egy értesítés, amely kapcsolatba lép a hello felhasználói toohave egy pillantást a webes nézet.
  Válassza ki a hello "Webes hirdetmény" tartalom.

> [!NOTE]
> Amikor a webes nézet, azt mindig tartalmaz egy értesítési először.

* Adja meg a webes tartalom hello (kiválasztását követően közlemény hello webtartalom, hello alszakasz jelenik meg, így toodefine hello webes nézet kívánt tartalmat toobe jelenik meg.)

![WebView4][31]

* (Választható) üdvözlőüzenetére hello tetején megjelenő hello cím írni.
* Ide írhatja a HTML kódot.
* Kattintson a Szerkesztés módban gomb tooswitch edition hello forrás, és például megjelenését.
* Írjon hello tartalmat, amely megjelenik majd hello akciógombra kattinthat (a Akciógomb lehetővé teszi a hello alkalmazás toomake egy adott művelet, például egy hello alkalmazás tooa tároló vagy bármilyen típusú adatforrások biztosíthat átirányítása lap megnyitása).
* Írási hello tartalom, amely megjelenik majd a Kilépés gombra hello (hello Kilépés gombra kattintva, hello webes nézet eltűnik).
* eredménye

![WebView5][32]

* hello felhasználói hello értesítést kapni, és kattintson rá.
* egy előugró lehetővé tevő hello felhasználói toointeract vele hello szöveges nézet jelenik meg.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

