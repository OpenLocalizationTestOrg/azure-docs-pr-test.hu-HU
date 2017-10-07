---
title: "aaaAzure Mobile Engagement felhasználói felület - tartalom eléréséhez"
description: "Ismerje meg, hogyan toomanage hello egyedi tartalom hello különböző típusú leküldéses értesítés kampányokra, az Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: add64f06-43c9-475c-8722-51cd00bb844b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: de389eb4368d986ef00135036c26e26a2464663e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-hello-unique-content-of-hello-different-types-of-push-notification-campaigns"></a>Hogyan toomanage hello hello különböző típusú leküldéses értesítéses kampányokkal egyedi tartalom
Hello tartalmi szakasz tartalmának egy új reach kampány toomodify hello a közlemények, szavazások, Adatleküldések és Csempék (csak Windows Phone) is használhatja. hello tartalom leküldéses kampányokra érték kampány adott toohello típusát. 

### <a name="content-types"></a>Tartalom típusa:
* Bejelentések
* Szavazások
* Adatleküldések
* Csempék (csak Windows Phone)

## <a name="content-of-announcements"></a>Közlemények tartalma
 ![Reach-Content1][30] 

### <a name="choose-hello-type-of-your-announcement"></a>Válassza ki a hello hirdetmény típusának kiválasztása:
* Csak értesítés: egy egyszerű standard szintű értesítési. Ami azt jelenti, hogy ha a felhasználó kattint, további nézet nélkül jelenik meg, de csak hello művelet társított tooit történik.
* Szöveg közlemény: egy értesítés, amely kapcsolatba lép a hello felhasználói toohave egy pillantást a szöveges nézet.
* Webes hirdetmény: egy értesítés, amely kapcsolatba lép a hello felhasználói toohave egy pillantást a webes nézet.

### <a name="see-also"></a>Lásd még:
* [A reach - hogyan Tos - közlemények][Link 3] 

### <a name="about-web-view-announcements"></a>Vonatkozó webes nézetre mutató hirdetmények:
Hello mintát "{deviceid}" hello HTML vagy JavaScript-kódban itt előfordulását automatikusan helyébe hello közlemény megjelenítő hello eszköz hello azonosítója. Ez az, hogy egy egyszerűen tooretrieve Azure Mobile Engagement-eszközazonosítók a külső webes az üzemeltetett szolgáltatás.
Ha azt szeretné, hogy toocreate teljes képernyőn webes nézet (hello alapértelmezett akciógombok és kilépési gombok nélkül) használhatja a következő funkciók a webes hirdetmény JavaScript-kódjában hello: 

* hello hirdetményművelet végrehajtása: ReachContent.actionContent()
* Kilépés a hello közlemény: ReachContent.exitContent()

### <a name="choose-your-action"></a>Válassza ki a műveletet:
### <a name="about-action-urls"></a>Kapcsolatos művelet URL-címek:
A megcélzott eszköz operációs rendszere által értelmezhető valamennyi URL-cím használható műveleti URL-címként.
Dedikált URL-címet, előfordulhat, hogy az alkalmazás támogatási (pl. toomake felhasználók tooa adott képernyőre irányítják) is használható műveleti URL-címként.
{Deviceid} hello minta összes előfordulásának automatikusan helyébe hello művelet végrehajtása hello eszköz hello azonosítója. Ez lehet használt tooeasily olvashatók be az Azure Mobile Engagement eszközazonosítók a üzemeltetett külső webszolgáltatáson keresztül.

* **Android + iOS műveletek**
  * Weblap megnyitása
  * http://\[web-site-tartomány\] 
  * Példa: http://www.azure.com
  * E-mail küldése
  * mailto:\[-címzett\]? tulajdonos =\[tulajdonos\]& body =\[üzenet\] 
  * Example:mailto:foo@example.com? tulajdonos = a hónap % 20from % 20Azure % 20Mobile % 20Engagement! & body = jó % 20stuff!
  * SMS küldése
  * SMS:\[-telefonszám\] 
  * Példa: sms:2125551212
  * Telefonszám tárcsázása
  * Tel:\[-telefonszám\] 
  * Példa: tel:2125551212
* **Android csak műveletek**
  * Play Store hello alkalmazás letöltése
  * Market://details?ID=\[alkalmazáscsomag\] 
  * Példa: market://details?id=com.microsoft.office.word
  * Geolokációs keresés indítása
  * GEO:0, 0? q =\[keresési lekérdezés\] 
  * Példa: geo:0, 0? q = starbucks, Párizsi
* **csak iOS-műveletek**
  * Alkalmazás-áruház hello alkalmazás letöltése
  * http://iTunes.apple.com/ [Ország] /app/ [alkalmazás neve] /id [alkalmazás azonosítója]? mt = 8 
  * Példa: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
  * Windows-műveletek
  * Weblap megnyitása
  * http://\[web-site-tartomány\] 
  * Példa: http://www.azure.com
  * E-mail küldése
  * mailto:\[-címzett\]? tulajdonos =\[tulajdonos\]& body =\[üzenet\] 
  * Example:mailto:foo@example.com? tulajdonos = a hónap % 20from % 20Azure % 20Mobile % 20Engagement! & body = jó % 20stuff!
  * SMS küldése (az Áruházból letölthető Skype alkalmazás szükséges hozzá)
  * SMS:\[-telefonszám\] 
  * Példa: sms:2125551212
  * Telefonszám tárcsázása (az Áruházból letölthető Skype alkalmazás szükséges hozzá)
  * Tel:\[-telefonszám\] 
  * Példa: tel:2125551212
  * Play Store hello alkalmazás letöltése
  * MS-windows-tároló: PDP? PFN =\[app Csomagazonosító\] 
  * Példa: ms-windows-tároló: PDP? PFN 4d91298a-07cb-40fb-aecc-4cb5615d53c1 =
  * Keresés a Bing Térképek szolgáltatásban
  * bingmaps:? q =\[keresési lekérdezés\] 
  * Példa: bingmaps:? q = starbucks, Párizsi
  * Egyéni séma használata
  * \[egyéni séma\]://\[egyéni séma paraméterei\] 
  * Példa: myCustomProtocol://myCustomParams
  * Csomagadatok (tárolóalkalmazás olvassa el a kiterjesztés kötelező) használata
  * \[mappa\]\[adatok\].\[ bővítmény\] 
  * Example:myfolderdata.txt

### <a name="build-a-tracking-url"></a>A követési URL-cím összeállítása:
* Lásd: hello "Beállítások" szakasza hello <UI Documentation> az utasítás felépítésével egy követési URL-címet, amely lehetővé teszi felhasználók toodownload a más alkalmazások közül.

### <a name="define-hello-texts-of-your-announcement"></a>Hirdetmény szövegeinek hello meghatározása
Töltse ki a hello cím, a tartalom és a hirdetmény szövegeinek kiválasztása gombra. Olyan célközönségnek juttathatja el, hogy a felhasználók válaszolt toothis kampány hello reach visszajelzések alapján jövőbeli kampány célba. Célközönség-e a kampány lett csak leküldött, megválaszolt, műveletet kiváltó, illetve amelyekből kiléptek hello visszajelzését alapulhatnak.

### <a name="see-also"></a>Lásd még:
* [Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]

## <a name="content-of-polls"></a>Szavazások tartalma
![Reach-Content2][31] 

Töltse ki hello címét, leírását és hirdetmény szövegeinek kiválasztása gombra. Adja hozzá a kérdések és hello válaszok tooyour kérdések lehetőségeit.
Olyan célközönségnek juttathatja el, hogy a felhasználók válaszolt toothis kampány hello reach visszajelzések alapján jövőbeli kampány célba. Célközönség-e a kampány lett csak leküldött, megválaszolt, műveletet kiváltó, illetve amelyekből kiléptek is alapulhat. Célközönség kiválasztását is alapulhat a lekérdezési válasz visszajelzést, ahol hello kérdés és válasz választott feltételként használják.

### <a name="see-also"></a>Lásd még:
* [Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]

## <a name="content-of-data-pushes"></a>Adatleküldések tartalma
![Reach-Content3][32] 

### <a name="choose-hello-type-of-your-data"></a>Az adatok hello típusának kiválasztása:
* Szöveg
* Bináris adatok
* A Base64 adatok

### <a name="define-hello-content-of-your-data"></a>Hello az adatok tartalmának meghatározása
* Ha toopush szöveges adatok választotta, másolja be hello szöveg hello "tartalom" mezőbe.
* Ha a kiválasztott toopush bináris vagy base64 adatok hello "a fájl feltöltése" gomb tooupload a fájl használatára.
* Olyan célközönségnek juttathatja el, hogy a felhasználók válaszolt toothis kampány hello reach visszajelzések alapján jövőbeli kampány célba. Célközönség-e a kampány lett csak leküldött, megválaszolt, műveletet kiváltó, illetve amelyekből kiléptek is alapulhat.

### <a name="see-also"></a>Lásd még:
* [Felhasználói felület - a Reach - dokumentáció új leküldéses feltétel][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Tartalom csempék (csak Windows Phone)
![Reach-Content4][33]

### <a name="define-hello-content-of-your-tile"></a>Hello a csempe tartalmának meghatározása
hello csempe payload hello szöveg toobe hello csempe az alkalmazás a Windows Phone-eszközökön jelenik meg.
Egy mozaik leküldéses a Windows Phone natív leküldéses hello a Microsoft leküldéses értesítési szolgáltatásának (MPNS) verziója telepítve. hello csempetípus leküldéses hello csak leküldéses típus, amely nem rendelkezik a válasz, és így a jövőbeli kampányok hello célközönség nem építhető hello eredmények a csempe leküldéses kampány. 

### <a name="see-also"></a>Lásd még:
* [API - a Reach API - dokumentáció natív leküldéssel][Link 4]

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

