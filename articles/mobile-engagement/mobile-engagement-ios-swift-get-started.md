---
title: "Ismerkedés az Azure Mobile Engagement Swift nyelven írt iOS-alkalmazásokkal való használatával | Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure Mobile Engagement az iOS-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítések tekintetében."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 196c282d-6f2f-4cbc-aeee-6517c5ad866d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: swift
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: piyushjo
ms.openlocfilehash: 1011b9823333e79a52cd2d187df4f8d063b1f799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a><span data-ttu-id="e4020-103">Ismerkedés az Azure Mobile Engagement Swift nyelven írt iOS-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="e4020-103">Get Started with Azure Mobile Engagement for iOS Apps in Swift</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="e4020-104">Ebben a témakörben elsajátíthatja, hogy miként használható az Azure Mobile Engagement az alkalmazás használatának megértéséhez, valamint leküldéses értesítések iOS-alkalmazásba történő küldéséhez a szegmentált felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="e4020-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users to an iOS application.</span></span>
<span data-ttu-id="e4020-105">Ebben az oktatóanyagban létrehoz egy üres iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="e4020-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="e4020-106">Az oktatóanyaghoz az alábbiakra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="e4020-106">This tutorial requires the following:</span></span>

* <span data-ttu-id="e4020-107">XCode 8, amely a MAC App Store áruházából telepíthető</span><span class="sxs-lookup"><span data-stu-id="e4020-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="e4020-108">a [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="e4020-108">the [Mobile Engagement iOS SDK]</span></span>
* <span data-ttu-id="e4020-109">Leküldéses értesítési tanúsítvány (.p12), amelyet az Apple fejlesztési központjában szerezhet be</span><span class="sxs-lookup"><span data-stu-id="e4020-109">Push notification certificate (.p12) that you can obtain on your Apple Dev Center</span></span>

> [!NOTE]
> <span data-ttu-id="e4020-110">Ez az oktatóanyag a Swift 3.0-s verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="e4020-110">This tutorial uses Swift version 3.0.</span></span> 
> 
> 

<span data-ttu-id="e4020-111">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Mobile Engagement-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="e4020-111">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="e4020-112">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="e4020-112">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="e4020-113">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="e4020-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="e4020-114">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span><span class="sxs-lookup"><span data-stu-id="e4020-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span></span>
> 
> 

## <span data-ttu-id="e4020-115"><a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="e4020-115"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="e4020-116"><a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="e4020-116"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="e4020-117">Ez az oktatóanyag egy „alapszintű integrációt” mutat be, ami minimálisan szükséges az adatok gyűjtéséhez és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="e4020-117">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="e4020-118">A teljes integrációs dokumentáció itt található: [Mobile Engagement iOS SDK-integráció](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="e4020-118">The complete integration documentation can be found in the [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="e4020-119">Létre fogunk hozni egy alapszintű alkalmazást az XCode segítségével az integráció bemutatásához:</span><span class="sxs-lookup"><span data-stu-id="e4020-119">We will create a basic app with XCode to demonstrate the integration:</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="e4020-120">Új iOS-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4020-120">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="e4020-121">Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="e4020-121">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="e4020-122">Töltse le a [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="e4020-122">Download the [Mobile Engagement iOS SDK]</span></span>
2. <span data-ttu-id="e4020-123">Bontsa ki a .tar.gz fájlt a számítógép egyik mappájába</span><span class="sxs-lookup"><span data-stu-id="e4020-123">Extract the .tar.gz file to a folder in your computer</span></span>
3. <span data-ttu-id="e4020-124">Kattintson jobb gombbal a projektre, majd válassza az „Add files to...” (Fájlok hozzáadása ehhez:) elemet</span><span class="sxs-lookup"><span data-stu-id="e4020-124">Right click the project and select "Add files to ..."</span></span>
   
    ![][1]
