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
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Ismerkedés az Azure Mobile Engagement Objective C nyelven írt iOS-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók tooan iOS-alkalmazás.
Ebben az oktatóanyagban létrehoz egy üres iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.

Ez az oktatóanyag hello következő szükséges:

* XCode 8, amely a MAC App Store áruházából telepíthető
* Hello [a Mobile Engagement iOS SDK]

Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Mobile Engagement-oktatóanyag elvégzéséhez.

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).
>
>

## <a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez. hello teljes integrációs dokumentáció itt található a hello [a Mobile Engagement iOS SDK-integráció](mobile-engagement-ios-sdk-overview.md)

Létre fogunk hozni egy alapszintű alkalmazást az XCode toodemonstrate hello integráció.

### <a name="create-a-new-ios-project"></a>Új iOS-projekt létrehozása
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
1. Töltse le a hello [a Mobile Engagement iOS SDK].
2. Bontsa ki a hello. tar.gz fájlt a számítógép tooa mappájába.
3. Kattintson a jobb gombbal a hello projektet, majd válassza ki **fájlok hozzáadása**.

    ![][1]
4. Keresse meg a toohello mappát, amelyikbe kibontotta hello SDK, válassza ki a hello `EngagementSDK` mappát, kattintson a **beállítások** a hello bal alsó sarokban található, és győződjön meg arról, hogy hello jelölőnégyzet **elemek másolása, szükség esetén** és hello a cél tartozó jelölőnégyzet be van jelölve, majd nyomja le az **OK**.

    ![][2]
5. Nyitott hello **Build fázisok** fülre, és a hello **bináris rendelkező Kódtárakon** menü hello keretrendszerek hozzáadása a lent látható módon:

    ![][3]
6. Lépjen vissza az alkalmazás Azure-portálon toohello **Kapcsolatinformáció** lapjáról, és másolja hello kapcsolati karakterláncot.

    ![][4]
7. Adja hozzá a következő kódsort hello a **AppDelegate.m** fájlt.

        #import "EngagementAgent.h"
8. Most illessze be a hello kapcsolati karakterláncot a hello `didFinishLaunchingWithOptions` delegálni.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. `setTestLogEnabled`van egy választható utasítás, amely lehetővé teszi, hogy az SDK-naplókat meg tooidentify problémákat.

## <a id="monitor"></a>Valós idejű figyelés engedélyezése
Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.

1. Nyissa meg hello **ViewController.h** fájlt, és importálja **EngagementViewController.h**:

    `#import "EngagementViewController.h"`
2. Cserélje le a hello hello szülőosztály **ViewController** felületének főosztályát `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
Mobile Engagement lehetővé teszi a felhasználókkal toointeract és elérése révén a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok. Ez a modul REACH neve hello a Mobile Engagement portálon.
hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Az alkalmazás tooreceive csendes leküldéses értesítések engedélyezése
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a>Hello Reach könyvtár tooyour projekt hozzáadása
1. Kattintson jobb gombbal a projektre.
2. Válassza az **Add file to** (Fájl hozzáadása ehhez:) elemet.
3. Keresse meg a toohello mappát, amelyikbe kibontotta hello SDK.
4. Jelölje be hello `EngagementReach` mappát.
5. Kattintson az **Add** (Hozzáadás) parancsra.

### <a name="modify-your-application-delegate"></a>Az alkalmazás delegáltjának módosítása
1. Vissza a **AppDeletegate.m** fájlt, importálja hello Engagement Reach modulját.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. Belső hello `application:didFinishLaunchingWithOptions` módszer, hozzon létre egy Reach modult, és adja át tooyour Engagement meglévő inicializációs sorának:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a>Az alkalmazás tooreceive APNS leküldéses értesítések engedélyezése
1. Adja hozzá a következő sor toohello hello `application:didFinishLaunchingWithOptions` módszert:

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
2. Adja hozzá a hello `application:didRegisterForRemoteNotificationsWithDeviceToken` módszert az alábbiak szerint:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. Adja hozzá a hello `didFailToRegisterForRemoteNotificationsWithError` módszert az alábbiak szerint:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed tooget token, error: %@", error);
        }
4. Adja hozzá a hello `didReceiveRemoteNotification:fetchCompletionHandler` módszert az alábbiak szerint:

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
