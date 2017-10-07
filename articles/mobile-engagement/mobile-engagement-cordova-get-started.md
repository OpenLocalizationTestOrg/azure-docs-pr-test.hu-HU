---
title: "az Azure Mobile Engagement a Cordova/Phonegap elindítva aaaGet"
description: "Megtudhatja, hogyan Cordova/Phonegap-alkalmazásokhoz az Azure Mobile Engagement az elemzések és leküldéses értesítések toouse."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 54fe9113-e239-4ed7-9fd1-a502d7ac7f47
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-phonegap
ms.devlang: js
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e67dabbdf7886802bb058f38964e558d5ae6854c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Ismerkedés az Azure Mobile Engagement Cordova/Phonegap-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand a használati és küldési leküldéses értesítések toosegmented felhasználók egy mobilalkalmazás Cordova használatával fejlesztett.

Ebben az oktatóanyagban létre fogunk hozni egy üres Cordova-alkalmazást Mac használatával, majd integráljuk a Mobile Engagement SDK-t. Az alkalmazás alapszintű elemzési adatokat gyűjt össze, és leküldéses értesítéseket fogad az iOS rendszerhez készült Apple leküldéses értesítési rendszerének (APNS) és az Android Google Cloud Messaging (GCM) szolgáltatásának a használatával. Fogjuk a tooan iOS vagy Android-eszköz tesztelési üzembe helyezni. 

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).
> 
> 

Ez az oktatóanyag hello következő szükséges:

* XCode, amely telepítése Mac App Store-ból (üzembe helyezéshez tooiOS)
* [Android SDK és -emulátor](http://developer.android.com/sdk/installing/index.html) (üzembe helyezéshez tooAndroid)
* leküldéses értesítési tanúsítvány (.p12), amelyet az Apple APNS fejlesztési központjában szerezhet be,
* GCM-projektszám, amelyet a Google Developer Console for GCM konzolon keresztül szerezhet be,
* [Mobile Engagement Cordova beépülő modul](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [!NOTE]
> Hello forráskód kereshet és információs hello hello Cordova beépülő modul a [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)
> 
> 

## <a id="setup-azme"></a>A Mobile Engagement beállítása a Cordova-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Kapcsolódás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt" mutat minimális hello határozza meg a szükséges toocollect dátumát és leküldéses értesítés küldéséhez. 

Létre fogunk hozni egy alapszintű alkalmazást a Cordova toodemonstrate hello integrációja:

### <a name="create-a-new-cordova-project"></a>Új Cordova-projekt létrehozása
1. Indítsa el *Terminálszolgáltatások* ablakot a Mac számítógép és a típus hello követő, amely létrehoz egy új Cordova-projekt hello alapértelmezett sablon alapján. Győződjön meg arról, hogy hello közzétételi profilt, végül használata toodeploy az iOS-alkalmazás által használt "com.mycompany.myapp", mert hello alkalmazás azonosítója. 
   
        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova
2. A projekt hajtható végre a következő tooconfigure hello **iOS** és futtassa azt hello iOS-szimulátor:
   
        $ cordova platform add ios 
        $ cordova run ios
3. A projekt hajtható végre a következő tooconfigure hello **Android** és hello Android-emulátorban való futtatáshoz. Győződjön meg arról, hogy az Android SDK Emulator beállításainál rendelkezik a cél Google APIs (Google Inc.) hello CPU / ABI értéke pedig a Google APIs ARM.  
   
        $ cordova platform add android
        $ cordova run android
4. Adja hozzá a Cordova-konzol beépülő modul hello. 

    ```
    $ cordova plugin add cordova-plugin-console
    ``` 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez
1. Hello Azure Mobile Engagement Cordova beépülő modul telepítése ugyanakkor biztosítható a hello változók értékeinek tooconfigure hello beépülő modult:
   
        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
             --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers hello app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android Reach Icon* : hello bármely kiterjesztést, vagy a rajzolható előtag nélkül hello erőforrás nevének kell lennie (például: mynotificationicon), és hello ikonfájlt az android-projekt (platformok/android/res/drawable) át kell másolni

*iOS Reach Icon* : hello a kiterjesztéssel együtt hello erőforrás nevének kell lennie (például: mynotificationicon.png), és hello ikonfájlt hozzá kell adni az iOS-projekthez az XCode (hello Hozzáadás fájlok menü használatával)

## <a id="monitor"></a>Valós idejű figyelés engedélyezése
1. A Cordova-projekt hello - szerkesztése **www/js/index.js** tooadd hello hívás tooMobile Engagement toodeclare új tevékenységet, egyszer hello *deviceReady* esemény érkezik.
   
         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }
2. Hello alkalmazás futtatásához:
   
   * **iOS esetén**
     
       A `Terminal` ablakban indítsa el az alkalmazást a Simulator új példányában hello alábbiak végrehajtásával:
     
           cordova run ios
   * **Android esetén**
     
       A `Terminal` ablakban indítsa el az alkalmazást az emulátor új példányában hello alábbiak végrehajtásával:
     
           cordova run android
3. Hello konzolnaplófájlokban hello következő látható:
   
        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
Mobile Engagement lehetővé teszi toointeract a felhasználókkal leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel hello kampányok. Ez a modul REACH neve hello a Mobile Engagement portálon.
hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.

### <a name="configure-push-credentials-for-mobile-engagement"></a>Leküldési hitelesítő adatok konfigurálása a Mobile Engagementhez
tooallow a Mobile Engagement toosend leküldéses értesítések küldése az Ön nevében, meg kell toogrant azt elérni tooyour Apple iOS-tanúsítvány vagy a GCM kiszolgálói API-kulcsot. 

1. Keresse meg a tooyour Mobile Engagement portálon. Nyissa meg azt a projekt használ, és kattintson a hello hello alkalmazásban **Engage** hello alsó gombra:
   
    ![][1]
2. Ekkor megnyílik a hello beállítások lapra az Engagement portálon. Kattintson a hello **natív leküldés** szakasz:
   
    ![][2]
3. iOS-tanúsítvány/GCM-kiszolgáló API-kulcsának konfigurálása
   
    **iOS**
   
    a. Jelölje ki .p12 tanúsítványát, töltse fel, és írja be jelszavát:
   
    ![][3]
   
    **Android**
   
    a. Hello Szerkesztés ikonra elé **API-kulcs** hello GCM Settings szakasz és hello előugró ablak, amely mutatja be, illessze be a hello GCM kiszolgálói kulcsot, és kattintson a **OK**. 
   
    ![][4]

### <a name="enable-push-notifications-in-hello-cordova-app"></a>Hello Cordova-alkalmazás leküldési értesítések engedélyezése
Szerkesztés **www/js/index.js** tooadd hello hívás tooMobile Engagement toorequest leküldéses értesítések és egy kezelő deklarálásához:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                 // on OpenUrl  
                 function(_url) {   
                 alert(_url);   
                 });  
            Engagement.startActivity("myPage",{});  
        }

