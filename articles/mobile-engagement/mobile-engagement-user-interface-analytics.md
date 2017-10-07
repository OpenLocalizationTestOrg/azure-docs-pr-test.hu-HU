---
title: "aaaAzure Mobile Engagement felhasználói felület - elemzés"
description: "Megtudhatja, hogyan segítségével az Azure Mobile Engagement az alkalmazás előzményadatainak tooanalyze"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6b2533ac-b8ec-4e35-872c-d563895bdc0c
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4a9df11226fed6710cfb1337ae84ece7596d482f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooanalyze-historical-data-about-your-application"></a>Hogyan tooanalyze az alkalmazás előzményadatainak
Ez a cikk ismerteti a hello **ANALYTICS** hello lapján **a Mobile Engagement** portálon. Hello használata **a Mobile Engagement** portál toomonitor és a mobilalkalmazások kezelése. Vegye figyelembe, hogy először toocreate hello portálon toostart egy **Azure Mobile Engagement** fiók.

hello hello felhasználói felület Analytics szakasza összesített tájékoztatást ad azokról az alkalmazás 24 óránként frissül történelmi adatokon alapulnak. a vonal vagy sáv/tortadiagramok, hálózatok és a maps álló különböző irányítópultokon hello információk jelennek meg. hello adatok is .csv fájlok letölthetők. Ezt az információt a legtöbb valós idejű hello figyelő szakasza hello felhasználói felület érhető el, és azt is elérhetők hello Analytics API.

> [!NOTE]
> Hello sok szakasza **a Mobile Engagement** portál felhasználói felületének tartalmazhat hello **megjelenítése SÚGÓ** gombra. Nyomja meg a gomb tooget szakasz környezetfüggő tájékozódhat.

## <a name="standard-and-custom-analytics"></a>Normál és egyéni elemzés
Az Azure Mobile Engagement biztosít alapszintű, standard TénylegesKöltség, amint hello SDK az alkalmazás integrálja az alkalmazások elemzési adatait. Az Azure Mobile Engagement bemutatja a hello képességét toogather további egyéni elemzés, amelyet a végfelhasználók viselkedése. Ehhez hozzon létre egy egyéni "címkék (app-info)", alapján létrehozott címkézési terv **beállítások** , hogy az Azure Mobile Engagement a további adatokat gyűjthet meg.

## <a name="analytics"></a>Elemzés
* Irányítópult: Az új és beillesztené felhasználók és a trendek általános információkat jeleníti meg.
* Felhasználók: Felhasználók azonosítják az eszközazonosító: minden eszköznél egyedi (egy új felhasználó tulajdonképpen egy új eszközt). A felhasználó akkor tekinthető új egy adott időintervallumban, ha az első munkamenete az adott időszakban történt. A felhasználó akkor tekinthető megtartottnak, ha legalább egy munkamenetet elvégzett során hello legutóbbi 7 nap. Aktív felhasználók a felhasználókat, hogy legalább egy munkamenetet egy adott időszakban. Rendezheti, havonta, hetente, nap, napi vagy óránkénti időszakok. Az összes hello diagramok hasonló, de lehetővé teszi, toofilter különböző szolgáltatások, például az alkalmazáshoz, majd toosort hello verziója által egy ideig. hello hello SDK integrálásával gyűjtött szabványos információkat hello következőket tartalmazza: az aktív felhasználók, új felhasználói, a munkamenetek számának, minden munkamenet, hello ország, helyi változók, hely, nyelvi szolgáltatónként, eszközök, belső vezérlőprogramok, kapcsolatos műszaki adatok hossza hálózati (Wi-Fi) hello app és az SDK-t és az ügyfelek által használt verzióját. Ezek az információk is megtekinthetők valós idejű hello figyelő szakaszából.

> [!NOTE]
> hello időszakra vonatkozó alapul hello felhasználói eszközbeállítások, hello dátumot, a felhasználó, akinek telefonhoz hello dátum helytelenül állította be a sikerült megjelennek hello megfelelő időszakra vonatkozóan.

