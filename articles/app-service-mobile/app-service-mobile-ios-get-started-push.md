---
title: "Leküldéses értesítések tooiOS alkalmazást az Azure Mobile Apps aaaAdd"
description: "Ismerje meg, hogyan toouse Azure Mobile Apps toosend leküldéses értesítések tooyour iOS-alkalmazás."
services: app-service\mobile
documentationcenter: ios
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: glenga
ms.openlocfilehash: 11588c56bc8987a71257dbad4fcdebf1b3209b1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-ios-app"></a>Leküldéses értesítések tooyour iOS-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [iOS quick start] projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.

Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában. Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.

Hello [iOS-szimulátorban nem támogatja a leküldéses értesítések](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Egy fizikai iOS-eszközön van szüksége, és egy [Apple Fejlesztőprogrambeli tagság](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Regisztrálja az alkalmazást a leküldéses értesítésekre
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-toosend-push-notifications"></a>Az Azure toosend leküldéses értesítések konfigurálása
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Frissítési háttér toosend leküldéses értesítések
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Leküldéses értesítések tooapp hozzáadása
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Teszt leküldéses értesítések
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Több
* Sablonok segítenek rugalmasságot toosend platformok közötti leküldéses értesítések és honosított leküldéses értesítések. [Hogyan tooUse iOS ügyféloldali kódtára a Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) bemutatja, hogyan tooregister sablonok.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS quick start]: app-service-mobile-ios-get-started.md
