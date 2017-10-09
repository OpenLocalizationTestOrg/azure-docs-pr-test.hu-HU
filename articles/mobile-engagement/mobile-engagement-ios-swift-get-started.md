---
title: "aaaGet elindítva az Azure Mobile Engagement Swift nyelven írt IOS |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések IOS-alkalmazásokkal."
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
ms.openlocfilehash: 9a3841d305745f8b80c6b0c86aabe18e0c7c0e59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a><span data-ttu-id="f0485-103">Ismerkedés az Azure Mobile Engagement Swift nyelven írt iOS-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="f0485-103">Get Started with Azure Mobile Engagement for iOS Apps in Swift</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="f0485-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók tooan iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f0485-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users tooan iOS application.</span></span>
<span data-ttu-id="f0485-105">Ebben az oktatóanyagban létrehoz egy üres iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="f0485-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="f0485-106">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="f0485-106">This tutorial requires hello following:</span></span>

* <span data-ttu-id="f0485-107">XCode 8, amely a MAC App Store áruházából telepíthető</span><span class="sxs-lookup"><span data-stu-id="f0485-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="f0485-108">Hello [a Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="f0485-108">hello [Mobile Engagement iOS SDK]</span></span>
* <span data-ttu-id="f0485-109">Leküldéses értesítési tanúsítvány (.p12), amelyet az Apple fejlesztési központjában szerezhet be</span><span class="sxs-lookup"><span data-stu-id="f0485-109">Push notification certificate (.p12) that you can obtain on your Apple Dev Center</span></span>

> [!NOTE]
> <span data-ttu-id="f0485-110">Ez az oktatóanyag a Swift 3.0-s verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="f0485-110">This tutorial uses Swift version 3.0.</span></span> 
> 
> 

<span data-ttu-id="f0485-111">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Mobile Engagement-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0485-111">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="f0485-112">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="f0485-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="f0485-113">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="f0485-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f0485-114">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span><span class="sxs-lookup"><span data-stu-id="f0485-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span></span>
> 
> 

## <span data-ttu-id="f0485-115"><a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="f0485-115"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="f0485-116"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="f0485-116"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="f0485-117">Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0485-117">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="f0485-118">hello teljes integrációs dokumentáció itt található a hello [a Mobile Engagement iOS SDK-integráció](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f0485-118">hello complete integration documentation can be found in hello [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="f0485-119">Létre fogunk hozni egy alapszintű alkalmazást az XCode toodemonstrate hello integráció:</span><span class="sxs-lookup"><span data-stu-id="f0485-119">We will create a basic app with XCode toodemonstrate hello integration:</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="f0485-120">Új iOS-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0485-120">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="f0485-121">Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="f0485-121">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="f0485-122">Töltse le a hello [a Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="f0485-122">Download hello [Mobile Engagement iOS SDK]</span></span>
2. <span data-ttu-id="f0485-123">Bontsa ki a hello. tar.gz fájlt a számítógép tooa mappájába</span><span class="sxs-lookup"><span data-stu-id="f0485-123">Extract hello .tar.gz file tooa folder in your computer</span></span>
3. <span data-ttu-id="f0485-124">Hello projektben kattintson jobb gombbal, majd jelölje ki "fájlok túl hozzáadása..."</span><span class="sxs-lookup"><span data-stu-id="f0485-124">Right click hello project and select "Add files too..."</span></span>
   
    ![][1]
4. <span data-ttu-id="f0485-125">Keresse meg a toohello mappa, amelyben kibontotta az hello SDK és select hello `EngagementSDK` mappát, majd kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="f0485-125">Navigate toohello folder where you extracted hello SDK and select hello `EngagementSDK` folder then press OK.</span></span>
   
    ![][2]
5. <span data-ttu-id="f0485-126">Nyissa meg hello `Build Phases` lapon és a hello `Link Binary With Libraries` menüben adja hozzá hello keretrendszerek alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="f0485-126">Open hello `Build Phases` tab and in hello `Link Binary With Libraries` menu add hello frameworks as shown below:</span></span>
   
    ![][3]
6. <span data-ttu-id="f0485-127">Hozzon létre egy áthidalási fejlécet toobe képes toouse hello SDK Objective C API-k fájl > Új > Fájl > iOS > Source > Header File.</span><span class="sxs-lookup"><span data-stu-id="f0485-127">Create a Bridging header toobe able toouse hello SDK's Objective C APIs by choosing File > New > File > iOS > Source > Header File.</span></span>
   
    ![][4]
7. <span data-ttu-id="f0485-128">Adatközponthíd-képzés fejléc fájl tooexpose Mobile Engagement Objective-C kód tooyour Swift-kód hello szerkesztése, importálja a következő hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f0485-128">Edit hello bridging header file tooexpose Mobile Engagement Objective-C code tooyour Swift code, add hello following imports :</span></span>
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. <span data-ttu-id="f0485-129">A Build Settings győződjön meg arról Objective-C Bridging fejléc build beállítása a Swift Compiler - Code Generation hello elérési toothis fejléc.</span><span class="sxs-lookup"><span data-stu-id="f0485-129">Under Build Settings, make sure hello Objective-C Bridging Header build setting under Swift Compiler - Code Generation has a path toothis header.</span></span> <span data-ttu-id="f0485-130">Íme egy példa az útvonalra: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (attól függően, hogy hello elérési útja)**</span><span class="sxs-lookup"><span data-stu-id="f0485-130">Here is a path example: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (depending on hello path)**</span></span>
   
   ![][6]
