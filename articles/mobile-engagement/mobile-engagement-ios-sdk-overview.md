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
# <a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK for Azure Mobile Engagement (iOS SDK és Azure Mobile Engagement)
Kezdje itt tooget összes hello részletes hogyan toointegrate Azure Mobile Engagement az iOS-alkalmazás. Ha azt szeretné, hogy toogive azt egy try először győződjön meg arról, hogy szeretne a [15 perc oktatóanyag](mobile-engagement-ios-get-started.md).

Kattintson a toosee hello [SDK tartalom](mobile-engagement-ios-sdk-content.md)

## <a name="integration-procedures"></a>Integráció eljárások
1. Kezdje itt: [hogyan toointegrate a Mobile Engagement az iOS-alkalmazás](mobile-engagement-ios-integrate-engagement.md)
2. Az értesítések: [hogyan toointegrate Reach (értesítések) az iOS-alkalmazás](mobile-engagement-ios-integrate-engagement-reach.md)
3. Megvalósítási terv címkét: [hogyan toouse hello speciális a Mobile Engagement az iOS-alkalmazás szerinti címkézését API](mobile-engagement-ios-use-engagement-api.md)

## <a name="release-notes"></a>Kibocsátási megjegyzések
### <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Rögzített jelvények háttérben törölve lett.
* Rögzített figyelmeztetések a XCode 9 kapcsolatos API-k nem hívja a fő várakozási sorba.
* Rögzített memóriavesztés Reach lekérdezéseinél.
* Támogatás az iOS eldobott 6.X. A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 7-es.

Korábbi verziójához lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-ios-release-notes.md)

## <a name="upgrade-procedures"></a>Frissítési eljárások
Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.

Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK tekintse meg a különböző verzióiban teljes hello [frissítése eljárások](mobile-engagement-ios-upgrade-procedure.md).

Minden egyes új verziójának hello SDK először cserélje le (távolítsa el, és importálja újra az xcode-ban) hello EngagementSDK és EngagementReach mappák.

### <a name="from-300-too400"></a>A 3.0.0 too4.0.0
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

#### <a name="usernotifications-framework"></a>UserNotifications keretrendszer
Tooadd hello kell `UserNotifications` keretrendszer a Build lépésből áll.

a project explorer hello nyissa meg a projekt ablaktáblán, majd válassza a hello helyes célobjektum. Ezután nyissa meg a hello **"Összeállítási fázisok"** lapon és a hello **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszer `UserNotifications.framework` -set hello hivatkozásokat`Optional`

#### <a name="application-push-capability"></a>Alkalmazás leküldéses funkció
XCode 8 lehetséges, hogy az alkalmazás alaphelyzetbe leküldéses funkció, ellenőrizze még egyszer a hello `capability` a kiválasztott cél fülre.

#### <a name="add-hello-new-ios-10-notification-registration-code"></a>Hello új iOS 10 értesítési regisztrációs kód hozzáadása
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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter delegált ütközések feloldása

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
