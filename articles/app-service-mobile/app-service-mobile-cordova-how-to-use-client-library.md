---
title: "Apache Cordova beépülő modul az Azure Mobile Apps aaaHow tooUse"
description: "Hogyan tooUse Apache Cordova beépülő modul Azure Mobile Apps-alkalmazáshoz"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: d3e0639e6478c409132af25304a2fb0f28401e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-apache-cordova-client-library-for-azure-mobile-apps"></a>Hogyan toouse Apache Cordova ügyféloldali kódtár Azure Mobile Apps-alkalmazáshoz
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ez az útmutató útmutatást ad, hogy tooperform szolgáltatást használó általános forgatókönyvhöz hello legújabb [Apache Cordova beépülő modul az Azure Mobile Apps]. Ha új tooAzure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] toocreate egy háttér, hozzon létre egy táblát, és egy előre elkészített Apache Cordova-projekt letöltése. Az útmutató azt összpontosítani hello ügyféloldali Apache Cordova beépülő modul.

## <a name="supported-platforms"></a>A támogatott platformok
Ez az SDK támogatja az Apache Cordova v6.0.0 és későbbi iOS, Android és Windows eszközökhöz.  hello eszközplatform-támogatás a következőképpen történik:

* Android API 19-24 (KitKat nugát keresztül).
* iOS 8.0-s és újabb verziók.
* Windows Phone 8.1-es.
* Az univerzális Windows Platform.

## <a name="Setup"></a>A telepítő és Előfeltételek
Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón. Ez az útmutató feltételezi, hogy hello táblához hello hello táblák ezen oktatóprogram az azonos sémából. Ez az útmutató feltételezi hozzáadásának hello Apache Cordova beépülő modul tooyour kódot.  Ha nem tette, bővítheti hello Apache Cordova beépülő modul tooyour projekt hello parancssorban:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

További létrehozásával kapcsolatos információkat [az első Apache Cordova-alkalmazás], azok dokumentációjában talál.

## <a name="ionic"></a>Ionos v2 alkalmazás beállítása

tooproperly ionos v2-projekt konfigurálása, hozzon létre egy alapszintű alkalmazást, majd hello Cordova beépülő modul hozzáadása:

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

Adja hozzá az alábbi túl hello`app.component.ts` toocreate hello objektumot:

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

Most már létrehozhatja és futtatását hello projekt hello böngészőben:

```
ionic platform add browser
ionic run browser
```

hello Azure Mobile Apps Cordova beépülő modul mindkét ionos v1 és v2 alkalmazásokat támogatja.  Csak hello ionos v2 alkalmazások a kiegészítő nyilatkozat kérése hello `WindowsAzure` objektum.

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Hogyan: hitelesíti a felhasználókat
Az Azure App Service támogat hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók: Facebook, Google, a Microsoft Account és Twitter. Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók. Használhatja a hitelesített felhasználók tooimplement az engedélyezési szabályok hello identitás server parancsfájlokban. További információkért lásd: hello [Bevezetés a hitelesítés használatába] oktatóanyag.

Ha hitelesítési Apache Cordova-alkalmazást használ, a következő Cordova beépülő modulok hello elérhetőnek kell lennie:

* [cordova-beépülőmodul-eszköz]
* [cordova-beépülőmodul-inappbrowser]

Két hitelesítési forgalom támogatottak: a kiszolgáló folyamata és egy ügyfél folyamatában.  hello kiszolgáló folyamata nyújt hello legegyszerűbb hitelesítési élmény hello szolgáltató webes hitelesítés felület támaszkodnak. hello ügyfél folyamata lehetővé teszi, hogy szorosabb integrációt eszközspecifikus képességeket például single-sign-on, szolgáltatói eszközspecifikus SDK-k támaszkodnak az.

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Útmutató: a Mobile App Service konfigurálása külső átirányítási URL-címeket.
Az Apache Cordova-alkalmazások számos különböző használja egy visszacsatolási funkció toohandle OAuth UI zajlik.  A localhost OAuth UI-adatfolyamok miatt a problémák, mivel az hello hitelesítési szolgáltatás csak hogyan tooutilize a szolgáltatás alapértelmezés szerint.  Hibás OAuth UI flow például a következők:

