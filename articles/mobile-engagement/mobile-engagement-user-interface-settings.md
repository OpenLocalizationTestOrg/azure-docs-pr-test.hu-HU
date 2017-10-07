---
title: "aaaAzure Mobile Engagement felhasználói felület – beállítások"
description: "Ismerje meg, hogyan toomanage hello segítségével az Azure Mobile Engagement az alkalmazás a globális beállítások"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 858f4cb4-14de-4bb5-826f-28cadbfc928b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 02d4a36c591fc5e097410b7e931d1c9ce81d68d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-hello-global-settings-of-your-application"></a>Hogyan toomanage hello az alkalmazás globális beállítások
Hello **beállítások** menü lehetőségeit egy alkalmazás vary, attól függően, hogy hello platform hello alkalmazás és a hello engedélyek hello alkalmazás rendelkezik. A beállítások hello következőket tartalmazzák: részletek, projektek, natív leküldés, leküldési sebességét, Tag (app-info) és a kereskedelmi nyomás. hello Tag (app-info) menüpont hello beállítások szakasz kezelheti az alkalmazás (hello SDK használatával) vagy a kiszolgáló (hello eszköz API használatával). 

> [!NOTE]
> Hello sok szakasza **a Mobile Engagement** portál felhasználói felületének tartalmazhat hello **megjelenítése SÚGÓ** gombra. Nyomja meg a gomb tooget szakasz környezetfüggő tájékozódhat.
> 
> 

## <a name="details"></a>Részletek
Lehetővé teszi a toochange hello nevét és leírását, az alkalmazás, az alkalmazás és a szerepköri jogosultságok megtekintése hello tulajdonosa. 

Elemzés a konfiguráció lehetővé teszi a tooview, vagy módosítsa hello nap, hét indítsa el a és hello nap és a megőrzési időt.

  ![settings1][46]

## <a name="projects"></a>Projektek
Lehetővé teszi a tooselect összes projektet, melyekben a kívánt az alkalmazás tooappear. 

A projekt és nézet hello név, a leírás, a tulajdonos is kereshet és a szerepkörengedélyek a projekt az alkalmazás része.

További információkért lásd: [felhasználói felületének dokumentációja – kezdőlap][Link 13]

  ![settings3][48]

## <a name="native-push"></a>Natív leküldéssel
Lehetővé teszi egy új tanúsítványt vagy törlése és a meglévő tanúsítvány használata a natív leküldéses tooregister. Natív leküldéses lehetővé teszi, hogy az Azure Mobile Engagement toopush tooyour alkalmazásból bármikor, még akkor is, ha az nem futna. 

Miután megadta a hitelesítő adatokat vagy tanúsítványokat legalább egy natív leküldés szolgáltatáshoz, kiválaszthatja a "Minden alkalommal" hello LEKÜLDÉSES API elérését a kampányok, valamint használata hello "bejelentő" paraméter létrehozásakor.

### <a name="apple-push-notification-service-apns"></a>Apple Push Notification szolgáltatás (APNS)
tooenable natív leküldés hello Apple Push Notification szolgáltatás használata akkor tooregister a tanúsítványt. Szüksége lesz toospecify hello típusának megfelelő tanúsítványtároló fejlesztési (fejlesztés) vagy a termelési (termék). Majd kell feltölti a tanúsítvány és hello jelszavát.

További információkért lásd: [SDK-dokumentáció - iOS - hogyan tooPrepare az alkalmazás az Apple leküldéses értesítések küldése][Link 5]

![settings4][49]

### <a name="windows-push-notification-service-wpns"></a>A Windows leküldéses értesítéseket kezelő szolgáltatása (WPNS)
Natív leküldéses értesítéseket kezelő szolgáltatással Windows tooenable, meg kell adnia az alkalmazás hitelesítő adatait. Szüksége lesz a csomag biztonsági azonosítóját (SID) és a titkos kulcsot.

![settings5][50]

### <a name="google-cloud-messaging-for-android-gcm"></a>A Google Cloud Messaging (GCM) Android rendszerhez
Natív leküldés GCM szolgáltatással, Google toofollow hello utasítást kell tooenable. Ezután be kell illesztenie egy egyszerű kiszolgálói API-kulcsot, IP-korlátozások nélkül konfigurált. Integráció a hello SDK igényel Android v1.12.0 +.

További információkért lásd: 

* [SDK dokumentációjának Android hogyan tooIntegrate GCM][Link 5]
* [Google fejlesztői GCM útmutató](http://developer.android.com/guide/google/gcm/gs.html)

### <a name="amazon-device-messaging-for-android-adm"></a>Amazon eszköz Messaging (ADM) Android rendszerhez
tooenable natív leküldés ADM használ, meg kell adnia az Amazon <OAuth credentials> egy ügyfél-azonosító és a titkos Ügyfélkulcs (az SDK-val való integráció szükséges Android v2.1.0 +).

További információkért lásd: 

* [SDK dokumentációjának Android hogyan tooIntegrate ADM][Link 5]
* [Amazon fejlesztői ADM dokumentáció](https://developer.amazon.com/sdk/adm/credentials.html#Getting)

![settings6][51]

## <a name="push-speed"></a>Leküldési sebesség
Az alkalmazás hello aktuális leküldési sebességét jeleníti meg, és lehetővé teszi az alkalmazás toodefine hello leküldési sebességét.

  ![settings7][52]

## <a name="tag-app-info"></a>Tag (app-info)
![settings11][56]

## <a name="commercial-pressure"></a>A kereskedelmi nyomás
![settings12][57]

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

