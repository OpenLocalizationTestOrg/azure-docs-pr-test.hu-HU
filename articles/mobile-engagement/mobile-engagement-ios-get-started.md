---
title: "Ismerkedés az Azure Mobile Engagement Objective C nyelven írt iOS-alkalmazásokkal való használatával | Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure Mobile Engagement az iOS-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítések tekintetében."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 117b5742-522b-41de-98c5-f432da2d98f8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 1b87a2ebb35b31ee3d3139ecead6267e62eb1033
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a><span data-ttu-id="86939-103">Ismerkedés az Azure Mobile Engagement Objective C nyelven írt iOS-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="86939-103">Get Started with Azure Mobile Engagement for iOS apps in Objective C</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="86939-104">Ebben a témakörben elsajátíthatja, hogy miként használható az Azure Mobile Engagement az alkalmazás használatának megértéséhez, valamint leküldéses értesítések iOS-alkalmazásba történő küldéséhez a szegmentált felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="86939-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users to an iOS application.</span></span>
<span data-ttu-id="86939-105">Ebben az oktatóanyagban létrehoz egy üres iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="86939-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="86939-106">Az oktatóanyaghoz az alábbiakra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="86939-106">This tutorial requires the following:</span></span>

* <span data-ttu-id="86939-107">XCode 8, amely a MAC App Store áruházából telepíthető</span><span class="sxs-lookup"><span data-stu-id="86939-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="86939-108">a [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="86939-108">the [Mobile Engagement iOS SDK]</span></span>

<span data-ttu-id="86939-109">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Mobile Engagement-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="86939-109">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="86939-110">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="86939-110">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="86939-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="86939-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="86939-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="86939-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span></span>
>
>

## <span data-ttu-id="86939-113"><a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="86939-113"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="86939-114"><a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="86939-114"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="86939-115">Ez az oktatóanyag egy „alapszintű integrációt” mutat be, ami minimálisan szükséges az adatok gyűjtéséhez és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="86939-115">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="86939-116">A teljes integrációs dokumentáció itt található: [Mobile Engagement iOS SDK-integráció](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="86939-116">The complete integration documentation can be found in the [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="86939-117">Létre fogunk hozni egy alapszintű alkalmazást az XCode segítségével az integráció bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="86939-117">We will create a basic app with XCode to demonstrate the integration.</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="86939-118">Új iOS-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="86939-118">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a><span data-ttu-id="86939-119">Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="86939-119">Connect your app to the Mobile Engagement backend</span></span>
1. <span data-ttu-id="86939-120">Töltse le a [Mobile Engagement iOS SDK].</span><span class="sxs-lookup"><span data-stu-id="86939-120">Download the [Mobile Engagement iOS SDK].</span></span>
2. <span data-ttu-id="86939-121">Bontsa ki a .tar.gz fájlt a számítógép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="86939-121">Extract the .tar.gz file to a folder in your computer.</span></span>
3. <span data-ttu-id="86939-122">Kattintson jobb gombbal a projektre, majd válassza az **Add files to** (Fájlok hozzáadása ehhez:) elemet.</span><span class="sxs-lookup"><span data-stu-id="86939-122">Right-click the project, and then select **Add files to**.</span></span>

    ![][1]
4. <span data-ttu-id="86939-123">Lépjen abba a mappába, ahová kibontotta az SDK-t, válassza ki az `EngagementSDK` mappát, kattintson a bal alsó sarokban található **Beállítások** elemre, és ellenőrizze, hogy az **Elemek másolása, ha szükséges** jelölőnégyzet és a célhoz tartozó jelölőnégyzet be van-e jelölve, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="86939-123">Navigate to the folder where you extracted the SDK, select the `EngagementSDK` folder, click **Options** on the bottom left corner and make sure that the checkbox **Copy items if needed** and the checkbox for your target are checked then press **OK**.</span></span>

    ![][2]
5. <span data-ttu-id="86939-124">Nyissa meg a **Build Phases** (Összeállítási fázisok) lapot, majd a **Link Binary With Libraries** (Bináris összekapcsolása könyvtárakkal) menüben adja hozzá a keretrendszereket az alábbiakban láthatók szerint:</span><span class="sxs-lookup"><span data-stu-id="86939-124">Open the **Build Phases** tab, and in the **Link Binary With Libraries** menu, add the frameworks as shown below:</span></span>

    ![][3]
6. <span data-ttu-id="86939-125">Lépjen vissza az Azure Portalra az alkalmazás **Connection Info** (Kapcsolati adatok) lapjáról, és másolja a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="86939-125">Go back to the Azure portal in your app's **Connection Info** page and copy the connection string.</span></span>

    ![][4]
7. <span data-ttu-id="86939-126">Adja hozzá a következő kódsort az **AppDelegate.m** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="86939-126">Add the following line of code in your **AppDelegate.m** file.</span></span>

        #import "EngagementAgent.h"
8. <span data-ttu-id="86939-127">Illessze be a kapcsolati karakterláncot a `didFinishLaunchingWithOptions` delegáltba.</span><span class="sxs-lookup"><span data-stu-id="86939-127">Now paste the connection string in the `didFinishLaunchingWithOptions` delegate.</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. <span data-ttu-id="86939-128">`setTestLogEnabled` egy választható utasítás, amely engedélyezi az SDK-naplókat a problémák azonosításához.</span><span class="sxs-lookup"><span data-stu-id="86939-128">`setTestLogEnabled` is an optional statement which enables SDK logs for you to identify issues.</span></span>

