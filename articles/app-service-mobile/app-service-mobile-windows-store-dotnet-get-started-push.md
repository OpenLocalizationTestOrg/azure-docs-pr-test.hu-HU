---
title: "aaaAdd leküldéses értesítések tooyour univerzális Windows Platform (UWP-) alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure App Service Mobile Apps és az Azure Notification Hubs toosend leküldéses értesítések tooyour univerzális Windows Platform (UWP) alkalmazást."
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 378ce59cab974830c0a3801108b24b30a21ae5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-windows-app"></a>Leküldéses értesítések tooyour Windows-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Windows gyors üzembe helyezési](app-service-mobile-windows-store-dotnet-get-started.md) projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.

Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában. Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.

## <a name="configure-hub"></a>Egy értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Alkalmazás regisztrálása leküldéses értesítésekhez
Toosubmit kell az alkalmazás toohello Windows áruház, majd konfigurálja a kiszolgáló projekt toointegrate Windows értesítési szolgáltatások (WNS) toosend leküldéses.

1. A Visual Studio Solution Explorerben, kattintson a jobb gombbal hello UWP-alkalmazás projekt, kattintson a **tároló** > **hello Áruházbeli alkalmazás társítása...** .

    ![Windows Áruházbeli alkalmazás társítása](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. Hello varázslóban kattintson **következő**, jelentkezzen be Microsoft-fiókjával, adjon meg egy nevet az alkalmazáshoz a **lefoglalni egy új alkalmazás neve**, majd kattintson a **tartalék**.
3. Sikeresen létrejött, jelölje be hello új alkalmazásnév hello alkalmazás regisztrálása után kattintson **következő**, és kattintson a **társítása**. Ez biztosítja a szükséges hello Windows Áruházbeli regisztrációs adatokat toohello alkalmazásjegyzék.  
4. Keresse meg a toohello [Windows fejlesztői központ](https://dev.windows.com/en-us/overview)Bejelentkezés Microsoft-fiókjával, kattintson hello új alkalmazás regisztrációs **alkalmazásaimat az**, majd bontsa ki a **szolgáltatások**  >  **Leküldéses értesítések**.
5. A hello **leküldéses értesítések** kattintson **Live Services webhely** alatt **Microsoft Azure Mobile Services**.
6. Hello regisztrációs lapon jegyezze fel a hello értéket **alkalmazás titkos kulcsok** és hello **CSOMAGAZONOSÍTÓT**, amely ezután használhatja tooconfigure a mobil-háttéralkalmazás.

    ![Windows Áruházbeli alkalmazás társítása](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > hello titkos ügyfélkulcs és a csomag biztonsági AZONOSÍTÓJÁT is fontos biztonsági hitelesítő adatok. Ezeket az értékeket ne ossza meg senkivel, és ne terjessze az alkalmazással. Hello **alkalmazásazonosító** hello titkos tooconfigure Microsoft Account hitelesítéssel szolgál.
   >
   >

## <a name="configure-hello-backend-toosend-push-notifications"></a>Hello háttér toosend leküldéses értesítések konfigurálása
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a id="update-service"></a>Frissítési hello server toosend leküldéses értesítések
Hello alábbi eljárással az backend project típusának megfelelő&mdash;vagy [.NET-háttérrendszer](#dotnet) vagy [Node.js-háttéralkalmazáshoz](#nodejs).

### <a name="dotnet"></a>.NET-háttéralkalmazás-projekt
1. A Visual Studióban, kattintson a jobb gombbal a projekt hello, és kattintson **NuGet-csomagok kezelése**, Microsoft.Azure.NotificationHubs keresni, majd kattintson a **telepítése**. Ezzel telepít hello Notification Hubs ügyféloldali kódtárára.
2. Bontsa ki a **tartományvezérlők**, nyissa meg a TodoItemController.cs, és adja hozzá hello következő using utasításokat:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. A hello **PostTodoItem** módszer, vegye fel a kód túl hello hívása után következő hello**InsertAsync**:

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create hello notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send hello push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write hello success result toohello logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write hello failure result toohello logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Ez a kód be van állítva hello notification hub toosend leküldéses értesítés után egy új cikk beszúrási.
4. Hello kiszolgálóprojektet közzé.

### <a name="nodejs"></a>NODE.js háttérrendszer-projekt
1. Ha még nem tette meg, [hello gyorsútmutató-projekt letöltése](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , vagy ellenkező esetben használja a hello [hello Azure-portálon az online szerkesztő](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Cserélje le a meglévő kódot hello hello todoitem.js fájlban hello következőre:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello WNS payload that contains hello new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget tooreturn hello results from hello context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Ez egy új teendőelemet behelyezésekor hello item.text tartalmazó WNS bejelentési értesítést küld.
3. A helyi számítógépen hello fájl szerkesztésekor közzé hello kiszolgálóprojektet.

## <a id="update-app"></a>Leküldéses értesítések tooyour alkalmazás hozzáadása
Az alkalmazás ezután a leküldéses értesítések indításkor kell regisztrálni. Ha már hitelesítés engedélyezve van, győződjön meg arról, hello felhasználó jelentkezik be a leküldéses értesítések tooregister előtt.

1. Nyissa meg hello **App.xaml.cs** le a fájlt, és adja hozzá a következő hello `using` utasításokat:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. A hello ugyanazt a fájlt, adja hozzá a következő hello **InitNotificationsAsync** metódus definícióját toohello **App** osztály:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register hello channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Ez a kód hello ChannelURI hello alkalmazás lekéri a wns-ből, és majd regisztrálja a ChannelURI az App Service Mobile Apps.
3. Hello hello tetején **OnLaunched** az eseménykezelő **App.xaml.cs**, adja hozzá a hello **aszinkron** módosító toohello metódusdefiníciót és hello következő hívás toohello új hozzáadása **InitNotificationsAsync** módszer, mint például a következő hello:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Ez biztosítja, hogy adott hello rövid élettartamú ChannelURI regisztrálva van-e hello alkalmazás minden indításakor.
4. Az UWP-alkalmazás projekt újraépítése. Az alkalmazás mostantól készen áll a tooreceive bejelentési értesítést.

## <a id="test"></a>Teszt leküldéses értesítések az alkalmazásban
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a id="more"></a>Következő lépések
További tudnivalók a leküldéses értesítések:

* [Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  Sablonok segítenek rugalmasságot toosend platformok közötti leküldéses értesítések és honosított leküldéses értesítések. Megtudhatja, hogyan tooregister sablonok.
* [Leküldéses értesítési eseményadatokat](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Oka különböző miért kerülhetnek vagy értesítések nem végül az eszközökön. Ez a témakör bemutatja, hogyan tooanalyze és vizsgálhatja meg hello legfelső szintű okozhatják a leküldéses értesítés sikertelen.

Vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:

* [Hitelesítési tooyour alkalmazás hozzáadása](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.
* [Az offline szinkronizálás engedélyezése az alkalmazás számára](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Ismerje meg, hogyan támogatják a kapcsolat nélküli tooadd a az alkalmazás egy Mobile Apps-háttéralkalmazás segítségével. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
