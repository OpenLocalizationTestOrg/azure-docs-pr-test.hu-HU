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
# <a name="add-push-notifications-tooyour-xamarinios-app"></a><span data-ttu-id="8f31c-103">Leküldéses értesítések tooyour Xamarin.iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f31c-103">Add push notifications tooyour Xamarin.iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="8f31c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8f31c-104">Overview</span></span>
<span data-ttu-id="8f31c-105">Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Xamarin.iOS gyors üzembe helyezési](app-service-mobile-xamarin-ios-get-started.md) projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="8f31c-105">In this tutorial, you add push notifications toohello [Xamarin.iOS quick start](app-service-mobile-xamarin-ios-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="8f31c-106">Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában.</span><span class="sxs-lookup"><span data-stu-id="8f31c-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="8f31c-107">Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="8f31c-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f31c-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f31c-108">Prerequisites</span></span>
* <span data-ttu-id="8f31c-109">Teljes hello [Xamarin.iOS gyors üzembe helyezés](app-service-mobile-xamarin-ios-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="8f31c-109">Complete hello [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) tutorial.</span></span>
* <span data-ttu-id="8f31c-110">Egy fizikai iOS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="8f31c-110">A physical iOS device.</span></span> <span data-ttu-id="8f31c-111">Leküldéses értesítések nem támogatottak az iOS-szimulátorban hello.</span><span class="sxs-lookup"><span data-stu-id="8f31c-111">Push notifications are not supported by hello iOS simulator.</span></span>

## <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="8f31c-112">Hello alkalmazás az Apple fejlesztői portálján a leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="8f31c-112">Register hello app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-toosend-push-notifications"></a><span data-ttu-id="8f31c-113">A mobilalkalmazás toosend leküldéses értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8f31c-113">Configure your Mobile App toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a><span data-ttu-id="8f31c-114">Frissítési hello server projekt toosend leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="8f31c-114">Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a><span data-ttu-id="8f31c-115">A Xamarin.iOS-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8f31c-115">Configure your Xamarin.iOS project</span></span>
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-tooyour-app"></a><span data-ttu-id="8f31c-116">Leküldéses értesítések tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f31c-116">Add push notifications tooyour app</span></span>
1. <span data-ttu-id="8f31c-117">A **QSTodoService**, adja hozzá a következő tulajdonság hello úgy, hogy **AppDelegate** hello mobil ügyfél szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="8f31c-117">In **QSTodoService**, add hello following property so that **AppDelegate** can acquire hello mobile client:</span></span>
   
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
2. <span data-ttu-id="8f31c-118">Adja hozzá a következő hello `using` utasítás toohello felső részén hello **AppDelegate.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8f31c-118">Add hello following `using` statement toohello top of hello **AppDelegate.cs** file.</span></span>
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. <span data-ttu-id="8f31c-119">A **AppDelegate**, bírálja felül a hello **FinishedLaunching** esemény:</span><span class="sxs-lookup"><span data-stu-id="8f31c-119">In **AppDelegate**, override hello **FinishedLaunching** event:</span></span>
   
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
4. <span data-ttu-id="8f31c-120">A hello ugyanazt a fájlt, bírálja felül a hello **RegisteredForRemoteNotifications** esemény.</span><span class="sxs-lookup"><span data-stu-id="8f31c-120">In hello same file, override hello **RegisteredForRemoteNotifications** event.</span></span> <span data-ttu-id="8f31c-121">Ez a kód egy egyszerű sablon értesítést küldjön el minden támogatott platformon hello kiszolgáló próbál regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="8f31c-121">In this code you are registering for a simple template notification that will be sent across all supported platforms by hello server.</span></span>
   
    <span data-ttu-id="8f31c-122">Sablonok a Notification hubs használatával kapcsolatban lásd: [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="8f31c-122">For more information on templates with Notification Hubs, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>

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


1. <span data-ttu-id="8f31c-123">Ezt követően felülbírálása hello **DidReceivedRemoteNotification** esemény:</span><span class="sxs-lookup"><span data-stu-id="8f31c-123">Then, override hello **DidReceivedRemoteNotification** event:</span></span>
   
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

<span data-ttu-id="8f31c-124">Az alkalmazás már frissített toosupport leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="8f31c-124">Your app is now updated toosupport push notifications.</span></span>

## <span data-ttu-id="8f31c-125"><a name="test"></a>Teszt leküldéses értesítések az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="8f31c-125"><a name="test"></a>Test push notifications in your app</span></span>
1. <span data-ttu-id="8f31c-126">Nyomja le az hello **futtatása** toobuild hello projekt gombra és hello alkalmazás indításához a kompatibilis iOS-eszközt, majd kattintson **OK** tooaccept leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="8f31c-126">Press hello **Run** button toobuild hello project and start hello app in an iOS capable device, then click **OK** tooaccept push notifications.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8f31c-127">Az alkalmazásból explicit módon el kell fogadnia a leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="8f31c-127">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="8f31c-128">A kérelem csak akkor történik meg hello először, hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="8f31c-128">This request only occurs hello first time that hello app runs.</span></span>
   > 
   > 
2. <span data-ttu-id="8f31c-129">Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (**+**) ikonra.</span><span class="sxs-lookup"><span data-stu-id="8f31c-129">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
3. <span data-ttu-id="8f31c-130">Győződjön meg arról, hogy értesítést kap, majd kattintson az **OK** toodismiss hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="8f31c-130">Verify that a notification is received, then click **OK** toodismiss hello notification.</span></span>
4. <span data-ttu-id="8f31c-131">Ismételje meg a lépés 2 és azonnal Bezárás hello alkalmazást, majd ellenőrizze, hogy megjelenik-e értesítést.</span><span class="sxs-lookup"><span data-stu-id="8f31c-131">Repeat step 2 and immediately close hello app, then verify that a notification is shown.</span></span>

<span data-ttu-id="8f31c-132">Ez az oktatóanyag sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8f31c-132">You have successfully completed this tutorial.</span></span>

<!-- Images. -->

<!-- URLs. -->



