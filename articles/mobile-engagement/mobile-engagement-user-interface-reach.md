---
title: "Mobile Engagement felhasználói felület - a Reach aaaAzure"
description: "Megtudhatja, hogyan tooreach toohello felhasználók az alkalmazás az Azure Mobile Engagement leküldéses értesítéseket"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 40d5162ddeccec82c2c9f5b0d72b4cb10c9ddc38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreach-out-toohello-users-of-your-application-with-push-notifications"></a>Hogyan tooreach toohello felhasználók az alkalmazás a leküldéses értesítések
Ez a cikk ismerteti a hello **ELÉRNI** hello lapján **a Mobile Engagement** portálon. Hello használata **a Mobile Engagement** portál toomonitor és a mobilalkalmazások kezelése. Vegye figyelembe, hogy először toocreate hello portálon toostart egy **Azure Mobile Engagement** fiók. További információkért lásd: [Azure Mobile Engagement-fiók létrehozása](mobile-engagement-create.md).

hello éri el a felhasználói felület hello szakasza hello leküldéses kampány felügyeleti eszköz itt lehetséges létrehozása/szerkesztése/aktiválása/Befejezés/figyelő, és megtekintheti a statisztikákat a leküldéses értesítési kampányt és hello Reach API (és néhány eleme hello alacsony keresztül is elérhető szolgáltatások szint leküldéses API). Ne feledje, függetlenül attól, hogy hello API-k vagy hello felhasználói felület, akkor kell toointegrate Azure Mobile Engagement és a Reach az alkalmazásba, az egyes platformokon hello SDK használata előtt a Reach-kampányok.

> [!NOTE]
> Hello sok szakasza **a Mobile Engagement** portál felhasználói felületének tartalmazhat hello **megjelenítése SÚGÓ** gombra. Nyomja meg a gomb tooget szakasz környezetfüggő tájékozódhat.
> 
> 

## <a name="four-types-of-push-notifications"></a>Leküldéses értesítések négy típusa
1. Közlemények - toosend hirdetési üzenetek toousers, amely átirányítja őket az alkalmazás vagy a toosend belül tooanother hely lehetővé teszi őket tooa weblap, vagy az alkalmazás kívül tárolja. 
2. Szavazások - lehetővé teszi a végfelhasználók toogather adatait által kérdések kéri a felhasználót.
3. Adatleküldések - lehetővé teszi egy bináris vagy base64 adatfájlt toosend. adatoknak lévő hello információt az alkalmazásban a felhasználók aktuális élmény tooyour alkalmazás toomodify küldött. Az alkalmazás kell toobe képes tooprocess hello adatok található adatokat.

## <a name="campaign-details"></a>Kampány adatai
Szerkesztése, klónozzon, törlése, és aktiválja a kampányt, amely rendelkezik még nincs aktiválva a nevek fölé vagy tooopen kattintva őket. Már aktiválták a nevek fölé kampányok klónozhat vagy tooopen kattintva őket. Azonban a kampány nem módosítható, miután aktiválva lett.

![Reach1][18]

## <a name="reach-feedback"></a>Visszajelzés elérni
Kattintson a **statisztika** toosee hello részletei egy Reach-kampány. Hello **egyszerű** nézet visual reprezentációja arról, mi történt egy kampány aktiválása után oszlop oszlopdiagramon hello formájában. Hello **speciális** nézet tartalmazza hello leküldéses kampány részletesebb adatait. Ezek az adatok nem lesz elérhető, ha a teszt kampány azaz leküldéses elküldött tooa vizsgálati eszköz. Itt látható, hogyan kell értelmezni ezeket az adatokat:

1. **Leküldött** -azt határozza meg, hogy hello toohello eszközökre leküldött üzenetek száma. Ez a szám hello leküldéses kampány létrehozásához a megadott hello célközönség függ. Ha nem adja meg a célközönség, majd a leküldéses elküldi tooall hello regisztrált eszközökre. Minden más leküldéses szolgáltatások, például a Microsoft nem leküldéses hello értesítések közvetlenül toohello eszközök, de ehelyett küldje le őket toohello adott platform adott leküldéses értesítéseket kezelő szolgáltatása (PNS - APNS/GCM/WNS), hogy azok által biztosított hello értesítések toohello eszközök. 
2. **I** -azt határozza meg hello sikeresen kézbesítette hello PNS toohello eszköz és a nyugtázott üzenetek száma, mint a Mobile Engagement SDK által fogadott. 
   
   *A kézbesített okait száma kevesebb, mint a megnyomott száma:*
   
   1. Ha hello felhasználó eltávolította hello app hello eszközről, de hello PNS nem ismeri az hello leküldéses toohello PNS küldünk hello időpontban üdvözlőüzenetére a program eltávolítja.
   2. Ha hello eszközzel hello alkalmazás van, de a hello eszközöktől hosszabb ideig offline állapotban volt, majd hello PNS nem toodeliver hello üzenet toohello eszköz. 
   3. Ha üdvözlőüzenetére toohello eszköz kézbesítve, de hello Mobile Engagement SDK hello alkalmazásban nem ismeri fel a üdvözlőüzenetére hello tartalmát, majd esik az üzenet. Ez akkor fordulhat elő, ha hello testreszabási hello értesítés hello App SDK és az vetett hello üdvözlőüzenetére kivételt, amely azt a catch hoz létre. Ez akkor is előfordulhat, ha hello alkalmazást hello eszközön hello platform által küldött hello Mobile Engagement SDK, amely nem tud toounderstand hello újabb verziójával leküldéses üdvözlőüzenetére verzióját használja, de ez csak akkor, ha hello app frissítették hello értesítés után hello service platformról elküldése. Hello **speciális** lapon megtudhatja, hogy hány üzenetek el lettek dobva. 
   4. Az iOS-eszközökön üzenetek néha nem kézbesítve az alacsony töltöttségű telepre vonatkozó egyik hello eszköz esetén, vagy ha hello app van fel power jelentős mennyiségű, távoli értesítések feldolgozása közben. Ez az iOS-eszközök hello korlátozása.   
