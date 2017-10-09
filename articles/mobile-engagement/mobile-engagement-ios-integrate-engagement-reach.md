---
title: "a Mobile Engagement iOS SDK-integráció elérni aaaAzure |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 40c9bfbdb475ab0b97bdbc9cea798a59cb8a71ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-ios"></a><span data-ttu-id="40e14-103">Hogyan tooIntegrate Engagement Reach IOS rendszerű eszközökön</span><span class="sxs-lookup"><span data-stu-id="40e14-103">How tooIntegrate Engagement Reach on iOS</span></span>
<span data-ttu-id="40e14-104">Hello ismertetett hello integrációs eljárást kell követni, [hogyan tooIntegrate Engagement iOS dokumentumon](mobile-engagement-ios-integrate-engagement.md) Ez az útmutató követése előtt.</span><span class="sxs-lookup"><span data-stu-id="40e14-104">You must follow hello integration procedure described in hello [How tooIntegrate Engagement on iOS document](mobile-engagement-ios-integrate-engagement.md) before following this guide.</span></span>

<span data-ttu-id="40e14-105">Ebben a dokumentációban XCode 8 igényel.</span><span class="sxs-lookup"><span data-stu-id="40e14-105">This documentation requires XCode 8.</span></span> <span data-ttu-id="40e14-106">Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="40e14-106">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="40e14-107">Nincs egy ismert hiba az előző verzió iOS 10-eszközök futtatása közben: nincsenek műveletet kiváltó rendszerértesítéseket.</span><span class="sxs-lookup"><span data-stu-id="40e14-107">There is a known bug on this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="40e14-108">toofix ezt követően tooimplement hello elavult API `application:didReceiveRemoteNotification:` az alkalmazás delegálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="40e14-108">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="40e14-109">**Ez a megoldás nem ajánlott** , ez a viselkedés a jövőbeli (még akkor is kisebb) iOS verzió frissítése is módosítható, mivel az iOS API elavult.</span><span class="sxs-lookup"><span data-stu-id="40e14-109">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="40e14-110">Amint lehetséges tooXCode 8 kell váltani.</span><span class="sxs-lookup"><span data-stu-id="40e14-110">You should switch tooXCode 8 as soon as possible.</span></span>
>
>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="40e14-111">Az alkalmazás tooreceive csendes leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="40e14-111">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a><span data-ttu-id="40e14-112">Integrációs lépések</span><span class="sxs-lookup"><span data-stu-id="40e14-112">Integration steps</span></span>
### <a name="embed-hello-engagement-reach-sdk-into-your-ios-project"></a><span data-ttu-id="40e14-113">Az iOS-projekthez az Engagement Reach SDK hello beágyazása</span><span class="sxs-lookup"><span data-stu-id="40e14-113">Embed hello Engagement Reach SDK into your iOS project</span></span>
* <span data-ttu-id="40e14-114">Hello Reach sdk hozzáadása az Xcode projektjében.</span><span class="sxs-lookup"><span data-stu-id="40e14-114">Add hello Reach sdk in your Xcode project.</span></span> <span data-ttu-id="40e14-115">Az Xcode-ban lépjen túl**projekt \> tooproject hozzáadása** , és válassza a hello `EngagementReach` mappát.</span><span class="sxs-lookup"><span data-stu-id="40e14-115">In Xcode, go too**Project \> Add tooproject** and choose hello `EngagementReach` folder.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="40e14-116">Az alkalmazás delegáltjának módosítása</span><span class="sxs-lookup"><span data-stu-id="40e14-116">Modify your Application Delegate</span></span>
* <span data-ttu-id="40e14-117">Hello felső a fájlt importálja az Engagement Reach modulját hello:</span><span class="sxs-lookup"><span data-stu-id="40e14-117">At hello top of your implementation file, import hello Engagement Reach module:</span></span>

      [...]
      #import "AEReachModule.h"
* <span data-ttu-id="40e14-118">Módszer belül `applicationDidFinishLaunching:` vagy `application:didFinishLaunchingWithOptions:`, hozzon létre egy reach modult, és adja át tooyour Engagement meglévő inicializációs sorának:</span><span class="sxs-lookup"><span data-stu-id="40e14-118">Inside method `applicationDidFinishLaunching:` or `application:didFinishLaunchingWithOptions:`, create a reach module and pass it tooyour existing Engagement initialization line:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* <span data-ttu-id="40e14-119">Módosítsa **"icon.png"** hello kép neve, amelyet az értesítési ikon karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="40e14-119">Modify **'icon.png'** string with hello image name you want as your notification icon.</span></span>
* <span data-ttu-id="40e14-120">Ha azt szeretné, hogy toouse hello beállítás *frissítés jelvény érték* Reach-kampányokat, vagy ha azt szeretné, toouse natív leküldéses \<SaaS/Reach API/kampány formátum/natív leküldéses\> rendszeren végrehajtott kampány segítségével hello Reach-modul kezelése hello jelvény ikon magát (emellett automatikusan törli a hello alkalmazás jelvény és is alaphelyzetbe állíthatja az Engagement által tárolt minden alkalommal, amikor hello alkalmazás elindítva, vagy foregrounded hello érték).</span><span class="sxs-lookup"><span data-stu-id="40e14-120">If you want toouse hello option *Update badge value* in Reach campaigns or if you want toouse native push \</SaaS/Reach API/Campaign format/Native Push\> campaigns, you must let hello Reach module manage hello badge icon itself (it will automatically clear hello application badge and also reset hello value stored by Engagement every time hello application is started or foregrounded).</span></span> <span data-ttu-id="40e14-121">Ehhez adja hozzá a következő sor Reach-modul inicializálása után hello:</span><span class="sxs-lookup"><span data-stu-id="40e14-121">This is done by adding hello following line after Reach module initialization:</span></span>

      [reach setAutoBadgeEnabled:YES];
