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
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a><span data-ttu-id="c61b3-104">Leküldéses értesítések küldése az Azure Notification Hubs használatával Windows Phone rendszerű eszközökre</span><span class="sxs-lookup"><span data-stu-id="c61b3-104">Sending push notifications with Azure Notification Hubs on Windows Phone</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="c61b3-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c61b3-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="c61b3-106">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="c61b3-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="c61b3-107">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="c61b3-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c61b3-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="c61b3-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="c61b3-109">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Windows Phone 8 vagy Windows Phone 8.1 Silverlight alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c61b3-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Windows Phone 8 or Windows Phone 8.1 Silverlight application.</span></span> <span data-ttu-id="c61b3-110">Ha a Windows Phone 8.1 (nem Silverlight) céloz meg, majd tekintse meg a toohello [univerzális Windows-](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) verziója.</span><span class="sxs-lookup"><span data-stu-id="c61b3-110">If you are targeting Windows Phone 8.1 (non-Silverlight), then refer toohello [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) version.</span></span>
<span data-ttu-id="c61b3-111">Ebben az oktatóanyagban hozzon létre egy üres Windows Phone 8-alkalmazást, amely leküldéses értesítéseket fogad hello a Microsoft leküldéses értesítési szolgáltatásának (MPNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="c61b3-111">In this tutorial, you create a blank Windows Phone 8 app that receives push notifications by using hello Microsoft Push Notification Service (MPNS).</span></span> <span data-ttu-id="c61b3-112">Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.</span><span class="sxs-lookup"><span data-stu-id="c61b3-112">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

> [!NOTE]
> <span data-ttu-id="c61b3-113">hello Notification Hubs Windows Phone SDK nem támogatja a Windows leküldéses értesítési szolgáltatásának (WNS) hello használata a Windows Phone 8.1 Silverlight-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c61b3-113">hello Notification Hubs Windows Phone SDK does not support using hello Windows Push Notification Service (WNS) with Windows Phone 8.1 Silverlight apps.</span></span> <span data-ttu-id="c61b3-114">Windows Phone 8.1 Silverlight-alkalmazásokkal (az MPNS helyett) WNS toouse kövesse hello [Notification Hubs – Windows Phone Silverlight-oktatóanyagot], amely REST API-kat használ.</span><span class="sxs-lookup"><span data-stu-id="c61b3-114">toouse WNS (instead of MPNS) with Windows Phone 8.1 Silverlight apps, follow hello [Notification Hubs - Windows Phone Silverlight tutorial], which uses REST APIs.</span></span>
> 
> 

<span data-ttu-id="c61b3-115">hello az oktatóanyag bemutatja, hogyan hello egyszerű küldési forgatókönyvet a Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="c61b3-115">hello tutorial demonstrates hello simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c61b3-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c61b3-116">Prerequisites</span></span>
<span data-ttu-id="c61b3-117">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="c61b3-117">This tutorial requires hello following:</span></span>

* <span data-ttu-id="c61b3-118">[Windows Phone-hoz készült Visual Studio 2012 Express] vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="c61b3-118">[Visual Studio 2012 Express for Windows Phone], or a later version.</span></span>

<span data-ttu-id="c61b3-119">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Windows Phone 8-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="c61b3-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Phone 8 apps.</span></span>

## <a name="create-your-notification-hub"></a><span data-ttu-id="c61b3-120">Az értesítési központ létrehozása</span><span class="sxs-lookup"><span data-stu-id="c61b3-120">Create your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="c61b3-121">Hello kattintson <b>értesítési szolgáltatások</b> szakasz (belül <i>beállítások</i>), kattintson a <b>Windows Phone (MPNS)</b> majd hello <b>nem hitelesített leküldés engedélyezése </b> jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c61b3-121">Click hello <b>Notification Services</b> section (within <i>Settings</i>), click on <b>Windows Phone (MPNS)</b> and then click hello <b>Enable unauthenticated push</b> check box.</span></span></p>
</li>
</ol>

&emsp;&emsp;![Azure portál – Nem hitelesített leküldéses értesítések engedélyezése](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

<span data-ttu-id="c61b3-123">A központ már létrehozott és beállított toosend nem hitelesített értesítését a Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="c61b3-123">Your hub is now created and configured toosend unauthenticated notification for Windows Phone.</span></span>

> [!NOTE]
> <span data-ttu-id="c61b3-124">Ez az oktatóanyag nem hitelesített módban használja az MPNS-t.</span><span class="sxs-lookup"><span data-stu-id="c61b3-124">This tutorial uses MPNS in unauthenticated mode.</span></span> <span data-ttu-id="c61b3-125">MPNS nem hitelesített módja korlátozásokat tartalmaz az, hogy tooeach csatorna küldhet értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="c61b3-125">MPNS unauthenticated mode comes with restrictions on notifications that you can send tooeach channel.</span></span> <span data-ttu-id="c61b3-126">A Notification Hubs támogatja [MPNS hitelesített módját](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) tooupload lehetővé a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c61b3-126">Notification Hubs supports [MPNS authenticated mode](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) by allowing you tooupload your certificate.</span></span>
> 
> 

## <a name="connecting-your-app-toohello-notification-hub"></a><span data-ttu-id="c61b3-127">Kapcsolódás az alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="c61b3-127">Connecting your app toohello notification hub</span></span>
1. <span data-ttu-id="c61b3-128">Hozzon létre egy új Windows Phone 8-alkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="c61b3-128">In Visual Studio, create a new Windows Phone 8 application.</span></span>
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    <span data-ttu-id="c61b3-129">A Visual Studio 2013 Update 2 vagy újabb verzióban Windows Phone Silverlight-alkalmazást hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="c61b3-129">In Visual Studio 2013 Update 2 or later, you instead create a Windows Phone Silverlight application.</span></span>
   
    ![Visual Studio – Új projekt – Üres alkalmazás – Windows Phone Silverlight][11]
2. <span data-ttu-id="c61b3-131">A Visual Studióban, kattintson a jobb gombbal a hello megoldás, és kattintson **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c61b3-131">In Visual Studio, right-click hello solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="c61b3-132">Ez megjeleníti a hello **NuGet-csomagok kezelése** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c61b3-132">This displays hello **Manage NuGet Packages** dialog box.</span></span>
3. <span data-ttu-id="c61b3-133">Keresse meg `WindowsAzure.Messaging.Managed` kattintson **telepítése**, és fogadja el a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="c61b3-133">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and then accept hello terms of use.</span></span>
   
    ![Visual Studio – NuGet Package Manager (NuGet-csomagkezelő)][20]
   
    <span data-ttu-id="c61b3-135">Ez letölti, telepíti, valamint egy hivatkozást toohello Azure üzenetküldési kódtárat hozzáadja a Windows hello segítségével <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="c61b3-135">This downloads, installs, and adds a reference toohello Azure Messaging library for Windows by using hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
4. <span data-ttu-id="c61b3-136">Nyissa meg az App.xaml.cs hello fájlt, és adja hozzá a következő hello `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="c61b3-136">Open hello file App.xaml.cs and add hello following `using` statements:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. <span data-ttu-id="c61b3-137">Adja hozzá a következő kódot a hello felső hello **Application_Launching** metódus App.xaml.cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="c61b3-137">Add hello following code at hello top of **Application_Launching** method in App.xaml.cs:</span></span>
   
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
   > <span data-ttu-id="c61b3-138">érték hello **MyPushChannel** értéke egy index, amely egy létező csatorna a hello használt toolookup [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="c61b3-138">hello value **MyPushChannel** is an index that is used toolookup an existing channel in hello [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) collection.</span></span> <span data-ttu-id="c61b3-139">Amennyiben nem létezik ott ilyen, hozzon létre egy bejegyzést ezen a néven.</span><span class="sxs-lookup"><span data-stu-id="c61b3-139">If there isn't one there, create a new entry with that name.</span></span>
   > 
   > 
   
    <span data-ttu-id="c61b3-140">Ellenőrizze, hogy tooinsert hello nevét a hub és hello kapcsolati karakterlánc neve **DefaultListenSharedAccessSignature** hello előző szakaszban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="c61b3-140">Make sure tooinsert hello name of your hub and hello connection string called **DefaultListenSharedAccessSignature** that you obtained in hello previous section.</span></span>
    <span data-ttu-id="c61b3-141">Ezt a kódot hello csatorna URI azonosítóját hello alkalmazás lekéri az mpns-ből, majd regisztrálja a csatorna URI Azonosítóját az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="c61b3-141">This code retrieves hello channel URI for hello app from MPNS, and then registers that channel URI with your notification hub.</span></span> <span data-ttu-id="c61b3-142">Emellett biztosítja azt, hogy hello csatorna URI azonosítója legyen regisztrálva az értesítési központ minden alkalommal hello alkalmazás lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="c61b3-142">It also guarantees that hello channel URI is registered in your notification hub each time hello application is launched.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c61b3-143">Ez az oktatóanyag egy bejelentési értesítés toohello eszköz küld.</span><span class="sxs-lookup"><span data-stu-id="c61b3-143">This tutorial sends a toast notification toohello device.</span></span> <span data-ttu-id="c61b3-144">Amikor csempeértesítést küld kell meghívnia hello **BindToShellTile** hello csatorna metódust.</span><span class="sxs-lookup"><span data-stu-id="c61b3-144">When you send a tile notification, you must instead call hello **BindToShellTile** method on hello channel.</span></span> <span data-ttu-id="c61b3-145">toosupport bejelentési és a csempe értesítések hívható meg mindkét **BindToShellTile** és **BindToShellToast**.</span><span class="sxs-lookup"><span data-stu-id="c61b3-145">toosupport both toast and tile notifications, call both **BindToShellTile** and  **BindToShellToast**.</span></span>
   > 
   > 
6. <span data-ttu-id="c61b3-146">A Megoldáskezelőben bontsa ki a **tulajdonságok**, nyissa meg hello `WMAppManifest.xml` fájlt, kattintson a hello **képességek** lapot, és győződjön meg arról, hogy hello **ID_CAP_PUSH_NOTIFICATION** funkció be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="c61b3-146">In Solution Explorer, expand **Properties**, open hello `WMAppManifest.xml` file, click hello **Capabilities** tab, and make sure that hello **ID_CAP_PUSH_NOTIFICATION** capability is checked.</span></span>
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt toosend a push notification toohello app will fail.
7. <span data-ttu-id="c61b3-147">Nyomja le az hello `F5` kulcs toorun hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c61b3-147">Press hello `F5` key toorun hello app.</span></span>
   
    <span data-ttu-id="c61b3-148">Hello app egy regisztrációs üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c61b3-148">A registration message is displayed in hello app.</span></span>
8. <span data-ttu-id="c61b3-149">Bezárás hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c61b3-149">Close hello app.</span></span>  
   
   > [!NOTE]
   > <span data-ttu-id="c61b3-150">a bejelentési leküldéses értesítések tooreceive, hello alkalmazás nem futhat hello előtérben.</span><span class="sxs-lookup"><span data-stu-id="c61b3-150">tooreceive a toast push notification, hello application must not be running in hello foreground.</span></span>
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a><span data-ttu-id="c61b3-151">Leküldéses értesítések küldése a háttérrendszerből</span><span class="sxs-lookup"><span data-stu-id="c61b3-151">Send push notifications from your backend</span></span>
<span data-ttu-id="c61b3-152">Leküldéses értesítéseket küldhet a Notification Hubs használatával bármilyen háttérrendszerből keresztül hello nyilvános <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-felület</a>.</span><span class="sxs-lookup"><span data-stu-id="c61b3-152">You can send push notifications by using Notification Hubs from any backend via hello public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="c61b3-153">Ebben az oktatóanyagban leküldéses értesítéseket küld egy .NET-konzolalkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="c61b3-153">In this tutorial, you send push notifications using a .NET console application.</span></span> 

<span data-ttu-id="c61b3-154">Például egy toosend a leküldéses értesítések a Notification Hubs szolgáltatással integrált ASP.NET WebAPI háttérrendszerből, lásd: [Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span><span class="sxs-lookup"><span data-stu-id="c61b3-154">For an example of how toosend push notifications from an ASP.NET WebAPI backend that's integrated with Notification Hubs, see [Azure Notification Hubs Notify Users with .NET backend](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span></span>  

<span data-ttu-id="c61b3-155">Például hogyan toosend leküldéses értesítések segítségével hello [REST API-k](https://msdn.microsoft.com/library/azure/dn223264.aspx), tekintse meg [hogyan toouse Notification Hubs Java](notification-hubs-java-push-notification-tutorial.md) és [hogyan toouse php-ből a Notification Hubs](notification-hubs-php-push-notification-tutorial.md) .</span><span class="sxs-lookup"><span data-stu-id="c61b3-155">For an example of how toosend push notifications by using hello [REST APIs](https://msdn.microsoft.com/library/azure/dn223264.aspx), check out [How toouse Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) and [How toouse Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

1. <span data-ttu-id="c61b3-156">Kattintson a jobb gombbal hello megoldás, jelölje be **Hozzáadás** és **új projekt...** , majd a **Visual C#**, kattintson a **Windows** és **Konzolalkalmazás**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c61b3-156">Right-click hello solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
       ![Visual Studio - New Project - Console Application][6]
   
    <span data-ttu-id="c61b3-157">Ezzel hozzáad egy új Visual C# konzol toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="c61b3-157">This adds a new Visual C# console application toohello solution.</span></span> <span data-ttu-id="c61b3-158">Ezt egy külön megoldásban is megteheti.</span><span class="sxs-lookup"><span data-stu-id="c61b3-158">You can also do this in a separate solution.</span></span>
2. <span data-ttu-id="c61b3-159">Kattintson az **Eszközök**, a **Library Package Manager** (Kódtár-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="c61b3-159">Click **Tools**, click **Library Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="c61b3-160">Ekkor megjelenik a Package Manager Console hello.</span><span class="sxs-lookup"><span data-stu-id="c61b3-160">This displays hello Package Manager Console.</span></span>
3. <span data-ttu-id="c61b3-161">A hello **Csomagkezelő konzol** ablakban, a set hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c61b3-161">In hello **Package Manager Console** window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   <span data-ttu-id="c61b3-162">Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="c61b3-162">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="c61b3-163">Nyissa meg hello `Program.cs` fájlt, és adja hozzá a következő hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="c61b3-163">Open hello `Program.cs` file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="c61b3-164">A hello `Program` osztály, adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="c61b3-164">In hello `Program` class, add hello following method:</span></span>
   
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
   
    <span data-ttu-id="c61b3-165">Győződjön meg arról, hogy tooreplace hello `<hub name>` helyőrzőt hello hello értesítési központ hello portálon megjelenő nevét.</span><span class="sxs-lookup"><span data-stu-id="c61b3-165">Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello portal.</span></span> <span data-ttu-id="c61b3-166">Továbbá cserélje hello kapcsolati karakterlánc helyőrzőjét nevű kapcsolati karakterláncra hello **DefaultFullSharedAccessSignature** beszerzett hello szakasz "Az értesítési központ konfigurálása".</span><span class="sxs-lookup"><span data-stu-id="c61b3-166">Also, replace hello connection string placeholder with hello connection string called **DefaultFullSharedAccessSignature** that you obtained in hello section "Configure your notification hub."</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c61b3-167">Győződjön meg arról, hogy használja-e hello kapcsolati karakterlánc **teljes** fér hozzá, nem **figyelésére** hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="c61b3-167">Make sure that you use hello connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="c61b3-168">hello figyelési-hozzáférési karakterlánc nem rendelkezik engedélyekkel toosend leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="c61b3-168">hello listen-access string does not have permissions toosend push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="c61b3-169">Adja hozzá a következő sort a hello a `Main` módszert:</span><span class="sxs-lookup"><span data-stu-id="c61b3-169">Add hello following line in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="c61b3-170">Az a Windows Phone-emulátort, és az alkalmazás lezárt, beállíthatja hello Konzolalkalmazás-projektet, hello alapértelmezett kezdőprojektként, és nyomja hello `F5` kulcs toorun hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c61b3-170">With your Windows Phone emulator running and your app closed, set hello console application project as hello default startup project, and then press hello `F5` key toorun hello app.</span></span>
   
    <span data-ttu-id="c61b3-171">Egy bejelentési leküldéses értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="c61b3-171">You will receive a toast push notification.</span></span> <span data-ttu-id="c61b3-172">Hello bejelentési szalagcímre kattintva betölti a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c61b3-172">Tapping hello toast banner loads hello app.</span></span>

<span data-ttu-id="c61b3-173">Minden hello lehetséges hasznos adatot megtalálja az hello [bejelentéskatalógussal] és [csempekatalógussal] témakörök az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="c61b3-173">You can find all hello possible payloads in hello [toast catalog] and [tile catalog] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c61b3-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c61b3-174">Next steps</span></span>
<span data-ttu-id="c61b3-175">Ez az egyszerű példában küldött leküldéses értesítések tooall a Windows Phone 8 rendszerű eszközökön.</span><span class="sxs-lookup"><span data-stu-id="c61b3-175">In this simple example, you broadcasted push notifications tooall your Windows Phone 8 devices.</span></span> 

<span data-ttu-id="c61b3-176">A rendezés tootarget adott felhasználókat, tekintse meg a toohello [Notification Hubs használata toopush értesítések toousers] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c61b3-176">In order tootarget specific users, refer toohello [Use Notification Hubs toopush notifications toousers] tutorial.</span></span> 

<span data-ttu-id="c61b3-177">Ha azt szeretné, toosegment a felhasználókat érdeklődési körök, olvasható [legfrissebb hírek Notification Hubs használata toosend].</span><span class="sxs-lookup"><span data-stu-id="c61b3-177">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> 

<span data-ttu-id="c61b3-178">További tudnivalók toouse értesítési központok [Notification Hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="c61b3-178">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance].</span></span>

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

