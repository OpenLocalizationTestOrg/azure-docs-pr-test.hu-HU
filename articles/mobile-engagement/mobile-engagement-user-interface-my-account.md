---
title: "aaaAzure Mobile Engagement felhasználói felület – saját fiók"
description: "Megtudhatja, hogyan toomanage az Azure Mobile Engagement segítségével fiók profil és a vizsgálati eszközök"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 22832678-3959-4b8c-9fb2-f2ff5974e5d1
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1d85f0e87c43605f59f6536ae42a7fb6a99ee36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-your-account-profile-and-test-devices"></a>Hogyan toomanage a fiók profil és a vizsgálati eszközök
Ez a cikk ismerteti a hello **Home** hello oldalán **a Mobile Engagement** portálon. Hello használata **a Mobile Engagement** portál toomonitor és a mobilalkalmazások kezelése. 

tooget toohello **fiókomat** fiókja a hello hello lap tetején kattintson.

Saját fiók szakasza a felhasználói felület, ahol megtekintheti és a fiókjához, beleértve a profilbeállításokat hello-beállítások módosításához és teszteléséhez eszközazonosítókat hello hello. Ezek a beállítások hello Device API-val keresztül is elérhető elemeket tartalmazhat.

![MyAccount1][7]  

## <a name="profile"></a>Profil:
Megtekintheti vagy módosíthatja az alább látható fiók beállításait. Is adhat meg egy másik felhasználó engedély toouse az alkalmazást a saját e-mail-cím a hello alapján [Home](mobile-engagement-user-interface-home.md).

![MyAccount2][8]  

## <a name="devices"></a>Eszközök:
Megtekintheti, hozzáadhat és eltávolíthat eszköz eseményazonosítók hello vizsgálati eszközök használható tootest tesztelése a **elérni** vagy **leküldéses** kampányok. Környezetfüggő utasításokat hogyan toofind hello Eszközazonosítót az eszközök az egyes platformokon (iOS, Android, Windows Phone, stb.) Ha kattintson az "Új eszköz" jelennek meg. 

![MyAccount3][9]  

toouse leküldéses API vagy a Device API-val tooknow a felhasználók az eszköz egyedi azonosítója (hello deviceid paraméter) van szüksége. Nincsenek számos módon tooretrieve azt:

1. A háttérrendszerből szolgáltatással hello "Get" hello Device API-val tooget hello teljes listájának eszközazonosítók.
2. Az alkalmazásból hello SDK tooget használhatja azt. (Az Android, hívja meg az ügynök osztály hello hello getDeviceID() függvényt, és IOS rendszerű eszközökön, olvassa el a hello deviceid tulajdonsága hello ügynök osztály.)
3. A Reach-közlemények Ha hello műveleti URL-cím hello közlemény társított tartalmaz hello {deviceid} mintát, azt automatikusan helyébe hello műveletet kiváltó hello eszköz hello azonosítója.
   http://<example>.com/registeruser? deviceid = {deviceid} & otherparam = myparamdata cseréli: http://<example>.com/registeruser? deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata 
4. A Reach-webalkalmazás közlemények Ha hello hello bejelentés HTML-kódot tartalmaz hello {deviceid} mintát, azt automatikusan helyébe hello webes hirdetmény megjelenítő hello eszköz hello azonosítója.
   Ez az eszközazonosító: {deviceid} cseréli: Ez az eszközazonosító: XXXXXXXXXXXXXXXX
5. Indítsa el az alkalmazást az eszközön, és végezze el az alkalmazást, amely címkézett esemény.
   A "Részletek – az alkalmazás - figyelő - események - UI" megkeresi a hello esemény hello listában hajtotta végre.
   Kattintson a figyelő hello toothis eseményre.
   Keresse meg az Eszközazonosítót hello eszközök végre ezt az eseményt hello listájában.
   Ezután másolja le az eszköz Azonosítót, és regisztrálja a hello "UI - fiókom - eszközök – új eszköz - kiválasztása eszközplatformhoz".
   >(Vegye figyelembe, hogy ha idfa-JÁT le van tiltva az iOS, hello Eszközazonosító idővel megváltozhat hello Ha távolítsa el, majd telepítse újra az alkalmazást.)

## <a name="troubleshooting-guide"></a>Hibaelhárítási útmutatója
* [Hibaelhárítási útmutató - szolgáltatás][Link 24]

## <a name="see-also"></a>Lásd még:
* [Felhasználói felületének dokumentációja – kezdőlap][Link 13]

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