* <span data-ttu-id="40e14-122">Ha azt szeretné, hogy toohandle Reach adatleküldés, az alkalmazás delegáltjának felel meg a toohello segítségével `AEReachDataPushDelegate` protokoll.</span><span class="sxs-lookup"><span data-stu-id="40e14-122">If you want toohandle Reach data push, you must let your Application delegate conform toohello `AEReachDataPushDelegate` protocol.</span></span> <span data-ttu-id="40e14-123">Adja hozzá a sor Reach-modul inicializálása után a következő hello:</span><span class="sxs-lookup"><span data-stu-id="40e14-123">Add hello following line after Reach module initialization:</span></span>

      [reach setDataPushDelegate:self];
* <span data-ttu-id="40e14-124">Akkor is létrehozható hello módszerek `onDataPushStringReceived:` és `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` a az alkalmazás delegáltjának:</span><span class="sxs-lookup"><span data-stu-id="40e14-124">Then you can implement hello methods `onDataPushStringReceived:` and `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in your application delegate:</span></span>

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a><span data-ttu-id="40e14-125">Kategória</span><span class="sxs-lookup"><span data-stu-id="40e14-125">Category</span></span>
<span data-ttu-id="40e14-126">hello kategória paraméter nem kötelező, amikor adatokat leküldéses kampány létrehozása, és lehetővé teszi, hogy toofilter adatok leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="40e14-126">hello category parameter is optional when you create a Data Push campaign and allows you toofilter data pushes.</span></span> <span data-ttu-id="40e14-127">Ez akkor hasznos, ha azt szeretné, hogy toopush különböző a `Base64` adatok és a kívánt tooidentify típusuk őket elemzése előtt.</span><span class="sxs-lookup"><span data-stu-id="40e14-127">This is useful if you want toopush different kinds of `Base64` data and want tooidentify their type before parsing them.</span></span>

<span data-ttu-id="40e14-128">**Az alkalmazás most már készen áll a tooreceive és megjelenítési elérni a tartalom van.**</span><span class="sxs-lookup"><span data-stu-id="40e14-128">**Your application is now ready tooreceive and display reach contents!**</span></span>

## <a name="how-tooreceive-announcements-and-polls-at-any-time"></a><span data-ttu-id="40e14-129">Hogyan tooreceive mutató hirdetmények és szavazások bármikor</span><span class="sxs-lookup"><span data-stu-id="40e14-129">How tooreceive announcements and polls at any time</span></span>
<span data-ttu-id="40e14-130">Engagement Reach értesítéseket küldhet tooyour a végfelhasználók bármikor hello Apple Push Notification szolgáltatás használatával.</span><span class="sxs-lookup"><span data-stu-id="40e14-130">Engagement can send Reach notifications tooyour end users at any time by using hello Apple Push Notification Service.</span></span>

<span data-ttu-id="40e14-131">tooenable ezt a funkciót, hogy lesz tooprepare az alkalmazás az Apple leküldéses értesítések és az alkalmazás delegáltjának módosítása.</span><span class="sxs-lookup"><span data-stu-id="40e14-131">tooenable this functionality, you'll have tooprepare your application for Apple push notifications and modify your application delegate.</span></span>

### <a name="prepare-your-application-for-apple-push-notifications"></a><span data-ttu-id="40e14-132">Az Apple leküldéses értesítések az alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="40e14-132">Prepare your application for Apple push notifications</span></span>
<span data-ttu-id="40e14-133">Kövesse a hello Útmutató: [hogyan tooPrepare az alkalmazás az Apple leküldéses értesítésekhez](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span><span class="sxs-lookup"><span data-stu-id="40e14-133">Please follow hello guide : [How tooPrepare your Application for Apple Push Notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

### <a name="add-hello-necessary-client-code"></a><span data-ttu-id="40e14-134">Hello szükséges Ügyfélkód hozzáadása</span><span class="sxs-lookup"><span data-stu-id="40e14-134">Add hello necessary client code</span></span>
<span data-ttu-id="40e14-135">*Az alkalmazás ezen a ponton hello Engagement előtér kell rendelkezniük a regisztrált Apple leküldéses tanúsítvány.*</span><span class="sxs-lookup"><span data-stu-id="40e14-135">*At this point your application should have a registered Apple push certificate in hello Engagement frontend.*</span></span>

<span data-ttu-id="40e14-136">Ha nem már elkészült, akkor tooregister az alkalmazás tooreceive leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="40e14-136">If it's not done already, you need tooregister your application tooreceive push notifications.</span></span>

* <span data-ttu-id="40e14-137">Importálás hello `User Notification` keretrendszer:</span><span class="sxs-lookup"><span data-stu-id="40e14-137">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>
* <span data-ttu-id="40e14-138">Adja hozzá az alkalmazás indításakor a sorban következő hello (általában a `application:didFinishLaunchingWithOptions:`):</span><span class="sxs-lookup"><span data-stu-id="40e14-138">Add hello following line when your application starts (typically in `application:didFinishLaunchingWithOptions:`):</span></span>

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

<span data-ttu-id="40e14-139">Ezt követően tooprovide tooEngagement hello eszköz jogkivonatát az Apple-kiszolgálók által visszaadott van szüksége.</span><span class="sxs-lookup"><span data-stu-id="40e14-139">Then, You need tooprovide tooEngagement hello device token returned by Apple servers.</span></span> <span data-ttu-id="40e14-140">Ezt nevű hello metódust `application:didRegisterForRemoteNotificationsWithDeviceToken:` a az alkalmazás delegáltjának:</span><span class="sxs-lookup"><span data-stu-id="40e14-140">This is done in hello method named `application:didRegisterForRemoteNotificationsWithDeviceToken:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

