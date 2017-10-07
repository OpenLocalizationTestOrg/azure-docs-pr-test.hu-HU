---
title: "az Azure Mobile Engagement xamarin.IOS elindítva aaaGet"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Xamarin.iOS-alkalmazásokhoz."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0448209e-fff6-47bd-985c-2cf074bac12f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 02340a744753dcc5cd1b6888a5fa87628be47b68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Ismerkedés az Azure Mobile Engagement Xamarin.iOS-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók Xamarin.iOS-alkalmazásba.
Ebben az oktatóanyagban létrehoz egy üres Xamarin.iOS-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad az Apple leküldéses értesítési rendszerének (APNS) használatával.

> [!NOTE]
> hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek. További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Ez az oktatóanyag hello következő szükséges:

* [Xamarin Studio](http://xamarin.com/studio). Használhatja a Visual Studiót is a Xamarinhoz, de ez az oktatóanyag a Xamarin Studiót használja. A telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése). 
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).
> 
> 

## <a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt" mutat minimális hello határozza meg a szükséges toocollect dátumát és leküldéses értesítés küldéséhez.

Létre fogunk hozni egy alapszintű alkalmazást a Xamarin toodemonstrate hello integrációja:

### <a name="create-a-new-xamarinios-project"></a>Új Xamarin.iOS-projekt létrehozása
1. Indítsa el a Xamarin Studiót. Nyissa meg túl**fájl** -> **új** -> **megoldás** 
   
    ![][1]
2. Válassza ki **egyetlen alkalmazás megtekintése**, ellenőrizze, hogy a kiválasztott hello nyelv **C#** majd **következő**.
   
    ![][2]
3. Adja meg a hello **alkalmazásnév** és hello **Organization Identifier** , majd **tovább**. 
   
    ![][3]
   
   > [!IMPORTANT]
   > Győződjön meg arról, hogy a közzétételi profil olyan alkalmazásazonosítót használ az iOS-alkalmazás az alkalmazás Azonosítóját, amely megfelel használ pontosan az itt rendelkeznek Bundle Identifier hello toodeploy hello. 
   > 
   > 
4. Frissítés hello **projektnevet**, **megoldás neve** és **hely** Ha szükséges, majd kattintson a **létrehozása**.
   
    ![][4]

Xamarin Studio létrehozza a hello bemutatóalkalmazást, amelyben integrálni fogjuk a Mobile Engagementet. 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez
1. Kattintson jobb gombbal a hello **csomagok** megoldás a windows hello, és válassza ki a mappát **csomagok hozzáadása...**
   
    ![][5]
2. Keresse meg a hello **Microsoft Azure Mobile Engagement Xamarin SDK** , és adja hozzá tooyour megoldás.  
   
    ![][6]
3. Nyissa meg **AppDelegate.cs** és adja hozzá hello következő using utasítást:
   
        using Microsoft.Azure.Engagement.Xamarin;
4. A hello **FinishedLaunching** módszer, adja hozzá a következő tooinitialize hello kapcsolat Mobile Engagement háttérrendszeréhez való hello. Győződjön meg arról, hogy tooadd a **ConnectionString**. Ezt a kódot is használ egy helyőrző **NotificationIcon** amelyet hello Mobile Engagement SDK, amelyet érdemes tooreplace. 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <a id="monitor"></a>Valós idejű figyelés engedélyezése
A sorrend toostart adatküldés és biztosítása hello felhasználók aktívak legalább egy képernyőt toohello Mobile Engagement háttérrendszeréhez el kell küldenie.

1. Nyissa meg **ViewController.cs** és adja hozzá hello következő using utasítást:
   
        using Microsoft.Azure.Engagement.Xamarin;
2. Cserélje le a hello osztály, amelyből `ViewController` örököl `UIViewController` túl`EngagementViewController`. 

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
Mobile Engagement lehetővé teszi a felhasználókkal toointeract és elérése révén a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok. Ez a modul REACH neve hello a Mobile Engagement portálon.
hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.

### <a name="modify-your-application-delegate"></a>Az alkalmazás delegáltjának módosítása
1. Nyissa meg hello **AppDelegate.cs** és adja hozzá hello következő using utasítást:
   
        using System; 
2. Most belül hello `FinishedLaunching` módszer, adja hozzá a következő leküldéses üzenetek után tooregister hello`EngagementAgent.init(...)`
   
        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }
3. Végül frissítse vagy vegye fel a következő módszerek hello:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }
   
        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }
   
        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed tooregister for remote notifications: Error '{0}'", error);
        }
4. Az a **Info.plist** hello megoldásban fájlt, győződjön meg arról, hogy hello **Bundle Identifier** megegyezik-e hello **Alkalmazásazonosító** a létesítési profilban az Apple fejlesztői hello rendelkezik Center. 
   
    ![][7]
5. Az azonos hello **Info.plist** fájlt, győződjön meg arról, hogy be van jelölve hello **Háttérmódok engedélyezése** és **távoli értesítések**. 
   
     ![][8]
6. Futtassa a hello alkalmazást ehhez a közzétételi profilhoz társított hello eszközön. 

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
