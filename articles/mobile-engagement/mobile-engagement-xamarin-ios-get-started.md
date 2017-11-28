---
title: "az Azure Mobile Engagement xamarin.IOS elindítva aaaGet"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Xamarin.iOS-alkalmazásokhoz."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0448209e-fff6-47bd-985c-2cf074bac12f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 02340a744753dcc5cd1b6888a5fa87628be47b68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a><span data-ttu-id="47f2c-103">Ismerkedés az Azure Mobile Engagement Xamarin.iOS-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="47f2c-103">Get Started with Azure Mobile Engagement for Xamarin.iOS Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="47f2c-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók Xamarin.iOS-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="47f2c-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users in a Xamarin.iOS application.</span></span>
<span data-ttu-id="47f2c-105">Ebben az oktatóanyagban létrehoz egy üres Xamarin.iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="47f2c-105">In this tutorial, you create a blank Xamarin.iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

> [!NOTE]
> <span data-ttu-id="47f2c-106">hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="47f2c-106">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="47f2c-107">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="47f2c-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="47f2c-108">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="47f2c-108">This tutorial requires hello following:</span></span>

* <span data-ttu-id="47f2c-109">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="47f2c-109">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="47f2c-110">Használhatja a Visual Studiót is a Xamarinhoz, de ez az oktatóanyag a Xamarin Studiót használja.</span><span class="sxs-lookup"><span data-stu-id="47f2c-110">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="47f2c-111">A telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="47f2c-111">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span> 
* [<span data-ttu-id="47f2c-112">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="47f2c-112">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="47f2c-113">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="47f2c-113">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="47f2c-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="47f2c-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="47f2c-115">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="47f2c-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="47f2c-116"><a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="47f2c-116"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="47f2c-117"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="47f2c-117"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="47f2c-118">Ez az oktatóanyag egy "alapszintű integrációt" mutat minimális hello határozza meg a szükséges toocollect dátumát és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="47f2c-118">This tutorial presents a "basic integration" which is hello minimal set required toocollect data and send a push notification.</span></span>

<span data-ttu-id="47f2c-119">Létre fogunk hozni egy alapszintű alkalmazást a Xamarin toodemonstrate hello integrációja:</span><span class="sxs-lookup"><span data-stu-id="47f2c-119">We will create a basic app with Xamarin toodemonstrate hello integration:</span></span>

### <a name="create-a-new-xamarinios-project"></a><span data-ttu-id="47f2c-120">Új Xamarin.iOS-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="47f2c-120">Create a new Xamarin.iOS project</span></span>
1. <span data-ttu-id="47f2c-121">Indítsa el a Xamarin Studiót.</span><span class="sxs-lookup"><span data-stu-id="47f2c-121">Launch Xamarin Studio.</span></span> <span data-ttu-id="47f2c-122">Nyissa meg túl**fájl** -> **új** -> **megoldás**</span><span class="sxs-lookup"><span data-stu-id="47f2c-122">Go too**File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="47f2c-123">Válassza ki **egyetlen alkalmazás megtekintése**, ellenőrizze, hogy a kiválasztott hello nyelv **C#** majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="47f2c-123">Select **Single View App**, make sure hello selected language is **C#** and then click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="47f2c-124">Adja meg a hello **alkalmazásnév** és hello **Organization Identifier** , majd **tovább**.</span><span class="sxs-lookup"><span data-stu-id="47f2c-124">Fill in hello **App Name** and hello **Organization Identifier** and then click **Next**.</span></span> 
   
    ![][3]
   
   > [!IMPORTANT]
   > <span data-ttu-id="47f2c-125">Győződjön meg arról, hogy a közzétételi profil olyan alkalmazásazonosítót használ az iOS-alkalmazás az alkalmazás Azonosítóját, amely megfelel használ pontosan az itt rendelkeznek Bundle Identifier hello toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="47f2c-125">Make sure that hello publishing profile you eventually use toodeploy your iOS app is using an App ID which matches exactly with hello Bundle Identifier you have here.</span></span> 
   > 
   > 
4. <span data-ttu-id="47f2c-126">Frissítés hello **projektnevet**, **megoldás neve** és **hely** Ha szükséges, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="47f2c-126">Update hello **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="47f2c-127">Xamarin Studio létrehozza a hello bemutatóalkalmazást, amelyben integrálni fogjuk a Mobile Engagementet.</span><span class="sxs-lookup"><span data-stu-id="47f2c-127">Xamarin Studio will create hello demo app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="47f2c-128">Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="47f2c-128">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="47f2c-129">Kattintson jobb gombbal a hello **csomagok** megoldás a windows hello, és válassza ki a mappát **csomagok hozzáadása...**</span><span class="sxs-lookup"><span data-stu-id="47f2c-129">Right click hello **Packages** folder in hello Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="47f2c-130">Keresse meg a hello **Microsoft Azure Mobile Engagement Xamarin SDK** , és adja hozzá tooyour megoldás.</span><span class="sxs-lookup"><span data-stu-id="47f2c-130">Search for hello **Microsoft Azure Mobile Engagement Xamarin SDK** and add it tooyour solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="47f2c-131">Nyissa meg **AppDelegate.cs** és adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="47f2c-131">Open **AppDelegate.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
4. <span data-ttu-id="47f2c-132">A hello **FinishedLaunching** módszer, adja hozzá a következő tooinitialize hello kapcsolat Mobile Engagement háttérrendszeréhez való hello.</span><span class="sxs-lookup"><span data-stu-id="47f2c-132">In hello **FinishedLaunching** method, add hello following tooinitialize hello connection with Mobile Engagement backend.</span></span> <span data-ttu-id="47f2c-133">Győződjön meg arról, hogy tooadd a **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="47f2c-133">Make sure tooadd your **ConnectionString**.</span></span> <span data-ttu-id="47f2c-134">Ezt a kódot is használ egy helyőrző **NotificationIcon** amelyet hello Mobile Engagement SDK, amelyet érdemes tooreplace.</span><span class="sxs-lookup"><span data-stu-id="47f2c-134">This code also uses a dummy **NotificationIcon** which is added by hello Mobile Engagement SDK which you may want tooreplace.</span></span> 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <span data-ttu-id="47f2c-135"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="47f2c-135"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="47f2c-136">A sorrend toostart adatküldés és biztosítása hello felhasználók aktívak legalább egy képernyőt toohello Mobile Engagement háttérrendszeréhez el kell küldenie.</span><span class="sxs-lookup"><span data-stu-id="47f2c-136">In order toostart sending data and ensuring hello users are active, you must send at least one screen toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="47f2c-137">Nyissa meg **ViewController.cs** és adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="47f2c-137">Open **ViewController.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
2. <span data-ttu-id="47f2c-138">Cserélje le a hello osztály, amelyből `ViewController` örököl `UIViewController` túl`EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="47f2c-138">Replace hello class from which `ViewController` inherits from `UIViewController` too`EngagementViewController`.</span></span> 

## <span data-ttu-id="47f2c-139"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="47f2c-139"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="47f2c-140"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="47f2c-140"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="47f2c-141">Mobile Engagement lehetővé teszi a felhasználókkal toointeract és elérése révén a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok.</span><span class="sxs-lookup"><span data-stu-id="47f2c-141">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="47f2c-142">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="47f2c-142">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="47f2c-143">hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="47f2c-143">hello following sections set up your app tooreceive them.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="47f2c-144">Az alkalmazás delegáltjának módosítása</span><span class="sxs-lookup"><span data-stu-id="47f2c-144">Modify your Application Delegate</span></span>
1. <span data-ttu-id="47f2c-145">Nyissa meg hello **AppDelegate.cs** és adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="47f2c-145">Open hello **AppDelegate.cs** and add hello following using statement:</span></span>
   
        using System; 
2. <span data-ttu-id="47f2c-146">Most belül hello `FinishedLaunching` módszer, adja hozzá a következő leküldéses üzenetek után tooregister hello`EngagementAgent.init(...)`</span><span class="sxs-lookup"><span data-stu-id="47f2c-146">Now inside hello `FinishedLaunching` method, add hello following tooregister for push messages after `EngagementAgent.init(...)`</span></span>
   
        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }
3. <span data-ttu-id="47f2c-147">Végül frissítse vagy vegye fel a következő módszerek hello:</span><span class="sxs-lookup"><span data-stu-id="47f2c-147">Finally - update or add hello following methods:</span></span>
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }
   
        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }
   
        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed tooregister for remote notifications: Error '{0}'", error);
        }
4. <span data-ttu-id="47f2c-148">Az a **Info.plist** hello megoldásban fájlt, győződjön meg arról, hogy hello **Bundle Identifier** megegyezik-e hello **Alkalmazásazonosító** a létesítési profilban az Apple fejlesztői hello rendelkezik Center.</span><span class="sxs-lookup"><span data-stu-id="47f2c-148">In your **Info.plist** file in hello solution, confirm that hello **Bundle Identifier** matches with hello **App ID** you have in your provisioning profile in hello Apple Dev Center.</span></span> 
   
    ![][7]
5. <span data-ttu-id="47f2c-149">Az azonos hello **Info.plist** fájlt, győződjön meg arról, hogy be van jelölve hello **Háttérmódok engedélyezése** és **távoli értesítések**.</span><span class="sxs-lookup"><span data-stu-id="47f2c-149">In hello same **Info.plist** file, make sure that you have checked hello **Enable Background Modes** and **Remote Notifications**.</span></span> 
   
     ![][8]
6. <span data-ttu-id="47f2c-150">Futtassa a hello alkalmazást ehhez a közzétételi profilhoz társított hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="47f2c-150">Run hello app on hello device you have associated with this publishing profile.</span></span> 

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