<span data-ttu-id="40e14-141">Végül tooinform hello Engagement SDK között, ha az alkalmazás távoli kapcsolatos értesítőt kap.</span><span class="sxs-lookup"><span data-stu-id="40e14-141">Finally, you have tooinform hello Engagement SDK when your application receives a remote notification.</span></span> <span data-ttu-id="40e14-142">Ezt követően hívható toodo hello metódus `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` a az alkalmazás delegáltjának:</span><span class="sxs-lookup"><span data-stu-id="40e14-142">toodo that, call hello method `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> <span data-ttu-id="40e14-143">Alapértelmezés szerint az Engagement Reach szabályozza hello completionHandler.</span><span class="sxs-lookup"><span data-stu-id="40e14-143">By default, Engagement Reach controls hello completionHandler.</span></span> <span data-ttu-id="40e14-144">Ha azt szeretné, hogy toomanually válaszoljon toohello `handler` letiltása a kódban, a hello üres átadhatók `handler` argumentum és vezérlés hello befejezési blokkolása magát.</span><span class="sxs-lookup"><span data-stu-id="40e14-144">If you want toomanually respond toohello `handler` block in your code, you can pass nil for hello `handler` argument and control hello completion block yourself.</span></span> <span data-ttu-id="40e14-145">Lásd: hello `UIBackgroundFetchResult` típusát a lehetséges értékek listáját.</span><span class="sxs-lookup"><span data-stu-id="40e14-145">See hello `UIBackgroundFetchResult` type for a list of possible values.</span></span>
>
>

### <a name="full-example"></a><span data-ttu-id="40e14-146">Teljes példa</span><span class="sxs-lookup"><span data-stu-id="40e14-146">Full example</span></span>
<span data-ttu-id="40e14-147">Ez az integráció teljes példa:</span><span class="sxs-lookup"><span data-stu-id="40e14-147">Here is a full example of integration:</span></span>

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="40e14-148">UNUserNotificationCenter delegált ütközések feloldása</span><span class="sxs-lookup"><span data-stu-id="40e14-148">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="40e14-149">*Ha az alkalmazás és a harmadik féltől származó tárak egyike nem valósítja meg a `UNUserNotificationCenterDelegate` akkor kihagyhatja ezt a részt.*</span><span class="sxs-lookup"><span data-stu-id="40e14-149">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="40e14-150">A `UNUserNotificationCenter` delegált hello SDK toomonitor hello életciklusa az Engagement értesítések 10 vagy újabb IOS-es eszközökön használják.</span><span class="sxs-lookup"><span data-stu-id="40e14-150">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="40e14-151">hello SDK rendelkezik saját hello végrehajtásának `UNUserNotificationCenterDelegate` protokoll, de lehet csak az egyik `UNUserNotificationCenter` alkalmazásonként delegálni.</span><span class="sxs-lookup"><span data-stu-id="40e14-151">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="40e14-152">Bármely más delegált hozzáadott toohello `UNUserNotificationCenter` objektum hello egy bevonási ütközik.</span><span class="sxs-lookup"><span data-stu-id="40e14-152">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="40e14-153">Ha a hello SDK a vagy bármely más harmadik fél delegált észlel, akkor nem fogja használni a saját megvalósítási toogive, egy alkalommal tooresolve hello ütközések.</span><span class="sxs-lookup"><span data-stu-id="40e14-153">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="40e14-154">Hogy tooadd hello bevonási programot tooyour saját sorrendben delegált tooresolve hello ütközések.</span><span class="sxs-lookup"><span data-stu-id="40e14-154">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="40e14-155">Nincsenek két módon tooachieve ez.</span><span class="sxs-lookup"><span data-stu-id="40e14-155">There are two ways tooachieve this.</span></span>

<span data-ttu-id="40e14-156">Javaslat: 1, a delegált működve egyszerűen meghívja toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="40e14-156">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="40e14-157">Vagy javaslat 2, hello való öröklődés `AEUserNotificationHandler` osztály</span><span class="sxs-lookup"><span data-stu-id="40e14-157">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="40e14-158">Azt is meghatározhatja, hogy értesítést az engagement szolgáltatásból, vagy nem úgy, hogy rendelkezik-e a `userInfo` szótár toohello ügynök `isEngagementPushPayload:` metódus osztályban.</span><span class="sxs-lookup"><span data-stu-id="40e14-158">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="40e14-159">Győződjön meg arról, hogy hello `UNUserNotificationCenter` objektum delegált tooyour delegált belül vagy hello beállítása `application:willFinishLaunchingWithOptions:` vagy hello `application:didFinishLaunchingWithOptions:` az alkalmazás delegáltjának metódusában.</span><span class="sxs-lookup"><span data-stu-id="40e14-159">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="40e14-160">Például ha megvalósította a fent javaslat 1 hello:</span><span class="sxs-lookup"><span data-stu-id="40e14-160">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-toocustomize-campaigns"></a><span data-ttu-id="40e14-161">Hogyan toocustomize kampányok</span><span class="sxs-lookup"><span data-stu-id="40e14-161">How toocustomize campaigns</span></span>
### <a name="notifications"></a><span data-ttu-id="40e14-162">Értesítések</span><span class="sxs-lookup"><span data-stu-id="40e14-162">Notifications</span></span>
<span data-ttu-id="40e14-163">Az értesítések két típusa van: rendszer és az alkalmazáson belüli értesítések.</span><span class="sxs-lookup"><span data-stu-id="40e14-163">There are two types of notifications: system and in-app notifications.</span></span>

<span data-ttu-id="40e14-164">Rendszer értesítések iOS kezeli, és nem szabható testre.</span><span class="sxs-lookup"><span data-stu-id="40e14-164">System notifications are handled by iOS, and cannot be customized.</span></span>

<span data-ttu-id="40e14-165">Alkalmazásbeli értesítéseket a toohello aktuális alkalmazás ablak dinamikusan hozzáadott nézetek.</span><span class="sxs-lookup"><span data-stu-id="40e14-165">In-app notifications are made of a view that is dynamically added toohello current application window.</span></span> <span data-ttu-id="40e14-166">Ez a lehetőség egy értesítési területre.</span><span class="sxs-lookup"><span data-stu-id="40e14-166">This is called a notification overlay.</span></span> <span data-ttu-id="40e14-167">Értesítési átfedések rendkívül gyors integrációs, mivel azok nem szükséges, toomodify az alkalmazás nézeten.</span><span class="sxs-lookup"><span data-stu-id="40e14-167">Notification overlays are great for a fast integration because they does not require you toomodify any view in your application.</span></span>

#### <a name="layout"></a><span data-ttu-id="40e14-168">Elrendezés</span><span class="sxs-lookup"><span data-stu-id="40e14-168">Layout</span></span>
<span data-ttu-id="40e14-169">Az alkalmazásbeli értesítések toomodify hello megjelenését, egyszerűen módosíthatja hello fájlt `AENotificationView.xib` tooyour van szüksége, mindaddig, amíg megőrzi hello előfizetéscímkék értékeit és hello meglévő subviews típusú.</span><span class="sxs-lookup"><span data-stu-id="40e14-169">toomodify hello look of your in-app notifications, you can simply modify hello file `AENotificationView.xib` tooyour needs, as long as you keep hello tag values and types of hello existing subviews.</span></span>

<span data-ttu-id="40e14-170">Alapértelmezés szerint az alkalmazásbeli értesítésekben üdvözlő képernyőt hello alján jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="40e14-170">By default, in-app notifications are presented at hello bottom of hello screen.</span></span> <span data-ttu-id="40e14-171">Ha jobban szeret toodisplay azokat a képernyő, Szerkesztés hello hello tetejére megadott `AENotificationView.xib` , és módosítsa a hello `AutoSizing` tulajdonság hello fő nézet, így is megmarad a superview hello tetején.</span><span class="sxs-lookup"><span data-stu-id="40e14-171">If you prefer toodisplay them at hello top of screen, edit hello provided `AENotificationView.xib` and change hello `AutoSizing` property of hello main view so it can be kept at hello top of its superview.</span></span>

#### <a name="categories"></a><span data-ttu-id="40e14-172">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="40e14-172">Categories</span></span>
<span data-ttu-id="40e14-173">Ha módosítja a megadott elrendezés hello, az értesítések hello megjelenésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="40e14-173">When you modify hello provided layout, you modify hello look of all your notifications.</span></span> <span data-ttu-id="40e14-174">Kategóriák engedélyezése toodefine, különböző megcélzott értesítések (valószínűleg viselkedések) keresi.</span><span class="sxs-lookup"><span data-stu-id="40e14-174">Categories allow you toodefine various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="40e14-175">A kategória a Reach-kampány létrehozásakor adható meg.</span><span class="sxs-lookup"><span data-stu-id="40e14-175">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="40e14-176">Ne feledje, hogy kategóriák is segítségével testre szabható mutató hirdetmények és szavazások, a dokumentum későbbi szakaszában ismertetett.</span><span class="sxs-lookup"><span data-stu-id="40e14-176">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="40e14-177">az értesítések kategória leíró tooregister, kell tooadd hívása után hello reach-modul inicializálása.</span><span class="sxs-lookup"><span data-stu-id="40e14-177">tooregister a category handler for your notifications, you need tooadd a call once hello reach module is initialized.</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

<span data-ttu-id="40e14-178">`myNotifier`egy olyan objektum, amely megfelel a toohello protokoll példányának kell lennie `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="40e14-178">`myNotifier` must be an instance of an object that conforms toohello protocol `AENotifier`.</span></span>