3. **Megjelenített** -azt határozza meg, hogy sikeresen látható toohello alkalmazás felhasználói hello értesítési központ leküldéses/out-az-app rendszerértesítőként vagy egy alkalmazásbeli értesítés belül hello hello űrlapján hello eszközön mobil üzenetek hello száma alkalmazás.  Hello **speciális** lapon megtudhatja, hány volt rendszer értesítéseket és alkalmazáson belüli értesítések. 
   
   *A megjelenő okait száma kevesebb, mint a kézbesített száma (várakozási toobe jelenik meg)*
   
   1. Hello értesítési kampány befejező dátumát volna meg, akkor lehetséges, hogy hello értesítés lett kézbesítve, de amikor hello idő léptek tooopen, és megjeleníti azt toohello alkalmazás felhasználói, ha azt már lejárt, a rendszer soha nem jelenik meg.   
   2. Ha hello értesítést az alkalmazáson belüli értesítést majd hello értesítési csak jelenik meg hello alkalmazás felhasználói hello alkalmazás megnyitása. Azokban az esetekben, ahol hello alkalmazás felhasználói nem nyitotta meg hello app hello SDK jelentést, hogy hello értesítési kézbesíteni, de még nem jelenik meg hello alkalmazás már meg van nyitva. 
   3. Ha hello értesítést az alkalmazáson belüli értesítést, és akkor is hello értesítési lesz jelentve szállított egy adott tevékenység/képernyőn megjelenő toobe konfigurálva, de amíg kézbesíteni még nem hello felhasználó megnyit egy adott képernyő hello alkalmazást. 
4. **Felhasználói kapcsolati** -azt határozza meg, hogy mely hello alkalmazás felhasználói rendelkezik kezelni és hello üzeneteket, amelyek műveletet kiváltó, illetve amelyekből kiléptek üzenetek hello száma. 
   
   * *hello alkalmazás felhasználói a következő módokon reagálhat értesítést hello a következő módszerek valamelyikével:*
     
     1. Ha hello értesítési rendszer/out-az-app értesítést vagy csak értesítést küld egy alkalmazásbeli értesítés akkor hello app felhasználó hello értesítési kattint.
     2. Ha hello értesítést az alkalmazáson belüli értesítést a szöveges vagy webes-nézet vagy szavazások majd hello alkalmazás felhasználói kattint hello hello értesítési művelet gombjára.
     3. Ha hello értesítés az alkalmazáson belüli értesítések, a webes nézet majd hello app felhasználó kattint hello webes nézetben [Android csak] egy URL-címen
   * *hello alkalmazás felhasználói a következő módokon léphet a következő módokon hello valamelyikével értesítést:*
     
     1. Közvetlenül kattintva hello hello értesítési Bezárás gombjára. 
     2. Lehúzni számítógépnél, vagy törölje a hello értesítést. 
     3. Alkalmazáson belüli értesítések szöveges vagy webes tartalom és szavazások esetén egy kétlépéses folyamat során általában megjelenített toohello alkalmazás felhasználói. Akkor jelenik meg értesítés először, és azok kattintson rá, ha azok hello későbbi a lekérdezési/szöveges/webes tartalom megtekintését. hello alkalmazás felhasználói a következő módokon léphet egy értesítést az alábbi lépéseket, és hello részletek hello speciális nézetben ez rögzíti. 
5. **Műveletet kiváltó** -azt határozza meg, hogy hello volt hello app felhasználó explicit módon műveletet kiváltó üzenetek száma. Ez az hello a legérdekesebb számát, ez alapján alkalmazást hány felhasználó által üdvözlőüzenetére hello értesítés leküldött volt interested. 

> [!NOTE]
> Az iOS és Windows platformra, ha hello felhasználó hello alkalmazást nyitott és hello kampány nem "Bármikor" kampány akkor lehetséges, hogy mindkét kívüli alkalmazás és az alkalmazáson belüli értesítések megjelenítése hello ugyanannyi időt vesz igénybe. Emiatt a magasabb, mint a kézbesített hello megjelenített számát. Ha hello felhasználói együttműködő, vagy a műveletek hello értesítés, még akkor is, majd hello felhasználói interakció/Actioned száma nagyobb, mint a kézbesített lehet. 
> 
> 

![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

