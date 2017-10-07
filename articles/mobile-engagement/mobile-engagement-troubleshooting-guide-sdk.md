---
title: "aaaAzure Mobile Engagement hibaelhárítási útmutatója - SDK"
description: "Az Azure Mobile Engagement SDK-integráció problémák elhárítása"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: de265cf1-2f88-43ef-8616-156ada5be7b6
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1c082b81d898f4bdb47b8efe6cfbacfd83fe9279
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Az SDK-integráció problémák hibaelhárítási útmutató
Az alábbiakban hello lehetséges problémák merülhetnek fel az, hogy hogyan integrálja az Azure Mobile Engagement az alkalmazás.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Az alkalmazás egy másik olyan területen meghibásodása felfedezett SDK
### <a name="issue"></a>Probléma
* Felhasználói felületi adatok gyűjtése sikertelen (elemzés, figyelés, Szegmentálás vagy irányítópultok).
* Hibák Push (leküldéses értesítések nem működnek a app alkalmazást, vagy mindkettőt).
* Speciális funkció hibák (követési, földrajzi hely meghatározásának vagy adott leküldéses értesítések nem működnek platform).
* Sikertelen API (API-k sikertelen gyakran hibaüzenet nélkül csendes).
* A Szolgáltatáshibák (Azure Mobile Engagement egyike sem működik, az alkalmazás).

### <a name="causes"></a>Okok
* A legtöbb során felmerülő kérdések hello Azure Mobile Engagement SDK feloldva toobe úgy fog történni az alkalmazásban (például a felhasználói felület gyűjtemény hiba, leküldéses hiba, speciális szolgáltatás hibája, Alkalmazásprogramozási felületében, alkalmazás-összeomlásokat, vagy nyilvánvaló szolgáltatás hibája kimaradásáról).  
* Ha egy adott szolgáltatással az Azure Mobile Engagement az alkalmazás előtt soha nem működött, szüksége lesz a toocomplete hello integráció. 
* Ha az Azure Mobile Engagement egy adott szolgáltatással dolgozott, illetve leállt, szükség lehet a tooupgrade toohello legfrissebb verziója hello Azure Mobile Engagement SDK-t. Ne feledje, hogy nincs-e az Azure Mobile Engagement SDK hello Azure Mobile Engagement (Android, iOS, Windows és Windows Phone) által támogatott platformokon egy másik verziója.

#### <a name="sdk-integration"></a>SDK-integráció
* Az Azure Mobile Engagement SDK-ban (elemzés) nincs megfelelően integrálva.
* Reach SDK (az alkalmazás és kívüli alkalmazás leküldéses értesítések) nincs megfelelően integrálva.
* A tanúsítvány lejárt vagy nem megfelelő termék vs. Fejlesztői (csak iOS esetén).
* GCM vagy ADM nem megfelelően az SDK integrált (Android csak - szolgáltatás adott leküldéses értesítések).
* Nyomon követése nem megfelelően integrált SDK (telepítés tároló nyomon követése).
* Lusta helyét vagy GPS helyét nem megfelelően integrált SDK (célcsoportkezelést által földrajzi helyhez).

**Lásd még:**

* [SDK-dokumentáció - integráció útmutatók][Link 5] 
* [Hibaelhárítási útmutató - leküldéses][Link 23]

#### <a name="sdk-upgrade"></a>SDK frissítése
* Kell tooupgrade SDK tooresolve problémái hello SDK (hello az eszköz operációs rendszere verziója gyakran kapcsolódó toonewer) régebbi verzióit.
* Az alkalmazás összes korábbi verziójának eltávolítása az eszközről, és telepítse újra az alkalmazás legújabb verzióját hello, hello regisztrálja újra az Eszközazonosítót az hello Azure Mobile Engagement felhasználói felület tooconfirm, hogy az eszköz által használt hello az alkalmazás legújabb verzióját.

**Lásd még:**