<span data-ttu-id="40e14-179">Megvalósíthat hello protokoll módszerek önállóan, vagy dönthet úgy tooreimplement hello meglévő osztály `AEDefaultNotifier` melyik már teljesít hello munka nagyobb része.</span><span class="sxs-lookup"><span data-stu-id="40e14-179">You can implement hello protocol methods by yourself or you can choose tooreimplement hello existing class `AEDefaultNotifier` which already performs most of hello work.</span></span>

<span data-ttu-id="40e14-180">Például ha egy adott konkrét kategóriával tooredefine hello értesítési nézet használni szeretne, ebben a példában követheti:</span><span class="sxs-lookup"><span data-stu-id="40e14-180">For example, if you want tooredefine hello notification view for a specific category, you can follow this example:</span></span>

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

<span data-ttu-id="40e14-181">Egyszerű példa a kategória azt feltételezik, hogy rendelkezik-e egy fájlt `MyNotificationView.xib` az alkalmazás fő kötegben.</span><span class="sxs-lookup"><span data-stu-id="40e14-181">This simple example of category assume that you have a file named `MyNotificationView.xib` in your main application bundle.</span></span> <span data-ttu-id="40e14-182">Ha hello metódus nem tudja a megfelelő toofind `.xib`, hello értesítés nem fog megjelenni, és Engagement kimeneteként hello konzolon üzenet.</span><span class="sxs-lookup"><span data-stu-id="40e14-182">If hello method is not able toofind a corresponding `.xib`, hello notification will not be displayed and Engagement will output a message in hello console.</span></span>

