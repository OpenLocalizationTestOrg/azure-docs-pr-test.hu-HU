---
title: "a Mobile Engagement iOS SDK frissítési eljárás aaaAzure |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 72a9e493-3f14-4e52-b6e2-0490fd04b184
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 5a81bcaaec72aec665b3334e6400d520454d56a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="a1c1b-103">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="a1c1b-103">Upgrade procedures</span></span>
<span data-ttu-id="a1c1b-104">Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-104">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="a1c1b-105">Minden egyes új verziójának hello SDK először cserélje le (távolítsa el, és importálja újra az xcode-ban) hello EngagementSDK és EngagementReach mappák.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-105">For each new version of hello SDK you must first replace (remove and re-import in xcode) hello EngagementSDK and EngagementReach folders.</span></span>

## <a name="from-300-too400"></a><span data-ttu-id="a1c1b-106">A 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="a1c1b-106">From 3.0.0 too4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="a1c1b-107">XCode 8</span><span class="sxs-lookup"><span data-stu-id="a1c1b-107">XCode 8</span></span>
<span data-ttu-id="a1c1b-108">XCode 8 megadása kötelező hello SDK 4.0.0 verziójával kezdve.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-108">XCode 8 is mandatory starting from version 4.0.0 of hello SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="a1c1b-109">Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="a1c1b-109">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="a1c1b-110">Nincs egy ismert hiba az előző verzió hello reach-modul iOS 10-eszközök futtatása közben: nincsenek műveletet kiváltó rendszerértesítéseket.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-110">There is a known bug on hello reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="a1c1b-111">toofix ezt követően tooimplement hello elavult API `application:didReceiveRemoteNotification:` az alkalmazás delegálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-111">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="a1c1b-112">**Ez a megoldás nem ajánlott** , ez a viselkedés a jövőbeli (még akkor is kisebb) iOS verzió frissítése is módosítható, mivel az iOS API elavult.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-112">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="a1c1b-113">Amint lehetséges tooXCode 8 kell váltani.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-113">You should switch tooXCode 8 as soon as possible.</span></span>
> 
> 

### <a name="usernotifications-framework"></a><span data-ttu-id="a1c1b-114">UserNotifications keretrendszer</span><span class="sxs-lookup"><span data-stu-id="a1c1b-114">UserNotifications framework</span></span>
<span data-ttu-id="a1c1b-115">Tooadd hello kell `UserNotifications` keretrendszer a Build lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-115">You need tooadd hello `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="a1c1b-116">a project explorer hello nyissa meg a projekt ablaktáblán, majd válassza a hello helyes célobjektum.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-116">in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="a1c1b-117">Ezután nyissa meg a hello **"Összeállítási fázisok"** lapon és a hello **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszer `UserNotifications.framework` -set hello hivatkozásokat`Optional`</span><span class="sxs-lookup"><span data-stu-id="a1c1b-117">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set hello link as `Optional`</span></span>

### <a name="application-push-capability"></a><span data-ttu-id="a1c1b-118">Alkalmazás leküldéses funkció</span><span class="sxs-lookup"><span data-stu-id="a1c1b-118">Application push capability</span></span>
<span data-ttu-id="a1c1b-119">XCode 8 lehetséges, hogy az alkalmazás alaphelyzetbe leküldéses funkció, ellenőrizze még egyszer a hello `capability` a kiválasztott cél fülre.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-119">XCode 8 may reset your app push capability, please double check it in hello `capability` tab of your selected target.</span></span>

### <a name="add-hello-new-ios-10-notification-registration-code"></a><span data-ttu-id="a1c1b-120">Hello új iOS 10 értesítési regisztrációs kód hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a1c1b-120">Add hello new iOS 10 notification registration code</span></span>
<span data-ttu-id="a1c1b-121">hello régebbi kód részlet tooregister hello app toonotifications továbbra is működik, de a által használt API-k elavult IOS 10 futtatása során.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-121">hello older code snippet tooregister hello app toonotifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="a1c1b-122">Importálás hello `User Notification` keretrendszer:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-122">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h> 

<span data-ttu-id="a1c1b-123">Az alkalmazás delegáltjának a `application:didFinishLaunchingWithOptions` metódus cseréje:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-123">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

