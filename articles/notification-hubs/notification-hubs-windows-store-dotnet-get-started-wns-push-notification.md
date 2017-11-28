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
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a><span data-ttu-id="7586f-103">Ismerkedés a Notification Hubs univerzális platformon futó Windows-alkalmazásokkal történő használatával</span><span class="sxs-lookup"><span data-stu-id="7586f-103">Getting started with Notification Hubs for Windows Universal Platform Apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="7586f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7586f-104">Overview</span></span>
<span data-ttu-id="7586f-105">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa univerzális Windows Platform (UWP) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7586f-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Universal Windows Platform (UWP) app.</span></span>

<span data-ttu-id="7586f-106">Ebben az oktatóanyagban hozzon létre egy üres Windows Áruházbeli alkalmazást, amely leküldéses értesítéseket fogad hello Windows leküldéses értesítési szolgáltatásának (WNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="7586f-106">In this tutorial, you create a blank Windows Store app that receives push notifications by using hello Windows Push Notification Service (WNS).</span></span> <span data-ttu-id="7586f-107">Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.</span><span class="sxs-lookup"><span data-stu-id="7586f-107">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7586f-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="7586f-108">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="7586f-109">az oktatóanyag befejezése hello kódja a Githubon található [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span><span class="sxs-lookup"><span data-stu-id="7586f-109">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7586f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7586f-110">Prerequisites</span></span>
<span data-ttu-id="7586f-111">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="7586f-111">This tutorial requires hello following:</span></span>

* <span data-ttu-id="7586f-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) vagy újabb</span><span class="sxs-lookup"><span data-stu-id="7586f-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) or later</span></span>
* [<span data-ttu-id="7586f-113">Az univerzális Windows-alkalmazások fejlesztőeszközei telepítve vannak</span><span class="sxs-lookup"><span data-stu-id="7586f-113">Universal Windows App Development Tools installed</span></span>](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* <span data-ttu-id="7586f-114">Aktív Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="7586f-114">An active Azure account</span></span> <br/><span data-ttu-id="7586f-115">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="7586f-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7586f-116">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="7586f-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span></span>
* <span data-ttu-id="7586f-117">Aktív Windows Áruházbeli fiók</span><span class="sxs-lookup"><span data-stu-id="7586f-117">An active Windows Store account</span></span>

<span data-ttu-id="7586f-118">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, univerzális platformon futó Windows-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="7586f-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Universal Platform apps.</span></span>

## <a name="register-your-app-for-hello-windows-store"></a><span data-ttu-id="7586f-119">Hello Windows Áruházbeli alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="7586f-119">Register your app for hello Windows Store</span></span>
<span data-ttu-id="7586f-120">toosend leküldéses értesítések tooUWP alkalmazások, társítania kell az alkalmazás toohello Windows Store.</span><span class="sxs-lookup"><span data-stu-id="7586f-120">toosend push notifications tooUWP apps, you must associate your app toohello Windows Store.</span></span> <span data-ttu-id="7586f-121">A notification hub toointegrate WNS majd be kell állítania.</span><span class="sxs-lookup"><span data-stu-id="7586f-121">You must then configure your notification hub toointegrate with WNS.</span></span>

1. <span data-ttu-id="7586f-122">Ha már nincs regisztrálva az alkalmazás, keresse meg a toohello [Windows fejlesztői központ](https://dev.windows.com/overview), jelentkezzen be Microsoft-fiókjával, és kattintson a **hozzon létre egy új alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="7586f-122">If you have not already registered your app, navigate toohello [Windows Dev Center](https://dev.windows.com/overview), sign in with your Microsoft account, and then click **Create a new app**.</span></span>

2. <span data-ttu-id="7586f-123">Írja be az alkalmazás nevét, majd kattintson az **Alkalmazásnév lefoglalása** parancsra.</span><span class="sxs-lookup"><span data-stu-id="7586f-123">Type a name for your app and click **Reserve app name**.</span></span> <span data-ttu-id="7586f-124">Ezzel létrehoz egy új Windows Áruházbeli regisztrációt az alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="7586f-124">This creates a new Windows Store registration for your app.</span></span>

3. <span data-ttu-id="7586f-125">A Visual Studio, hozzon létre egy új Visual C# Áruházbeli alkalmazások projektet az univerzális Windows hello segítségével **üres alkalmazás** sablont, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="7586f-125">In Visual Studio, create a new Visual C# Store Apps project by using hello Windows Universal **Blank App** template and click **OK**.</span></span>

4. <span data-ttu-id="7586f-126">Fogadja el a hello alapértelmezett hello cél- és minimális platformverziók értéket.</span><span class="sxs-lookup"><span data-stu-id="7586f-126">Accept hello defaults for hello target and minimum platform versions.</span></span>

5. <span data-ttu-id="7586f-127">A Megoldáskezelőben kattintson a jobb gombbal hello Windows Áruházbeli alkalmazás projektjére, kattintson a **tároló**, és kattintson a **hello Áruházbeli alkalmazás társítása...** . hello **társítsa az alkalmazás hello Windows áruház** varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7586f-127">In Solution Explorer, right-click hello Windows Store app project, click **Store**, and then click **Associate App with hello Store...**. hello **Associate Your App with hello Windows Store** wizard appears.</span></span>

6. <span data-ttu-id="7586f-128">Hello varázslót jelentkezzen be a Microsoft-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="7586f-128">In hello wizard, sign in with your Microsoft account.</span></span>

7. <span data-ttu-id="7586f-129">Hello regisztrált alkalmazásra, a 2. lépésben kattintson, majd **következő**, és kattintson a **társítása**.</span><span class="sxs-lookup"><span data-stu-id="7586f-129">Click hello app that you registered in step 2, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="7586f-130">Ez biztosítja a szükséges hello Windows Áruházbeli regisztrációs adatokat toohello alkalmazásjegyzék.</span><span class="sxs-lookup"><span data-stu-id="7586f-130">This adds hello required Windows Store registration information toohello application manifest.</span></span>

8. <span data-ttu-id="7586f-131">Vissza a hello [Windows fejlesztői központ](http://dev.windows.com/overview) az új alkalmazás lapján kattintson **szolgáltatások**, kattintson a **leküldéses értesítések**, és kattintson a **WNS/MPNS**.</span><span class="sxs-lookup"><span data-stu-id="7586f-131">Back on hello [Windows Dev Center](http://dev.windows.com/overview) page for your new app, click **Services**, click **Push notifications**, and then click **WNS/MPNS**.</span></span>

9. <span data-ttu-id="7586f-132">Kattintson az **New Notification** (Új értesítés) elemre.</span><span class="sxs-lookup"><span data-stu-id="7586f-132">Click **New Notification**.</span></span>

10. <span data-ttu-id="7586f-133">Kattintson az **Blank (Toast)** (Üres (Bejelentési)) sablonra, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7586f-133">Click **Blank (Toast)** template and then click **OK**.</span></span>

11. <span data-ttu-id="7586f-134">Adja meg az értesítés **nevét** és egy vizualizáció **környezetfüggő** üzenetet.</span><span class="sxs-lookup"><span data-stu-id="7586f-134">Enter a notification **Name** and Visual **Context** message.</span></span> <span data-ttu-id="7586f-135">Ezután kattintson a **Save as draft** (Mentés vázlatként) gombra.</span><span class="sxs-lookup"><span data-stu-id="7586f-135">Then click **Save as draft**.</span></span>

12. <span data-ttu-id="7586f-136">Keresse meg a toohello [alkalmazásregisztrációs portálra](http://apps.dev.microsoft.com) , és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="7586f-136">Navigate toohello [Application Registration Portal](http://apps.dev.microsoft.com) and log in.</span></span>

13. <span data-ttu-id="7586f-137">Kattintson az alkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="7586f-137">Click on your application name.</span></span> <span data-ttu-id="7586f-138">Jegyezze fel a hello **Alkalmazáskulcsot** jelszó és hello **csomag biztonsági azonosítóját (SID)** hello található **Windows áruház** platform beállításait.</span><span class="sxs-lookup"><span data-stu-id="7586f-138">Make a note of hello **Application Secret** password and hello **Package security identifier (SID)** located in hello **Windows Store** platform settings.</span></span>

     > [AZURE.WARNING]
    <span data-ttu-id="7586f-139">hello alkalmazás titkos kulcs és a csomag biztonsági AZONOSÍTÓJÁT is fontos biztonsági hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="7586f-139">hello application secret and package SID are important security credentials.</span></span> <span data-ttu-id="7586f-140">Ezeket az értékeket ne ossza meg senkivel, és ne terjessze az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="7586f-140">Do not share these values with anyone or distribute them with your app.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="7586f-141">Az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7586f-141">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="7586f-142">Jelölje be hello <b>értesítési szolgáltatások</b> beállítás és hello <b>Windows (WNS)</b> lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="7586f-142">Select hello <b>Notification Services</b> option and hello <b>Windows (WNS)</b> option.</span></span> <span data-ttu-id="7586f-143">Majd adja meg a hello <b>alkalmazáskulcsot</b> hello jelszót <b>biztonsági kulcs</b> mező.</span><span class="sxs-lookup"><span data-stu-id="7586f-143">Then enter hello <b>Application secret</b> password in hello <b>Security Key</b> field.</span></span> <span data-ttu-id="7586f-144">Adja meg a <b>CSOMAGAZONOSÍTÓT</b> érték, amely a wns-ből hello előző szakaszban beszerzett, és kattintson a <b>mentése</b>.</span><span class="sxs-lookup"><span data-stu-id="7586f-144">Enter your <b>Package SID</b> value that you obtained from WNS in hello previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

<span data-ttu-id="7586f-145">Az értesítési központ most már konfigurált toowork WNS-sel való, és rendelkezik hello kapcsolati karakterláncok tooregister az alkalmazás és az értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="7586f-145">Your notification hub is now configured toowork with WNS, and you have hello connection strings tooregister your app and send notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="7586f-146">Csatlakozás az alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="7586f-146">Connect your app toohello notification hub</span></span>
1. <span data-ttu-id="7586f-147">A Visual Studióban, kattintson a jobb gombbal a hello megoldás, és kattintson **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="7586f-147">In Visual Studio, right-click hello solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="7586f-148">Ez megjeleníti a hello **NuGet-csomagok kezelése** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7586f-148">This displays hello **Manage NuGet Packages** dialog box.</span></span>
2. <span data-ttu-id="7586f-149">Keresse meg `WindowsAzure.Messaging.Managed` kattintson **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="7586f-149">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and accept hello terms of use.</span></span>
   
    ![][20]
   
    <span data-ttu-id="7586f-150">Ez letölti, telepíti, valamint egy hivatkozást toohello Azure üzenetküldési kódtárat hozzáadja a Windows hello segítségével <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="7586f-150">This downloads, installs, and adds a reference toohello Azure Messaging library for Windows by using hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
3. <span data-ttu-id="7586f-151">Nyissa meg a hello App.xaml.cs projektfájlt, és adja hozzá a következő hello `using` utasításokat.</span><span class="sxs-lookup"><span data-stu-id="7586f-151">Open hello App.xaml.cs project file and add hello following `using` statements.</span></span> 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. <span data-ttu-id="7586f-152">Is App.xaml.cs fájlban adja hozzá a hello következő **InitNotificationsAsync** metódus definícióját toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="7586f-152">Also in App.xaml.cs, add hello following **InitNotificationsAsync** method definition toohello **App** class:</span></span>
   
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
   
    <span data-ttu-id="7586f-153">Ezt a kódot hello csatorna URI azonosítóját hello alkalmazás lekéri a wns-ből, majd regisztrálja a csatorna URI Azonosítóját az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="7586f-153">This code retrieves hello channel URI for hello app from WNS, and then registers that channel URI with your notification hub.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7586f-154">Győződjön meg arról, hogy tooreplace hello "a hub name" helyőrzőt hello hello értesítési központ hello Azure portálon megjelenő nevét.</span><span class="sxs-lookup"><span data-stu-id="7586f-154">Make sure tooreplace hello "your hub name" placeholder with hello name of hello notification hub that appears in hello Azure Portal.</span></span> <span data-ttu-id="7586f-155">Cserélje le hello kapcsolati karakterlánc helyőrzőjét hello **DefaultListenSharedAccessSignature** hello beszerzett kapcsolati karakterlánc **hozzáférési szabályzatok** az értesítési központ lapján egy előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7586f-155">Also replace hello connection string placeholder with hello **DefaultListenSharedAccessSignature** connection string that you obtained from hello **Access Polices** page of your Notification Hub in a previous section.</span></span>
   > 
   > 
5. <span data-ttu-id="7586f-156">Hello hello tetején **OnLaunched** eseménykezelő App.xaml.cs fájlban adja hozzá a következő új hívás toohello hello **InitNotificationsAsync** módszert:</span><span class="sxs-lookup"><span data-stu-id="7586f-156">At hello top of hello **OnLaunched** event handler in App.xaml.cs, add hello following call toohello new **InitNotificationsAsync** method:</span></span>
   
        InitNotificationsAsync();
   
    <span data-ttu-id="7586f-157">Ez biztosítja, hogy a hello csatorna URI azonosítója legyen regisztrálva az értesítési központ minden alkalommal hello alkalmazás lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="7586f-157">This guarantees that hello channel URI is registered in your notification hub each time hello application is launched.</span></span>
6. <span data-ttu-id="7586f-158">Nyomja le az hello **F5** kulcs toorun hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7586f-158">Press hello **F5** key toorun hello app.</span></span> <span data-ttu-id="7586f-159">Hello regisztrációs kulcs egy felugró párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7586f-159">A pop-up dialog that contains hello registration key is displayed.</span></span>

<span data-ttu-id="7586f-160">Az alkalmazás mostantól készen áll a tooreceive bejelentési értesítést.</span><span class="sxs-lookup"><span data-stu-id="7586f-160">Your app is now ready tooreceive toast notifications.</span></span>

## <a name="send-notifications"></a><span data-ttu-id="7586f-161">Értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="7586f-161">Send notifications</span></span>
<span data-ttu-id="7586f-162">Gyorsan tesztelheti értesítések fogadásának az alkalmazásban való értesítések a hello [Azure Portal](https://portal.azure.com/) hello segítségével **küldés tesztelése** hello értesítési központot, ahogy az üdvözlő képernyőt az alábbi gombra.</span><span class="sxs-lookup"><span data-stu-id="7586f-162">You can quickly test receiving notifications in your app by sending notifications in hello [Azure Portal](https://portal.azure.com/) using hello **Test Send** button on hello notification hub, as shown in hello screen below.</span></span>

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

<span data-ttu-id="7586f-163">A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7586f-163">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="7586f-164">Közvetlen toosend értesítési üzenetekben, ha a szalagtár nem érhető el a háttér-hello REST API-t is használható.</span><span class="sxs-lookup"><span data-stu-id="7586f-164">You can also use hello REST API directly toosend notification messages if a library is not available for your back-end.</span></span> 

<span data-ttu-id="7586f-165">Ebben az oktatóanyagban a rendszer egyszerűen és csak az ügyfélalkalmazás tesztelését küldött értesítésekkel hello .NET SDK használatával egy háttér-szolgáltatás helyett egy konzolalkalmazás értesítési központjának bemutatása.</span><span class="sxs-lookup"><span data-stu-id="7586f-165">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="7586f-166">Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers] oktatóanyagot hello következő lépés az értesítéseknek ASP.NET-háttérrendszerből történő küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="7586f-166">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="7586f-167">A következő módszerekkel hello azonban az értesítések küldésével használhatók:</span><span class="sxs-lookup"><span data-stu-id="7586f-167">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="7586f-168">**REST-felület**: bármely háttér platformon hello segítségével támogathatja az értesítéseket [REST-felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="7586f-168">**REST Interface**:  You can support notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="7586f-169">**A Microsoft Azure Notification Hubs .NET SDK**: hello Nuget-Csomagkezelőt a Visual Studio, a futtasson [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="7586f-169">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="7586f-170">**NODE.js** : [hogyan toouse Notification Hubs Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="7586f-170">**Node.js** : [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="7586f-171">**Az Azure Mobile Apps**: A példa bemutatja, hogyan toosend értesítések a Notification Hubs szolgáltatással integrált Azure Mobile Apps lásd [leküldéses értesítések hozzáadása Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="7586f-171">**Azure Mobile Apps**: For an example of how toosend notifications from an Azure Mobile App that's integrated with Notification Hubs, see [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="7586f-172">**Java / PHP**: hogyan toosend értesítések használatával hello REST API-k példát lásd: "hogyan toouse Notification Hubs Java/php-ből" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="7586f-172">**Java / PHP**: For an example of how toosend notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-console-app"></a><span data-ttu-id="7586f-173">(Választható) Értesítések küldése konzolalkalmazásból</span><span class="sxs-lookup"><span data-stu-id="7586f-173">(Optional) Send notifications from a console app</span></span>
<span data-ttu-id="7586f-174">toosend értesítések .NET-Konzolalkalmazás használatával kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7586f-174">toosend notifications by using a .NET console application follow these steps.</span></span> 

1. <span data-ttu-id="7586f-175">Kattintson a jobb gombbal hello megoldás, jelölje be **Hozzáadás** és **új projekt...** , majd a **Visual C#**, kattintson a **Windows** és **Konzolalkalmazás**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="7586f-175">Right-click hello solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
    <span data-ttu-id="7586f-176">Ezzel hozzáad egy új Visual C# konzol toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="7586f-176">This adds a new Visual C# console application toohello solution.</span></span> <span data-ttu-id="7586f-177">Ezt egy külön megoldásban is megteheti.</span><span class="sxs-lookup"><span data-stu-id="7586f-177">You can also do this in a separate solution.</span></span>

2. <span data-ttu-id="7586f-178">A Visual Studióban kattintson az **Eszközök**, a **NuGet Package Manager** (NuGet-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="7586f-178">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="7586f-179">Ez a Visual Studio hello Csomagkezelő konzol jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7586f-179">This displays hello Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="7586f-180">Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="7586f-180">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="7586f-181">Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="7586f-181">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="7586f-182">Nyissa meg hello Program.cs fájlt, és adja hozzá a következő hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="7586f-182">Open hello Program.cs file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="7586f-183">A hello **Program** osztály, adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="7586f-183">In hello **Program** class, add hello following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure tooreplace hello "hub name" placeholder with hello name of hello notification hub that as it appears in hello Azure Portal. Also, replace hello connection string placeholder with hello **DefaultFullSharedAccessSignature** connection string that you obtained from hello **Access Policies** page of your Notification Hub in hello section called "Configure your notification hub."
   
   > [!NOTE]
   > <span data-ttu-id="7586f-184">Győződjön meg arról, hogy használja-e hello kapcsolati karakterláncot, amelynek **teljes** fér hozzá, nem **figyelésére** hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="7586f-184">Make sure that you use hello connection string that has **Full** access, not **Listen** access.</span></span> <span data-ttu-id="7586f-185">hello figyelési-hozzáférési karakterlánc nem rendelkezik engedélyekkel toosend értesítések.</span><span class="sxs-lookup"><span data-stu-id="7586f-185">hello listen-access string does not have permissions toosend notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="7586f-186">Adja hozzá a következő sorokat hello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7586f-186">Add hello following lines in hello **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="7586f-187">Kattintson a jobb gombbal a hello Konzolalkalmazás-projektet a Visual Studióban, és kattintson a **beállítás kezdőprojektként** tooset hello indítási projektként azt.</span><span class="sxs-lookup"><span data-stu-id="7586f-187">Right-click hello console application project in Visual Studio, and click **Set as StartUp Project** tooset it as hello startup project.</span></span> <span data-ttu-id="7586f-188">Nyomja le az hello **F5** toorun hello alkalmazás kulcsát.</span><span class="sxs-lookup"><span data-stu-id="7586f-188">Then press hello **F5** key toorun hello application.</span></span>
   
    <span data-ttu-id="7586f-189">Egy bejelentési értesítést fog kapni az összes regisztrált eszközön.</span><span class="sxs-lookup"><span data-stu-id="7586f-189">You will receive a toast notification on all registered devices.</span></span> <span data-ttu-id="7586f-190">Hello bejelentési szalagcímre kattintva vagy koppintva betölti hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7586f-190">Clicking or tapping hello toast banner loads hello app.</span></span>

<span data-ttu-id="7586f-191">Hello található összes hello támogatott hasznos adatot [bejelentéskatalógussal], [csempekatalógussal], és [jelvények áttekintésével] témakörök az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="7586f-191">You can find all hello supported payloads in hello [toast catalog], [tile catalog], and [badge overview] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7586f-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7586f-192">Next steps</span></span>
<span data-ttu-id="7586f-193">Ez az egyszerű példában küldött értesítéseket tooall a Windows-eszközök hello portál vagy egy konzolalkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="7586f-193">In this simple example, you sent broadcast notifications tooall your Windows devices using hello portal or a console app.</span></span> <span data-ttu-id="7586f-194">Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers] oktatóanyagot hello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="7586f-194">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="7586f-195">Azt láthatja, hogyan toosend értesítést kapnak az ASP.NET háttérkiszolgáló használatával címkéket tootarget adott felhasználókra.</span><span class="sxs-lookup"><span data-stu-id="7586f-195">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="7586f-196">Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, lásd: [legfrissebb hírek Notification Hubs használata toosend].</span><span class="sxs-lookup"><span data-stu-id="7586f-196">If you want toosegment your users by interest groups, see [Use Notification Hubs toosend breaking news].</span></span> 

<span data-ttu-id="7586f-197">toolearn Notification Hubs további általános információt lásd: [Notification Hubs használatával](notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7586f-197">toolearn more general information about Notification Hubs, see [Notification Hubs Guidance](notification-hubs-push-notification-overview.md).</span></span>

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