<span data-ttu-id="40e14-183">a megadott hello nib fájl tiszteletben kell szabályainak hello:</span><span class="sxs-lookup"><span data-stu-id="40e14-183">hello provided nib file should respect hello following rules:</span></span>

* <span data-ttu-id="40e14-184">Csak egy nézetet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="40e14-184">It should only contain one view.</span></span>
* <span data-ttu-id="40e14-185">Subviews hello azonos típusokat, hello kiépítettektől eltérő nevű fájlban található a megadott hello nib belül kell lennie`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="40e14-185">Subviews should be of hello same types as hello ones inside hello provided nib file named `AENotificationView.xib`</span></span>
* <span data-ttu-id="40e14-186">Subviews hello azonos címkéket hello megadott hello nib fájl nevű kiépítettektől eltérő módon kell rendelkeznie.`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="40e14-186">Subviews should have hello same tags as hello ones inside hello provided nib file named `AENotificationView.xib`</span></span>

> [!TIP]
> <span data-ttu-id="40e14-187">Csak a megadott hello nib fájlt, másolja `AENotificationView.xib`, és ott munka megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="40e14-187">Just copy hello provided nib file, named `AENotificationView.xib`, and start working from there.</span></span> <span data-ttu-id="40e14-188">De legyen óvatos, a nib fájl belső tartozik toohello osztály nézet hello `AENotificationView`.</span><span class="sxs-lookup"><span data-stu-id="40e14-188">But be careful, hello view inside this nib file is associated toohello class `AENotificationView`.</span></span> <span data-ttu-id="40e14-189">Ez az osztály újradefiniálása hello metódus `layoutSubViews` toomove és annak megfelelően toocontext subviews átméretezése.</span><span class="sxs-lookup"><span data-stu-id="40e14-189">This class redefined hello method `layoutSubViews` toomove and resize its subviews according toocontext.</span></span> <span data-ttu-id="40e14-190">Érdemes lehet tooreplace azt egy `UIView` vagy egyéni nézet osztály.</span><span class="sxs-lookup"><span data-stu-id="40e14-190">You may want tooreplace it with an `UIView` or you custom view class.</span></span>
>
>