4. <span data-ttu-id="e4020-125">Lépjen abba a mappába, amelyben kibontotta az SDK-t, jelölje ki az `EngagementSDK` mappát, majd kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="e4020-125">Navigate to the folder where you extracted the SDK and select the `EngagementSDK` folder then press OK.</span></span>
   
    ![][2]
5. <span data-ttu-id="e4020-126">Nyissa meg a `Build Phases` lapot, majd a `Link Binary With Libraries` menüben adja hozzá a keretrendszereket az alábbiakban láthatók szerint:</span><span class="sxs-lookup"><span data-stu-id="e4020-126">Open the `Build Phases` tab and in the `Link Binary With Libraries` menu add the frameworks as shown below:</span></span>
   
    ![][3]
6. <span data-ttu-id="e4020-127">Hozzon létre egy áthidalási fejlécet, hogy használni tudja az SDK Objective C API-jait; ehhez válassza a File (Fájl) > New (Új) > File (Fájl) > iOS > Source (Forrás) > Header File (Fejlécfájl) elemet.</span><span class="sxs-lookup"><span data-stu-id="e4020-127">Create a Bridging header to be able to use the SDK's Objective C APIs by choosing File > New > File > iOS > Source > Header File.</span></span>
   
    ![][4]
7. <span data-ttu-id="e4020-128">Szerkessze az áthidalási fejlécfájlt a Mobile Engagement Objective-C kódjának a Swift-kód számára történő közzétételéhez, ehhez adja hozzá az alábbi importálásokat:</span><span class="sxs-lookup"><span data-stu-id="e4020-128">Edit the bridging header file to expose Mobile Engagement Objective-C code to your Swift code, add the following imports :</span></span>
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. <span data-ttu-id="e4020-129">A Build Settings (Létrehozási beállítások) részen ellenőrizze, hogy a Swift Compiler - Code Generation (Swift fordítóprogram – kódlétrehozás) alatt az Objective-C Bridging Header (Objective-C áthidalási fejléc) létrehozási beállításai erre a fejlécre mutató útvonalat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="e4020-129">Under Build Settings, make sure the Objective-C Bridging Header build setting under Swift Compiler - Code Generation has a path to this header.</span></span> <span data-ttu-id="e4020-130">Példa az útvonalra: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (az útvonaltól függően)**</span><span class="sxs-lookup"><span data-stu-id="e4020-130">Here is a path example: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (depending on the path)**</span></span>
   
   ![][6]
9. <span data-ttu-id="e4020-131">Lépjen vissza az Azure Portalra az alkalmazás *Connection Info* (Kapcsolati adatok) lapjáról, és másolja a kapcsolati karakterláncot</span><span class="sxs-lookup"><span data-stu-id="e4020-131">Go back to the Azure portal in your app's *Connection Info* page and copy the Connection String</span></span>
   
   ![][5]
10. <span data-ttu-id="e4020-132">Illessze be a kapcsolati karakterláncot a `didFinishLaunchingWithOptions` delegáltba</span><span class="sxs-lookup"><span data-stu-id="e4020-132">Now paste the connection string in the `didFinishLaunchingWithOptions` delegate</span></span>
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <span data-ttu-id="e4020-133"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e4020-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="e4020-134">Az adatok küldésének megkezdéséhez és annak biztosításához, hogy a felhasználók aktívak, legalább egy képernyőt (tevékenységet) el kell küldenie a Mobile Engagement háttérrendszere számára.</span><span class="sxs-lookup"><span data-stu-id="e4020-134">In order to start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="e4020-135">Nyissa meg a **ViewController.swift** fájlt, és cserélje le a **ViewController** alapszintű osztályát az **EngagementViewController** osztályra:</span><span class="sxs-lookup"><span data-stu-id="e4020-135">Open the **ViewController.swift** file and replace the base class of **ViewController** to be **EngagementViewController**:</span></span>
   
    `class ViewController : EngagementViewController {`

