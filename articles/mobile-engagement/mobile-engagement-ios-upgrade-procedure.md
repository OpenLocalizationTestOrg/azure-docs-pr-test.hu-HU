---
title: "Az Azure Mobile Engagement iOS SDK frissítési eljárás |} Microsoft Docs"
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
ms.openlocfilehash: 37c7f133d079186f828d58cabce0d2a259efd085
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="9bc3d-103">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="9bc3d-103">Upgrade procedures</span></span>
<span data-ttu-id="9bc3d-104">Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy az SDK-val történő frissítése során, vegye figyelembe a következő szempontokat.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-104">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="9bc3d-105">Az SDK minden új verziójának először cserélje le (távolítsa el, és importálja újra az xcode-ban) EngagementSDK és EngagementReach mappákat.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-105">For each new version of the SDK you must first replace (remove and re-import in xcode) the EngagementSDK and EngagementReach folders.</span></span>

## <a name="from-300-to-400"></a><span data-ttu-id="9bc3d-106">A 3.0.0 4.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="9bc3d-106">From 3.0.0 to 4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="9bc3d-107">XCode 8</span><span class="sxs-lookup"><span data-stu-id="9bc3d-107">XCode 8</span></span>
<span data-ttu-id="9bc3d-108">XCode 8 megadása kötelező, az SDK 4.0.0 verziójával kezdve.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-108">XCode 8 is mandatory starting from version 4.0.0 of the SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="9bc3d-109">Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja a [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="9bc3d-109">If you really depend on XCode 7 then you may use the [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="9bc3d-110">Nincs egy ismert hiba az előző verzió a reach-modul iOS 10-eszközök futtatása közben: nincsenek műveletet kiváltó rendszerértesítéseket.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-110">There is a known bug on the reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="9bc3d-111">A javítás kell megvalósítani a elavult API `application:didReceiveRemoteNotification:` az alkalmazás delegálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-111">To fix this you will have to implement the deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="9bc3d-112">**Ez a megoldás nem ajánlott** , ez a viselkedés a jövőbeli (még akkor is kisebb) iOS verzió frissítése is módosítható, mivel az iOS API elavult.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-112">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="9bc3d-113">Át kell váltania XCode 8 lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-113">You should switch to XCode 8 as soon as possible.</span></span>
> 
> 

### <a name="usernotifications-framework"></a><span data-ttu-id="9bc3d-114">UserNotifications keretrendszer</span><span class="sxs-lookup"><span data-stu-id="9bc3d-114">UserNotifications framework</span></span>
<span data-ttu-id="9bc3d-115">Hozzá kell adnia a `UserNotifications` keretrendszer a Build lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-115">You need to add the `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="9bc3d-116">a project explorer nyissa meg a projekt ablaktáblán, és jelölje ki a megfelelő cél.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-116">in the project explorer, open your project pane and select the correct target.</span></span> <span data-ttu-id="9bc3d-117">Ezután nyissa meg a **"Összeállítási fázisok"** lapon és a a **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszer `UserNotifications.framework` -állítsa be a hivatkozásokat`Optional`</span><span class="sxs-lookup"><span data-stu-id="9bc3d-117">Then, open the **"Build phases"** tab and in the **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set the link as `Optional`</span></span>

### <a name="application-push-capability"></a><span data-ttu-id="9bc3d-118">Alkalmazás leküldéses funkció</span><span class="sxs-lookup"><span data-stu-id="9bc3d-118">Application push capability</span></span>
<span data-ttu-id="9bc3d-119">XCode 8 lehetséges, hogy az alkalmazás alaphelyzetbe leküldéses funkció, ellenőrizze még egyszer a `capability` a kiválasztott cél fülre.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-119">XCode 8 may reset your app push capability, please double check it in the `capability` tab of your selected target.</span></span>