<span data-ttu-id="40e14-191">Ha módosítania kell az értesítések (Ha azt kívánja, például tooload a nézet közvetlenül a kódból hello) mélyebb testreszabása, ajánlott hello tekintse meg a kódot és az osztály dokumentációban forrás megadott tootake `Protocol ReferencesDefaultNotifier` és `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="40e14-191">If you need deeper customization of your notifications(if you want for instance tooload your view directly from hello code), it is recommended tootake a look at hello provided source code and class documentation of `Protocol ReferencesDefaultNotifier` and `AENotifier`.</span></span>

<span data-ttu-id="40e14-192">Vegye figyelembe, hogy is használhat ugyanazon bejelentő több kategóriákban hello.</span><span class="sxs-lookup"><span data-stu-id="40e14-192">Note that you can use hello same notifier for multiple categories.</span></span>

<span data-ttu-id="40e14-193">A kiválasztott is hello alapértelmezett bejelentő ehhez hasonló a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="40e14-193">You can also redefined hello default notifier like this:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a><span data-ttu-id="40e14-194">Értesítés kezelése</span><span class="sxs-lookup"><span data-stu-id="40e14-194">Notification handling</span></span>
<span data-ttu-id="40e14-195">Hello alapértelmezett kategóriát használatakor a hello egyes életciklus metódusok nevezik `AEReachContent` objektum tooreport statisztikák és a frissítés hello kampány állapota:</span><span class="sxs-lookup"><span data-stu-id="40e14-195">When using hello default category, some life cycle methods are called on hello `AEReachContent` object tooreport statistics and update hello campaign state:</span></span>

* <span data-ttu-id="40e14-196">Alkalmazás hello értesítés jelenik meg, amikor hello `displayNotification` módszer neve (amely statisztikáit) által `AEReachModule` Ha `handleNotification:` adja vissza `YES`.</span><span class="sxs-lookup"><span data-stu-id="40e14-196">When hello notification is displayed in application, hello `displayNotification` method is called (which reports statistics) by `AEReachModule` if `handleNotification:` returns `YES`.</span></span>
* <span data-ttu-id="40e14-197">Ha hello értesítési eltűnése hello `exitNotification` metódus lehívásra kerül, statisztika jelez, és tovább kampányok most már tudja feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="40e14-197">If hello notification is dismissed, hello `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="40e14-198">Ha hello értesítési kattint, `actionNotification` van nevű statisztika jelez, és hello tartozó művelet elvégzése.</span><span class="sxs-lookup"><span data-stu-id="40e14-198">If hello notification is clicked, `actionNotification` is called, statistic is reported and hello associated action is performed.</span></span>

<span data-ttu-id="40e14-199">Ha a végrehajtásának `AENotifier` Mellőzés hello alapértelmezett viselkedés telepítette, toocall életciklus módszerekhez önállóan.</span><span class="sxs-lookup"><span data-stu-id="40e14-199">If your implementation of `AENotifier` bypasses hello default behavior, you have toocall these life cycle methods by yourself.</span></span> <span data-ttu-id="40e14-200">hello a következő példák bemutatják, egyes esetekben, ahol hello alapértelmezett viselkedés elmarad:</span><span class="sxs-lookup"><span data-stu-id="40e14-200">hello following examples illustrate some cases where hello default behavior is bypassed:</span></span>

* <span data-ttu-id="40e14-201">Nem terjeszti ki `AEDefaultNotifier`, pl. megvalósította a teljesen új kategória kezelését.</span><span class="sxs-lookup"><span data-stu-id="40e14-201">You don't extend `AEDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="40e14-202">Ön overrode `prepareNotificationView:forContent:`, nem lehet rövidebb meg arról, hogy toomap `onNotificationActioned` vagy `onNotificationExited` tooone a U.I vezérlők.</span><span class="sxs-lookup"><span data-stu-id="40e14-202">You overrode `prepareNotificationView:forContent:`, be sure toomap at least `onNotificationActioned` or `onNotificationExited` tooone of your U.I controls.</span></span>

> [!WARNING]
> <span data-ttu-id="40e14-203">Ha `handleNotification:` jelez kivétel, hello tartalom törlése és `drop` van neve, ez statisztika jelez, és tovább kampányok most már tudja feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="40e14-203">If `handleNotification:` throws an exception, hello content is deleted and `drop` is called, this is reported in statistics and next campaigns can now be processed.</span></span>
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a><span data-ttu-id="40e14-204">Egy meglévő nézethez részeként értesítési tartalmazza</span><span class="sxs-lookup"><span data-stu-id="40e14-204">Include notification as part of an existing view</span></span>
<span data-ttu-id="40e14-205">Átfedések ideális az egy gyors integrációt, de lehet néha nem kényelmes, vagy nemkívánatos hatásai lehetnek.</span><span class="sxs-lookup"><span data-stu-id="40e14-205">Overlays are great for a fast integration but can be sometimes not convenient, or can have unwanted side effects.</span></span>

<span data-ttu-id="40e14-206">Ha nem elégedett hello átfedő rendszer egyes a nézetek, testre szabhatja, ezek a nézetek.</span><span class="sxs-lookup"><span data-stu-id="40e14-206">If you're not satisfied with hello overlay system in some of your views, you can customize it for these views.</span></span>

