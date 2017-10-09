---
title: "aaaGet elindítva az Azure Mobile Engagement Objective C nyelven írt IOS |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítéseket az iOS-alkalmazásokhoz."
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
ms.openlocfilehash: 51a5013f23d2d04a7b9b30c83b47017898b2bb00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a><span data-ttu-id="0b1e3-103">Ismerkedés az Azure Mobile Engagement Objective C nyelven írt iOS-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="0b1e3-103">Get Started with Azure Mobile Engagement for iOS apps in Objective C</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="0b1e3-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók tooan iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users tooan iOS application.</span></span>
<span data-ttu-id="0b1e3-105">Ebben az oktatóanyagban létrehoz egy üres iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="0b1e3-106">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-106">This tutorial requires hello following:</span></span>

* <span data-ttu-id="0b1e3-107">XCode 8, amely a MAC App Store áruházából telepíthető</span><span class="sxs-lookup"><span data-stu-id="0b1e3-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="0b1e3-108">Hello [a Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="0b1e3-108">hello [Mobile Engagement iOS SDK]</span></span>

<span data-ttu-id="0b1e3-109">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Mobile Engagement-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-109">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="0b1e3-110">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-110">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="0b1e3-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="0b1e3-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="0b1e3-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span></span>
>
>

## <span data-ttu-id="0b1e3-113"><a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="0b1e3-113"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="0b1e3-114"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="0b1e3-114"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="0b1e3-115">Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-115">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="0b1e3-116">hello teljes integrációs dokumentáció itt található a hello [a Mobile Engagement iOS SDK-integráció](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="0b1e3-116">hello complete integration documentation can be found in hello [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="0b1e3-117">Létre fogunk hozni egy alapszintű alkalmazást az XCode toodemonstrate hello integráció.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-117">We will create a basic app with XCode toodemonstrate hello integration.</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="0b1e3-118">Új iOS-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b1e3-118">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="0b1e3-119">Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="0b1e3-119">Connect your app toohello Mobile Engagement backend</span></span>
1. <span data-ttu-id="0b1e3-120">Töltse le a hello [a Mobile Engagement iOS SDK].</span><span class="sxs-lookup"><span data-stu-id="0b1e3-120">Download hello [Mobile Engagement iOS SDK].</span></span>
2. <span data-ttu-id="0b1e3-121">Bontsa ki a hello. tar.gz fájlt a számítógép tooa mappájába.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-121">Extract hello .tar.gz file tooa folder in your computer.</span></span>
3. <span data-ttu-id="0b1e3-122">Kattintson a jobb gombbal a hello projektet, majd válassza ki **fájlok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-122">Right-click hello project, and then select **Add files to**.</span></span>

    ![][1]
4. <span data-ttu-id="0b1e3-123">Keresse meg a toohello mappát, amelyikbe kibontotta hello SDK, válassza ki a hello `EngagementSDK` mappát, kattintson a **beállítások** a hello bal alsó sarokban található, és győződjön meg arról, hogy hello jelölőnégyzet **elemek másolása, szükség esetén** és hello a cél tartozó jelölőnégyzet be van jelölve, majd nyomja le az **OK**.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-123">Navigate toohello folder where you extracted hello SDK, select hello `EngagementSDK` folder, click **Options** on hello bottom left corner and make sure that hello checkbox **Copy items if needed** and hello checkbox for your target are checked then press **OK**.</span></span>

    ![][2]
5. <span data-ttu-id="0b1e3-124">Nyitott hello **Build fázisok** fülre, és a hello **bináris rendelkező Kódtárakon** menü hello keretrendszerek hozzáadása a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-124">Open hello **Build Phases** tab, and in hello **Link Binary With Libraries** menu, add hello frameworks as shown below:</span></span>

    ![][3]
6. <span data-ttu-id="0b1e3-125">Lépjen vissza az alkalmazás Azure-portálon toohello **Kapcsolatinformáció** lapjáról, és másolja hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-125">Go back toohello Azure portal in your app's **Connection Info** page and copy hello connection string.</span></span>

    ![][4]
7. <span data-ttu-id="0b1e3-126">Adja hozzá a következő kódsort hello a **AppDelegate.m** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-126">Add hello following line of code in your **AppDelegate.m** file.</span></span>

        #import "EngagementAgent.h"
