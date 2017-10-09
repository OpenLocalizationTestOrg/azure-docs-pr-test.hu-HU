---
title: "aaaSending leküldéses értesítések az Azure Notification Hubs – Windows Phone |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toopush értesítések tooa Windows Phone 8 vagy Windows Phone 8.1 Silverlight-alkalmazást."
services: notification-hubs
documentationcenter: windows
keywords: "leküldéses értesítés,leküldéses értesítés,windows phone leküldéses értesítés"
author: ysxu
manager: erikre
editor: erikre
ms.assetid: d872d8dc-4658-4d65-9e71-fa8e34fae96e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 1a0ad238fe7788ae2e4f47f02d113391af03dd1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Leküldéses értesítések küldése az Azure Notification Hubs használatával Windows Phone rendszerű eszközökre
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).
> 
> 

Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Windows Phone 8 vagy Windows Phone 8.1 Silverlight alkalmazás. Ha a Windows Phone 8.1 (nem Silverlight) céloz meg, majd tekintse meg a toohello [univerzális Windows-](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) verziója.
Ebben az oktatóanyagban hozzon létre egy üres Windows Phone 8-alkalmazást, amely leküldéses értesítéseket fogad hello a Microsoft leküldéses értesítési szolgáltatásának (MPNS) használatával. Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.

> [!NOTE]
> hello Notification Hubs Windows Phone SDK nem támogatja a Windows leküldéses értesítési szolgáltatásának (WNS) hello használata a Windows Phone 8.1 Silverlight-alkalmazásokhoz. Windows Phone 8.1 Silverlight-alkalmazásokkal (az MPNS helyett) WNS toouse kövesse hello [Notification Hubs – Windows Phone Silverlight-oktatóanyagot], amely REST API-kat használ.
> 
> 

hello az oktatóanyag bemutatja, hogyan hello egyszerű küldési forgatókönyvet a Notification Hubs használatával.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hello következő szükséges:

* [Windows Phone-hoz készült Visual Studio 2012 Express] vagy újabb verzió.

Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Windows Phone 8-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.

## <a name="create-your-notification-hub"></a>Az értesítési központ létrehozása
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Hello kattintson <b>értesítési szolgáltatások</b> szakasz (belül <i>beállítások</i>), kattintson a <b>Windows Phone (MPNS)</b> majd hello <b>nem hitelesített leküldés engedélyezése </b> jelölőnégyzetet.</p>
</li>
</ol>

&emsp;&emsp;![Azure portál – Nem hitelesített leküldéses értesítések engedélyezése](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

A központ már létrehozott és beállított toosend nem hitelesített értesítését a Windows Phone.

> [!NOTE]
> Ez az oktatóanyag nem hitelesített módban használja az MPNS-t. MPNS nem hitelesített módja korlátozásokat tartalmaz az, hogy tooeach csatorna küldhet értesítéseket. A Notification Hubs támogatja [MPNS hitelesített módját](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) tooupload lehetővé a tanúsítványt.
> 
> 

## <a name="connecting-your-app-toohello-notification-hub"></a>Kapcsolódás az alkalmazás toohello értesítési központ
1. Hozzon létre egy új Windows Phone 8-alkalmazást a Visual Studióban.
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    A Visual Studio 2013 Update 2 vagy újabb verzióban Windows Phone Silverlight-alkalmazást hozzon létre.
   
    ![Visual Studio – Új projekt – Üres alkalmazás – Windows Phone Silverlight][11]
2. A Visual Studióban, kattintson a jobb gombbal a hello megoldás, és kattintson **NuGet-csomagok kezelése**.
   
    Ez megjeleníti a hello **NuGet-csomagok kezelése** párbeszédpanel megnyitásához.
3. Keresse meg `WindowsAzure.Messaging.Managed` kattintson **telepítése**, és fogadja el a használati feltételek hello.
   
    ![Visual Studio – NuGet Package Manager (NuGet-csomagkezelő)][20]
   
    Ez letölti, telepíti, valamint egy hivatkozást toohello Azure üzenetküldési kódtárat hozzáadja a Windows hello segítségével <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet-csomag</a>.
4. Nyissa meg az App.xaml.cs hello fájlt, és adja hozzá a következő hello `using` utasításokat:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. Adja hozzá a következő kódot a hello felső hello **Application_Launching** metódus App.xaml.cs fájlban:
   
        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }
   
        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
   
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });
   
   > [!NOTE]
   > érték hello **MyPushChannel** értéke egy index, amely egy létező csatorna a hello használt toolookup [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) gyűjtemény. Amennyiben nem létezik ott ilyen, hozzon létre egy bejegyzést ezen a néven.
   > 
   > 
   
    Ellenőrizze, hogy tooinsert hello nevét a hub és hello kapcsolati karakterlánc neve **DefaultListenSharedAccessSignature** hello előző szakaszban beszerzett.
    Ezt a kódot hello csatorna URI azonosítóját hello alkalmazás lekéri az mpns-ből, majd regisztrálja a csatorna URI Azonosítóját az értesítési központban. Emellett biztosítja azt, hogy hello csatorna URI azonosítója legyen regisztrálva az értesítési központ minden alkalommal hello alkalmazás lett elindítva.
   
   > [!NOTE]
   > Ez az oktatóanyag egy bejelentési értesítés toohello eszköz küld. Amikor csempeértesítést küld kell meghívnia hello **BindToShellTile** hello csatorna metódust. toosupport bejelentési és a csempe értesítések hívható meg mindkét **BindToShellTile** és **BindToShellToast**.
   > 
   > 
