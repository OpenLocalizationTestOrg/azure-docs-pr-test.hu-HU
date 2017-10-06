---
title: "aaaAdd leküldéses értesítések tooApache Cordova-alkalmazás az Azure Mobile Apps |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Mobile Apps toosend leküldéses értesítések tooyour Apache Cordova-alkalmazáshoz."
services: app-service\mobile
documentationcenter: javascript
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 8e1b23d6145b446b6f01599337b677e2f2b31d7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-apache-cordova-app"></a>Leküldéses értesítések tooyour Apache Cordova-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban hozzáadja leküldéses értesítések toohello [Apache Cordova gyors üzembe helyezési] projekt, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.

Ha nem használ hello letöltése – első lépések, leküldéses értesítési kiterjesztési csomagot kell hello. További információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][1].

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hozzá van rendelve egy, a Visual Studio 2015-öt hello Google Android Emulator, Android-eszközön, a Windows-eszköz és az iOS-eszközök futó létrehozott Apache Cordova-alkalmazást.

toocomplete ebben az oktatóanyagban szüksége:

* Számítógép, amelyen fut [Visual Studio Community 2015] [ 2] vagy újabb verzió.
* [Visual Studio Tools for Apache Cordova][4].
* Egy [aktív Azure-fiók][3].
* A befejezett [Apache Cordova gyors üzembe helyezési] [ 5] projekt.
* (Android) A [Google-fiók] [ 6] egy hitelesített e-mail címmel.
* (csak iOS) Egy [Apple Fejlesztőprogrambeli tagság] [ 7] és egy iOS-eszközön (iOS-szimulátor nem támogatja a leküldéses).
* (Windows) A [Windows áruház fejlesztői fiókjába] [ 8] és a Windows 10-eszközre.

## <a name="configure-hub"></a>Egy értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Ebben a szakaszban lépéseket bemutató videó megtekintése][9]

## <a name="update-hello-server-project"></a>Hello server projekt frissítése
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Módosítsa a Cordova-alkalmazáshoz
Győződjön meg a Apache Cordova-alkalmazás projekt telepítése hello Cordova leküldéses beépülő modul és a platform-specifikus leküldéses szolgáltatások által készen toohandle leküldéses értesítéseket.

#### <a name="update-hello-cordova-version-in-your-project"></a>A projekt hello Cordova-verzió frissítése.
A projekt Apache Cordova v6.1.1-nél korábbi verzióját használja, ha hello ügyfél projekt frissítése. a projekt hello tooupdate:

* Kattintson a jobb gombbal `config.xml` tooopen hello configuration designer.
* Válassza ki a hello platformok fülre.
* Adja meg 6.1.1 hello **Cordova CLI** szövegmezőben.
* Válasszon **Build**, majd **megoldás fordítása** tooupdate hello projekt.

#### <a name="install-hello-push-plugin"></a>Hello leküldéses beépülő modul telepítése
Apache Cordova-alkalmazás natív módon nem kezeli a eszköz vagy a hálózatkezelő képességeit.  Ezek a képességek által biztosított beépülő modulok, amelyek közzé vagy [npm] [ 10] vagy a Githubon.  Hello `phonegap-plugin-push` beépülő modul használt toohandle hálózati leküldéses értesítések.

Hello leküldéses beépülő modul az alábbi módszerek egyikével telepítheti:

**Hello-parancssorból:**

A következő parancs hello hajtható végre:

    cordova plugin add phonegap-plugin-push

**A Visual studióban:**

1. A Solution Explorerben nyissa meg a hello `config.xml` fájl kattintson **beépülő modulok** > **egyéni**, jelölje be **Git** telepítési forrásként, majd adja meg `https://github.com/phonegap/phonegap-plugin-push`hello forrásaként.

   ![][img1]

2. Kattintson a hello nyíl következő toohello telepítési forrás.
3. A **SENDER_ID**, ha már rendelkezik egy numerikus Projektazonosítónak hello Google Developer Console projekt, hozzáadhatja azt itt. Ellenkező esetben adja meg a helyőrző érték, például 777777.  Android céloz meg, ha ezt az értéket config.xml később frissítheti.
4. Kattintson az **Add** (Hozzáadás) parancsra.

hello leküldéses beépülő modul telepítve van.

