---
title: "a Mobile Engagement iOS SDK – áttekintés aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 38f0da2f84df9c62f8fbca233bfda8b9936fdc0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ios-sdk-for-azure-mobile-engagement"></a><span data-ttu-id="93c8c-103">iOS SDK for Azure Mobile Engagement (iOS SDK és Azure Mobile Engagement)</span><span class="sxs-lookup"><span data-stu-id="93c8c-103">iOS SDK for Azure Mobile Engagement</span></span>
<span data-ttu-id="93c8c-104">Kezdje itt tooget összes hello részletes hogyan toointegrate Azure Mobile Engagement az iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="93c8c-104">Start here tooget all hello details on how toointegrate Azure Mobile Engagement in an iOS App.</span></span> <span data-ttu-id="93c8c-105">Ha azt szeretné, hogy toogive azt egy try először győződjön meg arról, hogy szeretne a [15 perc oktatóanyag](mobile-engagement-ios-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="93c8c-105">If you'd like toogive it a try first, make sure you do our [15 minutes tutorial](mobile-engagement-ios-get-started.md).</span></span>

<span data-ttu-id="93c8c-106">Kattintson a toosee hello [SDK tartalom](mobile-engagement-ios-sdk-content.md)</span><span class="sxs-lookup"><span data-stu-id="93c8c-106">Click toosee hello [SDK Content](mobile-engagement-ios-sdk-content.md)</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="93c8c-107">Integráció eljárások</span><span class="sxs-lookup"><span data-stu-id="93c8c-107">Integration procedures</span></span>
1. <span data-ttu-id="93c8c-108">Kezdje itt: [hogyan toointegrate a Mobile Engagement az iOS-alkalmazás](mobile-engagement-ios-integrate-engagement.md)</span><span class="sxs-lookup"><span data-stu-id="93c8c-108">Start here: [How toointegrate Mobile Engagement in your iOS app](mobile-engagement-ios-integrate-engagement.md)</span></span>
2. <span data-ttu-id="93c8c-109">Az értesítések: [hogyan toointegrate Reach (értesítések) az iOS-alkalmazás](mobile-engagement-ios-integrate-engagement-reach.md)</span><span class="sxs-lookup"><span data-stu-id="93c8c-109">For Notifications: [How toointegrate Reach (Notifications) in your iOS app](mobile-engagement-ios-integrate-engagement-reach.md)</span></span>
3. <span data-ttu-id="93c8c-110">Megvalósítási terv címkét: [hogyan toouse hello speciális a Mobile Engagement az iOS-alkalmazás szerinti címkézését API](mobile-engagement-ios-use-engagement-api.md)</span><span class="sxs-lookup"><span data-stu-id="93c8c-110">Tag plan implementation: [How toouse hello advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md)</span></span>

## <a name="release-notes"></a><span data-ttu-id="93c8c-111">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="93c8c-111">Release notes</span></span>
### <a name="410-07172017"></a><span data-ttu-id="93c8c-112">4.1.0 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="93c8c-112">4.1.0 (07/17/2017)</span></span>
* <span data-ttu-id="93c8c-113">Rögzített jelvények háttérben törölve lett.</span><span class="sxs-lookup"><span data-stu-id="93c8c-113">Fixed badges cleared in background.</span></span>
* <span data-ttu-id="93c8c-114">Rögzített figyelmeztetések a XCode 9 kapcsolatos API-k nem hívja a fő várakozási sorba.</span><span class="sxs-lookup"><span data-stu-id="93c8c-114">Fixed warnings on XCode 9 about APIs not called in main queue.</span></span>
* <span data-ttu-id="93c8c-115">Rögzített memóriavesztés Reach lekérdezéseinél.</span><span class="sxs-lookup"><span data-stu-id="93c8c-115">Fixed a memory leak in Reach polls.</span></span>
* <span data-ttu-id="93c8c-116">Támogatás az iOS eldobott 6.X.</span><span class="sxs-lookup"><span data-stu-id="93c8c-116">Dropped support for iOS 6.X.</span></span> <span data-ttu-id="93c8c-117">A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 7-es.</span><span class="sxs-lookup"><span data-stu-id="93c8c-117">Starting from this version hello deployment target of your application must be at least iOS 7.</span></span>

<span data-ttu-id="93c8c-118">Korábbi verziójához lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-ios-release-notes.md)</span><span class="sxs-lookup"><span data-stu-id="93c8c-118">For earlier version please see hello [complete release notes](mobile-engagement-ios-release-notes.md)</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="93c8c-119">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="93c8c-119">Upgrade procedures</span></span>
<span data-ttu-id="93c8c-120">Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.</span><span class="sxs-lookup"><span data-stu-id="93c8c-120">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="93c8c-121">Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK tekintse meg a különböző verzióiban teljes hello [frissítése eljárások](mobile-engagement-ios-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="93c8c-121">You may have toofollow several procedures if you missed several versions of hello SDK see hello complete [Upgrade Procedures](mobile-engagement-ios-upgrade-procedure.md).</span></span>

<span data-ttu-id="93c8c-122">Minden egyes új verziójának hello SDK először cserélje le (távolítsa el, és importálja újra az xcode-ban) hello EngagementSDK és EngagementReach mappák.</span><span class="sxs-lookup"><span data-stu-id="93c8c-122">For each new version of hello SDK you must first replace (remove and re-import in xcode) hello EngagementSDK and EngagementReach folders.</span></span>

### <a name="from-300-too400"></a><span data-ttu-id="93c8c-123">A 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="93c8c-123">From 3.0.0 too4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="93c8c-124">XCode 8</span><span class="sxs-lookup"><span data-stu-id="93c8c-124">XCode 8</span></span>
<span data-ttu-id="93c8c-125">XCode 8 megadása kötelező hello SDK 4.0.0 verziójával kezdve.</span><span class="sxs-lookup"><span data-stu-id="93c8c-125">XCode 8 is mandatory starting from version 4.0.0 of hello SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="93c8c-126">Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="93c8c-126">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="93c8c-127">Nincs egy ismert hiba az előző verzió hello reach-modul iOS 10-eszközök futtatása közben: nincsenek műveletet kiváltó rendszerértesítéseket.</span><span class="sxs-lookup"><span data-stu-id="93c8c-127">There is a known bug on hello reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="93c8c-128">toofix ezt követően tooimplement hello elavult API `application:didReceiveRemoteNotification:` az alkalmazás delegálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="93c8c-128">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
>
>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="93c8c-129">**Ez a megoldás nem ajánlott** , ez a viselkedés a jövőbeli (még akkor is kisebb) iOS verzió frissítése is módosítható, mivel az iOS API elavult.</span><span class="sxs-lookup"><span data-stu-id="93c8c-129">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="93c8c-130">Amint lehetséges tooXCode 8 kell váltani.</span><span class="sxs-lookup"><span data-stu-id="93c8c-130">You should switch tooXCode 8 as soon as possible.</span></span>
>
>

#### <a name="usernotifications-framework"></a><span data-ttu-id="93c8c-131">UserNotifications keretrendszer</span><span class="sxs-lookup"><span data-stu-id="93c8c-131">UserNotifications framework</span></span>
<span data-ttu-id="93c8c-132">Tooadd hello kell `UserNotifications` keretrendszer a Build lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="93c8c-132">You need tooadd hello `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="93c8c-133">a project explorer hello nyissa meg a projekt ablaktáblán, majd válassza a hello helyes célobjektum.</span><span class="sxs-lookup"><span data-stu-id="93c8c-133">in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="93c8c-134">Ezután nyissa meg a hello **"Összeállítási fázisok"** lapon és a hello **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszer `UserNotifications.framework` -set hello hivatkozásokat`Optional`</span><span class="sxs-lookup"><span data-stu-id="93c8c-134">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set hello link as `Optional`</span></span>

#### <a name="application-push-capability"></a><span data-ttu-id="93c8c-135">Alkalmazás leküldéses funkció</span><span class="sxs-lookup"><span data-stu-id="93c8c-135">Application push capability</span></span>
<span data-ttu-id="93c8c-136">XCode 8 lehetséges, hogy az alkalmazás alaphelyzetbe leküldéses funkció, ellenőrizze még egyszer a hello `capability` a kiválasztott cél fülre.</span><span class="sxs-lookup"><span data-stu-id="93c8c-136">XCode 8 may reset your app push capability, please double check it in hello `capability` tab of your selected target.</span></span>

#### <a name="add-hello-new-ios-10-notification-registration-code"></a><span data-ttu-id="93c8c-137">Hello új iOS 10 értesítési regisztrációs kód hozzáadása</span><span class="sxs-lookup"><span data-stu-id="93c8c-137">Add hello new iOS 10 notification registration code</span></span>
<span data-ttu-id="93c8c-138">hello régebbi kód részlet tooregister hello app toonotifications továbbra is működik, de a által használt API-k elavult IOS 10 futtatása során.</span><span class="sxs-lookup"><span data-stu-id="93c8c-138">hello older code snippet tooregister hello app toonotifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="93c8c-139">Importálás hello `User Notification` keretrendszer:</span><span class="sxs-lookup"><span data-stu-id="93c8c-139">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>

<span data-ttu-id="93c8c-140">Az alkalmazás delegáltjának a `application:didFinishLaunchingWithOptions` metódus cseréje:</span><span class="sxs-lookup"><span data-stu-id="93c8c-140">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

<span data-ttu-id="93c8c-141">szerint:</span><span class="sxs-lookup"><span data-stu-id="93c8c-141">by :</span></span>

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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="93c8c-142">UNUserNotificationCenter delegált ütközések feloldása</span><span class="sxs-lookup"><span data-stu-id="93c8c-142">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="93c8c-143">*Ha az alkalmazás és a harmadik féltől származó tárak egyike nem valósítja meg a `UNUserNotificationCenterDelegate` akkor kihagyhatja ezt a részt.*</span><span class="sxs-lookup"><span data-stu-id="93c8c-143">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="93c8c-144">A `UNUserNotificationCenter` delegált hello SDK toomonitor hello életciklusa az Engagement értesítések 10 vagy újabb IOS-es eszközökön használják.</span><span class="sxs-lookup"><span data-stu-id="93c8c-144">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="93c8c-145">hello SDK rendelkezik saját hello végrehajtásának `UNUserNotificationCenterDelegate` protokoll, de lehet csak az egyik `UNUserNotificationCenter` alkalmazásonként delegálni.</span><span class="sxs-lookup"><span data-stu-id="93c8c-145">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="93c8c-146">Bármely más delegált hozzáadott toohello `UNUserNotificationCenter` objektum hello egy bevonási ütközik.</span><span class="sxs-lookup"><span data-stu-id="93c8c-146">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="93c8c-147">Ha a hello SDK a vagy bármely más harmadik fél delegált észlel, akkor nem fogja használni a saját megvalósítási toogive, egy alkalommal tooresolve hello ütközések.</span><span class="sxs-lookup"><span data-stu-id="93c8c-147">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="93c8c-148">Hogy tooadd hello bevonási programot tooyour saját sorrendben delegált tooresolve hello ütközések.</span><span class="sxs-lookup"><span data-stu-id="93c8c-148">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="93c8c-149">Nincsenek két módon tooachieve ez.</span><span class="sxs-lookup"><span data-stu-id="93c8c-149">There are two ways tooachieve this.</span></span>

<span data-ttu-id="93c8c-150">Javaslat: 1, a delegált működve egyszerűen meghívja toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="93c8c-150">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="93c8c-151">Vagy javaslat 2, hello való öröklődés `AEUserNotificationHandler` osztály</span><span class="sxs-lookup"><span data-stu-id="93c8c-151">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="93c8c-152">Azt is meghatározhatja, hogy értesítést az engagement szolgáltatásból, vagy nem úgy, hogy rendelkezik-e a `userInfo` szótár toohello ügynök `isEngagementPushPayload:` metódus osztályban.</span><span class="sxs-lookup"><span data-stu-id="93c8c-152">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="93c8c-153">Győződjön meg arról, hogy hello `UNUserNotificationCenter` objektum delegált tooyour delegált belül vagy hello beállítása `application:willFinishLaunchingWithOptions:` vagy hello `application:didFinishLaunchingWithOptions:` az alkalmazás delegáltjának metódusában.</span><span class="sxs-lookup"><span data-stu-id="93c8c-153">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="93c8c-154">Például ha megvalósította a fent javaslat 1 hello:</span><span class="sxs-lookup"><span data-stu-id="93c8c-154">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }
