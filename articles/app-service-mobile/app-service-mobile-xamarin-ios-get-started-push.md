---
title: "aaaAdd leküldéses értesítések tooyour Xamarin.iOS-alkalmazás az Azure App Service"
description: "További tudnivalók az Azure App Service toosend toouse a leküldéses értesítések tooyour Xamarin.iOS-alkalmazás"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: 3e6439aee4f3fe0f60b9786d0bbfd74c4f5e52d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinios-app"></a>Leküldéses értesítések tooyour Xamarin.iOS-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Xamarin.iOS gyors üzembe helyezési](app-service-mobile-xamarin-ios-get-started.md) projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.

Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában. Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.

## <a name="prerequisites"></a>Előfeltételek
* Teljes hello [Xamarin.iOS gyors üzembe helyezés](app-service-mobile-xamarin-ios-get-started.md) oktatóanyag.
* Egy fizikai iOS-eszközön. Leküldéses értesítések nem támogatottak az iOS-szimulátorban hello.

## <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a>Hello alkalmazás az Apple fejlesztői portálján a leküldéses értesítések regisztrálása
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-toosend-push-notifications"></a>A mobilalkalmazás toosend leküldéses értesítések konfigurálása
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a>Frissítési hello server projekt toosend leküldéses értesítések
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>A Xamarin.iOS-projekt konfigurálása
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-tooyour-app"></a>Leküldéses értesítések tooyour alkalmazás hozzáadása
1. A **QSTodoService**, adja hozzá a következő tulajdonság hello úgy, hogy **AppDelegate** hello mobil ügyfél szerezheti be:
   
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }
2. Adja hozzá a következő hello `using` utasítás toohello felső részén hello **AppDelegate.cs** fájlt.
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. A **AppDelegate**, bírálja felül a hello **FinishedLaunching** esemény:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());
   
            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
   
            return true;
        }
4. A hello ugyanazt a fájlt, bírálja felül a hello **RegisteredForRemoteNotifications** esemény. Ez a kód egy egyszerű sablon értesítést küldjön el minden támogatott platformon hello kiszolgáló próbál regisztrálni.
   
    Sablonok a Notification hubs használatával kapcsolatban lásd: [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


1. Ezt követően felülbírálása hello **DidReceivedRemoteNotification** esemény:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;
   
            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();
   
            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Az alkalmazás már frissített toosupport leküldéses értesítéseket.

## <a name="test"></a>Teszt leküldéses értesítések az alkalmazásban
1. Nyomja le az hello **futtatása** toobuild hello projekt gombra és hello alkalmazás indításához a kompatibilis iOS-eszközt, majd kattintson **OK** tooaccept leküldéses értesítéseket.
   
   > [!NOTE]
   > Az alkalmazásból explicit módon el kell fogadnia a leküldéses értesítések. A kérelem csak akkor történik meg hello először, hello alkalmazást futtat.
   > 
   > 
2. Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (**+**) ikonra.
3. Győződjön meg arról, hogy értesítést kap, majd kattintson az **OK** toodismiss hello értesítést.
4. Ismételje meg a lépés 2 és azonnal Bezárás hello alkalmazást, majd ellenőrizze, hogy megjelenik-e értesítést.

Ez az oktatóanyag sikeresen befejeződött.

<!-- Images. -->

<!-- URLs. -->