<span data-ttu-id="40e14-207">Dönthet úgy is tooinclude az értesítési elrendezés a meglévő nézetekben.</span><span class="sxs-lookup"><span data-stu-id="40e14-207">You can decide tooinclude our notification layout in your existing views.</span></span> <span data-ttu-id="40e14-208">toodo Igen, van két megvalósítási stílusok:</span><span class="sxs-lookup"><span data-stu-id="40e14-208">toodo so, there is two implementation styles:</span></span>

1. <span data-ttu-id="40e14-209">Adja hozzá a felület builder használatával hello értesítési nézet</span><span class="sxs-lookup"><span data-stu-id="40e14-209">Add hello notification view using interface builder</span></span>

   * <span data-ttu-id="40e14-210">Nyissa meg *jelentéskészítő csatoló*</span><span class="sxs-lookup"><span data-stu-id="40e14-210">Open *Interface Builder*</span></span>
   * <span data-ttu-id="40e14-211">Helyezze a 320 x 60 (vagy ha iPad 768 x 60) `UIView` hello értesítési tooappear, ahová</span><span class="sxs-lookup"><span data-stu-id="40e14-211">Place a 320x60 (or 768x60 if you are on iPad) `UIView` where you want hello notification tooappear</span></span>
   * <span data-ttu-id="40e14-212">Ebben a nézetben hello címke értékének túl beállítása: **36822491**</span><span class="sxs-lookup"><span data-stu-id="40e14-212">Set hello Tag value for this view too: **36822491**</span></span>
2. <span data-ttu-id="40e14-213">Programozott módon vegye fel a hello értesítési nézet.</span><span class="sxs-lookup"><span data-stu-id="40e14-213">Add hello notification view programmatically.</span></span> <span data-ttu-id="40e14-214">A nézet inicializálásakor a következő kód hello hozzá:</span><span class="sxs-lookup"><span data-stu-id="40e14-214">Just add hello following code when your view has been initialized:</span></span>

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values tooyour needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

<span data-ttu-id="40e14-215">`NOTIFICATION_AREA_VIEW_TAG`makró található `AEDefaultNotifier.h`.</span><span class="sxs-lookup"><span data-stu-id="40e14-215">`NOTIFICATION_AREA_VIEW_TAG` macro can be found in `AEDefaultNotifier.h`.</span></span>

> [!NOTE]
> <span data-ttu-id="40e14-216">hello alapértelmezett bejelentő automatikusan észleli, hogy hello értesítési elrendezés Ez a nézet tartalmazza, és nem adja hozzá egy átmeneti területre az.</span><span class="sxs-lookup"><span data-stu-id="40e14-216">hello default notifier automatically detects that hello notification layout is included in this view and will not add an overlay for it.</span></span>
>
>

### <a name="announcements-and-polls"></a><span data-ttu-id="40e14-217">Hirdetmények és szavazások esetén</span><span class="sxs-lookup"><span data-stu-id="40e14-217">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="40e14-218">Elrendezés</span><span class="sxs-lookup"><span data-stu-id="40e14-218">Layouts</span></span>
<span data-ttu-id="40e14-219">Módosíthatja a hello fájlok `AEDefaultAnnouncementView.xib` és `AEDefaultPollView.xib` mindaddig, amíg megőrzi hello előfizetéscímkék értékeit és hello meglévő subviews típusú.</span><span class="sxs-lookup"><span data-stu-id="40e14-219">You can modify hello files `AEDefaultAnnouncementView.xib` and `AEDefaultPollView.xib` as long as you keep hello tag values and types of hello existing subviews.</span></span>

#### <a name="categories"></a><span data-ttu-id="40e14-220">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="40e14-220">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="40e14-221">Alternatív elrendezések</span><span class="sxs-lookup"><span data-stu-id="40e14-221">Alternate layouts</span></span>
<span data-ttu-id="40e14-222">Értesítések, például a hello kampány kategória lehet használt toohave alternatív elrendezések a hirdetmények és szavazások esetén.</span><span class="sxs-lookup"><span data-stu-id="40e14-222">Like notifications, hello campaign's category can be used toohave alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="40e14-223">toocreate bejelentés kategóriáját, ki kell terjesztenie **AEAnnouncementViewController** , és regisztrálhatja azt hello reach-modul inicializálása után:</span><span class="sxs-lookup"><span data-stu-id="40e14-223">toocreate a category for an announcement, you must extend **AEAnnouncementViewController** and register it once hello reach module has been initialized:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> <span data-ttu-id="40e14-224">Minden alkalommal, amikor a felhasználó fog kattintson a közlemény hello kategória az értesítés "a\_kategória", a regisztrált nézetvezérlő (ebben az esetben `MyCustomAnnouncementViewController`) fog hello metódus meghívásával inicializálható `initWithAnnouncement:` és hello lesz a hozzáadott toohello aktuális alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="40e14-224">Each time a user will click on a notification for an announcement with hello category "my\_category", your registered view controller (in that case `MyCustomAnnouncementViewController`) will be initialized by calling hello method `initWithAnnouncement:` and hello view will be added toohello current application window.</span></span>
>
>