## <span data-ttu-id="e4020-136"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="e4020-136"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="e4020-137"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e4020-137"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="e4020-138">A Mobile Engagement lehetővé teszi a felhasználókkal folytatott interakciót és a felhasználók elérését a kampányok részeként megjelenő leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel.</span><span class="sxs-lookup"><span data-stu-id="e4020-138">Mobile Engagement allows you to interact and REACH with your users with Push Notifications and In-app Messaging in the context of campaigns.</span></span> <span data-ttu-id="e4020-139">Ez a modul REACH (Elérés) néven érhető el a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="e4020-139">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="e4020-140">Az alábbi szakaszok állítják be az alkalmazást a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="e4020-140">The following sections will setup your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a><span data-ttu-id="e4020-141">Csendes leküldéses értesítések fogadásának engedélyezése az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="e4020-141">Enable your app to receive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a><span data-ttu-id="e4020-142">A Reach könyvtár hozzáadása a projekthez</span><span class="sxs-lookup"><span data-stu-id="e4020-142">Add the Reach library to your project</span></span>
1. <span data-ttu-id="e4020-143">Kattintson a jobb gombbal a projektre</span><span class="sxs-lookup"><span data-stu-id="e4020-143">Right click your project</span></span>
2. <span data-ttu-id="e4020-144">A következők szerint válasszon: `Add file to ...`</span><span class="sxs-lookup"><span data-stu-id="e4020-144">Select `Add file to ...`</span></span>
3. <span data-ttu-id="e4020-145">Lépjen abba a mappába, amelyben kibontotta az SDK-t</span><span class="sxs-lookup"><span data-stu-id="e4020-145">Navigate to the folder where you extracted the SDK</span></span>
4. <span data-ttu-id="e4020-146">Jelölje ki az `EngagementReach` mappát</span><span class="sxs-lookup"><span data-stu-id="e4020-146">Select the `EngagementReach` folder</span></span>
5. <span data-ttu-id="e4020-147">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="e4020-147">Click Add</span></span>
6. <span data-ttu-id="e4020-148">Szerkessze az áthidalási fejlécfájlt a Mobile Engagement Objective-C Reach fejléceinek közzétételéhez, és adja hozzá az alábbi importálásokat:</span><span class="sxs-lookup"><span data-stu-id="e4020-148">Edit the bridging header file to expose Mobile Engagement Objective-C Reach headers and add the following imports :</span></span>
   
        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a><span data-ttu-id="e4020-149">Az alkalmazás delegáltjának módosítása</span><span class="sxs-lookup"><span data-stu-id="e4020-149">Modify your Application Delegate</span></span>
1. <span data-ttu-id="e4020-150">A `didFinishLaunchingWithOptions` módszerben hozzon létre egy Reach modult, és adja át azt az Engagement meglévő inicializációs sorának:</span><span class="sxs-lookup"><span data-stu-id="e4020-150">Inside  the `didFinishLaunchingWithOptions` -  create a reach module and pass it to your existing Engagement initialization line:</span></span>
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a><span data-ttu-id="e4020-151">APNS leküldéses értesítések fogadásának engedélyezése az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="e4020-151">Enable your app to receive APNS Push Notifications</span></span>
1. <span data-ttu-id="e4020-152">Adja a következő sort az `didFinishLaunchingWithOptions` módszerhez:</span><span class="sxs-lookup"><span data-stu-id="e4020-152">Add the following line to the `didFinishLaunchingWithOptions` method:</span></span>
   
        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }
2. <span data-ttu-id="e4020-153">Adja hozzá a `didRegisterForRemoteNotificationsWithDeviceToken` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e4020-153">Add the `didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. <span data-ttu-id="e4020-154">Adja hozzá a `didReceiveRemoteNotification:fetchCompletionHandler:` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e4020-154">Add the `didReceiveRemoteNotification:fetchCompletionHandler:` method as follows:</span></span>
   
        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
<span data-ttu-id="e4020-155">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span><span class="sxs-lookup"><span data-stu-id="e4020-155">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
