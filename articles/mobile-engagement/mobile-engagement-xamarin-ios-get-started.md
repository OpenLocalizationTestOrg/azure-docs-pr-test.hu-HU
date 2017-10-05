---
title: "Ismerkedés az Azure Mobile Engagement Xamarin.iOS-alkalmazásokkal való használatával"
description: "Ismerje meg, hogyan használható az Azure Mobile Engagement a Xamarin.iOS-alkalmazásokhoz kapcsolódó elemzésekkel és leküldéses értesítésekkel."
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
ms.openlocfilehash: 9938c3e994acf31244825b1afb347f8c9f90ebe3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a><span data-ttu-id="3a128-103">Ismerkedés az Azure Mobile Engagement Xamarin.iOS-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="3a128-103">Get Started with Azure Mobile Engagement for Xamarin.iOS Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="3a128-104">Ebben a témakörben elsajátíthatja, hogy miként használható az Azure Mobile Engagement az alkalmazáshasználat megértéséhez, valamint leküldéses értesítések Xamarin.iOS-alkalmazásba történő küldéséhez szegmentált felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="3a128-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users in a Xamarin.iOS application.</span></span>
<span data-ttu-id="3a128-105">Ebben az oktatóanyagban létrehoz egy üres Xamarin.iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="3a128-105">In this tutorial, you create a blank Xamarin.iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

> [!NOTE]
> <span data-ttu-id="3a128-106">Az Azure Mobile Engagement szolgáltatást 2018 márciusától megszüntetjük, és jelenleg csak meglévő ügyfelek számára érhető el.</span><span class="sxs-lookup"><span data-stu-id="3a128-106">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="3a128-107">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="3a128-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="3a128-108">Az oktatóanyaghoz az alábbiakra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="3a128-108">This tutorial requires the following:</span></span>