* Megőrzési: A felhasználó akkor tekinthető megtartottnak egy adott időintervallumban, ha az első munkamenete az adott időszakban történt. Megtartott felhasználók (és az új felhasználók) számlálási időintervalluma toohours, nap, hét és hónap hello időközök módosíthatja. hello felhasználói megőrzési analytics cohorts épül. Egy kohorszok egy hello összes hello új felhasználót érzékelt egy adott időszakra (azaz hello felhasználók csoportja végrehajtása az első munkamenete az ebben az időszakban). Az 1 napos, 2 napos, 4 napos, 7 napos és 1 hónapos cohorts használjuk. A megadott kohorszok, minden 1 napos, 2 napos, 4 napos, 7 napos és 1 hónapos Azure Mobile Engagement kiszámítja hello a felhasználók egy csoportja összes tartozó toohello kohorszok és a még mindig aktív (vagyis hello beállítása a felhasználók, akik hello időszak során legalább egy munkamenetet végrehajtani). A felhasználók egy kohorszok verzió nevezzük. (Az azure Mobile Engagement láthatja, hogy hány felhasználó használják az alkalmazást, de az adott tároló csak hello platform segítségével megállapíthatja, hogy hány felhasználó eltávolítva az alkalmazás – például GooglePlay, iTunes, a Windows áruház, stb.).
* Munkamenetek: Hello alkalmazás a felhasználó használja. Munkamenetek jönnek létre a felhasználó által végrehajtott műveletek sorozata hello (tevékenységet egy képernyőt hello alkalmazás általában toohello használatát, de ez eltérőek lehetnek attól függően, hogy hello módon hello SDK hello alkalmazás lett integrálva). A felhasználó egyszerre csak hajthat végre egy tevékenység: a munkamenet akkor kezdődik, amint hello felhasználó elindítja az első tevékenységet, és befejezésével ér véget, hogy az utolsó tevékenység. Ha a felhasználó csak néhány másodpercig marad, a tevékenység végrehajtása nélkül, majd a tevékenységek sorrendjét két különböző munkamenetek egy van osztva.
* Tevékenységek: a következő képernyőkön minden képernyő az alkalmazásban hello nevét és amikor a felhasználók hello hosszát töltött. Tevékenységek egy egyéni elemzési beállítást, amely a saját alkalmazás beállítása toohello "alkalmazásadatok" címkék felel meg a következők:
* Felhasználói elérési út: Mutatja, hogy a felhasználók hogyan navigálnak az alkalmazás tevékenységei (képernyői). Áthelyezheti részletességi hello csúszkát tooadjust hello szint. Kék csomópontok jelölik a tevékenységeket az alkalmazásban. A méretük azzal arányos toohello felhasználók mennyi időt töltöttek azt. A fehér csomópontok a munkamenetek kezdetét és végét jelölik. Piros csomópontok jelölik a összeomlik. A nyilak az alkalmazás tevékenységei (vagy a tevékenységek és az összeomlások) közötti váltásokat jelölik. Kattintson a kívánt csomópontra vagy egy hivatkozás toodisplay egy elemleírás, amely további információkkal szolgál az adatokról: hello képernyőn töltött időről egy adott, hello váltások számáról, valamint hello tevékenység toohello cél forrástevékenység átmenetek hello százalékát. (A---60 %---> B azt jelenti, hogy a felhasználók tevékenység egy is megnőnek hello idő 60 % tooactivity B.) Hello graph átrendezéséhez tooclarify kívánt; minden alkalommal, amikor módosítja a rendszer menti a pozícióját. Megjelenítheti vagy hello összeomlások toolighten hello graph elrejtése.
* Események: A hello alkalmazás a felhasználó által végrehajtott műveleteket adott. az események hello terjesztési események munkamenetenként felhasználónként hello száma jelenik meg. Egy esemény valamilyen azonnali műveletet, például egy gombra való kattintást vagy hello egy értesítés fogadását a jelöli. (hello az esemény pontos jelentése függ hogyan hello SDK hello alkalmazás lett integrálva.) Egy esemény egy munkamenet vagy feladat során fordulhatnak elő, vagy önállóan.
* Feladatok: Hasonló tooevents, kivéve azokat hello művelet hello hosszát összpontosítanak. Például feladatok volt meg, hogy mennyi ideig tart a tartalom tooload vagy hívás tooweb szolgáltatás technikai információkat. Sikerült is megjelenítése, hogy mennyi ideig tart a felhasználói toofill űrlapot, hozzon létre egy fiókot, vagy a vásárlás. Egy feladat hello időtartamának jelöli, például egy letöltési művelet vagy hello hello időtartama valahányszor egy szalagcím képernyőn jelenik meg hello. (hello jelentése függ hogyan hello SDK hello alkalmazás lett integrálva.) Feladatok általában olyan kapcsolódnak háttér feladatok végrehajtására kerül sor hello kereteit a munkamenetek (azaz felhasználói tevékenység nélkül).
* Technicals: Technikai információkat hello eszközök hello felhasználók az alkalmazás, amely nyomon követheti, például a hello hello a felhasználói eszközök területi, vivőjel, hálózati, eszköz, belső vezérlőprogram és képernyő méretétől és hello az alkalmazás és a használt SDK-verzió hello verzióját az alkalmazásban.
* Hibák: Hibákkal kapcsolatos információkért műszaki hello alkalmazáson belüli hello alkalmazás toocrash nem váltják ki. Hiba közvetlen problémát, például hálózati meghibásodást vagy rendellenes kezelést jelent. (hello az esemény pontos jelentése függ hogyan hello SDK hello alkalmazás lett integrálva.) Hiba történhet munkamenet vagy feladat közben, vagy önállóan.
* Összeomlik: Az alkalmazás toocrash okozó hibák információt. Egy összeomlást (crash) nem várt állapotot adott hello alkalmazás nem képes tovább ellátni a tőle elvárt funkciókat, és le kell állítani. Egy összeomlást (crash) általában hello alkalmazás tooa hibája miatt.

