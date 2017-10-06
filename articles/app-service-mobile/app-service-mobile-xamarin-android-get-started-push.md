---
title: "aaaAdd leküldéses értesítések tooyour Xamarin.Android-alkalmazásokhoz |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure App Service és az Azure Notification Hubs toosend leküldéses értesítések tooyour Xamarin.Android-alkalmazás"
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: c93d1d0cae06ab15e3e3e5c4b342808b2cd49113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinandroid-app"></a>Leküldéses értesítések tooyour Xamarin.Android-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Xamarin.Android gyors üzembe helyezési](app-service-mobile-windows-store-dotnet-get-started.md) projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.

Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában. Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hello következő szükséges:

* Aktív Google-fiók. A Google-fiókja, regisztrálhat [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
* [A Google Cloud Messaging Client összetevő](http://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Egy értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Engedélyezze a Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-toosend-push-requests"></a>Konfigurálhatja az Azure toosend leküldéses kérések
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Frissítési hello server projekt toosend leküldéses értesítések
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>A leküldéses értesítések hello ügyfél-projekt konfigurálása
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Leküldéses értesítések kód tooyour alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Teszt leküldéses értesítések az alkalmazásban
Egy virtuális eszközzel hello emulátorban hello alkalmazást tesztelheti. Nincsenek az emulátor futtatásához szükséges további konfigurációs lépéseket.

1. Győződjön meg arról, hogy telepít egy virtuális eszközön, amely rendelkezik a Google API-k hello célként beállítva hello Android virtuális eszközt (AVD) kezelő az alábbiak szerint hibakeresés tooor.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. Google fiók toohello Android-eszköz hozzáadásához kattintson **alkalmazások** > **beállítások** > **fiók hozzáadása**, majd kövesse a hello megjelenő utasításokat.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. Futtassa a hello todolist alkalmazást előtt, és egy új teendő elem beszúrására. Megadott idő hello értesítési területén egy értesítési ikon jelenik meg. Hello értesítési fiók tooview hello teljes szöveges hello értesítés nyithatja meg.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