### <a name="add-the-new-ios-10-notification-registration-code"></a><span data-ttu-id="9bc3d-120">Adja hozzá az új iOS 10 értesítési regisztrációs kódot</span><span class="sxs-lookup"><span data-stu-id="9bc3d-120">Add the new iOS 10 notification registration code</span></span>
<span data-ttu-id="9bc3d-121">A régebbi kódrészletet értesítések az alkalmazás regisztrálásához továbbra is működik, de iOS 10 futtatott elavult API-kat használ.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-121">The older code snippet to register the app to notifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="9bc3d-122">Importálás a `User Notification` keretrendszer:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-122">Import the `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h> 

<span data-ttu-id="9bc3d-123">Az alkalmazás delegáltjának a `application:didFinishLaunchingWithOptions` metódus cseréje:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-123">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

<span data-ttu-id="9bc3d-124">szerint:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-124">by :</span></span>

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="9bc3d-125">UNUserNotificationCenter delegált ütközések feloldása</span><span class="sxs-lookup"><span data-stu-id="9bc3d-125">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="9bc3d-126">*Ha az alkalmazás és a harmadik féltől származó tárak egyike nem valósítja meg a `UNUserNotificationCenterDelegate` akkor kihagyhatja ezt a részt.*</span><span class="sxs-lookup"><span data-stu-id="9bc3d-126">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="9bc3d-127">A `UNUserNotificationCenter` delegált az SDK figyelésére szolgál a életciklusának Engagement értesítések IOS 10 vagy újabb rendszert futtató eszközökön.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-127">A `UNUserNotificationCenter` delegate is used by the SDK to monitor the life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="9bc3d-128">Az SDK-val rendelkezik saját megvalósítása a `UNUserNotificationCenterDelegate` protokoll, de lehet csak az egyik `UNUserNotificationCenter` alkalmazásonként delegálni.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-128">The SDK has its own implementation of the `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="9bc3d-129">Bármely más delegált hozzáadni a `UNUserNotificationCenter` objektum ütközik az egyik Engagement.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-129">Any other delegate added to the `UNUserNotificationCenter` object will conflict with the Engagement one.</span></span> <span data-ttu-id="9bc3d-130">Ha az SDK-t a vagy bármely más harmadik fél delegált észlel, akkor azt nem használja a saját megvalósítási Önnek arra, hogy oldja fel az ütközéseket.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-130">If the SDK detects your or any other third party's delegate then it will not use its own implementation to give you a chance to resolve the conflicts.</span></span> <span data-ttu-id="9bc3d-131">A bevonási logika hozzáadása a saját delegált ahhoz, hogy oldja fel az ütközéseket fog.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-131">You will have to add the Engagement logic to your own delegate in order to resolve the conflicts.</span></span>

<span data-ttu-id="9bc3d-132">Ennek eléréséhez két módja van.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-132">There are two ways to achieve this.</span></span>

<span data-ttu-id="9bc3d-133">Javaslat 1, egyszerűen működve a delegált hívások a SDK-val:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-133">Proposal 1, simply by forwarding your delegate calls to the SDK:</span></span>

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

<span data-ttu-id="9bc3d-134">Vagy javaslat 2, való öröklődés a `AEUserNotificationHandler` osztály</span><span class="sxs-lookup"><span data-stu-id="9bc3d-134">Or proposal 2, by inheriting from the `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="9bc3d-135">Azt is meghatározhatja, hogy értesítést az engagement szolgáltatásból, vagy nem úgy, hogy rendelkezik-e a `userInfo` ügynökkel szótár `isEngagementPushPayload:` metódus osztályban.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-135">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary to the Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="9bc3d-136">Győződjön meg arról, hogy a `UNUserNotificationCenter` objektum delegált belül vagy a meghatalmazott értékre van állítva a `application:willFinishLaunchingWithOptions:` vagy a `application:didFinishLaunchingWithOptions:` az alkalmazás delegáltjának metódusában.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-136">Make sure that the `UNUserNotificationCenter` object's delegate is set to your delegate within either the `application:willFinishLaunchingWithOptions:` or the `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="9bc3d-137">Például ha megvalósította a fenti 1 javaslat:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-137">For instance, if you implemented the above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-to-300"></a><span data-ttu-id="9bc3d-138">A 2.0.0 3.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="9bc3d-138">From 2.0.0 to 3.0.0</span></span>
<span data-ttu-id="9bc3d-139">Támogatás az iOS eldobott 4.X.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-139">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="9bc3d-140">A központi telepítés célja az alkalmazás ebben a verzióban kiindulva kell legalább iOS 6.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-140">Starting from this version the deployment target of your application must be at least iOS 6.</span></span>

<span data-ttu-id="9bc3d-141">Ha az alkalmazás Reach használ, hozzá kell adnia `remote-notification` egy érték a `UIBackgroundModes` tömb távoli értesítések fogadásához az Info.plist fájlban.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-141">If you are using Reach in your application, you must add `remote-notification` value to the `UIBackgroundModes` array in your Info.plist file in order to receive remote notifications.</span></span>

<span data-ttu-id="9bc3d-142">A metódus `application:didReceiveRemoteNotification:` által le kell cserélni `application:didReceiveRemoteNotification:fetchCompletionHandler:` a az alkalmazás delegáltjának.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-142">The method `application:didReceiveRemoteNotification:` needs to be replaced by `application:didReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate.</span></span>

