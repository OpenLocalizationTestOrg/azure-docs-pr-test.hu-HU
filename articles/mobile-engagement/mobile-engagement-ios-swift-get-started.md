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
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Ismerkedés az Azure Mobile Engagement Swift nyelven írt iOS-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók tooan iOS-alkalmazás.
Ebben az oktatóanyagban létrehoz egy üres iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.

Ez az oktatóanyag hello következő szükséges:

* XCode 8, amely a MAC App Store áruházából telepíthető
* Hello [a Mobile Engagement iOS SDK]
* Leküldéses értesítési tanúsítvány (.p12), amelyet az Apple fejlesztési központjában szerezhet be

> [!NOTE]
> Ez az oktatóanyag a Swift 3.0-s verzióját használja. 
> 
> 

Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Mobile Engagement-oktatóanyag elvégzéséhez.

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).
> 
> 

## <a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez. hello teljes integrációs dokumentáció itt található a hello [a Mobile Engagement iOS SDK-integráció](mobile-engagement-ios-sdk-overview.md)

Létre fogunk hozni egy alapszintű alkalmazást az XCode toodemonstrate hello integráció:

### <a name="create-a-new-ios-project"></a>Új iOS-projekt létrehozása
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toomobile-engagement-backend"></a>Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez
1. Töltse le a hello [a Mobile Engagement iOS SDK]
2. Bontsa ki a hello. tar.gz fájlt a számítógép tooa mappájába
3. Hello projektben kattintson jobb gombbal, majd jelölje ki "fájlok túl hozzáadása..."
   
    ![][1]
4. Keresse meg a toohello mappa, amelyben kibontotta az hello SDK és select hello `EngagementSDK` mappát, majd kattintson az OK gombra.
   
    ![][2]
5. Nyissa meg hello `Build Phases` lapon és a hello `Link Binary With Libraries` menüben adja hozzá hello keretrendszerek alább látható módon:
   
    ![][3]
6. Hozzon létre egy áthidalási fejlécet toobe képes toouse hello SDK Objective C API-k fájl > Új > Fájl > iOS > Source > Header File.
   
    ![][4]
7. Adatközponthíd-képzés fejléc fájl tooexpose Mobile Engagement Objective-C kód tooyour Swift-kód hello szerkesztése, importálja a következő hello hozzáadása:
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. A Build Settings győződjön meg arról Objective-C Bridging fejléc build beállítása a Swift Compiler - Code Generation hello elérési toothis fejléc. Íme egy példa az útvonalra: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (attól függően, hogy hello elérési útja)**
   
   ![][6]
9. Lépjen vissza az alkalmazás Azure-portálon toohello *Kapcsolatinformáció* lapjáról, és másolja a kapcsolati karakterlánc hello
   
   ![][5]
10. Most illessze be a hello kapcsolati karakterláncot a hello `didFinishLaunchingWithOptions` delegálása
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <a id="monitor"></a>Valós idejű figyelés engedélyezése
Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.

1. Nyissa meg hello **ViewController.swift** fájlt, és cserélje le az alaposztály alaposztályát hello **ViewController** toobe **EngagementViewController**:
   
    `class ViewController : EngagementViewController {`

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
Mobile Engagement lehetővé teszi toointeract és a felhasználókkal leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel REACH kampányok hello környezetében. Ez a modul REACH neve hello a Mobile Engagement portálon.
hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Az alkalmazás tooreceive csendes leküldéses értesítések engedélyezése
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a>Hello Reach könyvtár tooyour projekt hozzáadása
1. Kattintson a jobb gombbal a projektre
2. A következők szerint válasszon: `Add file too...`
3. Keresse meg a toohello mappát, amelyikbe kibontotta hello SDK
4. Jelölje be hello `EngagementReach` mappa
5. Kattintson az Add (Hozzáadás) parancsra
6. Adatközponthíd-képzés fejléc fájl tooexpose Mobile Engagement Objective-C Reach fejlécek hello szerkesztése, és importálja a következő hello hozzáadása:
   
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

### <a name="modify-your-application-delegate"></a>Az alkalmazás delegáltjának módosítása
1. Belső hello `didFinishLaunchingWithOptions` – hozzon létre egy reach modult, és adja át tooyour Engagement meglévő inicializációs sorának:
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a>Az alkalmazás tooreceive APNS leküldéses értesítések engedélyezése
1. Adja hozzá a következő sor toohello hello `didFinishLaunchingWithOptions` módszert:
   
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
2. Adja hozzá a hello `didRegisterForRemoteNotificationsWithDeviceToken` módszert az alábbiak szerint:
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. Adja hozzá a hello `didReceiveRemoteNotification:fetchCompletionHandler:` módszert az alábbiak szerint:
   
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