8. <span data-ttu-id="0b1e3-127">Most illessze be a hello kapcsolati karakterláncot a hello `didFinishLaunchingWithOptions` delegálni.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-127">Now paste hello connection string in hello `didFinishLaunchingWithOptions` delegate.</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. <span data-ttu-id="0b1e3-128">`setTestLogEnabled`van egy választható utasítás, amely lehetővé teszi, hogy az SDK-naplókat meg tooidentify problémákat.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-128">`setTestLogEnabled` is an optional statement which enables SDK logs for you tooidentify issues.</span></span>

## <span data-ttu-id="0b1e3-129"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0b1e3-129"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="0b1e3-130">Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-130">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="0b1e3-131">Nyissa meg hello **ViewController.h** fájlt, és importálja **EngagementViewController.h**:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-131">Open hello **ViewController.h** file and import **EngagementViewController.h**:</span></span>

    `#import "EngagementViewController.h"`
2. <span data-ttu-id="0b1e3-132">Cserélje le a hello hello szülőosztály **ViewController** felületének főosztályát `EngagementViewController`:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-132">Now replace hello super class of hello **ViewController** interface by `EngagementViewController`:</span></span>

    `@interface ViewController : EngagementViewController`

## <span data-ttu-id="0b1e3-133"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="0b1e3-133"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="0b1e3-134"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0b1e3-134"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="0b1e3-135">Mobile Engagement lehetővé teszi a felhasználókkal toointeract és elérése révén a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-135">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="0b1e3-136">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-136">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="0b1e3-137">hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-137">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="0b1e3-138">Az alkalmazás tooreceive csendes leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0b1e3-138">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a><span data-ttu-id="0b1e3-139">Hello Reach könyvtár tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0b1e3-139">Add hello Reach library tooyour project</span></span>
1. <span data-ttu-id="0b1e3-140">Kattintson jobb gombbal a projektre.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-140">Right-click your project.</span></span>
2. <span data-ttu-id="0b1e3-141">Válassza az **Add file to** (Fájl hozzáadása ehhez:) elemet.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-141">Select **Add file to**.</span></span>
3. <span data-ttu-id="0b1e3-142">Keresse meg a toohello mappát, amelyikbe kibontotta hello SDK.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-142">Navigate toohello folder where you extracted hello SDK.</span></span>
4. <span data-ttu-id="0b1e3-143">Jelölje be hello `EngagementReach` mappát.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-143">Select hello `EngagementReach` folder.</span></span>
5. <span data-ttu-id="0b1e3-144">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-144">Click **Add**.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="0b1e3-145">Az alkalmazás delegáltjának módosítása</span><span class="sxs-lookup"><span data-stu-id="0b1e3-145">Modify your Application Delegate</span></span>
1. <span data-ttu-id="0b1e3-146">Vissza a **AppDeletegate.m** fájlt, importálja hello Engagement Reach modulját.</span><span class="sxs-lookup"><span data-stu-id="0b1e3-146">Back in **AppDeletegate.m** file, import hello Engagement Reach module.</span></span>

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. <span data-ttu-id="0b1e3-147">Belső hello `application:didFinishLaunchingWithOptions` módszer, hozzon létre egy Reach modult, és adja át tooyour Engagement meglévő inicializációs sorának:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-147">Inside hello `application:didFinishLaunchingWithOptions` method, create a Reach module and pass it tooyour existing Engagement initialization line:</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a><span data-ttu-id="0b1e3-148">Az alkalmazás tooreceive APNS leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0b1e3-148">Enable your app tooreceive APNS Push Notifications</span></span>
1. <span data-ttu-id="0b1e3-149">Adja hozzá a következő sor toohello hello `application:didFinishLaunchingWithOptions` módszert:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-149">Add hello following line toohello `application:didFinishLaunchingWithOptions` method:</span></span>

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
2. <span data-ttu-id="0b1e3-150">Adja hozzá a hello `application:didRegisterForRemoteNotificationsWithDeviceToken` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-150">Add hello `application:didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. <span data-ttu-id="0b1e3-151">Adja hozzá a hello `didFailToRegisterForRemoteNotificationsWithError` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-151">Add hello `didFailToRegisterForRemoteNotificationsWithError` method as follows:</span></span>

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed tooget token, error: %@", error);
        }
4. <span data-ttu-id="0b1e3-152">Adja hozzá a hello `didReceiveRemoteNotification:fetchCompletionHandler` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0b1e3-152">Add hello `didReceiveRemoteNotification:fetchCompletionHandler` method as follows:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[a Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