6. A Megoldáskezelőben bontsa ki a **tulajdonságok**, nyissa meg hello `WMAppManifest.xml` fájlt, kattintson a hello **képességek** lapot, és győződjön meg arról, hogy hello **ID_CAP_PUSH_NOTIFICATION** funkció be van jelölve.
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt toosend a push notification toohello app will fail.
7. Nyomja le az hello `F5` kulcs toorun hello alkalmazást.
   
    Hello app egy regisztrációs üzenet jelenik meg.
8. Bezárás hello alkalmazást.  
   
   > [!NOTE]
   > a bejelentési leküldéses értesítések tooreceive, hello alkalmazás nem futhat hello előtérben.
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a>Leküldéses értesítések küldése a háttérrendszerből
Leküldéses értesítéseket küldhet a Notification Hubs használatával bármilyen háttérrendszerből keresztül hello nyilvános <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-felület</a>. Ebben az oktatóanyagban leküldéses értesítéseket küld egy .NET-konzolalkalmazás használatával. 

Például egy toosend a leküldéses értesítések a Notification Hubs szolgáltatással integrált ASP.NET WebAPI háttérrendszerből, lásd: [Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Például hogyan toosend leküldéses értesítések segítségével hello [REST API-k](https://msdn.microsoft.com/library/azure/dn223264.aspx), tekintse meg [hogyan toouse Notification Hubs Java](notification-hubs-java-push-notification-tutorial.md) és [hogyan toouse php-ből a Notification Hubs](notification-hubs-php-push-notification-tutorial.md) .

1. Kattintson a jobb gombbal hello megoldás, jelölje be **Hozzáadás** és **új projekt...** , majd a **Visual C#**, kattintson a **Windows** és **Konzolalkalmazás**, és kattintson a **OK**.
   
       ![Visual Studio - New Project - Console Application][6]
   
    Ezzel hozzáad egy új Visual C# konzol toohello megoldás. Ezt egy külön megoldásban is megteheti.
2. Kattintson az **Eszközök**, a **Library Package Manager** (Kódtár-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.
   
    Ekkor megjelenik a Package Manager Console hello.
3. A hello **Csomagkezelő konzol** ablakban, a set hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.
4. Nyissa meg hello `Program.cs` fájlt, és adja hozzá a következő hello `using` utasítást:
   
        using Microsoft.Azure.NotificationHubs;
5. A hello `Program` osztály, adja hozzá a következő metódus hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }
   
    Győződjön meg arról, hogy tooreplace hello `<hub name>` helyőrzőt hello hello értesítési központ hello portálon megjelenő nevét. Továbbá cserélje hello kapcsolati karakterlánc helyőrzőjét nevű kapcsolati karakterláncra hello **DefaultFullSharedAccessSignature** beszerzett hello szakasz "Az értesítési központ konfigurálása".
   
   > [!NOTE]
   > Győződjön meg arról, hogy használja-e hello kapcsolati karakterlánc **teljes** fér hozzá, nem **figyelésére** hozzáférést. hello figyelési-hozzáférési karakterlánc nem rendelkezik engedélyekkel toosend leküldéses értesítéseket.
   > 
   > 
6. Adja hozzá a következő sort a hello a `Main` módszert:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Az a Windows Phone-emulátort, és az alkalmazás lezárt, beállíthatja hello Konzolalkalmazás-projektet, hello alapértelmezett kezdőprojektként, és nyomja hello `F5` kulcs toorun hello alkalmazást.
   
    Egy bejelentési leküldéses értesítést fog kapni. Hello bejelentési szalagcímre kattintva betölti a hello alkalmazást.

Minden hello lehetséges hasznos adatot megtalálja az hello [bejelentéskatalógussal] és [csempekatalógussal] témakörök az MSDN Webhelyén.

## <a name="next-steps"></a>Következő lépések
Ez az egyszerű példában küldött leküldéses értesítések tooall a Windows Phone 8 rendszerű eszközökön. 

A rendezés tootarget adott felhasználókat, tekintse meg a toohello [Notification Hubs használata toopush értesítések toousers] oktatóanyag. 

Ha azt szeretné, toosegment a felhasználókat érdeklődési körök, olvasható [legfrissebb hírek Notification Hubs használata toosend]. 

További tudnivalók toouse értesítési központok [Notification Hubs használatával].

<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Windows Phone-hoz készült Visual Studio 2012 Express]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Notification Hubs használata toopush értesítések toousers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[bejelentéskatalógussal]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[csempekatalógussal]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Notification Hubs – Windows Phone Silverlight-oktatóanyagot]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