* [SDK-dokumentáció – kibocsátási megjegyzések](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [SDK-dokumentáció - frissítési útmutatók](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Egyéb SDK
* Az Application Manifest "AndroidManifest.xml" hibákat okozhat az Azure Mobile Engagement toowork (csak Android esetén).
* Általános hiba az SDK-integráció és API-használati tooconfuse hello SDK kulcs, és az API-kulcs hello.

**Lásd még:**

* [Alapfogalmak - szószedet][Link 6]

## <a name="advanced-coding-issues"></a>Speciális problémák kódolása
### <a name="issue"></a>Probléma
* Adott platformkódot nem közvetlenül kapcsolódó tooAzure a Mobile Engagement az iOS, Android és Windows Phone problémákat okozhat.

### <a name="causes"></a>Okok
* Számos speciális az Azure Mobile Engagement kódolási problémák miatt nem megfelelően megírt platform által megadott kódja nem közvetlenül kapcsolódó tooAzure a Mobile Engagement. Szüksége lesz tooconsult dokumentáció adott toohello platform fejleszt a továbbá tooAzure a Mobile Engagement dokumentáció (Android, iOS, webalkalmazás, Windows és Windows Phone).
* Nem megfelelően állítja be "kategóriák", megakadályozza, hogy a linking helyről értesítési tooanother belül vagy kívül hello alkalmazás (csak Android esetén). 
* Nincs beállítva, az "UIKit.framework" túl "választható" a kódban iOS "Szimbólum nem található a hiba" mutat be és/vagy a régebbi iOS-eszközök (csak iOS) összeomlik.
* Lejárt tanúsítványok, vagy fejlesztői hello vagy hello cert termék verziója nem megfelelően használja, okok leküldéses kapcsolatos problémákat (csak iOS esetén).
* Nincsenek bizonyos korlátozások rejlő tooa platform, amely az Azure Mobile Engagement vezérlésére nem képes (ilyen például a system center hello működéséről az alkalmazásból leküldéses értesítések Android és iOS).
* Az Azure Mobile Engagement közzéteszi hello belső csomagok által használt Azure Mobile Engagement az iOS és Android hivatkozás teljes listáját. Ne feledje, hogy az Azure Mobile Engagement néhány szolgáltatása adott toohello platform (Android, iOS, webalkalmazás, Windows és Windows Phone).

### <a name="see-also"></a>Lásd még:
* [Hibaelhárítási útmutató - leküldéses][Link 23] 
* [SDK-dokumentáció – kibocsátási megjegyzések][Link 5]
* [SDK-dokumentáció - frissítési útmutatók][Link 5]

## <a name="application-crashes"></a>Alkalmazás-összeomlásokat
### <a name="issue"></a>Probléma
* Az alkalmazás összeomlik hello végfelhasználói eszközön.

### <a name="causes"></a>Okok
* Összeomlási adatok megtekinthetők a hello *Analytics felhasználói felület* vagy hello *Analytics API*
* A vizsgálati eszköz Eszközazonosító hello hajtanak végre bizonyos hello található, ami miatt a kérelem toocrash egy végfelhasználó toohelp az azonos művelet azonosítására a összeomlási hello okát.
* Azure Mobile Engagement SDK hello ismert problémái következtében az alkalmazások toocrash néha megoldó toohello hello SDK legújabb verziójának frissítése. Győződjön meg arról, hogy toocheck hello kibocsátási megjegyzések a platformra vonatkozó összeomlások vizsgálatakor.

### <a name="see-also"></a>Lásd még:
* [SDK-dokumentáció – kibocsátási megjegyzések][Link 5]
* [SDK-dokumentáció - frissítési útmutatók][Link 5]

## <a name="app-store-upload-failures"></a>Alkalmazás-áruház sikertelen feltöltés
### <a name="issue"></a>Probléma
* Kapcsolatos hibák toouploading hello legújabb verzióját az alkalmazás tooApple, Google vagy hello Windows alkalmazás-áruházból.

### <a name="causes"></a>Okok
* Alkalmazás tárolja néha letiltja az alkalmazások bizonyos engedélyezett szolgáltatásokkal (pl. hello Apple Store megakadályozza, hogy hello felhasználása idfv fogja elvégezni lehetővé hello áruházban található alkalmazásokra és hello GooglePlay tároló megakadályozza, hogy az alkalmazással kapcsolatos adatok alkalmazások közötti megosztásának hello). 
* Győződjön meg arról, hogy ellenőrizze hello kibocsátási megjegyzések a platform és az hello SDK jelenlegi verzióját, ha az alkalmazás-áruházra toohello feltöltése nehézséget.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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