<span data-ttu-id="9bc3d-143">Elavult "AEPushDelegate.h" illesztőfelület, és át kell távolítson el minden hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-143">"AEPushDelegate.h" is deprecated interface and you need to remove all references.</span></span> <span data-ttu-id="9bc3d-144">Ez magában foglalja eltávolításával `[[EngagementAgent shared] setPushDelegate:self]` és az alkalmazás delegáltjának delegált metódusait:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-144">This includes removing `[[EngagementAgent shared] setPushDelegate:self]` and the delegate methods from your application delegate:</span></span>

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-to-200"></a><span data-ttu-id="9bc3d-145">A 1.16.0 2.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="9bc3d-145">From 1.16.0 to 2.0.0</span></span>
<span data-ttu-id="9bc3d-146">A következő ismerteti, hogyan telepíthetők át az SDK-integráció az alkalmazásba az Azure Mobile Engagement technológiával Capptain SAS által kínált Capptain szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-146">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span>
<span data-ttu-id="9bc3d-147">Ha egy korábbi verziójáról telepít, tekintse meg a Capptain webhely 1.16 először át, akkor alkalmazza az alábbi eljárást.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-147">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.16 first then apply the following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9bc3d-148">Capptain és a Mobile Engagement nem ugyanazok a szolgáltatások és az alábbi eljárás csak emel ki, hogyan telepítheti át az ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-148">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="9bc3d-149">Az SDK-t az alkalmazás áttelepítése rendszer nem telepíti át az adatok a Capptain kiszolgálókról a Mobile Engagement-kiszolgálókon</span><span class="sxs-lookup"><span data-stu-id="9bc3d-149">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

### <a name="agent"></a><span data-ttu-id="9bc3d-150">Ügynök</span><span class="sxs-lookup"><span data-stu-id="9bc3d-150">Agent</span></span>
<span data-ttu-id="9bc3d-151">A metódus `registerApp:` felváltotta az új módszer `init:`.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-151">The method `registerApp:` has been replaced by the new method `init:`.</span></span> <span data-ttu-id="9bc3d-152">Az alkalmazás delegáltjának frissíteni kell ennek megfelelően, és használja a kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-152">Your application delegate must be updated accordingly and use connection string:</span></span>

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

<span data-ttu-id="9bc3d-153">SmartAd követési el lett távolítva csak el kell távolítania az összes példányát SDK `AETrackModule` osztály</span><span class="sxs-lookup"><span data-stu-id="9bc3d-153">SmartAd tracking has been removed from SDK you just have to remove all instances of `AETrackModule` class</span></span>

### <a name="class-name-changes"></a><span data-ttu-id="9bc3d-154">Osztály neve megváltozik</span><span class="sxs-lookup"><span data-stu-id="9bc3d-154">Class Name Changes</span></span>
<span data-ttu-id="9bc3d-155">A rebranding részeként nincsenek osztály/fájlnevek módosítani kell néhány.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-155">As part of the rebranding, there are couple of class/file names that need to be changed.</span></span>

<span data-ttu-id="9bc3d-156">Minden osztály előtagként a "CP" átnevezi "AE" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-156">All classes prefixed with "CP" are renamed with "AE" prefix.</span></span>

<span data-ttu-id="9bc3d-157">Példa:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-157">Example:</span></span>

* <span data-ttu-id="9bc3d-158">`CPModule.h`a rendszer átnevezi `AEModule.h`.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-158">`CPModule.h` is renamed to `AEModule.h`.</span></span>

<span data-ttu-id="9bc3d-159">Minden osztály előtagként a "Capptain" átnevezi "Engagement" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-159">All classes prefixed with "Capptain" are renamed with "Engagement" prefix.</span></span>

<span data-ttu-id="9bc3d-160">Példák:</span><span class="sxs-lookup"><span data-stu-id="9bc3d-160">Examples:</span></span>

* <span data-ttu-id="9bc3d-161">Az osztály `CapptainAgent` neve módosult az `EngagementAgent`.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-161">The class `CapptainAgent` is renamed to `EngagementAgent`.</span></span>
* <span data-ttu-id="9bc3d-162">Az osztály `CapptainTableViewController` neve módosult az `EngagementTableViewController`.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-162">The class `CapptainTableViewController` is renamed to `EngagementTableViewController`.</span></span>
* <span data-ttu-id="9bc3d-163">Az osztály `CapptainUtils` neve módosult az `EngagementUtils`.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-163">The class `CapptainUtils` is renamed to `EngagementUtils`.</span></span>
* <span data-ttu-id="9bc3d-164">Az osztály `CapptainViewController` neve módosult az `EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="9bc3d-164">The class `CapptainViewController` is renamed to `EngagementViewController`.</span></span>

