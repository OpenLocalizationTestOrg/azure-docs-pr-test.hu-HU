---
title: "aaaAzure Mobile Engagement hibaelhárítási útmutatók"
description: "Az Azure Mobile Engagement a hibaelhárítási útmutatója"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 31134a29-a513-4e5e-b626-f6cf6fe04769
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: dd69bfd7019907c3e1da8df590db3b5f61606173
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure Mobile Engagement – hibaelhárítási útmutató
## <a name="introduction"></a>Bevezetés
hibaelhárítási útmutató követése hello segítenek megérteni az alapvető okok néhány gyakran látható problémákat és a rendszer lehetővé teszik a saját tootroubleshoot. 

## <a name="general"></a>Általános kérdések
Általában hello következő mindig biztosítja:

1. Győződjön meg arról, hogy minden hello lépéseit az integráció a lezajlott a [bevezető oktatóanyagok](mobile-engagement-windows-store-dotnet-get-started.md)
2. Hello Platform SDK-k hello a legújabb verzióját használja. 
3. Tesztelése tényleges eszközre és az emulátor, mert az egyes problémák csak az adott tooemulator. 
4. Ön nem elérte-e bármilyen korlátok/szabályozások a Mobile Engagement ismertetett [Itt](../azure-subscription-service-limits.md)
5. Ha nem sikerül tooconnect toohello a Mobile Engagement service háttér vagy az adatok nem betöltését folyamatosan jelent, akkor győződjön meg arról, hogy nincsenek-e nincs folyamatban lévő szolgáltatás incidensek ellenőrzésével [Itt](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>"A figyelő" problémák
### <a name="i-am-not-seeing-my-device-showing-up-on-hello-monitor-tab"></a>Nem látható az eszközöm a hello figyelő lapon jelenik meg
Figyelés lapján hello eszközök csatlakoztatott tooyour a Mobile Engagement platform valós időben jeleníti meg. Ha az emulátor és az eszköz hibakeresést, majd megtekintheti az itt legalább egy munkamenetet. Hello app került, majd látják hello mérőműszer tükrözze hello eszközök, amelyek csatlakoztatott toohello platform valós idejű aktív munkamenetek. 

Ha nem Ön az eszköz hello figyelő lapon akkor célszerű valószínűleg SDK-integráció hibát. Néhány általános lépéseket tootake tootroubleshoot a következők:

1. Győződjön meg arról, hogy hello mobilalkalmazás hello megfelelő kapcsolati karakterláncot használ a és hello SDK kulcsai című szakaszban, és nem hello API-kulcsokat szakasz. hello kapcsolati karakterlánc hello Mobile Engagement-alkalmazást, amelyben látni fogja az eszköz hello figyelő lapon a mobilalkalmazás toohello példányának csatlakozik. 
2. Windows Platform - Ha az oldala felülírja hello `OnNavigatedTo` módszert, győződjön meg arról, hogy toocall `base.OnNavigatedTo(e)`.
3. Ha egy meglévő mobilalkalmazás rendszer integrálása a Mobile Engagement, akkor is biztosítható, hogy nem hiányoznak-e be a lépéshez sem integrációs lépések speciális hello megtekintésével [Itt](mobile-engagement-windows-store-integrate-engagement.md)
4. Győződjön meg arról, legalább egy képernyőt/tevékenység küldi hello lapon EngagementActivity attól függően, hogy a hello dolgozik hello platform felülbírálásával [bevezető oktatóanyagok](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-hello-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Hello figyelő lap egy munkamenet jelenik hivatkozva még akkor is rendelkezik kapcsolata megszakadt vagy esetén az alkalmazás bezárása / emulátor.
Ha hello csak egy csatlakoztatott toohello platform ezen a ponton, és az emulátor tooopen hello alkalmazást használ majd valószínű tooemulator régi stílusú miatt. Általában akkor kell, hogy Ön térjen vissza toohello kezdőképernyőn hello app munkamenet toodisconnect hello-emulátor sikeresen tooensure. Emellett a Windows platformra, hibakeresése a Visual Studio során szükség lehet, hogy a Visual Studio módba toohello tooensure **életciklus-események** egérrel a menüsoron, majd kattintson a **Suspend** tooreally bezárása hello munkamenet. Lásd: [Windows oktatóanyag](mobile-engagement-windows-store-dotnet-get-started.md) részleteiről. 

## <a name="analytics-issues"></a>"Analytics" problémák
### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>I nem jelennek meg adatok / Analytics lapon adatok frissítése
Analitikai adatok rendszeres időközönként újraszámítja, és eltarthat legfeljebb 24 órát a frissítést. Az adatok nem valós idejű, és látni fogja a 24 órás időszakban belül frissülnek.
Győződjön meg arról, azonban, hogy küldendő legalább egy képernyőt vagy tevékenység toohello platform háttér által vagy felülbíráló legalább egy oldalt a `EngagementActivity` és a `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-hello-analytics-tab"></a>Egy eszköz helytelenül rögzített dátum/idő hivatkozva hello Analytics lap
hello Analytics időszakát alapul hello felhasználói eszközbeállítások hello dátumot. Ezért győződjön meg arról, hogy hello eszköz megfelelő hello dátummal rendelkezik. 

## <a name="segment-issues"></a>"Szegmens" problémák
### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>A szegmens létrehozott és jelenik meg, mert szürkén jelenik meg, vagy nem jelennek meg adatok
Szegmens létrehozása nem a valós idejű hello pillanatban. A hello számítható azonos időben hello analitikai adatok összesített értéket és így is telhet, amíg legfeljebb 24 óra. Meg kell, próbálkozzon újra később, de eközben gondoskodnia kell arról, hogy a mobile apps szolgáltatásban valóban küldi hello adatok, amelyek hello szegmensek vannak képező hello alapon. Például Ha egy esemény mondja ki a "PEL" nem küldött minden olyan mobileszközre által, akkor egy szegmens EventName létre szegmens adatokat így nem = PEL hello feltételként. Azt is ellenőrizze az SDK-integráció tooensure a mobilalkalmazás megfelelő hello adatokat küld. 

## <a name="reach-or-push-notifications-issues"></a>"Elérni" vagy a leküldéses értesítésekkel kapcsolatos problémák
### <a name="my-push-messages-are-not-being-delivered"></a>A leküldéses üzenetek nem kézbesíthetők
1. Próbáljon meg, hogy minden hello - mobilalkalmazás, az SDK és hello szolgáltatás összetevői megfelelően csatlakoztatott, és képes toodeliver leküldéses értesítések értesítések tooa vizsgálati eszköz első tooensure küldeni. 
2. Mindig értesítésküldés hello legegyszerűbb "out-az-app" először egy kampányt, amely nem ütemezett keresztül, és sem rendelkezik a megadott célközönség-feltétel. Ez a újra tooprove, hogy értesítést kapcsolat megfelelően működik-e. 
3. Ha az alkalmazásbeli értesítések kézbesítéséhez, akkor is célszerű jó problémák első lépés az alkalmazáson belüli out értesítések küldése először tootry. 
4. Győződjön meg arról, hogy a mobilalkalmazás megfelelően konfigurálva a natív leküldés hello. Attól függően, hogy hello platform, vagy tartalmazzon (Android, Windows) kulcsokat vagy tanúsítványokat (iOS). Lásd: [felhasználói felület – beállítások](mobile-engagement-user-interface-settings.md)
5. Értesítések is letiltotta hello alkalmazásból hello mobil operációs rendszerek ezért figyeljen oda arra a felhasználó a helyzet nem hello. 
6. Győződjön meg arról, hogy nem állítja hello *figyelmen kívül a célközönséget, leküldéssel fogják megkapni keresztül hello API toousers* hello beállítása **kampány** egy Reach szakasza kampány, mivel ezzel biztosíthatja, hogy a leküldéses értesítések sikerült csak elküldeni API-k használatával. 
7. Győződjön meg arról, hogy mindkét Wi-Fi és a telefon operátor hálózati tooeliminate hello hálózati kapcsolaton keresztül a problémák lehetséges forrásaként csatlakoztatott eszközökkel a leküldéses kampány teszteli.
8. Győződjön meg arról, hogy hello rendszer dátum/idő az eszköz/emulátorának helyességéről, mert minden szinkronizált eszköz is zavarja hello leküldéses értesítési szolgáltatás képes toodeliver értesítések. 

További platformspecifikus hibaelhárítás az alábbi utasításokat:

1. **iOS** 
   
   * Győződjön meg arról, hogy hello tanúsítványok érvényes és leküldéshez IOS leküldéses értesítések küldése. 
   * Győződjön meg arról, hogy megfelelően konfigurálja a *éles* tanúsítványt a Mobile Engagement-alkalmazáshoz. 
   * Győződjön meg arról, hogy a tesztelés egy *valódi, fizikai eszköz.* hello iOS-szimulátorban történő leküldéses üzenet nem dolgozható fel.
   * Győződjön meg arról, hogy hello Bundle Identifier hello mobilalkalmazásban megfelelően van beállítva. Hello utasítások [Itt](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
   * Tesztelési, használjon Ad-Hoc"terjesztési a mobil létesítési profiljához. Nem fogja tudni tooreceive értesítést, ha az alkalmazás fordítása a "Debug"
2. **Android**
   
   * Győződjön meg arról, hogy a mobilalkalmazás AndroidManifest.xml fájlhoz, amely \n karakter követ hello megfelelő projektszám megadott. 
     
           <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
   * Győződjön meg arról, hogy nem hiányoznak, vagy hibásan konfigurált azokat az engedélyeket a hello Android-jegyzékfájl 
   * Gondoskodjon arról, hogy hello Projektszám tooyour ügyfélalkalmazás ad hozzá a fiókot használja, ahol portáltól hello GCM kiszolgálói kulcsot hello. Két hello bármely eltérést kikerülését megakadályozza a leküldéses értesítések. 
   * Ha rendszer kap értesítést, de nem alkalmazásbeli tekintse át a hello [adja meg az értesítések ikonjának](mobile-engagement-android-get-started.md) , valószínűleg nem meg hello megfelelő ikon hello Android Manifest fájl. 
   * Ha BigPicture értesítést küldenek, akkor győződjön meg arról, hogy ha külső kép kiszolgálóval rendelkezik, akkor toobe képes toosupport HTTP van szükségük "GET" és "HEAD".
3. **Windows**
   
   * Győződjön meg arról, hogy társított hello app egy érvényes Windows áruház egy alkalmazásához. A Visual Studio - projekt hello kattintson, és válassza a "Társítsa az Alkalmazásáruházból" lehetőséget, és válassza hello alkalmazás Windows áruház hello létrehozott tooright fog rendelkezni. A Windows Áruházbeli alkalmazás hello ugyanazt a ahol portáltól hello natív leküldéses hitelesítő adatok tooconfigure hello Mobile Engagement portál kell lennie.
   * Ha azért kapta, out-az-app leküldéses értesítések, de nem alkalmazáson belüli értesítések `EngagementOverlay` integrációs majd gondoskodjon arról, hogy a gyökérelem rács a lapon. EngagementOverlay használ hello azt az első "Rács" elemet, az xaml fájl tooadd két webes nézetek a lapon talál. Ha azt szeretné, hogy hol lesz beállítva, webes nézetek toolocate, definiálhat ilyen nevű "EngagementGrid", azonban meg kell tooensure rács nincs elegendő magasságát és szélességét hello két további webalkalmazás-nézetek, amely hello értesítés megjelenítése és a következő hello az alkalmazáson belüli értesítés hirdetményben:
     
           <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-hello-notification-it-is-showing-as-active-what-does-it-mean"></a>A leküldéses értesítési/közlemény létrehozott/kampány, és akkor is, az elküldött me hello értesítési, hogy azt "Aktív". Mit jelent?
Hello **kampány** létrehozott Mobile Engagement nevezik, mivel egy hosszú ideig tartó leküldéses értesítési jelentése szerint az új eszközök csatlakoznak tooyour mobile engagement platform, azok automatikusan küldi hello értesítés Itt konfigurált, mindaddig, amíg hello kampány beállítása hello feltételnek megfelelnek. Ez az nem egy egy hibaüzenetet egyetlen értesítés beállítása. Kattintson a hello toomanually követően **Befejezés** tooterminate hello kampány gombra, így nem további értesítések küldéséhez. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-hello-app-i-get-hello-same-notification-even-when-i-had-actioned-it-before"></a>A leküldéses kampány létrehozott és kapok értesítések sikeresen azonban hello alkalmazás megnyitásakor, amikor jelenik meg: hello azonos értesítési akkor is, ha korábban műveletet kiváltó azt előtt?
Ez az valószínűleg toohappen tesztelése során, és ha az emulátorok vagy néhány TestFlight például keretrendszeréhez. Mi történik, minden példány futtatása-alkalmazást, hogy egy új DeviceID beszerzése és küldéséhez tooour háttér hello a Mobile Engagement platform tootreat okozó Ez az új eszköz-és hello értesítést küld. 

## <a name="getting-support"></a>Keresztüli támogatás
Ha Ön nem tooresolve hello probléma saját kezűleg majd a következőket teheti:

1. Keresse meg a problémát a meglévő szálak hello StackOverflow fórumon és [MSDN fórum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) , és ha nem, majd kérdése van. 
2. Ha megtalálta egy szolgáltatást, majd adja hozzá vagy szavazattal hello kérelem található meg a [UserVoice fórum](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Ha rendelkezik a Microsoft támogatja a nyissa meg a támogatási incidensek, adja meg a következő adatok hello: 
   * Az Azure előfizetés-azonosító
   * Platform (pl. iOS, Android stb.)
   * Alkalmazásazonosító
   * A kampány Azonosítóját (a leküldéses értesítési problémák)
   * Eszközazonosító
   * Mobile Engagement SDK-verzió (pl. Android SDK v2.1.0)
   * A pontos hibaüzenet és forgatókönyv hiba részletei

