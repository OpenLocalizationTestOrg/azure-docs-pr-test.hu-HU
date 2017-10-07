---
title: "aaaGet Azure Notification Hubs Windows Universal Platform alkalmazásokkal való használatába |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toopush értesítések tooa univerzális Windows Platform-alkalmazás."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: erikre
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 11056842d205522ed493dc61c76ecf78ebb5a363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a>Ismerkedés a Notification Hubs univerzális platformon futó Windows-alkalmazásokkal történő használatával
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa univerzális Windows Platform (UWP) alkalmazást.

Ebben az oktatóanyagban hozzon létre egy üres Windows Áruházbeli alkalmazást, amely leküldéses értesítéseket fogad hello Windows leküldéses értesítési szolgáltatásának (WNS) használatával. Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.

## <a name="before-you-begin"></a>Előkészületek
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

az oktatóanyag befejezése hello kódja a Githubon található [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hello következő szükséges:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) vagy újabb
* [Az univerzális Windows-alkalmazások fejlesztőeszközei telepítve vannak](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Aktív Azure-fiók <br/>Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Aktív Windows Áruházbeli fiók

Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, univerzális platformon futó Windows-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.

## <a name="register-your-app-for-hello-windows-store"></a>Hello Windows Áruházbeli alkalmazás regisztrálása
toosend leküldéses értesítések tooUWP alkalmazások, társítania kell az alkalmazás toohello Windows Store. A notification hub toointegrate WNS majd be kell állítania.

1. Ha már nincs regisztrálva az alkalmazás, keresse meg a toohello [Windows fejlesztői központ](https://dev.windows.com/overview), jelentkezzen be Microsoft-fiókjával, és kattintson a **hozzon létre egy új alkalmazást**.

2. Írja be az alkalmazás nevét, majd kattintson az **Alkalmazásnév lefoglalása** parancsra. Ezzel létrehoz egy új Windows Áruházbeli regisztrációt az alkalmazás számára.

3. A Visual Studio, hozzon létre egy új Visual C# Áruházbeli alkalmazások projektet az univerzális Windows hello segítségével **üres alkalmazás** sablont, és kattintson **OK**.

4. Fogadja el a hello alapértelmezett hello cél- és minimális platformverziók értéket.

5. A Megoldáskezelőben kattintson a jobb gombbal hello Windows Áruházbeli alkalmazás projektjére, kattintson a **tároló**, és kattintson a **hello Áruházbeli alkalmazás társítása...** . hello **társítsa az alkalmazás hello Windows áruház** varázsló jelenik meg.

6. Hello varázslót jelentkezzen be a Microsoft-fiókjával.

7. Hello regisztrált alkalmazásra, a 2. lépésben kattintson, majd **következő**, és kattintson a **társítása**. Ez biztosítja a szükséges hello Windows Áruházbeli regisztrációs adatokat toohello alkalmazásjegyzék.

8. Vissza a hello [Windows fejlesztői központ](http://dev.windows.com/overview) az új alkalmazás lapján kattintson **szolgáltatások**, kattintson a **leküldéses értesítések**, és kattintson a **WNS/MPNS**.

9. Kattintson az **New Notification** (Új értesítés) elemre.

10. Kattintson az **Blank (Toast)** (Üres (Bejelentési)) sablonra, majd kattintson az **OK** gombra.

11. Adja meg az értesítés **nevét** és egy vizualizáció **környezetfüggő** üzenetet. Ezután kattintson a **Save as draft** (Mentés vázlatként) gombra.

12. Keresse meg a toohello [alkalmazásregisztrációs portálra](http://apps.dev.microsoft.com) , és jelentkezzen be.

13. Kattintson az alkalmazás nevére. Jegyezze fel a hello **Alkalmazáskulcsot** jelszó és hello **csomag biztonsági azonosítóját (SID)** hello található **Windows áruház** platform beállításait.

     > [AZURE.WARNING]
    hello alkalmazás titkos kulcs és a csomag biztonsági AZONOSÍTÓJÁT is fontos biztonsági hitelesítő adatokat. Ezeket az értékeket ne ossza meg senkivel, és ne terjessze az alkalmazással.

## <a name="configure-your-notification-hub"></a>Az értesítési központ konfigurálása
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Jelölje be hello <b>értesítési szolgáltatások</b> beállítás és hello <b>Windows (WNS)</b> lehetőséget. Majd adja meg a hello <b>alkalmazáskulcsot</b> hello jelszót <b>biztonsági kulcs</b> mező. Adja meg a <b>CSOMAGAZONOSÍTÓT</b> érték, amely a wns-ből hello előző szakaszban beszerzett, és kattintson a <b>mentése</b>.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Az értesítési központ most már konfigurált toowork WNS-sel való, és rendelkezik hello kapcsolati karakterláncok tooregister az alkalmazás és az értesítések küldéséhez.

## <a name="connect-your-app-toohello-notification-hub"></a>Csatlakozás az alkalmazás toohello értesítési központ
1. A Visual Studióban, kattintson a jobb gombbal a hello megoldás, és kattintson **NuGet-csomagok kezelése**.
   
    Ez megjeleníti a hello **NuGet-csomagok kezelése** párbeszédpanel megnyitásához.
2. Keresse meg `WindowsAzure.Messaging.Managed` kattintson **telepítése**, és el kell fogadnia a használati feltételek hello.
   
    ![][20]
   
    Ez letölti, telepíti, valamint egy hivatkozást toohello Azure üzenetküldési kódtárat hozzáadja a Windows hello segítségével <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet-csomag</a>.
3. Nyissa meg a hello App.xaml.cs projektfájlt, és adja hozzá a következő hello `using` utasításokat. 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. Is App.xaml.cs fájlban adja hozzá a hello következő **InitNotificationsAsync** metódus definícióját toohello **App** osztály:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("< your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Ezt a kódot hello csatorna URI azonosítóját hello alkalmazás lekéri a wns-ből, majd regisztrálja a csatorna URI Azonosítóját az értesítési központban.
   
   > [!NOTE]
   > Győződjön meg arról, hogy tooreplace hello "a hub name" helyőrzőt hello hello értesítési központ hello Azure portálon megjelenő nevét. Cserélje le hello kapcsolati karakterlánc helyőrzőjét hello **DefaultListenSharedAccessSignature** hello beszerzett kapcsolati karakterlánc **hozzáférési szabályzatok** az értesítési központ lapján egy előző szakaszban.
   > 
   > 
5. Hello hello tetején **OnLaunched** eseménykezelő App.xaml.cs fájlban adja hozzá a következő új hívás toohello hello **InitNotificationsAsync** módszert:
   
        InitNotificationsAsync();
   
    Ez biztosítja, hogy a hello csatorna URI azonosítója legyen regisztrálva az értesítési központ minden alkalommal hello alkalmazás lett elindítva.
6. Nyomja le az hello **F5** kulcs toorun hello alkalmazást. Hello regisztrációs kulcs egy felugró párbeszédpanel jelenik meg.

Az alkalmazás mostantól készen áll a tooreceive bejelentési értesítést.

## <a name="send-notifications"></a>Értesítések küldése
Gyorsan tesztelheti értesítések fogadásának az alkalmazásban való értesítések a hello [Azure Portal](https://portal.azure.com/) hello segítségével **küldés tesztelése** hello értesítési központot, ahogy az üdvözlő képernyőt az alábbi gombra.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET. Közvetlen toosend értesítési üzenetekben, ha a szalagtár nem érhető el a háttér-hello REST API-t is használható. 

Ebben az oktatóanyagban a rendszer egyszerűen és csak az ügyfélalkalmazás tesztelését küldött értesítésekkel hello .NET SDK használatával egy háttér-szolgáltatás helyett egy konzolalkalmazás értesítési központjának bemutatása. Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers] oktatóanyagot hello következő lépés az értesítéseknek ASP.NET-háttérrendszerből történő küldéséhez. A következő módszerekkel hello azonban az értesítések küldésével használhatók:

* **REST-felület**: bármely háttér platformon hello segítségével támogathatja az értesítéseket [REST-felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **A Microsoft Azure Notification Hubs .NET SDK**: hello Nuget-Csomagkezelőt a Visual Studio, a futtasson [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **NODE.js** : [hogyan toouse Notification Hubs Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Az Azure Mobile Apps**: A példa bemutatja, hogyan toosend értesítések a Notification Hubs szolgáltatással integrált Azure Mobile Apps lásd [leküldéses értesítések hozzáadása Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java / PHP**: hogyan toosend értesítések használatával hello REST API-k példát lásd: "hogyan toouse Notification Hubs Java/php-ből" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-console-app"></a>(Választható) Értesítések küldése konzolalkalmazásból
toosend értesítések .NET-Konzolalkalmazás használatával kövesse az alábbi lépéseket. 

1. Kattintson a jobb gombbal hello megoldás, jelölje be **Hozzáadás** és **új projekt...** , majd a **Visual C#**, kattintson a **Windows** és **Konzolalkalmazás**, és kattintson a **OK**.
   
    Ezzel hozzáad egy új Visual C# konzol toohello megoldás. Ezt egy külön megoldásban is megteheti.

2. A Visual Studióban kattintson az **Eszközök**, a **NuGet Package Manager** (NuGet-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.
   
    Ez a Visual Studio hello Csomagkezelő konzol jeleníti meg.
3. Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Nyissa meg hello Program.cs fájlt, és adja hozzá a következő hello `using` utasítást:
   
        using Microsoft.Azure.NotificationHubs;
5. A hello **Program** osztály, adja hozzá a következő metódus hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure tooreplace hello "hub name" placeholder with hello name of hello notification hub that as it appears in hello Azure Portal. Also, replace hello connection string placeholder with hello **DefaultFullSharedAccessSignature** connection string that you obtained from hello **Access Policies** page of your Notification Hub in hello section called "Configure your notification hub."
   
   > [!NOTE]
   > Győződjön meg arról, hogy használja-e hello kapcsolati karakterláncot, amelynek **teljes** fér hozzá, nem **figyelésére** hozzáférést. hello figyelési-hozzáférési karakterlánc nem rendelkezik engedélyekkel toosend értesítések.
   > 
   > 
6. Adja hozzá a következő sorokat hello hello **fő** módszert:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Kattintson a jobb gombbal a hello Konzolalkalmazás-projektet a Visual Studióban, és kattintson a **beállítás kezdőprojektként** tooset hello indítási projektként azt. Nyomja le az hello **F5** toorun hello alkalmazás kulcsát.
   
    Egy bejelentési értesítést fog kapni az összes regisztrált eszközön. Hello bejelentési szalagcímre kattintva vagy koppintva betölti hello alkalmazást.

Hello található összes hello támogatott hasznos adatot [bejelentéskatalógussal], [csempekatalógussal], és [jelvények áttekintésével] témakörök az MSDN Webhelyén.

## <a name="next-steps"></a>Következő lépések
Ez az egyszerű példában küldött értesítéseket tooall a Windows-eszközök hello portál vagy egy konzolalkalmazás használatával. Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers] oktatóanyagot hello következő lépésre. Azt láthatja, hogyan toosend értesítést kapnak az ASP.NET háttérkiszolgáló használatával címkéket tootarget adott felhasználókra.

Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, lásd: [legfrissebb hírek Notification Hubs használata toosend]. 

toolearn Notification Hubs további általános információt lásd: [Notification Hubs használatával](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Notification Hubs használata toopush értesítések toousers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[bejelentéskatalógussal]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[csempekatalógussal]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[jelvények áttekintésével]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
