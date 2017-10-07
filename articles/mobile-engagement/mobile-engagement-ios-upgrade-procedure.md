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
# <a name="upgrade-procedures"></a>Frissítési eljárások
Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.

Minden egyes új verziójának hello SDK először cserélje le (távolítsa el, és importálja újra az xcode-ban) hello EngagementSDK és EngagementReach mappák.

## <a name="from-300-too400"></a>A 3.0.0 too4.0.0
### <a name="xcode-8"></a>XCode 8
XCode 8 megadása kötelező hello SDK 4.0.0 verziójával kezdve.

> [!NOTE]
> Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Nincs egy ismert hiba az előző verzió hello reach-modul iOS 10-eszközök futtatása közben: nincsenek műveletet kiváltó rendszerértesítéseket. toofix ezt követően tooimplement hello elavult API `application:didReceiveRemoteNotification:` az alkalmazás delegálása az alábbiak szerint:
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Ez a megoldás nem ajánlott** , ez a viselkedés a jövőbeli (még akkor is kisebb) iOS verzió frissítése is módosítható, mivel az iOS API elavult. Amint lehetséges tooXCode 8 kell váltani.
> 
> 

### <a name="usernotifications-framework"></a>UserNotifications keretrendszer
Tooadd hello kell `UserNotifications` keretrendszer a Build lépésből áll.

a project explorer hello nyissa meg a projekt ablaktáblán, majd válassza a hello helyes célobjektum. Ezután nyissa meg a hello **"Összeállítási fázisok"** lapon és a hello **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszer `UserNotifications.framework` -set hello hivatkozásokat`Optional`

### <a name="application-push-capability"></a>Alkalmazás leküldéses funkció
XCode 8 lehetséges, hogy az alkalmazás alaphelyzetbe leküldéses funkció, ellenőrizze még egyszer a hello `capability` a kiválasztott cél fülre.

### <a name="add-hello-new-ios-10-notification-registration-code"></a>Hello új iOS 10 értesítési regisztrációs kód hozzáadása
hello régebbi kód részlet tooregister hello app toonotifications továbbra is működik, de a által használt API-k elavult IOS 10 futtatása során.

Importálás hello `User Notification` keretrendszer:

        #import <UserNotifications/UserNotifications.h> 

Az alkalmazás delegáltjának a `application:didFinishLaunchingWithOptions` metódus cseréje:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

szerint:

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter delegált ütközések feloldása

*Ha az alkalmazás és a harmadik féltől származó tárak egyike nem valósítja meg a `UNUserNotificationCenterDelegate` akkor kihagyhatja ezt a részt.*

A `UNUserNotificationCenter` delegált hello SDK toomonitor hello életciklusa az Engagement értesítések 10 vagy újabb IOS-es eszközökön használják. hello SDK rendelkezik saját hello végrehajtásának `UNUserNotificationCenterDelegate` protokoll, de lehet csak az egyik `UNUserNotificationCenter` alkalmazásonként delegálni. Bármely más delegált hozzáadott toohello `UNUserNotificationCenter` objektum hello egy bevonási ütközik. Ha a hello SDK a vagy bármely más harmadik fél delegált észlel, akkor nem fogja használni a saját megvalósítási toogive, egy alkalommal tooresolve hello ütközések. Hogy tooadd hello bevonási programot tooyour saját sorrendben delegált tooresolve hello ütközések.

Nincsenek két módon tooachieve ez.

Javaslat: 1, a delegált működve egyszerűen meghívja toohello SDK:

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

Vagy javaslat 2, hello való öröklődés `AEUserNotificationHandler` osztály

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
> Azt is meghatározhatja, hogy értesítést az engagement szolgáltatásból, vagy nem úgy, hogy rendelkezik-e a `userInfo` szótár toohello ügynök `isEngagementPushPayload:` metódus osztályban.

Győződjön meg arról, hogy hello `UNUserNotificationCenter` objektum delegált tooyour delegált belül vagy hello beállítása `application:willFinishLaunchingWithOptions:` vagy hello `application:didFinishLaunchingWithOptions:` az alkalmazás delegáltjának metódusában.
Például ha megvalósította a fent javaslat 1 hello:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-too300"></a>A 2.0.0 too3.0.0
Támogatás az iOS eldobott 4.X. A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 6.

Ha az alkalmazás Reach használ, hozzá kell adnia `remote-notification` érték toohello `UIBackgroundModes` tömb rendelés tooreceive távoli értesítések az Info.plist fájlban.

hello metódus `application:didReceiveRemoteNotification:` helyébe toobe kell `application:didReceiveRemoteNotification:fetchCompletionHandler:` a az alkalmazás delegáltjának.

Elavult "AEPushDelegate.h" illesztőfelület, és át kell tooremove összes hivatkozást. Ez magában foglalja eltávolításával `[[EngagementAgent shared] setPushDelegate:self]` és módszerek delegálása a az alkalmazás delegáltjának hello:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-too200"></a>A 1.16.0 too2.0.0
hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba.
Ha egy korábbi verziójáról telepít, tekintse át először hello Capptain webhely toomigrate too1.16, majd alkalmazza a következő eljárás hello.

> [!IMPORTANT]
> Capptain és a Mobile Engagement nem hello azonos szolgáltatások és hello alábbi eljárás csak emel ki, hogyan toomigrate hello ügyfélalkalmazást. Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement kiszolgálókról
> 
> 

### <a name="agent"></a>Ügynök
hello metódus `registerApp:` hello új metódus lett cserélve `init:`. Az alkalmazás delegáltjának frissíteni kell ennek megfelelően, és használja a kapcsolati karakterlánc:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

SmartAd követési el lett távolítva ugyanúgy lehet tooremove minden példánya SDK `AETrackModule` osztály

### <a name="class-name-changes"></a>Osztály neve megváltozik
Hello rebranding részeként nincsenek osztály/fájlnevek toobe módosítani kell néhány.

Minden osztály előtagként a "CP" átnevezi "AE" előtaggal.

Példa:

* `CPModule.h`túl a rendszer átnevezi`AEModule.h`.

Minden osztály előtagként a "Capptain" átnevezi "Engagement" előtaggal.

Példák:

* osztály hello `CapptainAgent` túl a rendszer átnevezi`EngagementAgent`.
* osztály hello `CapptainTableViewController` túl a rendszer átnevezi`EngagementTableViewController`.
* osztály hello `CapptainUtils` túl a rendszer átnevezi`EngagementUtils`.
* osztály hello `CapptainViewController` túl a rendszer átnevezi`EngagementViewController`.

