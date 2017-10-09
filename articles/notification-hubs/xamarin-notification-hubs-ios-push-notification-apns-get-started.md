---
title: "aaaiOS leküldéses értesítések küldése a Notification hubs használatával Xamarin-alkalmazásokba |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Xamarin iOS-alkalmazás."
services: notification-hubs
keywords: "ios leküldéses értesítések,leküldéses üzenetek,leküldéses értesítések,leküldéses üzenet"
documentationcenter: xamarin
author: ysxu
manager: erikre
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8db60338047dd53074b4d3d4bb127aa6d9f13a25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS leküldéses értesítések küldése a Notification Hubs használatával Xamarin-alkalmazásokba
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
> [!IMPORTANT]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooan iOS-alkalmazás.
Lesz hozzon létre egy üres Xamarin.iOS-alkalmazást, amely leküldéses értesítéseket fogad hello segítségével [Apple Push Notification szolgáltatás (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html). Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató. hello befejezett kód is elérhető hello [NotificationHubs-alkalmazásban] [ GitHub] minta.

Ez az oktatóanyag bemutatja, hogyan hello egyszerű leküldéses üzenetküldési forgatókönyvet a Notification hubs használatával.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hello következő szükséges:

* [XCode 6.0][Install Xcode]
* Az iOS 7.0-tel (vagy újabb verzióval) kompatibilis eszköz
* iOS fejlesztőprogrambeli tagság
* [Xamarin Studio]
  
  > [!NOTE]
  > Az iOS leküldéses értesítések konfigurációs követelményei miatt kell telepítenie és hello mintaalkalmazás egy fizikai iOS-eszközön (iPhone vagy iPad) tesztelje hello szimulátor helyett.
  > 
  > 

Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Xamarin iOS-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a>Az értesítési központ konfigurálása
Ez a szakasz végigvezeti egy új értesítési központ létrehozása és konfigurálása a hitelesítés az apns-sel hello segítségével **.p12** létrehozott leküldéses tanúsítvány. Ha azt szeretné, hogy toouse egy már létrehozott értesítési központot, kihagyhatja toostep 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p>Tooconfigure hello APNS-kapcsolatot az Azure-portálon hello szeretnénk, nyissa meg az értesítési központ beállításait, ande kattintson azon <b>értesítési szolgáltatások</b>, és kattintson a hello <b>Apple (APNS)</b> hello lista elemére. Ha végzett, kattintson a <b>tanúsítvány feltöltése</b> és select hello <b>.p12</b> korábbi, valamint hello tanúsítvány jelszava hello exportált tanúsítványt.</p>

<p>Győződjön meg arról, hogy tooselect <b>védőfal</b> módot, mivel küldi leküldéses üzenetek fejlesztői környezetben. Csak a hello használata <b>éles</b> beállítást, ha azt szeretné, hogy toosend leküldéses értesítések toousers akik már megvásárolták az alkalmazást hello áruházból.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

Az értesítési központ most már konfigurált toowork az apns-sel, és rendelkezik hello kapcsolati karakterláncok tooregister az alkalmazást, és leküldéses értesítések küldéséhez.

## <a name="connect-your-app-toohello-notification-hub"></a>Csatlakozás az alkalmazás toohello értesítési központ
#### <a name="create-a-new-project"></a>Új projekt létrehozása
1. A Xamarin Studióban hozzon létre egy új iOS-projektet, és válassza ki a hello **egyesített API** > **egyetlen nézetben alkalmazás** sablont.
   
     ![Xamarin Studio – Alkalmazástípus kiválasztása][31]
2. Hivatkozás toohello Azure Messaging összetevő hozzáadása. A hello megoldás nézetet, kattintson a jobb gombbal hello **összetevők** mappa a projekthez, és válassza a **további összetevők beszerzése**. Keresse meg a hello **Azure Messaging** összetevő, és adja hozzá a hello összetevő tooyour projekt.
3. A **AppDelegate.cs**, adja hozzá hello következő using utasítást:
   
        using WindowsAzure.Messaging;
4. Deklarálja az **SBNotificationHub** egy példányát:
   
        private SBNotificationHub Hub { get; set; }
5. Hozzon létre egy **Constants.cs** hello változók a következő osztályra:
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. A **AppDelegate.cs**, frissítése **FinishedLaunching()** toomatch hello következő:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
7. Bírálja felül a hello **RegisteredForRemoteNotifications()** metódus a **AppDelegate.cs**:
   
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
8. Bírálja felül a hello **ReceivedRemoteNotification()** metódus a **AppDelegate.cs**:
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. Hozza létre a következőket hello **ProcessNotification()** metódus a **AppDelegate.cs**:
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check toosee if hello dictionary has hello aps key.  This is hello notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get hello aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract hello alert text
                // NOTE: If you're using hello simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from hello aps dictionary will be another NSDictionary.
                // Basically hello JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from hello ReceivedRemoteNotification while hello app was running,
                // we of course need toomanually process things like hello sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
   
   > [!NOTE]
   > Kiválaszthatja a toooverride **FailedToRegisterForRemoteNotifications()** toohandle olyan helyzetekben, például a hálózati kapcsolat. Ez akkor kifejezetten fontos, ahol hello felhasználói (például repülőgép üzemmódban) kapcsolat nélküli módban is elindíthatják az alkalmazást, és azt szeretné, hogy toohandle leküldéses üzenetküldési forgatókönyveket adott tooyour alkalmazás.
   > 
   > 
10. Hello alkalmazás futtatható az eszközön.

## <a name="sending-push-notifications"></a>Leküldéses értesítések küldése
Leküldéses értesítések fogadásának az alkalmazásban a hello küldött értesítésekkel tesztelheti [Azure Portal] keresztül hello **küldés tesztelése** hello képesség **hibaelhárítás** eszközkészlet, közvetlenül hello értesítési központ lapján, ahogy az alábbi üdvözlő képernyőt.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET. Hello REST API-t használhatja közvetlen toosend leküldéses üzenetek, ha a szalagtár nem érhető el a forgatókönyvben is. 

Ebben az oktatóanyagban a rendszer egyszerűen és csak az ügyfélalkalmazás tesztelését küldött értesítésekkel hello .NET SDK használatával egy háttér-szolgáltatás helyett egy konzolalkalmazás értesítési központjának bemutatása. Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyagot hello következő lépés az értesítéseknek ASP.NET-háttérrendszerből történő küldéséhez. A következő módszerekkel hello azonban az értesítések küldésével használhatók:

* **REST-felület**: bármely háttér platformon hello segítségével támogathatja a leküldéses értesítési [REST-felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **A Microsoft Azure Notification Hubs .NET SDK**: hello Nuget-Csomagkezelőt a Visual Studio, a futtasson [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **NODE.js** : [hogyan toouse Notification Hubs Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile Apps**: A példa bemutatja, hogyan toosend értesítések a Notification Hubs szolgáltatással integrált Azure App Service Mobile Apps-háttérrendszerből lásd [Hozzáadás leküldéses értesítések tooyour mobilalkalmazás](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: hogyan toosend leküldéses értesítések segítségével hello REST API-k példát lásd: "hogyan toouse Notification Hubs Java/php-ből" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a>(Választható) Leküldéses értesítések küldése .NET-konzolalkalmazásból
Ebben a szakaszban egy egyszerű .NET konzolalkalmazás használatával küldünk leküldéses értesítéseket. A jelen példában hello célokra átváltunk tooa Windows fejlesztői környezetre, a Visual Studio már telepítve van.

1. A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást:
   
       ![Visual Studio - Create a new console application][213]
2. A Visual Studióban kattintson az **Eszközök**, a **NuGet Package Manager** (NuGet-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.
   
    a Visual Studio munkaterületének aljához rögzített toohello hello Csomagkezelő konzolján jelenik meg.
3. Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Nyissa meg hello `Program.cs` fájlt, és adja hozzá a következő hello `using` utasítást, amely biztosítja, hogy használhatunk Azure-osztályokat és -függvényeket a fő osztályban:
   
        using Microsoft.Azure.NotificationHubs;
5. Az a `Program` osztály, adja hozzá a következő metódus hello (ne feledje tooreplace hello **kapcsolati karakterlánc** és **központnév**):
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. Adja hozzá az alábbi hello a `Main` módszert:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Nyomja le az hello F5 kulcs toorun hello alkalmazást. Néhány másodpercen belül meg kell jelennie egy leküldéses értesítésnek az eszközön. Függetlenül attól, hogy Wi-Fi- vagy mobil adatátviteli hálózathoz csatlakozik, győződjön meg arról, hogy rendelkezik-e aktív internetkapcsolat hello eszközön.

Minden hello lehetséges hasznos adatot megtalálja az hello Apple [helyi és leküldéses értesítések programozásával foglalkozó útmutatójában].

#### <a name="optional-send-notifications-from-a-mobile-service"></a>(Választható) Értesítések küldése mobilszolgáltatásból
Ebben a szakaszban egy mobilszolgáltatás használatával küldünk leküldéses értesítéseket egy csomópontparancsfájl segítségével.

hajtsa végre a toosend egy mobilszolgáltatás használatával értesítést [Ismerkedés a Mobile Services], majd:

1. Jelentkezzen be toohello [klasszikus Azure portál], és jelölje ki a mobilszolgáltatást.
2. Jelölje be hello **Feladatütemező** hello legfelső lap.
   
       ![Azure Classic Portal - Scheduler][215]
3. Hozzon létre egy új ütemezett feladatot, szúrjon be egy nevet, és válassza az **On demand** (Igény szerint) lehetőséget.
   
       ![Azure Classic Portal - Create new job][216]
4. Ha hello feladat jön létre, kattintson a hello feladat neve. Kattintson a hello **parancsfájl** hello felső sávon fülre.
5. Helyezze be a következő parancsfájlt a scheduler függvényébe hello. Győződjön meg arról, hogy tooreplace hello helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultFullSharedAccessSignature* korábban beszerzett. Kattintson a **Save** (Mentés) gombra.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                  "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );
6. Kattintson a **egyszeri futtatás** hello alsó sáv. Az eszköznek egy riasztást kell fogadnia.

## <a name="next-steps"></a>Következő lépések
Ez az egyszerű példában küldött leküldéses értesítések tooall az iOS-eszközökre. A rendezés tootarget adott felhasználók, tekintse meg a toohello oktatóanyag [Notification Hubs használata toopush értesítések toousers]. Ha azt szeretné, toosegment a felhasználókat érdeklődési körök, olvasható [legfrissebb hírek Notification Hubs használata toosend]. További tudnivalók toouse értesítési központok [Notification Hubs használatával] és hello [Notification Hubs útmutató-toofor iOS].

<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Ismerkedés a Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[klasszikus Azure portál]: https://manage.windowsazure.com/
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs útmutató-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Notification Hubs használata toopush értesítések toousers]: /manage/services/notification-hubs/notify-users-aspnet
[legfrissebb hírek Notification Hubs használata toosend]: /manage/services/notification-hubs/breaking-news-dotnet

[helyi és leküldéses értesítések programozásával foglalkozó útmutatójában]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com