## <span data-ttu-id="86939-129"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="86939-129"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="86939-130">Az adatok küldésének megkezdéséhez és annak biztosításához, hogy a felhasználók aktívak, legalább egy képernyőt (tevékenységet) el kell küldenie a Mobile Engagement háttérrendszere számára.</span><span class="sxs-lookup"><span data-stu-id="86939-130">In order to start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="86939-131">Nyissa meg a **ViewController.h** fájlt, és importálja az **EngagementViewController.h** fájlt:</span><span class="sxs-lookup"><span data-stu-id="86939-131">Open the **ViewController.h** file and import **EngagementViewController.h**:</span></span>

    `#import "EngagementViewController.h"`
2. <span data-ttu-id="86939-132">Cserélje le a **ViewController** felületének főosztályát az `EngagementViewController` fájllal:</span><span class="sxs-lookup"><span data-stu-id="86939-132">Now replace the super class of the **ViewController** interface by `EngagementViewController`:</span></span>

    `@interface ViewController : EngagementViewController`

## <span data-ttu-id="86939-133"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="86939-133"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="86939-134"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="86939-134"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="86939-135">A Mobile Engagement lehetővé teszi a felhasználókkal folytatott interakciót és a felhasználók elérését a kampányok részeként megjelenő leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel.</span><span class="sxs-lookup"><span data-stu-id="86939-135">Mobile Engagement allows you to interact with your users and REACH with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="86939-136">Ez a modul REACH (Elérés) néven érhető el a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="86939-136">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="86939-137">Az alábbi szakaszok állítják be az alkalmazást a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="86939-137">The following sections set up your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a><span data-ttu-id="86939-138">Csendes leküldéses értesítések fogadásának engedélyezése az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="86939-138">Enable your app to receive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a><span data-ttu-id="86939-139">A Reach könyvtár hozzáadása a projekthez</span><span class="sxs-lookup"><span data-stu-id="86939-139">Add the Reach library to your project</span></span>
1. <span data-ttu-id="86939-140">Kattintson jobb gombbal a projektre.</span><span class="sxs-lookup"><span data-stu-id="86939-140">Right-click your project.</span></span>
2. <span data-ttu-id="86939-141">Válassza az **Add file to** (Fájl hozzáadása ehhez:) elemet.</span><span class="sxs-lookup"><span data-stu-id="86939-141">Select **Add file to**.</span></span>
3. <span data-ttu-id="86939-142">Lépjen abba a mappába, amelyben kibontotta az SDK-t.</span><span class="sxs-lookup"><span data-stu-id="86939-142">Navigate to the folder where you extracted the SDK.</span></span>
4. <span data-ttu-id="86939-143">Jelölje ki az `EngagementReach` mappát.</span><span class="sxs-lookup"><span data-stu-id="86939-143">Select the `EngagementReach` folder.</span></span>
5. <span data-ttu-id="86939-144">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="86939-144">Click **Add**.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="86939-145">Az alkalmazás delegáltjának módosítása</span><span class="sxs-lookup"><span data-stu-id="86939-145">Modify your Application Delegate</span></span>
1. <span data-ttu-id="86939-146">Az **AppDeletegate.m** fájlban importálja az Engagement Reach modulját.</span><span class="sxs-lookup"><span data-stu-id="86939-146">Back in **AppDeletegate.m** file, import the Engagement Reach module.</span></span>

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. <span data-ttu-id="86939-147">Az `application:didFinishLaunchingWithOptions` módszerben hozzon létre egy Reach modult, és adja át azt az Engagement meglévő inicializációs sorának:</span><span class="sxs-lookup"><span data-stu-id="86939-147">Inside the `application:didFinishLaunchingWithOptions` method, create a Reach module and pass it to your existing Engagement initialization line:</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a><span data-ttu-id="86939-148">APNS leküldéses értesítések fogadásának engedélyezése az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="86939-148">Enable your app to receive APNS Push Notifications</span></span>
1. <span data-ttu-id="86939-149">Adja a következő sort az `application:didFinishLaunchingWithOptions` módszerhez:</span><span class="sxs-lookup"><span data-stu-id="86939-149">Add the following line to the `application:didFinishLaunchingWithOptions` method:</span></span>

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }
2. <span data-ttu-id="86939-150">Adja hozzá a `application:didRegisterForRemoteNotificationsWithDeviceToken` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="86939-150">Add the `application:didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. <span data-ttu-id="86939-151">Adja hozzá a `didFailToRegisterForRemoteNotificationsWithError` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="86939-151">Add the `didFailToRegisterForRemoteNotificationsWithError` method as follows:</span></span>

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed to get token, error: %@", error);
        }
4. <span data-ttu-id="86939-152">Adja hozzá a `didReceiveRemoteNotification:fetchCompletionHandler` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="86939-152">Add the `didReceiveRemoteNotification:fetchCompletionHandler` method as follows:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
<span data-ttu-id="86939-153">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span><span class="sxs-lookup"><span data-stu-id="86939-153">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