#### <a name="install-hello-device-plugin"></a>Hello eszköz beépülő modul telepítése
Hajtsa végre az azonos hello tooinstall hello leküldéses beépülő modul használt művelet.  Hello eszköz beépülő modul hozzáadása hello Core beépülő modulok listában (kattintson **beépülő modulok** > **Core** toofind azt). A beépülő modul tooobtain hello platform név van szüksége.

#### <a name="register-your-device-on-application-start-up"></a>Regisztrálja az eszközt, az alkalmazás indítási
Kezdetben magában foglalja az egyes minimális kód Android rendszerhez. Hello app toorun iOS-vagy Windows 10 később módosíthatja.

1. Adjon hozzá egy túl**registerForPushNotifications** hello bejelentkezési folyamat vagy hello hello alján hello visszahívás során **onDeviceReady** módszert:

        // Login toohello service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added tooregister for push notifications.
                registerForPushNotifications();

            }, handleError);

    Ez a példa bemutatja hívása **registerForPushNotifications** sikeres hitelesítést követően.  Hívása `registerForPushNotifications()` gyakran szükség.

2. Adja hozzá az új hello **registerForPushNotifications** módszert az alábbiak szerint:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle hello registration event.
        pushRegistration.on('registration', function (data) {
          // Get hello native platform of hello device.
          var platform = device.platform;
          // Get hello handle returned during registration.
          var handle = data.registrationId;
          // Set hello device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }
3. (Android) Cserélje le a hello megelőző kódot, `Your_Project_ID` hello numerikus project azonosítója a az alkalmazás a [Google Developer Console][18].

## <a name="optional-configure-and-run-hello-app-on-android"></a>(Választható) Konfigurálása és az Android hello alkalmazás futtatása
Ez a szakasz tooenable leküldéses értesítések Android befejezéséhez.

#### <a name="enable-gcm"></a>Engedélyezze a Firebase Cloud Messaging
Mivel kezdetben vannak célzó hello Google Android platform, engedélyeznie kell a Firebase Cloud Messaging.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>Hello mobilalkalmazás háttér toosend leküldéses kérelmek FCM történő konfigurálása
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Konfigurálja a Cordova-alkalmazás Android rendszerhez
A Cordova-alkalmazáshoz, nyissa meg a config.xml, és cserélje le `Your_Project_ID` hello numerikus project azonosítója az alkalmazás a hello [Google Developer Console][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Nyissa meg a index.js és hello kód toouse frissítése a numerikus projekt ID azonosítójával.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>Android-eszköz az USB-hibakeresés konfigurálása
Az alkalmazás tooyour Android-eszköz telepítése előtt kell tooenable USB-hibakeresést.  Androidos telefonja hajtsa végre a következő lépéseket:

1. Nyissa meg túl**beállítások** > **névjegye**, koppintson a hello **buildszáma** mindaddig, amíg a fejlesztői mód engedélyezve van (körülbelül hét alkalommal).
2. Vissza a **beállítások** > **fejlesztői beállítások** engedélyezése **USB-hibakeresés**, majd csatlakoztassa az Android-telefonon tooyour fejlesztési számítógép USB-kábellel.

Tesztelt Ez egy Android 6.0 (Marshmallow) rendszert futtató Google Nexus 5 X-eszköz használatával.  Hello technikák azonban által közösen bármelyik modern Android verzióját.

#### <a name="install-google-play-services"></a>Google Play-szolgáltatások telepítése
hello leküldéses beépülő modul az Android Google Play-szolgáltatások a leküldéses értesítések támaszkodik.

1. A Visual Studióban kattintson **eszközök** > **Android** > **Android SDK Manager**, bontsa ki a hello **kiegészítő funkciók** mappa és ellenőrzés hello mezőben toomake a következő SDK-k hello mindegyikének telepítve van-e.

   * Android 2.3-as vagy újabb
   * Google-tárház változat 27 vagy nagyobb
   * Google Play-szolgáltatásokra 9.0.2 vagy újabb

2. Kattintson a **csomagok telepítése** és hello telepítési toocomplete várja.

hello aktuális szükséges kódtárak hello szereplő [phonegap-beépülőmodul-leküldéses telepítési dokumentáció][19].

#### <a name="test-push-notifications-in-hello-app-on-android"></a>Teszt leküldéses értesítések Android rendszeren hello alkalmazásban
Most teszt leküldéses értesítések futtatásával hello alkalmazás és hello TodoItem tábla beszúrni elemeket is. Tesztelheti a hello ugyanarra az eszközre, illetve egy második eszközről mindaddig, amíg használ hello azonos háttér. Hello Android platformon a Cordova-alkalmazás tesztelése a következő módokon hello egyikében:

* **A fizikai eszközön:** a Android-eszköz tooyour fejlesztési számítógép USB-kábellel csatlakoztassa.  Ahelyett, hogy **Google Android Emulator**, jelölje be **eszköz**. Visual Studio hello alkalmazás toohello eszköz telepíti, és ezután a hello alkalmazást futtat.  Majd végezhet hello eszközön hello alkalmazással.

  A fejlesztési felhasználói élmény javítása.  A képernyő megosztása például alkalmazások [Mobizen] [ 20] segíthet az Android alkalmazások fejlesztése.  Mobizen projektek Android képernyő tooa webböngészőben a számítógépen.

* **Az Android-emulátorban:** az emulátor futtatásához szükséges további konfigurációs lépésekre.

    Ellenőrizze, hogy tooa virtuális eszköz, amely rendelkezik a Google API-k hello célként beállítása látható módon hello Android virtuális eszközt (AVD) manager telepít.

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Ha azt szeretné, toouse gyorsabb x86 emulátor, akkor [hello HAXM illesztőprogram telepítése] [ 11] és hello emulátor toouse konfigurálja azt.

    Google fiók toohello Android-eszköz hozzáadásához kattintson **alkalmazások** > **beállítások** > **fiók hozzáadása**, majd kövesse a hello megjelenő utasításokat.

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Futtassa a hello todolist alkalmazást előtt, és egy új teendő elem beszúrására. Megadott idő hello értesítési területén egy értesítési ikon jelenik meg. Hello értesítési fiók tooview hello teljes szöveges hello értesítés nyithatja meg.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(Választható) Konfigurálására és futtatására IOS rendszerű eszközökön
Ez a szakasz az iOS-eszközök hello Cordova-projekt futtatása. Ha nem dolgozik iOS-eszközökkel, ez a szakasz kihagyhatja.

#### <a name="install-and-run-hello-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Telepítés és távoli futási hello iOS ügynök létrehozása Mac vagy felhőalapú szolgáltatásként-kiszolgálón
A Cordova-alkalmazást a Visual Studio használatával iOS futtatása előtt végrehajtania hello lépéseket a hello [iOS beállítási útmutatója] [ 12] tooinstall és távoli futási hello készítsen ügynök.

Ellenőrizze, hogy hello alkalmazást IOS rendszerre hozhat létre. hello a hello beállítási útmutatója lépésekre az IOS rendszerhez készült Visual Studio szükséges toobuild. Ha nem rendelkezik a Mac, IOS-egy szolgáltatás, például MacInCloud hello távoli build ügynök használatával hozhat létre. További információk: [az iOS-alkalmazás futtatása hello felhőben][21].

> [!NOTE]
> XCode 7-es vagy nagyobb szükség toouse hello leküldéses IOS beépülő modul.

#### <a name="find-hello-id-toouse-as-your-app-id"></a>Az App-ID hello azonosító toouse keresése
Regisztrálja az alkalmazást a leküldéses értesítésekre, mielőtt config.xml nyissa meg a Cordova-alkalmazáshoz, a hello található `id` attribútumú hello widget elemben, és másolja azt a későbbi használatra. A következő XML hello, hello azonosító: `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Ez az azonosító újabb verzióival való használatához, amikor egy alkalmazás azonosítója hoz létre az Apple fejlesztői portáljáról. Ha hello fejlesztői portálján hoz létre egy másik alkalmazás Azonosítóját, akkor néhány további lépés szükséges, az oktatóanyag későbbi részében. a widget elem hello Azonosítójának meg kell egyeznie a hello Alkalmazásazonosító hello developer portálon.

#### <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a>Hello alkalmazás az Apple fejlesztői portálján a leküldéses értesítések regisztrálása
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Tekintse meg hasonló lépéseket ismertető videót](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-toosend-push-notifications"></a>Az Azure toosend leküldéses értesítések konfigurálása
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Győződjön meg arról, hogy az alkalmazás azonosítója egyezik-e a Cordova-alkalmazáshoz
Ha már létrehozott az Apple fejlesztői fiókban Alkalmazásazonosító hello hello widget elemének config.xml hello azonosítója megegyezik, kihagyhatja ezt a lépést. Azonban ha hello azonosító nem egyezik, ideig hello a következő lépéseket:

1. A projekt hello platformok mappa törlése.
2. A projekt hello beépülő modulok mappa törlése.
3. A projekt hello node_modules mappa törlése.
4. Frissítés hello az id attribútum hello widget elemének config.xml toouse hello alkalmazás azonosítója, amelyet az Apple fejlesztői fiókban hozott létre.
5. A projekt újraépítéséhez.

##### <a name="test-push-notifications-in-your-ios-app"></a>Teszt leküldéses értesítéseket az iOS-alkalmazás
1. Visual Studio, ügyeljen arra, hogy **iOS** hello központi telepítés céljaként van kiválasztva, és válassza a **eszköz** toorun csatlakoztatott iOS-eszközön.

    Az iOS-eszköz csatlakoztatásakor tooyour PC futtathatja iTunes használatával. hello iOS-szimulátor nem támogatja a leküldéses értesítéseket.

2. Nyomja le az hello **futtatása** gomb vagy **F5** a Visual Studio toobuild hello projektet és a start hello alkalmazást az iOS-eszközt, majd kattintson **OK** tooaccept leküldéses értesítéseket.

   > [!NOTE]
   > hello app során először futtassa hello leküldéses értesítések megerősítést kér.

3. Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (+) ikonra.
4. Győződjön meg arról, hogy értesítést kap, majd kattintson az OK toodismiss hello értesítést.

## <a name="optional-configure-and-run-on-windows"></a>(Választható) Konfigurálja és futtassa a Windows
Ez a szakasz olyan hello Apache Cordova-alkalmazás projekt futó Windows 10 rendszerű eszközökhöz (Windows 10 hello PhoneGap leküldéses beépülő modul támogatja). Ha nem dolgozik Windows-eszközökkel, ez a szakasz kihagyhatja.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Alkalmazás regisztrálása a Windows leküldéses értesítéseket a WNS-sel való
toouse hello tárolási lehetőségek Visual Studio, válasszon ki egy Windows cél listából hello megoldás platformok, például **Windows-x64** vagy **Windows-x86** (elkerülése érdekében **Windows-AnyCPU** a leküldéses értesítések).

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Egy hasonló lépéseket bemutató videó][13]

#### <a name="configure-hello-notification-hub-for-wns"></a>A WNS hello értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-toosupport-windows-push-notifications"></a>A Cordova-alkalmazás toosupport Windows leküldéses értesítések konfigurálása
Nyissa meg hello configuration designer (kattintson a jobb gombbal a config.xml, és válassza **adatforrásnézet-tervezőből**), jelölje be hello **Windows** lapot, és válassza a **Windows 10** alatt**Windows verzió cél**.

toosupport leküldéses értesítések az alapértelmezett (hibakereséshez) hoz létre, nyissa meg build.json fájl. Másolja a "release" konfigurációs tooyour hibakeresési beállításait.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Hello frissítés után hello build.json tartalmaznia kell a következő kód hello:

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Hozza létre hello alkalmazását, és ellenőrizze, hogy rendelkezik-e hibák. Az ügyfélalkalmazás most hello Mobile Apps-háttéralkalmazás hello értesítéseihez kell regisztrálni. Ismételje meg minden Windows-projektet a megoldásban ez a szakasz.

#### <a name="test-push-notifications-in-your-windows-app"></a>Teszt leküldéses értesítések a Windows-alkalmazásokban
A Visual Studio, győződjön meg arról, hogy a Windows platform van kiválasztva a telepítés céljaként hello, például a **Windows-x64** vagy **Windows-x86**. Válassza ki a Visual Studio üzemeltető Windows 10 rendszerű toorun hello app **helyi számítógép**.

Nyomja le az hello futtassa gomb toobuild hello projektet, és indítsa el hello alkalmazást.

Hello alkalmazásban, írja be egy új todoitem nevét, és kattintson a hello plusz (+) ikonra tooadd azt.

Győződjön meg arról, hogy értesítést kapott, hello cikk felvételekor.

## <a name="next-steps"></a>Következő lépések
* További információ a [Notification Hubs] [ 17] toolearn kapcsolatos leküldéses értesítéseket.
* Ha még nem tette meg, továbbra is hello oktatóanyag által [hozzáadása hitelesítési] [ 14] tooyour Apache Cordova-alkalmazáshoz.

Ismerje meg, hogyan toouse hello SDK-k.

* [Apache Cordova-SDK][15]
* [ASP.NET Server SDK][1]
* [NODE.js Server SDK][16]

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