<span data-ttu-id="40e14-225">Hello példányában `AEAnnouncementViewController` tooread hello tulajdonság követően osztály `announcement` tooinitialize a subviews.</span><span class="sxs-lookup"><span data-stu-id="40e14-225">In your implementation of hello `AEAnnouncementViewController` class you will have tooread hello property `announcement` tooinitialize your subviews.</span></span> <span data-ttu-id="40e14-226">Tekintse meg az alábbi, hello példát, ahol két címkék használatával inicializálása `title` és `body` hello tulajdonságainak `AEReachAnnouncement` osztály:</span><span class="sxs-lookup"><span data-stu-id="40e14-226">Consider hello example below, where two labels are initialized using `title` and `body` properties of hello `AEReachAnnouncement` class:</span></span>

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

<span data-ttu-id="40e14-227">Ha nem szeretné, hogy tooload a nézetek önállóan, de csak kívánt tooreuse hello alapértelmezett bejelentés nézet elrendezését, egyszerűen elvégezheti a egyéni nézetvezérlő kiterjeszti a megadott hello osztály `AEDefaultAnnouncementViewController`.</span><span class="sxs-lookup"><span data-stu-id="40e14-227">If you don't want tooload your views by yourself but you just want tooreuse hello default announcement view layout, you can simply make your custom view controller extends hello provided class `AEDefaultAnnouncementViewController`.</span></span> <span data-ttu-id="40e14-228">Ebben az esetben a hello nib fájl ismétlődő `AEDefaultAnnouncementView.xib` , és nevezze át, az egyéni nézetvezérlő betölthető legyen (nevű tartományvezérlő `CustomAnnouncementViewController`, meg kell hívnia a nib fájl `CustomAnnouncementView.xib`).</span><span class="sxs-lookup"><span data-stu-id="40e14-228">In that case, duplicate hello nib file `AEDefaultAnnouncementView.xib` and rename it so it can be loaded by your custom view controller (for a controller named `CustomAnnouncementViewController`, you should call your nib file `CustomAnnouncementView.xib`).</span></span>

<span data-ttu-id="40e14-229">tooreplace hello alapértelmezett kategória hirdetmények, egyszerűen regisztrálja a hello kategória definiált egyéni nézetvezérlő `kAEReachDefaultCategory`:</span><span class="sxs-lookup"><span data-stu-id="40e14-229">tooreplace hello default category of announcements, simply register your custom view controller for hello category defined in `kAEReachDefaultCategory`:</span></span>

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

<span data-ttu-id="40e14-230">Szavazások lehet testreszabott hello ugyanúgy:</span><span class="sxs-lookup"><span data-stu-id="40e14-230">Polls can be customized hello same way :</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

<span data-ttu-id="40e14-231">Az idő, a megadott hello `MyCustomPollViewController` kell terjesztenie `AEPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="40e14-231">This time, hello provided `MyCustomPollViewController` must extend `AEPollViewController`.</span></span> <span data-ttu-id="40e14-232">Vagy tooextend hello alapértelmezett vezérlő közül választhat: `AEDefaultPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="40e14-232">Or you can choose tooextend from hello default controller: `AEDefaultPollViewController`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40e14-233">Ne feledkezzen meg vagy toocall `action` (`submitAnswers:` az egyéni lekérdezési nézet vezérlők) vagy `exit` metódus hello nézetvezérlő eltűnése előtt.</span><span class="sxs-lookup"><span data-stu-id="40e14-233">Don't forget toocall either `action` (`submitAnswers:` for custom poll view controllers) or `exit` method before hello view controller is dismissed.</span></span> <span data-ttu-id="40e14-234">Ellenkező esetben statisztika nem küldhető el (azaz nem elemzés hello kampány), és további fontos következő kampányok nem kap értesítést hello alkalmazás folyamatának újraindításáig.</span><span class="sxs-lookup"><span data-stu-id="40e14-234">Otherwise, statistics won't be sent (i.e. no analytics on hello campaign) and more importantly next campaigns will not be notified until hello application process is restarted.</span></span>
>
>

##### <a name="implementation-example"></a><span data-ttu-id="40e14-235">Megvalósítási példa</span><span class="sxs-lookup"><span data-stu-id="40e14-235">Implementation example</span></span>
<span data-ttu-id="40e14-236">Ebben a megvalósításban hello egyéni bejelentés nézet be van töltve egy külső xib fájlból.</span><span class="sxs-lookup"><span data-stu-id="40e14-236">In this implementation hello custom announcement view is loaded from an external xib file.</span></span>

<span data-ttu-id="40e14-237">Például a speciális értesítési testreszabáshoz ajánlott: hello forráskódját hello szabványos implementációjától toolook.</span><span class="sxs-lookup"><span data-stu-id="40e14-237">Like for advanced notification customization, it is recommended toolook at hello source code of hello standard implementation.</span></span>

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
