---
title: "Az Azure Mobile Engagement iOS SDK áttekintése |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3a03bbd6-bcf8-436c-9775-5a8188629252
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 6acd343782a3ee07750e27ec3022ff81cedfadee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="ios-sdk-for-azure-mobile-engagement"></a><span data-ttu-id="40b0e-103">iOS SDK for Azure Mobile Engagement (iOS SDK és Azure Mobile Engagement)</span><span class="sxs-lookup"><span data-stu-id="40b0e-103">iOS SDK for Azure Mobile Engagement</span></span>
<span data-ttu-id="40b0e-104">A részletek hozzáférhet integrálása az Azure Mobile Engagement az iOS-alkalmazás, kezdje itt.</span><span class="sxs-lookup"><span data-stu-id="40b0e-104">Start here to get all the details on how to integrate Azure Mobile Engagement in an iOS App.</span></span> <span data-ttu-id="40b0e-105">Ha meg szeretné adni az első egy try, tegye meg a [15 perc oktatóanyag](mobile-engagement-ios-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="40b0e-105">If you'd like to give it a try first, make sure you do our [15 minutes tutorial](mobile-engagement-ios-get-started.md).</span></span>

<span data-ttu-id="40b0e-106">Megtekintéséhez kattintson ide a [SDK tartalom](mobile-engagement-ios-sdk-content.md)</span><span class="sxs-lookup"><span data-stu-id="40b0e-106">Click to see the [SDK Content](mobile-engagement-ios-sdk-content.md)</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="40b0e-107">Integráció eljárások</span><span class="sxs-lookup"><span data-stu-id="40b0e-107">Integration procedures</span></span>
1. <span data-ttu-id="40b0e-108">Kezdje itt: [integrálása a Mobile Engagement az iOS-alkalmazás](mobile-engagement-ios-integrate-engagement.md)</span><span class="sxs-lookup"><span data-stu-id="40b0e-108">Start here: [How to integrate Mobile Engagement in your iOS app](mobile-engagement-ios-integrate-engagement.md)</span></span>
2. <span data-ttu-id="40b0e-109">Az értesítések: [Reach (értesítések) integrálása az iOS-alkalmazás](mobile-engagement-ios-integrate-engagement-reach.md)</span><span class="sxs-lookup"><span data-stu-id="40b0e-109">For Notifications: [How to integrate Reach (Notifications) in your iOS app](mobile-engagement-ios-integrate-engagement-reach.md)</span></span>
3. <span data-ttu-id="40b0e-110">Megvalósítási terv címkét: [a speciális a Mobile Engagement az iOS-alkalmazás szerinti címkézését API használata](mobile-engagement-ios-use-engagement-api.md)</span><span class="sxs-lookup"><span data-stu-id="40b0e-110">Tag plan implementation: [How to use the advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md)</span></span>

## <a name="release-notes"></a><span data-ttu-id="40b0e-111">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="40b0e-111">Release notes</span></span>
### <a name="410-07172017"></a><span data-ttu-id="40b0e-112">4.1.0 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="40b0e-112">4.1.0 (07/17/2017)</span></span>
* <span data-ttu-id="40b0e-113">Rögzített jelvények háttérben törölve lett.</span><span class="sxs-lookup"><span data-stu-id="40b0e-113">Fixed badges cleared in background.</span></span>
* <span data-ttu-id="40b0e-114">Rögzített figyelmeztetések a XCode 9 kapcsolatos API-k nem hívja a fő várakozási sorba.</span><span class="sxs-lookup"><span data-stu-id="40b0e-114">Fixed warnings on XCode 9 about APIs not called in main queue.</span></span>
* <span data-ttu-id="40b0e-115">Rögzített memóriavesztés Reach lekérdezéseinél.</span><span class="sxs-lookup"><span data-stu-id="40b0e-115">Fixed a memory leak in Reach polls.</span></span>
* <span data-ttu-id="40b0e-116">Támogatás az iOS eldobott 6.X.</span><span class="sxs-lookup"><span data-stu-id="40b0e-116">Dropped support for iOS 6.X.</span></span> <span data-ttu-id="40b0e-117">A központi telepítés célja az alkalmazás ebben a verzióban kiindulva kell legalább iOS 7-es.</span><span class="sxs-lookup"><span data-stu-id="40b0e-117">Starting from this version the deployment target of your application must be at least iOS 7.</span></span>

<span data-ttu-id="40b0e-118">Korábbi verziójához lásd: a [végezze el a kibocsátási megjegyzések](mobile-engagement-ios-release-notes.md)</span><span class="sxs-lookup"><span data-stu-id="40b0e-118">For earlier version please see the [complete release notes](mobile-engagement-ios-release-notes.md)</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="40b0e-119">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="40b0e-119">Upgrade procedures</span></span>
<span data-ttu-id="40b0e-120">Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy az SDK-val történő frissítése során, vegye figyelembe a következő szempontokat.</span><span class="sxs-lookup"><span data-stu-id="40b0e-120">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="40b0e-121">Kövesse a számos eljárást, ha nem fogadta a teljes SDK lásd: a több verzióját is [frissítése eljárások](mobile-engagement-ios-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="40b0e-121">You may have to follow several procedures if you missed several versions of the SDK see the complete [Upgrade Procedures](mobile-engagement-ios-upgrade-procedure.md).</span></span>

<span data-ttu-id="40b0e-122">Az SDK minden új verziójának először cserélje le (távolítsa el, és importálja újra az xcode-ban) EngagementSDK és EngagementReach mappákat.</span><span class="sxs-lookup"><span data-stu-id="40b0e-122">For each new version of the SDK you must first replace (remove and re-import in xcode) the EngagementSDK and EngagementReach folders.</span></span>

### <a name="from-300-to-400"></a><span data-ttu-id="40b0e-123">A 3.0.0 4.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="40b0e-123">From 3.0.0 to 4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="40b0e-124">XCode 8</span><span class="sxs-lookup"><span data-stu-id="40b0e-124">XCode 8</span></span>
<span data-ttu-id="40b0e-125">XCode 8 megadása kötelező, az SDK 4.0.0 verziójával kezdve.</span><span class="sxs-lookup"><span data-stu-id="40b0e-125">XCode 8 is mandatory starting from version 4.0.0 of the SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="40b0e-126">Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja a [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="40b0e-126">If you really depend on XCode 7 then you may use the [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="40b0e-127">Nincs egy ismert hiba az előző verzió a reach-modul iOS 10-eszközök futtatása közben: nincsenek műveletet kiváltó rendszerértesítéseket.</span><span class="sxs-lookup"><span data-stu-id="40b0e-127">There is a known bug on the reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="40b0e-128">A javítás kell megvalósítani a elavult API `application:didReceiveRemoteNotification:` az alkalmazás delegálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="40b0e-128">To fix this you will have to implement the deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
>
>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="40b0e-129">**Ez a megoldás nem ajánlott** , ez a viselkedés a jövőbeli (még akkor is kisebb) iOS verzió frissítése is módosítható, mivel az iOS API elavult.</span><span class="sxs-lookup"><span data-stu-id="40b0e-129">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="40b0e-130">Át kell váltania XCode 8 lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="40b0e-130">You should switch to XCode 8 as soon as possible.</span></span>
>
>

#### <a name="usernotifications-framework"></a><span data-ttu-id="40b0e-131">UserNotifications keretrendszer</span><span class="sxs-lookup"><span data-stu-id="40b0e-131">UserNotifications framework</span></span>
<span data-ttu-id="40b0e-132">Hozzá kell adnia a `UserNotifications` keretrendszer a Build lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="40b0e-132">You need to add the `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="40b0e-133">a project explorer nyissa meg a projekt ablaktáblán, és jelölje ki a megfelelő cél.</span><span class="sxs-lookup"><span data-stu-id="40b0e-133">in the project explorer, open your project pane and select the correct target.</span></span> <span data-ttu-id="40b0e-134">Ezután nyissa meg a **"Összeállítási fázisok"** lapon és a a **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszer `UserNotifications.framework` -állítsa be a hivatkozásokat`Optional`</span><span class="sxs-lookup"><span data-stu-id="40b0e-134">Then, open the **"Build phases"** tab and in the **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set the link as `Optional`</span></span>

#### <a name="application-push-capability"></a><span data-ttu-id="40b0e-135">Alkalmazás leküldéses funkció</span><span class="sxs-lookup"><span data-stu-id="40b0e-135">Application push capability</span></span>
<span data-ttu-id="40b0e-136">XCode 8 lehetséges, hogy az alkalmazás alaphelyzetbe leküldéses funkció, ellenőrizze még egyszer a `capability` a kiválasztott cél fülre.</span><span class="sxs-lookup"><span data-stu-id="40b0e-136">XCode 8 may reset your app push capability, please double check it in the `capability` tab of your selected target.</span></span>

#### <a name="add-the-new-ios-10-notification-registration-code"></a><span data-ttu-id="40b0e-137">Adja hozzá az új iOS 10 értesítési regisztrációs kódot</span><span class="sxs-lookup"><span data-stu-id="40b0e-137">Add the new iOS 10 notification registration code</span></span>
<span data-ttu-id="40b0e-138">A régebbi kódrészletet értesítések az alkalmazás regisztrálásához továbbra is működik, de iOS 10 futtatott elavult API-kat használ.</span><span class="sxs-lookup"><span data-stu-id="40b0e-138">The older code snippet to register the app to notifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="40b0e-139">Importálás a `User Notification` keretrendszer:</span><span class="sxs-lookup"><span data-stu-id="40b0e-139">Import the `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>

<span data-ttu-id="40b0e-140">Az alkalmazás delegáltjának a `application:didFinishLaunchingWithOptions` metódus cseréje:</span><span class="sxs-lookup"><span data-stu-id="40b0e-140">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

<span data-ttu-id="40b0e-141">szerint:</span><span class="sxs-lookup"><span data-stu-id="40b0e-141">by :</span></span>

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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="40b0e-142">UNUserNotificationCenter delegált ütközések feloldása</span><span class="sxs-lookup"><span data-stu-id="40b0e-142">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="40b0e-143">*Ha az alkalmazás és a harmadik féltől származó tárak egyike nem valósítja meg a `UNUserNotificationCenterDelegate` akkor kihagyhatja ezt a részt.*</span><span class="sxs-lookup"><span data-stu-id="40b0e-143">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="40b0e-144">A `UNUserNotificationCenter` delegált az SDK figyelésére szolgál a életciklusának Engagement értesítések IOS 10 vagy újabb rendszert futtató eszközökön.</span><span class="sxs-lookup"><span data-stu-id="40b0e-144">A `UNUserNotificationCenter` delegate is used by the SDK to monitor the life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="40b0e-145">Az SDK-val rendelkezik saját megvalósítása a `UNUserNotificationCenterDelegate` protokoll, de lehet csak az egyik `UNUserNotificationCenter` alkalmazásonként delegálni.</span><span class="sxs-lookup"><span data-stu-id="40b0e-145">The SDK has its own implementation of the `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="40b0e-146">Bármely más delegált hozzáadni a `UNUserNotificationCenter` objektum ütközik az egyik Engagement.</span><span class="sxs-lookup"><span data-stu-id="40b0e-146">Any other delegate added to the `UNUserNotificationCenter` object will conflict with the Engagement one.</span></span> <span data-ttu-id="40b0e-147">Ha az SDK-t a vagy bármely más harmadik fél delegált észlel, akkor azt nem használja a saját megvalósítási Önnek arra, hogy oldja fel az ütközéseket.</span><span class="sxs-lookup"><span data-stu-id="40b0e-147">If the SDK detects your or any other third party's delegate then it will not use its own implementation to give you a chance to resolve the conflicts.</span></span> <span data-ttu-id="40b0e-148">A bevonási logika hozzáadása a saját delegált ahhoz, hogy oldja fel az ütközéseket fog.</span><span class="sxs-lookup"><span data-stu-id="40b0e-148">You will have to add the Engagement logic to your own delegate in order to resolve the conflicts.</span></span>

<span data-ttu-id="40b0e-149">Ennek eléréséhez két módja van.</span><span class="sxs-lookup"><span data-stu-id="40b0e-149">There are two ways to achieve this.</span></span>

<span data-ttu-id="40b0e-150">Javaslat 1, egyszerűen működve a delegált hívások a SDK-val:</span><span class="sxs-lookup"><span data-stu-id="40b0e-150">Proposal 1, simply by forwarding your delegate calls to the SDK:</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

<span data-ttu-id="40b0e-151">Vagy javaslat 2, való öröklődés a `AEUserNotificationHandler` osztály</span><span class="sxs-lookup"><span data-stu-id="40b0e-151">Or proposal 2, by inheriting from the `AEUserNotificationHandler` class</span></span>

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> <span data-ttu-id="40b0e-152">Azt is meghatározhatja, hogy értesítést az engagement szolgáltatásból, vagy nem úgy, hogy rendelkezik-e a `userInfo` ügynökkel szótár `isEngagementPushPayload:` metódus osztályban.</span><span class="sxs-lookup"><span data-stu-id="40b0e-152">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary to the Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="40b0e-153">Győződjön meg arról, hogy a `UNUserNotificationCenter` objektum delegált belül vagy a meghatalmazott értékre van állítva a `application:willFinishLaunchingWithOptions:` vagy a `application:didFinishLaunchingWithOptions:` az alkalmazás delegáltjának metódusában.</span><span class="sxs-lookup"><span data-stu-id="40b0e-153">Make sure that the `UNUserNotificationCenter` object's delegate is set to your delegate within either the `application:willFinishLaunchingWithOptions:` or the `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="40b0e-154">Például ha megvalósította a fenti 1 javaslat:</span><span class="sxs-lookup"><span data-stu-id="40b0e-154">For instance, if you implemented the above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }
