---
title: "aaaAdd leküldéses értesítések tooyour Android-alkalmazást a Mobile Apps |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Mobile Apps toosend leküldéses értesítések tooyour Android-alkalmazás."
services: app-service\mobile
documentationcenter: android
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: dcfb8463b395904b4bd0bf9bde2f71f259894066
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-android-app"></a>Leküldéses értesítések tooyour Android-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Android gyors üzembe helyezési] projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.

Ha nem használ hello letöltése – első lépések, leküldéses értesítési kiterjesztési csomagot kell hello. További információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Előfeltételek
Hello a következőkre lesz szüksége:

* Egy IDE, attól függően, hogy a projekt háttér:

  * [Android Studio](https://developer.android.com/sdk/index.html) a Node.js háttérből az alkalmazás-e.
  * [A Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) vagy újabb verzióját, ha az alkalmazás a Microsoft .NET-háttérből.
* Android 2.3-as vagy újabb verzió, a Google-tárház változat 27 vagy újabb és a Google Play-szolgáltatásokra 9.0.2 vagy újabb Firebase Cloud Messaging.
* Teljes hello [Android gyors üzembe helyezési].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>A Firebase Cloud Messaginget támogató projekt létrehozása
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Egy értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-toosend-push-notifications"></a>Az Azure toosend leküldéses értesítések konfigurálása
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-hello-server-project"></a>Engedélyezze a leküldéses értesítéseket hello server projekthez
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-tooyour-app"></a>Leküldéses értesítések tooyour alkalmazás hozzáadása
Ebben a szakaszban frissíti az ügyfél Android-alkalmazás toohandle leküldéses értesítéseket.

### <a name="verify-android-sdk-version"></a>Android SDK-verziójának ellenőrzése
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

A következő lépés tooinstall Google Play-szolgáltatásokra. Google Cloud Messaging rendelkezik néhány minimális API szintre vonatkozó követelményeinek fejlesztéshez és teszteléshez, mely hello **minSdkVersion** tulajdonság hello jegyzékben meg kell felelnie.

Ha egy régebbi eszközzel rendelkező teszteli, tekintse meg [beállítva fel Google Play Services SDK] toodetermine hogyan alacsony értékre állítja, és állítsa be megfelelően.

### <a name="add-google-play-services-toohello-project"></a>Google Play services toohello projekt hozzáadása
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Adja hozzá a kódot
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-hello-app-against-hello-published-mobile-service"></a>Teszt hello alkalmazás hello mobilszolgáltatás közzététele
USB-kábellel Androidos telefonnal közvetlenül csatolásával, vagy egy virtuális eszközzel hello emulátorban hello alkalmazást tesztelheti.

## <a name="next-steps"></a>Következő lépések
Most, hogy ez az oktatóanyag befejezése vegye figyelembe az alábbi oktatóanyagok hello tooone a folytatása:

* [Hitelesítési tooyour Android-alkalmazás hozzáadása](app-service-mobile-android-get-started-users.md).
  Ismerje meg, hogyan tooadd hitelesítési toohello todolist gyors üzembe helyezési projekt támogatott identitásszolgáltató használatával Android rendszeren.
* [Az Android-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése](app-service-mobile-android-get-started-offline-data.md).
  Ismerje meg, hogyan tooadd offline tooyour alkalmazás használatával támogatják a Mobile Apps háttérből. Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- URLs -->
[Android gyors üzembe helyezési]: app-service-mobile-android-get-started.md

[beállítva fel Google Play Services SDK]:https://developers.google.com/android/guides/setup