* hello Fodrozás emulátor.
* Élő rendelkező ionos be újra.
* Hello mobil-háttéralkalmazást helyileg futó
* Egy másik Azure App Service, mint egy olyan hitelesítési hello hello mobil-háttéralkalmazást fut.

Kövesse ezeket az utasításokat tooadd a helyi toohello beállításokat:

1. Jelentkezzen be toohello [Azure-portálon]
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás hello nevére.
3. Kattintson a **eszközök**
4. Kattintson a **erőforrás-kezelő** hello OBSERVE menüben, majd kattintson a **Ugrás**.  Megnyílik egy új ablakot vagy lapot.
5. Bontsa ki a hello **config**, **authsettings** a hely hello bal oldali navigációs csomópontok.
6. Kattintson a **szerkesztése**
7. Keresse meg hello "allowedExternalRedirectUrls" elemet.  Toonull vagy egy tömböt is beállítható.  A következő érték hello érték toohello módosítása:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Cserélje le a szolgáltatás URL-címei hello hello URL-címeket.  Például "http://localhost: 3000" (a hello Node.js sample szolgáltatás), vagy "http://localhost:4400" (a hello Fodrozás szolgáltatás).  Azonban ezen URL-címei, példák - hello szolgáltatások említett hello példák, beleértve a helyzetnek eltérő lehet.
8. Kattintson a hello **olvasási/írási** hello képernyő jobb felső sarkában hello gombra.
9. Kattintson a zöld hello **PUT** gombra.

hello-beállítások mentése ezen a ponton.  Ne zárja be az hello böngészőablakban amíg tart hello-beállítások mentése.
Ezek visszacsatolási URL-címek toohello CORS-beállítások az App Service is hozzáadhat:

1. Jelentkezzen be toohello [Azure-portálon]
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás hello nevére.
3. hello-beállítások panel automatikusan megnyílik.  Ha nem, kattintson a gombra **összes beállítás**.
4. Kattintson a **CORS** hello API menüjében.
5. Meg kívánja hello mezőben megadott tooadd hello URL-címet, és nyomja le az ENTER billentyűt.
6. Adja meg a további URL-címek, szükség szerint.
7. Kattintson a **mentése** toosave hello beállításait.

Hello új beállítások tootake hatás körülbelül 10 – 15 másodpercre vesz igénybe.

## <a name="register-for-push"></a>Útmutató: a leküldéses értesítések regisztrálása
Telepítse a hello [phonegap-beépülőmodul-leküldéses] toohandle leküldéses értesítéseket.  A beépülő modul könnyen segítségével adhatók a `cordova plugin add` parancsot a parancssorban hello, vagy hello Git beépülő modul telepítő Visual Studio segítségével.  A következő kódot az Apache Cordova-alkalmazás regisztrálja az eszközt a leküldéses értesítések:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use hello device plugin toodetermine hello device
    // Best is toouse device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by hello PNS - check hello format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Hello Notification Hubs SDK toosend leküldéses értesítések hello kiszolgálóról használja.  Soha ne küldjön leküldéses értesítéseket közvetlenül az ügyfelektől. Ez így használt tootrigger egy szolgáltatásmegtagadási támadások elleni Notification Hubs vagy hello PNS.  hello PNS sikerült bA a forgalom miatt az ilyen támadásokat.

## <a name="more-information"></a>További információ

A részletes API részletei a [API-JÁNAK dokumentációja](http://azure.github.io/azure-mobile-apps-js-client/).

<!-- URLs. -->
[Azure-portálon]: https://portal.azure.com
[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md
[Bevezetés a hitelesítés használatába]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova beépülő modul az Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[az első Apache Cordova-alkalmazás]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-beépülőmodul-leküldéses]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-beépülőmodul-eszköz]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-beépülőmodul-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