<span data-ttu-id="a1c1b-124">szerint:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-124">by :</span></span>

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="a1c1b-125">UNUserNotificationCenter delegált ütközések feloldása</span><span class="sxs-lookup"><span data-stu-id="a1c1b-125">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="a1c1b-126">*Ha az alkalmazás és a harmadik féltől származó tárak egyike nem valósítja meg a `UNUserNotificationCenterDelegate` akkor kihagyhatja ezt a részt.*</span><span class="sxs-lookup"><span data-stu-id="a1c1b-126">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="a1c1b-127">A `UNUserNotificationCenter` delegált hello SDK toomonitor hello életciklusa az Engagement értesítések 10 vagy újabb IOS-es eszközökön használják.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-127">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="a1c1b-128">hello SDK rendelkezik saját hello végrehajtásának `UNUserNotificationCenterDelegate` protokoll, de lehet csak az egyik `UNUserNotificationCenter` alkalmazásonként delegálni.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-128">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="a1c1b-129">Bármely más delegált hozzáadott toohello `UNUserNotificationCenter` objektum hello egy bevonási ütközik.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-129">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="a1c1b-130">Ha a hello SDK a vagy bármely más harmadik fél delegált észlel, akkor nem fogja használni a saját megvalósítási toogive, egy alkalommal tooresolve hello ütközések.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-130">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="a1c1b-131">Hogy tooadd hello bevonási programot tooyour saját sorrendben delegált tooresolve hello ütközések.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-131">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="a1c1b-132">Nincsenek két módon tooachieve ez.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-132">There are two ways tooachieve this.</span></span>

<span data-ttu-id="a1c1b-133">Javaslat: 1, a delegált működve egyszerűen meghívja toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-133">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="a1c1b-134">Vagy javaslat 2, hello való öröklődés `AEUserNotificationHandler` osztály</span><span class="sxs-lookup"><span data-stu-id="a1c1b-134">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="a1c1b-135">Azt is meghatározhatja, hogy értesítést az engagement szolgáltatásból, vagy nem úgy, hogy rendelkezik-e a `userInfo` szótár toohello ügynök `isEngagementPushPayload:` metódus osztályban.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-135">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="a1c1b-136">Győződjön meg arról, hogy hello `UNUserNotificationCenter` objektum delegált tooyour delegált belül vagy hello beállítása `application:willFinishLaunchingWithOptions:` vagy hello `application:didFinishLaunchingWithOptions:` az alkalmazás delegáltjának metódusában.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-136">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="a1c1b-137">Például ha megvalósította a fent javaslat 1 hello:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-137">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-too300"></a><span data-ttu-id="a1c1b-138">A 2.0.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="a1c1b-138">From 2.0.0 too3.0.0</span></span>
<span data-ttu-id="a1c1b-139">Támogatás az iOS eldobott 4.X.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-139">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="a1c1b-140">A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 6.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-140">Starting from this version hello deployment target of your application must be at least iOS 6.</span></span>

<span data-ttu-id="a1c1b-141">Ha az alkalmazás Reach használ, hozzá kell adnia `remote-notification` érték toohello `UIBackgroundModes` tömb rendelés tooreceive távoli értesítések az Info.plist fájlban.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-141">If you are using Reach in your application, you must add `remote-notification` value toohello `UIBackgroundModes` array in your Info.plist file in order tooreceive remote notifications.</span></span>

<span data-ttu-id="a1c1b-142">hello metódus `application:didReceiveRemoteNotification:` helyébe toobe kell `application:didReceiveRemoteNotification:fetchCompletionHandler:` a az alkalmazás delegáltjának.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-142">hello method `application:didReceiveRemoteNotification:` needs toobe replaced by `application:didReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate.</span></span>