* <span data-ttu-id="3a128-109">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="3a128-109">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="3a128-110">Használhatja a Visual Studiót is a Xamarinhoz, de ez az oktatóanyag a Xamarin Studiót használja.</span><span class="sxs-lookup"><span data-stu-id="3a128-110">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="3a128-111">A telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="3a128-111">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span> 
* [<span data-ttu-id="3a128-112">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="3a128-112">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="3a128-113">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="3a128-113">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="3a128-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="3a128-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3a128-115">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="3a128-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="3a128-116"><a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="3a128-116"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="3a128-117"><a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="3a128-117"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="3a128-118">Ez az oktatóanyag egy „alapszintű integrációt” mutat be, ami minimálisan szükséges az adatok gyűjtéséhez és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a128-118">This tutorial presents a "basic integration" which is the minimal set required to collect data and send a push notification.</span></span>

<span data-ttu-id="3a128-119">Létre fogunk hozni egy alapszintű alkalmazást a Xamarin segítségével az integráció bemutatásához:</span><span class="sxs-lookup"><span data-stu-id="3a128-119">We will create a basic app with Xamarin to demonstrate the integration:</span></span>

### <a name="create-a-new-xamarinios-project"></a><span data-ttu-id="3a128-120">Új Xamarin.iOS-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a128-120">Create a new Xamarin.iOS project</span></span>
1. <span data-ttu-id="3a128-121">Indítsa el a Xamarin Studiót.</span><span class="sxs-lookup"><span data-stu-id="3a128-121">Launch Xamarin Studio.</span></span> <span data-ttu-id="3a128-122">Lépjen a **File** -> **New** -> **Solution** (Fájl > Új > Megoldás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3a128-122">Go to **File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="3a128-123">Válassza a **Single View App** (Egynézetes alkalmazás) lehetőséget, majd ellenőrizze, hogy a választott nyelv a **C#**-e, és kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="3a128-123">Select **Single View App**, make sure the selected language is **C#** and then click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="3a128-124">Töltse ki az **App Name** (Alkalmazás neve) és az **Organization Identifier** (Szervezeti azonosító) mezőt, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="3a128-124">Fill in the **App Name** and the **Organization Identifier** and then click **Next**.</span></span> 
   
    ![][3]
   
   > [!IMPORTANT]
   > <span data-ttu-id="3a128-125">Győződjön meg róla, hogy az iOS-alkalmazás majdani üzembe helyezéséhez használandó közzétételi profil olyan alkalmazásazonosítót használ, amely pontosan megegyezik az itt szereplő csomagazonosítóval.</span><span class="sxs-lookup"><span data-stu-id="3a128-125">Make sure that the publishing profile you eventually use to deploy your iOS app is using an App ID which matches exactly with the Bundle Identifier you have here.</span></span> 
   > 
   > 
4. <span data-ttu-id="3a128-126">Frissítse a **Project Name** (Projekt neve), a **Solution Name** (Megoldás neve) és a **Location** (Hely) értékét, ha szükséges, majd kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3a128-126">Update the **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="3a128-127">A Xamarin Studio létrehozza a bemutatóalkalmazást, amelybe integrálni fogjuk a Mobile Engagementet.</span><span class="sxs-lookup"><span data-stu-id="3a128-127">Xamarin Studio will create the demo app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="3a128-128">Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="3a128-128">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="3a128-129">Kattintson a jobb gombbal a **Packages** mappára a Solution (Megoldás) ablakban, és válassza az **Add Packages...** (Csomagok hozzáadása...) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3a128-129">Right click the **Packages** folder in the Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="3a128-130">Keresse meg a **Microsoft Azure Mobile Engagement Xamarin SDK**-t, és adja hozzá a megoldásához.</span><span class="sxs-lookup"><span data-stu-id="3a128-130">Search for the **Microsoft Azure Mobile Engagement Xamarin SDK** and add it to your solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="3a128-131">Nyissa meg az **AppDelegate.cs** fájlt, és adja hozzá a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="3a128-131">Open **AppDelegate.cs** and add the following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
4. <span data-ttu-id="3a128-132">A **FinishedLaunching** metódusban adja hozzá a következőket a Mobile Engagement háttérrendszeréhez való csatlakozás inicializálásához.</span><span class="sxs-lookup"><span data-stu-id="3a128-132">In the **FinishedLaunching** method, add the following to initialize the connection with Mobile Engagement backend.</span></span> <span data-ttu-id="3a128-133">Győződjön meg arról, hogy hozzáadta a **ConnectionString** karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="3a128-133">Make sure to add your **ConnectionString**.</span></span> <span data-ttu-id="3a128-134">Ez a kód egy helyőrző **NotificationIcon** elemet is használ, amelyet a Mobile Engagement SDK ad hozzá, és amelyet érdemes lecserélni.</span><span class="sxs-lookup"><span data-stu-id="3a128-134">This code also uses a dummy **NotificationIcon** which is added by the Mobile Engagement SDK which you may want to replace.</span></span> 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <span data-ttu-id="3a128-135"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3a128-135"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="3a128-136">Az adatok küldésének megkezdéséhez és annak biztosításához, hogy a felhasználók aktívak, legalább egy képernyőt el kell küldenie a Mobile Engagement háttérrendszere számára.</span><span class="sxs-lookup"><span data-stu-id="3a128-136">In order to start sending data and ensuring the users are active, you must send at least one screen to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="3a128-137">Nyissa meg a **ViewController.cs** fájlt, és adja hozzá a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="3a128-137">Open **ViewController.cs** and add the following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
2. <span data-ttu-id="3a128-138">Cserélje le az osztályt, amelytől a `ViewController` örököl, a `UIViewController` osztályról az `EngagementViewController` osztályra.</span><span class="sxs-lookup"><span data-stu-id="3a128-138">Replace the class from which `ViewController` inherits from `UIViewController` to `EngagementViewController`.</span></span> 

## <span data-ttu-id="3a128-139"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="3a128-139"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="3a128-140"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3a128-140"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="3a128-141">A Mobile Engagement lehetővé teszi a felhasználókkal folytatott interakciót és a felhasználók elérését a kampányok részeként megjelenő leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel.</span><span class="sxs-lookup"><span data-stu-id="3a128-141">Mobile Engagement allows you to interact with your users and REACH with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="3a128-142">Ez a modul REACH (Elérés) néven érhető el a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="3a128-142">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="3a128-143">Az alábbi szakaszok állítják be az alkalmazást a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="3a128-143">The following sections set up your app to receive them.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="3a128-144">Az alkalmazás delegáltjának módosítása</span><span class="sxs-lookup"><span data-stu-id="3a128-144">Modify your Application Delegate</span></span>
1. <span data-ttu-id="3a128-145">Nyissa meg az **AppDelegate.cs** fájlt, és adja hozzá a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="3a128-145">Open the **AppDelegate.cs** and add the following using statement:</span></span>
   
        using System; 
2. <span data-ttu-id="3a128-146">Most a `FinishedLaunching` metóduson belül adja hozzá az alábbiakat a leküldéses üzenetek regisztrálásához a következő után: `EngagementAgent.init(...)`</span><span class="sxs-lookup"><span data-stu-id="3a128-146">Now inside the `FinishedLaunching` method, add the following to register for push messages after `EngagementAgent.init(...)`</span></span>
   
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
3. <span data-ttu-id="3a128-147">Végül frissítse vagy vegye fel a következő metódusokat:</span><span class="sxs-lookup"><span data-stu-id="3a128-147">Finally - update or add the following methods:</span></span>
   
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
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }
4. <span data-ttu-id="3a128-148">A megoldás **Info.plist** fájljában ellenőrizze, hogy a **csomagazonosító** megegyezik-e az **alkalmazásazonosítóval**, amely az Apple fejlesztési központján belüli létrehozási profiljában szerepel.</span><span class="sxs-lookup"><span data-stu-id="3a128-148">In your **Info.plist** file in the solution, confirm that the **Bundle Identifier** matches with the **App ID** you have in your provisioning profile in the Apple Dev Center.</span></span> 
   
    ![][7]
5. <span data-ttu-id="3a128-149">Ugyanebben az **Info.plist** fájlban ellenőrizze, hogy kiválasztotta-e az **Enable Background Modes** (Háttérmódok engedélyezése) és a **Remote Notifications** (Távoli értesítések) lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="3a128-149">In the same **Info.plist** file, make sure that you have checked the **Enable Background Modes** and **Remote Notifications**.</span></span> 
   
     ![][8]
6. <span data-ttu-id="3a128-150">Futtassa az alkalmazást azon az eszközön, amelyet ehhez a közzétételi profilhoz társított.</span><span class="sxs-lookup"><span data-stu-id="3a128-150">Run the app on the device you have associated with this publishing profile.</span></span> 

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
