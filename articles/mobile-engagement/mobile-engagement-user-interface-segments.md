---
title: "Mobile Engagement felhasználói felület – aaaAzure szegmensek"
description: "Megtudhatja, hogyan toocreate és kezelheti a szegmenseket, az Azure Mobile Engagementet használó felhasználók tooidentify használati minták"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: bb214c45d05ebfbf243978658a7e331d4a7c6e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-segments-of-users-tooidentify-usage-patterns"></a>Hogyan toocreate és kezelheti a szegmenseket felhasználók tooidentify használati minták
Ez a cikk ismerteti a hello **SZEGMENSEK** hello lapján **a Mobile Engagement** portálon. Hello használata **a Mobile Engagement** portál toomonitor és a mobilalkalmazások kezelése.

felhasználói felület hello hello szegmensek szakasza lehetővé teszi toowork szegmentálja a felhasználók hello másképp viselkednek és elemzés, amelyet az alkalmazás hello kérheti le és hello szegmensek API keresztül is elérheti a. Szegmensek először vannak számított 24 órával azt követően hozza létre őket, és azok vannak recomputed 24 óránként hello legújabb analytics adatok alapján. Miután egy szegmens kiszámítása, azt jeleníti meg a "Nap tooday előzmények" naponta.

> [!NOTE]
> Hello sok szakasza **a Mobile Engagement** portál felhasználói felületének tartalmazhat hello **megjelenítése SÚGÓ** gombra. Nyomja meg a gomb tooget szakasz környezetfüggő tájékozódhat.
> 
> 

## <a name="create-segments"></a>Szegmensek létrehozása
A szegmens hello too60 napokat be egy adott időszakban a too10 feltételek alapján hozhat létre múltbeli hello analytics szakaszából. Például egy szegmens hello személyeket, akik bizonyos oldalait vagy keresni az alkalmazáson belül hello belül adott tartalomtípusok 10 napja alapján is létrehozhat. Ez az információ hello analytics szakaszában érhető el. Igen, toocreate egy szegmens használják, és hozzon létre egy leküldéses értesítési tootarget a felhasználók tooget részhalmazát őket toocome hátsó toohello alkalmazás. 

> [!NOTE]
> Miután egy szegmens kiszámított, ezért nem lehet szerkeszteni; azt is csak klónozáshoz (másolt) vagy megsemmisül (törölve). Egy szegmens belül hello klónozható ugyanahhoz az alkalmazáshoz (az azonos AppID hello), és azt is klónozható más alkalmazásokba (a különböző AppID). 

 ![segments1][35] 

## <a name="examples-segments"></a>Példák szegmensek
 ![segments2][36]

Szegmensek toosegment hello végfelhasználók számára az alkalmazás lehetővé teszi.
A felhasználók szegmentálásával egy fontos marketing stratégia. Az Azure Mobile Engagement lehetővé teszi a tooget régebbi adatok, és hozzon létre egyéni szegmensek. Az erőteljes eszköz lehetővé teszi az alkalmazás a felhasználók tapasztalatairól toolearn. Egyszerűen elemezheti a szegmenseket, és használja a szegmenseket leküldéses célként.
Egy gyakori használati eset, amelyet egy értesítési tooencourage leküldéses toosend a végfelhasználók toorate hello áruházban az alkalmazás. Ahelyett, hogy egy értesítési tooall küld a végfelhasználók számára, létrehozhat kellene megadnia csak azok a felhasználók, amelyek az alkalmazás minden nap az előző hónap hello használt, és rendelkezésére állt-e a kiváló felhasználói felülettel szegmenssel. Kevesebb, jól célzott leküldéses értesítéseket küldeni, a jobb Megtérülési nyílik meg.

 ![segments3][37]

### <a name="segments-you-can-create-based-on-hello-major-azure-mobile-engagement-elements"></a>Létrehozhat alapuló szegmens hello fő Azure Mobile Engagement elemet:
* Esemény: hozzon létre egy szegmenst a célokat egy adott esemény hello alkalmazás, amely hetente legfeljebb kétszer került sor. 
* Munkamenet: hozzon létre egy részének a több mint 5 alkalommal múlt héten hello alkalmazást használó felhasználókat.
* Tevékenység: hozzon létre egy részének a használó felhasználókat egy oldal, vagy a tartalom több vagy kevesebb mint 10 ideje elmúlt hónapban.
* Feladat: a szegmens, amely naponta legfeljebb kétszer befejeződött egy feladat létrehozása.
* Összeomlási: hozzon létre egy szegmens az összes olyan hello felhasználó 10 szintnél mélyebben múlt héten crash volt. (Ebben a szegmensben egy Bocsánatkérő vagy akár egy ráta sikerült leküldéses!)
* Hiba: a szegmens, amely több mint 100 alkalommal 3 napnál korábban hibába hello felhasználók létrehozása.
* Alkalmazásadat: hozzon létre egy szegmenst, amelynek célja egy egyéni App-Info 25 napja hello során történt.
  
  ![segments4][38]

toouse szegmens optimálisan, még kell meg egy testreszabott hello SDK integrációja "Alkalmazásadatok" címkék címkézési tervvel az alkalmazásban.
Folytassa a kezdőlap toohello hello illesztőfelület, ki hello alkalmazást, majd kattintson a hello "Szegmensek" szakasz.

1. Válassza ki a hello "Szegmensek" szakaszt.
2. Kattintson az "Új szegmens" hello toocreate új szegmens gombra kattint.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Valós élettartama. példa: Hozzon létre egy egyszerű szegmens "Munkamenet" adatok alapján
Hozzon létre egy szegmens az összes hello végfelhasználók számára, amely használták az alkalmazást legalább 50 alkalommal hello múlt héten. Ott csak az alkalmazás munkamenetenként legalább 30 másodperces töltötték hello végfelhasználók található. Ez az alkalmazás egy pozitív élményt rendelkező összes hello végfelhasználók jelennek meg. Ezután létre hello szegmens használt toopush egy értesítési toothese végfelhasználók tooask lehet őket toorate az alkalmazást a hello tárolja.

 ![segments5][39]

1. Nevezze el a szegmensek a rendelés toofind legyen gyorsan hello "Szegmens" lista.
2. Kattintson a hello "Létrehozás" gombra.
   
   ![segments6][40]

Jelölje ki a munkamenetet.

 ![segments7][41]

1. Válassza az "Elmúlt hét" hello időszak.
2. Kattintson a Tovább gombra.
   
   ![segments8][42]
3. SELECT hello között hello lista fontos operátor: =; ≥, ≤.
4. Adja meg a hello kívánt száma.
5. Válassza ki a kívánt előfordulási hello. 
6. Kattintson a Tovább gombra.
   Ebben a példában van beállítva, így az adott over hello múlt héten egyezés felhasználók, amelyek legalább 50 munkamenetek történtek.
   
   ![segments9][43]

Hello munkamenet szegmentálását kiválaszthatja feltételként munkamenetenként hello hossza.

1. Válassza ki a hello operátor hello listából.
2. Adja meg a hello hossza munkamenetenként.
3. Kattintson a Next (Tovább) gombra.
   Ebben a példában a felirat látható, hogy folyamatosan hello munkamenetek, amelyek rendelkeznek lett szegmentált hello előfordulási szakasz, jelölje ki a csak hello felhasználókat, akik rendelkeznek a munkamenetenként több mint 30 másodperc.
   
   ![segments10][44]

Nevet a feltételnek rendelés tooretrieve legyen hello végezze el a tölcsér, és kattintson a Befejezés gombra.

 ![segments11][45]

Miután végzett a beállítás mentése a feltételnek, a hello szegmens tölcsér jelenik meg.
A szegmens analytics adatokon alapul, mert a szegmensek naponta egyszer kiszámítása a történik.
Ebben a példában 47,7 hello teljes végfelhasználók % hello feltételnek megfelel. Hello felhasználók rendelkezésére állt-e a megfelelő környezet kell őket, és fog kell egy magasabb értékelése, ha értesítést leküldéses őket valószínűleg tooprovide kéri a felhasználót toorate hello alkalmazás hello áruházban.

## <a name="see-also"></a>Lásd még:
* [Alapfogalmak][Link 6]
* [Hibaelhárítási útmutató szolgáltatás][Link 24]

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