<span data-ttu-id="a1c1b-143">Elavult "AEPushDelegate.h" illesztőfelület, és át kell tooremove összes hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-143">"AEPushDelegate.h" is deprecated interface and you need tooremove all references.</span></span> <span data-ttu-id="a1c1b-144">Ez magában foglalja eltávolításával `[[EngagementAgent shared] setPushDelegate:self]` és módszerek delegálása a az alkalmazás delegáltjának hello:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-144">This includes removing `[[EngagementAgent shared] setPushDelegate:self]` and hello delegate methods from your application delegate:</span></span>

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-too200"></a><span data-ttu-id="a1c1b-145">A 1.16.0 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="a1c1b-145">From 1.16.0 too2.0.0</span></span>
<span data-ttu-id="a1c1b-146">hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-146">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span>
<span data-ttu-id="a1c1b-147">Ha egy korábbi verziójáról telepít, tekintse át először hello Capptain webhely toomigrate too1.16, majd alkalmazza a következő eljárás hello.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-147">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.16 first then apply hello following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1c1b-148">Capptain és a Mobile Engagement nem hello azonos szolgáltatások és hello alábbi eljárás csak emel ki, hogyan toomigrate hello ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-148">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="a1c1b-149">Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement kiszolgálókról</span><span class="sxs-lookup"><span data-stu-id="a1c1b-149">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

### <a name="agent"></a><span data-ttu-id="a1c1b-150">Ügynök</span><span class="sxs-lookup"><span data-stu-id="a1c1b-150">Agent</span></span>
<span data-ttu-id="a1c1b-151">hello metódus `registerApp:` hello új metódus lett cserélve `init:`.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-151">hello method `registerApp:` has been replaced by hello new method `init:`.</span></span> <span data-ttu-id="a1c1b-152">Az alkalmazás delegáltjának frissíteni kell ennek megfelelően, és használja a kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-152">Your application delegate must be updated accordingly and use connection string:</span></span>

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

<span data-ttu-id="a1c1b-153">SmartAd követési el lett távolítva ugyanúgy lehet tooremove minden példánya SDK `AETrackModule` osztály</span><span class="sxs-lookup"><span data-stu-id="a1c1b-153">SmartAd tracking has been removed from SDK you just have tooremove all instances of `AETrackModule` class</span></span>

### <a name="class-name-changes"></a><span data-ttu-id="a1c1b-154">Osztály neve megváltozik</span><span class="sxs-lookup"><span data-stu-id="a1c1b-154">Class Name Changes</span></span>
<span data-ttu-id="a1c1b-155">Hello rebranding részeként nincsenek osztály/fájlnevek toobe módosítani kell néhány.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-155">As part of hello rebranding, there are couple of class/file names that need toobe changed.</span></span>

<span data-ttu-id="a1c1b-156">Minden osztály előtagként a "CP" átnevezi "AE" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-156">All classes prefixed with "CP" are renamed with "AE" prefix.</span></span>

<span data-ttu-id="a1c1b-157">Példa:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-157">Example:</span></span>

* <span data-ttu-id="a1c1b-158">`CPModule.h`túl a rendszer átnevezi`AEModule.h`.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-158">`CPModule.h` is renamed too`AEModule.h`.</span></span>

<span data-ttu-id="a1c1b-159">Minden osztály előtagként a "Capptain" átnevezi "Engagement" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-159">All classes prefixed with "Capptain" are renamed with "Engagement" prefix.</span></span>

<span data-ttu-id="a1c1b-160">Példák:</span><span class="sxs-lookup"><span data-stu-id="a1c1b-160">Examples:</span></span>

* <span data-ttu-id="a1c1b-161">osztály hello `CapptainAgent` túl a rendszer átnevezi`EngagementAgent`.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-161">hello class `CapptainAgent` is renamed too`EngagementAgent`.</span></span>
* <span data-ttu-id="a1c1b-162">osztály hello `CapptainTableViewController` túl a rendszer átnevezi`EngagementTableViewController`.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-162">hello class `CapptainTableViewController` is renamed too`EngagementTableViewController`.</span></span>
* <span data-ttu-id="a1c1b-163">osztály hello `CapptainUtils` túl a rendszer átnevezi`EngagementUtils`.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-163">hello class `CapptainUtils` is renamed too`EngagementUtils`.</span></span>
* <span data-ttu-id="a1c1b-164">osztály hello `CapptainViewController` túl a rendszer átnevezi`EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="a1c1b-164">hello class `CapptainViewController` is renamed too`EngagementViewController`.</span></span>

