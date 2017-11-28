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
# <a name="add-push-notifications-tooyour-windows-app"></a><span data-ttu-id="b7005-103">Leküldéses értesítések tooyour Windows-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7005-103">Add push notifications tooyour Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="b7005-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b7005-104">Overview</span></span>
<span data-ttu-id="b7005-105">Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Windows gyors üzembe helyezési](app-service-mobile-windows-store-dotnet-get-started.md) projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="b7005-105">In this tutorial, you add push notifications toohello [Windows quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="b7005-106">Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában.</span><span class="sxs-lookup"><span data-stu-id="b7005-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="b7005-107">Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="b7005-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <span data-ttu-id="b7005-108"><a name="configure-hub"></a>Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7005-108"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="b7005-109">Alkalmazás regisztrálása leküldéses értesítésekhez</span><span class="sxs-lookup"><span data-stu-id="b7005-109">Register your app for push notifications</span></span>
<span data-ttu-id="b7005-110">Toosubmit kell az alkalmazás toohello Windows áruház, majd konfigurálja a kiszolgáló projekt toointegrate Windows értesítési szolgáltatások (WNS) toosend leküldéses.</span><span class="sxs-lookup"><span data-stu-id="b7005-110">You need toosubmit your app toohello Windows Store, then configure your server project toointegrate with Windows Notification Services (WNS) toosend push.</span></span>

1. <span data-ttu-id="b7005-111">A Visual Studio Solution Explorerben, kattintson a jobb gombbal hello UWP-alkalmazás projekt, kattintson a **tároló** > **hello Áruházbeli alkalmazás társítása...** .</span><span class="sxs-lookup"><span data-stu-id="b7005-111">In Visual Studio Solution Explorer, right-click hello UWP app project, click **Store** > **Associate App with hello Store...**.</span></span>

    ![Windows Áruházbeli alkalmazás társítása](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. <span data-ttu-id="b7005-113">Hello varázslóban kattintson **következő**, jelentkezzen be Microsoft-fiókjával, adjon meg egy nevet az alkalmazáshoz a **lefoglalni egy új alkalmazás neve**, majd kattintson a **tartalék**.</span><span class="sxs-lookup"><span data-stu-id="b7005-113">In hello wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="b7005-114">Sikeresen létrejött, jelölje be hello új alkalmazásnév hello alkalmazás regisztrálása után kattintson **következő**, és kattintson a **társítása**.</span><span class="sxs-lookup"><span data-stu-id="b7005-114">After hello app registration is successfully created, select hello new app name, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="b7005-115">Ez biztosítja a szükséges hello Windows Áruházbeli regisztrációs adatokat toohello alkalmazásjegyzék.</span><span class="sxs-lookup"><span data-stu-id="b7005-115">This adds hello required Windows Store registration information toohello application manifest.</span></span>  
4. <span data-ttu-id="b7005-116">Keresse meg a toohello [Windows fejlesztői központ](https://dev.windows.com/en-us/overview)Bejelentkezés Microsoft-fiókjával, kattintson hello új alkalmazás regisztrációs **alkalmazásaimat az**, majd bontsa ki a **szolgáltatások**  >  **Leküldéses értesítések**.</span><span class="sxs-lookup"><span data-stu-id="b7005-116">Navigate toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), sign-in with your Microsoft account, click hello new app registration in **My apps**, then expand **Services** > **Push notifications**.</span></span>
5. <span data-ttu-id="b7005-117">A hello **leküldéses értesítések** kattintson **Live Services webhely** alatt **Microsoft Azure Mobile Services**.</span><span class="sxs-lookup"><span data-stu-id="b7005-117">In hello **Push notifications** page, click **Live Services site** under **Microsoft Azure Mobile Services**.</span></span>
6. <span data-ttu-id="b7005-118">Hello regisztrációs lapon jegyezze fel a hello értéket **alkalmazás titkos kulcsok** és hello **CSOMAGAZONOSÍTÓT**, amely ezután használhatja tooconfigure a mobil-háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b7005-118">In hello registration page, make a note of hello value under **Application secrets** and hello **Package SID**, which you will next use tooconfigure your mobile app backend.</span></span>

    ![Windows Áruházbeli alkalmazás társítása](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > <span data-ttu-id="b7005-120">hello titkos ügyfélkulcs és a csomag biztonsági AZONOSÍTÓJÁT is fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="b7005-120">hello client secret and package SID are important security credentials.</span></span> <span data-ttu-id="b7005-121">Ezeket az értékeket ne ossza meg senkivel, és ne terjessze az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="b7005-121">Do not share these values with anyone or distribute them with your app.</span></span> <span data-ttu-id="b7005-122">Hello **alkalmazásazonosító** hello titkos tooconfigure Microsoft Account hitelesítéssel szolgál.</span><span class="sxs-lookup"><span data-stu-id="b7005-122">hello **Application Id** is used with hello secret tooconfigure Microsoft Account authentication.</span></span>
   >
   >

## <a name="configure-hello-backend-toosend-push-notifications"></a><span data-ttu-id="b7005-123">Hello háttér toosend leküldéses értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7005-123">Configure hello backend toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <span data-ttu-id="b7005-124"><a id="update-service"></a>Frissítési hello server toosend leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="b7005-124"><a id="update-service"></a>Update hello server toosend push notifications</span></span>
<span data-ttu-id="b7005-125">Hello alábbi eljárással az backend project típusának megfelelő&mdash;vagy [.NET-háttérrendszer](#dotnet) vagy [Node.js-háttéralkalmazáshoz](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="b7005-125">Use hello procedure below that matches your backend project type&mdash;either [.NET backend](#dotnet) or [Node.js backend](#nodejs).</span></span>

### <span data-ttu-id="b7005-126"><a name="dotnet"></a>.NET-háttéralkalmazás-projekt</span><span class="sxs-lookup"><span data-stu-id="b7005-126"><a name="dotnet"></a>.NET backend project</span></span>
1. <span data-ttu-id="b7005-127">A Visual Studióban, kattintson a jobb gombbal a projekt hello, és kattintson **NuGet-csomagok kezelése**, Microsoft.Azure.NotificationHubs keresni, majd kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="b7005-127">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**, search for Microsoft.Azure.NotificationHubs, then click **Install**.</span></span> <span data-ttu-id="b7005-128">Ezzel telepít hello Notification Hubs ügyféloldali kódtárára.</span><span class="sxs-lookup"><span data-stu-id="b7005-128">This installs hello Notification Hubs client library.</span></span>
2. <span data-ttu-id="b7005-129">Bontsa ki a **tartományvezérlők**, nyissa meg a TodoItemController.cs, és adja hozzá hello következő using utasításokat:</span><span class="sxs-lookup"><span data-stu-id="b7005-129">Expand **Controllers**, open TodoItemController.cs, and add hello following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="b7005-130">A hello **PostTodoItem** módszer, vegye fel a kód túl hello hívása után következő hello**InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="b7005-130">In hello **PostTodoItem** method, add hello following code after hello call too**InsertAsync**:</span></span>

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

    <span data-ttu-id="b7005-131">Ez a kód be van állítva hello notification hub toosend leküldéses értesítés után egy új cikk beszúrási.</span><span class="sxs-lookup"><span data-stu-id="b7005-131">This code tells hello notification hub toosend a push notification after a new item is insertion.</span></span>
4. <span data-ttu-id="b7005-132">Hello kiszolgálóprojektet közzé.</span><span class="sxs-lookup"><span data-stu-id="b7005-132">Republish hello server project.</span></span>

### <span data-ttu-id="b7005-133"><a name="nodejs"></a>NODE.js háttérrendszer-projekt</span><span class="sxs-lookup"><span data-stu-id="b7005-133"><a name="nodejs"></a>Node.js backend project</span></span>
1. <span data-ttu-id="b7005-134">Ha még nem tette meg, [hello gyorsútmutató-projekt letöltése](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , vagy ellenkező esetben használja a hello [hello Azure-portálon az online szerkesztő](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="b7005-134">If you haven't already done so, [download hello quickstart project](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) or else use hello [online editor in hello Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="b7005-135">Cserélje le a meglévő kódot hello hello todoitem.js fájlban hello következőre:</span><span class="sxs-lookup"><span data-stu-id="b7005-135">Replace hello existing code in hello todoitem.js file with hello following:</span></span>

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

    <span data-ttu-id="b7005-136">Ez egy új teendőelemet behelyezésekor hello item.text tartalmazó WNS bejelentési értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="b7005-136">This sends a WNS toast notification that contains hello item.text when a new todo item is inserted.</span></span>
3. <span data-ttu-id="b7005-137">A helyi számítógépen hello fájl szerkesztésekor közzé hello kiszolgálóprojektet.</span><span class="sxs-lookup"><span data-stu-id="b7005-137">When editing hello file on your local computer, republish hello server project.</span></span>

## <span data-ttu-id="b7005-138"><a id="update-app"></a>Leküldéses értesítések tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7005-138"><a id="update-app"></a>Add push notifications tooyour app</span></span>
<span data-ttu-id="b7005-139">Az alkalmazás ezután a leküldéses értesítések indításkor kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="b7005-139">Next, your app must register for push notifications on start-up.</span></span> <span data-ttu-id="b7005-140">Ha már hitelesítés engedélyezve van, győződjön meg arról, hello felhasználó jelentkezik be a leküldéses értesítések tooregister előtt.</span><span class="sxs-lookup"><span data-stu-id="b7005-140">When you have already enabled authentication, make sure that hello user signs-in before trying tooregister for push notifications.</span></span>

1. <span data-ttu-id="b7005-141">Nyissa meg hello **App.xaml.cs** le a fájlt, és adja hozzá a következő hello `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="b7005-141">Open hello **App.xaml.cs** project file and add hello following `using` statements:</span></span>

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. <span data-ttu-id="b7005-142">A hello ugyanazt a fájlt, adja hozzá a következő hello **InitNotificationsAsync** metódus definícióját toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="b7005-142">In hello same file, add hello following **InitNotificationsAsync** method definition toohello **App** class:</span></span>

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register hello channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    <span data-ttu-id="b7005-143">Ez a kód hello ChannelURI hello alkalmazás lekéri a wns-ből, és majd regisztrálja a ChannelURI az App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="b7005-143">This code retrieves hello ChannelURI for hello app from WNS, and then registers that ChannelURI with your App Service Mobile App.</span></span>
3. <span data-ttu-id="b7005-144">Hello hello tetején **OnLaunched** az eseménykezelő **App.xaml.cs**, adja hozzá a hello **aszinkron** módosító toohello metódusdefiníciót és hello következő hívás toohello új hozzáadása **InitNotificationsAsync** módszer, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b7005-144">At hello top of hello **OnLaunched** event handler in **App.xaml.cs**, add hello **async** modifier toohello method definition and add hello following call toohello new **InitNotificationsAsync** method, as in hello following example:</span></span>

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    <span data-ttu-id="b7005-145">Ez biztosítja, hogy adott hello rövid élettartamú ChannelURI regisztrálva van-e hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="b7005-145">This guarantees that hello short-lived ChannelURI is registered each time hello application is launched.</span></span>
4. <span data-ttu-id="b7005-146">Az UWP-alkalmazás projekt újraépítése.</span><span class="sxs-lookup"><span data-stu-id="b7005-146">Rebuild your UWP app project.</span></span> <span data-ttu-id="b7005-147">Az alkalmazás mostantól készen áll a tooreceive bejelentési értesítést.</span><span class="sxs-lookup"><span data-stu-id="b7005-147">Your app is now ready tooreceive toast notifications.</span></span>

## <span data-ttu-id="b7005-148"><a id="test"></a>Teszt leküldéses értesítések az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="b7005-148"><a id="test"></a>Test push notifications in your app</span></span>
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <span data-ttu-id="b7005-149"><a id="more"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7005-149"><a id="more"></a>Next steps</span></span>
<span data-ttu-id="b7005-150">További tudnivalók a leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="b7005-150">Learn more about push notifications:</span></span>

* [<span data-ttu-id="b7005-151">Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél</span><span class="sxs-lookup"><span data-stu-id="b7005-151">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  <span data-ttu-id="b7005-152">Sablonok segítenek rugalmasságot toosend platformok közötti leküldéses értesítések és honosított leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="b7005-152">Templates give you flexibility toosend cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="b7005-153">Megtudhatja, hogyan tooregister sablonok.</span><span class="sxs-lookup"><span data-stu-id="b7005-153">Learn how tooregister templates.</span></span>
* [<span data-ttu-id="b7005-154">Leküldéses értesítési eseményadatokat</span><span class="sxs-lookup"><span data-stu-id="b7005-154">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="b7005-155">Oka különböző miért kerülhetnek vagy értesítések nem végül az eszközökön.</span><span class="sxs-lookup"><span data-stu-id="b7005-155">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="b7005-156">Ez a témakör bemutatja, hogyan tooanalyze és vizsgálhatja meg hello legfelső szintű okozhatják a leküldéses értesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="b7005-156">This topic shows you how tooanalyze and figure out hello root cause of push notification failures.</span></span>

<span data-ttu-id="b7005-157">Vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:</span><span class="sxs-lookup"><span data-stu-id="b7005-157">Consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="b7005-158">Hitelesítési tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7005-158">Add authentication tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  <span data-ttu-id="b7005-159">Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="b7005-159">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="b7005-160">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="b7005-160">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="b7005-161">Ismerje meg, hogyan támogatják a kapcsolat nélküli tooadd a az alkalmazás egy Mobile Apps-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="b7005-161">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="b7005-162">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b7005-162">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