![Analytics2][11]

## <a name="accessing-hello-retention-overview"></a>Való hozzáférés hello megőrzési áttekintése
![Analytics3][12]

hello megőrzési áttekintése hello közel be több kártyával, minden egyes ábrázoló hello – áttekintés egy bizonyos megőrzési ideig oszlanak meg. hello példa hello 2 napos megőrzési időtartam látható. hello más kártyák megjelenítése hello 4 napos és 7 napos megőrzési időtartamát.

## <a name="understanding-hello-retention-overview-cards"></a>Hello megőrzési áttekintése kártyák ismertetése
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Minden egyes kártya 3 fő részből áll:
1. 1: hello kohorszok és hello időszak
2. 2 – 4: hello hello adatmegőrzési aktuális időszak
3. 5: hello előzmények értékgörbe

### <a name="here-is-detailed-information-about-each-element"></a>Minden elem részletes információkat a következő:
1. Kohorszok és időszak: A főcím kohorszok hello típusú biztosít. Itt "2 napos időszak" azt jelenti, hogy követően áttekintjük felhasználók hello viselkedését 2 nap alatt érkező egy meghatározott időtartamra vonatkozóan 2 nap, illetve hogy felhasználók csatlakozik a következő 2 nap-blokkok hello. hello példában úgy ítéli meg, a felhasználók közötti hello hello tevékenység 21 és 22. november.
2. Így az hello megőrzési arányának hello 21 és november 22 keresztül érkező 19 és 20 november hello felhasználók. Itt volt 1 aktív felhasználói között hello 21 és 22nd, keresztül hello 3, melyeket az új felhasználók közötti hello 19th és 20.
3. A vizuális kijelző által biztosított hello fent megegyező adatok grafikusan. (harmadik hello hello 33 % száma az kör hello.) hello szín nyújt további információt: zöld állapot azt jelzi, hogy ez a szám hello előző számítási az egyre. Sárga azt jelenti, hogy a stabil, és a piros azt jelenti, hogy csökkentése.
4. Ez azt jelzi, hogy hello kiszámításához használt hello értékek.
5. Ez a hello megtartási értékek hello előzményeit értékgörbe. Toosee hello értékek lehetővé teszi a hello toohave túli hogyan továbbfejlődtek széleskörű nézetét.

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