### <a name="run-hello-app"></a>Hello alkalmazás futtatása
**iOS**

1. Rendszer XCode toobuild használja és hello eszköz tootest leküldéses értesítések hello alkalmazás telepítése, mivel az iOS csak lehetővé teszi a leküldéses értesítések tooan tényleges eszközön. Nyissa meg a Cordova-projekt létrehozási helyének toohello helyét, és keresse meg a túl**...\platforms\ios** helyét. Nyissa meg hello natív .xcodeproj fájlt az xcode-ban. 
2. Hozza létre és hello Cordova app toohello iOS-eszközt hello fiókot, amely rendelkezik hello létesítési profilt, amely tartalmazza az imént feltöltött szolgáltatáscsomagban toohello Mobile Engagement portálra és hello alkalmazásazonosító, amely megfelel egy létrehozásakor megadott hello hello tanúsítvány telepítése hello Cordova-alkalmazáshoz. Hello megtekintheti *csomagazonosítót* a a **erőforrások\*-info.plist** fájlt az XCode toomatch azt be. 
3. Hello szokásos iOS előugró ablak, az eszközön hello alkalmazást kér engedélyt toosend értesítések jelenik meg. Hello engedélyt. 

**Android**

Segítségével egyszerűen hello emulátor toorun hello Android-alkalmazás GCM értesítések támogatottak a hello Android-emulátorban. 

    cordova run android

## <a id="send"></a>Egy értesítési tooyour app küldése
Most létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses-tooyour hello eszközön futó alkalmazáshoz:

1. Keresse meg a toohello **elérni** a Mobile Engagement portálra a lap
2. Kattintson a **új hirdetmény** toocreate a leküldéses kampány
   
    ![][6]
3. Adja meg a bemeneti adatok toocreate a kampány **[Android]**
   
   * Adja meg a kampány nevét a **Name** mezőben. 
   * Jelölje be hello **kézbesítési típust** , *Rendszerértesítő* *egyszerű*
   * Jelölje be hello **kézbesítési időhöz** , *"Minden alkalommal"*
   * Adjon meg egy **cím** a Ez az első sor hello hello leküldéses értesítést.
   * Adjon meg egy **üzenet** az értesítéshez, amely hello üzenettörzs erre a célra. 
     
     ![][11]
4. Adja meg a bemeneti adatok toocreate a kampány **[iOS]**
   
   * Adja meg a kampány nevét a **Name** mezőben. 
   * Jelölje be hello **kézbesítési időhöz** , *"csak az alkalmazáson kívül"*
   * Adjon meg egy **cím** a Ez az első sor hello hello leküldéses értesítést.
   * Adjon meg egy **üzenet** az értesítéshez, amely hello üzenettörzs erre a célra. 
     
     ![][12]
5. Görgessen lefelé, és a hello tartalomszakasz válassza **csak értesítés**
   
    ![][8]
6. (Választható) Megadhatja egy művelet URL-címét is. Győződjön meg arról, hogy használja-e a hello beépülő modul konfigurálása során megadott URL-sémát **AZME\_ÁTIRÁNYÍTÁSI\_URL-cím** változó pl. *myapp://test*.  
7. Elkészült a beállítással hello lehető legegyszerűbb kampányt lehetséges. Görgessen lefelé, és kattintson a hello **létrehozása** toosave gombra a kampány.
8. Végül aktiválja a kampányt az **Activate** (Aktiválás) gombra kattintva.
   
    ![][10]
9. Ekkor meg kellene jelennie egy leküldéses értesítésnek az eszközön vagy az emulátorban a jelen kampány részeként. 

## <a id="next-steps"></a>Következő lépések
[A Cordova Mobile Engagement SDK-val elérhető összes módszer áttekintése](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