9. <span data-ttu-id="f0485-131">Lépjen vissza az alkalmazás Azure-portálon toohello *Kapcsolatinformáció* lapjáról, és másolja a kapcsolati karakterlánc hello</span><span class="sxs-lookup"><span data-stu-id="f0485-131">Go back toohello Azure portal in your app's *Connection Info* page and copy hello Connection String</span></span>
   
   ![][5]
10. <span data-ttu-id="f0485-132">Most illessze be a hello kapcsolati karakterláncot a hello `didFinishLaunchingWithOptions` delegálása</span><span class="sxs-lookup"><span data-stu-id="f0485-132">Now paste hello connection string in hello `didFinishLaunchingWithOptions` delegate</span></span>
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <span data-ttu-id="f0485-133"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0485-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="f0485-134">Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.</span><span class="sxs-lookup"><span data-stu-id="f0485-134">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="f0485-135">Nyissa meg hello **ViewController.swift** fájlt, és cserélje le az alaposztály alaposztályát hello **ViewController** toobe **EngagementViewController**:</span><span class="sxs-lookup"><span data-stu-id="f0485-135">Open hello **ViewController.swift** file and replace hello base class of **ViewController** toobe **EngagementViewController**:</span></span>
   
    `class ViewController : EngagementViewController {`

## <span data-ttu-id="f0485-136"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="f0485-136"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="f0485-137"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0485-137"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="f0485-138">Mobile Engagement lehetővé teszi toointeract és a felhasználókkal leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel REACH kampányok hello környezetében.</span><span class="sxs-lookup"><span data-stu-id="f0485-138">Mobile Engagement allows you toointeract and REACH with your users with Push Notifications and In-app Messaging in hello context of campaigns.</span></span> <span data-ttu-id="f0485-139">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="f0485-139">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="f0485-140">hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="f0485-140">hello following sections will setup your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="f0485-141">Az alkalmazás tooreceive csendes leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0485-141">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a><span data-ttu-id="f0485-142">Hello Reach könyvtár tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f0485-142">Add hello Reach library tooyour project</span></span>
1. <span data-ttu-id="f0485-143">Kattintson a jobb gombbal a projektre</span><span class="sxs-lookup"><span data-stu-id="f0485-143">Right click your project</span></span>
2. <span data-ttu-id="f0485-144">A következők szerint válasszon: `Add file too...`</span><span class="sxs-lookup"><span data-stu-id="f0485-144">Select `Add file too...`</span></span>
3. <span data-ttu-id="f0485-145">Keresse meg a toohello mappát, amelyikbe kibontotta hello SDK</span><span class="sxs-lookup"><span data-stu-id="f0485-145">Navigate toohello folder where you extracted hello SDK</span></span>
4. <span data-ttu-id="f0485-146">Jelölje be hello `EngagementReach` mappa</span><span class="sxs-lookup"><span data-stu-id="f0485-146">Select hello `EngagementReach` folder</span></span>
5. <span data-ttu-id="f0485-147">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="f0485-147">Click Add</span></span>
6. <span data-ttu-id="f0485-148">Adatközponthíd-képzés fejléc fájl tooexpose Mobile Engagement Objective-C Reach fejlécek hello szerkesztése, és importálja a következő hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f0485-148">Edit hello bridging header file tooexpose Mobile Engagement Objective-C Reach headers and add hello following imports :</span></span>
   
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

### <a name="modify-your-application-delegate"></a><span data-ttu-id="f0485-149">Az alkalmazás delegáltjának módosítása</span><span class="sxs-lookup"><span data-stu-id="f0485-149">Modify your Application Delegate</span></span>
1. <span data-ttu-id="f0485-150">Belső hello `didFinishLaunchingWithOptions` – hozzon létre egy reach modult, és adja át tooyour Engagement meglévő inicializációs sorának:</span><span class="sxs-lookup"><span data-stu-id="f0485-150">Inside  hello `didFinishLaunchingWithOptions` -  create a reach module and pass it tooyour existing Engagement initialization line:</span></span>
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a><span data-ttu-id="f0485-151">Az alkalmazás tooreceive APNS leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0485-151">Enable your app tooreceive APNS Push Notifications</span></span>
1. <span data-ttu-id="f0485-152">Adja hozzá a következő sor toohello hello `didFinishLaunchingWithOptions` módszert:</span><span class="sxs-lookup"><span data-stu-id="f0485-152">Add hello following line toohello `didFinishLaunchingWithOptions` method:</span></span>
   
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
2. <span data-ttu-id="f0485-153">Adja hozzá a hello `didRegisterForRemoteNotificationsWithDeviceToken` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f0485-153">Add hello `didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. <span data-ttu-id="f0485-154">Adja hozzá a hello `didReceiveRemoteNotification:fetchCompletionHandler:` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f0485-154">Add hello `didReceiveRemoteNotification:fetchCompletionHandler:` method as follows:</span></span>
   
        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[a Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
